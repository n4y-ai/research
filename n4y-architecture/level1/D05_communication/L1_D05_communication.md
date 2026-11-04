# УРОВЕНЬ 1 / ИЗМЕРЕНИЕ 05: КОММУНИКАЦИОННОЕ

> **Уровень:** L1 — Агент (свойства Логоса)  
> **Измерение:** D05 — Communication  
> **Версия:** 4.0  
> **Статус:** Финал v4.0 | Дата: 02.04.2026  
> **Зависимости:** L1/D01, L1/D04, L2/D04, L3/D02

---

## Ключевой вопрос

**Как ты взаимодействуешь с людьми, другими агентами и системами и какие протоколы при этом используешь?**

---

## Аннотация

Прямой ответ на вопрос измерения таков: Логос использует три разных контура коммуникации, потому что у них разные цели и разные требования к совместимости. Для взаимодействия с человеком базовым интерфейсом является `AG-UI`; для взаимодействия с другими агентами базовым межагентным протоколом является `A2A v1.0.0` как текущий L1 baseline (детали → `L2/D04 §1.2`) [A10][A11]; для доступа к инструментам и внешним системам базовым invocation layer является `MCP`; а для конфиденциального и DID-rooted обмена сообщениями поверх межагентного контура используется `DIDComm v2.1`. Production-сценарий допускает одновременную активацию всех трёх поверхностей. [C1][C2][C3][C6][C7][C10]

В сравнении с `drafts/v0_legacy.md` этот черновик сохраняет трёхтиповую модель коммуникации, но ужесточает ownership-дисциплину. `MCP` описывается только как проекция инструментального слоя `→ L1/D04 §3`; `Structured Handoff` описывается только как проекция intra-Logos orchestration `→ L1/D04 §4.3`; семантика `Performative` остаётся проекцией `→ L3/D02 §3`; а low-level delivery, gateway и масштабируемость A2A остаются проекцией `→ L2/D04 §1-§5`. [C2][C6][C10]

> «Общий язык, позволяющий агентам беспрепятственно обнаруживать возможности друг друга, вести переговоры и безопасно совершать транзакции.» `[стратегия: вектор]`

Для D05 этот стратегический тезис переводится в практический стек так: discovery и capability matching начинаются с `Agent Card`, переговоры и task lifecycle идут через `A2A`, безопасный обмен чувствительными payloads опирается на `DIDComm`, а tool access к внешним системам уходит в `MCP`. [C1][C4][C6][C7]

---

## 1. Трёхтиповая модель коммуникации

### 1.1 Базовое разделение типов

Коммуникационное измерение Логоса удобно нормализовать в три типа. Такое разделение не сводится к transport choice; оно отражает разные роли собеседника, разные ожидания к statefulness и разные trust assumptions. [C2][C3][C6]

| Тип | Участники | Главная цель | Канонический интерфейс | Что не надо смешивать |
|-----|-----------|--------------|------------------------|------------------------|
| **Тип 1** | человек ↔ Логос | постановка задачи, контроль, подтверждение, объяснение | `AG-UI` | не подменять human UI `A2A input-required` |
| **Тип 2** | Логос ↔ Логос | кооперация, делегирование, capability matching, task lifecycle | `A2A` | не сводить межагентное взаимодействие к tool call |
| **Тип 3** | Логос ↔ внешние системы | вызов tools, API, data sources, enterprise systems | `MCP` | не путать tool invocation с peer-agent collaboration |

`AG-UI`, `A2A` и `MCP` не конкурируют между собой. Они покрывают разные направления связи, и production-run может одновременно держать человеко-ориентированный stream, несколько A2A-задач и набор MCP-вызовов к внешним системам. [C2][C3][C6][C10]

### 1.2 Тип 1: человек ↔ Логос

Для человека коммуникация должна выглядеть как управляемый, наблюдаемый и объяснимый интерфейс, а не как сырой протокольный обмен. Поэтому базовым слоем является `AG-UI` с typed streaming events и state synchronization. Он даёт потоковый ответ, видимость tool calls и обновление состояния сессии без привязки к конкретному frontend stack. [C2][C3]

