# УРОВЕНЬ 3 / ИЗМЕРЕНИЕ 05: КООПЕРАЦИЯ

> **Уровень:** L3 — Межагентные взаимодействия  
> **Измерение:** D05 — Cooperation  
> **Версия:** 4.0  
> **Статус:** Финал v4.0 | Дата: 04.04.2026  
> **Зависимости:** L3/D01, L3/D02, L3/D03, L1/D07, L2/D09, L3/D04

---

## Ключевой вопрос

**Как Логосы объединяют усилия для решения сложных задач, которые не под силу одному агенту (делегирование, распределение ролей)?**

---

## Аннотация

Этот черновик фиксирует кооперацию как отдельный слой архитектуры N4Y.AI: здесь определяется, когда достаточно простого делегирования, когда нужна временная коалиция, а когда взаимодействие должно перейти в устойчивый alliance-режим. D05 владеет моделями кооперации и `Shapley Value`, но использует проекции для `LOA`, `Delegation VC`, `MissionEscrow`, `CSB`, `Saga Pattern` и governance-механизмов. [C1][C2][C4][C8][C10][C14]

По сравнению с `v3.1` этот rerun переводит документ от illustrative схем к evidence-grounded правилам: у коалиции появляется формальный state machine, у incentive layer - математическая опора через `MAC-SPGG`, у sub-delegation - явная политика глубины, а у cross-sandbox cooperation - архитектурный режим `CSB + dual escrow + rule reconciliation`. При этом два участка остаются consciously incomplete: порог переключения exact/approximate `Shapley` под реальной нагрузкой и корпоративная dual-delegation. [C5][C6][C7][C8][C9][C11][C12][C14]

---

## Содержание

