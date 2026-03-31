---
name: strategy
description: Use when user needs to create a marketing strategy, media plan, communication strategy, or brand strategy. Triggers on briefs, strategy requests, "write a strategy", "create a marketing plan", "develop a media strategy", or when user shares a brief/TZ document. Also use when user says /strategy.
---

# Marketing Strategy Builder

Build comprehensive, data-driven marketing strategies through structured discovery, parallel research, and professional presentation.

<HARD-GATE>
Do NOT start writing the strategy until discovery questions are answered and research agents have completed. Strategy without data is speculation.
</HARD-GATE>

<HARD-GATE>
**Data Verification Rule:** NEVER use data from training knowledge without cross-checking via WebSearch. All macro numbers (GDP, inflation, exchange rates, market sizes, platform audiences) MUST be verified against real-time sources (ЦБ РФ, Росстат, АКАР, Mediascope). If a research agent returns data that was generated from training knowledge rather than web search results, flag it and re-search. Incorrect data (e.g., wrong exchange rate direction) destroys credibility of the entire strategy.

**Cross-verification of external sources:** Data from Perplexity, AI-generated summaries, and aggregator sites can contain errors and hallucinations. When research agents use such sources:
1. **Save raw source data** — each research agent must save its raw findings to `strategies/<slug>/raw/` folder (e.g., `raw/perplexity_macro.md`, `raw/perplexity_competitors.md`) before processing
2. **Cross-check critical numbers** — verify key figures (market sizes, exchange rates, visitor counts, revenue) against at least 2 independent primary sources (official sites, ЦБ РФ, Росстат, АКАР, company reports)
3. **Flag unverified data** — if a number could not be cross-verified, mark it as "[оценка]" or "[требует проверки]" in the strategy text
4. **Never trust a single AI-aggregated source** for financial data, exchange rates, or market volumes

**Source Priority Hierarchy (EXPANDED):**

| Приоритет | Тип источника | Примеры | Когда использовать |
|-----------|--------------|---------|-------------------|
| P1 | Государственные/регуляторные | cbr.ru, rosstat.gov.ru, mos.ru, ФНС | Макро-данные, ставки, инфляция, население |
| P2 | Крупные федеральные СМИ | rbk.ru, kommersant.ru, vedomosti.ru, interfax.ru, tass.ru | Подтверждённые факты, аналитика |
| P3 | Отраслевые исследовательские компании | mediascope.net, nielsen.com, kantar.com, ipsos-comcon.ru, getbrand.ru | Медиа-данные, потребитель, Brand Health |
| P4 | Индустриальные ассоциации и аналитика | akarussia.ru (АКАР), interactivead.ru (АРИР), autostat.ru, digitalbudget.ru | Рекламный рынок, отраслевые данные |
| P5 | Профессиональные отраслевые медиа | sostav.ru, adindex.ru, autonews.ru, habr.com | Тренды, кейсы, экспертные мнения |
| P6 | Платформы и их аналитика | yandex.ru/adv, ads.vk.com, avito.tech, vk.company | Данные по платформам, аудитории |
| P7 | Консалтинг и аналитические компании | yakovpartners.ru, bcg.com, mckinsey.com | Стратегические обзоры, прогнозы |
| P8 | Агрегаторы, TG-каналы, блоги | Новостные TG-каналы, блоги, форумы | ПОСЛЕДНИЙ приоритет, перепроверять |

**Правило приоритизации:** Если ЦБ РФ говорит одно, а новостной TG-канал другое — верим ЦБ РФ. Если Mediascope даёт цифры по аудитории, а блогер другие — верим Mediascope. Всегда предпочитать первоисточник агрегатору.

**Правило отбора информации:** При ограничении объёма (количество слайдов, длина текста) — приоритизировать от наиболее значимого к наименее. Идти от общего к частному. НЕ выбирать рандомно — ранжировать по влиянию на бренд, категорию, потребителя.
</HARD-GATE>

## Process Flow

```
DISCOVERY → BRIEF.YAML → RESEARCH (Perplexity primary + WebSearch spot-check)
    → ANALYTICS BLOCK → STRATEGY BLOCK
    → EXECUTIVE SUMMARY + FIRST 7 DAYS CHECKLIST + WEEKLY ROADMAP
    → HTML PRESENTATION + DEPLOY → KPI DASHBOARD TEMPLATE
```

### Research Strategy: Perplexity + WebSearch Combo

**If Perplexity API key is available** (check `.env` for `PERPLEXITY_API_KEY`):
- Use **Perplexity sonar-pro** as primary research tool (via curl to `https://api.perplexity.ai/chat/completions`)
- After all research agents complete, launch **1 spot-check agent** using WebSearch to verify 10 key numbers
- Mark any conflicting data with `[требует проверки]`

**If no Perplexity key:** Use WebSearch as before.

**NEVER mention Perplexity in output files.** Use "открытые источники" or specific source names instead.

## Phase 1: Discovery & Brief Analysis

### Step 1A: Brief Intake

If the user provides a brief (PDF, PPTX, document, or text), **read and analyze it first**. Extract and present:

1. **Задачи брифа** — что клиент хочет получить в качестве готового продукта (например, презентация из N блоков, HTML-сайт, медиаплан)
2. **Стратегические задачи бизнеса и маркетинга** — сформулировать, даже если клиент их не описал. Вывести из контекста брифа: что стоит за запросом?
3. **Задачи агентства** — какие функции агентство должно исполнять, какая нужна команда (SMM, ORM, PR, медиа, креатив, аналитика)
4. **Дисциплины стратегии** — определить, какие блоки нужны:
   - ☑ Аналитика (всегда)
   - ☑ Коммуникационная стратегия (всегда)
   - ☐/☑ SMM (отдельный блок с контент-планом и медиапланом)
   - ☐/☑ ORM/SERM (репутация, social listening, работа с отзывами)
   - ☐/☑ PR (медиа-отношения, спецпроекты, работа с журналистами)
   - ☐/☑ Influence-маркетинг (KOL/KOC стратегия)
   - ☐/☑ Медийная стратегия (медиаплан с CPM/CPF/Reach)
