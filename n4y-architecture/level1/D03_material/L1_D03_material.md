# УРОВЕНЬ 1 / ИЗМЕРЕНИЕ 03: МАТЕРИАЛЬНОЕ

> **Уровень:** L1 — Агент (свойства Логоса)  
> **Измерение:** D03 — Material  
> **Версия:** 4.0  
> **Статус:** Финал v4.0 | Дата: 02.04.2026  
> **Зависимости:** L1/D01, L1/D02, L2/D03, L1/D08

---

## Ключевой вопрос

**Какими вычислительными и техническими ресурсами ты располагаешь и как ты получаешь доступ к ним?**

---

## Аннотация

Прямой ответ на вопрос измерения таков: Логос располагает не одним "облаком", а управляемым набором из четырёх классов ресурсов `Compute / Storage / Network / Tooling`, получаемых через гибридный `Resource Marketplace` и отбираемых локальным `Resource Router` по цене, latency, privacy и SLA. Для premium и SLA-критичных задач используются централизованные или premium-провайдеры, для cost-sensitive и elastic workloads - децентрализованные рынки GPU, а для постоянных артефактов и RAG применяется температурно-разделённый storage stack. [C1][C6][C8][C10]

В сравнении с `[PREVIOUS VERSION]` документ сохраняет ядро legacy-логики — гибридную модель провайдеров, `Resource Router`, `IPFS + Arweave`, `TEE` и multi-provider failover — но перестраивает материал под новый шаблон и ownership-режим. Все claim-теги `[C{N}]` отсылают к `evidence/normalized-corpus.md`. DID-аутентификация остаётся проекцией идентификационного слоя `→ L1/D01 §1, §6`, смысл `ESq ≥ ESc` остаётся проекцией компетентностного слоя `→ L1/D02 §5.5`, бюджетный контроль и `Dual Constraint` остаются проекцией экономического слоя `→ L1/D08 §2, §6`, а технические детали `TEE`, smart-contract infra и storage implementation принадлежат `→ L2/D03 §3-§6`. [C9][C10][C15][C18]

---

## Содержание