1. [Модели кооперации и условия выбора](#1-модели-кооперации-и-условия-выбора)
2. [Lifecycle коалиции и распределение ролей](#2-lifecycle-коалиции-и-распределение-ролей)
3. [Справедливое распределение и устойчивость коалиции](#3-справедливое-распределение-и-устойчивость-коалиции)
4. [Стимулы и защита от free-riding](#4-стимулы-и-защита-от-free-riding)
5. [Сбои, re-recruitment и sub-delegation](#5-сбои-re-recruitment-и-sub-delegation)
6. [Alliance-режим и корпоративная граница](#6-alliance-режим-и-корпоративная-граница)
7. [Cross-sandbox коалиции и перенос доверия](#7-cross-sandbox-коалиции-и-перенос-доверия)

---

## 1. Модели кооперации и условия выбора

Нормализованный корпус поддерживает трёхчастный ландшафт кооперации, но не даёт готовой "официальной" N4Y-таксономии; поэтому границы ниже являются архитектурным synthesis rule для `v4.0`, а не цитатой из одного источника. D05 владеет именно этой таксономией как answer на вопрос "как Логосы объединяют усилия". [C1]

| Модель | Формальное условие выбора | Тип coordination overhead | Горизонт | Что является owner в D05 |
|--------|----------------------------|---------------------------|----------|--------------------------|
| **Simple Delegation** | `1 cfp`, `1 executor`, задача класса `A/B`, нет существенной межагентной взаимозависимости | Низкий | Один run / одна задача | Да: базовая кооперация без устойчивой командной структуры |
| **Coalition** | `2-7` агентов, задача класса `C/D`, нужны role allocation, shared budget и взаимозависимые результаты | Средний | Одна Миссия или bounded mission window | Да: временная команда со своим lifecycle и settlement |
| **Alliance** | Повторяемость `>=5` миссий между теми же сторонами, есть standing agreement и governance rules | Высокий при учреждении, ниже в операционном цикле | Долгий, многомиссионный | Да: устойчивый режим кооперации поверх повторного trust/history |

`Simple Delegation` не должен раздуваться до коалиции только из-за множества сообщений. Если фактически есть один principal workflow и один исполнитель, а остальное - обычные tool/vendor calls, это остаётся delegation-pattern. Наоборот, если возникает shared outcome, взаимная зависимость слотов и необходимость общего settlement, это уже coalition even when one actor remains coordinator. [C1][C3]

`Alliance` начинается не с "любого повторного контракта", а с момента, когда стороны перестают каждый раз заново проходить полный discovery/negotiation loop и переносят часть trust, routing и governance в standing layer. Предварительно согласованные условия описываются полностью в `→ L2/D06 §4`, а governance DAO patterns - в `→ L2/D09 §1` и `→ L2/D09 §3`. Здесь D05 фиксирует только cooperation meaning такого перехода. [C1][C10]

---

## 2. Lifecycle коалиции и распределение ролей

Коалиционный lifecycle в D05 не дублирует owner-описание `CNP` или `MissionEscrow`; он соединяет эти внешние механизмы в lifecycle временной команды. Перформативы `cfp/propose/accept` operationalized в `→ L3/D02 §2`, а полная экономика `MissionEscrow/SubEscrow` - в `→ L3/D03 §3`. D05 владеет тем, как из них складывается жизненный цикл коалиции как cooperative unit. [C2]

### 2.1 State machine коалиции

| Состояние | Что происходит | Условия перехода |
|-----------|----------------|------------------|
| **cfp** | Координатор публикует `cfp`, определяет mission scope, критерии оценки и требуемые роли | Есть валидный набор откликов либо истёк `cfp_ttl` |
| **Proposal Evaluation** | Кандидаты ранжируются по capability, reliability, price, availability и совместимости ролей; при необходимости идёт итерация переговоров | Выбран согласованный состав и распределены роли |
| **MissionEscrow** | Коалиция фиксируется экономически: создаётся `MissionEscrow`, слоты получают `SubEscrow`, задаются penalty/release conditions | Бюджет зафиксирован, роли приняты, команда активирована |
| **Task Execution** | Идёт исполнение, heartbeat-monitoring, промежуточные checkpoints, reallocation при сбое | Все слоты завершены либо зафиксирован failure requiring settlement/dissolution |
| **Settlement / Dissolution** | Проверка результатов, release/refund/penalty, расчёт кооперативной ценности, архивирование coalition pattern | Миссия закрыта, команда распущена или повышена до alliance-pattern |

Такая схема intentionally уже, чем 8-фазная аналитическая модель из поиска, и именно поэтому подходит для D05: здесь нужны состояния, важные для cooperation governance, а не полный транспортный или платёжный decomposition. При необходимости более детальный message-level lifecycle читается через `→ L3/D02 §2`; платёжный decomposition - через `→ L3/D03 §1`. [C2]

### 2.2 Role allocation

Role allocation и coalition formation являются computationally hard, поэтому архитектура не должна обещать exhaustive optimality для каждой Миссии. Практический baseline для N4Y - task decomposition, bounded search, approximate coalition selection, dynamic reallocation и explicit fallback, а не "идеальный глобальный оптимум". [C3]

Для `v4.0` это означает четыре operational rules:

1. decomposition предшествует selection, иначе коалиция строится по неясным слотам;
2. coordinator выбирает not "лучших вообще", а минимально достаточный and complementary set;
3. любое распределение ролей должно быть checkpointable и reassignable;
4. если подбор команды требует неоправданно дорогого поиска, система должна деградировать к более простому execution plan, а не зависать в meta-optimization. [C3][C12]

---

## 3. Справедливое распределение и устойчивость коалиции

`Shapley Value` - owner-концепт D05, потому что без него вопрос о справедливом кооперативном settlement и устойчивости команды остаётся неотвеченным. В N4Y он нужен не как абстрактная "красивая формула", а как механизм, который связывает contribution, fairness and coalition stability. [C4][C5]

### 3.1 Базовый принцип

`Shapley Value` остаётся предпочтительным fairness baseline, потому что лучше других retained mechanisms обоснован аксиоматически и устойчив к наивным манипуляциям распределением ценности. Для sequential or dynamic settings он совместим с Markov-style credit assignment и с альтернативами вроде difference rewards, но именно `Shapley` даёт strongest canonical fairness anchor для D05. [C4][C13]

Для `v4.0` принимается следующая позиция:

- для малых и средних коалиций N4Y стремится к exact or near-exact `Shapley`, а не к эвристическому "справедливому на глаз";
- для больших `N` approximation допустима как compute-budget decision;
- переход к approximation вызывается ограничениями off-chain compute, а не on-chain verification bottleneck. [C4][C5]

### 3.2 DAO-Agent breakthrough

Ключевое обновление rerun-версии: `DAO-Agent` показывает, что off-chain calculation можно сочетать с on-chain proof verification постоянной стоимости. В evidence-pack это выражено как `O(1)` verification и примерно `99.9%` сокращение gas (`~27k` против `28.6M` gas при `n=10`, источник: DAO-Agent paper ICML analogue), что снимает прежний аргумент "Shapley непрактичен на блокчейне". Для D05 это означает: bottleneck переходит из settlement verification в design policy - когда exact value считать достаточно полезным и когда выгоднее approximate. [C5][E3]

### 3.3 Fairness не равна стабильности

Справедливое распределение не гарантирует автоматически пустоту отклонений, поэтому D05 должен различать:

- **fair allocation**: кто сколько внёс в общий результат;
- **coalition stability**: выгодно ли участникам оставаться внутри коалиции;
- **surplus quantification**: есть ли вообще added value от команды сверх суммы соло-результатов. [C4][C12][C13]

Следовательно, `Shapley Value` в N4Y используется как settlement baseline, а устойчивость большой коалиции усиливается дополнительными transfer/bonding/reputation mechanisms, а не только формулой распределения. Именно этим объясняется, почему scalable coalition computation и stable large-scale cooperation в корпусе не считаются тождественными вещами. [C12]

---

## 4. Стимулы и защита от free-riding

Если `Shapley` отвечает на вопрос "как делить кооперативную ценность", то incentive layer отвечает на вопрос "почему рациональному агенту вообще выгодно вносить реальный вклад, а не паразитировать на команде". В retained evidence strongest mechanism здесь даёт `MAC-SPGG`. [C7]

### 4.1 MAC-SPGG baseline

Для `v4.0` D05 фиксирует incentive skeleton:

`R_i = rho * c_i + gamma * H_i + P * 1[task fails]`

где вклад агента, командный эффект и failure-linked component образуют единую reward/penalty function. У retained source это даёт уникальное subgame-perfect Nash equilibrium для effortful contribution, то есть free-riding перестаёт быть рациональным default strategy при корректной настройке параметров. [C7][E42]

### 4.2 Практическая reward stack для N4Y

| Слой | Назначение | Owner-status в D05 |
|------|------------|--------------------|
| **Base slot payment** | Компенсация за выполнение выделенного слота | Проекция: расчёт и release fully described in `→ L3/D03 §3` |
| **CoordinatorReward** | Вознаграждение за сборку, orchestration и удержание коалиции в рабочем состоянии | Совместное использование D03/D05; в D05 важен кооперативный смысл этой премии |
| **Coalition Synergy Bonus** | Выплата за положительный cooperative surplus сверх суммы раздельных результатов | Да: стимул к командной, а не атомизированной стратегии |
| **Slash / stake component** | Наказание за dropout, strategic withholding или нарушение принятых условий | Проекция в economic/security layer; в D05 фиксируется как обязательный anti-free-rider companion |

Отсюда следует практический design rule: `Synergy Bonus` нельзя выплачивать как flat team bonus "за сам факт команды". Он должен зависеть от наблюдаемого surplus и быть согласован с contribution-sensitive distribution, иначе появляется новый канал для free-riding. Slash-stake logic, reputation-weighted reward reshaping и supporter-transfer mechanisms выступают не заменой, а stabilizer bundle вокруг основного payoff rule. [C7][C13]

---

## 5. Сбои, re-recruitment и sub-delegation

Кооперация в D05 должна быть fault-aware by design: dropout, частичный срыв, ложное завершение и cascading breakdowns относятся не к исключениям, а к нормальным branch-ам coalition lifecycle. Retained evidence поддерживает сочетание monitoring, reassignment, modular coordinator/worker split и delegation-chain-aware penalties. [C6]

### 5.1 Failure handling baseline

| Сценарий | Baseline response в D05 | Ownership note |
|----------|-------------------------|----------------|
| **Участник выпал до завершения** | cfp reissue для освободившегося слота, повторный подбор по affinity/capability, команда не распускается целиком | re-recruitment rule belongs to coalition lifecycle here [C6] |
| **Частичное выполнение** | частичный release допустим только по условиям `SubEscrow`; остальное уходит в refund/penalty path | escrow mechanics fully described in `→ L3/D03 §3` |
| **Ложный success / низкое качество** | verification gate before settlement; penalty goes through `SubEscrow`/dispute path | verification and dispute routing projected to `→ L3/D03 §1`, `→ L3/D03 §6` |
| **Риск каскадного отказа** | coordinator stops new dependent dispatches, freezes unsafe fan-out, resumes only from valid checkpoint | `Saga Pattern` and checkpoint stack fully described in `→ L1/D04 §6`; circuit-breaker stack in `→ L2/D07 §6` and `→ L2/D06 §5` |

Таким образом D05 принимает конкретную triad:

1. **re-recruitment через повторный `cfp`**, а не full restart mission;
2. **compensation через `SubEscrow` penalty/refund**, а не ручной постфактум пересчёт;
3. **cascade prevention через coordinator circuit-breaker**, а не надежду, что downstream branches "сами догорят корректно". [C6]

### 5.2 Recursive sub-delegation

JAAMAS 2026 materially strengthens the case for recursive delegation inside coalitions: поддержаны `AND/OR` decomposition, trust-based partner choice, delegation chains and failure propagation along the chain. D05 therefore признаёт sub-delegation как нормальный cooperative mechanism, а не edge-case. [C14][E30]

Для `v4.0` принимаются четыре правила:

1. sub-delegation допустима только после явного decomposition родительского слота;
2. chain trust должен учитывать не только reputation, но и fit к конкретной подзадаче;
3. каждое звено обязано сужать authority, а не расширять её;
4. ответственность и explainability ухудшаются с каждым hop, поэтому глубина цепи сама является risk variable. [C14][C11]

На этой основе `OI-016` закрывается частично: в `v4.0` вводится hard cap `max_depth = 4`. Это design governance rule, а не эмпирически доказанный универсальный максимум. Иначе говоря, evidence уже поддерживает recursion, но не доказывает, что глубина `5+` безопасна, legible or operationally worthwhile. [C14]

---

## 6. Alliance-режим и корпоративная граница

`Alliance` в D05 - это не просто "долгая коалиция", а смена режима: часть trust, условий и governance переносится из mission-by-mission negotiation в standing layer. Поэтому у alliance должна быть не только история успешных миссий, но и явная процедура membership, amendment и escalation. [C1][C10]

### 6.1 Governance baseline для alliance

Полное DAO/governance owner-описание находится в `→ L2/D09 §1` и `→ L2/D09 §3`. Здесь D05 фиксирует alliance projection:

- вход в alliance требует verified history и standing agreement;
- operational decisions могут приниматься обычным большинством или заранее оговорённым lightweight process;
- constitutional changes, admission/expulsion и смена базовых правил требуют `2/3` supermajority;
- voting weight лучше строить на reputation-weighted or role-weighted logic, а не на чистом token-weighted whale capture. [C10]

Именно поэтому alliance не следует смешивать с обычным marketplace repeat business: без explicit governance rules repeated cooperation остаётся repeated coalition, но ещё не alliance. [C10]

### 6.2 Corporate dual-delegation

Корпоративная кооперация не сводится к "ещё одному заказчику". Retained evidence показывает, что multi-principal setting требует явного переноса authority, responsibility and accountability. `LOA` и `Delegation VC` описываются полностью в `→ L1/D07 §1-§2`, а D05 использует их как boundary conditions cooperative design. [C11]

Для `v4.0` безопасный provisional rule выглядит так:

- personal и corporate authority должны жить в разных credential chains;
- effective autonomy для корпоративной миссии не может превышать более жёсткий из двух ceilings;
- override, veto and escalation обязаны оставлять explicit trace;
- конфликт двух masters нельзя считать закрытым только потому, что агент "умеет согласовывать приоритеты". [C11]

Но `G-08` остаётся открытым: corpus поддерживает framing и constraints, однако production-ready N4Y design для dual-delegation всё ещё требует целевой синтез, а не простое заимствование одной статьи. [C11]

---

## 7. Cross-sandbox коалиции и перенос доверия

Cross-sandbox coalition не сводится к partner discovery. Она требует identity/trust portability, routing through isolation boundary, совместимый escrow path и rule reconciliation между sandbox'ами. Именно поэтому D05 рассматривает такую коалицию как отдельный режим cooperation complexity. [C8][C9]

### 7.1 Baseline architecture

`CSB` как платформа owned outside D05 and fully described in `→ L2/D06 §5`. Здесь D05 фиксирует cooperation-level composition:

1. coordinator подбирает межsandbox состав и проверяет минимальный trust baseline;
2. `CSB` маршрутизирует взаимодействие и применяет наиболее строгие rule constraints;
3. расчёты идут через dual escrow posture: local mission commitments внутри командных слотов + neutral cross-sandbox settlement path;
4. команда исполняется только пока обе стороны продолжают соответствовать policy boundary своего sandbox. [C8]

Такой режим нужен, потому что "одна общая коалиция" в юридически и policy-неоднородной среде без посредника приводит к неразрешимым конфликтам release rules, auditability и dispute jurisdiction. D05 therefore assumes `CSB + dual escrow + rule compliance` as minimum architecture, not optional optimization. [C8]

### 7.2 Reputation portability is necessary but insufficient

Переносимые reputation/trust signals полезны для первоначального матчинга, но не должны интерпретироваться как окончательный verdict о надёжности партнёра. `Reputation` owner-context находится в `→ L1/D09 §6`, а cross-domain trust transfer - в `→ L3/D09 §5`. Здесь D05 использует их только как cooperation input. [C8][C9]

Для `v4.0` принимается сильная caveat: imported ratings across domains могут сохранять лишь около `35%` explanatory power от native context (источник: Cho et al. study), если не дополнены freshness, vouching, domain context и provenance-rich evidence bundle. Поэтому portability является входным сигналом для coalition selection, но не заменой local verification и probation logic. [C9][E46]

---

## Связь с другими измерениями

| Измерение | Связь |
|-----------|-------|
| **L3/D01 (Network Structure)** | Кооперация использует сетевые роли и sandbox-subnetwork boundaries, но не владеет топологией. Структура hub/subnet и bridge-ограничения описаны в `→ L3/D01 §1`, `→ L3/D01 §3`. |
| **L3/D02 (Semantics)** | Message-level `cfp/propose/accept` и trust-establishment flow operationalized в `→ L3/D02 §2`; D05 поверх этого определяет coalition lifecycle как lifecycle команды. |
| **L3/D03 (Value Exchange)** | `MissionEscrow`, `SubEscrow`, release/refund/dispute и settlement fully described in `→ L3/D03 §3`, `→ L3/D03 §6`; D05 использует их как economic substrate cooperation. |
| **L3/D04 (Контрактно-договорное)** | Alliance standing agreement uses Cooperation Agreement (L3/D04 §1 type 2) as contractual basis. Task Contract (L3/D04 §1 type 1) = single coalition slot contract. → `L3/D04 §1`, `L3/D04 §3` |
| **L1/D04 (Functional)** | `Saga Pattern`, checkpoints и recovery stack технически реализуются как execution mechanisms в `→ L1/D04 §6`; D05 использует их как failure-handling projection для коалиций. |
| **L1/D07 (Autonomy)** | `LOA` и `Delegation VC` задают autonomy envelope, ceiling и escalation rules, но не принадлежат D05. Полное owner-описание - `→ L1/D07 §1-§2`, `→ L1/D07 §5`. |
| **L2/D06 (Coordination)** | `ADS`, `CNP` и `CSB` составляют инфраструктурную механику подбора и cross-sandbox routing. D05 использует их как проекции: `→ L2/D06 §4`, `→ L2/D06 §5`. |
| **L2/D07 (Security)** | Circuit-breaker and quarantine logic ограничивают blast radius coalition failures, но security ownership остаётся в `→ L2/D07 §6`. |
| **L2/D09 (Governance)** | Alliance governance использует DAO/AIP patterns и quorum logic из `→ L2/D09 §1`, `→ L2/D09 §3`, но D05 отвечает только за cooperative meaning этих правил. |
| **L1/D09 (Reputation) / L3/D09 (Trust)** | Portable reputation и cross-domain trust transfer используются как input for partner selection, но full owner-context расположен в `→ L1/D09 §6` и `→ L3/D09 §5`. |

---

## Открытые вопросы

### Требуют решения (Critical / High)

1. **[High] OI-016: безопасная глубина sub-delegation внутри коалиции.** Корпус уже поддерживает recursive delegation, но не закрывает empirically вопрос о глубине без потери legibility и controllability.  
> **Resolved (Phase 8):** Для `v4.0` принимается `max_depth = 4` как governance cap. Это policy choice для N4Y, а не универсальный научный максимум. [C14]

2. **[High] G-08: корпоративная dual-delegation.** Как согласовывать personal и corporate authority, не разрушая accountability chain и user primacy, всё ещё не закрыто готовым корпусом.  
> **Open.** Нужен отдельный N4Y design synthesis по multi-master conflict resolution, override logging, dual-credential precedence и domain-scoped veto rules. [C11]

### Требуют исследования (Medium)

3. **[Medium] Порог exact -> approximate `Shapley` при реальной production-нагрузке.** Verification bottleneck снят, но compute-budget threshold для off-chain calculation остаётся policy and benchmarking question.  
> **Open.** Нужны benchmarks на типичных coalition sizes и cost envelope, чтобы формально зафиксировать default switch rule. [C5]

4. **[Medium] Калибровка portability/trust transfer across domains.** Evidence already supports the caveat, что imported ratings недостаточны без context, но точные коэффициенты переноса остаются эмпирически неустойчивыми.  
> **Open.** После накопления production data нужна синхронизация с `→ L1/D09 §6` и `→ L3/D09 §5` по domain-specific transfer coefficients и probation logic. [C9]
