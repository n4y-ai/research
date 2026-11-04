# УРОВЕНЬ 3 / ИЗМЕРЕНИЕ 04: КОНТРАКТНО-ДОГОВОРНОЕ

> **Уровень:** L3 — Межагентные взаимодействия  
> **Измерение:** D04 — Contractual Relations  
> **Версия:** 4.0  
> **Статус:** Финал v4.0 | Дата: 04.04.2026  
> **Зависимости:** L3/D02, L3/D03, L2/D01, L2/D03, L2/D08, L1/D07, L3/D05

---

## Ключевой вопрос

**Как формализуются договорённости между агентами (смарт-контракты, устные сделки, SLA) и как контролируется их исполнение сторонами?**

---

## Аннотация

Контрактно-договорное измерение отвечает не на вопрос о transport-слое, не на вопрос о платёжной инфраструктуре и не на вопрос о правовом статусе среды как таковой, а на вопрос о том, как из межагентного согласования возникает связанный набор обязательств, прав, ограничений и механизмов исполнения. По состоянию на 04.04.2026 retained corpus поддерживает три главных вывода: во-первых, AI-mediated contracting может работать без отдельной правосубъектности AI, если у договора есть атрибутируемые стороны, authority chain и доказуемое assent; во-вторых, контракт должен моделироваться как явная finite-state machine с проверяемыми guards; в-третьих, для N4Y.AI договор всегда остаётся гибридом `off-chain legal text + on-chain enforcement hooks`, а не "самодостаточным байткодом". [C1][C2][C4][E1][E2][E3][E4]

Нормативная позиция этого черновика такова: `D04` fully owns `SLA`, `Knowledge Transfer Package` и `Knowledge License (KL)` как типы межагентных договорённостей, но не забирает ownership у `L2/D03` для smart-contract/oracle stack, у `L3/D03` для escrow patterns, у `L2/D08` для GDPR/eIDAS/Data Act legal frame и у `L1/D07` для `Delegation VC`. Поэтому в `v4.0` прежние quasi-FULL описания чужих слоёв заменены на verified projections с реальными ссылками на владельцев, а собственный ответ D04 собран вокруг таксономии контрактов, contract lifecycle, SLA enforcement, knowledge licensing, precedence rules и dispute routing. [C3][C5][C6][C8][C9][C10]

---

## Содержание

