# УРОВЕНЬ 3 / ИЗМЕРЕНИЕ 01: СЕТЕВАЯ СТРУКТУРА

> **Уровень:** L3 — Взаимодействия (динамика)  
> **Измерение:** D01 — Network Structure  
> **Версия:** 4.0  
> **Статус:** Финал v4.0 | Дата: 04.04.2026  
> **Зависимости:** L1/D01, L1/D05, L1/D09, L2/D06, L2/D04, L3/D02 (semantic layer), L3/D05 (cooperation patterns), L3/D09 (trust propagation)

---

## Ключевой вопрос

**Какие сетевые структуры (P2P, иерархии, рои, хабы) образуются в результате связей между Логосами?**

---

## Аннотация

Этот черновик фиксирует структурный ответ L3/D01 как синтез из нормализованного корпуса и подтверждённых источников: сеть N4Y.AI описывается не как чистый P2P и не как чистая иерархия, а как трёхслойная гибридная топология, где discovery опирается на hub-and-spoke precedent, межлогосное взаимодействие идёт через P2P mesh, а внутрилогосная координация остаётся hub-and-spoke вокруг `Orchestrator`. Внешние источники подтверждают жизнеспособность гибридных и multi-hub топологий, но не дают готовой canonical N4Y-спецификации end-to-end, поэтому итоговая схема здесь является архитектурным синтезом, а не буквальным пересказом одного источника. [C1][E1][E2][E12][E29]

По сравнению с `v3.1` сохранены сильные секции про гибридную топологию, роли, `Sandbox` и handshake, но они перезаписаны строже по ownership-границам. `ADS`, `CSB`, `A2A`, `DID`, `DIDComm`, `Reputation Tier`, `PoP`, `Circuit Breaker` и trust-алгоритмы не разворачиваются здесь как owned concepts; D01 использует их только настолько, насколько это нужно для объяснения формы сети, путей связности, возникновения хабов и границ между subnetworks. [C1][C4][C6][C9][C12]

---

## Содержание