5. **Запросы к клиенту** — конкретный список того, что нужно дополнительно:
   - Ретроспективные данные (продажи, трафик, подписчики, бюджеты прошлых периодов)
   - Brand Health данные (UBA, NPS, Brand Attributes), если есть
   - Текущие медиапланы и их результаты
   - Данные из CRM / аналитики (Google Analytics, Яндекс.Метрика)
   - Доступ к платным отчётам (Autostat, Mediascope, Digital Budget и т.д.)
   - Бизнес-цели, если не сформулированы в брифе

Present this analysis to the user and confirm before proceeding.

### Step 1B: Discovery Questions

Ask questions **one at a time**. Prefer multiple choice. If brief already provided answers, skip those and only ask what's missing.

| # | Question | Why needed |
|---|----------|------------|
| 1 | **Что за бренд/продукт?** Название, категория, что продаёт/делает, сайт | Определяет всё исследование |
| 2 | **Текущее состояние маркетинга?** (a) Старт с нуля (b) Есть активности, нужна систематизация (c) Есть стратегия, нужно обновить | Глубина аналитики |
| 3 | **Главная цель стратегии?** (a) Узнаваемость/awareness (b) Трафик/посетители/лиды (c) Продажи/конверсия (d) Всё вместе | Фокус стратегии |
| 4 | **Целевая аудитория** — кто ваш клиент? Возраст, пол, доход, география, интересы. Есть ли конкретные сегменты (например, мужчины 18-44)? | Каналы и месседжи |
| 5 | **Ключевые конкуренты** — назовите 3-5 прямых конкурентов | Конкурентный анализ |
| 6 | **География продвижения?** (a) Один город (b) Россия (c) Россия + СНГ (d) Международный | Масштаб исследования |
| 7 | **Бюджетный коридор?** (a) до 5 млн ₽/год (b) 5-20 млн (c) 20-50 млн (d) 50-100 млн (e) 100+ млн (f) не определён | Распределение каналов |
| 8 | **Горизонт стратегии?** (a) 3 месяца (b) 6 месяцев (c) 12 месяцев (d) 12+ месяцев | Этапность планов |
| 9 | **Какие дисциплины нужны?** (a) Только стратегия (b) + SMM (c) + ORM (d) + PR (e) Полный цикл (SMM+ORM+PR) | Объём стратегии |

### Optional Deep-Dive Questions (ask if relevant)

| Question | When to ask |
|----------|-------------|
| Нужна ли международная аналитика? | Geography = международный |
| Работаете ли с блогерами? Есть опыт? | Любой B2C бренд |
| Есть ли сезонность в бизнесе? | Retail, туризм, развлечения, HoReCa |
| Какие каналы уже используете? | Состояние != старт с нуля |
| Есть ли brand guidelines / TOV? | Нужен для креативных направлений |
| Есть ли данные по текущим метрикам? (трафик, конверсия, подписчики) | Состояние != старт с нуля |
| Есть ли платные отчёты (Autostat, Mediascope, Brand Health)? | Для точности данных |
| Какие платформы/соцсети приоритетны? | Для SMM-блока |

### After Discovery: Confirm Scope

Present user with a summary of what the strategy will contain:

```
Подтверждение scope стратегии:
- Бренд: [название]
- Категория: [категория]
- Бизнес-цели: [сформулированные цели]
- Маркетинговые цели: [цели]
- ЦА: [описание + ключевые сегменты]
- Конкуренты: [список]
- География: [гео]
- Бюджет: [коридор]
- Горизонт: [срок]

Блок 1 (Аналитика):
☑ Макроэкономика + потребитель
☑ Рыночные сегменты (розница, e-comm, категория)
☑ Потребительская аналитика по сегментам ЦА
☑ Обзор медиа и рекламного рынка
☑ Фокус на категорию + предиктивная аналитика
☑ Анализ конкурентов (бизнес + коммуникации + соцсети)
☐/☑ Brand Health оценка (UBA proxy)
☐/☑ Международная аналитика
☑ SWOT

Блок 2 (Стратегия):
☑ Каскад: бизнес-цели → маркетинг → коммуникации → KPI
☑ Big Creative Idea
☑ Коммуникационная территория + ЦА-сегменты
☑ Каналы + контент-рубрики + блогеры
☑ Platform-specific стратегия
☐/☑ Креативные направления
☑ Бюджет
☑ Прогрессивные KPI (с конкурентным бенчмарком)

Блок 3 (Дисциплины — опционально):
☐/☑ SMM (контент-план, медиаплан, конкурентный бенчмарк)
☐/☑ ORM/SERM (social listening, sentiment, отзывы)
☐/☑ PR (медиа-ландшафт, журналисты, спецпроекты)
☐/☑ Hero Project (1-2 флагманских спецпроекта с ROI)

Формат финального продукта:
☑ HTML-сайт (deploy на Vercel)
☐/☑ PPTX презентация (в темплейте клиента)
☐/☑ Dashboard template

Подтверждаете? Есть что добавить?
```

Wait for user confirmation before proceeding.

### Save Brief (REQUIRED after confirmation)

After user confirms scope, **immediately save the brief** to `strategies/<brand-slug>/brief.yaml` before launching research. This allows re-running the strategy without re-asking all questions.