`A2A` может поднять событие `input-required`, но это только HITL-мост, а не полноценный UI-протокол. Человеческий интерфейс остаётся отдельной поверхностью: пользователь видит stream, получает state updates и при необходимости подтверждает действие через UI, а не через peer-agent envelope. [C3]

### 1.3 Тип 2: Логос ↔ Логос

Межагентное взаимодействие требует capability discovery, stateful task lifecycle и явной модели частей сообщения. Поэтому L1 baseline использует `A2A v1.0.0` как текущую межагентную норму, а transport/versioning mechanics остаются проекцией на владельца `→ L2/D04 §1.2`. [A10][A11][C1][C5]

Внутри одного Логоса execution-передача контекста не принадлежит D05 как отдельная FULL-спецификация. `Structured Handoff` описывается полностью в `→ L1/D04 §4.3`; в D05 он нужен только как граница: intra-Logos handoff не должен подменять собой inter-agent A2A и не должен путаться с discovery или trust establishment между независимыми агентами. [C2]

### 1.4 Тип 3: Логос ↔ внешние системы

Когда Логосу нужно вызвать инструмент, API, корпоративную систему или источник данных, граница проходит не по transport, а по роли второй стороны. Если вторая сторона не является peer-agent с собственным task lifecycle, а представляет tool/resource surface, используется `MCP`. Это остаётся проекцией `→ L1/D04 §3`, где описан канонический tool protocol и его security baseline. [C2][C6]

Практический вывод прост: "другой агент" и "внешний инструмент" нельзя маршрутизировать одинаково. В первом случае нужен capability-driven task exchange, во втором случае нужен bounded tool invocation с отдельными authz-проверками и runtime controls. [C2][C6][C10][C12]

---

## 2. Протокольный стек

### 2.1 Верхнеуровневая схема

L1/D05 владеет не транспортной инфраструктурой масштаба, а агентным выбором протокольных поверхностей. На этом уровне стек читается так: `AG-UI` отвечает за human-facing stream, `A2A v1.0.0` отвечает за peer-agent collaboration (детали → `L2/D04 §1.2`) [A10][A11], `MCP` отвечает за tool invocation, а `DIDComm v2.1` отвечает за confidential DID-rooted messaging там, где нужна защищённая доставка payloads поверх agent interaction. [C1][C2][C3][C6][C7]

```text
Человек
  ↕ AG-UI
Логос / Orchestrator
  ↔ A2A v1.0.0 ↔ Другой агент
  ↔ MCP ↔ Внешняя система / tool
  ↔ DIDComm ↔ Защищённый DID-rooted канал
```

### 2.2 `A2A v1.0.0` как L1 baseline (детали → `L2/D04 §1.2`)

В этом черновике `A2A v1.0.0` фиксируется как текущий L1 baseline: на уровне D05 этого достаточно, чтобы определить peer-agent surface и отделить его от `MCP` и `Structured Handoff`, а transport/versioning mechanics читаются как проекция `→ L2/D04 §1.2`. [A10][A11][C1]

Это особенно важно для границы с `MCP`: в production стек больше не выглядит как "выбрать один стандарт". Он выглядит как сочетание зрелого tool layer и растущего inter-agent layer, которые могут жить в одном gateway и в одном runtime, но с разными routing semantics. [C1][C2][C6][C10]

### 2.3 Границы между `A2A`, `MCP`, `DIDComm` и `AG-UI`

| Протокол | Роль в D05 | Что он делает | Чего он не делает |
|----------|------------|---------------|-------------------|
| **`AG-UI`** | human-agent surface | typed streaming events, state sync, human-visible execution | не заменяет agent collaboration |
| **`A2A`** | agent-agent surface | task lifecycle, message parts, capability exchange | не является tool protocol |
| **`MCP`** | agent-tool surface | invocation of tools/resources | не описывает peer-agent negotiation |
| **`DIDComm`** | security overlay | confidential and DID-rooted messaging, async relay support | не заменяет task model `A2A` |