1. [Таксономия и правовая квалификация контрактов](#1-таксономия-и-правовая-квалификация-контрактов)
2. [Lifecycle контракта и формальная модель](#2-lifecycle-контракта-и-формальная-модель)
3. [On-chain enforcement и dual-scenario kill switch](#3-on-chain-enforcement-и-dual-scenario-kill-switch)
4. [SLA tiers, penalties и OracleEscrow](#4-sla-tiers-penalties-и-oracleescrow)
5. [Knowledge Transfer Package и ограничения права](#5-knowledge-transfer-package-и-ограничения-права)
6. [Иерархия, disputes и кросс-юрисдикционное исполнение](#6-иерархия-disputes-и-кросс-юрисдикционное-исполнение)

---

## 1. Таксономия и правовая квалификация контрактов

**[C1][C2]** Самый сильный retained legal reading состоит не в том, что AI-агентам нужна отдельная правосубъектность, а в том, что достаточно доказуемой цепочки authority, идентифицируемых сторон и фиксируемого assent. Для N4Y.AI это означает, что канонические contract families должны специфицироваться не как "контракты между AI как самостоятельными юрлицами", а как формализованные отношения между attributable principals и их агентами с on-chain evidence trail. При этом dominant interpretation по-прежнему ближе к модели "smart contract as technical execution of a broader agreement", а не к тезису, что байткод сам по себе исчерпывает правовую форму. [E1][E2][E3][E4][E21]

| Тип контракта | Стороны | Предмет | Форма в N4Y.AI | Что означает юридически |
|---------------|---------|---------|----------------|--------------------------|
| **Task Contract** | Coordinator `<->` Provider Logos | Выполнение конкретной задачи, deliverable, deadline, budget | machine-readable task terms + acceptance record + escrow binding hooks | Базовый договор оказания услуги/выполнения работ; binding сила зависит от authority chain и acceptance evidence |
| **Cooperation Agreement** | Coordinator `<->` coalition of Logos | Общая миссия, распределение ролей, contribution rules, shared constraints | рамочный off-chain agreement + on-chain mission anchors + sub-contract references | Коалиционный договор, поверх которого создаются task-level obligations |
| **SLA** | Service Provider `<->` Service Consumer | Метрики качества, SLO, penalties, remediation windows | structured SLA schedule + telemetry/oracle feeds + `OracleEscrow` enforcement | Полностью owned в D04 форма сервисного обязательства |
| **Knowledge License (KL)** | Knowledge Seller/Author `<->` Buyer/Licensee | Право использовать `Knowledge Transfer Package`, производные работы, attribution, royalty logic | `KTP` metadata + license text + `Knowledge License VC` + on-chain provenance anchor | Лицензионное отношение, а не перенос "естественной собственности" AI между агентами |
| **Delegation** | Human/organization principal `<->` Logos | Scope делегированных полномочий, autonomy envelope, revocation | `Delegation VC` + revocation/escalation hooks; полная структура `→ L1/D07 §2` | Мандат на действие, ограничивающий всё, что Logos может обещать дальше |

**[C2][C10]** Эта таблица является именно N4Y-synthesis: corpus достаточен, чтобы оценить критерии enforceability по каждой family, но не даёт готовой end-to-end классификации ровно под пятичленную N4Y taxonomy. Поэтому distinction между типами задаётся не "чистой цитатой" из одного источника, а комбинацией legal criteria, architecture constraints и ownership rules. [E1][E2][E3][E4][E19][E21]

**[C1][C10]** Важная граница ownership: smart-contract stack, oracle stack и identity binding не принадлежат D04. `D04` отвечает за договорную форму и precedence of obligations; технический стек контрактов описывается полностью в `→ L2/D03 §5`, а `Delegation VC` как credential-мандат - в `→ L1/D07 §2`. Здесь они участвуют только как проекции, потому что без них нельзя ответить на вопрос D04, но они не являются owner-concepts данного измерения. [E21]

---

## 2. Lifecycle контракта и формальная модель

**[C4][C8]** Контракт в N4Y.AI должен моделироваться как explicit finite-state machine, а не как непрозрачный кусок исполняемого кода. Для contract-level abstraction пригодна формальная рамка `Agent Contract`:

```text
C = (I, O, S, R, T, α, β)
```

где `I` = допустимые inputs, `O` = ожидаемые outputs, `S` = допустимые состояния, `R` = ресурсные ограничения, `T` = временные bounds, а `α` и `β` фиксируют pre/post-behavioral constraints. В самой литературе встречается и альтернативная символика `Φ/Ψ`; содержательно речь идёт об одном и том же formal skeleton для bounded coordination. [C8][E28]

**[C4]** Для `D04` канонический lifecycle контракта задаётся как:

```text
Draft -> Active -> Completed
      \-> Expired
Active -> Disputed
Disputed -> Completed | Expired
```

| Состояние | Что означает | Guard входа | Enforcement при входе/выходе |
|-----------|--------------|-------------|-------------------------------|
| **Draft** | Условия сформулированы, но ещё не стали исполнимым обязательством | определены parties, subject, consideration/budget, authority proof, acceptance window | фиксируются digest условий, governing-law/arbitration fields и deadline на подписание |
| **Active** | Контракт вступил в силу и допускает runtime obligations | есть assent/activation event, budget lock или иной commitment anchor | включаются state guards, telemetry, oracle hooks, deadline tracking, pause rights |
| **Completed** | Обязательство исполнено и outcome подтверждён | deliverable/success criteria подтверждены автоматически или сторонами | release/settlement, issuance of receipts, closure of revocation window |
| **Disputed** | Есть contested performance, quality, scope or authority issue | challenge filed within dispute window | pause contested leg, freeze disputed release, open escalation path |
| **Expired** | Контракт прекратился по сроку, revocation или unresolved termination | deadline passed, revocation valid, or termination upheld | unspent funds/rights return per precedence rules, future execution blocked |

**[C4][C9]** На каждом переходе требуется не только смена статуса, но и конкретный enforcement event. `Draft -> Active` требует assent evidence и budget/commitment lock; `Active -> Completed` требует подтверждения deliverable или oracle-verified metrics; `Active -> Disputed` требует pause/freeze contested branch; `Draft -> Expired` и `Disputed -> Expired` требуют termination record и archival trail. Именно эта transition discipline отличает договорную FSM от "жизненного цикла тикета". [E17][E18][E24]

**[C4]** Неформальные и «устные» договорённости (oral/informal agreements) фиксируются в N4Y.AI через тот же механизм: assent event при переходе `Draft -> Active` выступает машиночитаемым эквивалентом согласия — независимо от того, предшествовал ли ему письменный текст или только performative-обмен (`cfp/propose/accept-proposal`). Off-chain evidence bundle (hash текста, цифровая подпись, timestamp) делает такое assent доказуемым без требования on-chain full-text. Таким образом, форма соглашения (устная, письменная или смарт-контрактная) не меняет FSM-модель контракта, а только влияет на полноту evidence trail. → `L3/D02 §3` для семантики performative-актов. [C4][C10]

**[C4]** Retained formal-verification evidence поддерживает набор минимальных свойств: reachability terminal states, safety (`Completed` и `Disputed` не могут быть истинны одновременно), liveness (любой dispute eventually reaches a remedy path), отсутствие deadlocks и запрет неавторизованных переходов. Корпус strongest on `FSM -> CTL -> nuXmv` pipeline; для N4Y дополнительно разумно держать TLA+-зеркало этой state machine как design-time specification, но это уже рекомендация по инженерной реализации, а не отдельное evidence-backed требование. [E17][E18][E32]

---

## 3. On-chain enforcement и dual-scenario kill switch

**[C3]** `D04` не владеет smart contracts как инфраструктурой: полный стек `EscrowFactory.sol`, `CredentialRegistry.sol`, `StateChannelManager.sol` и oracle composition описан в `→ L2/D03 §5`, а Base L2 как текущая boundary-среда - в `→ L2/D01 §4.1-§4.2`. Здесь важен другой вопрос: как этот стек используется для contractual enforcement, не подменяя собой правовую квалификацию договора. Нормативный ответ `v4.0`: on-chain logic в N4Y.AI исполняет guardrails, escrow conditions, receipts и revocation hooks, но юридически сильный договор остаётся гибридом `readable terms + attributable consent + code enforcement`. [C1][C3][E3][E4]

**[C3]** Самая критическая regulatory tension связана с EU Data Act Art.36: статья всё ещё действует для covered data-sharing arrangements, но policy process предлагает её полное удаление. Поэтому D04 не может проектировать один "вечный" contract pattern; нужен dual-scenario design.

| Сценарий | Что должен предполагать контрактный дизайн | Что это меняет для D04 |
|----------|--------------------------------------------|-------------------------|
| **Art.36 остаётся в силе** | договор обязан поддерживать safe interruption/termination, archival continuity и управляемый future-stop mechanism | нужно явно включать termination clause, archival path и kill-switch-compatible execution hooks |
| **Art.36 удаляется** | строгая kill-switch duty может исчезнуть, но contractual pause/termination и evidence preservation всё равно остаются good practice | можно ослабить mandatory stop-layer, но нельзя убирать dispute/termination governance полностью |

**[C3]** Пока эта неопределённость не снята, retained evidence поддерживает пять допустимых implementation approaches для covered contracts:
1. `Proxy Pattern` - логика заменяема, state continuity сохраняется.
2. `UUPS` - upgrade path при том же адресе и юридически непрерывном anchor.  
3. `Pausable / Circuit Breaker` - остановка future execution без стирания history.
4. `Time-Locked Termination` - прекращение после notice period.
5. `Oracle Consensus Termination` - multi-oracle vote on stop condition. [E6][E7][E8]

**[C3][C10]** Для N4Y.AI нормативно предпочтительна комбинация `UUPS + Pausable + Time-Locked Termination`: она лучше всего согласует blockchain finality, доказуемость изменений и идею, что termination касается future execution, а не переписывания прошлого. При этом сам legal frame kill switch, eIDAS-signature equivalence и GDPR-alignment принадлежат `→ L2/D08 §3-§4`; D04 владеет только тем, как эти ограничения меняют contract clauses и lifecycle transitions. [E6][E7][E8]

**[C10]** Отдельно остаётся open проблема off-chain agreement evidencing: corpus достаточно силён, чтобы требовать readable off-chain text, jurisdiction clause и evidence mapping, но ещё не закрывает N4Y-specific canonical annex format для того, как hash terms, human approvals и arbitration clauses должны связываться с on-chain state. Поэтому D04 фиксирует обязанность такого evidence bundle, но не притворяется, что его canonical serialization уже окончательно определена. [E3][E4][E21]

---

## 4. SLA tiers, penalties и OracleEscrow

**[C5]** `SLA` является owner-concept этого измерения, поэтому именно здесь задаётся full contractual structure service obligations. Retained evidence подтверждает, что SLA automation возможна через telemetry mapping, clause extraction, oracle security, remediation workflows и dynamic slashing, но точные thresholds и coefficients остаются local design choices N4Y.AI. (Источник метрик SLA-тиров: Verdikta whitepaper 2025) Ниже фиксируется рабочий baseline `v4.0`, а не навсегда замороженная конституция сервиса. [E9][E11]

| Tier | Availability target | Response time | Accuracy target | Throughput baseline | Remedy baseline |
|------|----------------------|---------------|-----------------|--------------------|-----------------|
| **Basic** | `95%` | `5-10s` | `>80%` | `10 req/min` | `5-10%` service credit |
| **Standard** | `98%` | `2-3s` | `>90%` | `100 req/min` | `10-20%` penalty/credit |
| **Premium** | `99%` | `<1s` | `>95%` | `1000 req/min` | `20-30%` penalty + priority remediation |
| **Enterprise** | negotiated annex | negotiated annex | `>98%` or custom | custom | `30-50%` tiered remedy + escalation rights |

**[C5]** Penalty logic в D04 должна поддерживать минимум три формулы, потому что разные service patterns не сводятся к одному KPI:

```text
Linear:    Penalty = Base_Fee x (1 - Actual_SLO / Target_SLO)
Tiered:    Penalty = Base_Fee x TierMultiplier(breach_band)
Composite: Penalty = Base_Fee x [w1(availability_gap) + w2(latency_gap) + w3(accuracy_gap)]
```

Такой набор согласуется с corpus: penalties могут быть credits, withheld release, slashing or combined remedy, но не должны превращаться в "абстрактный reputation hit без contract consequences". [E9][E11]

**[C5][C12]** `OracleEscrow` описывается полностью как escrow subtype в `→ L3/D03 §3`, а oracle stack - в `→ L2/D03 §5`; здесь D04 задаёт их договорную композицию:
1. SLA schedule фиксирует measurable metrics, source of telemetry и challenge window.
2. `OracleEscrow` удерживает расчёт до oracle attestation.
3. При compliance release идёт автоматически; при breach применяется credit/slash formula.
4. При contested metrics контракт переходит в `Disputed` и использует escalation ladder из §6. [E9][E11][E24]

**[C12][C13]** Наиболее сильная часть evidence закрывает objective metrics и manipulation resistance для deterministic or semi-deterministic feeds. Для non-deterministic AI quality вопрос остаётся только partially solved: dynamic slashing, truth-discovery и multi-oracle consensus помогают, но subjective quality review всё ещё требует escalation beyond pure oracle logic. Поэтому для creative/analytical outputs SLA должен различать `objective metrics lane` и `subjective quality lane`, а не обещать полную автоматизацию там, где corpus её не подтверждает. [E9][E22][E23][E31]

---

## 5. Knowledge Transfer Package и ограничения права

**[C6]** `Knowledge Transfer Package` и `Knowledge License (KL)` принадлежат `D04` fully: это не просто "marketplace asset", а договорная форма передачи knowledge-based value между агентами. Технические provenance and royalty tools улучшают attribution, tracking и routing of payments, но не устраняют основной legal факт: purely AI-generated works во многих юрисдикциях остаются некопирайтабельными, а AI-to-AI ownership не имеет устойчиво закрытого режима. [E12][E13][E14][E27]

**[C6]** Для `v4.0` минимальный KTP/Knowledge License bundle должен включать:
1. provenance block: author/principal reference, source lineage, content hash;
2. rights block: scope of use, derivative rights, redistribution status, exclusivity or non-exclusivity;
3. warranty block: training/provenance representations, known limitations, revocation/deprecation trigger;
4. compensation block: price, royalty routing, refund/dispute conditions;
5. evidence block: `Knowledge License VC`, timestamped acceptance, on-chain anchor.  
Это уже собственный ответ D04: corpus поддерживает такую contractual decomposition лучше, чем legacy перечисления "типов лицензий" без связи с enforceability. [E12][E13][E14]

**[C6]** Самый неприятный unresolved point не технический, а правовой. US Copyright Office жёстко удерживает human-authorship baseline; retained pack также фиксирует "ownership vacuum" для EU-style regimes и прямо отмечает, что UK `CDPA §9(3)` выглядит наиболее благоприятным аналогом для computer-generated works, но остаётся неиспытанным и не закрывает кейс "AI agent A licenses output to AI agent B" сам по себе. Поэтому `Knowledge License` в N4Y должен исходить не из фикции "AI already owns IP", а из модели `contractual allocation of use-rights under uncertainty`. [C6][E12][E13][E14][E27]

**[C7]** GDPR Art.17 добавляет второе ограничение: даже идеально оформленный `Knowledge License` нельзя строить так, будто blockchain immutability автоматически совместима с erasure duties. Полное regulatory раскрытие принадлежит `→ L2/D08 §4`; здесь contractual implication такова: personal data и reversible payload должны жить off-chain, on-chain должен оставаться только hash/pointer layer без избыточных PII, а revocation/deprecation clauses должны предусматривать crypto-shredding or key destruction как один из remediation paths. Это не гарантирует "чистую" правовую победу, но снижает exposure и согласуется с retained evidence лучше, чем хранение контента целиком on-chain. [E15][E16]

**[C11]** Anti-gaming для knowledge marketplace закрыт только частично. Репутационный stake, truth-discovery и market-based challenge mechanisms дают полезные building blocks, но корпус не закрывает точный N4Y design для stake sizing, Sybil resistance и periodic revalidation knowledge claims. Поэтому в `v4.0` knowledge contracts должны предусматривать challengeable warranties и evidence-backed dispute path, но calibration этих механизмов остаётся открытой задачей. [E29][E31]

---

## 6. Иерархия, disputes и кросс-юрисдикционное исполнение

**[C8][C9][C10]** Contract hierarchy в N4Y.AI читается сверху вниз:

```text
Delegation VC > Cooperation Agreement > Task Contract
```

`Delegation VC` полностью описывается в `→ L1/D07 §2`; здесь важно только правило precedence: ни `Cooperation Agreement`, ни `Task Contract` не могут расширить authority beyond delegated scope. `SLA` и `Knowledge License` в этой логике являются contract-specific annexes: они конкретизируют service or rights obligations, но не могут переопределить верхнюю authority envelope. [E28]

**[C8][C13]** Из `Agent Contract` formalism следует ещё одно правило: conservation of delegated resources. Если верхний контракт задаёт пределы `R`, нижестоящий контракт не может "создать" новый бюджет, новый authority scope или новый time horizon поверх него. Для D04 это превращается в простой precedence algorithm:
1. проверить authority and budget ceiling из `Delegation VC`;
2. применить coalition-level constraints из `Cooperation Agreement`;
3. затем активировать task-level terms;
4. при конфликте lower contract с higher envelope — lower clause void or reduced to compliant subset. [E28]

**[C9]** Dispute routing strongest in current pack around a five-tier escalation ladder:
1. auto-execution without dispute;
2. oracle-consensus review;
3. on-chain arbitration (`Kleros V2`);
4. off-chain arbitration under selected institutional rules;
5. national courts as last resort.  
`Kleros` здесь важен не как "магический глобальный суд", а как strongest included bridge between on-chain and off-chain adjudication: pack содержит и integration guidance, и 2025 court-recognition signal from Mexican practice (Kleros 2026 Update). Но integrator всё равно остаётся ответственным за enforcement of ruling on the contractual side. [C9][E24][E25][E26]

**[C10]** Кросс-юрисдикционная enforceability strongest там, где одновременно сходятся три вещи: e-signature/digital identity recognition, recognition of electronic records/assets and a viable arbitration pathway. Для N4Y.AI practically strongest corridor сегодня выглядит так:
- EU side: `eIDAS Regulation 910/2014` and its trust-services logic;
- international trade side: `UNCITRAL` DLT guidance and arbitration conventions;
- dispute bridge side: judicial willingness to recognise on-chain evidentiary chains, for which the Mexican `Kleros` signal is currently the best retained example. [E3][E19][E21][E24][E26]

**[C10]** U.S. pathway в retained corpus лучше всего подтверждён через general electronic-contract and UCC-style recognition of controllable electronic records, а не через один полностью harmonised clause for all states. Поэтому `v4.0` intentionally не делает вид, что вопрос New York-specific contract routing уже закрыт: choice-of-law, forum selection и annex-level evidencing должны быть явной частью readable agreement, а не молчаливо "подразумеваться блокчейном". Это и есть та точка, где `G-01` и `G-10` остаются частично открытыми несмотря на сильный общий corridor. [E19][E21]

---

## Связь с другими измерениями

| Измерение | Связь |
|-----------|-------|
| **L3/D02 (Семантико-протокольное)** | `cfp/propose/accept-proposal` и semantic commitments возникают в `→ L3/D02 §2-§3`; D04 отвечает за то, как они превращаются в формальный contract state и enforceable obligations. |
| **L3/D03 (Транзакционно-обменное)** | `OracleEscrow`, escrow patterns и settlement mechanics описываются полностью в `→ L3/D03 §3` и `→ L3/D03 §6`; D04 использует их как enforcement substrate для contractual promises. |
| **L2/D01 (Онтология среды)** | Base L2 и managed boundary среды задают технический и онтологический контекст contract anchoring, но не заменяют contractual logic; см. `→ L2/D01 §4.1-§4.4`. |
| **L2/D03 (Инфраструктура)** | Smart-contract stack, oracle composition и contract deployment layer fully described in `→ L2/D03 §5`; D04 не повторяет инфраструктуру, а специфицирует договорные формы поверх неё. |
| **L2/D08 (Нормативно-правовое)** | `eIDAS`, `GDPR` и Data Act legal frame описываются полностью в `→ L2/D08 §3-§4`; в D04 они действуют как constraints на contract design, termination и evidence mapping. |
| **L1/D07 (Автономность)** | `Delegation VC`, `LOA` и revocation semantics fully described in `→ L1/D07 §2`; D04 использует их как верхний authority envelope, который ограничивает нижестоящие contracts. |
| **L3/D05 (Кооперация)** | Cooperation Agreement (L3/D04 §1 type 2) formalized in D04 defines coalition contract structure; Alliance standing agreements use Cooperation Agreement as template. → `L3/D05 §2`, `L3/D05 §4` |

---

## Открытые вопросы

### Требуют решения (Critical / High)

1. **[High] G-01: окончательная legal qualification по каждому из пяти типов контрактов всё ещё зависит от юрисдикции и формы evidence bundle.**  
> **Open.** Corpus уже позволяет отделить attributable AI-mediated contracting от fiction of AI personhood, но не закрывает единый per-type mapping для всех юрисдикций и всех fact patterns.

2. **[High] G-05: `Knowledge License` остаётся в зоне IP ownership vacuum для AI-to-AI случаев.**  
> **Open.** Blockchain provenance и royalty routing усиливают traceability, но не создают сами по себе устойчивое право собственности на purely AI-generated knowledge assets.

3. **[High] G-10: off-chain agreement evidencing ещё не сведён к одному canonical annex format.**  
> **Open.** Нужен единый N4Y template для hash-linked legal text, authority proof, arbitration clause, governing law и human approvals.

### Требуют исследования (Medium)

4. **[Medium] G-09: governance structure для `Cooperation Agreement` пока задана только на уровне baseline-принципов.**  
> **Open.** Нужны role taxonomy, amendment rules, quorum/escalation logic и alignment с `→ L3/D05`.

5. **[Medium] G-08/G-11: anti-gaming mechanisms для knowledge contracts и dispute-cost model ещё не откалиброваны.**  
> **Open.** Есть staking/truth-discovery building blocks и сильная Kleros ladder, но нет окончательных параметров стоимости challenge, jury funding и reputation/stake coupling.

6. **[Medium] G-12: subjective quality verification для non-deterministic AI outputs остаётся частично решённой.**  
> **Open.** Objective metrics хорошо автоматизируются, но для analytic/creative outputs contract must preserve escalation beyond pure oracle logic.