1. [Трёхслойная гибридная топология](#1-трёхслойная-гибридная-топология)
2. [Эмерджентность, хабы и роли узлов](#2-эмерджентность-хабы-и-роли-узлов)
3. [Sandbox как субсеть и роль CSB](#3-sandbox-как-субсеть-и-роль-csb)
4. [Bootstrap, discovery и рост сети](#4-bootstrap-discovery-и-рост-сети)
5. [Handshake, делегирование и жизненный цикл связи](#5-handshake-делегирование-и-жизненный-цикл-связи)
6. [Организационные формы поверх сети](#6-организационные-формы-поверх-сети)
7. [Защита, наблюдаемость и архитектурные границы](#7-защита-наблюдаемость-и-архитектурные-границы)

---

## 1. Трёхслойная гибридная топология

[C1] Внешний корпус подтверждает жизнеспособность hybrid / multi-hub topologies для больших agent-like networks, но не даёт готового полного описания именно N4Y-сети. Поэтому для D01 канонический ответ формулируется как архитектурный синтез из трёх связанных слоёв. [E1][E2][E12][E29]

| Слой | Что связывает | Базовый паттерн | Что важно для D01 |
|------|---------------|-----------------|-------------------|
| **L1 discovery** | поиск и первичное разрешение peer'ов | `hub-and-spoke` поверх `ADS` | discovery не тождественен transport; для масштаба сеть допускает реестрово-центричный discovery path `→ L2/D06 §4`, `→ L2/D04 §6` |
| **L2 inter-Logos** | межлогосное task / message взаимодействие | `P2P mesh` | рабочая сеть между независимыми `Logos` не должна иметь единого обязательного центра; agent-level протоколы остаются в `→ L1/D05 §2-§5` |
| **L3 intra-Logos** | координация микроагентов внутри одного `Logos` | `hub-and-spoke` вокруг `Orchestrator` | структурно это локальная иерархия, а не внешняя сеть; детали orchestration принадлежат `→ L1/D04 §2` |

Практическая форма этой схемы:

```text
L1 discovery:     Logos -> ADS / Almanac / registry-backed lookup
L2 inter-Logos:   Logos <-> Logos <-> Logos (P2P mesh)
L3 intra-Logos:   Orchestrator -> microagents (local hub-and-spoke)
```

Гибридность здесь нужна по двум причинам. Во-первых, pure P2P плохо отвечает на вопрос discovery и cold-start при росте до больших размеров. Во-вторых, pure hierarchy создаёт лишнюю зависимость от узких центров координации. Поэтому D01 принимает разделение: централизованнее discovery, децентрализованнее execution path между независимыми `Logos`, локальная иерархия внутри одного `Logos`. [C1][E1][E22][E23][E24]

---

## 2. Эмерджентность, хабы и роли узлов

[C2][C3] На уровне макроструктуры сеть ожидаемо стремится к scale-free profile: preferential attachment и mixed attachment models поддерживают возникновение хабов, а fitness-based extension позволяет интерпретировать репутацию как network fitness. Для D01 это означает, что хабы не назначаются вручную, а возникают статистически из повторяющегося выбора надёжных peers. [E5][E7][E8][E38]

Базовые формулы, которые здесь важны как structural intuition:

```text
γ = 1 + 1 / p
Π_i(t) = (f_i * k_i(t)) / Σ_j (f_j * k_j(t))
```

Где `p` задаёт долю preferential attachment, `k_i(t)` - текущую степень узла, а `f_i` интерпретируется как fitness. Для N4Y естественная архитектурная гипотеза состоит в том, что `Reputation Tier` влияет на `f_i`, а production-scale диапазон `γ` ожидается примерно в пределах `2.3-2.8`. Это именно синтез для Logos-network, а не уже наблюдённый production факт. Аналогично, отождествление `Diamond`-tier `Logos` с будущими хабами правдоподобно, но остаётся Logos-specific interpretation, а не прямым внешним наблюдением. [C2][C3][E5][E7][E8][E38]

[C8][C15] На уровне операционной роли D01 использует шесть node roles. Внешние FIPA-источники подтверждают management / discovery / transport primitives и traceability delegation chains, но сам каталог из шести ролей является архитектурным разложением N4Y, а не готовой внешней taxonomy. [E9][E11][E20]

| Роль | Структурная функция в сети | Ближайший аналог |
|------|----------------------------|------------------|
| **Initiator** | инициирует миссию и запускает сетевую волну поиска / кооперации | requester / initiator |
| **Executor** | принимает слот и исполняет работу в конкретном edge сети | provider / participant |
| **Coordinator** | удерживает локальную звезду коалиции и управляет sub-delegation | coordination role |
| **Broker** | снижает friction discovery между subnetworks и пулами исполнителей | `DF` / broker |
| **Oracle** | добавляет внешний верификационный сигнал в сетевые переходы состояния | verifying agent |
| **Relay** | обеспечивает перенос сообщений и офлайн-устойчивость на маршруте | `MTS` / relay |

Один `Logos` может занимать несколько ролей в разных контекстах. Для D01 важен не юридический статус роли, а то, как эти роли меняют форму графа: `Coordinator` и `Broker` повышают вероятность локальных хабов, `Relay` поддерживает связность при offline-режимах, а `Oracle` добавляет дополнительные доверительные рёбра в критических маршрутах. [C8][C15][E9][E11]

---

## 3. Sandbox как субсеть и роль CSB

[C6][C7] `Microsandbox` в D01 рассматривается как структурная граница subnetworks: узлы внутри одного `Sandbox` связаны плотнее, чем узлы по разные стороны границы, а кросс-sandbox путь должен проходить через обязательный bridge. Полное описание `CSB` принадлежит `→ L2/D06 §5`; здесь важен только сетевой факт, что `CSB` выступает единственной нормированной точкой межсандбоксной связности. [E25][E28]

```text
Microsandbox A --\
                  > CSB -> cross-sandbox path
Microsandbox B --/
```

Для D01 это даёт две structural consequences:

1. `Sandbox` не просто execution boundary, а отдельная subnetwork с собственным профилем связности и отказов.
2. Cross-sandbox communication не должна интерпретироваться как обычное прямое P2P-ребро; это управляемый маршрут через `CSB`. [C6][C7][E25][E26][E27][E28]

Вопрос singleton vs distributed bridge evidence закрывает только частично. Included sources подтверждают proxy / bridge pattern и сильную изоляцию, но не решают окончательно, должен ли `CSB` быть singleton или federated network. Для production-scale D01 принимает рабочую рекомендацию: distributed `CSB` profile с `5+` relay-узлами и `2/3` quorum лучше соответствует целям отказоустойчивости, чем одиночный bridge. Это архитектурная рекомендация по аналогии с bridge / relayer systems, а не прямой agent-network precedent. [arch-rec, not measured] [C6][C7][E21][E25][E28]

| Вариант | Плюс для структуры | Минус для структуры |
|---------|--------------------|---------------------|
| **Singleton CSB** | проще route graph и governance | создаёт явный SPOF между subnetworks |
| **Distributed CSB** | выдерживает частичные отказы и распределяет bridge-capacity | усложняет quorum, задержки и governance |

---

## 4. Bootstrap, discovery и рост сети

[C4] Discovery-прецеденты из Fetch.ai показывают, что registry-backed registration, weighted endpoint resolution и resolver APIs дают реалистичную основу для `ADS` как bootstrap layer. D01 использует это не как ownership claim про `ADS`, а как аргумент, почему рост сети начинается с более hub-like discovery path, а затем переходит к более self-organizing mesh. [E22][E23][E24]

Cold-start для D01 логично описывать как комбинацию registry-backed bootstrap и DHT-style expansion. В evidence-pack этот переход закрывается как отдельный gap: сеть сначала держится на bootstrap hubs, затем preferential attachment начинает выращивать локальные хабы, после чего граф стабилизируется как scale-free структура. [C2][C4][E1][E22][E23][E24]

| Фаза роста | Ориентир по размеру | Структурная картина |
|------------|---------------------|---------------------|
| **Phase 1** | `0-1K` | bootstrap hubs и registry-centred discovery доминируют |
| **Phase 2** | `1K-100K` | preferential attachment ускоряет появление локальных хабов и кластеров |
| **Phase 3** | `100K-10M` | сеть стабилизируется как scale-free / hybrid structure с редкими hubs и множеством low-degree узлов |

Ориентиры по размеру фаз — архитектурная гипотеза, не измеренные пороги. [arch-rec, not measured]

[C5] Вопрос onboarding / offboarding evidence закрывает только на уровне primitive operations: регистрация, разрешение endpoint'ов и deregistration поддержаны, но формальная end-to-end процедура с timeout, revocation и pending-mission edge cases внешними источниками не показана. Поэтому D01 может зафиксировать только минимальный structural baseline:

1. onboarding должен завершаться публикацией `Agent Card` и активацией discovery path;
2. offboarding должен отзывать discovery visibility и прекращать новые входящие связи;
3. судьба незавершённых миссий и cascading effects остаётся отдельным open protocol question. [C4][C5][E22][E23][E24]

---

## 5. Handshake, делегирование и жизненный цикл связи

[C9][C16] Included evidence поддерживает `DIDComm`-based identity / messaging и A2A trust verification как основу privacy-preserving trust establishment, но не даёт published latency benchmarks для полной связки `ADS -> DIDComm -> A2A session`. Поэтому D01 фиксирует handshake как логическую последовательность шагов, а не как уже измеренный SLA. [E13][E14][E15][E16]

Рекомендуемая пятишаговая последовательность для D01:

1. **ADS discovery:** поиск peer'а по capability / domain и получение `Agent Card` / endpoint. `→ L2/D06 §4`, `→ L1/D05 §3`
2. **Identity resolution:** проверка `DID`, publisher integrity и trust signals. `→ L1/D01 §1`, `→ L1/D05 §3-§4`
3. **Trust establishment:** запуск `DIDComm` / A2A trust layer для защищённого старта. `→ L1/D05 §4.2`
4. **A2A session open:** переход к task negotiation и peer-to-peer message exchange. `→ L1/D05 §2`; для semantics of negotiation performatives (cfp/propose/accept-proposal) → `L3/D02 §2-§3`
5. **Execution handoff:** перевод связи из фазы установления в рабочий контур выполнения / кооперации. `→ L2/D04 §1-§4`

Важно не смешивать structural sequence с окончательной privacy policy. [C10] Included `E...` sources не показывают production-scale `ADS` privacy model, который надёжно разделял бы Corporate `Logos` visibility и privacy Personal `Logos`. Поэтому D01 фиксирует лишь boundary: discovery layer должен поддерживать privacy-preserving trust establishment, но не канонизирует финальную visibility matrix. [C9][C10][E13][E14][E15][E16]

[C8] Делегирование внутри сети допустимо, но глубина sub-delegation не должна быть бесконечной. FIPA- и capability-based precedents поддерживают traceability delegation chains, однако не подтверждают валидированный безопасный максимум глубины. Для D01 рабочая рекомендация - cap на `4` hops: этого достаточно для цепочки `Initiator -> Coordinator -> Executor -> Sub-Executor`, но не создаёт длинные непрозрачные маршруты ответственности. Это рекомендация по структурной управляемости сети, а не externally validated law. [E9][E11]

---

## 6. Организационные формы поверх сети

[C11] Evidence поддерживает существование устойчивых coordination forms выше уровня одного `Logos`: coalition, alliance и guild / collective имеют убедимые governance и coordination precedents, но корпус не даёт полного формального state machine для всех переходов. Поэтому D01 описывает не юридическую форму, а сетевую морфологию этих образований. [E17][E18][E19][E20][E34][E35][E36][E37]

| Форма | Сетевой смысл | Ожидаемая длительность | Что остаётся вне ownership D01 |
|-------|---------------|------------------------|--------------------------------|
| **Рой (Swarm)** | децентрализованный временный кластер без выделенного координатора; агенты самоорганизуются вокруг задачи через stigmergy — косвенную координацию через изменение shared среды (например, обновление ADS-доступности, reputation scores) | часы; распадается при выполнении задачи | механика stigmergy и collective decision-making `→ L3/D07 §6`; threshold conditions `→ L3/D07 §2` |
| **Coalition** | временный кластер вокруг одной миссии с явным координатором | часы - дни | payoff mechanics и cooperation model `→ L3/D05 §1-§4` |
| **Alliance** | повторяемая сеть доверенных связей и standing routes | недели - месяцы | формальная governance-механика `→ L3/D05 §2-§4`, `→ L2/D09 §1, §5-§6` |
| **Guild** | более плотный domain cluster с устойчивыми правилами membership | месяцы - долго | DAO / council governance `→ L2/D09 §1, §5-§6` |

[C15] Теория role adoption, coalition stability и agent collectives поддерживает гипотезу, что такие формы могут быть incentive-compatible, но production validation для agent networks не показана. Поэтому D01 может сказать, что организационные формы правдоподобны и полезны для объяснения network clustering, но не должен объявлять их game-theoretically closed design. [E20][E34][E35][E37]

---

## 7. Защита, наблюдаемость и архитектурные границы

[C12][C13] Attack surface сети нельзя закрыть одним механизмом. Для D01 структурно важен layered defense stack:

1. **Admission / uniqueness:** `PoP` ограничивает массовое размножение identity roots. Полное описание `→ L1/D01 §3`. [E42]
2. **Reputation / trust attenuation:** trust- и reputation-based filters снижают ценность ложных узлов. Полные механизмы `→ L1/D09 §1-§5`, `→ L3/D09 §2-§4`. [E39][E31][E32]
3. **Graph defense:** Sybil-resistant graph methods ограничивают атаку на структуру графа. Полный security owner = `→ L2/D07 §2, §6, §9`. [E30][E40][E41]
4. **Operational containment:** rate limits, quorum, redundancy и `Circuit Breaker` снижают cascading effects. Детали `Circuit Breaker` не принадлежат D01 и читаются как проекция `→ L2/D07 §6, §9`. [E30][E32]

Смысл D01 здесь в том, что эти механизмы дополняют друг друга, потому что защищают разные structural failure modes: fake node admission, manipulation of trust edges, graph partitioning и overload of hubs. Они не являются взаимозаменяемыми. [C12][C13][E30][E31][E32][E39][E40][E41][E42]

[C14] Included evidence также поддерживает scalable communication / monitoring patterns, но не даёт topology-aware observability precedent для production agent networks на масштабе `10M+`. Поэтому D01 фиксирует только требование: сеть должна наблюдать не только transport, но и свою форму - концентрацию хабов, крупные partitions и изменение degree profile. Полный monitoring stack остаётся проектировать вне D01. [E3][E33]

Наконец, D01 сознательно не объявляет закрытым вопрос `NetworkInvariantRegistry.sol`. В evidence-pack прямо зафиксировано отсутствие внешнего прецедента для on-chain registry of network invariants; значит, это остаётся внутренним архитектурным решением платформы и должно быть вынесено в deferred block, а не masquerade as evidence-closed claim.

---

## Связь с другими измерениями

| Измерение | Связь |
|-----------|-------|
| **L1/D01 (Идентификационное)** | `DID` и `PoP` определяют, кто вообще может быть узлом сети. D01 использует их как якоря membership и identity traceability, но полное описание остаётся в `→ L1/D01 §1` и `§3`. |
| **L1/D05 (Коммуникационное)** | `A2A`, `DIDComm` и `Agent Card` дают форму handshake и peer-level communication surfaces, но не принадлежат D01. Полный agent-level communication baseline находится в `→ L1/D05 §2-§5`. |
| **L1/D09 (Репутационное)** | `Reputation Tier` влияет на preferential attachment и вероятность hub emergence, однако сама модель score / tiers не принадлежит D01. Полный owner-ответ находится в `→ L1/D09 §1`, `§4-§5`. |
| **L2/D06 (Координация и исполнение)** | `ADS`, `CSB`, `Microsandbox` и related execution boundaries задают формальные network gateways и subnetworks. D01 использует их как structural constraints: `→ L2/D06 §4-§5`. |
| **L2/D04 (Коммуникационная инфраструктура)** | L2/D04 отвечает за delivery, sharding, multi-region layout и broker topology; D01 использует эти механизмы только как infrastructural substrate для сетевой формы. См. `→ L2/D04 §4-§6`. |
| **L3/D02 (Семантико-протокольное)** | Семантика negotiation performatives (cfp/propose/accept-proposal) и модель намерений принадлежат `→ L3/D02 §2-§3`. D01 ссылается на них при описании handshake step 4. |
| **L3/D05 (Кооперация)** | Коалиции и устойчивые формы совместной работы порождают локальные кластеры и повторяемые рёбра, но модели кооперации и payoff allocation принадлежат `→ L3/D05 §1-§4`. |
| **L3/D09 (Доверие)** | D01 описывает форму графа, а L3/D09 - механику trust propagation по этому графу. EigenTrust-like logic и Trust Graph остаются в `→ L3/D09 §2-§4`. |

---

## Открытые вопросы

### Требуют решения (Critical / High)

1. **[High] Финализировать governance-profile для distributed `CSB` (→ OI-017: status open в реестре).** Phase 8 фиксирует structural recommendation в пользу distributed bridge-профиля (`5+` relays, `2/3` quorum), но не закрывает operator set, bonding model и failure governance. [C6][C7]  
   > **Структурная рекомендация принята (Phase 8):** singleton `CSB` не является preferred baseline для production-scale сети; default смещён к distributed model. **OI-017 остаётся formally open в реестре** — governance design не завершён. [E21][E25][E28]

2. **[High] Формализовать privacy split для Corporate / Personal `Logos` в `ADS`.** Корпус подтверждает trust-establishment primitives, но не показывает production-scale privacy model для discovery visibility. [C9][C10]  
   > **Open.** До отдельного решения не следует канонизировать final visibility matrix для `ADS`; D01 фиксирует только необходимость privacy-preserving trust establishment. [E13][E14][E15][E16]

3. **[High] Формализовать onboarding / offboarding протокол с edge cases.** Регистрационные примитивы подтверждены, но timeout, revocation, pending-mission settlement и cascade handling не закрыты. [C5]  
   > **Open.** Нужен отдельный protocol profile поверх registration / resolver primitives. [E22][E23][E24]

### Требуют исследования (Medium)

4. **[Medium] `NetworkInvariantRegistry.sol` как on-chain registry сетевых инвариантов.**  
   > **Deferred:** В evidence-pack зафиксировано отсутствие внешнего прецедента; это остаётся внутренним архитектурным решением, а не evidence-closed design.

5. **[Medium] Handshake latency benchmarks для `ADS -> DIDComm -> A2A session`.** Included A2A / DIDComm evidence не содержит опубликованных benchmark'ов. [C16]  
   > **Deferred:** Нужен PoC с измерением реальной latency на целевом стеке до фиксации SLA. [E14][E15][E16]

6. **[Medium] Topology-aware observability на масштабе `10M+`.** Общие monitoring patterns подтверждены, но не production precedent для наблюдения именно формы agent graph. [C14]  
   > **Open.** Нужно отдельно определить, какие graph-level metrics становятся governance- и ops-significant baseline. [E3][E33]

7. **[Medium] Формальный lifecycle state machine для coalition -> alliance -> guild.** Теоретические и отраслевые precedents есть, но унифицированной замкнутой модели переходов нет. [C11][C15]  
   > **Open.** D01 пока использует только structural stages, не канонизируя полный lifecycle automaton. [E17][E20][E34][E36]