Такое разбиение важно ещё и потому, что многие flows активируют все четыре слоя одновременно. Пользовательский вопрос приходит через `AG-UI`, planner зовёт внешний инструмент через `MCP`, делегирует подзадачу внешнему агенту через `A2A`, а чувствительный payload внутри межагентного обмена отправляет через `DIDComm`. [C2][C3][C6][C7][C10]

### 2.4 Граница с `MCP` и `Structured Handoff`

В `v0_legacy` эти границы были размыты. В `v1` они фиксируются жёстко:

- `MCP` описывается как проекция функционального слоя `→ L1/D04 §3`, а не как самостоятельный owned concept D05. [C6]
- `Structured Handoff` описывается как проекция orchestration-pattern слоя `→ L1/D04 §4.3`, а не как внешний peer-agent transport. [C2]
- `Performative` описывается как проекция semantic layer `→ L3/D02 §3`, а не как транспортная единица D05. [C2]

Это делает ответ D05 чище: измерение объясняет, какой communication surface включается для какого типа взаимодействия, но не переписывает владельцев соседних концептов. [C2][C6][C10]

---

## 3. Discovery и Agent Card

### 3.1 От well-known endpoint к capability matching

Discovery в D05 начинается не с "магического реестра всех агентов", а с машиночитаемого профиля агента. Нормализованный корпус фиксирует `Agent Card` как точку входа для capability discovery; официальный A2A baseline использует well-known publication point `/.well-known/agent.json`. В предыдущих черновиках проекта встречался shorthand `/.well-known/agent-card.json`; в этой версии он сохраняется только как legacy-указатель на тот же discovery intent, но L1 baseline нормализует именно `agent.json`. [C1][C4]

`Agent Card` нужен не для transport secrecy, а для ответа на вопрос "кто ты, что умеешь и где тебя вызывать". В `v1.0.0` он остаётся частью текущего baseline, а детали publication/integrity profile и transport/versioning читаются как проекция `→ L2/D04 §1.2`. [A10][A11][C1][C7]

### 3.2 Три уровня discovery

На L1 discovery удобно описывать как трёхуровневую цепочку:

1. `Agent Card` на well-known endpoint даёт первичную capability surface. [C4]
2. Registry layer расширяет поиск и сопоставление по capabilities; `Almanac` выступает подтверждённым precedent для registry-based discovery. [C4]
3. `Trust Registry` даёт trust-extending signals: attestations, provenance и проверяемость участника. [C4][C7]

Это не означает, что весь ADS fully defined уже на L1. Корпус прямо предупреждает: полная архитектура discovery service остаётся downstream design problem и не должна быть переописана здесь сверх того, что нужно для agent-level выбора. Подробная infrastructure topology относится к `→ L2/D04 §4`, а coordination semantics, matching и `cfp` handoff — к `→ L2/D06 §4`. [A8][A52]

### 3.3 Что именно читает Логос при выборе другого агента

С практической точки зрения Логос в discovery phase читает как минимум четыре класса сигналов:

| Сигнал | Зачем нужен | Источник |
|--------|-------------|----------|
| identity / endpoint | понять, куда и как обращаться | `Agent Card` |
| capabilities / skills | проверить semantic fit задачи | `Agent Card` + registry enrichment |
| integrity / publisher proof | снизить риск подмены карточки | signed `Agent Card` |
| trust / attestations | снизить риск работы с непроверенным агентом | `Trust Registry` / identity layer |

Такой профиль естественно стыкуется с `→ L1/D01 §1` и `§5`: идентичность, DID-rooted trust и registry-backed attestation не принадлежат D05 полностью, но именно D05 определяет, как эти сигналы участвуют в запуске межагентной коммуникации. [C4][C7]

### 3.4 Discovery как граница между L1 и L2

L1/D05 отвечает на вопрос, какие сигналы агенту нужны, чтобы заговорить с другим агентом. L2/D04 отвечает на вопрос, как эти сигналы доставляются при масштабе, кэшируются, маршрутизируются и живут в gateway/broker topology. Поэтому в D05 discovery нужно удерживать на уровне `Agent Card -> registry -> trust extension`, не проваливаясь в брокеры, шардинг и network topology. [C4][C10]