```yaml
# strategies/brand-slug/brief.yaml
# Auto-generated by /strategy skill — do not edit manually
# To re-run: /strategy --from-brief strategies/brand-slug/brief.yaml

brand:
  name: "Солнце Москвы"
  slug: "solntse-moskvy"
  category: "Колесо обозрения / развлечения"
  website: "moscow-sun.ru"
  description: "Самое высокое колесо обозрения в Европе (140 м), ВДНХ"

marketing_state: "start"  # start | active | refresh

goals:
  primary: "awareness"  # awareness | traffic | sales | all
  details: "Повышение потока посетителей в комплекс"

audience:
  description: "Семьи с детьми, пары 25-40, туристы из регионов"
  age: "25-45"
  income: "средний+"
  geography: "Москва + внутренний туризм"

competitors:
  - "PANORAMA360"
  - "Останкинская башня"
  - "Остров Мечты"
  - "Москвариум"
  - "Зарядье"

geography: "city"  # city | country | cis | international
city: "Москва"
country: "Россия"

budget:
  range: "30-50 млн ₽/год"  # or: "unknown"
  code: "20-50m"  # <5m | 5-20m | 20-50m | 50-100m | 100m+ | unknown

horizon: "12m"  # 3m | 6m | 12m | 12m+

disciplines:
  smm: true           # Отдельный SMM-блок с контент-планом и медиапланом
  orm: false           # ORM/SERM блок
  pr: false            # PR блок с медиа-ландшафтом
  hero_project: false  # Флагманский спецпроект с ROI

scope:
  international_analytics: true
  creative_directions: true
  influencer_strategy: true
  seasonality: true
  b2b_segment: false
  brand_health_proxy: true  # Оценка UBA на основе медиаданных

extra_context: |
  Бренд на старте активной маркетинговой активности.
  Открыт в 2022, нуждается в системном продвижении.
  Хотят международный бенчмаркинг (London Eye, Ain Dubai).

created_at: "2026-03-27"
```

**When `/strategy --from-brief <path>` is used:** Read the brief file, skip Phase 1 entirely, show the scope summary for quick confirmation, and proceed directly to Phase 2 (research).

**When a brief already exists in the target folder:** Ask the user: "Найден бриф от [date]. Использовать его или начать с нуля?" If reuse — load and skip to Phase 2.

## Phase 2: Parallel Research

Launch **4-6 research agents simultaneously** using the Agent tool. Each agent uses WebSearch and WebFetch to gather real data.

### Agent 1: Brand Research
```
Search: "[brand name]", "[brand] отзывы", "[brand] сайт", "[brand] соцсети"
Gather: description, prices, services, social media presence,
        follower counts per platform (TG, VK, OK, Dzen), ER%,
        ratings, reviews, press coverage, current positioning, USP,
        current content strategy assessment (AS IS)
Output: src/brand.md
```

### Agent 2: Macro Economy + Consumer
```
Sources: cbr.ru, rosstat.gov.ru, rbk.ru, yakovpartners.ru
Search: "экономика [country] [year] ВВП инфляция ключевая ставка",
        "потребительские расходы [category]", "потребительское поведение [year]"
Gather:
  Макро: GDP, inflation, key rate, disposable income, consumer confidence
  Рыночные сегменты: продовольственная розница, непродовольственная, e-commerce
    — какие растут, какие падают, на сколько % vs прошлый год
  Потребитель: поведение в текущих условиях, какие бренды выбирает,
    доход, настроение, тренды потребления
    — по конкретным сегментам ЦА бренда (например, мужчины 25-44)
  Предиктивная аналитика: долгосрочные риски, прогнозы трендов
Sources: mediascope.net/brandpulse, nielsen.com, kantar.com, ipsos-comcon.ru
Output: src/macro.md

IMPORTANT: Из всего объёма информации выбрать то, что ВЛИЯЕТ на:
  1) потребителя данного бренда
  2) индустрию/категорию
  3) компанию в частности
Не перечислять всё подряд — приоритизировать по значимости.
```

### Agent 3: Competitors (Business + Communications)
```
Search: "[competitor] продажи [year]", "[competitor] маркетинг стратегия",
        "[competitor] соцсети подписчики", "рынок [category] [year]"
Gather:
  Бизнес: продажи, доля рынка, динамика YoY, модельный ряд, цены
  Коммуникации: соцсети (подписчики по платформам, рост YoY%, ER%),
    медиабюджеты (если доступно), позиционирование, TOV, контент-подход
  SMM бенчмарк (ОБЯЗАТЕЛЬНАЯ ТАБЛИЦА):
    | Бренд | TG подп. | TG рост% | TG ER% | VK подп. | VK рост% | VK ER% | Посты/мес |
  Активности: кампании, спецпроекты, коллаборации, блогеры
  Brand Territory: с какими событиями/брендами ассоциируется конкурент

  ФИНАНСОВЫЕ ДАННЫЕ ИЗ НАЛОГОВОЙ (для российских компаний):
  Для каждого конкурента с российским юрлицом — запросить данные через DaData API.
  API key хранится в .env как DADATA_API_KEY.

  Поиск компании по названию:
  curl -s -X POST "https://suggestions.dadata.ru/suggestions/api/4_1/rs/suggest/party" \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -H "Authorization: Token $DADATA_API_KEY" \
    -d '{"query": "[название компании]", "count": 3}'

  Поиск по ИНН (если известен):
  curl -s -X POST "https://suggestions.dadata.ru/suggestions/api/4_1/rs/findById/party" \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -H "Authorization: Token $DADATA_API_KEY" \
    -d '{"query": "[ИНН]"}'

  Из ответа DaData извлечь:
  - Полное юридическое название, ИНН, ОГРН
  - Дата регистрации, статус (действующее/ликвидировано)
  - Руководитель (ФИО, должность)
  - Адрес регистрации
  - Уставный капитал
  - Основной ОКВЭД (вид деятельности)
  - Количество сотрудников (если доступно)
  - Финансовые данные из налоговой отчётности (data.finance):
    - Выручка (revenue) за последние доступные годы
    - Расходы (expense)
    - Чистая прибыль/убыток (income)
    - Налоги уплаченные (tax)
    - Долги (debt)
    - Пени (penalty)

  ОБЯЗАТЕЛЬНАЯ ТАБЛИЦА ФИНАНСОВ КОНКУРЕНТОВ:
  | Компания | ИНН | Выручка 2023 | Выручка 2024 | Динамика | Чистая прибыль | Сотрудники |
  Если данные недоступны (иностранная компания, нет в DaData) — пометить "н/д".
  Если компания найдена, но финансовые данные пусты — пометить "[нет данных в отчётности]".

  ВАЖНО: Читать API key из .env, НЕ хардкодить в промптах.
  Команда: source .env && curl -s -X POST ...

Output: src/competitors.md
```

