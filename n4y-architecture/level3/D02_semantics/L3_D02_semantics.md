# УРОВЕНЬ 3 / ИЗМЕРЕНИЕ 02: СЕМАНТИКО-ПРОТОКОЛЬНОЕ

> **Уровень:** L3 — Межагентные взаимодействия  
> **Измерение:** D02 — Semantic-Protocol  
> **Версия:** 4.0  
> **Статус:** Финал v4.0 | Дата: 04.04.2026  
> **Зависимости:** L1/D05, L2/D06, L2/D04, L3/D01, L3/D03, L3/D04

---

## Ключевой вопрос

**На каком языке и через какие смысловые протоколы Логосы понимают намерения и контекст друг друга при взаимодействии?**

---

## Аннотация

Семантико-протокольное измерение отвечает не на вопрос о transport-инфраструктуре и не на вопрос о low-level agent identity, а на вопрос о том, как независимые Логосы связывают значения, намерения, доверие, переговоры и исполнение в один межагентный цикл. По состоянию на 04.04.2026 такой цикл лучше всего описывается как синтез пяти уровней коммуникационного стека и пяти фаз взаимодействия: `Discovery -> Trust Establishment -> Negotiation -> A2A Execution -> Settlement`. При этом ни один внешний стандарт не покрывает весь стек целиком: `A2A v1.0` даёт execution baseline, `DIDComm v2.1` даёт trust messaging, `FIPA ACL` остаётся основой performative semantics, `XAP` даёт полезную lifecycle vocabulary, а `AGNTCY/SLIM` и observability standards закрывают routing и multi-hop context.

**Ответ на ключевой вопрос:** Логосы общаются на языке FIPA ACL перформативов (22 стандартных + 3 N4Y-специфичных), передавая намерения через гибридную BDI/speech-act модель с вероятностной прагматикой. Семантические протоколы реализованы через 5-уровневый стек (Semantic → Protocol → Routing → Identity → Discovery) и 5-фазный lifecycle взаимодействия (Discovery → Trust → Negotiation → A2A Execution → Settlement). Vocabulary mismatch между sandboxes устраняется Verifiable Semantics (51-96% снижение расхождений). Agent Behavioral Contracts (ABC) обеспечивают формальную верифицируемость выполнения намерений.

Нормативный вывод для N4Y.AI таков: `D02` владеет семантикой речевых актов, intention model и правилами semantic interoperability, но не забирает ownership у `L2/D06` для `ADS/CNP/CSB`, у `L2/D04` для `A2A/SLIM/NATS`, у `L1/D05` для `DIDComm` security overlay и у `L3/D03`/`L3/D04` для settlement и формализации договорённостей. Поэтому этот черновик сознательно заменяет прежние FULL-описания чужих слоёв на проверенные проекции с реальными ссылками на владельцев.

---

## Содержание