---

## 4. Security layer и авторизация

### 4.1 Почему `A2A` недостаточно для security baseline

`A2A` решает inter-agent lifecycle и структуру обмена, но не даёт сам по себе confidential transport для payloads. Поэтому L1 security baseline строится как layer split: `A2A` отвечает за task collaboration, а `DIDComm v2.1` отвечает за E2E encrypted DID-rooted messaging. Подписанная `Agent Card` усиливает identity/integrity profile, но не заменяет protected channel. [C7]

Базовая связка therefore выглядит так: peer discovery и collaboration идут через `A2A`, а payloads, которые требуют confidentiality или приватного DID-rooted обмена, должны уходить в `DIDComm`. Именно из-за этого `OI-045` остаётся открытым: архитектурная связка выглядит правдоподобной, но production-validated combined trust layer пока не показан. [C7][C8]

### 4.2 `DIDComm v2.1` как security overlay

Для L1 важны три свойства `DIDComm v2.1`:

- E2E encryption на DID-rooted ключах, включая X25519-based key agreement. [C7]
- async-friendly relay semantics, то есть защищённый обмен не зависит от одновременного онлайна обеих сторон. [C7][C9]
- отделение secure channel от business/task model, благодаря чему `DIDComm` не ломает A2A lifecycle, а оборачивает чувствительный обмен. [C7]

Это также держит границу с `→ L1/D01`: identity layer, DID documents и trust chains принадлежат идентификационному измерению, а D05 отвечает только за то, где этот слой подключается в коммуникационном контуре. [C7]

### 4.3 Action-level authorization

Корпус прямо показывает, что DID-идентичности и даже integrity-подписи недостаточно для решения вопроса "может ли агент выполнить именно это действие". Поэтому action-level authorization должен добавляться отдельным policy/capability layer поверх identity. На 2025-2026 evidence в L1 видны три жизнеспособных паттерна. [C12]

| Паттерн | L1-смысл | Пример класса решений |
|---------|----------|-----------------------|
| **centralized policy gate** | перед tool/action execution стоит deterministic gate | AgentLock-style policy enforcement |
| **capability-based authorization** | агент получает не общую роль, а узко-ослабленную capability | `OCap` / `OCapN` |
| **governance / policy toolkit** | policy and audit controls вынесены в отдельный control plane | toolkit-style governance enforcement |

Нормативный вывод для D05 такой: A2A/DID identity открывает разговор, но право совершить конкретное действие должно проверяться ещё раз на action boundary. Это особенно важно на границе D05↔D04, где коммуникация переходит в execution. [C6][C12]

### 4.4 Статус `OAP`

В исследовательской воронке проекта `OAP` фигурирует как интересный proposal-level вариант deterministic pre-action authorization, но текущий normalized baseline на него не опирается. Причина простая: для канонического L1-описания уже достаточно трёх подтверждённых классов решений из `C12`, а `OAP` пока разумнее держать как watch item, а не как обязательный слой. [C12]

### 4.5 Что именно остаётся открытым

Security layer в этом черновике не объявляется полностью закрытым. Закрыт базовый принцип layer split, но остаются открытыми как минимум два практических вопроса: production PoC для комбинированного `A2A + DIDComm` trust profile и convergence между identity proof, signed `Agent Card` и action-level authorization в одном interoperable flow. Именно поэтому `OI-045` уходит в хвост как medium unresolved item. [C8][C12]

---

## 5. Async/offline и fault-tolerance

### 5.1 Async как нормальный режим, а не исключение

L1 baseline не предполагает, что все участники всегда online одновременно. `A2A` уже поддерживает asynchronous task flow, а `DIDComm` добавляет relay-friendly delivery model. Поэтому async/offline нельзя считать edge case; это базовый communication profile для long-running multi-agent work. [C9]

Для агентного уровня это означает две вещи:

- при межагентном вызове следует считать нормой non-blocking task flow, а не ждать немедленного ответа; [C9]
- над протоколом должен стоять durable queue-backed operational layer, потому что A2A сам по себе не обещает delivery guarantees, DLQ или crash recovery. [C11]