### Agent 4: Media & Advertising Market
```
Sources: akarussia.ru, interactivead.ru (АРИР), sostav.ru, adindex.ru,
         digitalbudget.ru, yandex.ru/adv, ads.vk.com, mediascope.net
Search: "рекламный рынок [country] [year] АКАР", "digital реклама [year]",
        "медиаинфляция", "influencer маркетинг рынок"
Gather:
  Рекламный рынок: объём по каналам (Internet, TV, OOH, Radio, Press)
  Digital подробно: performance, branding, video, classifieds, retail media,
    messengers, influencers, audio ads — инвестиции + инфляция по каждому
  Платформы: MAU/DAU по VK, TG, OK, Dzen, YouTube + аудиторные профили
  Тренды: 5-7 ключевых, ранжированных по влиянию на категорию клиента
  Инвестиции конкурентов: если доступно (Mediascope, Digital Budget)
  Немеряемые каналы: контекст, CPA — агентская оценка

IMPORTANT: Выбирать наиболее значимую информацию. Идти от общего к частному,
от наиболее значимых трендов к наименее. Не рандомно — приоритизировать.
Output: src/media.md
```

### Agent 5: Category Deep-Dive + Predictive Analytics
```
Search: "[category] рынок [country] [year]", "[category] прогноз",
        "[brand] модели продажи", "top-10 [category] продажи"
Gather:
  Рынок категории: объём, динамика, прогнозы экспертов, сезонность
  Top-10 по продажам: динамика YoY + выводы
  Top-10 групп: динамика YoY
  Разбивка бренда по моделям: какие делают объём, какие нишевые
  Предиктивная аналитика: не только фиксация текущего,
    но эстимация трендов на основании данных (куда движется рынок)
  Brand Health proxy (если нет платных данных):
    — оценка динамики UBA на основе медиа-упоминаний + рекламной активности
    — "бренд скорее всего потерял/нарастил знание" с обоснованием
Sources: autostat.ru (или отраслевой аналог), autonews.ru, getbrand.ru
Output: src/category.md
```

### Agent 6: International Benchmarking (if scope includes)
```
Search: "[category] marketing best practices international",
        "influencer marketing [category] case study",
        "digital marketing trends [category] [year]"
Gather: Global best practices, case studies, influencer ROI benchmarks,
        international platform strategies, brand territory examples
Output: src/international.md
```

**Important:** Each agent prompt must specify:
- Exact search queries to run
- Exact data points to collect
- Return format: structured, in Russian, with sources
- Save results to markdown files in the project subfolder (`strategies/brand-name-slug/`)

**Before launching agents:** Create the project folder:
```bash
mkdir -p strategies/brand-name-slug
```

## Phase 2.5: Spot-Check Agent (after all research agents complete)

Launch **1 verification agent** that:
1. Reads all research files
2. Extracts 10 most critical numbers (prices, market sizes, visitor counts, exchange rates)
3. Cross-checks each using **source priority hierarchy:**
   - **P1: Official brand site via WebFetch** (tickets.brand.ru, brand.ru/tariffs/) — for prices, services
   - **P2: Government/regulator via WebFetch** (cbr.ru, rosstat.gov.ru, mos.ru) — for macro data
   - **P3: Industry associations** (АКАР, Mediascope) — for market sizes
   - **P4: Quality media** (RBC, Interfax, TASS) — for confirmed facts
   - **P5: Aggregators/blogs** — LAST resort, treat as potentially outdated
4. **CRITICAL for prices:** ALWAYS WebFetch the official ticket/pricing URL. Aggregators often have OLD prices.
5. Each verified number MUST have a **clickable source URL** in the report
6. If sources disagree — trust higher-priority source, flag the discrepancy
7. Saves verification report to `strategies/<slug>/verification.md`

### Verification Report Format
Each row must have a clickable URL:
```markdown
| # | Показатель | В исследовании | Проверено | Статус | Источник |
|---|-----------|---------------|-----------|--------|---------|
| 1 | Цена билета | 950 ₽ | 950 ₽ | ✓ | [tickets.moscow-sun.ru](https://tickets.moscow-sun.ru) |
```

### Data Freshness Tags

Every key number in the strategy MUST have a date tag:

```markdown
Рынок ИИ в России — **$2,1 млрд** (2025, РБК/Smart Ranking)
Посещаемость — **28 млн** (2025, ВДНХ официально)
Ключевая ставка — **15,00%** (март 2026, ЦБ РФ)
```

### Confidence Scoring

Mark data reliability in research files:

| Score | Meaning | When to use |
|---|---|---|
| `[✓ подтверждено]` | Verified from 2+ primary sources | Official stats, company reports |
| `[~ оценка]` | Single source or analytical estimate | Market projections, expert opinions |
| `[? требует проверки]` | Conflicting data or unverified | When sources disagree |

### Footnote Citations (MANDATORY)

**EVERY fact, statistic, or data point in the strategy MUST have a numbered footnote citation.**

This is non-negotiable. A strategy without verifiable sources has zero credibility.

**How it works:**

