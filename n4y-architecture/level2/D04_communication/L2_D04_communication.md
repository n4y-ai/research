# УРОВЕНЬ 2 / ИЗМЕРЕНИЕ 04: КОММУНИКАЦИОННО-ПРОТОКОЛЬНОЕ

> **Уровень:** L2 — Среда (инфраструктура)  
> **Измерение:** D05 — Communication Infrastructure  
> **Версия:** 4.0  
> **Статус:** Финал v4.0 | Дата: 02.04.2026  
> **Зависимости:** L1/D05, L2/D06, L2/D03, L2/D07, L2/D09

---

## Ключевой вопрос

**Как среда связывает агентов друг с другом и с внешними системами, и какие сетевые и коммуникационные стандарты при этом используются?**

---

## Аннотация

**Scoping note:** OI-004 (message broker final choice) и OI-006 (Protocol Gateway production spec) зарегистрированы как Critical/open — они не блокируют архитектурную спецификацию D05; D05 описывает текущий working baseline (NATS JetStream + Agentgateway v1.0.1), финальный выбор через governance. OI-005 (ADS topology) — D05 владеет инфраструктурной топологией ADS; coordination semantics → L2/D06 §4 (OI-169); topology finalization pending OI-005 governance. Все claim-теги `[C{N}]` ссылаются на `evidence/normalized-corpus.md` (C1–C14).

Этот черновик фиксирует инфраструктурный ответ `L2/D04`: связь между агентами в N4Y.AI строится не одним протоколом, а слоистым коммуникационным стеком, где `NATS JetStream` закрывает low-latency broker plane, `Agent Gateway` закрывает ingress / protocol translation, `ADS` даёт discovery-topology, а `OpenTelemetry` с `traceparent` закрывает end-to-end observability. При этом семантика сообщений и coordination-логика не дублируются: граница с `L1/D05` проходит по agent-level protocol semantics, а граница с `L2/D06` проходит по ADS matching semantics. [C1][C4][C5][C11][C13]

По сравнению с `[PREVIOUS VERSION]` документ сохраняет ключевые опоры legacy-версии, то есть `NATS` как primary control-path broker, gateway-центричную работу с внешними системами, `10M+` scaling posture и distributed tracing. Однако `v4.0` жёстче проводит ownership discipline: `ADS` разделён по `OI-169` на infrastructure topology здесь и coordination semantics в `→ L2/D06 §4`; `A2A` обновлён до фактов `v1.0` по transport/versioning; large-payload path вынесен в `JetStream Object Store`; а security/governance детали оставлены проекциями на соседние измерения. [C8][C9][C11][C13][C14]

---

## Содержание

