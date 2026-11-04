# УРОВЕНЬ 2 / ИЗМЕРЕНИЕ 02: ФУНКЦИОНАЛЬНО-ЦЕЛЕВОЕ

> **Уровень:** L2 — Среда (инфраструктура)  
> **Измерение:** D02 — Instruments  
> **Версия:** 4.0  
> **Статус:** Финал v4.0 | Дата: 02.04.2026  
> **Зависимости:** L1/D02, L1/D09, L2/D01, L2/D05, L2/D06, L2/D08, L2/D09

---

## Ключевой вопрос

**Какие типы активностей, рынков и агентов эта среда поддерживает и для каких целей она создана?**

---

## Аннотация

Этот черновик отвечает на вопрос `L2/D02` через шесть связанных тем: миссию среды и семь системных проблем, типологию агентов, четыре маркетплейса и классы активностей, dual-token архитектуру `AGC` + `Qi Energy`, модель `Revenue Distribution` и правовую границу токенов по `MiCA`. Основа ответа сохраняет ядро [PREVIOUS VERSION], но перестраивается под текущий template, нормализованный корпус `C1-C12` и ownership-правила: где `D02` владеет концептом, описание даётся полно; где owner находится в другом измерении, здесь остаётся только рабочая проекция с проверенной ссылкой на владельца. [C1][C5][C9]

Ключевой тезис этой версии таков: N4Y.AI создаётся не как "ещё один рынок AI-услуг", а как управляемая среда для `Augmented Economy`, где агенты, рынки и денежные инструменты собраны в единую инфраструктуру выполнения задач, обмена ресурсами, переноса знаний и накопления доверия. При этом операционная ликвидность и скорость оборота принадлежат `AGC`, а долгосрочная координация ценности, governance и инвестиционного интереса вынесены в `Qi Energy`; именно это разделение делает инструментальный слой совместимым и с economic design, и с будущей regulatory boundary. [C1][C8][C9][C10]

---

## Содержание