1. **In-text:** Each fact gets a superscript number linking to the sources page:
```markdown
Рынок алготрейдинга оценивается в $20-25 млрд ⁽¹⁾, при этом ритейл-сегмент растёт на 12.7% CAGR ⁽²⁾.
```

2. **Sources registry:** Each research agent MUST maintain a numbered source list as it works. When writing src/*.md files, include sources inline as footnotes.

3. **Master sources file:** After all research and writing is complete, create `src/sources.md` — a single consolidated list of ALL sources used in the strategy:

```markdown
# Источники

1. Technavio, "Algorithmic Trading Market 2025-2030", technavio.com, 2025
2. Grand View Research, "Retail Algorithmic Trading Market Report", grandviewresearch.com, 2024
3. Crypto.com, "Crypto Market Sizing Report", crypto.com/research, Jan 2025
4. J.P. Morgan, "Retail Trading Flows", jpmorgan.com, Q1 2025
...
```

Each entry MUST include: author/org, title/description, domain, date.

4. **In strategy.md:** Use superscript numbers `⁽¹⁾` `⁽²⁾` `⁽³⁾` after every fact or number. The number corresponds to the entry in sources.md.

5. **In HTML (index.html):** Footnote numbers are rendered as clickable superscript links `<sup><a href="sources.html#src-1">[1]</a></sup>` that jump to the sources page.

6. **sources.html:** A dedicated styled HTML page listing all sources with anchors (`id="src-1"`, `id="src-2"`, etc.), using the same design system as other doc-*.html pages.

**Research agent source tracking:**

Each research agent prompt MUST include this instruction:
```
IMPORTANT: For every fact or number you include, record the source with:
- Sequential number
- Author/organization
- Title or description
- URL (full)
- Date accessed or published

Format at the end of your output:
## Sources
1. [Author], "[Title]", [url], [date]
2. ...
```

**Minimum citation density:** At least 1 citation per paragraph in analytics sections. Tables with data must cite sources in a footer row or per-cell.

## Phase 3: Write Analytics Block (Block 1)

Write sections based on research data. Each section must have:
- Real data with sources and **date tags**
- Tables and comparisons
- Key insights/conclusions (boxed)
- **Confidence scores** on critical numbers

### Structure

```markdown
# БЛОК 1. АНАЛИТИКА

## 1.1. Экономика, рынок, потребитель
- Macro table (GDP, inflation, income, key rate) — ALL with date tags
- How market and consumer react
- Consumer portrait (what drives decisions)
- Key insight box

## 1.2. Обзор медиа рынка
- Ad market size by channel (table)
- Media inflation by channel
- Key trends (5-7 trends with data)
- Platform audiences table
- Influencer market overview with pricing tiers

## 1.3. Фокус на категорию
- Market size and growth
- Key trends in category
- Seasonality chart (by month)
- Relevant platform/ecosystem data

## 1.4. Анализ конкурентов
- Comparison table (format, price, visitors, USP, social media)
- Detailed analysis of top 5+
- SMM comparison
- Price positioning map
- Competitive conclusions

## 1.5. Международная аналитика (if in scope)
- International best practices (3-5 case studies with FINANCIALS)
- Digital marketing trends with ROI data
- Influencer case studies with concrete ROI numbers
- Foreign audience attraction strategies

## 1.6. SWOT-анализ бренда
- Strengths (5-8 items)
- Weaknesses (5-6 items)
- Opportunities (5-9 items)
- Threats (5-7 items)
- SWOT strategy matrix (SO, ST, WO, WT)
```

## Phase 4: Write Strategy Block (Block 2)

### 2.0. Goals Cascade (REQUIRED — BEFORE strategy sections)

```markdown
## КАСКАД ЦЕЛЕЙ

| Бизнес-цели | Маркетинговые цели | Коммуникационные задачи | КАК | KPI |
|-------------|-------------------|------------------------|-----|-----|
| Рост продаж и доли рынка | Brand Push: рост знания | Увеличить UBA, построить образ бренда | Media pressure, content strategy, KOL | UBA +X%, Reach Y |
| Рост прибыли | Sales Push: конверсия | Работа по всем этапам воронки | Performance, e-comm, регионы | Leads +X%, Conv +Y% |
| ... | Ответ конкурентам | Отстроиться от конкурентов | Уникальное позиционирование | SOV, Share of Voice |
```

**IMPORTANT:** Даже если клиент не сформулировал бизнес-цели, стратег должен их вывести из контекста и данных аналитики. Каскад показывает, как каждая бизнес-цель трансформируется в конкретные действия и метрики.

### 2.0.5. Big Creative Idea (REQUIRED)

Before content strategy, formulate a **Big Creative Idea** — центральный креативный концепт, из которого вытекают все рубрики, TOV, и визуальный стиль.

```markdown
## BIG IDEA

**Инсайт:** [наблюдение о потребителе/рынке, на котором строится идея]
**Идея:** [формулировка в 1-2 предложения]
**Слоган/хештег:** [короткая формула]
**Почему это работает для [бренд]:** [связь с позиционированием и ЦА]
```

### Structure

```markdown
# БЛОК 2. СТРАТЕГИЯ

## 2.1. Стратегические задачи
- 3-5 tasks, each with:
  - Current situation
  - Target state
  - Key metric

## 2.2. Коммуникационная территория и ЦА

### Positioning Formula (REQUIRED)
Use this format: "Для [кого], кто [проблема], [бренд] — это [категория], которая [отличие], в отличие от [альтернатива]"

- Brand platform (essence, emotional territory, rational proof, TOV)
- **TOV-матрица** (table: "Мы говорим" | "Мы НЕ говорим")
- Key messages by audience segment
- Audience segments with portraits (3-5 segments):
  - Demographics, income, behavior, motivation
  - Channels, share of visitors, average spend
- **Customer Journey Map** for primary segment:
  - Awareness → Interest → Consideration → Purchase → Loyalty
  - Touchpoints at each stage
  - Content/channel at each stage

## 2.3. Каналы продвижения

### Platform-Specific Strategy (REQUIRED for each active platform)
For each platform (VK, Telegram, OK, Dzen, YouTube, etc.):
- **Роль платформы** в коммуникационной экосистеме бренда
- **Целевая аудитория** платформы (кто, зачем, как потребляет)
- **Форматы** — какие работают именно здесь
- **Частота** — сколько постов/stories/видео в неделю/месяц
- **KPI платформы** — подписчики, ER%, reach, конверсии

### Content Strategy
- **Content balance AS IS → TO BE** (если бренд уже ведёт соцсети):
  | Тип контента | Сейчас % | Будет % |
- **Branded рубрики** (REQUIRED) — именованные рубрики, привязанные к Big Idea:
  | Рубрика | Описание | Формат | Направление | Постов/мес |
  Каждая рубрика должна иметь уникальное имя, связанное с Big Idea бренда.
- Media mix by funnel stage (awareness → consideration → conversion → loyalty)
- **TOFU/MOFU/BOFU content breakdown:**
  - TOFU (Top of Funnel): awareness content types
  - MOFU (Middle): consideration content types
  - BOFU (Bottom): conversion content types
- **Content plan for Month 1:** 20+ specific topics (не просто частоты — конкретные темы по дням)
- Content calendar by channel (format, frequency)
- Influencer recommendations:
  - Strategy (micro/macro split)
  - Screening criteria
  - Priority categories with pool sizes
  - Integration formats
- **Tech Stack recommendation:** CRM, analytics, email service, UTM convention

## 2.4. Креативные направления (if in scope)
- 2-3 creative vectors, each with:
  - Name/concept
  - Core message
  - TOV
  - Hashtag
  - **3 example social media posts** (actual text + visual description)
  - **1 Reels/video scenario** (brief script)

## 2.5. Бюджет
- Distribution by channel (% and absolute, for 3/6/12 months)
- Distribution by funnel stage
- Monthly pulsation model (seasonality index)
- Rationale for each line item
- **A/B test budget** for first 4-6 weeks (5-10% of total)
- For budgets 20M+: include **Reach/Frequency estimates** per channel

## 2.6. KPI (Progressive, Competitive)
- **Прогрессивные KPI с конкурентным бенчмарком:**
  | Метрика | Сейчас | Через 3 мес | Через 6 мес | Через 12 мес | Бенчмарк (конкурент) |
  Привязка к конкретным конкурентам: "сократить отставание от Top-3 на 50%",
  "войти в Top-3 по [метрика]", "сократить gap от Top-2"
- KPIs per strategic task (current → target, with competitor reference)
- Influencer KPIs
- Content KPIs per platform
- Measurement instruments for each
- **Recommended analytics tools** (free + paid options)

## 2.7. Brand Territory (if in scope)
- Карта культурных/событийных партнёрств, соответствующих ДНК бренда
- Категории: мероприятия, фестивали, музеи, спортивные события, бренды-партнёры
- Обоснование: почему каждое партнёрство усиливает позиционирование

## 2.8. Hero Project (if in scope)
- 1-2 флагманских спецпроекта с:
  - Концепция и название
  - Формат (серия видео, роадшоу, спецпроект со СМИ, коллаборация)
  - Timeline
  - Бюджет (детализация по статьям)
  - KPI projections (reach, views, media value)
  - Projected ROI
  - Что нужно от клиента (машины, доступ, спикеры)
```

## Phase 4.1: ORM/SERM Block (if disciplines.orm = true)

```markdown
# БЛОК 3. ORM/SERM

## 3.1. Текущее состояние репутации
- Social Listening данные (Brand Analytics или аналог):
  | Метрика | Бренд | Бенчмарк (среднее конкурентов) | Место |
  | Mentions | ... | ... | ... |
  | Positive % | ... | ... | ... |
  | NSI (Net Sentiment Index) | ... | ... | ... |
  | Engagement | ... | ... | ... |
- Sentiment heatmap по темам (качество, цена, технологии, дилеры и т.д.)
- Top источники по sentiment (YouTube, Drive2, VK, Telegram, Dzen)

## 3.2. Ключевые проблемы и барьеры
- Что мешает покупке? (негативные отзывы, отсутствие информации, слабый SERP)
- Где бренд проигрывает конкурентам по восприятию?

## 3.3. Стратегия ORM
- Работа с отзывами (авто-площадки, карты, маркетплейсы)
- SERM: оптимизация поисковой выдачи
- Работа с YouTube: комментарии, реакции, поддержка органического контента
- Работа с картами (Яндекс.Карты, 2ГИС): рейтинги дилеров
- Мониторинг и быстрое реагирование (SLA: время ответа)
- Антикризисный план

## 3.4. KPI ORM
- Прогрессивные цели по positive mentions, NSI, рейтингам на площадках
- Привязка к конкурентным позициям
```

## Phase 4.2: PR Block (if disciplines.pr = true)

```markdown
# БЛОК 4. PR

## 4.1. Медиа-ландшафт
- Доля публикаций бренда vs конкуренты (по данным Mediascope/Медиалогия)
- Sentiment анализ по уровням СМИ (федеральные, региональные, отраслевые, блоги)
- Доля brand-owned news (инициированных брендом vs реактивных)
- Активность в блогосфере

## 4.2. Ключевые выводы
- Где бренд проигрывает конкурентам в медиа-поле
- Что журналисты думают о бренде (если есть опрос/фидбэк)
- Возможности для роста

## 4.3. PR-стратегия
- Медиа-отношения: Tier 1-2-3 СМИ, автомедиа, региональная пресса
- Генерация инфоповодов: коллаборации, рейтинги, исследования, инфографика
- Работа с журналистами: Press Circle Club, пресс-туры, эксклюзивы
- Brand Territory: партнёрства с событиями/институциями
- Спецпроекты со СМИ (концепция + бюджет)

## 4.4. KPI PR
- Количество публикаций (позитивных/нейтральных)
- Доля в медиа-поле vs конкуренты
- Количество лояльных СМИ
- ROI спецпроектов (ADE/Cost)
```

## Phase 4.3: Ecosystem & Synergy (if multiple disciplines)

```markdown
## ЭКОСИСТЕМА

Показать, как SMM, ORM и PR работают в синергии:
- SMM генерирует контент → ORM использует для работы с отзывами
- ORM выявляет барьеры → SMM/PR адресует их в коммуникациях
- PR создаёт инфоповоды → SMM амплифицирует в соцсетях
- KOL/UGC discovery → используется во всех дисциплинах

Визуализировать как экосистемную диаграмму в HTML.
```

## Phase 4.5: Executive Summary + Action Plan (NEW — REQUIRED)

After writing Blocks 1 and 2, add these sections at the TOP of the strategy document:

### Executive Summary (1 page max)
```markdown
# EXECUTIVE SUMMARY

**Бренд:** [название]
**Цель:** [одно предложение]
**Ключевой инсайт:** [главный вывод из аналитики]
**Стратегический подход:** [2-3 предложения]
**Бюджет:** [рекомендованный сценарий]
**Ожидаемый результат к [горизонт]:** [3-5 ключевых KPI]
```

### Первые 7 дней — чек-лист (REQUIRED)
```markdown
# ЧТО ДЕЛАТЬ В ПЕРВЫЕ 7 ДНЕЙ

- [ ] День 1-2: [конкретное действие]
- [ ] День 1-2: [конкретное действие]
- [ ] День 3-4: [конкретное действие]
- [ ] День 3-4: [конкретное действие]
- [ ] День 5-7: [конкретное действие]
- [ ] День 5-7: [конкретное действие]
- [ ] День 5-7: [конкретное действие]
- [ ] День 5-7: [конкретное действие]
- [ ] День 5-7: [конкретное действие]
- [ ] День 5-7: [конкретное действие]
```

### Execution Roadmap — по неделям (REQUIRED)
```markdown
# РОАДМАП: ПЕРВЫЕ 12 НЕДЕЛЬ

| Неделя | Фокус | Ключевые действия | Результат |
|--------|-------|-------------------|-----------|
| 1 | Запуск | ... | ... |
| 2 | Контент | ... | ... |
| 3-4 | Первые лиды | ... | ... |
| 5-8 | Масштабирование | ... | ... |
| 9-12 | Оптимизация | ... | ... |
```

## Project Structure

Each strategy gets its own dedicated folder with a **unique name**. Never mix strategies in one directory.

### Folder Naming

Generate a unique folder name: `<brand-slug>-<YYYYMMDD>` (e.g., `solntse-moskvy-20260327`, `tokenlab-20260328`). If the folder already exists, append a counter: `-v2`, `-v3`.

```
strategies/
  solntse-moskvy-20260327/
    brief.yaml               # Saved brief for re-runs
    index.html               # Main strategy website (deployed to Vercel)
    doc-strategy.html         # Full strategy text (markdown rendered)
    doc-competitors.html      # Competitor research
    doc-media.html            # Media market research
    doc-international.html    # International benchmarks (if applicable)
    sources.html              # All sources with numbered anchors (linked from footnotes)
    raw/                      # Raw source data from research agents
      perplexity_macro.md     # Raw Perplexity/AI-aggregated data
      perplexity_competitors.md
      websearch_results.md    # Raw WebSearch results
    src/                      # Source markdown files (NOT linked from HTML)
      strategy.md
      competitors.md
      media_market.md
      international.md
      sources.md              # Master list of all numbered sources
```

**IMPORTANT:** The `src/` folder contains source markdown — these are working files, NOT for end users. The `index.html` appendix section must link ONLY to `doc-*.html` files, NEVER to `.md` files. End users see only HTML.

Create the folder at the start of Phase 2 (research), save all research files into `src/` and raw data into `raw/`.

## Phase 5: HTML Presentation

**REQUIRED:** Invoke the `frontend-design` skill before generating the HTML site. The site must follow frontend-design principles — distinctive, production-grade, NOT generic AI aesthetics. Use Tailwind CSS.

### Design Process

1. **Invoke `frontend-design` skill** — load the design guidelines
2. **Choose aesthetic direction** based on the brand's identity:
   - Luxury/premium brand → dark theme, serif display fonts, gold/warm accents
   - Tech/SaaS → clean, minimal, monospace accents, cool tones
   - Consumer/playful → bright colors, rounded shapes, animated elements
   - Corporate/B2B → structured, authoritative, navy/deep blue palette
3. **Adapt accent colors** to the brand's own colors if brand guidelines exist
4. **Generate site** with Tailwind CSS via CDN

### Design System Defaults (adapt per brand)

**Fonts (Google Fonts, must support Cyrillic if Russian):**
- Display (h1): distinctive serif or display font (NOT Inter, NOT Roboto)
- Headings (h2-h4): geometric or modern sans
- Body: refined readable sans

**Layout Patterns:**
- Alternate dark/light sections for visual rhythm
- `max-w-7xl` container with `px-6`
- Stat cards with glass-morphism or brand-appropriate effect
- Horizontal bar charts for percentages
- Vertical bar charts for seasonality (use fixed px height on container, not % on children)
- SWOT as 2x2 colored grid

**CRITICAL — Bar/Chart Rendering Rules (applies to ALL animated bars and charts):**
- Animated bar fills MUST have explicit `width: 0` (horizontal) or `height: 0` (vertical) in CSS as initial state — otherwise CSS transitions have no start value and render nothing
- Container elements (`.bar`, chart wrappers) MUST use fixed `px` dimensions, NEVER percentage-based height/width that depends on content
- Vertical bars: set container to fixed `height: 200px` (or similar), bars use `height: X%` inside it
- Horizontal bars: container is full width, bar fills use `width: 0` initial → animated to target `width: X%`
- Always test: if an element animates from A to B, both A and B must be explicitly defined in CSS — browsers cannot transition from `auto` or missing values
- IntersectionObserver pattern: set `data-width` or `data-height` on bar elements, apply via JS when visible. CSS must define the `0` starting point.
- Example correct pattern:
  ```css
  .bar { height: 8px; width: 100%; background: rgba(0,0,0,0.1); }
  .bar-fill { height: 100%; width: 0; transition: width 1.2s ease; }
  ```

**Animations:**
- Scroll-reveal with IntersectionObserver
- Animated counters for key numbers
- Hover effects on cards (translateY + border glow)
- Sticky navbar with scroll-triggered background

**Required Sections:**
1. Hero (brand name, key stats, tagline)
2. Block 1 header
3. Sections 1.1-1.6 (alternating dark/light)
4. Block 2 header
5. Sections 2.1-2.6
6. Appendix: links to research documents + sources page
7. Footer

**Footnote Citations in HTML:**
- Every fact/number in index.html must have a superscript link: `<sup><a href="sources.html#src-N" class="footnote">[N]</a></sup>`
- Style footnotes: small, accent color, no underline, hover underline
- The `sources.html` page lists all sources with anchor IDs (`id="src-1"`, `id="src-2"`, etc.)
- sources.html uses the same design system as doc-*.html pages
- sources.html is linked from the appendix section AND from every footnote

**Tech Stack:**
- Tailwind CSS via CDN (`https://cdn.tailwindcss.com`)
- Vanilla JS (no build step)
- Single `index.html` file
- Marked.js CDN for research doc pages

### Research Document Pages

For each research file, create a styled HTML viewer:
- Same design system (fonts, colors, theme)
- Sticky nav with back link to main page
- Markdown rendered via marked.js with styled output
- Color-coded accent per document type

### Deployment to Vercel

**MANDATORY: Deploy automatically after HTML generation is complete.** Do not wait for user to ask — deploy immediately after all HTML files are created and verified.

```bash
# Navigate to project folder
cd strategies/brand-name-slug/

# Deploy to Vercel production — each strategy gets its own Vercel project
npx --yes vercel deploy --prod --yes
```

**Post-deploy:** Include the Vercel URL prominently in the final output. Format:
```
Стратегия задеплоена: https://brand-slug-xxxxx.vercel.app
```

**Re-deploy after fixes:** If user requests changes to content or design, re-deploy after applying fixes. Always show the updated URL.

## Adapting to Different Industries

The structure is universal. Adapt research queries and category focus:

| Industry | Category Focus (1.3) | Special Research |
|----------|---------------------|-----------------|
| FMCG | Retail trends, shelf share | Nielsen/GfK data, retail media |
| Real Estate | Market dynamics, mortgage rates | Developer competition, Yandex Realty |
| Entertainment | Attendance, seasonality | Event marketing, tourism stats |
| E-commerce | GMV, conversion benchmarks | Marketplace analytics, CPA benchmarks |
| B2B/SaaS | Market penetration, CAC/LTV | LinkedIn, content marketing benchmarks |
| HoReCa | Foot traffic, review platforms | 2GIS/Yandex Maps, delivery platforms |
| Fashion | Trend cycles, brand perception | Fashion media, lookbook strategies |

## Red Flags — STOP

- Starting to write strategy without finishing discovery → STOP, ask remaining questions
- Writing recommendations without research data → STOP, launch research agents first
- Skipping SWOT → STOP, it connects analytics to strategy
- Budget without channel rationale → STOP, each line needs justification
- KPIs without measurement instruments → STOP, unmeasurable KPIs are useless
- Deploying without user review → STOP, present draft for approval first

## Quality Checklist

### Data Quality
- [ ] All discovery questions answered (or extracted from brief)
- [ ] Scope confirmed with user
- [ ] Brief saved to `brief.yaml`
- [ ] 4+ research agents completed with real data
- [ ] Spot-check agent verified 10 key numbers
- [ ] All critical numbers have date tags
- [ ] No Perplexity/AI tool mentions in output files
- [ ] All analytics sections have sources
- [ ] Every fact/number has a footnote citation ⁽ⁿ⁾
- [ ] src/sources.md created with all numbered sources
- [ ] sources.html created with anchor IDs for each source

### Content Completeness
- [ ] Executive Summary present (1 page)
- [ ] "First 7 Days" checklist present (10 items)
- [ ] Weekly Roadmap present (12 weeks)
- [ ] Competitor table has 5+ entries
- [ ] SWOT has 5+ items per quadrant
- [ ] Positioning uses formula (Для кого / Проблема / Категория / Отличие)
- [ ] TOV matrix present (говорим / НЕ говорим)
- [ ] Customer Journey Map present for primary segment
- [ ] TOFU/MOFU/BOFU content breakdown present
- [ ] Month 1 content plan has 20 specific topics
- [ ] Creative vectors have 3 example posts each
- [ ] Strategic tasks have metrics
- [ ] Channel recommendations have frequency and formats
- [ ] Budget adds up to 100%
- [ ] A/B test budget allocated (5-10%)
- [ ] KPIs have current/target/instrument
- [ ] Tech Stack recommended (CRM, analytics, email)

### Delivery
- [ ] HTML renders correctly in browser (dark bg works!)
- [ ] All doc-*.html pages link correctly from index.html
- [ ] No .md files linked from index.html (only doc-*.html)
- [ ] Body tag has `style="background-color:... !important;"` on ALL pages
- [ ] Footnote links `[N]` in index.html point to sources.html#src-N
- [ ] sources.html has all numbered sources with anchor IDs
- [ ] Deployed to Vercel, URL shared with user
