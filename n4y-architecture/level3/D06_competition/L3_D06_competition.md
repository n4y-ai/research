# УРОВЕНЬ 3 / ИЗМЕРЕНИЕ 06: КОНКУРЕНТНО-РЫНОЧНОЕ

> **Уровень:** L3 — Межагентные взаимодействия  
> **Измерение:** D06 — Competition  
> **Версия:** 4.0  
> **Статус:** Финал v4.0 | Дата: 04.04.2026  
> **Зависимости:** L3/D02, L2/D06, L2/D07, L1/D09

---

## Ключевой вопрос

**Как формируются рынки, цены, спрос и предложение в результате конкуренции Логосов за ресурсы и задачи?**

---

## Аннотация

Этот draft отвечает на вопрос измерения через пять связанных слоёв: рыночные арены, аукционные механизмы, anti-collusion architecture, competitive ranking и antitrust governance. Главный тезис таков: в N4Y.AI конкуренция не сводится к одному аукциону или одному рынку. Она возникает в трёх аренах одновременно: `Task Marketplace`, `Resource Marketplace` и `Knowledge Marketplace`, а устойчивые ценовые режимы определяются не только bid-by-bid логикой, но и cross-market spillovers, reputation effects и concentration controls. [C9][C11][C12]

По сравнению с `v3.1` этот draft убирает лишнюю фиксацию недоказанных параметров и переводит чужие owned concepts в корректные проекции. `CNP` и `ADS` остаются owned concepts `→ L2/D06 §3-§4`, семантика `cfp` остаётся в `→ L3/D02 §2-§3`, `AI Watcher` и `ZKP` security stack остаются в `→ L2/D07 §3` и `§8`, а `Reputation Score` и tier logic как свойства Логоса остаются в `→ L1/D09 §1`, `§3`, `§5`. Здесь они описываются только в той мере, в какой формируют конкурентное поведение и market structure. [C5][C8][C10][C11]

---

## 1. Рыночные арены и межрыночные эффекты

### 1.1 Три арены конкуренции

| Арена | Что распределяется | Базовый механизм конкуренции | Ключевой сигнал цены/качества | Роль в D06 |
|-------|--------------------|------------------------------|-------------------------------|------------|
| **Task Marketplace** | Миссии, task slots, bundles of execution | `Reverse Vickrey Auction` в простом режиме, `VCG` в сложном режиме | price + SLA + reputation + latency | Каноническая арена для task competition |
| **Resource Marketplace** | GPU/CPU/LLM capacity и связанные execution resources | reverse-auction pressure со стороны провайдеров | цена при соблюдении SLA | Связывает стоимость исполнения с task pricing |
| **Knowledge Marketplace** | Knowledge Assets и их reusable value | price discovery для intangible goods, где качество не наблюдаемо напрямую | reputation и quality signals сильнее posted price | Формирует premium за проверенное знание |

`Task Marketplace`, `Resource Marketplace` и `Knowledge Marketplace` не являются изолированными. Сильный исполнитель может демпфировать цену в одной арене ради захвата спроса в другой, если ecosystem complementarity делает такой кросс-субсидирующий режим устойчивым. Это теоретическая модель (`arXiv:2601.15303`) при допущении сильной ecosystem complementarity; эмпирическая валидация для agent economies и N4Y.AI пока отсутствует. Поэтому D06 должен описывать не отдельный auction clearing, а конкурентную систему из нескольких взаимосвязанных рынков. [C9][C12]

### 1.2 Task competition как проекция CNP и ADS

`CNP` описывается полностью в `→ L2/D06 §3`, а его семантический lifecycle и performatives описываются в `→ L3/D02 §2-§3`. Здесь `CNP` важен как оболочка, в которой competition переводится в сравнимые proposals: кто отвечает на `cfp`, в каком окне, с каким bid quality, и как из набора предложений возникает цена задачи.

`ADS` описывается полностью в `→ L2/D06 §4`. Здесь `ADS` важен только как competitive ranking interface: он определяет видимость участников, очередность shortlisting и то, как `Reputation Score`, SLA history и latency участвуют в фактическом спросе на исполнителей. D06 не переопределяет ADS как инфраструктуру, но фиксирует его роль в price discovery и visibility competition. [C10][C11]