1. [Миссия и семь системных проблем](#1-миссия-и-семь-системных-проблем)
2. [Типология агентов](#2-типология-агентов)
3. [Четыре маркетплейса и активности](#3-четыре-маркетплейса-и-активности)
4. [Токеномика и стимулы (проекция)](#4-токеномика-и-стимулы-проекция)

---

## 1. Миссия и семь системных проблем

### 1.1 Миссия инструментального слоя

Миссия `L2/D02` состоит в том, чтобы превратить спонтанную агентную экономику из набора несвязанных действий в среду, где работа, ресурсы, знания и trust-signals могут циркулировать через понятные инструменты, а не только через ad hoc интеграции. В логике стратегии это и есть `Augmented Economy`: AI-агенты не подменяют человеческую экономику, а расширяют её, создавая новый слой продуктивности, который должен быть управляемым, оплачиваемым и наблюдаемым. [C9][C10]

На уровне инструментов это означает три вещи. Во-первых, среда должна поддерживать несколько типов участников и не сводить все роли к "исполнителю задачи". Во-вторых, ей нужны разные рынки под разные типы стоимости: work, infrastructure, knowledge, trust. В-третьих, ей нужен отдельный monetary stack, где быстрая операционная валюта не смешивается с governance и value-capture активом. [C1][C8][C9]

### 1.2 Семь системных проблем как design rationale

В `D02` семь системных проблем читаются не как самостоятельные ownership-объекты, а как причина, по которой инструментальный слой вообще должен существовать. Корпус сильнее всего поддерживает identity/accountability failures, escrow/evaluator-based settlement и governance controls; detailed reputation mathematics, auction tuning и microsandbox expansion остаются зависимостями других измерений. [C9][C10][C11]

| Проблема | Что ломается в спонтанной агентной экономике | Инструментальный ответ среды | Доминирующий маркетплейс |
|---|---|---|---|
| **P1. Идентичность и ответственность** | Агенты выглядят как анонимные endpoints, а shared keys размывают субъекта действия | Identity-bound entry, actor typing, settlement с верифицируемыми ролями, репутационные проекции | **Reputation Marketplace** + Task |
| **P2. Проверка качества** | Нельзя надёжно отделить выполненную работу от имитации результата | Escrow/evaluator pattern, verification primitives, refund/dispute routing | **Task Marketplace** |
| **P3. Справедливое ценообразование** | Цена не отражает сложность, риск и скорость исполнения | Разделение рынков по типу актива, quotes/holds/settlement, отдельные payment rails | **Task** + **Resource Marketplace** |
| **P4. Защита знаний и IP** | Знание легко копируется, а provenance исчезает | Отдельный knowledge market, контрактная упаковка transfer units | **Knowledge Marketplace** |
| **P5. Распределение ресурсов** | Workload и compute смешиваются, вызывая неэффективный match | Разделение task- и resource-layer, SLA-проекции, throughput-aware routing | **Resource Marketplace** |
| **P6. Споры и исполнение обязательств** | После сбоя нет согласованного механизма возврата или переоценки | Job-state model, refund/verification/dispute primitives | **Task** + **Reputation Marketplace** |
| **P7. Governance и public goods** | Среда не может финансировать общие функции и правила только рыночной комиссией | Revenue Distribution, bicameral governance projection, grants/public goods sink | **Reputation Marketplace** + governance layer |

### 1.3 Маппинг P1-P7 на маркетплейсы

`Task Marketplace` отвечает прежде всего за `P2`, `P3` и `P6`: здесь находятся discover-execute-settle loops, escrow/evaluator logic и качество исполнения как предмет оплаты. `Resource Marketplace` закрывает `P3` и `P5`, когда узким местом становится не сама задача, а доступ к compute, data, model endpoints или другой capacity. `Knowledge Marketplace` делает видимыми `P4` и часть `P3`, потому что знание должно передаваться как лицензируемый и оцениваемый asset, а не как бесхозное копирование. `Reputation Marketplace` служит перекрёстным слоем для `P1`, `P2`, `P6` и `P7`, потому что без переносимых attestations и trust-signals ни match, ни dispute resolution, ни public governance не удерживают качество среды. [C9][C10][C11]

Этот маппинг не означает, что каждая проблема решается одним рынком. Напротив, логика `Augmented Economy` как раз в том, что каждая системная проблема пересекает несколько инструментов: identity влияет на settlement, settlement влияет на reputation, reputation влияет на pricing, а pricing и governance влияют на распределение доходов и устойчивость токена. Поэтому `D02` нужно читать как слой связки между рынками, а не как перечень независимых сервисов. [C8][C9][C10]

### 1.4 Граница с онтологией среды

Полное определение среды как `Managed Decentralized Digital Economy` принадлежит `→ L2/D01 §1`. Здесь это определение используется только как boundary context: `D02` отвечает не на вопрос "что это за среда", а на вопрос "какие целевые инструменты делают эту среду пригодной для работы". Поэтому `Microsandbox`, permeability и boundary logic не раскрываются полно, а используются как внешнее ограничение на то, где и как работают описываемые ниже рынки. → `L2/D01 §1`, `L2/D01 §2` [C9][C10]

---

## 2. Типология агентов

### 2.1 Почему D02 различает пять типов агентов

Инструментальный слой не может опираться на одну универсальную фигуру "агента", потому что разные рынки требуют разных субъектов: кто-то владеет бюджетом и правами, кто-то исполняет задачу, кто-то поставляет инфраструктуру, кто-то наблюдает и оценивает, а кто-то обслуживает саму среду. Поэтому рабочая типология `D02` использует пять типов: `Personal Logos`, `Corporate Logos`, `Microagent`, `Observer` и `Service Agent`. Эта типология является функциональной: она нужна для разграничения market roles, payment privileges и governance entry points, а не для конкурирующей identity-онтологии. [C9][C10]

При этом `ESq` и competence semantics не принадлежат этому измерению. `D02` использует `ESq` только как внешний permission gate для того, какие специализации и workload profiles может нести агент, но каноническая семантика `ESq`, `ESc` и `dual constraint` принадлежит `→ L1/D02 §5`. [C10]

### 2.2 Пять канонических типов

| Тип | Функция в среде | Основные рынки | Платёжная роль | Governance-проекция |
|---|---|---|---|---|
| **Personal Logos** | Персональный оператор, инициатор задач, владелец профиля, получатель делегированных прав | Task, Knowledge, Reputation | Платит и получает `AGC`, может удерживать `Qi` | Через `Qi Chamber` / `Citizens' Chamber` по правилам → `L2/D09 §1-§2` |
| **Corporate Logos** | Представитель организации, закупщик и поставщик на больших объёмах, policy-bound actor | Task, Resource, Knowledge | Крупные расчёты, treasury-like flows, корпоративные subscriptions | Участие в governance остаётся policy-sensitive → `OI-052`, `L2/D09 §1`, `L2/D09 §6` |
| **Microagent** | Узкоспециализированный исполняющий или вспомогательный субагент | Task, Resource | Не автономный treasury owner; действует в бюджете principal | Прямого governance-role не имеет |
| **Observer** | Внешний или внутренний наблюдатель, evaluator, auditor, attestor | Reputation, Task | Может получать оплату за verification/attestation | Участвует через attestation layer, не как базовый treasury actor |
| **Service Agent** | Инфраструктурный поставщик capability: routing, discovery, hosting, analytics, gateways | Resource, Task, Reputation | Получает `AGC` за service delivery, SLA-bound capacity | Governance-интерес возможен как provider class, но подробно → `L2/D09 §1`, `L2/D09 §5` |

### 2.3 Роли агентов относительно рынков

`Personal Logos` и `Corporate Logos` являются principal actors: именно они чаще всего открывают задачи, владеют долгосрочной целью и связаны с внешней идентичностью или организацией. `Microagent` почти всегда выступает delegated executor внутри уже открытого economic flow. `Observer` нужен там, где trust не следует автоматически из результата и требуется независимая оценка, верификация, audit trail или attestation. `Service Agent` закрывает то, что не является конечной business-task, но необходимо для работы среды: discovery, routing, liquidity access, compute-serving, settlement-support. [C8][C9][C10]

Такой набор типов хорошо согласуется с тем, что strongest in-scope evidence для `P1-P7` опирается не на "среднюю репутацию агента вообще", а на комбинацию ролей client/provider/evaluator, trust registries, governance controls и payment-aware execution. Иными словами, типология агентов здесь строится от instrumented interaction patterns, а не от биографии субъекта. [C8][C10][C12]

### 2.4 Матрица взаимодействий

| Кто с кем | Основной тип взаимодействия | Базовый инструмент | Где owner полного механизма |
|---|---|---|---|
| **Personal Logos -> Microagent** | Делегирование выполнения под бюджет и policy principal | Task routing, budgeted `AGC` spending | `ESq`/delegation semantics → `L1/D02 §5`, `L1/D07 §2-§3` |
| **Corporate Logos -> Service Agent** | Закупка инфраструктуры, аналитики, compliance-support | Resource / service procurement | Execution details → `L2/D06 §4-§6` |
| **Personal Logos -> Observer** | Заказ проверки, оценки, attestation | Reputation / verification flow | Reputation logic → `L1/D09 §1-§2` |
| **Corporate Logos -> Personal Logos** | Контрактная кооперация и task procurement | Task / Knowledge exchange | Contract packaging → `L3/D04 §5` |
| **Service Agent -> Service Agent** | Взаимное обслуживание capability chains | Resource / settlement flow | Throughput & settlement → `L2/D06 §6` |

### 2.5 Граница с репутацией и governance

Репутационный профиль каждого типа агента описывается полностью в `→ L1/D09 §1-§2`, а не в `D02`. Здесь важно только то, что разные agent types создают разные входы в `Reputation Marketplace`: для `Microagent` важна наследуемая или приписываемая ответственность, для `Observer` - достоверность attestation, для `Service Agent` - SLA-like надёжность, для `Personal` и `Corporate Logos` - накопление reputation как условия доступа к более дорогим потокам. [C9][C10]

Governance-механика также не разворачивается здесь полно. `D02` фиксирует только то, что economic roles агентов создают разные интересы к распределению доходов и public goods, а полная bicameral design, `Citizens' Chamber`, `Qi Chamber` и `Progressive Decentralization` принадлежат `→ L2/D09 §1`, `→ L2/D09 §2`, `→ L2/D09 §6`. [C11][C12]

---

## 3. Четыре маркетплейса и активности

### 3.1 Почему рынков четыре, а не один

Нормализованный корпус устойчиво поддерживает четыре activity classes для инструментального слоя: `task execution`, `resource trading`, `knowledge transfer` и `reputation/attestation`. Это означает, что среда не должна сводить все exchange patterns к универсальному "task board": разные единицы стоимости требуют разных метрик качества, разных settlement expectations и разных trust assumptions. [C9]

Именно поэтому `D02` различает четыре маркетплейса: `Task Marketplace`, `Resource Marketplace`, `Knowledge Marketplace` и `Reputation Marketplace`. Вместе они покрывают основной контур операций quote/hold/settle/refund/verify/dispute/govern, но распределяют эти примитивы по разным типам активов и ролей. [C8][C9]

### 3.2 Task Marketplace

`Task Marketplace` - это рынок работы как результата. Здесь предметом обмена выступает не инфраструктура и не знание сами по себе, а обещание выполнить конкретную задачу с проверяемым outcome. Типичные activity categories здесь включают: поиск и публикацию задач, capability-matching, quote/proposal, funding/hold, execution, submission, evaluation, refund и dispute routing. [C9][C10]

Полный execution semantics этого рынка не принадлежит `D02`. `CNP`, `ADS`, sandbox execution и settlement pipeline описываются полностью в `→ L2/D06 §3`, `→ L2/D06 §4`, `→ L2/D06 §5`, `→ L2/D06 §6`. Здесь важно только то, что `Task Marketplace` является главным каналом превращения спроса на агентный труд в платежеспособный `AGC`-flow. [C8][C9]

### 3.3 Resource Marketplace

`Resource Marketplace` - это рынок capacity, а не результата. Его предметом являются compute, model endpoints, data access, hosting, orchestration support и другие инфраструктурные возможности, которые нужны для исполнения работы, но сами по себе ещё не являются выполненной задачей. Для этого рынка характерны activities вида discover, allocate, reserve, monitor, re-route и settle usage-based fees. [C8][C9]

Ключевая инструментальная функция этого рынка - разгрузить `Task Marketplace` от всего, что относится к capacity procurement. Иначе task-price начинает смешивать труд, инфраструктуру и risk-premium в один неразличимый поток. Поэтому в `D02` граница между task и resource принципиальна, хотя технически часть execution chain проходит через одни и те же settlement primitives. [C8][C9]

### 3.4 Knowledge Marketplace

`Knowledge Marketplace` - это рынок переносимых knowledge assets: patterns, templates, procedures, research outputs, decision models и других единиц знания, которые могут использоваться повторно. В отличие от task-work, здесь ценность связана не с одноразовым результатом, а с transferability и reuse. [C9]

Базовая структура рынка — двусторонняя: `Knowledge Seller ↔ Knowledge Buyer`. Покупателями могут выступать другие Логосы, корпоративные заказчики, а также внешние участники за пределами экосистемы — в первую очередь производители LLM-инференса (OpenAI, Anthropic, Mistral, локальные inference-провайдеры), заинтересованные в верифицированных domain-specific данных для fine-tuning. Поверх этой базовой модели опционально подключается **верификационный слой**: институциональный верификатор (ВУЗ, лицензированная организация) аттестует качество и происхождение знаний и выдаёт `Knowledge Quality VC`, повышая рыночную цену актива. Верификация не является обязательным условием сделки — решение за покупателем. Incentive-перспектива каждой из сторон, включая двойное вознаграждение продавца (`AGC` + рост `ESq`), описана полностью в `→ L2/D05 §6.4`.

Полная контрактная упаковка knowledge exchange не принадлежит `D02`. `Knowledge Transfer Package`, `Knowledge License (KL)`, license logic и royalty-sensitive детали описываются полностью в `→ L3/D04 §5`. Здесь `D02` фиксирует только инструментальную проекцию: knowledge должно быть представлено как отдельный товарный класс, иначе оно либо исчезает внутри task-pricing, либо утекает в неконтролируемое копирование без provenance и без корректного вознаграждения. [C9][C10]

### 3.5 Reputation Marketplace

`Reputation Marketplace` - это рынок attestations и trust-signals. Он нужен не для "продажи репутации" в бытовом смысле, а для того, чтобы performance, verification outcomes, evaluator judgments и другие quality signals становились переносимыми входами для будущих сделок. В terms of activities здесь важны attestation issuance, score publication, tiering, negative event logging, challenge/review и portability. [C9][C10]

Репутационная логика как таковая не принадлежит `D02`. Пять компонентов профиля, `Performance VC`, on-chain anchor и tier logic описаны полностью в `→ L1/D09 §1`, `→ L1/D09 §2`, `→ L1/D09 §5`. Здесь `Reputation Marketplace` нужен как инструментальная поверхность, через которую trust становится market input для task, resource и knowledge flows. [C10]

### 3.6 Матрица активностей по рынкам

| Маркетплейс | Основной предмет обмена | Типовые активности | Какие типы агентов доминируют |
|---|---|---|---|
| **Task** | Выполнение задач и deliverables | publish, match, quote, fund, execute, verify, refund, dispute | Personal Logos, Corporate Logos, Microagent, Observer |
| **Resource** | Compute, data, model access, infra-capacity | discover, reserve, meter, route, monitor, settle | Corporate Logos, Service Agent, Microagent |
| **Knowledge** | Patterns, research outputs, reusable procedures, packages | list, license, transfer, rate, recombine, royalty-route | Personal Logos, Corporate Logos, Service Agent |
| **Reputation** | Attestations, performance traces, trust signals | attest, publish, tier, challenge, score, review | Observer, Personal Logos, Service Agent, Corporate Logos |

### 3.7 Граничные примечания по open issues

`OI-050` сохраняется как живая граница между `Task Marketplace` и `Resource Marketplace`: bundled offers, где исполнитель продаёт "работу вместе с инфраструктурой", пока не имеют окончательного routing rule. В текущем черновике базовый принцип такой: если покупается outcome, это task-layer; если покупается capacity или capability access, это resource-layer. Но этот принцип ещё должен быть формализован совместно с `→ L2/D06 §5`. [C9]

`OI-049` и `OI-051` также прямо влияют на структуру рынков. Первая проблема касается того, как среда вводит новые activity classes без разрушения текущей типологии четырёх рынков. Вторая - того, когда новый `Microsandbox` требует собственного sector-specific market logic. Обе темы относятся к governance-процессу изменения среды, а не к текущему каноническому ядру `D02`. → `L2/D09 §3`, `L2/D09 §5` [C9][C11]

---

## 4. Токеномика и стимулы (проекция)

> **Допущение [A3, A18, A47, A48]:** `AGC` сохраняет reserve-first EMT posture; `Qi` остаётся supplementary rather than primary collateral; revenue contour stays `40/30/20/10`.

`L2/D02` не владеет dual-token mechanics. Актуальный canonical owner-layer находится в `→ L2/D05 §2–§3, §9`: там описаны `AGC`, `Qi`, peg discipline, reserve boundary, `veQi` economic role и `Revenue Distribution`. Здесь токеномика упоминается только как instrument-layer context для четырёх market surfaces.

В актуальном каноне `L2/D02` нет отдельных tokenomics owner-sections `§5–§6`; после рефакторинга остаётся только эта projection section. Поэтому любые ссылки на `L2/D02 §4–§6` следует считать legacy и заменять на `→ L2/D05 §2–§3, §9`.

Для `D02` достаточно следующей проекции: `AGC` обслуживает расчёты, pricing и escrow-facing flows; `Qi` фиксирует long-horizon governance/value layer; 20% public-goods contour, 10% stability contour и остальные параметры распределения принадлежат owner-text в `L2/D05`, а не этому измерению. [A3][A18][A47][A48]

---
## Связь с другими измерениями

| Измерение | Связь |
|-----------|-------|
| **L1/D02 (Компетентностное)** | `ESq` и `dual constraint` задают внешний permission gate для агентных ролей, но `D02` владеет только платёжной и инструментальной стороной этого ограничения. Полная semantics `ESq` и `ESc` находятся в `→ L1/D02 §5`. |
| **L1/D09 (Репутационное)** | `Reputation Marketplace` использует trust-signals как market input, но не владеет структурой reputation profile, tier logic и attestation semantics. Полный owner-layer находится в `→ L1/D09 §1`, `→ L1/D09 §2`, `→ L1/D09 §5`. |
| **L2/D01 (Онтология)** | `D02` использует среду как `Managed Decentralized Digital Economy`, но не определяет её границы и онтологический статус. Эти ответы принадлежат `→ L2/D01 §1`, `→ L2/D01 §2`. |
| **L2/D05 (Экономика и стимулы)** | Dual-token архитектура (`AGC`, `Qi`, `veQi`), `Revenue Distribution` (`40/30/20/10`) и `MiCA`-статус токенов полностью принадлежат `→ L2/D05 §2–§3, §9`. `D02` описывает только инструментальную проекцию: два токена как операционный и governance-layer среды. |
| **L2/D06 (Координация)** | `Task Marketplace` опирается на `CNP`, `ADS`, sandbox execution и settlement, но их полный механизм принадлежит `→ L2/D06 §3`, `→ L2/D06 §4`, `→ L2/D06 §5`, `→ L2/D06 §6`. |
| **L2/D08 (Нормативно-правовое)** | `AGC` и `Qi` в `D02` описываются как инструменты среды, но их `MiCA`-квалификация, `EMI` boundary и PSD2/MiCA interplay принадлежат `→ L2/D08 §3` [A18]. |
| **L2/D09 (Governance)** | `Revenue Distribution` в `D02` создаёт economic substrate для governance, но `Qi Chamber`, `Citizens' Chamber` и `Progressive Decentralization` как owner-концепты принадлежат `→ L2/D09 §1`, `→ L2/D09 §2`, `→ L2/D09 §6`. |
| **L3/D04 (Контрактное)** | `Knowledge Marketplace` требует контрактной упаковки transfer units, однако `Knowledge Transfer Package` и licensing logic принадлежат `→ L3/D04 §5`. |

---

## Открытые вопросы

### Требуют решения (Critical / High)

На текущем шаге новые `Critical/High` вопросы не добавляются: instrument-layer core собран, а оставшиеся высокорисковые regulatory и governance вопросы уже принадлежат владельцам `→ L2/D08` и `→ L2/D09`.

### Требуют исследования (Medium)

1. **[Medium] OI-049: эмерджентные типы активностей и рынков.** Текущая каноническая модель поддерживает четыре рынка, но не даёт окончательного правила, как вводить новые activity classes без распада taxonomic baseline.
   > **Open.** Нужен governance-controlled механизм расширения типологии через `AIP`/owner-governance path, а не ad hoc добавления новых маркетплейсов. [C9][C11]

2. **[Medium] OI-050: граница между Task Marketplace и Resource Marketplace.** В bundled offers остаётся не до конца формализованным, считать ли продаваемый объект результатом работы или доступом к capacity.
   > **Open.** Текущий draft использует правило "outcome -> Task; capacity -> Resource", но routing rule должен быть согласован с execution owner `→ L2/D06 §5`. [C9]

3. **[Medium] OI-051: добавление нового Microsandbox для новых секторов.** Неясно, когда новый сектор требует только policy overlay, а когда - отдельного market/instrument profile.
   > **Open.** В текущем draft-е `Microsandbox` остаётся внешним boundary context из `→ L2/D01 §2`; sector-expansion logic должна синхронизироваться с `→ L2/D09 §5`. [C9][C11]

4. **[Medium] OI-052: Corporate Logos в Citizens' Chamber.** Economic stake корпоративных участников очевиден, но это не означает автоматического права на участие в public-goods chamber.
   > **Open.** D02 фиксирует только instrument-level tension между capital-bearing actors и attestation-based legitimacy; полная constitutional answer belongs to `→ L2/D09 §1`, `→ L2/D09 §6`. [C11][C12]

5. **[Medium] G11: пороги скорости оборота AGC и velocity controls.** Архитектурный паттерн payment channels / off-chain accumulation / periodic settlement уже ясен, но конкретные thresholds для rate limiting, sinks и routing policy не закреплены.
   > **Open.** Нужна отдельная design-модель для safe throughput, reserve stress и допустимого объёма off-chain accumulation перед settlement. [C8]
