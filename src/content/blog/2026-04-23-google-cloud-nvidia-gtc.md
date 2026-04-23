---
title: 'Google Cloud расширила связку с NVIDIA на GTC 2026'
description: 'Preview fractional G4 VMs на vGPU RTX PRO 6000 Blackwell, интеграция NVIDIA Dynamo с GKE Inference Gateway, поддержка Vera Rubin NVL72.'
pubDate: 2026-04-23
cover: 'https://replicate.delivery/xezq/TXFyc4yTcYoIKRXmd0ns2AaD3iRsuFYxBg6QFngMRWnirfOLA/out-0.webp'
source: 'https://cloud.google.com/blog/products/compute/google-cloud-ai-infrastructure-at-nvidia-gtc-2026'
tags: ['ai', 'ml', 'cloud', 'infrastructure']
---

На NVIDIA GTC 2026 Google Cloud объявил пакет обновлений AI-инфраструктуры. Ставка та же — Google Cloud AI Hypercomputer, интегрированная платформа с оптимизированным железом, ПО и моделями потребления. Новости распределились по трём линиям: железо и аппаратное ускорение, софт и платформа, экосистема.

Железо. Растёт использование G4 VMs на базе NVIDIA RTX PRO 6000 Blackwell Server Edition — Google сообщает о «сильной динамике» с момента запуска. Главное нововведение — preview дробных G4 VMs через NVIDIA vGPU. Это первая в индустрии реализация vGPU для RTX PRO 6000 Blackwell Server Edition. Дробные слоты позволяют платить за часть GPU вместо всей карты — полезно для команд, чьи нагрузки не забивают полный ускоритель. Управляются через Google Kubernetes Engine с продвинутым binpacking контейнеров, fallback-приоритеты обеспечивает Dynamic Workload Scheduler. В этом же блоке — объявленная поддержка платформы NVIDIA Vera Rubin NVL72 в ближайших релизах.

Софт. Главная новость для ML-инженеров — интеграция NVIDIA Dynamo с GKE Inference Gateway. Это модульный open-source control plane между прикладным уровнем и железом. Связка даёт командам точный контроль над ROI ускорителей, ускоряет time-to-market для новых моделей и закрывает вопросы привязки к платформе. Для MoE-архитектур Google Cloud опубликовал расширенные scaling-рецепты на A4X VMs (GB200 NVL72 + Dynamo) — как обходить узкие места памяти и interconnect при inference-нагрузках на AI Hypercomputer. Дополнительно NVIDIA-поддержка расширяется в Vertex AI Training и Model Garden.

Экосистема. Google и NVIDIA запускают Kaggle-соревнование на базе NVIDIA Nemotron, работающего на G4 VMs, — зацепка для дата-сайентистов и ML-команд, которые хотят получить опыт на актуальном железе без собственной инвестиции. Параллельно стартовал dedicated public sector AI startup accelerator для компаний, работающих с государственным сектором.

Что это значит на практике. Для ML-команд, которые ведут собственные inference-стеки, Dynamo на GKE снимает необходимость писать свой control plane — открытый интерфейс поверх GKE обрабатывает routing, backpressure и прогрев моделей. Для продуктовых разработчиков fractional G4 VMs снижают входной порог на RTX PRO 6000 Blackwell — теперь можно начать с четверти карты и расти. Для тех, кто масштабирует MoE, появилась готовая reference-конфигурация на A4X.

GTC 2026 — очередная итерация плотного сотрудничества Google Cloud и NVIDIA. Параллельно Google продвигает собственные TPU 8t и TPU 8i, разделённые на тренинг и инференс. Покупателям AI-инфраструктуры Google предлагает обе дороги — NVIDIA GPU и Google TPU — внутри одной платформы AI Hypercomputer.
