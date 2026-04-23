---
title: 'Microsoft запустила три AI-модели семейства MAI'
description: 'MAI-Transcribe-1 обходит Whisper на 25 языках, MAI-Voice-1 клонирует голос за секунды, MAI-Image-2 попал в топ-3 Arena.ai.'
pubDate: 2026-04-23
cover: 'https://replicate.delivery/xezq/BuIUyQn3rjIpJ9cxFHJK98IqcMAhhsf288THzegSvcAFce7sA/out-0.webp'
source: 'https://venturebeat.com/technology/microsoft-launches-3-new-ai-models-in-direct-shot-at-openai-and-google'
tags: ['ai', 'microsoft', 'models']
---

Microsoft выпустила три модели семейства MAI: `MAI-Transcribe-1` (speech-to-text), `MAI-Voice-1` (text-to-speech) и `MAI-Image-2` (генерация изображений). Все доступны через Microsoft Foundry и новый MAI Playground. Это первый публичный релиз superintelligence-команды, которую Мустафа Сулейман собрал шесть месяцев назад для курса на «AI self-sufficiency». По заявлению компании, модели обучены на вдвое меньшем числе GPU, чем state-of-the-art у конкурентов.

`MAI-Transcribe-1` бьёт OpenAI Whisper-large-v3 по Word Error Rate на всех 25 топовых языках бенчмарка FLEURS. Средний WER — 3.8%. Против Google Gemini 3.1 Flash Lite Microsoft выигрывает на 22 языках из 25, против ElevenLabs Scribe v2 и OpenAI GPT-Transcribe — на 15 из 25. Сулейман в интервью VentureBeat назвал это «лучшей транскрипцией в мире». Модель напрямую бьёт в open-source-доминирование Whisper и в пуш Google по Gemini.

`MAI-Voice-1` генерирует 60 секунд естественного аудио за одну секунду вычислений. Сохраняет идентичность спикера в длинных текстах и поддерживает кастомный голос из нескольких секунд сэмпла. Цена — $22 за 1M символов через Microsoft Foundry. Это прямая конкуренция ElevenLabs, Resemble AI и экосистеме voice-AI-стартапов. Главное преимущество Microsoft — дистрибуция: любой разработчик Foundry получает доступ через тот же API, которым уже пользуется для GPT-моделей.

`MAI-Image-2` дебютировал в топ-3 семействе на Arena.ai. Генерация на Foundry и Copilot ускорена минимум вдвое относительно предыдущей версии. Цена — $5 за 1M входных токенов, $33 за 1M выходных. Microsoft выкатывает модель в Bing и PowerPoint. Среди первых корпоративных клиентов — WPP, один из крупнейших рекламных холдингов мира.

Релиз приходится на сложный момент для Microsoft. По данным VentureBeat, прошедший квартал — худший для компании с финансового кризиса 2008 года: инвесторы требуют доказательств, что сотни миллиардов вложений в AI-инфраструктуру вернутся выручкой. Агрессивная цена MAI-моделей и позиционирование на снижение собственной себестоимости услуг — первый ответ команды Сулеймана на это давление. Для разработчиков вывод прост: в Foundry теперь есть нативная альтернатива Whisper, ElevenLabs и OpenAI Images, и за неё платит не сторонний API-провайдер, а Microsoft.