### 1.3 Cross-market subsidization и tipping risk

Корпус подтверждает, что multi-sided agent marketplaces могут поддерживать below-cost pricing в одной арене как стабильное равновесие, если выигрыш переносится в соседние арены. Практически это означает следующий риск: доминирование в `Task Marketplace` может быть использовано для загрузки собственного `Resource Marketplace` или для наращивания `Knowledge Marketplace` reputation loops, а не только для прямой прибыли в текущей сделке. [C9][C12]

Из этого следует два вывода для D06. Во-первых, price competition нельзя оценивать по одной transaction margin: низкая цена может быть рациональным захватом экосистемной value pool, а не ошибкой. Во-вторых, antitrust monitoring должен отслеживать не только HHI внутри одной арены, но и cross-market dependency patterns, потому что tipping может происходить через coupled marketplaces, а не через один obvious monopoly node. [C9][C12]

---

## 2. Аукционная архитектура и переход Phase 1 -> Phase 2

### 2.1 Phase 1: быстрый `Reverse Vickrey Auction`

Для fast-path auction D06 закрепляет `Reverse Vickrey Auction` как базовый механизм первой фазы [A58]. Он применяется там, где `Call for Proposals` не требует bundle allocation, а winner determination должно оставаться быстрым, предсказуемым и дешёвым в операционном исполнении. В этой фазе приоритетом является low-latency clearing, а не полная welfare-optimal combinatorial allocation. [C1][C2]

На этом уровне важно не переусложнить антиколлюзионный слой: Phase 1 должен оставаться дешёвым и быстрым, поэтому по умолчанию опирается на sealed-bid secrecy, commit-reveal и сходные privacy-preserving patterns. `MACI` не становится универсальной аукционной оболочкой; он остаётся selective wrapper only for higher-collusion-risk cases [A59]. Такой split сохраняет быстрый path без смешения real-time allocation и heavy proof pipeline. [C3][C4]

### 2.2 Phase 2: `VCG` как сложный режим allocation

`VCG` остаётся теоретически сильным решением для сложных задач, но в production D06 больше не притворяется, что exact combinatorial winner determination остаётся дешёвым при любом масштабе. Exact `VCG` используется только для multi-slot allocations до `15` слотов, где winner determination ещё помещается в governance-bounded solver budget и не разрушает runtime envelope второй фазы [A58]. [C1][C2]

Для `>15` слотов production baseline переключается не на "идеальный exact VCG любой ценой", а на `MLHCA`-class (Multi-Level Heuristic Combinatorial Auction) approximate allocation. Это означает четыре явных ограничения:
1. allocation search остаётся **time- и query-bounded heuristic search** — не polynomial-time theorem, а engineered solver с явными wall-clock и iteration budgets;
2. целевой welfare-retention band задаётся как `rho ~= 0.8-0.9` относительно N4Y calibration benchmark (это **design-time target band**, а не externally-proved guarantee; final band requires production telemetry calibration);
3. с момента входа approximation dominant-strategy guarantees больше не заявляются как exact `DSIC`, а трактуются как bounded-IC / `ε-BIC` (epsilon-Bayesian Incentive Compatibility — relaxed truthfulness in expectation) — это design interpretation, требующий явного verification на deployed N4Y mechanism, а не прямое следствие literature results;
4. при выходе за solver budget или деградации query efficiency задача раскладывается на более простой lot structure и, при необходимости, возвращается в fast-path routing первой фазы вместо непрозрачного partial optimum. [C2][C4][C5]

### 2.3 Когда нужен переход между фазами

Переход `Phase 1 -> Phase 2` в D06 теперь формулируется как explicit cutover contract, а не как открытая догадка о будущем тюнинге.

> **Допущение [A89, proposal]:** Переход `Phase 1 -> Phase 2` активируется только после `30-day rolling window`, в котором одновременно наблюдаются sustained efficiency gain `>=15%` у Phase 2 candidate относительно `Reverse Vickrey Auction` baseline, нагрузка не ниже `500 auctions/day`, и доменный рынок входит в concentration posture `HHI > 2000`. Это N4Y-designed cutover rule: public production precedent такого formal rule не даёт, поэтому он фиксируется как продуктовый контракт поверх уже committed mechanism split [A58].