1. [Пятиуровневый коммуникационный стек](#1-пятиуровневый-коммуникационный-стек)
2. [Пятифазный lifecycle межлогосного взаимодействия](#2-пятифазный-lifecycle-межлогосного-взаимодействия)
3. [Язык намерений: FIPA ACL, BDI и ABC](#3-язык-намерений-fipa-acl-bdi-и-abc)
4. [Семантическая совместимость и multi-hop context](#4-семантическая-совместимость-и-multi-hop-context)
5. [Security overlay и операционные границы](#5-security-overlay-и-операционные-границы)

---

## 1. Пятиуровневый коммуникационный стек

**[C1][C2][C16]** N4Y.AI не может просто "принять один стандарт" для межагентной коммуникации: retained evidence показывает, что discovery, trust, semantic alignment, routing и execution находятся в разных source families. Поэтому канонический ответ D02 задаётся как пятиуровневый синтез `Semantic / Protocol / Routing / Identity / Discovery`. `A2A v1.0.0` (март 2026) заменяет прежний baseline `v0.3.0` как execution-layer reference, но не закрывает semantic layer и не делает cross-ecosystem interoperability автоматической. [E1][E34][E35][E36]

| Уровень | Что отвечает на уровне смысла | N4Y.AI-роль | Основные building blocks | Ownership |
|---------|-------------------------------|-------------|--------------------------|-----------|
| **Semantic** | Что означает сообщение и какое обязательство оно создаёт | Performative semantics, intention model, vocabulary discipline, semantic mismatch handling | `FIPA ACL`, hybrid BDI/speech-act semantics, `ABC`, `Verifiable Semantics` | **FULL в D02** для performatives и semantic composition |
| **Protocol** | Как оформляется межагентная сессия | Negotiation vocabulary + task/session framing | `A2A v1.0`, `DIDComm v2.1`, comparative `XAP` lifecycle primitives | `A2A` полностью в `→ L2/D04 §1-§2`; `DIDComm` полностью в `→ L1/D05 §4.2` |
| **Routing** | Как сообщения проходят через fabric и стриминг | Delivery, fan-out, pub/sub, gatewaying, trace carriage | `NATS`, `SLIM`, Agent Gateway, gRPC/HTTP2 streaming | Полностью в `→ L2/D04 §1-§3` |
| **Identity** | Кто именно говорит и чем подтверждает права | DID/VC-backed trust establishment и capability proof | DID resolution, VC verification, signed/encrypted envelopes | Технические детали вне D02; здесь только проекция trust role |
| **Discovery** | Кого вообще можно вызвать и по каким capability filters | Initial candidate set и entry metadata | `Agent Card`, ADS, Almanac-style registries | Полностью в `→ L2/D06 §4` |

**[C3]** Роль `XAP` в этом стеке не в замене `A2A`, а в том, что он даёт связную vocabulary для целого lifecycle и шесть primitive objects: `AgentManifest`, `AgentIdentity`, `NegotiationContract`, `SettlementIntent`, `ExecutionReceipt`, `VerityReceipt`. Эти примитивы хорошо ложатся на N4Y-модель из пяти фаз с дополнительным audit хвостом. [E6]

**[C16]** Важное ограничение: conformant `A2A` execution не делает heterogeneous ecosystems взаимозаменяемыми. Сравнительные источники по `Fetch.ai`, `SLIM/AGNTCY` и `OASF` показывают, что для cross-sandbox и cross-ecosystem flows всё равно нужен adapter/bridge layer, иначе capability metadata, schema digests и semantic assumptions расходятся. [E34][E35][E36]

---

## 2. Пятифазный lifecycle межлогосного взаимодействия

**[C3][C17]** Для ответа на ключевой вопрос достаточно не "общего communication loop", а полного межлогосного lifecycle: `Discovery -> Trust Establishment -> Negotiation -> A2A Execution -> Settlement`. Этот lifecycle собирается из нескольких стандартов и поэтому является N4Y-синтезом, а не копией одного spec. Settlement не может считаться "неявным завершением task": retained evidence требует явных settlement states, verification artifacts и reconciliation hooks. [E1][E6][E7][E8][E9][E10]

| Фаза | Семантическая цель | Основные артефакты/протоколы | Что важно для D02 | Ownership граница |
|------|--------------------|------------------------------|-------------------|-------------------|
| **1. Discovery** | Найти подходящего контрагента и сопоставить capability claims | `Agent Card`, ADS query, capability pre-screening | Discovery должен приводить не к "любому endpoint", а к candidate set с пригодной семантикой capability disclosure | Полное описание в `→ L2/D06 §4`; здесь важен смысл перехода в диалог |
| **2. Trust Establishment** | Подтвердить identity, channel authenticity и credential context | `DIDComm` modes: `anoncrypt`, `authcrypt`, signed messages | Для D02 это фаза установления интерпретируемого и доверенного диалога перед переговорами | Полное описание в `→ L1/D05 §4.2` |
| **3. Negotiation** | Согласовать условия, обязанности и критерии accept/reject | `FIPA CNP`, ACNBP binding steps, capability attestation | Здесь D02 владеет performative semantics `cfp/propose/accept-proposal/reject-proposal` | CNP как execution-coordination protocol полностью в `→ L2/D06 §3` |
| **4. A2A Execution** | Выполнить согласованную задачу как stateful inter-agent collaboration | `A2A Task`, `Messages`, `Artifacts`, `sessionId`, task states | D02 использует execution state machine как фазу после negotiation | Полная transport/task mechanics в `→ L2/D04 §1-§2` |
| **5. Settlement** | Превратить "completed" в проверенный экономический и нормативный исход | `SettlementIntent`, AP2 states, VCAP verification, escrow/reconciliation | D02 фиксирует смысл перехода от завершения к расчёту и фиксации результата | Полное описание в `→ L3/D03 §1-§2` |

**[C2]** Для execution baseline теперь следует использовать `A2A v1.0.0` марта 2026, а не прежний `v0.3.0`: retained evidence указывает на `Task` lifecycle states, `JSON-RPC 2.0 over HTTP(S)`, различение `Messages` и `Artifacts`, discovery via `/.well-known/agent.json` и multi-turn threading через `sessionId`. [E1]

**[C3]** `XAP` полезен как comparative lifecycle check: `DISCOVER -> VERIFY -> NEGOTIATE -> EXECUTE -> SETTLE -> AUDIT` и шесть primitive objects укладываются в N4Y-пятифазную модель так, что `VERIFY` совпадает с trust establishment, а `AUDIT` трактуется как post-settlement observability хвост, а не как отдельная обязательная фаза ключевого ответа. [E6]

**[C4]** Negotiation phase должна включать не только транспорт сообщений, но и disclosure capability, credential proof, uncertainty handling, multi-objective trade-offs и explainability/escalation. Это значит, что `cfp -> propose -> accept-proposal` в N4Y не является "голым auction exchange": речь идёт о сообщениях, которые уже содержат условия доверия и ограничения исполнения. [E2][E7]

**[C6][C7]** Runtime obligations не заканчиваются accept/reject. Для long-running tasks N4Y нужны progress/feedback signals и contract-level guardrails: phase 3 порождает negotiated commitments, а phase 4 исполняет их под наблюдением `ABC`-инвариантов и recovery rules. Полная формализация `Task Contract` и `SLA` принадлежит `→ L3/D04 §1-§4`; здесь важно только то, что после negotiation возникают интерпретируемые взаимные обязательства. [E4][E5]

---

## 3. Язык намерений: FIPA ACL, BDI и ABC

**[C5][C13]** D02 является canonical owner для `FIPA ACL` semantics: именно здесь определяется, какой illocutionary force имеет сообщение и как Логосы интерпретируют намерение друг друга. Retained evidence подтверждает, что для N4Y нужен не чисто менталистский и не чисто institutional подход, а hybrid model: `BDI` описывает внутреннее состояние агента, speech-act semantics описывает общественную силу сообщения, institutional semantics описывает создаваемые обязательства, а probabilistic intention inference помогает при неполной наблюдаемости. [E3][E20][E21][E22][E23][E24]

### 3.1 Классические performatives

**[C5]** Канонический классический набор состоит из 22 performatives, а не из 26: `accept-proposal`, `agree`, `cancel`, `cfp`, `confirm`, `disconfirm`, `failure`, `inform`, `inform-if`, `inform-ref`, `not-understood`, `propagate`, `propose`, `proxy`, `query-if`, `query-ref`, `refuse`, `reject-proposal`, `request`, `request-when`, `request-whenever`, `subscribe`. Для D02 особенно важны `cfp`, `propose`, `accept-proposal`, `reject-proposal`, `inform`, `request`, поскольку они формируют negotiate-and-commit pathway между discovery и execution. [E3][E21][E23]

`Call for Proposals` как performative остаётся owner-content D02: здесь фиксируется смысл запроса, минимальный состав proposal fields и правила интерпретации ответа контрагентом. Числовые scoring weights и winner-selection logic этому разделу не принадлежат; они описываются как market owner-content в `→ L3_D06 §4.1`, а D02 фиксирует только то, что `cfp` payload должен нести интерпретируемые поля цены, capability/SLA, reputation reference и latency metadata.

### 3.2 N4Y.AI extensions для long-running interaction

**[C6]** Современная MAS evidence оправдывает ограниченное расширение классического набора для long-running и partially observable tasks. N4Y.AI включает как минимум три расширения:

| Расширение | Роль в N4Y.AI | Почему нужно |
|------------|---------------|--------------|
| **`request-feedback`** | Запрос feedback по качеству предложения, промежуточного результата или semantic fit | long-running negotiations и peer review не сводятся к binary accept/reject |
| **`mission-status`** | Поток статусов и progress-сигналов в длительных миссиях | нужен для observability и explicit mid-task coordination |
| **`escalation`** | Сигнал о выходе за policy/authority/resource boundary | нужен для explainability, guardrails и HITL/escalation paths |

Эти расширения не заменяют `A2A Task` states, а добавляют illocutionary precision поверх transport/task model. [E4]

### 3.3 Intention model и контрактное ограничение поведения

**[C13]** Нормативная позиция D02: intention representation должна быть hybrid:
- `BDI` описывает beliefs, desires и committed intentions агента;
- speech-act layer описывает иллокутивную силу сообщения;
- institutional layer описывает публичные обязательства и promises/declarations;
- probabilistic intention inference обновляет гипотезу о намерении контрагента при неполной и шумной информации.

Такой hybrid лучше соответствует heterogenous LLM-agent ecosystems, чем попытка выбрать только один theory camp. [E19][E20][E21][E22][E23][E24]

**[C7]** `Agent Behavioral Contracts (ABC)` усиливают semantic layer формальной Design-by-Contract рамкой: `Preconditions`, `Invariants`, `Governance` и `Recovery` становятся runtime-enforceable constraints на поведение агента. Evidence поддерживает формализм `(p,delta,k)-satisfaction` для вероятностного соблюдения обязательств и `Drift Bounds Theorem`: при `gamma > alpha` отклонение ограничивается `D* = alpha / gamma`. Для D02 это важно потому, что performative без bounded compliance остаётся слишком слабым для открытых multi-agent chains. [E5]

**[C5][C7]** При этом `CNP` как coordination mechanism описывается полностью в `→ L2/D06 §3`, а `Task Contract` и `SLA` описываются полностью в `→ L3/D04 §1-§4`. Здесь: `cfp`, `propose`, `accept-proposal` и `reject-proposal` задают meaning of commitment; там: commitment превращается в executable coordination и contractual form.

---

## 4. Семантическая совместимость и multi-hop context

**[C11][C12][C14][C15][C16]** Полная shared ontology между всеми sandbox-ами и внешними ecosystem-ами не подтверждается evidence и не должна предполагаться по умолчанию. Жизнеспособнее другой путь: ограниченный certified core vocabulary, recertification при drift, ontology matching/negotiation и adapter layer для гетерогенных ecosystems. При этом у N4Y пока нет собственного завершённого ES-capability ontology или explicit versioning protocol, так что semantic layer уже достаточно обоснован для draft, но ещё не полностью закрыт для finalization. [E14][E15][E16][E17][E18][E36]

| Проблема | Что показывает evidence | Нормативный вывод для N4Y.AI |
|----------|-------------------------|-------------------------------|
| **Vocabulary mismatch** | `Verifiable Semantics` снижает disagreement на `51-96%` [E14][E15] и удерживает его около `~2%` при divergence через certified terms и recertification | Нужен certified core vocabulary вместо предположения о perfect shared language |
| **Semantic drift / ontology evolution** | `Semantic Fusion` и recertification подходят для реактивной корректировки, но не дают proactive semver-style protocol | Нужен governance/versioning gap; пока это open item |
| **Capability ontology gap** | Есть blocks для capability attestation и schema building, но нет прямого retained source для N4Y-specific ES ontology/ESCO alignment | `ES` semantics остаётся partially closed и требует отдельной design work |
| **Cross-ecosystem interoperability** | `A2A`, `Fetch.ai`, `SLIM/AGNTCY`, `OASF` показывают разные metadata and schema assumptions | Adapter/bridge layer обязателен; simple A2A conformance недостаточна |

**[C14]** Multi-hop context propagation (`A -> B -> C -> D`) считается по evidence substantially closed: базовой рамкой служат `W3C Trace Context` (`traceparent`, `tracestate`) и `OpenTelemetry Context/Baggage`, а практический framework evidence показывает, что такие headers реально переносятся через remote agent calls. Для D02 это означает, что межагентный смысл должен переноситься не только через payload, но и через causal context. [E26][E27][E28][E30]

**[C14][C15]** Практический минимум для N4Y semantic context envelope выглядит так: causal trace (`traceparent`), vendor-specific route state (`tracestate`), строго ограниченный `baggage` для переносимых key-value hints и session/thread correlation из task layer. При этом `baggage` нельзя считать безопасным контейнером для чувствительных данных, потому что retained evidence прямо предупреждает о его видимости в заголовках на недоверенных сетях. [E1][E26][E27][E28]

**[C11][C15]** Для оценки semantic interoperability evidence предлагает не один "магический KPI", а набор proxied metrics: disagreement rate, intention-model quality, observability conventions, common-ground tracking и generalization across unseen teammates. Значит, D02 может закрыть architectural answer, но не должен притворяться, что unified metric suite уже стандартизована. [E15][E24][E29][E37][E38]

---

## 5. Security overlay и операционные границы

**[C8][C9][C10]** Base `A2A` не задаёт mandatory security baseline, поэтому N4Y обязан вводить protocol-level overlay сам: authenticated/encrypted trust establishment, replay defense, spoofing/downgrade resistance и explicit message integrity должны считаться обязательными частями межлогосной коммуникации, а не optional add-ons. При этом retained performance evidence покрывает crypto micro-benchmarks и DID-auth/TLS feasibility, но не даёт end-to-end latency benchmark для полной `A2A + DIDComm` session establishment. [E1][E10][E11][E12][E13][E33][E35]

| Угроза | Подтверждённая защита | Нормативный статус для N4Y.AI |
|--------|------------------------|-------------------------------|
| **MITM / tampering** | `DIDComm authcrypt/signed`, mTLS, sender verification | Mandatory during trust establishment; не оставлять на optional ecosystem profiles |
| **Replay** | nonce, timestamp, bounded replay window, sequence discipline | Mandatory for business messages и trust handshakes |
| **Injection / spoofing** | signed messages, capability proof, behavioral verification | Mandatory on protocol boundary; payload не должен интерпретироваться без verification |
| **Downgrade / discovery spoofing** | protocol pinning, signed discovery metadata, trusted agent card publication | Mandatory for discovery-to-session transition |

**[C10]** Ключевой architectural вывод: security должна навешиваться не "после A2A", а на semantic transition между discovery, trust, negotiation и execution. Иначе формально корректный transport может переносить неподтверждённые или replayed obligations. `DIDComm` описывается полностью в `→ L1/D05 §4.2`; здесь фиксируется только его роль как trust-establishment layer в общем межагентном lifecycle. [E10][E33]

**[C9]** `OI-045` остаётся открытым и должен быть прямо помечен как deferred-to-PoC: есть данные о микрооперациях подписи/верификации и о приемлемости DID-auth TLS при cache-hit, но отсутствуют e2e benchmarks для реалистичных N4Y scenarios с discovery, trust establishment, negotiation и multi-hop execution. [E11][E12][E13]

**[C6][C10]** Error handling не должен снова разрастаться в самостоятельный FULL-owned раздел D02. D02 владеет semantic boundaries отказов по фазам, но не universal execution recovery. Retry/backoff/circuit breaker/compensation patterns должны проектироваться из владельцев: `Saga Pattern` полностью в `→ L1/D04 §4.4`, execution fabric — в `→ L2/D04 §2`, settlement recovery — в `→ L3/D03 §1-§2`.

---

## Связь с другими измерениями

| Измерение | Связь |
|-----------|-------|
| **L1/D05 (Communication)** | `DIDComm` и agent-level protocol choice описываются полностью в `→ L1/D05 §2` и `→ L1/D05 §4.2`. В D02 они используются как trust/security projection внутри межлогосного lifecycle. |
| **L2/D06 (Coordination)** | `CNP` как coordination protocol описывается полностью в `→ L2/D06 §3`, `ADS` в `→ L2/D06 §4`, `CSB` в `→ L2/D06 §5`. D02 использует их только как execution-near phases и semantic transition points. |
| **L2/D04 (Communication Infrastructure)** | `A2A`, `SLIM`, `NATS`, gatewaying и routing fabric описываются полностью в `→ L2/D04 §1-§3`. D02 не повторяет инфраструктурную спецификацию, а фиксирует семантическую композицию поверх неё. |
| **L3/D01 (Network Structure)** | Топология, sandbox-as-subnetwork, P2P handshake и роли узлов (Initiator/Coordinator/Broker) описаны в `→ L3/D01 §2`, `→ L3/D01 §3` и `→ L3/D01 §5`. D02 опирается на них, когда говорит о том, кто вообще может вступить в межлогосный диалог и как multi-hop flows проходят через сеть. |
| **L3/D03 (Value Exchange)** | Settlement, escrow patterns и payment state machines описываются полностью в `→ L3/D03 §1-§2`. В D02 settlement фигурирует только как последняя фаза semantic lifecycle после execution. |
| **L3/D04 (Contracts)** | `Task Contract`, `SLA` и knowledge-related obligations описываются полностью в `→ L3/D04 §1`, `→ L3/D04 §2` и `→ L3/D04 §4`. D02 фиксирует только момент, когда performative commitment должен перейти в formal contractual form. |

---

## Открытые вопросы

### Требуют решения (Critical / High)

1. **[High] OI-028: Cross-ecosystem semantic interoperability остаётся adapter-dependent.** Даже после перехода на `A2A v1.0` retained evidence не показывает end-to-end стандартизации capability metadata, schema digests и semantic negotiation across `A2A`, `Fetch.ai`, `SLIM/AGNTCY` и аналогичные ecosystems.  
> **Open.** Нужен explicit adapter policy: какие поля считаются canonical core, как выполняется schema translation, где хранится recertification state.

### Требуют исследования (Medium)

2. **[Medium] OI-045: end-to-end latency для `A2A + DIDComm` всё ещё не измерена на realistic N4Y workload.**  
> **Deferred:** требуется PoC с полной цепочкой `discovery -> trust -> negotiation -> execution`, а не только micro-benchmarks криптоопераций.

3. **[Medium] G-04/G-12: у N4Y пока нет завершённой ES-capability ontology и proactive versioning protocol.**  
> **Open.** Есть building blocks для attestation, ontology matching и recertification, но нет каноничного semver-like механизма эволюции vocabulary.