1. [6-уровневый коммуникационный стек](#1-6-уровневый-коммуникационный-стек)
2. [Транспортный стек](#2-транспортный-стек)
3. [Agent Gateway](#3-agent-gateway)
4. [ADS инфраструктурная топология](#4-ads-инфраструктурная-топология)
5. [Масштабирование и надёжность](#5-масштабирование-и-надёжность)
6. [Observability](#6-observability)

---

## 1. 6-уровневый коммуникационный стек

Эта секция отвечает на `G2`, но в `v4.0` стек нормализуется как **рабочая модель ответственности**, а не как попытка заново переписать semantics-layer. На языке ownership правило простое: `L1/D05` отвечает за то, **какие agent-facing protocol surfaces существуют и где проходит граница между `A2A`, `MCP`, `DIDComm` и `AG-UI`**, а `L2/D04` отвечает за то, **какой инфраструктурой эти поверхности доставляются при реальном масштабе и реальной отказоустойчивости**. `→ L1/D05 §2`. [C4][C11][C13]

### 1.1 Шестислойная модель

```text
L6 Семантическая поверхность    -> A2A task semantics, DIDComm overlay, MCP boundary
L5 Протокольные bindings        -> gRPC / JSON-RPC / REST interfaces, protocolVersion
L4 Gateway и ingress control    -> Agent Gateway, authz hooks, rate limiting, policy
L3 Broker / messaging plane     -> NATS JetStream, Kafka, Redis Streams
L2 Discovery infrastructure     -> Edge Cache -> DHT -> Almanac
L1 Network underlay             -> HTTP/2, HTTP/3, QUIC, mTLS, service mesh
```

Эта схема полезна тем, что отделяет «что именно агент говорит» от «через какие transport rails это проходит». Верхние два слоя проецируют уже существующие выводы `L1/D05`: `A2A` остаётся peer-agent surface, `MCP` остаётся agent-to-tool surface, `DIDComm` остаётся security overlay, а не transport replacement. `→ L1/D05 §2.3`. Здесь, в `D05`, owned не их semantics, а обеспечивающая их delivery infrastructure. [C6][C13]

### 1.2 Ответственность по слоям

| Слой | Owned / projection | Что фиксируется в `L2/D04` |
|------|--------------------|----------------------------|
| `L6` | projection | Какая поверхность включается для данного маршрута; полная semantics остаётся в `→ L1/D05 §2` |
| `L5` | shared | `A2A v1.0.0` приносит `gRPC`, `supportedInterfaces[]` и per-interface `protocolVersion`; это важно как infrastructural binding rule и как база staged migration / rollback policy [A10][C13][C14] |
| `L4` | FULL | ingress, translation, rate limiting, protocol-aware proxying и observability gateway [C4] |
| `L3` | FULL | выбор основного broker plane, delivery semantics, DLQ/backpressure posture, artifact routing [C1][C2][C3][C8][C9][C10] |
| `L2` | FULL with split | physical discovery topology owned здесь; scoring/matching logic проецируется в `→ L2/D06 §4` [C11] |
| `L1` | FULL with security projection | underlay transports и mesh posture описываются здесь; full security policy остаётся в `→ L2/D07 §6-§9` [C6] |

### 1.3 Граница L1 vs L2

Практическая граница между `L1/D05` и `L2/D04` проходит не по слову «протокол», а по вопросу владения. Если вопрос звучит как «какую роль играет `A2A` относительно `MCP` и `DIDComm` в agent behavior», это `L1`. Если вопрос звучит как «какой gateway, broker, cache и network underlay обеспечивают этот обмен на масштабе», это `L2`. Именно поэтому в этом черновике `A2A` используется как инфраструктурный факт про transport bindings и version negotiation, но не как место для полного пересказа task lifecycle. `→ L1/D05 §2.2-§2.4`. [C13][C14]

Отдельно это снимает дублирование с `L2/D06`. `ADS` как выбор кандидата, `cfp`-логика и ranking semantics описываются полностью в `→ L2/D06 §4`; в `D05` `ADS` нужен как сеть кэшей, DHT и fallback-to-Almanac. Это и есть формальная трактовка `OI-169`. [C11]

### 1.4 Рабочее правило маршрутизации

Для N4Y.AI полезно держать одно короткое routing rule:

1. Если нужен peer-agent lifecycle, входить через `A2A` surface.
2. Если нужен tool/resource invocation, входить через `MCP` surface.
3. Если нужен confidential DID-rooted exchange, поднимать `DIDComm` как overlay, а не заменять им весь broker plane.
4. Если нужен scale-out delivery, опускаться в `Agent Gateway` + `NATS` + `ADS` infrastructure.

Такой порядок совместим с `L1/D05`, но устраняет старую путаницу `[PREVIOUS VERSION]`, где semantic, gateway и broker layers местами были сшиты в один narrative block. [C1][C4][C11][C13]

---

## 2. Транспортный стек

Эта секция отвечает прежде всего на `G1`, `G9`, `G10` и `G11`. Корпус даёт достаточно сильную опору, чтобы зафиксировать **committed production baseline**: `NATS JetStream` для low-latency control path, `Kafka` как комплементарный event/audit rail, `Redis Streams` как локальный stateful / HITL-путь, а под ними - `HTTP/2` / `HTTP/3`, `gRPC`, `mTLS` и service-mesh controls. Для N4Y.AI это не формулируется как irrevocable vendor lock-in, но production baseline control plane зафиксирован: `NATS JetStream` остаётся primary broker, а companion rails покрывают другие workload-профили. [A50][C1][C2][C6][C8][C9][C10]

### 2.1 Базовая роль transport-компонентов

| Компонент | Роль в `L2/D04` | Почему не взаимозаменяем |
|-----------|-----------------|--------------------------|
| **`NATS JetStream`** | committed production broker baseline для N4Y.AI control path | низкая latency и простая ops-модель делают его базовым выбором для intra-platform messaging [A50][C1] |
| **`Kafka`** | audit / durable event rail / Kafka-compatible high-throughput streams | полезен как companion stream, но не как drop-in replacement low-latency control path [A50][C10] |
| **`Redis Streams`** | локальные очереди, transient state, HITL-adjacent buffering | удобен как near-runtime buffer, но не как canonical multi-region broker baseline |
| **`Object Store` поверх JetStream** | large-payload artifact path | снимает ограничение прямой oversized messaging и keeps same persistence substrate [C8][C9] |

`Redpanda` рассматривается как companion option для Kafka-compatible high-throughput / large-payload streams, а не как replacement control-path broker. [A50]

Из этого следует инфраструктурная норма: N4Y.AI не обязан быть «single broker system». Напротив, transport stack intentionally tiered: fast broker, durable event rail и auxiliary transient queues решают разные задачи и не должны схлопываться в один продуктовый выбор. [A50][C1][C8][C10]

### 2.2 Delivery semantics: что реально обещает JetStream

Ключевая корректировка `v4.0` относительно более уверенных legacy-формулировок состоит в том, что `JetStream` **не даёт exactly-once как нативную гарантию**. Screened baseline — `at-least-once`, а значит, совместимый design должен прямо предполагать explicit ACK, redelivery и idempotent обработку на прикладном уровне. [C2]

Отсюда следует три operational consequences:

1. масштабируемые consumers должны строиться как `pull consumers`, а не как бесконтрольный push fan-out; [C2]
2. retry budget должен задаваться через `MaxDeliver`, `BackOff`, delayed redelivery и bounded processing windows; [C3]
3. exactly-once effects можно получить только через idempotency keys и stateful deduplication вне самого broker. [C2]

DLQ-path также не встроен как first-class primitive. Корректная трактовка `G9` - это advisory-driven DLQ pattern: отдельный stream / consumer path, слушающий `MaxDeliver` и termination advisories, а не магический встроенный dead-letter topic. [C3]

Для aggressive latency requirements применяется trust-separated pattern: `DIDComm` используется для initial trust establishment и для confidential DID-rooted payloads, а рабочие hot-path messages маршрутизируются через `NATS` с `mTLS`. Это сохраняет mitigation `OI-029` и не превращает `DIDComm` в обязательную обёртку каждого broker message. [A10][A50][C9][C16]

**Addendum (OI-045): runtime characterization для `A2A + DIDComm`.**

| Компонент | Evidence-backed behavior | Architectural consequence |
|-----------|--------------------------|---------------------------|
| cached DID resolution | при warmed cache возможен sub-millisecond path | cache warming materially reduces hot-path overhead, но не является universal SLA [C12] |
| DIDComm pack / unpack | reference implementations показывают примерно `4-10 ms` на authcrypt / anoncrypt message | приемлемо для bootstrap / confidential exchange, но не аргумент для оборачивания каждого hot-path broker message [C10] |
| signature verification / key agreement | отдельные crypto micro-operations лежат в диапазоне tens of microseconds to roughly `50-125` microseconds | чистая криптография сама по себе редко является главным bottleneck; больше влияют resolver / runtime / deployment conditions [C11] |
| cold DID / credential path | cold resolution и SSI-heavy flows могут занимать от `<0.5 s` до `3-4 s` | cold-path budget должен оставаться governance-tunable, а не фиксироваться как hard universal number [C13] |
| deployed runtime variance | healthy message path может держаться в районе `10-30 ms`, но under throttling p95 способен деградировать до `1-5 s` | production budgeting должен учитывать CPU / deployment posture и explicitly избегать invented universal SLA [C14] |

Batching и streaming amortize transport setup only: `DIDComm` envelope semantics, freshness protection и replay discipline сохраняются per message и не исчезают от reuse connection. [C9][C16] Production precedents для DIDComm-heavy stacks чаще подтверждают async / eventual-consistency operating model, чем synchronous low-latency guarantee. [C17]

Payload overhead и status checks тоже требуют дисциплины: reference-based status artifacts (`Bitstring Status List`) лучше подходят для runtime verification, чем попытка переносить revocation payload в каждое сообщение, а ciphertext / attachment overhead зависит от envelope format и implementation choice. [C18][C19][C20] Следовательно, inline-vs-reference profile, cache cadence для DID Document / status artifacts и throughput budgets фиксируются как governance-tunable platform policy, а не как upstream-mandated defaults. [C8][C18]

### 2.3 Underlay: gRPC, HTTP/3, QUIC, mTLS

Под broker plane у `L2/D04` есть ещё один слой — network underlay. Здесь важно не переобещать: screened evidence не говорит, что `QUIC`/`HTTP/3` должны заменить весь внутренний messaging fabric. Он говорит более узкую и полезную вещь: `HTTP/3` и `QUIC` особенно релевантны для lossy, mobile или short-lived external paths, тогда как на стабильных внутренних long-lived links выигрыш может быть умеренным. [C6]

Практический baseline получается такой:

| Path | Предпочтительный underlay | Комментарий |
|------|----------------------------|-------------|
| north-south ingress | `gRPC` / `HTTP/2`, selective `HTTP/3` | разумное место для внешних клиентов, gateways и межрегионых edge-paths [C6][C13] |
| east-west stable links | `gRPC` + `mTLS` + service mesh | лучший баланс между зрелостью и наблюдаемостью [C6] |
| lossy / short-lived networks | `QUIC` / `HTTP/3` where implementation mature | особенно полезно там, где connection recovery и stream independence дают реальный выигрыш [C6] |

Полная zero-trust и systemic-risk policy при этом не owned здесь. Она описывается полностью в `→ L2/D07 §6-§9`. Здесь: `mTLS`, mesh selection и underlay choice рассматриваются как infrastructural enablers secure delivery, а не как вся security model целиком. [C6]

### 2.4 Большие сообщения и artifact path

`G11` закрывается не тезисом «увеличим max message size», а архитектурным разделением small-message и large-object paths. Корпус подтверждает, что у `JetStream` есть практическое ограничение на oversized messages и documented сбои в диапазоне около `32MB` для части версий `v2.10/v2.11`; следовательно, прямая пересылка крупных payloads не должна быть default path. [C8][C9]

Рабочее правило для `v4.0`:

1. `<= 1MB` — обычный broker message path;
2. `1MB .. ~32MB` — избегать прямого oversized publish, предпочитать object-backed indirection;
3. `> ~32MB` — `JetStream Object Store` или комплементарный large-stream rail с pointer-in-message pattern. [C8][C9][C10]

Это не означает, что `Redpanda` становится новым primary broker. Более точная формулировка такая: он рассматривается как **комплементарная** опция для Kafka-compatible larger-payload / high-throughput streams, а не как замена `JetStream` control plane. Для `>10MB` это архитектурный split-path verdict, а не head-to-head benchmark claim; switching threshold остаётся governance-tunable. [A50][C10]

**Quantum-safe (long-horizon):** `NIST PQC` `FIPS 203/204/205` уже финализированы, а `AGNTCY SLIM` включает PQC roadmap через MLS, но текущая реализация линии `0.1.x` не является production-ready transport baseline. Для N4Y актуальна позиция мониторинга: интеграция PQC возвращается в активный план при достижении production maturity implementation libraries, ориентировочно не ранее горизонта `2027+` (оценка на основании текущего production maturity landscape — AGNTCY SLIM v0.1.x, NIST PQC finalized Aug 2024). [A50]

---

## 3. Agent Gateway

`G3` и часть `G7` сходятся здесь: внешняя коммуникация среды не должна приходить напрямую в broker plane. Нужна protocol-aware точка входа, которая понимает agent-specific surfaces и одновременно умеет работать с привычными enterprise ingress patterns. В корпусе таким baseline выступает `Agent Gateway`, а в refresh-пакете — `Agentgateway` Linux Foundation / Solo.io line. [C4][C13]

### 3.1 Почему gateway нужен как отдельный слой

`Agent Gateway` в `L2/D04` — это не просто API proxy. Его роль уже строже и сильнее:

| Функция | Почему она принадлежит gateway-слою |
|---------|-------------------------------------|
| A2A-aware ingress | peer-agent calls должны различаться от обычного REST traffic [C4][C13] |
| A2A ↔ gRPC / REST bridging | стандарт не даёт NATS binding из коробки; нужен translation edge [C13] |
| authn/authz hooks | внешняя коммуникация должна получать identity-aware admission control [C4] |
| rate limiting и shaping | backpressure нельзя откладывать только на broker-level [C2][C4] |
| observability injection | traces, correlation IDs и policy decisions должны попадать в единый telemetry path [C5] |

В terms of ownership полная agent-level semantics A2A остаётся в `→ L1/D05 §2`, но gateway как infrastructure ingress layer owned здесь. `Agent Gateway` / `Agentgateway` фиксируется как mandatory ingress и policy enforcement baseline для внешних агентских коммуникаций N4Y.AI. [A51][C4][C13]

Для covered scenarios по `EU Data Act` gateway должен экспонировать только auditable ingress hook для команд `pause` / `withdraw`, уже авторизованных owner-layer в `L3_D03 §7.3`; legal trigger taxonomy, `PLD` posture и escrow semantics здесь не дублируются (источники и trigger-таксономия см. `L3_D03 §7.1` Art. 36 requirements + `L3_D03 §7.3` kill-switch governance trigger). [A51]

Это остаётся PROJECTION-ONLY: gateway проверяет identity / policy context, пишет trace и передаёт команду в bounded execution path, но не принимает самостоятельного решения о применимости `Kill switch` или распределении liability (подробности → `L3_D03 §7.3`). [A51]

### 3.2 Agentgateway baseline и policy layer

В исследовательском baseline `research.md` gateway зафиксирован как `Agentgateway v1.0.1`; refresh evidence одновременно показывает продолжающуюся enterprise line `2.3.x`, Rust-based proxy, K8s Gateway API routes, rate limiting, extAuthz hooks, `OpenTelemetry` и A2A+MCP protocol awareness. Поэтому корректнее фиксировать не узкий marketing version claim, а **устойчивую platform line Agentgateway / Agent Gateway**, пригодную как reference implementation для N4Y ingress. [C4]

С policy-side в `v4.0` фиксируется deployment contract: policy layer должен использовать extAuthz-compatible declarative engine (`OPA`, `OAuth`, custom auth hooks) через `Agentgateway 2026.5.x`-совместимую platform line; policy decisions должны быть externally auditable и пригодными для cross-sandbox admission rules. N4Y-specific boundary `A2A surface -> gateway -> NATS subject space` оформляется как gateway-mediated envelope adapter, а не как built-in upstream capability; telemetry и correlation hooks должны проходить через весь `gateway -> broker -> consumer` path. [A51]

### 3.3 A2A ↔ gRPC ↔ NATS translation

> **Note:** `A2A v1.0.0` является canonical baseline для transport facts §3.3, включая `gRPC`, `supportedInterfaces[]` и per-interface version negotiation. [A10]

`A2A v1.0.0` приносит важные transport facts: `gRPC` bidirectional streaming, `supportedInterfaces[]`, per-interface `protocolVersion` и более явную версификацию `Agent Card`. Но он **не стандартизует** binding к `NATS JetStream`. Следовательно, связка `A2A surface -> gateway -> NATS subject space` остаётся N4Y-specific adapter decision, а не частью A2A spec. [A51][C13][C14]

> **Design note (G8):** `A2A ↔ NATS` принимается как gateway-mediated envelope adapter, а не как normative protocol binding. Это совместимо с `A2A v1.0.0`, но остаётся архитектурным выбором N4Y.AI, требующим собственного contract и telemetry discipline. [A51][C13][C14]

**Migration plan:** staged rollout `A2A v0.3.0` → `v1.0.0` идёт в три фазы: Phase 1 — staging validation `gRPC` bindings; Phase 2 — canary production deployment с negotiation через `supportedInterfaces[]`; Phase 3 — full cutover. Rollback выполняется на gateway boundary через fallback `JSON-RPC 2.0`, без смены control-path broker. [A10]

Минимальная логика translation слоя выглядит так:

1. north-south запрос приходит как `A2A` / `gRPC` / `REST`;
2. gateway аутентифицирует, нормализует identity и проверяет policy;
3. gateway либо роутит напрямую в service path, либо публикует envelope в broker subject space;
4. reply path идёт через correlation / inbox pattern;
5. trace context протаскивается через весь hop chain. [C4][C5][C13]

### 3.4 Cross-sandbox policy

`G7` нельзя закрыть только словами «gateway применяет политику». Ownership split здесь двойной:

- `Microsandbox` как объект среды принадлежит `→ L2/D01`;
- `CSB` и execution-side межсредовая координация принадлежат `→ L2/D06 §5`;
- full systemic-security guardrails принадлежат `→ L2/D07 §6-§9`.

Здесь, в `D05`, owned более узкий вопрос: **какой инфраструктурный enforcement point стоит на пути межсредовой коммуникации**. Ответ: это gateway / routing layer, применяющий route admission, identity context, policy tags и observability hooks до того, как трафик попадёт в lower transport plane. [A51][C4][C5]

Именно поэтому `OI-030` остаётся открыт. Корпус позволяет закрепить enforcement location, но не закрывает окончательно N4Y-specific policy grammar, rule distribution и precedence между sandbox-local и global policies. [C4]

---

## 4. ADS инфраструктурная топология

> ADS как координационный механизм (discovery logic, candidate ranking, `cfp` path, scoring, privacy-aware field release) → `L2/D06 §4`. Данный раздел описывает физическую топологию инфраструктуры ADS: `Edge Cache → Regional DHT → Almanac`, sync cadence, conflict resolution, failover, partition posture и freshness bounds. [A8][A52]

Эта секция отвечает на `OI-005`, `OI-057` и `OI-169`, но одновременно обязана жёстко сохранить ownership split. Поэтому первое правило `v4.0` звучит так: **`ADS` имеет два разных аспекта**. Coordination semantics, то есть meta-registry logic, candidate ranking, `cfp` path и scoring, принадлежат `→ L2/D06 §4`. Infrastructure topology, то есть как discovery network физически кэширует, реплицирует и деградирует при сбоях, принадлежит `L2/D04`. [A8][A52]

### 4.1 Трёхтировая топология

Рабочая инфраструктурная схема для `ADS`:

```text
Tier 1: Edge Cache      -> ближайший lookup, короткий TTL, локальный latency shield
Tier 2: Regional DHT    -> resilient lookup mesh, federated convergence, partition tolerance
Tier 3: Almanac         -> authoritative fallback / anchoring registry
```

Эта нормализация лучше legacy-версии по двум причинам. Во-первых, она перестаёт смешивать discovery network и matching algorithm. Во-вторых, она соответствует screened evidence, где discovery resilience приходит не от одного global coordinator, а от сочетания local/cache-first lookup, DHT-backed resilience и authoritative fallback. Все semantic decisions about admissibility remain in `→ L2/D06 §4`. [A8][A52]

### 4.2 Роль каждого слоя

| Слой | Infrastructure role | Что не надо ему приписывать |
|------|---------------------|-----------------------------|
| **Edge Cache** | быстрый локальный ответ, absorption of hot lookups, shielding against repeated remote fetches | не выполнять full candidate ranking |
| **Regional DHT** | resilient distributed directory, federated convergence, conflict merge under bounded staleness | не быть единственным source of truth |
| **Almanac** | authoritative fallback / anchoring registry | не становиться low-latency path for every query |

Такое разнесение совместимо и с `L2/D06`, и с глоссарием. Координационное измерение продолжает владеть вопросом «как discovery влияет на matching», а `L2/D04` фиксирует, что быстрый путь должен по возможности заканчиваться на кэше, а не на дальнем registry lookup. `→ L2/D06 §4.1-§4.5`. [A8][A52]

### 4.3 Federated sync, freshness и partition posture

`ADS` topology не обещает perfect global consistency. Напротив, рабочий baseline таков: discovery infrastructure должна быть partition-tolerant, cache-first и bounded-staleness-aware. Screened evidence поддерживает resilient DHT / gossip-style posture и периодические refresh loops, а не centralized always-fresh directory. [A8][A52]

Numeric anchors ниже — governance-tunable baselines, derived from precedent screened evidence (`normalized-corpus.md` `[C8]` Almanac freshness expiry; `[C9]` Kademlia/IPFS DHT refresh defaults). Конкретные значения подлежат governance-калибровке по operational telemetry. [C8][C9]

| Параметр | Текущий topology baseline (governance-tunable) | Source trace |
|----------|------------------------------------------------|---------------|
| cadence / trigger model | event-driven invalidation for hot changes + periodic Regional DHT refresh every ~30 minutes (governance-tunable) | [C9] |
| conflict resolution | OR-Set membership + Last-Writer-Wins register for mutable card attributes | [A52] |
| consistency model | eventual consistency with cache-first reads and authoritative fallback | [A8] |
| staleness budget | Edge Cache should converge before next DHT refresh window; `Almanac` expiry precedent treated as ~3600s authoritative freshness anchor (governance-tunable) | [C8] |

`Regional DHT` therefore хранит membership semantics как `OR-Set`, чтобы add/remove operations не терялись при federated merge, а mutable card attributes (availability snapshot, freshness stamp, projection metadata) сливаются по `Last-Writer-Wins`. Это даёт deterministic merge without forcing strong global lockstep. [A52]

> **Допущение [A75, proposal]:** Revocation and eviction propagate as ordered infrastructure cascade: semantic hide happens upstream in `L2/D06`, Edge Cache performs immediate local invalidation on event or TTL breach, Regional DHT converges on the next refresh window, and `Almanac` remains the authoritative residual liveness anchor until its own expiry/update cycle closes the record.

HA/failover в таком контуре означает не только replica count, но и **route degradation order**: local lookup -> regional DHT -> authoritative fallback. Если часть сети изолирована, инфраструктура должна продолжать обслуживать stale-but-bounded discovery, а не мгновенно падать в глобальный stop-the-world режим. [A8][A52]

### 4.4 Что остаётся вне этой секции

Важно зафиксировать негативные границы:

- `ADS` scoring formula не owned здесь: `→ L2/D06 §4.5`;
- `cfp` lifecycle не owned здесь: `→ L2/D06 §4.4`;
- privacy posture, visibility matrix и capability-only matching не owned здесь: `→ L2/D06 §4.3`, legal doctrine → `L2/D08 §4.3-§4.4`;
- governance-финализация broader federation policy остаётся projection-only к `→ L2/D09 §6`.

Эта секция намеренно отвечает только на вопрос «как discovery network живёт как инфраструктура», а не на вопрос «как платформа выбирает победителя матчинга». [A8][A52]

---

## 5. Масштабирование и надёжность

Здесь сходятся `G5`, `G8` и `G13`. Корпус подтверждает архитектурную масштабируемость transport/discovery stack, но не даёт прямой production case study на `10M+` агентов. Поэтому формулировка `v4.0` должна быть дисциплинированной: scaling confidence — **architectural, not empirically closed**. [C12]

### 5.1 Горизонтальное масштабирование

Основной scaling posture `L2/D04` выглядит так:

| Компонент | Preferred scale mode | Почему |
|-----------|----------------------|--------|
| `JetStream` consumers | horizontal + pull-based | лучше контролирует backpressure и consumer pacing [C2] |
| gateway layer | stateless horizontal replicas | ingress/policy/translation должны масштабироваться независимо от broker [C4] |
| ADS edge layer | cache multiplication near demand | снимает lookup pressure с deeper tiers [C11] |
| DHT tier | horizontal node growth with eventual convergence | resilience grows through distribution, not centralization [C11] |

Отдельно важно сохранить границу с `L2/D03`: provider-side compute scaling, infra substrate и SLA enforcement path описываются там, а здесь рассматривается только communication-plane scaling. `→ L2/D03 §6-§7`. [C12]

### 5.2 Backpressure и overload behavior

В старых narrative-описаниях масштабирование легко выглядело как просто «добавим больше узлов». Корпус делает такую формулу слишком слабой. Правильная версия выглядит так: high-scale messaging работает только вместе с явным backpressure discipline. Для `JetStream` это означает pull consumers, bounded batches, retries с backoff и отказ от implicit infinite redelivery loops. [C2][C3]

Практический набор средств:

1. rate limiting и admission shaping на gateway edge; [C4]
2. pull-based flow control на broker plane; [C2]
3. delayed redelivery и DLQ path для repeatedly failing workloads; [C3]
4. object-backed indirection для large payloads, чтобы не убивать hot message path oversized publishes. [C8][C9]

### 5.3 G8: A2A gRPC + NATS как design decision

Текущий canonical baseline `A2A v1.0.0` усилил infrastructural relevance протокола через `gRPC`, bidirectional streaming и explicit versioning facts. Но screened corpus одновременно не даёт evidence, что community-standard binding `A2A -> NATS JetStream` уже существует как стандарт. Следовательно, `N4Y` формулирует этот слой как **local design note**, а не как external standard guarantee. [A51][C13][C14]

Это важно по двум причинам:

1. compatibility testing ложится на N4Y contract surface, а не на A2A ecosystem;
2. observability, replay и schema governance у gateway-envelope path должны быть own-engineered, а не borrowed from the standard.

Именно в таком виде `G8` можно считать architectural closed, но not standard-closed. [A51][C13][C14]

### 5.4 Schema evolution и versioning

`G13` даёт ещё один признак зрелости инфраструктуры: transport stack перестаёт быть просто сетью сокетов и становится contract-governed system. Screened evidence поддерживает package-based versioning, `reserved` fields, schema-diff checks и staged rollout как необходимую дисциплину для `gRPC` / `Protobuf` interfaces. [C14]

Для `L2/D04` это означает следующие нормы:

| Правило | Infrastructure consequence |
|---------|----------------------------|
| не переиспользовать field numbers | mixed-version agents не ломают decoding path [C14] |
| хранить removed fields как `reserved` | снимается риск hidden wire-format regressions [C14] |
| version control по package / interface | gateway и adapters могут вести multi-version routing [C13][C14] |
| schema diff в CI/CD | broker/gateway contracts не дрейфуют бесконтрольно [C14] |

Поскольку `A2A v1.0.0` уже вводит per-interface `protocolVersion`, N4Y stack может согласованно вести и protocol-level negotiation, и internal protobuf evolution. Mixed-version migration идёт staged rollout-последовательностью: Phase 1 = staging validation `gRPC` bindings; Phase 2 = canary production deployment с negotiation через `supportedInterfaces[]`; Phase 3 = full cutover. Rollback = fallback `JSON-RPC 2.0` на gateway boundary. [A10][C13][C14]

### 5.5 Надёжность как composition, а не как один механизм

Надёжность communication plane не рождается из одного «надёжного брокера». Она получается как composition из четырёх слоёв:

1. ingress controls на gateway;
2. predictable delivery semantics на broker plane;
3. graceful degradation на discovery topology;
4. version-safe evolution на interface contracts.

Это ещё одна причина, почему в `v4.0` транспортный стек описывается как несколько связанных слоёв, а не как выбор одного продукта. [C1][C2][C4][C11][C14]

---

## 6. Observability

`G6` в screened corpus закрыт наиболее чисто. В `L2/D04` observability — это не «мониторинг после факта», а самостоятельный слой коммуникационной инфраструктуры: без end-to-end tracing нельзя доказуемо понять, где оборвался route, кто применил policy и в каком месте broke request chain across gateway, broker и downstream agent surfaces. [C5]

### 6.1 Базовый стек наблюдаемости

Текущий baseline:

| Компонент | Роль |
|-----------|------|
| **`OpenTelemetry`** | единая instrumentation model для gateway, broker-adjacent services и agent hops [C5] |
| **`W3C traceparent`** | переносит trace context через межсервисные и межагентные hops [C5] |
| **`Langfuse`** | практический observability backend для agent traces, prompt/tool spans и debugging flows |

`Langfuse` не является новым owned concept документа и не требует здесь отдельного полного теоретического описания; он важен как concrete fit для OTel-native agent tracing path, особенно если N4Y нужен self-hostable backend для длинных execution chains. [C5]

### 6.2 Что именно надо трассировать

Минимальная цепочка трассировки в `L2/D04`:

1. ingress at gateway;
2. authz / policy decision;
3. publish or route to broker/service path;
4. consumer dispatch or downstream handler;
5. reply / callback / stream completion.

Если chain проходит через несколько агентных hops, `traceparent` должен идти через все hops, иначе observability распадается на локальные логи и перестаёт быть полезной для debugging multi-agent systems. [C5]

### 6.3 Граница с соседними измерениями

Нужно снова удержать ownership discipline:

- semantic interpretation tool / task traces как часть execution narrative относится к `L1/D04`;
- security anomaly interpretation относится к `L2/D07`;
- coordination outcome verification относится к `L2/D06`.

Здесь, в `D05`, owned более узкий вопрос: **как транспортный путь остаётся наблюдаемым на всём своём маршруте**. Это включает propagation, correlation и telemetry hooks, но не заменяет соседние доменные интерпретации. [C5]

### 6.4 Почему observability стоит в теле, а не в приложении

В legacy-артефактах observability часто выглядела «вспомогательной» темой после транспорта. Корпус `v4.0` показывает, что это уже не опциональная добавка. Как только communication stack включает gateway policy, multi-hop routing, object indirection и discovery degradation, отсутствие trace continuity становится архитектурной слепой зоной. Поэтому observability остаётся полноценной шестой секцией тела, а не appendix. [C4][C5][C8][C11]

### 6.5 Kubernetes-native deployment baseline

`L2/D04` фиксирует production ops-baseline для communication plane: `NATS JetStream` разворачивается через `Helm` charts + `NACK` (NATS Kubernetes Controller), поскольку legacy `NATS Operator` deprecated; Kafka-compatible companion rail при необходимости operator-managed deployment ведётся через `Strimzi v1.0.0`; `Redis Streams` / buffer path требует Redis HA с `Sentinel`. Это N4Y ops policy, а не vendor default. [A50][A51]

Практический deployment contract:

| Layer | Production baseline | Ops caveat |
|------|---------------------|------------|
| `NATS JetStream` | `Helm` + `NACK` | `NACK` покрывает `Streams`, `Consumers`, `KeyValues`, `ObjectStores`, `Accounts`, но controller line остаётся singleton-oriented и требует явного ops monitoring |
| Kafka-compatible companion rail | `Strimzi v1.0.0` | применяется для high-throughput / large-payload workloads и не заменяет control-path broker |
| `Redis Streams` | Redis HA + `Sentinel` | используется как local buffer / HITL-adjacent path, а не как canonical broker replacement |

HPA-рекомендация для gateway, broker-adjacent workers и consumers задаётся по двум сигналам одновременно: CPU около `70%` и pending-message / consumer-lag queue depth. Минимальный health/SLO набор должен включать ingress latency, authz error rate, publish-to-consume lag, redelivery / DLQ rate, object-upload stall rate и trace continuity across `gateway -> broker -> consumer`. [A50][A51]

---

## Связь с другими измерениями

| Измерение | Связь |
|-----------|-------|
| **L1/D05 (Коммуникационное)** | `L1/D05` владеет границей между `A2A`, `MCP`, `DIDComm` и `AG-UI`, а `L2/D04` владеет тем, как эти surfaces доставляются через gateway, broker и discovery infrastructure. См. `→ L1/D05 §2`. |
| **L2/D06 (Координация)** | `ADS` имеет split ownership: `L2/D06` описывает matching semantics, `cfp` lifecycle и scoring, а `L2/D04` описывает Edge→DHT→Almanac topology. См. `→ L2/D06 §4`, `OI-169`. |
| **L2/D03 (Инфраструктура)** | `L2/D03` владеет compute/storage substrate, provider failover и SLA enforcement rails, тогда как `L2/D04` владеет communication-plane delivery, ingress и observability. См. `→ L2/D03 §6-§7`. |
| **L2/D07 (Безопасность)** | mTLS, zero-trust posture, systemic-risk policy и security interpretation не принадлежат этому документу полно; здесь они присутствуют как infrastructure enablers communication layer. См. `→ L2/D07 §6-§9`. |
| **L2/D09 (Governance)** | topology federation posture, policy precedence и часть cross-sandbox rule governance остаются governance-tunable, а не окончательно зашитыми в `D05`. См. `→ L2/D09 §6`. |
| **L3/D04 (Контракты)** | SLA tiers, penalties и юридически значимые service commitments не owned в `D05`; здесь описывается только communication infrastructure, которая помогает их исполнять. См. `→ L3/D04 §4`. |

---

## Открытые вопросы

### Требуют решения (Critical / High)

1. **[Critical] OI-004: broker baseline зафиксирован.** `NATS JetStream` принят как committed production broker baseline для low-latency control path, а companion rails остаются отдельными workload-путями.
> **Resolved.** В `v4.0` `NATS JetStream` принят как production baseline, `Kafka` — как companion audit/event rail, `Redis Streams` — как auxiliary queue/cache path. Дополнительный Kafka-compatible rail не отменяет control-path commitment. [A50][C1][C10]

2. **[Critical] OI-005: ADS topology finalization.** D04 использует рабочий ADS baseline и семантически связывает matching/coordination с `L2/D06`, но финальная topology/federation posture, precedence rules и failover policy остаются открытыми до governance-фиксации.
> **Phase 8 baseline adopted (OI remains open):** ADS multi-source pattern принят как рабочий baseline, но final topology contract и policy governance не закрыты. [C2][C3][C5]

3. **[Critical] OI-006: productization и enforcement shape Agent Gateway.** Выбран устойчивый reference path через `Agent Gateway` / `Agentgateway`; deployment contract и adaptation boundary теперь описаны как часть production baseline.
> **Resolved.** Gateway принят как обязательный ingress layer с extAuthz-compatible declarative policy runtime, telemetry/correlation hooks и N4Y-specific `A2A -> NATS` envelope adapter. [A51]

### Требуют исследования (Medium)

4. **[Medium] OI-029: latency overhead security overlays.** `DIDComm` остаётся допустимым security overlay только для trust establishment, а не для hot message path.
> **Resolved.** Зафиксирован trust-separated pattern: `DIDComm` используется для initial trust establishment и для confidential DID-rooted payloads, рабочие control-path сообщения идут через `NATS` с `mTLS`, а `DID Document Cache` управляется как policy-controlled governance-tunable cache с rotation invalidation (см. `§2.2 Addendum`). [A10][A50][A76]

5. **[High] OI-031: Kubernetes-native deployment операторов.** `NATS Operator` deprecated; нужен ops-baseline через `Helm + NACK`, `Strimzi` и `Redis Sentinel`.
> **Resolved.** Production deployment baseline зафиксирован: `NATS JetStream` — через `Helm + NACK` (NATS Kubernetes Controller, legacy `NATS Operator` archived); Kafka-compatible companion rail — через `Strimzi v1.0.0`; `Redis` HA — через `Sentinel`. HPA для broker-adjacent workers строится по CPU-threshold (~70%) + consumer-lag depth. [A50][A51]

7. **[Medium] OI-030: cross-sandbox communication policy grammar.** Enforcement point понятен, но rule distribution, precedence и ownership между global и local policies ещё не закрыты.
> **Open.** Требуется связанное решение между `L2/D04`, `L2/D07` и `L2/D09` по policy model, auditability и rollout discipline. [C4][C5]

8. **[Medium] OI-056: comparative benchmark для больших payloads.** Current evidence already supports `Object Store` / indirection path и позиционирует `Redpanda` как companion option, но не закрывает вопрос head-to-head на `>10MB` workloads.
> **Resolved.** Для `v4.0` принят split-path baseline: large-object routing отделяется от обычного messaging path, а `Redpanda` / Kafka-compatible rail рассматривается как companion option для high-throughput / large-payload workloads. `>10MB` трактуется как архитектурный boundary, а не как closed benchmark race. [A50][C8][C9][C10]

9. **[Medium] OI-058: миграция `A2A v0.3 -> v1.0`.** `v1.0` уже даёт важные infrastructure facts, а operational sequencing зафиксировано как staged migration plan.
> **Resolved.** Принят staged rollout: staging validation `gRPC` bindings -> canary production deployment с negotiation через `supportedInterfaces[]` -> full cutover; rollback идёт через fallback `JSON-RPC 2.0` на gateway boundary. См. `§3.3` и `§5.4`. [A10][C13][C14]

10. **[Medium] OI-055: long-horizon quantum-safe posture.** Quantum-safe transport hardening относится к отдельному горизонту зрелости и не входит в production baseline текущего стека.
> **Deferred.** `NIST PQC` и `AGNTCY SLIM` подтверждают направление эволюции, но интеграция PQC возвращается в активный план только после production-grade maturity implementation libraries; текущий transport baseline не меняется. [A50]