Из этого следуют пять обязательных условий переключения (согласованы с `[A89]`):

1. `slot_count > 15` или task complexity уже включает bundle complementarities, multi-slot coupling либо выраженные multi-attribute trade-offs;
2. `30-day rolling window` подтверждает measurable allocative gain второй фазы не ниже `15%` против первой;
3. домен достиг operational scale не ниже `500 auctions/day`, чтобы Phase 2 не включалась на шуме единичных кейсов;
4. **доменный рынок входит в concentration posture `HHI > 2000`** (условие зрелости конкуренции, при котором Phase 2 обосновано; при HHI≤2000 продолжается Phase 1 даже при выполнении условий 1-3);
5. product/governance review явно подтверждает activation, откладывает её или откатывает назад, если market telemetry противоречит calibration result — fallback path всегда остаётся явным: при ухудшении welfare proxy, превышении solver budget или росте operational risk домен возвращается в `Reverse Vickrey Auction`, а `MACI` при этом остаётся selective wrapper rather than universal plumbing [A59]. [C1][C2][C3]

---

## 3. Коллюзия, манипуляции и anti-collusion architecture

### 3.1 Что показывает evidence о collusion by agents

Корпус подтверждает, что algorithmic agents способны поддерживать supra-competitive outcomes в repeated interaction settings. Для LLM agents особенно важен следующий паттерн: symmetric agent populations могут давать примерно `~22%` markup above competitive benchmark, тогда как heterogeneity по patience или information access снижает uplift примерно до `7-10%`, но не гарантирует исчезновение collusion altogether. Это transfer estimates из LLM-agent auction simulations 2025-2026; условия N4Y.AI могут отличаться из-за mandatory heterogeneity policies. Более того, capability asymmetry может сохранять coordinated behavior через leader-follower dynamic; canonical definition и classification criteria cartel behavior см. `→ L3/D07 §4.1`, `§4.3`. [C3][C4]

Для D06 это означает, что heterogeneity нужно рассматривать как mitigation lever, а не как финальное решение. Diversity policy полезна, но не отменяет необходимость explicit antitrust controls, потому что рынок может снова собраться вокруг asymmetric hubs even after symmetry is broken. [C3][C4]

### 3.2 Где уместен `MACI`

`MACI` остаётся релевантным для D06 именно как batch anti-collusion layer второй фазы, а не как universal wrapper для любого auction. Его ключевые достоинства - encrypted bidding, receipt-freeness и key-change escape hatch - хорошо соответствуют задачам periodic sealed allocation. Но current proof-generation overhead делает его плохим кандидатом для fast Phase 1 path: evidence-pack фиксирует порядок `~2 seconds per signature` и batch latency, плохо совместимую с short auction windows. [C5]

Поэтому архитектурное решение для `v4.0` формулируется так: fast first phase опирается на commit-reveal secrecy; `MACI` включается только там, где есть justification в виде higher stakes, larger bidder set или stronger collusion risk, и где batch processing приемлемо. Криптографическая архитектура `ZKP` и security proofs описывается полностью в `→ L2/D07 §8`; в D06 остаётся только рыночная роль этого слоя. [C5][C6]

### 3.3 Layered defense вместо одной "волшебной" меры

Корпус последовательно поддерживает layered defense. На уровне механизма это anti-extraction structure и bid secrecy; на уровне runtime это behavioral detection и cartel screening; на уровне governance это formal investigation и remedies. Ни одна из этих мер по отдельности не даёт достаточной защиты, но в комбинации они materially повышают manipulation resistance. [C7][C8]

`AI Watcher` описывается полностью в `→ L2/D07 §3`. Здесь он выступает только как Tier-1 monitoring component для suspicious correlation, variance anomalies, HHI drift и bidder behavior patterns. D06 не переопределяет `AI Watcher`, но фиксирует, что antitrust enforcement в agent economy должен быть одновременно mechanism-level и behavior-level. [C7][C8]

---

## 4. Price discovery, scoring и competitive advantage

### 4.1 Конкурентная проекция `ADS`