1. [Типология ресурсов Логоса](#1-типология-ресурсов-логоса)
2. [Гибридная модель поставщиков и tier-архитектура](#2-гибридная-модель-поставщиков-и-tier-архитектура)
3. [Resource Router и механизм доступа](#3-resource-router-и-механизм-доступа)
4. [Storage layer](#4-storage-layer)
5. [Верификация compute и anti-cheating](#5-верификация-compute-и-anti-cheating)
6. [Tooling, внешние API и quota enforcement](#6-tooling-внешние-api-и-quota-enforcement)
7. [Отказоустойчивость, наблюдаемость и SLA](#7-отказоустойчивость-наблюдаемость-и-sla)

---

## 1. Типология ресурсов Логоса

### 1.1 Четыре материальных класса

Стратегия напрямую фиксирует `LLM-мощности`, аренду `GPU/CPU` и `внешние сервисы` как базовые ресурсные категории `[стратегия: вектор]`. В v4.0 это разворачивается в полную 4-компонентную типологию: `Compute`, `Storage`, `Network` и `Tooling`, потому что именно так Логос реально потребляет материальный слой в runtime, а не только на уровне закупки железа. [C1][C5][C13]


| Класс       | Что включает                                                                             | Зачем нужен Логосу                                                | Канонический статус                                                                          |
| ----------- | ---------------------------------------------------------------------------------------- | ----------------------------------------------------------------- | -------------------------------------------------------------------------------------------- |
| **Compute** | LLM inference, GPU/CPU-кластеры, burst compute, специализированные accelerator workloads | Выполнение reasoning, tool use, batch jobs, fine-tuned pipelines  | FULL в D03 как ответ на вопрос о доступных ресурсах; low-level infra detail → `L2/D03 §1-§3` |
| **Storage** | Hot retrieval, warm distribution, permanent records, on-chain anchors                    | Память, RAG, traces, audit artifacts, переносимость данных        | FULL в D03 как агентный слой доступа; storage internals → `L2/D03 §4`                        |
| **Network** | API endpoints, provider endpoints, gateways, bandwidth, egress-sensitive data paths      | Доступ к внешним системам и перенос вычислений между провайдерами | FULL в D03 как слой доступа и portability cost [C8]                                          |
| **Tooling** | MCP-based tools, external APIs, provider-side platform services, monetized endpoints     | Инструменты действия Логоса во внешнем мире                       | FULL в D03 как practical access layer [C13][C14][C15]                                        |


### 1.2 Что считается ресурсом в границах D03

> **Допущение [A35]:** Resource access в N4Y строится вокруг tier-aware hybrid provider model (`Premium / Open / Specialized`), поэтому price disclosure для `LLM` не может быть one-size-fits-all.

В рамках L1/D03 ресурсом считается не только "железо", но любой технический контур, без которого Логос не может выполнить задачу: модель, GPU-час, vector index, bandwidth, credentialed API и платежно-ограниченный tool endpoint. Такое расширение нужно, потому что архитектурно вопрос измерения касается не инвентаризации серверов, а управляемого доступа к вычислительной и технической среде. [C13][C15][C17]

Для `LLM`-ресурсов quoted price должен раскрываться component-wise. Базовый owner-side breakdown выглядит так: `60-70%` = compute floor (GPU/TPU time, batching efficiency, context length, quantization profile), `15-25%` = inference / serving overhead (routing, tokenizer / gateway path, caching, observability, provisioned throughput), `5-15%` = provider margin за SLA, reserved capacity и support. Поверх этого итоговый visible quote может включать platform fee и network / egress / on-chain overhead, если workload пересекает провайдерские границы или требует settlement trace. [C13][C15][C16][C17][A35]

Tier variance должна быть explicit: `Premium` чаще несёт более высокий margin и lower-variance SLA cost; `Open` может давать более низкий compute floor, но wider variance по throughput и portability cost; `Specialized` добавляет premium за attested / domain-specific / privacy-sensitive execution. Иначе одна усреднённая цена скрывает, какой именно provider profile покупается и почему quoted rate отличается между sandbox- и workload-типами. [C14][C16][C17][A35]

При этом D03 не должен присваивать себе владельцев соседних концептов. `DID` и runtime identity описываются полностью в `→ L1/D01 §1` и `→ L1/D01 §6`; здесь они фигурируют только как prerequisite доступа к провайдеру. `ESq/ESc` описываются полностью в `→ L1/D02 §1.3` и `→ L1/D02 §5.5`; здесь они фигурируют только как quota gate перед получением ресурса. `Budget Controller` описывается полностью в `→ L1/D08 §2`; здесь он выступает как бюджетный контур, решающий, может ли Логос оплатить выбранный ресурс. [C15][C17]

### 1.3 Практический ответ уровня L1

Ответ L1/D03 therefore звучит не как "у Логоса есть свой дата-центр", а как "у Логоса есть доступ к нескольким классам внешних и внутренних ресурсов через маршрутизируемый рынок, governed by identity, quota and budget". Именно гибридность, а не ownership hardware, является главным свойством материального слоя Логоса. [C1][C10][C18]

---

## 2. Гибридная модель поставщиков и tier-архитектура

### 2.1 Почему не один провайдер

Нормализованный корпус достаточно однозначен: для Логоса не подходит neither purely centralized, nor purely decentralized stack. Централизованные облака остаются релевантными для premium и SLA-critical workloads, а децентрализованные GPU-рынки дают дешевизну, эластичность и portability, уже доступные в 2025-2026, а не только обещанные roadmap'ом. [C1][C2][C3][C4]

Это соответствует legacy-идее `progressive decentralization` `[стратегия: вектор]`, но теперь фиксируется в более строгой форме: децентрализация не отменяет premium tier, а tiering не отменяет необходимость market-based failover. [C1][C10]

### 2.2 Три провайдерских tier

> **Mapping note (OI-094):** L1/D03 использует policy-ориентированную таксономию `Premium / Open / Specialized`. L2/D03 использует инфраструктурную таксономию `Tier 1 Premium / Tier 2 Standard / Tier 3 Edge`. Грубое соответствие: `Premium ≈ Tier 1`, `Open ≈ Tier 2/3`, `Specialized` — cross-cutting (может попадать в любой Tier в зависимости от SLA workload). Канонические SLA-определения по тирам → L2/D03 §1.


| Tier            | Профиль                                                                  | Типичные провайдеры                                                                                                                               | Когда выбирается                                                         | Ограничения                                                                       |
| --------------- | ------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------ | --------------------------------------------------------------------------------- |
| **Premium**     | Максимально предсказуемый SLA, управляемая latency, enterprise support   | Centralized clouds, managed premium AI endpoints, model-serving vendors; для market-side precedent также подходит Render premium-style AI serving | Критичные миссии, HITL-sensitive tasks, дорогие последствия ошибки       | Наиболее высокая цена и vendor dependence [C1][C4]                                |
| **Open**        | Дешёвый или средний compute, высокая эластичность, marketplace economics | Akash, io.net, open-model hosting, decentralized GPU aggregation                                                                                  | Cost-sensitive inference, batch, elastic scaling, non-critical workloads | Разброс качества и необходимость сильнее проверять provider quality [C2][C3][C10] |
| **Specialized** | Нишевые capability- или hardware-specific предложения                    | Render AI-market supply, TEE-capable providers, tuned GPU clusters, domain-specific tools                                                         | Особые требования по модели, privacy, benchmarking или географии         | Могут быть vendor-specific и менее ликвидны [C4][C9][C12]                         |


### 2.3 Роль Akash, io.net и Render

`Akash` выступает зрелым прецедентом дешёвого decentralized compute: у него live `Burn-Mint Equilibrium`, live WASM smart contracts, заметно более низкая marketplace-цена GPU и no-egress positioning, что делает его сильным кандидатом для `Open` tier. При этом конкретные цены надо трактовать как рыночный диапазон, а не как инвариантную константу. [C2][C8]

`io.net` важен не только как "ещё один GPU-провайдер", а как precedent aggregated supply: verified GPU inventory, active marketplace capacity, agent-side provisioning и developer spending controls. Для D03 это особенно важно, потому что `Resource Router` должен работать не только с одиночным облаком, а с heterogeneous capacity pool. [C3][C10][C15]

`Render` закрывает третий archetype: AI-oriented model-serving supply с usage-linked burn-and-mint economics. Он полезен не как общий ответ на все случаи, а как premium/alternative marketplace precedent для workloads, где Логосу нужен не просто дешёвый GPU, а более готовая AI-serving среда. [C4]

### 2.4 Что это значит для Логоса

На уровне Логоса tier-архитектура должна интерпретироваться так:

1. Critical path задачи сначала ищет `Premium` или trusted `Specialized` capacity.
2. Cost-sensitive or batch path сначала ищет `Open` capacity.
3. Если workload требует confidentiality or attestation, router поднимает weight на privacy / verification и сузит пул до подходящих провайдеров. [C9][C10][C12]

Именно поэтому материальный слой должен хранить не один "основной провайдер", а policy профили выбора. Конкретный провайдер - это outcome маршрутизации, а не hardcoded зависимость. [C10][C18]

---

## 3. Resource Router и механизм доступа (проекция)

Логос получает доступ к ресурсам через Resource Router среды: запрашивает ресурс (тип, объём, SLA), Router выбирает провайдера по критериям price/latency/privacy/SLA, открывает AGC-эскроу. Агентная цепочка доступа: `DID → ESq gate → budget gate (→ L1/D08 §2) → provider routing`. Полная механика маршрутизации и enforcement → `L2/D03 §6`.

---

## 4. Storage layer

### 4.1 Температурно-разделённая модель

Для L1/D03 storage должен быть описан не как один backend, а как слой с разной "температурой" данных. Корпус поддерживает следующую схему: hot retrieval хранится локально и максимально близко к inference path; warm distribution уходит в `IPFS / Filecoin` с managed gateways и CDN; permanent and immutable records - в `Arweave`; on-chain остаются только критические anchors и hashes. [C6][C7]


| Tier          | Что хранится                                                                  | Типичная технология                                  | Почему здесь                                                                          |
| ------------- | ----------------------------------------------------------------------------- | ---------------------------------------------------- | ------------------------------------------------------------------------------------- |
| **Hot**       | Активный retrieval index, in-flight context, latency-sensitive fragments      | Local vector DB / local cache рядом с execution path | Для RAG bottleneck - network proximity, а не просто "децентрализация" [C7]            |
| **Warm**      | Распространяемые knowledge artifacts, shared corpora, less-frequent retrieval | `IPFS / Filecoin` + managed gateway/CDN              | Даёт portability и content-addressability без требования permanent retention [C6][C7] |
| **Permanent** | Audit logs, immutable records, high-value archives                            | `Arweave`                                            | Подходит для неизменяемых и долгоживущих артефактов [C6]                              |
| **Anchor**    | Хэши, pointers, минимальные доказательные следы                               | On-chain anchors                                     | On-chain storage должен быть минимальным, а не общим data lake [C6]                   |


### 4.2 Почему hot retrieval не должен жить в public IPFS

Материал по latency достаточно чёткий: single-digit millisecond retrieval достигается локальным vector DB, тогда как dedicated IPFS gateway/CDN даёт десятки-сотни миллисекунд, а публичная propagation может занимать намного дольше. Поэтому hot RAG и active episodic access нельзя проектировать как "чтение прямо из public IPFS". [C6][C7]

Практический вывод для D03 простой: `IPFS` - это warm distribution layer, а не замена локальному retrieval engine. Если Логосу нужна быстрая память для текущей миссии, она должна быть размещена ближе к inference runtime, а IPFS используется как portable content rail и слой распространения. [C6][C7]

### 4.3 Egress и portability как material cost

`G13` показывает, что egress cost - это не вторичная бухгалтерия, а first-order architecture cost для переноса checkpoints, синхронизации RAG и смены провайдера. Это особенно важно для multi-provider design: если провайдер дорог в egress, router фактически получает hidden lock-in, даже если номинальная цена GPU низкая. [C8]

Следовательно, storage layer в D03 нужно оценивать не только по цене хранения, но и по:

1. цене выхода данных,
2. latency retrieval,
3. переносимости между провайдерами,
4. устойчивости к sync drift между source data и embeddings. [C7][C8]

### 4.4 OI-067 как design note, а не пустой gap

`OI-067` после evidence narrowed до конкретной архитектурной развилки: как именно разделить `Episodic Memory`, `Pattern Library` и RAG-контент между hot local vector storage, warm decentralized distribution и permanent archive. Это уже не "storage не определён", а "storage split needs productized retention policy". [C6][C7][C18]

В этом документе корректно зафиксировать следующий baseline:

- active retrieval и low-latency search - hot/local,
- exchangeable corpora и portable artifacts - warm/IPFS,
- immutable legal/audit traces - permanent/Arweave,
- критические доказательные anchors - on-chain only. [C6][C7]

Точная длительность retention, sync cadence и ownership отдельных классов памяти остаются open design work и выносятся в хвост документа как отдельный вопрос. [C18]

### 4.5 Граница с L2/D03

`Storage Layer` описывается полностью в `→ L2/D03 §4`. Здесь, в L1/D03, нужен только agent-side ответ: какие storage classes доступны Логосу и как он выбирает между ними по latency, portability и permanence. Это делает D03 material-interface документом, а не infra-implementation спецификацией. [C6][C7]

---

## 5. Верификация compute и anti-cheating

### 5.1 Почему compute нужно верифицировать

Для material-layer недостаточно знать, что провайдер "обещал GPU". Логосу нужен хотя бы минимально проверяемый контур качества: реальное железо, реальное исполнение inference и механизм снижения cheating risk при биллинге. Именно поэтому compute verification входит в D03 как practical access requirement. [C9][C11][C12]

### 5.2 Трёхслойный trust pattern

Корпус поддерживает комбинированную, а не единую модель проверки:


| Слой                                 | Прецедент                                                                            | Что проверяется                                                     | Ограничение                                                                |
| ------------------------------------ | ------------------------------------------------------------------------------------ | ------------------------------------------------------------------- | -------------------------------------------------------------------------- |
| **Hardware / provider verification** | `TEE Attestation`, Akash attestation pipeline, io.net zk-based hardware verification | Подлинность среды и заявленного hardware profile                    | Частично roadmap-dependent для части провайдеров [C9][C12]                 |
| **Inference verification**           | `VeriLLM`                                                                            | Что inference действительно был выполнен, а не симулирован          | Сильная академическая база, но production deployment пока не доказан [C11] |
| **Useful-work validation**           | `Cortensor` PoI + PoUW, reputation-linked validation                                 | Что вычисление было не только исполнено, но и содержательно полезно | Часть экосистемы остаётся в `testnet/mainnet-lite roadmap` режиме [C12]    |


### 5.3 VeriLLM как сильнейший anti-cheating precedent

`VeriLLM` особенно важен для D03, потому что показывает: verifiable LLM inference возможен без полного on-chain recompute модели. Архитектурно это даёт Логосу право требовать verification-grade providers там, где биллинг или dispute sensitivity высоки. Но зрелость этого механизма следует описывать аккуратно: это сильное техническое направление, а не доказанный массовый production baseline. [C11]

### 5.4 Cortensor, zkTFLOPs и provider-side signals

`Cortensor` и аналогичные PoUW-подходы полезны как design pattern более широкого класса: провайдер/валидатор должен уметь показать не только факт выделения GPU, но и confidence в полезности выполненной работы. В этом же классе находится `io.net` hardware / zk-verification signal. Для L1/D03 этого достаточно, чтобы сформулировать policy: quality-sensitive workloads должны получать повышенный verification weight в router policy. [C12]

При этом важно не перепродать зрелость рынка. Для части таких систем корректна только формулировка `draft/testnet` или `roadmap-backed precedent`, а не "уже устоявшийся стандарт". Это особенно относится к experimental validation stacks и маршрутам, где Mainnet-Lite / testnet ещё не перешли в стабильный production режим. [C11][C12]

### 5.5 Что означает privacy в compute

`TEE` описывается полностью в `→ L2/D03 §3`, а в D03 должен фигурировать как provider-selection rule: если workload затрагивает чувствительные данные владельца, router обязан поднимать вес privacy и сужать пул до провайдеров, способных предъявить достаточную attestation/verification историю. То есть L1/D03 отвечает на вопрос "когда требовать confidential compute", а не "как устроен enclave". [C9]

### 5.6 Практический baseline

Для v4.0 baseline можно формулировать так:

1. Для дешёвых и low-risk workloads достаточно обычного provider reputation + usage traces.
2. Для дорогих или contestable workloads желательны inference verification patterns.
3. Для sensitive data workloads требуется privacy-aware provider selection с `TEE`/attestation signals.
4. Для high-value billing нужна возможность post-hoc dispute evidence, а не только счёт провайдера. [C9][C11][C12][C17]

---

## 6. Tooling, внешние API и quota enforcement

### 6.1 Tooling - это тоже material resource

В legacy-версии tooling mostly растворялся в "внешних сервисах". В v4.0 это фиксируется отдельно: tools и external APIs являются материальным ресурсом, потому что consume budget, latency, credentials and provider policy точно так же, как GPU или storage. D03 therefore должен описывать не только compute renting, но и tool access layer. [C13][C14][C15]

### 6.2 Discovery и governance tooling layer

Корпус показывает, что MCP Registry / Gateway patterns уже существуют как практический рынок discovery, auth controls и governance для tools. Это означает, что Логос может получать tools не как статически прошитый список интеграций, а как наблюдаемый и политически фильтруемый реестр доступных capability endpoints. [C13]

Для D03 отсюда следуют два вывода:

1. tooling access должен быть discoverable, а не hardcoded;
2. tool endpoint должен иметь policy envelope: auth, spend controls, observability и revocation path. [C13][C15]

### 6.3 Payment-gated tool access

`x402`, `xpay`, SettleGrid/Stripe-style metering и сходные precedents показывают, что per-call monetization и payment-gated API access уже можно считать concrete design pattern, а не гипотезой. Для Логоса это важно потому, что многие действия во внешнем мире будут выглядеть именно как tool invocation with payment policy, а не как long-running compute lease. [C14]

Здесь D03 не владеет полным payment protocol stack, но должен зафиксировать material implication: tool endpoint может быть доступен only if Логос может пройти auth, quota и payment checks в реальном времени. Иначе "наличие инструмента" в каталоге ничего не значит для реальной агентной способности действовать. [C14][C15]

### 6.4 Quota enforcement: от ESq к provider-specific limit

`D02` описывает смысл `ESq ≥ ESc`, но не описывает всю операционную механику runtime gating. В D03 это получает material projection: competence gate должен быть связан с provider-facing quota и budget envelope до вызова ресурса. То есть `ESq` не заменяет денежный лимит, а просто блокирует недопустимые по профилю задачи ещё до отправки запроса. [C15]

Практически chain выглядит так:

1. `ESq/ESc` отвечает на вопрос "разрешено ли";
2. `Budget Controller` отвечает на вопрос "доступно ли по бюджету";
3. tool/provider policy отвечает на вопрос "пропустит ли endpoint данный вызов";
4. usage ledger отвечает на вопрос "как это будет тарифицироваться и ограничиваться". [C14][C15]

### 6.5 Real-time spend governance

Корпус поддерживает модель quota enforcement как `real-time spend governance`, а не только request counting. Gateway economic firewalls, per-endpoint caps, escrow ceilings, hard tenant budgets и provider-side spending limits уже наблюдаются как precedents. Это важно для D03, потому что heterogeneous providers нельзя контролировать одним лишь счётчиком "количества запросов". [C15]

Следовательно, хороший material access profile должен учитывать:

- spending cap,
- per-tool / per-endpoint limits,
- burst control,
- duplicate protection,
- provider-native budget controls,
- возможность блокировать retries после error loops. [C14][C15]

### 6.6 Maturity caveat

Некоторые precedents из quota-enforcement слоя следует маркировать явно:

- `LEP100-3` - usable как design input, но `draft/early-standard`, не production canon;
- `BudgetGuard` - полезный precedent hard-budget gateway, но `early-stage`;
- часть x402 ecosystem tooling остаётся fragmented market, а не единым зрелым стеком. [C15]

Это не ослабляет архитектурный вывод. Напротив: вывод состоит в том, что pattern уже жив, но стандартизация ещё не закрыта, и потому D03 должен фиксировать capability boundary, а не один vendor-specific продукт. [C14][C15]

---

## 7. Отказоустойчивость, наблюдаемость и SLA

### 7.1 Multi-provider failover как обязательное свойство

Current evidence уже достаточен, чтобы считать multi-provider fallback обязательным требованием к `Resource Router`. Failover нужен не потому, что decentralization сама по себе "красивее", а потому, что ecosystems уже предоставляют discovery, observability и spend-control primitives, позволяющие реально переключаться между поставщиками. [C10][C18]

Это означает, что в material architecture Логоса должен существовать не просто список "backup провайдеров", а policy перехода между ними:

- когда деградация считается значимой,
- что происходит с in-flight state,
- какой провайдер считается safe fallback,
- как фиксируется стоимость переключения. [C10][C18]

### 7.2 Что должно наблюдаться

`OpenTelemetry gen_ai.`* conventions дают strongest current precedent для trace-derived SLA observability across providers. Для D03 это означает, что доступ к ресурсу считается полноценным только тогда, когда Логос может наблюдать latency, availability, token/cost signals и error budget через единый tracing слой. [C16]

Минимальный набор наблюдаемых метрик:


| Метрика                              | Зачем нужна                                                    |
| ------------------------------------ | -------------------------------------------------------------- |
| **Latency**                          | Решать, когда provider уже не годится для текущей миссии [C16] |
| **Availability / error rate**        | Понимать, является ли отказ локальным или системным [C16]      |
| **Token / usage counts**             | Соотносить compute и tooling usage с биллингом [C16]           |
| **Cost attribution**                 | Связывать routing choice и economic consequence [C16]          |
| **Provider-specific incident trace** | Разбирать постфактум споры и деградацию [C16][C17]             |


### 7.3 SLA enforcement (проекция)

Провайдеры ресурсов предоставляют SLA (availability, error rate, penalty support). SLA enforcement — паттерны штрафов, escrow-backed disputes, verify-then-pay → `L3/D04 §4`.

### 7.4 OI-068 design note

`OI-068` теперь можно формулировать точнее: нужен не "concept failover", а документированный switch policy для mid-mission routing. Решение должно определить:

1. timeout thresholds,
2. warm-standby / cold-standby режимы,
3. state handoff rules,
4. idempotency and duplicate-charge protection,
5. observability trigger, после которого switchover разрешён автоматически. [C10][C18]

### 7.5 Итоговая material posture

Материальный слой Логоса therefore считается зрелым тогда, когда он:

- умеет выбрать лучший ресурс, а не только "достать какой-то";
- может доказуемо наблюдать его качество;
- умеет пережить деградацию провайдера;
- не теряет budget governance при переключении;
- отделяет permanent, warm и hot storage semantics;
- не смешивает compute ownership, auth ownership и economic ownership в одну секцию. [C6][C10][C15][C16][C18]

---

## Связь с другими измерениями


| Измерение                      | Связь                                                                                                                                                                                                                      |
| ------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **L1/D01 (Идентификационное)** | DID, Binding VC и runtime identity нужны для доступа к providers и tooling gateways, но их полная модель принадлежит `→ L1/D01 §1` и `→ L1/D01 §6`.                                                                        |
| **L1/D02 (Компетентностное)**  | D02 определяет смысл `ESq`, `ESc` и competence-side gate; D03 превращает этот смысл в execution/resource gating policy. См. `→ L1/D02 §5.5`.                                                                               |
| **L1/D08 (Экономическое)**     | Budget Controller и `Dual Constraint` ограничивают фактическую доступность ресурсов, even when provider technically reachable. См. `→ L1/D08 §2` и `→ L1/D08 §6`.                                                          |
| **L2/D03 (Инфраструктура)**    | L1/D03 отвечает на вопрос "какими ресурсами и как пользуется Логос", а L2/D03 - "на какой инфраструктуре всё это построено". Полное описание `TEE`, storage internals и router specification находится в `→ L2/D03 §3-§6`. |
| **L1/D04 (Функциональное)**    | Оркестрация задач использует материальный слой как execution substrate: router choices, model calls и tools становятся частью runtime-пути исполнения.                                                                     |
| **L1/D05 (Коммуникационное)**  | Сетевые и tooling ресурсы materialize коммуникационный слой: endpoint latency, gateway auth и transport reliability напрямую ограничивают способность Логоса взаимодействовать.                                            |
| **L1/D09 (Репутационное)**     | Provider- и tool-choice могут downstream учитывать reputation signals, но сама модель репутации и score lifecycle D03 не принадлежат.                                                                                      |


---

## Открытые вопросы

### Требуют решения (Medium)

1. **[Medium] OI-067: финализировать production split между hot retrieval, warm distribution и permanent storage.** Evidence уже показывает, что local vector retrieval, `IPFS/Filecoin` и `Arweave` играют разные роли, но правила retention, sync и ownership для `Episodic Memory` и RAG ещё не зафиксированы окончательно. [C6][C7][C18]
  > **Open.** Для v4.0 зафиксирован только baseline: hot/local, warm/IPFS, permanent/Arweave, on-chain anchors only. Нужен отдельный design note по retention windows, sync cadence и legal-hold semantics.
2. **[Medium] OI-068: определить failover policy для `Resource Router` mid-mission.** Multi-provider fallback уже обязателен, но условия switchover и state handoff semantics не канонизированы. [C10][C18]
  > **Open.** Нужен design note с timeout thresholds, idempotency rules, duplicate-charge protection и критериями automatic vs HITL switchover.

### Требуют исследования (Medium)

1. **[Medium] G13 design note: как измерять sync drift и migration cost при переносе retrieval stack между провайдерами.** Egress и IPFS propagation уже признаны архитектурно значимыми, но не сведены в единый benchmarking profile для D03. [C7][C8][C18]
  > **Open.** Требуется benchmark pack для re-embedding cadence, provider switch cost, owned-gateway vs public-IPFS latency и checkpoint portability.
