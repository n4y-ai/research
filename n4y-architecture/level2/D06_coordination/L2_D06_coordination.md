# УРОВЕНЬ 2 / ИЗМЕРЕНИЕ 06: МЕХАНИЗМЫ КООРДИНАЦИИ И ИСПОЛНЕНИЯ

> **Уровень:** L2 — Среда (инфраструктура)  
> **Измерение:** D04 — Coordination & Execution  
> **Версия:** 4.0  
> **Статус:** Финал v4.0 | Дата: 02.04.2026  
> **Зависимости:** L2/D01, L2/D02, L2/D03, L2/D07, L2/D08, L3/D03, L3/D06, L3/D08

---

## Ключевой вопрос

**Как в среде исполняются транзакции, аукционы, матчинги и другие экономические и протокольные процессы?**

---

## Аннотация

**Scoping note:** Все claim-теги `[C{N}]` в этом документе ссылаются на `evidence/normalized-corpus.md` (C1–C10), который содержит ссылки на `evidence-pack.md` (E1–E33). OI-001 (SC legal qualification) и OI-002 (kill switch) открыты как expert-opinion задачи для production deployment — они не блокируют архитектурную спецификацию D04; coordination-routing ownership принадлежит D04, правовая квалификация → L3/D04. OI-005 (ADS topology) открыт как design task — D04 описывает текущий architectural baseline; финальная topology finalization pending governance (OI-005).

`L2/D06` отвечает на этот вопрос как на вопрос о координационном стеке: как запрос проходит путь от discovery и отбора исполнителя к escrow, sandbox execution, verification, settlement и dispute routing. В `v4.0` сохраняются seven-phase happy path и четыре escrow-паттерна из предыдущей версии, но их роль переопределяется через ownership map: `D04` полно владеет `ADS`, `CNP` и `CSB`, а `AGC`, `Microsandbox`, smart-contract/oracle stack, escrow internals, auction theory и `DisputeModule` используются только как проверенные проекции на execution layer. [C1][C2][C3][C4][C5][C7][C8][C9]

Ключевой тезис черновика такой: N4Y.AI исполняет coordination flows не одним модулем, а связкой из off-chain matching, escrow-anchored commitment, sandbox-aware execution и dispute-aware settlement. Там, где корпус уже даёт сильный baseline, это фиксируется как working rule; там, где остаются unresolved topology или threshold questions, они прямо маркируются как `resolve-in-draft` или `Open`. [C3][C4][C7][C9][C10]

---

## Содержание