`ADS` описывается полностью в `→ L2/D06 §4`, а semantic payload `cfp` описывается в `→ L3/D02 §3`. Здесь D06 фиксирует owner-level scoring weights для конкурентного выбора победителя:

| Компонент | Начальный вес | Роль |
|-----------|---------------|------|
| **Price** | **40%** | главный pressure signal для competitive price discovery |
| **Capability / SLA fit** | **30%** | соответствие task requirements и promised execution quality |
| **Reputation Score** | **20%** | historical reliability signal без создания автономного local score |
| **Latency** | **10%** | bounded responsiveness penalty для time-sensitive tasks |

Weights governance-tunable, но не считаются "иллюстративными по умолчанию": это стартовый policy baseline для `cfp Scoring`. Компонент `Reputation Score` использует уже committed publication-layer baseline [A20], а тот же scoring contour служит входом и для fast-path shortlist, и для comparative selection в complex-allocation regime [A58]. Таким образом D06 владеет numeric competition logic, не забирая semantic ownership у `FIPA ACL`. [C10][C11][C12]

### 4.2 Reputation as pricing power, not just badge

`Reputation Score` и `Reputation Tier` описываются полностью в `→ L1/D09 §1`, `§3` и `§5`. Здесь репутация важна как market-facing signal. Она даёт visibility advantage, повышает expected conversion в `ADS`, позволяет удерживать premium relative to otherwise similar competitors и особенно важна в markets, где качество ex ante плохо наблюдаемо.

Это особенно заметно в `Knowledge Marketplace`: для intangible goods posted price сам по себе оказывается слабым сигналом качества, тогда как reputation и validated quality cues работают лучше. Но тот же механизм создаёт risk of preferential attachment: winners accumulate more demand because they were already visible and trusted. Следовательно, D06 должен считать репутацию одновременно источником efficient sorting и источником concentration pressure. [C11]

### 4.3 Knowledge pricing и limits of pure price competition

В `Knowledge Marketplace` price discovery не может опираться только на lowest bid logic. Для credence-like assets higher price не гарантирует higher quality, а низкая цена не обязательно означает better value. Поэтому competition здесь строится вокруг комбинации reputation signals, evidence of past usefulness и platform-specific quality filters, а не вокруг обычного commodity auction. [C11]

Это также объясняет, почему D06 не должен механически переносить rules из `Resource Marketplace` в knowledge domain. Для compute resources competition ближе к reverse-auction logic, а для knowledge assets рынок требует quality-weighted discovery и stronger anti-lock-in controls. [C11]

### 4.4 Как конкуренция формирует спрос, предложение и цену

На микроуровне отдельные Логосы конкурируют через `cfp/CNP`: каждый участник решает, на какие задачи откликаться, какую цену заявлять, сколько capacity резервировать и где допустить cross-market subsidy ради будущего спроса. Эти индивидуальные стратегии агрегируются в `ADS` как наблюдаемый demand signal: чем больше валидных откликов и выше их quality-adjusted score, тем яснее платформа видит текущий спрос на task execution, compute resources и trusted knowledge. [C9][C10][C11][C12]

На мезоуровне price discovery возникает в двух шагах. В `Reverse Vickrey Auction` первой фазы раскрывается willingness-to-pay / willingness-to-accept в быстром competitive screen: рынок видит, какие исполнители готовы взять задачу при данном SLA и latency profile. Во второй фазе `VCG` для сложных allocations переводит этот набор заявок в winner determination и payment rule, так что из индивидуальных bid strategies возникает уже наблюдаемый price signal сделки, а не просто набор несравнимых предложений. [C1][C5][C6][C10]

На макроуровне repeated outcomes начинают формировать предложение. Diamond-tier hubs получают больше `cfp`, потому что их reputation и historical reliability повышают вероятность shortlisting; это тянет supply в сторону узлов с лучшей видимостью и execution history. Одновременно `Level Playing Field` ограничивает чрезмерную концентрацию через set-asides, commission caps и gatekeeper-style controls, поэтому supply concentration не должна автоматически переходить в uncontested monopoly. Cross-market subsidization дополнительно связывает три арены: дешёвое task execution может использоваться для загрузки собственных compute resources или для накопления knowledge reputation, из-за чего спрос и цена становятся coupled across markets, а итоговое рыночное равновесие возникает как цепочка `micro strategy -> meso auction outcome -> macro price/supply pattern`, а не как результат одного isolated auction. [C9][C10][C11][C12]