### 5.2 `blocking:false`, локальная очередь и relay

Практический baseline D05 для офлайн-устойчивости можно описать так:

| Слой | Роль |
|------|------|
| **`A2A blocking:false`** | не держать caller в синхронном ожидании завершения длинной задачи |
| **local durable queue** | временно удерживать intent/message до доставки; в Logos profile это может быть SQLite-backed очередь |
| **`DIDComm relay`** | доставить зашифрованный payload получателю после reconnect |

Здесь важно не переобещать. Queue и local persistence являются implementation pattern вокруг протокола, а не свойством A2A core. Поэтому SQLite queue допустимо описывать как реалистичный local profile Логоса, но не как часть стандартной гарантии A2A. [C9][C11]

### 5.3 Ошибки, retry и DLQ

Fault tolerance в D05 должна строиться как operational overlay поверх communication stack. Нормализованный корпус фиксирует, что core A2A не определяет production-ready retry semantics, circuit breakers, DLQ handling и containment of cascading failures; всё это добавляется вокруг протокола. [C11]

Для L1 полезно держать минимальную дисциплину:

1. retry допустим только для transient failures и с backoff, а не для всех ошибок подряд; [C11]
2. repeated failures должны открывать circuit breaker на communication path; [C11]
3. сообщения после исчерпания retries должны уходить в DLQ вместе с reason metadata; [C11]
4. failure в `MCP` и failure в `A2A` должны различаться: tool failure деградирует capability, agent failure ломает delegation path целиком. [C10][C11]

### 5.4 Что D05 отдаёт соседним измерениям

Глубокая error taxonomy, rollback discipline и execution recovery принадлежат не D05, а функциональному и инфраструктурному слоям. Поэтому D05 только фиксирует, что communication path обязан быть async-first и queue-aware; detailed compensations остаются у `Saga Pattern` и execution control `→ L1/D04 §4.4` и `§6`. [C11]

---

## 6. Rate limiting и observability

### 6.1 Gateway-aware rate limiting

Коммуникационный слой без rate limiting быстро превращается в источник abuse и каскадных отказов. Корпус фиксирует gateway-level enforcement как практический baseline: policy-aware components типа `Agentgateway v1.0.1` умеют применять rate limits и routing decisions на границе протоколов, а не внутри ad hoc business logic каждого агента. [C10]

Для L1 это даёт три правила:

- лимитируются не только HTTP requests как таковые, но и направление/тип взаимодействия; [C10]
- `A2A`, `MCP` и `AG-UI` требуют разных routing semantics и therefore не должны попадать под один "общий" throttle bucket; [C10]
- human-facing `AG-UI` stream должен оставаться отзывчивым даже тогда, когда peer-agent or tool traffic деградирует. [C3][C10]

### 6.2 Runtime routing rules

На уровне D05 runtime routing rule можно сформулировать очень просто:

| Вопрос | Куда маршрутизировать |
|--------|------------------------|
| это user-facing stream? | в `AG-UI` |
| это peer-agent coordination? | в `A2A` |
| это tool/resource invocation? | в `MCP` |
| это confidential DID-rooted payload? | в `DIDComm` overlay |

Именно такое разделение закрывает gap вокруг runtime protocol selection без изобретения "универсального агенто-протокола". В реальной миссии эти поверхности сосуществуют; выбор делается по направлению взаимодействия и по типу capability, а не по вкусу разработчика. [C2][C6][C10]

### 6.3 Наблюдаемость: что принадлежит D05, а что нет

D05 не владеет full observability stack. Полный trace, telemetry loop и evaluation остаются у `→ L1/D04 §7`. Но D05 обязан зафиксировать коммуникационные сигналы, без которых trace будет сломан: protocol chosen, peer/tool direction, task state transitions, delivery failures, retry exhaustion, rate-limit hits и human-stream events. [C10][C11]

Это особенно важно для коммуникаций через gateway: если routing decisions и rate-limit events не попадают в общую наблюдаемость, Orchestrator теряет возможность объяснить, почему именно задача ушла в peer-agent branch, tool branch или degraded fallback. Поэтому observability здесь описывается как обязательная projection boundary к `Execution Trace`, а не как отдельный owned subsystem. [C10]