1. [AGC транзакционный движок](#1-agc-транзакционный-движок)
2. [EscrowFactory и паттерны эскроу](#2-escrowfactory-и-паттерны-эскроу)
3. [Аукционные механизмы](#3-аукционные-механизмы)
4. [ADS и CNP матчинг](#4-ads-и-cnp-матчинг)
5. [Microsandbox execution и CSB](#5-microsandbox-execution-и-csb)
6. [Oracle trust и DisputeModule](#6-oracle-trust-и-disputemodule)
7. [Интеграция координационного стека](#7-интеграция-координационного-стека)

---

## 1. AGC транзакционный движок

### 1.1 Ownership и граница секции

`AGC` как stable operational instrument принадлежит `→ L2/D02 §4`; в `D04` owned не monetary policy, а transaction orchestration. Здесь корректно описывать seven-phase lifecycle, transition gates, rollback rules и binding interfaces между matching, execution и settlement. Некорректно заново полно раскрывать peg logic, reserve policy и `Revenue Distribution`, потому что это owner-layer `D02`.

### 1.2 Seven-phase happy path

В `v4.0` сохраняется рабочий seven-phase happy path:

1. `Initiation`
2. `Escrow`
3. `Matching`
4. `Execution`
5. `Verification`
6. `Settlement`
7. `Distribution`

Эта цепочка важна не как narrative flow, а как state machine, где ранние стадии допускают speculative work, а поздние должны открываться только после прохождения validation gates. Screened evidence для `G11` усиливает именно эту логику: phase transition должно происходить по frontier-gated rule, а не по implicit "ну вроде готово". [C4]

| Фаза | Основной вход | Разрешённый результат | Что ещё нельзя делать |
|------|---------------|-----------------------|-----------------------|
| `Initiation` | task intent, budget intent, policy context | создать request context | нельзя финализировать payout |
| `Escrow` | price envelope, payer authority, escrow type | materialize commitment surface | нельзя считать winner selected |
| `Matching` | `cfp`, ranking context, candidates | winner binding | нельзя release funds |
| `Execution` | execution ticket, sandbox route | perform work and collect evidence | нельзя считать quality accepted |
| `Verification` | outputs, telemetry, oracle checks | outcome classification | нельзя делать final distribution |
| `Settlement` | verified outcome or dispute route | release / refund / split | нельзя переписывать result без нового state |
| `Distribution` | terminal settlement record | final fan-out | нельзя заново открывать matching |

### 1.3 Trigger gates и rollback

Для `D04` рабочее правило простое: переходы должны быть `stage -> validate -> commit`. Это означает, что `Escrow -> Matching` требует действительного escrow context, `Matching -> Execution` требует winner binding, `Execution -> Verification` требует attested completion marker, а `Verification -> Settlement` требует либо objective pass, либо formal dispute escalation. [C4]

> **Resolve-in-draft (G11):** в черновике принят frontier-gated transition model и idempotent compensation path, но не закрыты exact event names, timeout windows и retry budgets. [C4]

Rollback должен идти не как "стереть запись", а как cascade unwind в обратном порядке зависимости. Speculative reservations, queued verification jobs и dependent subtasks должны откатываться автоматически; irreversible effects требуют manual intervention gate. [C4]

| Класс эффекта | Пример | Правило abort path |
|---------------|--------|--------------------|
| reversible reservation | provisional escrow lock, slot reservation | cancel and mark aborted |
| dependent work | queued proofs, child subtasks | leaves-first unwind |
| compensable effect | provisional accrual, temporary hold | compensating transaction |
| irreversible effect | external transfer, human-signed release | stop-the-line and escalate |

Seven-phase engine здесь важен не как визуальный flow, а как coordination kernel: speculative effects допустимы до `Settlement`, `Verification` остаётся последней безопасной точкой abort/refund, а `Distribution` трактуется как post-settlement fan-out, а не как часть winner selection. [C4]

---

## 2. EscrowFactory и паттерны эскроу

`EscrowFactory.sol` как deployment surface опирается на `→ L2/D03 §5.2`, а полные escrow semantics принадлежат `→ L3/D03 §3-§6`. В `D04` owned более узкий вопрос: как coordination layer маршрутизирует задачи к нужному escrow path и как этот выбор влияет на verification и settlement.

### 2.2 Четыре канонических паттерна (проекция)

Coordination layer маршрутизирует задачи к четырём escrow path: TimelockEscrow (deadline-bound), MilestoneEscrow (staged), MultiSigEscrow (review), OracleEscrow (measurable). Выбор по task profile — описан в §2.3. Детальные паттерны, gas economics, settlement ladder → `L3/D03 §3`.

### 2.3 Routing by task profile

| Профиль задачи | Предпочтительный escrow | Coordination reason |
|----------------|-------------------------|---------------------|
| one-shot, objective, short TTL | `TimelockEscrow` | cheap bounded path |
| staged deliverables | `MilestoneEscrow` | localize failure per milestone |
| committee sign-off | `MultiSigEscrow` | explicit human review |
| benchmarkable output | `OracleEscrow` | link verification to payout |

Выбор escrow делается во второй фазе lifecycle, но его последствия распространяются дальше:

- в `Matching` он ограничивает тип допустимого winner binding;
- в `Verification` он задаёт admissible evidence format;
- в `Settlement` он определяет, доступен ли auto-release, partial split или dispute path.

Именно поэтому `D04` не дублирует exchange-layer theory, а удерживает coordination-layer routing. `Эскроу` описывается полностью в → `L3/D03` (Транзакционно-обменное). Здесь: escrow используется как execution constraint и settlement surface.

---

## 3. Аукционные механизмы

Полная auction grammar, market structure и price discovery принадлежат `→ L3/D06 §2-§4`. В `D04` owned не "какой аукцион лучший вообще", а **какой механизм должен быть назначен данному task profile внутри coordination flow**. Это различие важно: `D04` не заменяет competition layer, а привязывает mechanism selection к execution consequences.

### 3.2 Рабочий набор механизмов (проекция)

Coordination layer выбирает аукционный механизм по task profile: Reverse Vickrey (fast path), VCG (multi-slot, tractable), approximation/dynamic VCG (complex), MACI wrapper (high collusion risk). Детальная аукционная архитектура, market structure, price discovery → `L3/D06 §2`.

### 3.3 MACI как selective anti-collusion layer

Корпус поддерживает `MACI v2` как production-ready anti-collusion option, но не как universal default. Его разумная роль в `D04` - selective wrapper там, где encrypted bidding materially снижает bribery/collusion risk и где operational overhead приемлем. [C1]

Для coordination layer это означает три вещи: encrypted bids suppress explicit collusive signaling, batch-oriented flow совместим с periodic clearing, а current production readiness делает `MACI` usable now, not merely aspirational. Одновременно marketplace valuations остаются heterogeneous, bundle bids и dynamic bid updates требуют adaptation, а price discovery становится менее прозрачным. [C1]

> **Resolve-in-draft (G8):** `MACI` принимается как second-layer mechanism для high-collusion-risk branches, а не как wrapper for every `cfp` auction. [C1]

### 3.4 VCG и scaling envelope (проекция)

VCG применяется для truthful multi-slot allocation в tractable envelope (до ~15 slots). При росте complexity — explicit cutover to approximation. Coordination layer фиксирует это как routing rule; детали VCG-механизма → `L3/D06 §2.2`.

### 3.5 Routing by task features

Сильнейший synthesis для `G15` говорит, что выбор механизма должен идти от четырёх признаков:

- combinatorial complexity;
- urgency / latency budget;
- collusion risk;
- valuation heterogeneity. [C8]

| Условие | Предпочтительный механизм | Почему |
|---------|---------------------------|--------|
| low complexity, single winner, low collusion risk | `Reverse Vickrey` | fastest clearing path |
| bounded multi-slot, need truthful ranking | `VCG` | strong allocation logic in tractable cases |
| high complexity or many slots | approximation / dynamic `VCG` family | preserve feasibility |
| elevated collusion risk | `MACI` over chosen base mechanism | add privacy/anti-collusion layer |

> **Resolve-in-draft (G15):** `v4.0` принимает routing-by-features, но не закрывает N4Y-specific thresholds и fallback table. [C8]

`Аукцион` описывается полностью в → `L3/D06` (Конкурентно-рыночное). Здесь: coordination layer использует auction mechanism как winner-selection branch и связывает его с escrow/settlement path. `Price discovery` остаётся owner-topic competition layer; `winner binding -> execution` остаётся owner-topic `D04`.

---

## 4. ADS и CNP матчинг

> **ADS ownership split (OI-005, OI-169):** `L2/D06` владеет semantic layer `ADS`: intake signed `Agent Cards`, normalization and deduplication, eligibility predicates, privacy-aware field release, candidate ranking и handoff в `cfp`. Физическая discovery-сеть `Edge Cache → Regional DHT → Almanac`, sync cadence, failover и partition posture описываются полностью в `→ L2/D04 §4`. [A8][A52]

`ADS` в смысле `L2/D06` — coordination-side meta-registry over signed `Agent Cards`, curated/public registries и `Almanac`-derived presence records. Эта секция отвечает на вопрос "когда запись считается пригодной для координации и как она превращается в admissible candidate set", не переописывая инфраструктурную репликацию и failover. [A8][A52]

Здесь важно удержать ownership boundary: `A2A` как коммуникационный протокол принадлежит `→ L2/D04`, `Performative` semantics принадлежит `→ L3/D02 §3`, а aggregation, ranking, candidate normalization, privacy-aware filtering и discovery-to-matching path принадлежат `L2/D06`. [A8][A52]

### 4.1 Семантический контур ADS

`ADS` принимает multi-source discovery inputs, нормализует их к единому candidate profile и только после этого передаёт результат в `CNP`. Смысловой owner здесь отвечает не на вопрос "где физически лежит запись", а на вопрос "можно ли её использовать для match-making и какие поля допустимо показать на этой стадии". [A8][A52]

> **Допущение [A73, proposal]:** При расхождении между direct signed `Agent Card`, curated-registry overlay и `Almanac`-derived presence `ADS` применяет deterministic source precedence: self-published signed `Agent Card` canonically определяет endpoint и capability claims; curated registry добавляет trust/policy annotations и может сужать visibility через policy tags; `Almanac` остаётся authoritative/fallback для liveness и reachability, когда richer metadata отсутствует или устарела.

В semantic pipeline `ADS` выполняет пять последовательных шагов: intake raw records, normalization to one candidate profile, deduplication by stable identity/service surface, eligibility filtering и handoff в ranking stage. Уже на этом уровне вводится split-marker: topology, replication windows и cache invalidation принадлежат `→ L2/D04 §4`, а здесь остаётся только смысл того, какая запись считается admissible для discovery. [A8][A52]

| Шаг | Что делает semantic layer | Что остаётся вне owner-zone |
|-----|---------------------------|-----------------------------|
| intake | принимает signed `Agent Card`, curated overlays и `Almanac` presence | network replication и cache refresh |
| normalization | собирает единый candidate profile с policy tags и capability surface | multi-region storage placement |
| deduplication | устраняет дубликаты по identity/endpoint surface | DHT merge transport |
| eligibility | проверяет capability fit, visibility policy и минимальные trust predicates | failover / partition routing |
| handoff | публикует admissible candidate set для `cfp` и ranking | delivery/distribution path |

### 4.2 Discovery-to-`cfp` handoff

`ADS` завершает свою semantic работу не выбором победителя, а выпуском bounded candidate set для `cfp`. `CNP` получает только тех кандидатов, которые уже прошли capability fit, visibility controls и policy-tag filtering; тем самым discovery не подменяет negotiation, а делает её управляемой. [A8][A52]

> **Допущение [A74, position]:** Ranking signals работают только после прохождения eligibility gate: capability fit и policy-tag compliance определяют discoverability, а freshness, availability, latency и reputation only reorder already-eligible candidates и не могут сделать недопустимого кандидата discoverable.

`cfp` handoff therefore строится по принципу `eligible first, rank second, negotiate third`: сначала semantic layer формирует admissible set, затем ranking упорядочивает его, и только после этого `CNP` открывает proposal intake. Это держит границу между discovery semantics и competition lifecycle прозрачной. [A8][A52]

### 4.3 ADS privacy model

Privacy controls принадлежат semantic owner, потому что именно здесь определяется, какие поля раскрываются до начала переговоров и какие профили вообще видимы в discovery surface. Полная правовая doctrine `GDPR`, `DPIA`, controller split и `Art.22` sensitivity описываются полностью в `→ L2/D08 §4.3-§4.4`; здесь фиксируется только ADS-specific control behavior. [A1][A8][A52]

`Corporate Logos` могут быть registry-addressable как сервисные профили, но это не отменяет anti-Sybil root и не раскрывает underlying personal identity: exception `[A1]` относится к KYC/role-bound corporate profile, а не к bypass PoP/root identity constraints для человека-владельца. [A1]

| Поверхность discovery | Corporate Logos | Personal Logos | Правило ADS |
|-----------------------|-----------------|----------------|-------------|
| direct DID lookup | допустим для service profile и публичного endpoint | допустим только при explicit opt-in или при already-established relation | прямой DID lookup не раскрывает sensitive fields по умолчанию [A1][A8][A52] |
| capability-only matching | допустим | допустим и является preferred default | match returns capability envelope before identity-rich profile [A1][A8][A52] |
| public profile fields | service class, endpoint class, public policy tags | минимальный public profile по opt-in | disclosure ограничивается data minimization rule [A1][A8][A52] |
| sensitive profile fields | скрыты до negotiation / authorization gate | скрыты до negotiation / authorization gate | sensitive fields не выдаются на pre-match stage [A1][A8][A52] |
| policy-tag filtering | обязателен | обязателен | tags могут сужать видимость без раскрытия identity details [A1][A8][A52] |

Практический baseline therefore таков: для Personal Logos discovery по умолчанию capability-first и opt-in, для Corporate Logos — service-profile discoverability с policy tags, но без раскрытия underlying human DID и без обхода identity safeguards. Это ADS-specific operationalization data minimization, а не самостоятельная попытка сделать `L2/D06` owner-документом по `GDPR`. [A1][A8][A52]

### 4.4 CNP lifecycle

`CNP` также owned в `L2/D06`. Его coordination-side shell в `v4.0` теперь прямо опирается на `ADS` handoff:

1. `ADS` выпускает admissible candidate set.
2. `cfp` publication адресуется только этому множеству.
3. proposal intake принимает ответы и привязывает их к candidate context.
4. scoring and filtering упорядочивает только eligible proposals.
5. accept / reject формирует winner binding.
6. execution ticket issuance открывает escrow-bound start.
7. inform / verify / settle завершает lifecycle.

Тут важно не смешивать уровни: `cfp` есть performative and competitive request, `CNP` есть whole protocol shell, а meaning of performatives stays in `→ L3/D02 §3`. `L2/D06` владеет связкой `ADS -> cfp -> winner binding`, но не транспортным delivery path этих сообщений. [A8][A52]

### 4.5 Ranking signals и bounded scoring

`cfp` scoring не должен сводиться к цене. Coordination layer учитывает capability fit, availability, price, latency, reputation и accountability posture, но exact weights остаются governance-tunable и не могут нарушать eligibility-first rule. [A8][A52][A74]

| Компонент score | Роль в matching | Ограничение |
|-----------------|-----------------|-------------|
| capability fit | подтверждает, что кандидат релевантен задаче | не заменяется reputation bonus [A8][A52][A74] |
| availability / load | снижает risk of no-show | не делает ineligible candidate discoverable [A8][A52][A74] |
| price | сохраняет market discipline | учитывается только после eligibility gate [A8][A52][A74] |
| latency / urgency fit | учитывает deadline pressure | вторичен к policy/visibility constraints [A8][A52][A74] |
| reputation | переносит historical quality into ordering | не подменяет capability fit [A8][A52][A74] |
| accountability posture | penalizes low-accountability bids | работает как secondary ordering signal [A8][A52][A74] |

Корпус поддерживает accountability surface через direct staking/bonding и insurer-backed guarantees for operational agents, но здесь это остаётся only ranking/selection implication, а не отдельная infrastructure topology. [A8][A52]

### 4.6 Onboarding/offboarding lifecycle FSM

`DIDComm` в этом контуре используется только как secure handshake/payload overlay для onboarding и sensitive transition messages; DID method choice, `Binding VC` format и revocation registry architecture принадлежат соседним owner-sections и здесь не переопределяются. [A52]

> **Допущение [A75, proposal]:** Revocation cascade строится как semantic hard-hide plus infrastructural eviction cascade: authoritative invalidation или credential revocation немедленно убирает запись из semantic candidate set; Edge Cache, Regional DHT и `Almanac` сходятся через свой propagation contract в `→ L2/D04 §4`, не меняя owner-границу.

| Состояние | Вход | Выход / переход |
|-----------|------|-----------------|
| `PendingPublish` | агент инициирует publish | `DIDCommHandshake` при accepted secure channel; `Abandoned` при timeout |
| `DIDCommHandshake` | identity-rooted handshake и endpoint proof | `CapabilityValidated` при success; `PendingPublish` при retry; `Abandoned` при repeated failure |
| `CapabilityValidated` | card schema, policy tags, required claims validated | `Published` при successful semantic normalization; `Quarantined` при policy mismatch |
| `Published` | запись стала видимой для ADS | `Active` после propagation-ready acknowledgment; `EvictionPending` при missing freshness renewal |
| `Active` | запись участвует в discovery и `cfp` | `SoftDeregistered`, `Quarantined` или `Revoked` по trigger |
| `SoftDeregistered` | graceful offboarding or operator stop | `EvictionPending` until stale windows close |
| `Quarantined` | validation failure, suspicious mismatch or policy breach (ADS record quarantine, distinct from circuit-breaker `Agent Quarantine` в `→ L2/D07 §6`) | `Active` after remediation or `Revoked` if failure confirmed |
| `Revoked` | authoritative invalidation / credential revocation | immediate semantic hide + transition to `EvictionPending` |
| `EvictionPending` | stale-card window or revocation propagation in progress | `Evicted` after cache/DHT windows elapse |
| `Evicted` | запись удалена из admissible set | terminal state until new onboarding cycle |
| `Abandoned` | onboarding did not complete within retry budget | terminal state until fresh publish request |

Практическое правило FSM простое: first publish не считается завершённым до `Published/Active`, graceful exit идёт через `SoftDeregistered -> EvictionPending -> Evicted`, а security or credential invalidation переводит запись в `Revoked` с immediate semantic hide even before infrastructure convergence completes. [A52][A75]

**Mid-`cfp` Revoked rule:** если кандидат transitions в `Revoked` во время активного `cfp`, его запись немедленно удаляется из admissible и eligible sets матчинга `L2/D06 §4.5`; никакие новые binding offers в его адрес не выдаются с момента revocation event. Infrastructure-side propagation timing (Edge Cache invalidation, DHT convergence, Almanac fallback) — задача `→ L2/D04 §4.3` per `[A75]`. [A74][A75]

`ADS` отвечает на вопрос "кого можно рассматривать и какие поля допустимо показать", `CNP` отвечает на вопрос "как получить winner binding", а escrow/start ticket отвечает на вопрос "как превратить winner binding в исполнимое обязательство". В этом и состоит owned answer `L2/D06` на matching problem. [A8][A52]

---

## 5. Microsandbox execution и CSB

`Microsandbox` как boundary object принадлежит `→ L2/D01 §2`; `Circuit Breaker` как security owner-concept принадлежит `→ L2/D07 §6`. В `D04` полно описываются only execution semantics inside and across sandbox boundaries. То есть `D04` владеет не определением sandbox как такового, а вопросом, как coordination flow проходит через policy gates и как cross-sandbox handoff становится допустимым.

### 5.2 Execution inside Microsandbox

Coordination layer должен трактовать `Microsandbox` как execution context с явными predicates: допустимые tools и network paths, egress restrictions, need for stronger confidential compute path, allowance or denial of cross-sandbox handoff и required dispute/oversight path. Именно поэтому `D04` использует `Microsandbox` как procedural execution envelope, сохраняя boundary-definition owner в `→ L2/D01 §2`.

### 5.3 Formal permeability model

Screened evidence для `G14` поддерживает layered permeability model rather than one ideal primitive. Для `D04` это раскладывается так: policy verification, isolation primitive, domain-specific enforcement и empirical escape benchmarking должны работать совместно. [C9]

| Слой | Coordination question |
|------|-----------------------|
| policy layer | разрешён ли класс операции в данном profile |
| isolation layer | каким runtime primitive выполняется workload |
| domain layer | какие sector-specific obligations добавляются |
| empirical layer | как измеряется unacceptable escape / leakage risk |

> **Resolve-in-draft (G14):** `v4.0` принимает layered permeability model как correct direction, но не закрывает formal metric и governance thresholds по `OI-053`. [C9]

### 5.4 CSB как owned concept D04

`CSB` является owned concept `D04`. Его минимально полное определение в этом draft-е: coordination component, который делает cross-sandbox transition atomically explainable и policy-aware. Он удерживает общий context для rule reconciliation, neutral settlement handoff и audit continuity; поэтому это не просто "шлюз", а активный coordination surface.

| Функция `CSB` | Зачем нужна |
|---------------|-------------|
| identity / credential check | нельзя слепо доверять local assumptions другой sandbox |
| rule reconciliation | нужно вычислить admissible stricter path |
| neutral settlement handoff | переносит match result в cross-sandbox-safe escrow path |
| audit continuity | сохраняет единый trace для обеих сторон |

### 5.5 OI-017: federation vs single instance

Сохранённый internal note полезен для synthesis, но не закрывает topology. Главный вопрос `OI-017`: должен ли `CSB` быть single coordination surface или federation of broker instances with shared policy semantics. [C10]

Single instance упрощает deterministic routing и auditability; federation повышает resilience and locality, но усложняет policy convergence, sequencing и dispute attribution. Поэтому в `v4.0` корректно зафиксировать только одно жёсткое правило: любой cross-sandbox handoff обязан идти через brokered coordination context. [C10]

> **Resolve-in-draft (OI-017):** `CSB` фиксируется как обязательный cross-sandbox surface, но topology remains open. [C10]

В рабочем варианте черновика принимаются три правила:

- local execution stays local while policy predicates remain satisfiable;
- task/data/funds crossing sandbox boundary must go through `CSB`;
- cross-sandbox settlement without common broker context считается architecturally invalid.

Так `D04` отвечает на вопрос execution across boundaries не абстракцией, а governed state transition.

---

## 6. Oracle trust и DisputeModule

`Oracle stack` как infrastructure layer принадлежит `→ L2/D03 §5.4`. `DisputeModule` как canonical conflict-routing component принадлежит `→ L3/D08 §2-§3`. В `D04` owned only the consumption layer: как verification boundary использует oracle trust bundle и как non-terminal cases передаются в dispute routing instead of improvised local settlement.

### 6.2 Oracle trust model

Сильнейший screened вывод здесь такой: coordination layer не должен полагаться на one-shot oracle primitive. Для `OracleEscrow` и first-line verification нужен trust bundle из dynamic oracle selection, staking/slashing-backed reliability и aggregated quality scoring. [C5]

| Trust signal | Coordination role |
|--------------|-------------------|
| dynamic selection | снижает capture risk и поддерживает task-specific oracle choice |
| stake / slashing | создаёт downside for bad oracle behavior |
| aggregated score | не даёт одному noisy oracle deterministically decide payout |

Это не заменяет owner-description oracle stack в `D03`, но даёт enough guidance для settlement gate.

### 6.3 Three-level escalation

В `v4.0` сохраняется coordination-side three-level escalation:

| Уровень | Что делает | Когда нужен |
|---------|------------|-------------|
| Level 1 | automated/oracle-backed verification | objective disputes |
| Level 2 | bounded panel or human review | subjective or conflicting evidence |
| Level 3 | external/legal wrapper | jurisdictional or enforceability-heavy cases |

Полные triage semantics, conflict taxonomy и dispute records описываются в `→ L3/D08 §1-§4`; здесь важно только то, что settlement must wait for terminal outcome once Level 1 becomes insufficient.

### 6.4 DisputeModule as projection

`DisputeModule` описывается полностью в → `L3/D08` (Конфликтно-арбитражное). Здесь: coordination layer использует его как boundary between `Verification` and `Settlement`.

Практически это означает, что `Verification` produces evidence bundle, non-terminal outcome переводит state в dispute route, `DisputeModule` назначает classification/automation level/resolver path, а `Settlement` resumes only after terminal outcome.

Для `D04` корректно зафиксировать три правила:

1. oracle-backed settlement допустим only with explicit trust bundle; [C5]
2. dispute escalation есть state transition, not operator improvisation; [C5]
3. settlement finality is conditional on dispute outcome when objective verification is insufficient. [C5]

---

## 7. Интеграция координационного стека

Ключевой вопрос `D04` закрывается только тогда, когда видно, как off-chain matching связывается с on-chain escrow, sandbox execution и dispute-aware settlement в одну последовательность. Именно integration path делает отдельные механизмы стеком, а не списком модулей. [C2][C3][C4][C5][C9]

### 7.2 Binding path

Рабочий pipeline `v4.0` выглядит так:

1. `ADS` собирает admissible candidate set. [C2][C3]
2. `CNP`/auction branch формирует winner binding. [C1][C7][C8]
3. `EscrowFactory` materializes settlement-safe commitment surface.
4. `Microsandbox` gate определяет local execution constraints, а `CSB` обслуживает cross-sandbox handoff. [C9][C10]
5. `Verification` получает trust signals и либо открывает settlement, либо создаёт dispute route. [C5]
6. `Settlement` и `Distribution` завершают flow по rules selected escrow path. [C4]

### 7.3 Sequence diagram concept

```text
Requester
  -> ADS: discover candidates
ADS
  -> Coordinator: ranked candidate set
Coordinator
  -> CNP/Auction: cfp / bids / winner selection
CNP/Auction
  -> EscrowFactory: instantiate selected escrow path
EscrowFactory
  -> Microsandbox or CSB: issue execution ticket
Microsandbox / CSB
  -> Verification: output + attestation + telemetry
Verification
  -> Oracle trust bundle: score / validate
Oracle trust bundle
  -> Settlement: release / refund / split
Settlement
  -> Distribution: final fan-out
Settlement
  -> DisputeModule: if verification remains non-terminal
```

Эта схема intentionally остаётся sequence concept, а не formal API spec. Но она фиксирует правильную архитектурную границу: matching is flexible and mostly off-chain; commitment, settlement and non-terminal dispute routing remain auditable state transitions.

### 7.4 Minimal interface artifacts

| Артефакт | Где возникает | Зачем нужен |
|----------|---------------|-------------|
| `task_spec_hash` | `Initiation` / `cfp` | фиксирует предмет координации |
| `candidate_context` | `ADS` | связывает discovery и ranking |
| `winner_binding` | `CNP` / auction | делает selection auditable |
| `escrow_id` | `EscrowFactory` | даёт settlement anchor |
| `execution_ticket` | sandbox gate / `CSB` | authorizes execution |
| `evidence_bundle` | `Verification` | переносит output and policy outcome |
| `dispute_record_id` | dispute route | удерживает non-terminal case in one lifecycle |

Координационный стек N4Y.AI в `v4.0` следует понимать как hybrid execution fabric, где discovery and ranking stay flexible, commitment and settlement stay escrow-anchored, sandbox boundaries stay first-class constraints, oracle/dispute routing gates finality, а cross-sandbox work is valid only through brokered coordination context. Именно это и есть прямой ответ `D04` на ключевой вопрос.

---

## Связь с другими измерениями

| Измерение | Связь |
|-----------|-------|
| **L2/D01 (Онтология)** | `Microsandbox` определяется в `→ L2/D01 §2`, а `D04` отвечает за execution semantics внутри и между этими границами. |
| **L2/D02 (Инструменты)** | `AGC`, `Qi Energy` и `Revenue Distribution` используются здесь только как operational settlement instruments; полное описание в `→ L2/D02 §4-§5`. |
| **L2/D03 (Инфраструктура)** | Smart-contract stack, `EscrowFactory.sol` deployment surface и `Oracle stack` живут в `→ L2/D03 §5`; `D04` использует их для coordination routing. |
| **L2/D07 (Безопасность)** | Circuit breakers и security hardening принадлежат `→ L2/D07 §5-§8`; в `D04` остаётся их execution-side projection. |
| **L2/D08 (Нормативно-правовое)** | AI Act, MiCA, GDPR и legal finality ограничивают dispute/settlement paths, но owner-описание находится в `→ L2/D08 §1-§5`. |
| **L3/D03 (Обмен)** | `D04` выбирает escrow path по task profile, но полные escrow patterns, settlement rails и dispute economics принадлежат `→ L3/D03 §3-§6`. |
| **L3/D06 (Конкуренция)** | Auction theory и anti-collusion market logic живут в `→ L3/D06 §2-§4`; `D04` использует их как mechanism-selection layer. |
| **L3/D08 (Конфликты)** | `DisputeModule` и conflict-routing lifecycle принадлежат `→ L3/D08 §2-§3`; в `D04` они используются как boundary between verification and settlement. |

---

## Открытые вопросы

### Требуют решения (Critical / High)

1. **[High] G11: formal state machine for phase transitions and rollback.**
   > **Resolved (Phase 8):** В `v4.0` принят frontier-gated pattern `stage -> validate -> commit`, а abort path сформулирован как idempotent compensation + cascade unwind. [C4]
   > **Open.** Не закрыты exact transition events, timeout windows, retry budgets и список irreversible-effect cases.

2. **[High] OI-053: formal permeability metric for `Microsandbox` execution.**
   > **Phase 8 baseline adopted (OI remains open):** Принят layered model `policy -> isolation -> domain enforcement -> empirical benchmarking`. [C9]
   > **Open.** Нужны measurable thresholds, governance-tunable limits и production calibration.

### Требуют исследования (Medium)

3. **[Medium] OI-005: final ADS topology.**
   > **Phase 8 baseline adopted (OI remains open):** `ADS` принят как multi-source meta-registry over signed `Agent Cards`, curated registries and decentralized registries. [C2][C3]
   > **Open.** Не закрыты precedence rules, stale-card eviction, privacy posture и failover semantics.

4. **[Medium] OI-017: `CSB` federation vs single instance.**
   > **Resolved (Phase 8):** Зафиксировано, что любой cross-sandbox handoff обязан идти через brokered coordination surface. [C10]
   > **Open.** Не определены topology rule, sequencing model и dispute attribution across brokers.

5. **[Medium] OI-054: `VCG` scaling beyond tractable envelope.**
   > **Phase 8 baseline adopted (OI remains open):** Exact `VCG` признан bounded path; для high-complexity cases допустимы approximation/dynamic alternatives. [C7]
   > **Open.** N4Y-specific cutover threshold и acceptable loss of incentive guarantees не определены.

6. **[Medium] G9: exact cutover rule for complex auctions.**
   > **Resolved (Phase 8):** Введён routing-by-features: complexity, urgency, collusion risk, valuation heterogeneity. [C7][C8]
   > **Open.** Нужна canonical decision table with calibrated thresholds and fallback rules.

7. **[Medium] G14: governance calibration of permeability enforcement.**
   > **Resolved (Phase 8):** Execution isolation больше не трактуется как one-primitive question; принята layered enforcement model. [C9]
   > **Open.** Не определены operational metrics, escalation triggers и acceptable false-positive cost.

8. **[Medium] G15: task-to-mechanism routing policy.**
   > **Resolved (Phase 8):** `MACI` принят как selective anti-collusion wrapper, `Reverse Vickrey` как fast path, `VCG` как bounded complex path. [C1][C8]
   > **Open.** Нужны canonical mechanism catalog, calibration dataset и feedback loop for routing updates.