---

## 5. Level Playing Field и competition authority

### 5.1 `Level Playing Field` как набор ex ante controls

`Level Playing Field` в D06 теперь задаёт измеримые contestability rules, а не только общий принцип. Целью является не отмена reputation advantage, а предотвращение ситуации, в которой `Reputation Score` превращается в необратимый lock-in while still preserving the publication-layer baseline [A20].

Практически это означает следующий пакет мер. Во-первых, когда домен входит в challenger-window posture, каждый auction cycle обязан резервировать shortlist visibility как минимум для двух challengers или для более строгого governance-defined set-aside rule. Во-вторых, repeated winners проходят через `5-10` auction-cycle rotation window: reputation credit сохраняется, но guaranteed first-position exposure временно ослабляется, чтобы рынок увидел альтернативных исполнителей. В-третьих, если hard eligibility filters оставляют меньше двух challengers, marketplace продлевает challenger window вместо автоматического uncontested award. В-четвёртых, tightening controls происходит не ad hoc, а через measured persistence of concentration: commission caps, portability/interoperability duties и set-aside escalation усиливаются только после того, как monitoring logic в `§5.2` удерживает домен в challenger posture в течение review cadence. [C10][C11][C12]

Selective anti-collusion tooling может накладываться на такие окна для high-risk cases, но `MACI` остаётся targeted wrapper only where the risk justifies it [A59]. Это не конфликтует с `Reputation Score`, а превращает reputation-driven sorting в contestable market process, а не в self-sealing moat. [C10][C11]

### 5.2 `HHI` и platform antitrust monitoring

`HHI` является owned concept D06 и остаётся базовым structural indicator concentration, но здесь он получает N4Y-specific posture вместо расплывчатой ссылки на generic merger bands.

> **Допущение [A90, position]:** Для доменных hub-markets N4Y использует трёхступенчатую posture: advisory investigation trigger at `HHI = 1800`, mandatory challenger-window activation at `HHI = 2000`, structural remedies review at `HHI = 2500`. Эта ladder grounded in public EU/US bands, но фиксируется как собственная продуктовая позиция для multi-sided agent economy.

Практическое чтение шкалы следующее: ниже `1800` рынок остаётся в baseline monitoring; диапазон `1800-1999` включает advisory investigation и cross-market dependency review; диапазон `2000-2499` автоматически включает challenger-window posture и weekly concentration review; уровень `>=2500` переводит домен в structural-remedies track с обязательной проверкой set-asides, access duties и commission tightening. Но даже здесь `HHI` остаётся necessary but not sufficient measure: его reading обязано сопоставляться с price parallelism, bid variance, repeated interaction patterns, reputation-driven lock-in premia и cross-market subsidization maps. [C6][C7][C12]

### 5.3 Трёхуровневая competition authority

Корпус поддерживает трёхуровневую enforcement logic, а D06 конкретизирует её под monitoring posture `§5.2`:

| Tier | Функция | Типовые инструменты |
|------|---------|---------------------|
| **Tier 1 — automated screening** | раннее обнаружение concentration drift и contestability erosion | `HHI` bands, challenger-win-rate, price-spread drift, bid-pattern screens, cross-market dependency map |
| **Tier 2 — investigation** | отделение structural lock-in от innocent parallelism | network analysis, counterfactual reruns, portability/interoperability review, formal governance check |
| **Tier 3 — remedies** | восстановление contestability | challenger-window activation, set-aside escalation, commission tightening, temporary access duties, structural review |

Для calibration этой authority D06 использует `ABM` contour по трём аренам: `Task Marketplace`, `Resource Marketplace` и `Knowledge Marketplace`. Population mix в simulation runs задаётся как Pareto-like / power-law distribution между incumbents, challengers, specialists и low-cost entrants; governance-tunable parameters включают switching costs, reputation premium, cross-market subsidy intensity, entry rate и latency sensitivity. Из simulation outputs используются `HHI` trajectory, challenger win share, price dispersion, cross-market dependency score и convergence diagnostics over `20-50` iterations. Эти outputs служат decision support для `HHI` thresholds и early-warning projection в `→ L3_D07 §5`, но не трактуются как production truth without observed telemetry back-check. [C8][C9][C10]