### 6.4 Статус `OI-045`

С точки зрения `D05` `OI-045` закрыт как projection-only consumer question: `A2A` остаётся peer-agent lifecycle surface, `DIDComm` остаётся confidential DID-rooted overlay, а production transport / performance characterization для этой связки полностью принадлежит → `L2/D04 §2.2`. [A10][A11]

`Structured Handoff` здесь не расширяется до inter-Logos performance pattern: он остаётся intra-Logos orchestration boundary, тогда как latency budgets, cache policy и batching behavior читаются только как проекция к владельцу `L2/D04`. [A11]

---

## Связь с другими измерениями

| Измерение | Связь |
|-----------|-------|
| **L1/D01 (Идентификационное)** | `DID`, `VC`, `Trust Registry` и runtime identity signals нужны D05 как вход для discovery и security, но не принадлежат ему. Полный identity layer находится в `→ L1/D01 §1` и `§5`. |
| **L1/D04 (Функциональное)** | `MCP` остаётся owned concept функционального слоя и должен ссылаться на `→ L1/D04 §3`; `Structured Handoff` остаётся owned concept orchestration layer и должен ссылаться на `→ L1/D04 §4.3`. Именно здесь исправляются stale refs из `OI-097` и `OI-099`. |
| **L2/D04 (Коммуникационная инфраструктура)** | D05 описывает agent-level surfaces, а `L2/D04` описывает delivery/gateway/scale mechanics. Низкоуровневые transport details, broker topology и gateway deployment должны читаться как проекция `→ L2/D04 §1-§5`. |
| **L3/D02 (Семантика)** | `Performative` и FIPA-derived semantics не владеются D05. D05 лишь использует semantic layer для пояснения intent в сообщении; полная семантика и mapping находятся в `→ L3/D02 §3`. |

---

## Открытые вопросы

### Требуют решения (Critical / High)

Критические и high-gap'ы текущего L1 baseline закрыты корпусом на уровне выбора стека: границы `AG-UI`/`A2A`/`MCP`, базовый `DIDComm` security overlay, discovery-поверхность и gateway-aware rate limiting уже достаточно определены для черновика v4.0. [C1][C2][C3][C4][C7][C10]

### Требуют исследования (Medium)

1. **[Medium] OI-045: consumer projection для `A2A + DIDComm` закрыта.** Для `D05` достаточно зафиксировать split ролей: `A2A` ведёт peer task lifecycle, `DIDComm` даёт confidential DID-rooted overlay, а production performance ownership остаётся у `L2/D04`. [A10][A11]
   > **Resolved.** Consumer-side baseline закреплён без дублирования transport detail: performance characterization, cache posture и governance-tunable budgets читаются через `→ L2/D04 §2.2`. [A10][A11]

2. **[Medium] G8: оркестрация как отдельный owned concept.** Коммуникация фиксирует surfaces, но не должна переписывать orchestration-модель.  
   > **Deferred:** полный orchestration answer находится в `→ L1/D04 §2` и `§4.3`. [C2]

3. **[Medium] G13: точный mapping `Performative -> A2A message types`.** L1 достаточно знает, что semantic layer нужен, но не должен канонизировать весь FIPA mapping.  
   > **Deferred:** semantic ownership остаётся в `→ L3/D02 §3-§4`. [C2]

4. **[Medium] G14: глубокая distributed observability для multi-hop communication.** D05 может требовать communication signals в trace, но не владеет полной telemetry architecture.  
   > **Deferred:** execution-wide observability и trace normalization должны разворачиваться через `→ L1/D04 §7`. [C10][C11]

5. **[Medium] Proposal-level action authorization profiles.** Воронка содержит новые кандидаты вроде `OAP`, но они пока не нужны как canonical baseline D05.  
   > **Open.** Держать `AgentLock`-style gates, `OCap`/`OCapN` и toolkit-based policy control как текущую опору; proposal profiles мониторить отдельно. [C12]
