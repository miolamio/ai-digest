# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

Автоматический AI-дайджест на Astro + Vercel. Проект курса Claude Code Basics: восемь ступеней превращают блог из пустого шаблона в пайплайн автопубликости (поиск новостей → написание → обложка → коммит).

## Стек

- Astro `^6.1.1` (`src/content/blog/` — статьи в markdown с frontmatter).
- Node `>=22.12.0` (из `engines` в `package.json`).
- Интеграции Astro: `@astrojs/mdx` (MDX в постах), `@astrojs/sitemap` (генерация `sitemap-index.xml` при билде), `@astrojs/rss` (RSS-фид из той же коллекции). Оптимизация изображений — `sharp` через `astro:assets`.
- SSR-адаптера пока нет — чистый статический билд. Если фаза курса потребует серверную логику, понадобится `@astrojs/vercel` и `output: 'server' | 'hybrid'` в `astro.config.mjs`.
- TypeScript strict (`astro/tsconfigs/strict` + `strictNullChecks`).
- Vercel (автодеплой на push в `main`).
- MCP: Tavily (поиск новостей), Replicate (обложки).

## Команды

- `npm run dev` — локальный сервер на `:4321`.
- `npm run build` — production-сборка.
- `npm run preview` — просмотр сборки.
- `./run-claude.sh` — запускает Claude Code с прогруженным `.env` (нужно для MCP-серверов Tavily и Replicate; их ключи лежат в `.env`, который git-игнорируется).

Тестов и линтера нет. Единственная валидация — Stop-хук (см. «Хуки»).

## Архитектура

### Контентная коллекция и роутинг

- `src/content.config.ts` — описывает коллекцию `blog` через Zod-схему. Markdown/MDX из `src/content/blog/` грузятся через `glob`-лоадер.
- Роуты:
  - `/` — главная (`src/pages/index.astro`).
  - `/blog/` — список постов, сортировка по `pubDate` убыванию (`src/pages/blog/index.astro`).
  - `/blog/[...slug]/` — отдельный пост (`src/pages/blog/[...slug].astro`, `getStaticPaths` из коллекции).
  - `/rss.xml` — фид из той же коллекции (`src/pages/rss.xml.js`).
  - `/about` — статичная страница.
  - `/sitemap-index.xml` — генерируется при билде интеграцией `sitemap`.
- `src/consts.ts` — `SITE_TITLE`, `SITE_DESCRIPTION` (используются в `<head>`, RSS).
- `astro.config.mjs` — `site` сейчас `https://example.com`. Это значение попадает в canonical-URL, RSS и sitemap; перед реальным деплоем заменить на домен сайта.

### ⚠️ Расхождение поля обложки

Три источника правки не сходятся — учитывайте при работе со статьями:

- **Zod-схема** (`src/content.config.ts`) описывает `heroImage` через `image()` (локальный ассет Astro). Поля `cover` в схеме нет.
- **Layout** `BlogPost.astro` и индекс `blog/index.astro` рендерят `heroImage`, не `cover`.
- **Пайплайн** (агент `cover-artist`, Stop-хук, редполитика ниже) работает с полем `cover` — URL-строкой от Replicate.

На практике: агент `cover-artist` вписывает `cover:` (URL) во frontmatter, Stop-хук проверяет, что `cover` заполнен, но layout это поле не читает и обложку не показывает. Не правьте одну часть, не сверившись с двумя другими.

### Пайплайн автопубликации (`/digest`)

Оркестрация — скилл `.claude/skills/digest.md`, запускается командой `/digest`. Последовательность:

1. Агент `news-scout` — ищет новости через Tavily, возвращает 3 темы `[{title, source_url, angle}]`.
2. Для каждой темы (параллельно, 3 потока): `writer` пишет статью → `cover-artist` генерирует обложку и вписывает `cover:` в frontmatter.
3. Агент `page-builder` — проверяет frontmatter, коммитит в ветку `digest/auto`, пушит.

Агенты живут в `.claude/agents/` (`news-scout`, `writer`, `cover-artist`, `page-builder`), у каждого свой набор инструментов в frontmatter. Внутри темы writer → cover-artist строго последовательно; параллелизм только между темами. Скилл `/cover` (`.claude/skills/cover.md`) — ручная генерация одной обложки через Replicate (`flux-schnell`, 16:9, webp).

### Хуки (сторожа)

Конфиг в `.claude/settings.json`:

- **PreToolUse** (`block-main-push.sh`, matcher: `Bash`) — блокирует `git push` в `main`/`master`. Обход только флагом `CAPSTONE_ALLOW_MAIN_PUSH=1`.
- **PostToolUse** (`pipeline-log.sh`, matcher: `.*`) — пишет каждый вызов инструмента в `logs/pipeline.log` (каталог `logs/` git-игнорируется).
- **Stop** (`validate-article.js`) — валидирует статьи в `src/content/blog/`, изменённые за последние 10 минут: обязательные поля `title`, `description`, `pubDate`, `cover`, `source` заполнены; `title` ≤ 60 символов; `description` ≤ 160 символов. Блокирует завершение (exit 2) при ошибках.

`.claude/hooks/` — CommonJS (`package.json` с `"type": "commonjs"`), хуки на bash используют `jq`.

## Структура статьи

Файл `src/content/blog/YYYY-MM-DD-slug.md` с frontmatter:

```yaml
---
title: Заголовок до 60 символов без точки
description: Одно предложение до 160 символов.
pubDate: 2026-04-23
cover: https://replicate.delivery/...
source: https://example.com/news
tags: [ai, llm]
---
```

## Редполитика

### Тематика

Новости AI, машинного обучения, больших языковых моделей. Инструменты для разработчиков и продуктовых менеджеров.

### Стиль

- Информационный стиль (Ильяхов). Без маркетинговой лексики, оценочных прилагательных, канцелярита.
- Язык — русский.
- Предложения до 25 слов. Точки в конце буллетов.
- Без шаблона «от X до Y». Перечисляем конкретно.
- Без AI-маркеров: «погружаться», «ландшафт», «ключевой момент», «является свидетельством».

### Формат

- Заголовок: до 60 символов, без точки.
- Описание: одно предложение до 160 символов.
- Тело: 300–500 слов, 2–4 абзаца.
- Источник: обязательная ссылка на первоисточник.
- Обложка: URL от Replicate, соотношение 16:9.

### Обязательные поля frontmatter

`title`, `description`, `pubDate`, `cover`, `source` — все заполнены. Stop-хук блокирует публикацию, если хоть одно поле пустое.

## Git

- Ветка по умолчанию `main`.
- PreToolUse-хук блокирует прямой push в `main`. Автоматика коммитит в ветку `digest/auto`, слияние в `main` — вручную по желанию.
- Запрещены `git push --force`, `git reset --hard`, `git commit --amend`.