Такой authority не должен ждать textbook cartel proof. В agent economy repeated interaction, shared infrastructure и algorithmic adaptation могут породить stable price premia раньше, чем появится явная "человеческая" коммуникация. Поэтому enforcement здесь по природе более diagnostic and preventive, чем в классическом late-stage antitrust. [C8][C12]

---

## Связь с другими измерениями

| Измерение | Связь |
|-----------|-------|
| **L3/D02 (Семантика)** | `cfp` и FIPA semantics задают смысловую форму competitive negotiation, но не владеют auction design. Для lifecycle и performatives см. `→ L3/D02 §2-§3`. |
| **L2/D06 (Координация)** | `CNP`, `ADS` и execution-side matching описываются там как platform mechanisms. D06 использует их как проекцию рыночной конкуренции: `→ L2/D06 §3-§4`. |
| **L2/D07 (Безопасность)** | `AI Watcher` и `ZKP` infrastructure обеспечивают monitoring и privacy/security controls, но не владеют market logic. Для owner-описания см. `→ L2/D07 §3`, `→ L2/D07 §8`. |
| **L1/D09 (Репутация)** | `Reputation Score` и tier logic являются свойствами Логоса; D06 использует их только как signals of pricing power, visibility and concentration pressure: `→ L1/D09 §1`, `§3`, `§5`. |
| **L3/D07 (Организационно-эмерджентное)** | Эмерджентные картели (`D07 §4.1`) = trigger для competition authority enforcement (`D06 §5`). Flash crash и ценовые каскады `D07` → `D06` anti-manipulation stack. `→ L3/D07 §4.1`, `§5` |
| **L3/D08 (Конфликтно-арбитражное)** | Ценовые манипуляции, bid fraud, wash trading → dispute categories `D08`. Competition authority decisions → potential for `D08` escalation. `→ L3/D08 §1`, `§3` |

---

## Открытые вопросы

### Требуют решения (Critical / High)

1. **[High] OI-019: N4Y Phase 1→2 auction cutover criteria.** Proposed solution зафиксирован в `§2.3` как `[A89]`: 30-day window, ≥15% VCG efficiency gain, 500 auctions/day, HHI>2000. Formal proof of cutover correctness и scale envelope для production — в `§2.2`.
   > **Awaiting-decision.** Cutover criteria и Phase 2 mechanism choice (VCG/<15 slots, MLHCA/>15 slots) задокументированы в `§2.3` и `§2.2`. Финальная closure — после user approval `[A89]` (ESC-030). [C1][C2]

2. **[High] OI-020: N4Y HHI concentration threshold for domain hubs.** Proposed posture зафиксирован в `§5.2` как `[A90]`: advisory trigger HHI=1800, mandatory challenger window HHI=2000, structural remedies review HHI=2500.
   > **Awaiting-decision.** HHI three-step ladder documented in `§5.2`; grounded in EU (1000/2000) and US DoJ 2023 (1800) bands. Финальная closure — после user approval `[A90]` (ESC-031). [C10]

### Требуют исследования (Medium)

4. **[Medium] Reputation lock-in vs contestability.** Reputation improves sorting, but can also reinforce preferential attachment and Diamond-tier dominance.
   > **Open.** Нужны formal rules для decay, percentile ranking, multi-homing incentives и visibility rotation, чтобы premium за качество не превращался в permanent moat. [C11][C12]

5. **[Medium] Emergent price dynamics under heterogeneous agents.** Competition in N4Y likely depends on agent composition, patience asymmetry and cross-market spillovers more than on static textbook equilibrium.
   > **Open.** Желательна ABM / simulation workstream для task-resource-knowledge coupling, чтобы калибровать thresholds на основе synthetic market runs, а не только теории. [C3][C9][C12]

---

## Источники

> Все цитаты `[C1]`–`[C12]` трассируются через [`evidence/evidence-pack.md`](evidence/evidence-pack.md). Числовые параметры (10x/58% MLHCA, ~22% markup, HHI-пороги) — transfer estimates; условия производственного применения N4Y.AI могут отличаться.
