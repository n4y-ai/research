# УРОВЕНЬ 2 / ИЗМЕРЕНИЕ 03: ИНФРАСТРУКТУРНО-МАТЕРИАЛЬНОЕ

> **Уровень:** L2 — Среда (инфраструктура)  
> **Измерение:** D03 — Infrastructure  
> **Версия:** 4.0  
> **Статус:** Финал v4.0 | Дата: 02.04.2026  
> **Зависимости:** L1/D03, L2/D01, L2/D02, L2/D06, L2/D07, L2/D08, L3/D04, L3/D06

---

## Ключевой вопрос

**На какой технической и вычислительной инфраструктуре построена среда и как обеспечиваются её надёжность и масштабируемость?**

---

## Аннотация

Этот черновик фиксирует инфраструктурный ответ уровня L2: среда N4Y.AI строится как гибридная 3-тировая compute-архитектура с централизованным Tier 1 для SLA-критичных задач, децентрализованным Tier 2 для экономичного масштабирования и edge-tier для best-effort и data-local execution. Внутри этого каркаса описываются `vLLM`-ориентированный inference layer, `TEE`-контур конфиденциальных вычислений, 4-уровневый storage stack, смарт-контрактный стек на Base L2, `Resource Router` и механизмы SLA enforcement. [C1][C5][C7][C10][C18][C20]

В сравнении с `[PREVIOUS VERSION]` сохраняется ядро legacy-логики: hybrid compute, `Akash` и `io.net` как Tier 2 precedents, `TEE`, `IPFS` / `Filecoin` / `Arweave`, `ERC-4337 Paymaster`, `State Channels`, oracle stack и multi-provider failover. При этом ownership ужесточён: `AGC` и dual-token economics остаются проекцией `→ L2/D02 §4`, SLA-метрики и штрафы проецируются в `→ L3/D04 §4`, аукционная механика и reverse-auction semantics проецируются в `→ L3/D06 §2`, а security/legal-применения инфраструктуры проецируются в `→ L2/D07 §6-§9` и `→ L2/D08 §4-§5`. [C13][C14][C15][C16][C17][C19]

---

## Содержание

1. [3-тировая compute архитектура](#1-3-тировая-compute-архитектура)
2. [LLM routing и inference](#2-llm-routing-и-inference)
3. [TEE и конфиденциальные вычисления](#3-tee-и-конфиденциальные-вычисления)
4. [Storage layer](#4-storage-layer)
5. [Смарт-контрактный стек и oracle](#5-смарт-контрактный-стек-и-oracle)
6. [Resource Router и SLA enforcement](#6-resource-router-и-sla-enforcement)
7. [Надёжность и масштабируемость](#7-надёжность-и-масштабируемость)

---

## 1. 3-тировая compute архитектура

### 1.1 Почему D03 владеет этой секцией

`L1/D03` отвечает на вопрос, какими ресурсами пользуется Логос и как он получает к ним доступ; `L2/D03` отвечает на вопрос, на какой инфраструктуре этот доступ вообще возможен. Поэтому типология `Compute / Storage / Network / Tooling` и agent-side access flow остаются проекцией `→ L1/D03 §1-§4`, а физическая и вычислительная конфигурация среды описывается здесь полно. [C1][C26]

Связь с `L2/D01` также принципиальна: Base L2 задаёт substrate boundary среды, но D01 не владеет полным compute stack. D01 фиксирует, что среда имеет явную техническую границу на Base Stage 1, а D03 разворачивает, какой execution fabric находится поверх этой границы. `→ L2/D01 §4`. [C17]

### 1.2 Трёхтировая модель

Базовая архитектура среды - не monocloud и не fully decentralized fabric, а иерархия из трёх execution tiers. Она нужна, потому что разные workload-профили конфликтуют по четырём осям: SLA, стоимость, privacy и latency. [C1][C18][C26]

| Tier | Профиль | Типичные провайдеры / паттерны | Когда используется | Основное ограничение |
|------|---------|--------------------------------|--------------------|----------------------|
| **Tier 1** | Централизованный, SLA-first | AWS, GCP, Azure, managed inference endpoints | mission-critical execution, HITL-sensitive flows, гарантированный failover | наивысшая стоимость и vendor dependence [C1][C26] |
| **Tier 2** | Гибридный, cost-sensitive scale-out | `Akash`, `io.net`, другие decentralized GPU markets | inference burst, batch, open-model serving, elastic expansion | вариативность качества и неполная прозрачность networking economics [C2][C4][C23][C27] |
| **Tier 3** | Edge / local / `HomeNode` | data-local nodes, private edge resources, локальные execution pools | минимальная latency, offline tolerance, особые privacy-case'ы | ограниченная ёмкость и weaker SLA [C1] |

Из этой таблицы следует operational principle: SLA-критичная среда не может полностью жить на Tier 2, но и экономически не должна оставаться только в Tier 1. Поэтому гибридность здесь не временный компромисс, а базовая инфраструктурная норма. [C1][C17][C23]

### 1.3 Tier 2 как основной рычаг масштабирования

`Akash` остаётся каноническим precedent'ом для Tier 2. Корпус подтверждает, что сеть обеспечивает materially lower GPU pricing relative to hyperscalers и одновременно продолжает расширять инфраструктурную глубину. В марте 2026 у `Akash` live `Burn-Mint Equilibrium`: tenants burn `AKT` и mint `ACT` как non-transferable compute credits, priced via oracle data; сам факт BME важен здесь не как токеномика N4Y.AI, а как precedent provider-side compute economics для decentralized inference fabric. [C2][C3]

Важно удерживать ownership boundary: `Burn-Mint Equilibrium` в N4Y.AI не переносится на `AGC`. Полная логика `AGC` и `Qi Energy` принадлежит `→ L2/D02 §4`, а в D03 `Akash BME` нужен только как внешний аналог того, как decentralized provider market может стабилизировать billing surface для вычислительных услуг. [C3]

`io.net` дополняет `Akash` как Tier 2 reference не потому, что повторяет ту же модель, а потому, что добавляет agent-driven provisioning. В corpus зафиксировано, что `io.net Agent Compute` позволяет агентам автономно provision GPU through MCP-like orchestration, сочетая hardware verification и cost savings. Абсолютный fleet-size headline меняется по источникам и времени, поэтому здесь важнее не одна цифра, а capability pattern: agent provisions compute, мониторит прогресс и инициирует failover без обязательного human relay. [C4][C23]

Практический baseline D03 таков: Tier 1 удерживает latency-sensitive и legally sensitive execution, Tier 2 принимает bulk inference и burst expansion, а Tier 3 используется там, где locality или privacy outweigh абсолютную ёмкость. Это согласуется с distinction из `L1/D03`: на уровне агента ресурс выглядит как marketplace choice, а на уровне среды - как инфраструктурная дисциплина разнесения workload'ов по tiers. `→ L1/D03 §2-§3`. [C1][C4][C23]

---

## 2. LLM routing и inference

### 2.1 Routing как инфраструктурная функция

Маршрутизация LLM в D03 понимается не как продуктовая feature типа "выбери лучшую модель", а как infrastructure control plane: она определяет, какой inference engine, на каком tier и у какого провайдера обслужит конкретный запрос. Это делает routing одновременно compute concern, cost concern и reliability concern. [C5][C6][C18][C24]

При этом ценовая политика самих операций не является owned-concept D03. Экономическая интерпретация token-denominated pricing остаётся в `→ L2/D02 §4`, а D03 владеет тем, каким стеком реализуется выбор модели и как этот выбор влияет на задержку, throughput и utilitization. [C16]

### 2.2 Базовый inference engine: vLLM

`vLLM` выступает рекомендованным baseline engine для shared inference clusters. Причина не в бренде, а в том, что corpus устойчиво связывает `PagedAttention` и continuous batching с резким ростом throughput и utilization для общих пулов GPU. Это особенно важно для N4Y.AI, где одна и та же среда должна уметь одновременно обслуживать heterogeneous agent workloads, а не один жёстко выделенный tenant. [C5]

Практический вывод: `vLLM` снижает инфраструктурную цену не через дешёвый compute как таковой, а через better packing density. Там, где без него требовалось бы преждевременно расширять GPU fleet, `vLLM` сначала выжимает больше из уже занятого hardware. [C5]

### 2.3 Speculative decoding как второй слой оптимизации

`Speculative Decoding` не заменяет `vLLM`, а надстраивается над ним как дополнительный latency and throughput optimization layer. В corpus это зафиксировано отдельно: engine-level optimization и decoding-level optimization - разные механизмы, которые можно стековать. [C6]

Для среды это означает двухуровневый паттерн: `vLLM` отвечает за memory/layout efficiency и batching, `Speculative Decoding` - за сокращение времени до токена и ускорение generation path. Вместе они уменьшают вероятность того, что рост нагрузки придётся решать исключительно новым железом; именно так `G2` закрывается в `v4.0`. [C5][C6][C21]

### 2.4 Router-level failover для LLM

LLM routing должен выбирать execution path, а не просто "лучшую модель": сначала проверяются privacy/regulatory constraints и latency-budget envelope, затем определяется open-weight self-hosted path и назначаются primary/fallback routes. Точные provider-priority rules и failover thresholds ещё не закрыты, поэтому `OI-068` остаётся активным: architecture уже ясна, а конечные timeout thresholds и escalation rules ещё должны быть доопределены. [C24][C25]

---

## 3. TEE и конфиденциальные вычисления

### 3.1 Ownership и граница с соседними измерениями

По ownership map технические параметры `TEE` принадлежат D03, а security-применения - соседней security-плоскости; поэтому здесь даётся hardware and deployment posture, а security playbooks и defense-in-depth отсылаются в `→ L2/D07 §6-§9`. `Microsandbox` как boundary object полно описывается в `→ L2/D01 §2`, но confidential-compute substrate для `HC` и частично `FS` зон раскрывается уже здесь. [C7][C17]

### 3.2 Текущий TEE baseline

Корпус подтверждает production relevance сразу нескольких TEE pathways: `AMD SEV-SNP`, `Intel TDX` и emerging GPU TEE patterns. Их нельзя свести к одной универсальной технологии; правильнее видеть в них матрицу trade-offs между зрелостью, накладными расходами и supported workloads. [C7]

| Технология | Сильная сторона | Ограничение | Роль в D03 |
|------------|-----------------|-------------|------------|
| **Intel TDX** | зрелые confidential VMs, cloud support, расширение к confidential AI | CPU-first heritage, требуются validated GPU paths | рекомендованный production baseline для HC/FS-sensitive execution; Intel TDX или AMD SEV-SNP — оба обязательны для HC/FS (любая из двух технологий); Intel TDX предпочтителен по широте cloud GA (GCP + Azure). [C8] [OI-161] |
| **AMD SEV-SNP** | production-grade VM isolation, хороший performance/security trade-off | ecosystem fragmentation по cloud/provider support | альтернативный confidential VM path [C7] |
| **GPU TEE / Phala-style** | attested AI inference ближе к real model execution | maturity зависит от провайдера и hardware mode | лучший путь для confidential inference, когда CPU-only enclave недостаточен [C7][C9] |

### 3.3 Intel TDX как production-ready anchor

`Intel TDX` в 2026 уже нельзя описывать как экспериментальный option. Normalized corpus фиксирует expansion from confidential VMs to cloud-supported confidential AI deployments with GPU-connected data paths and multi-region availability. Дополнительный search-draft detail усиливает этот тезис: у Azure TDX confidential VMs есть GA с 5th Gen Intel Xeon и AMX support, а GCP расширил TDX к GKE nodes и Confidential Space with GPU support. [C8]

Для D03 это значит, что `Intel TDX` годится не только для "секретной VM", но и как реальный anchor для HC-like pipelines, где данные проходят через CPU-GPU path и не могут покидать attested boundary в plaintext. [C8]

### 3.4 Phala и GPU TEE

`Phala Network` даёт важный production precedent: live GPU TEE infrastructure с attested H100/H200 inference и reported overhead below 2% на крупных моделях. Это существенно лучше ранних generic expectations о GPU-TEE overhead, и именно поэтому corpus трактует отличие как maturity over time, а не как contradiction. [C9]

Следствие для N4Y.AI простое: confidential inference больше не обязан означать только CPU enclave + remote GPU outside trust boundary. Возможен путь, где сама inference-side GPU execution остаётся attested. Для healthcare и других privacy-heavy сценариев это радикально уменьшает архитектурный компромисс между privacy и usable model performance. [C7][C9]

### 3.5 Где TEE обязателен

В текущем draft baseline таков: для `HC Microsandbox` `TEE` обязателен; для `FS` и иных high-sensitivity flows он рекомендуется по умолчанию и де-факто становится mandatory там, где риск утечки выше стоимости накладных расходов; для owner-data inference и private RAG предпочтителен GPU TEE; для публичных batch workloads он может быть опциональным. Legal qualification privacy-by-design, DPIA и controller obligations описываются полностью в `→ L2/D08 §4`, а D03 фиксирует только технический baseline. [C7][C8][C9]

---

## 4. Storage layer

### 4.1 Четырёхуровневая модель хранения

Storage в D03 должен оставаться multi-tier, а не сводиться к одному "децентрализованному хранилищу". Причина тривиальна: разные данные конфликтуют по permanence, latency, verifiability и deletion semantics. [C10]

| Слой | Что хранится | Основная технология | Почему именно здесь |
|------|--------------|--------------------|---------------------|
| **On-chain** | hashes, attestations, pointers, critical settlement metadata | Base L2 / EAS-style anchors | минимальный доказательный след и settlement coupling [C10][C17] |
| **Hot** | active session state, latency-sensitive retrieval artifacts, in-flight execution context | managed hot stores, cache-adjacent memory layers | нельзя выносить всё в public content-addressed storage без потери latency [C10] |
| **Warm** | shareable corpora, verifiable retrieval assets, intermediate archives | `IPFS`, `Filecoin Onchain Cloud`, Storacha | сочетание portability, content-addressability и proof-backed economics [C10][C11] |
| **Permanent** | audit artifacts, immutable logs, long-lived evidence, canonical publication | `Arweave` | one-time permanence и proximity к provenance-heavy workloads [C10][C12] |

Эта модель сознательно немного отличается от `[PREVIOUS VERSION]`: логика там уже присутствовала, но теперь фиксируется канонический порядок `on-chain / hot / warm / permanent`, чтобы разделить anchor-layer и true permanence-layer. [C10]

Warm storage должен быть verifiable, addressable и экономически переносимым, поэтому `IPFS` + `Filecoin Onchain Cloud` + Storacha образуют strong warm tier для portability и proof-backed retrieval. `Arweave`, в свою очередь, нужен для practically immutable and long-lived artifacts; `Arweave AO` остаётся promising medium-confidence pattern для data-heavy workloads рядом с permanent storage. Одновременно `on-chain` слой должен оставаться минимальным, а `hot` слой - максимально близким к execution path, иначе router-level recovery превращается в повторный full-fetch из warm/permanent storage. [C10][C11][C12][C17][C24]

### 4.2 OI-067 как design note

`OI-067` остаётся открытым не потому, что storage model отсутствует, а потому что не закрыт вопрос placement policy для Episodic Memory и RAG-sensitive data. Corpus достаточно силён, чтобы утверждать многослойную модель хранения, но не фиксирует финальное распределение между hot, warm и permanent tiers для этих классов данных. [C10]

Поэтому корректная формулировка здесь такая: storage architecture уже определена, а placement policy для части knowledge-state остаётся design note. Это лучше, чем ложное впечатление, будто весь storage stack ещё не сформирован. [C10]

---

## 5. Смарт-контрактный стек и oracle

### 5.1 Почему эта секция остаётся в D03

По ownership map смарт-контракты и oracle stack принадлежат D03 как инфраструктурный слой среды. При этом D03 не владеет economics токенов, юридической силой договоров или SLA semantics как social contract. Поэтому техническая реализация контрактов раскрывается здесь, а:

- `AGC` / `Qi Energy` остаются проекцией `→ L2/D02 §4`;
- contractual semantics и SLA metrics - проекцией `→ L3/D04 §3-§4`;
- legal treatment of smart-contract operations - проекцией `→ L2/D08 §5`. [C13][C14][C16]

### 5.2 Текущий контрактный инвентарь

Корпус фиксирует N4Y-specific smart-contract inventory как internal architecture synthesis. Это usable material для project-specific draft, но confidence по нему medium: независимой внешней документации по всему набору 11 контрактов пока нет. Значит, список можно использовать как рабочий infrastructure pack, но не выдавать за externally validated public spec. [C13]

| Контракт | Роль в стеке |
|----------|--------------|
| `QiToken.sol` | value/governance token primitive; economics owned elsewhere |
| `AgentCredits.sol` | operational token contract for `AGC`; pricing and peg logic → `L2/D02 §4` |
| `VotingEscrow.sol` | `veQi` locking primitive; governance semantics → `L2/D09` |
| `QiGovernor.sol` | governance execution contract |
| `CredentialRegistry.sol` | anchoring DID / VC-related registry events |
| `ReputationSBT.sol` | reputation-linked non-transferable badge layer |
| `StakingPool.sol` | staking and incentive pool primitive |
| `EscrowFactory.sol` | escrow deployment surface for marketplace flows |
| `AgentMarketplace.sol` | marketplace-side coordination entry point |
| `CircuitBreaker.sol` | emergency pause / breaker primitive; systemic-risk policy → `L2/D07 §6` |
| `StateChannelManager.sol` | off-chain micro-settlement coordination layer |

Этот стек имеет смысл читать как infrastructure substrate, а не как окончательный contract taxonomy для всех уровней. Отношения и типы контрактов как социально-правовые формы принадлежат `→ L3/D04 §1-§4`; D03 владеет тем, что эти отношения исполняются через on-chain machinery на Base. [C13][C17]

### 5.3 Account abstraction и Paymaster

`Paymaster (ERC-4337)` нужен здесь как инфраструктурный механизм reliability and usability. Он делает возможными gas sponsorship и gasless UX, а значит снижает вероятность того, что агентный runtime сорвётся из-за того, что конкретный smart account не может синхронно оплатить every individual operation. [C16]

Практический смысл секции не в том, чтобы заново описывать dual-token model. Скорее наоборот: `ERC-4337` создаёт execution rail, по которому экономические инструменты могут работать без постоянного friction на уровне L2 gas. Как именно это увязывается с `AGC` settlement и value accounting, полно описывается в `→ L2/D05 §2` (canonical dual-token economics) и `→ L2/D05 §2.3` (oracle пег). [C16]

### 5.4 Oracle stack: layered, not singleton

> **Допущение [A3, A47]:** `AGC` peg depends on reserve/redemption discipline plus monitored oracle layer; `Qi` may act only as supplementary buffer, so oracle failure must not become a singleton dependency.

Oracle layer должен быть не одним feed, а fail-closed composition из трёх source classes:

- `Chainlink` как selected canonical reference layer, whose concrete heartbeat/deviation profile is feed-specific;
- `Pyth` как независимый feed с freshness и confidence signals, whose latency profile is deployment-specific;
- median-style aggregation with governance-tunable `2-minute` baseline `TWAP` / on-chain validation layer как tertiary confirmation, last-good-price anchor и outlier filter.

Именно это делает oracle stack owned concept D03: он является технической инфраструктурой среды, а не только economic accessory. `D05` требует oracle discipline для token stability, но не владеет тем, как именно устроена multi-source oracle plumbing. `→ L2/D05 §2.3`.

Минимальный infrastructure rule таков: цена публикуется только при quorum живых feeds и согласованном median-style consensus. Fallback sequence фиксируется явно: `Chainlink -> Pyth -> median/TWAP-derived last-good-price`; stale-feed timeout и freshness gate обязательны, а divergence halt mechanism при governance-tunable inter-feed расхождении surviving feeds более чем на `20%` или при потере quorum должен эмитить halt signal в breaker / peg-defense path вместо молчаливой публикации singleton price. [A3][A47]

Критический negative lesson для owner-text очевиден: single-oracle dependency может распространяться в redemption halt и system-wide de-peg risk. Поэтому D03 требует oracle independence по reserve path и резервирует за собой detection, fallback и stale-feed logic; policy ownership circuit-breaker response остаётся в `→ L2/D07`, а economic deployment reserves / supplementary `Qi` buffer — в `→ L2/D05 §2.3`. [A3][A47]

### 5.5 State Channels как инфраструктурный слой микроплатежей

`State Channels` в D03 нужно описывать как infrastructure pattern для low-cost, high-frequency settlement. Corpus подтверждает, что state-channel and `x402`-style rails поддерживают low-cost infrastructure settlement suitable for agent microtransactions. [C15]

Здесь важно снова не смешать ownership:

- D03 владеет тем, что `State Channels` и `StateChannelManager.sol` существуют как infrastructural payment rail.
- Полная модель value exchange и escrow semantics принадлежит `→ L3/D03 §3-§4`.
- SLA semantics каналов как договорных отношений принадлежат `→ L3/D04 §4`. [C15]

Именно в таком виде `G7` закрывается корректно: не через перенос всей payment theory в D03, а через фиксацию технологического рельса для frequent low-value settlements. [C15]

---

## 6. Resource Router и SLA enforcement

### 6.1 Что именно owned в этой секции

`Resource Router` - инфраструктурный компонент Логоса для выбора поставщика ресурсов. Его full description принадлежит D03. Но не всё вокруг router'а owned здесь:

- аукционные механизмы как рыночные формы принадлежат `→ L3/D06 §2`;
- ADS / CNP orchestration как coordination layer принадлежат `→ L2/D06 §3-§5`;
- SLA metrics and penalties как contractual semantics принадлежат `→ L3/D04 §4`. [C18][C19][C20]

Следовательно, D03 владеет routing pipeline и enforcement plumbing, а не всей переговорной и договорной онтологией.

### 6.2 Базовый pipeline маршрутизации

> **Допущение [A35, A54]:** Tier 2 routing сохраняет hybrid provider tiers и применяет hard constraints before weighted scoring; transfer pricing therefore starts only after privacy / geo / attestation gates have already passed.

Evidence поддерживает двухшаговую архитектуру:
1. сначала hard constraints: privacy, geo, minimum capacity, latency envelope, required attestation;
2. затем weighted scoring среди оставшихся провайдеров. [C18][C22][A54]

Для inter-provider transfer этот порядок принципиален. Observable public-cloud precedent даёт консервативный anchor `~$0.085-$0.12/GB` для standard egress leg; Tier 2/Open/Specialized providers могут рекламировать lower или zero-egress path, но router не должен принимать это как guaranteed floor, пока hidden-cost path не подтверждён. Поэтому baseline transfer price = measured network cost + topology buffer + fixed settlement markup, а не "whatever provider banner says". [C18][C19][C22][A35][A54]

### 6.3 G10: resolve-in-draft

`G10` остаётся resolve-in-draft не потому, что router weights неясны, а потому что price signals должны сочетать execution score и transfer economics without violating hard-constraint-first sequencing. [C19][C22]

Рабочая конфигурация для `v4.1`:

```text
score(provider) =
  0.30 * success_rate
+ 0.15 * schema_or_policy_compliance
+ 0.20 * latency_score
+ 0.15 * trust_or_reputation_score
+ 0.10 * locality_and_capacity_score
+ 0.10 * budget_fit

transfer_cost(path) =
  transfer_volume_gb * benchmark_egress_rate
+ topology_buffer
+ fixed_settlement_markup
```

Где `benchmark_egress_rate` по умолчанию калибруется against observable `~$0.085-$0.12/GB` precedent, `topology_buffer` покрывает retries, redundancy, compression failure, cross-zone relays и format-conversion risk, а `fixed_settlement_markup` покрывает reconciliation / path-setup overhead между провайдерами. Так D03 остаётся cost-based и не скатывается в speculative tax-style transfer-pricing doctrine. [C18][C19][C20][C21][C22][A35][A54]

### 6.4 Enforcement: от score к проверяемому исполнению

После выбора провайдера router не должен останавливаться на "best guess". Tier 2 transfer pricing должен быть settlement-aware: originating provider несёт measured egress leg, receiving provider — ingress / execution leg, а router фиксирует topology buffer и fixed markup как отдельные строки пути, а не прячет их внутри opaque blended score. [C19][C20][C22][A35][A54]

Corpus подтверждает feasibility verifiable enforcement через сочетание:
- measured transfer volume ? path logs;
- TEE-backed monitoring;
- cryptographic / audit-grade attestation;
- bonded or on-chain settlement reconciliation. [C19][C20][C22]

Это означает, что router и transfer-pricing baseline образуют одну цепочку: сначала допускаемый path, затем scored provider choice, затем measured transfer leg, затем inter-provider settlement с отдельно видимым markup и hidden-cost treatment. Именно поэтому D03 владеет этой частью FULL, а `D05` получает только provider-incentive projection. [C20][C22][A35][A54]

### 6.5 State Channels в enforcement path

`State Channels` полезны не только для дешёвых микроплатежей, но и для SLA-adjacent micro-settlement: периодическое обновление usage state, подтверждение исполнения, быстрый расчёт по коротким интервалам без постоянного on-chain write pressure. [C15][C20]
Вместо того чтобы привязывать каждый шаг к Base L2, среда может агрегировать состояние off-chain и выводить его on-chain only when needed: at close, at dispute, at threshold breach. [C15][C17]

### 6.6 Граница с координацией

`L2/D06` уже содержит coordination precedents для ADS scoring и sandbox execution. D03 не должен дублировать этот слой. Правильная граница такая:

- D04 отвечает на вопрос, как участники и sandbox'ы координируются.
- D03 отвечает на вопрос, на какой инфраструктуре router может технически выбрать, проверить и зафиксировать provider path.

Подробности coordination semantics и auction choreography находятся в `→ L2/D06 §3-§5`. [C18]

---

## 7. Надёжность и масштабируемость

### 7.1 Settlement baseline: Base Stage 1

Для инфраструктурной надёжности важно, что среда якорится не к абстрактному "L2 вообще", а к конкретной execution base. Corpus фиксирует Base Stage 1 fault proofs и multi-provider failover patterns как reasonable baseline для settlement and storage anchoring infrastructure. Это согласуется с `L2/D01 §4`, где Base описан как текущая техническая граница среды. [C17]

Практически это означает:

- settlement anchor = Base on top of Ethereum;
- fault proofs и Security Council дают минимально достаточный decentralization/recovery baseline для Phase 1;
- сам факт Stage 1 не отменяет необходимости application-level failover выше по стеку. [C17]

Иначе говоря, надёжность нельзя "делегировать" Base. Base даёт settlement foundation, но compute, inference и observability всё равно должны проектироваться как multi-provider system. [C17][C24]

### 7.2 Observability и failover

Cross-provider observability должна совмещать `OpenTelemetry` collectors, exporter failover и Prometheus-like health thresholds. Это не optional add-on, а обязательный control plane для гибридной инфраструктуры. [C24]

Минимальный operational baseline, который already supported by evidence:

- regional collectors per provider cluster;
- W3C Trace Context across provider boundaries;
- priority-based exporter failover;
- auto-switch при export failure rate выше 5%;
- alerts при queue saturation выше 85%. [C24]

Decentralized or provider-side signals дополняют этот слой, а не заменяют его. `check_d`, Vigil и Forta-like patterns показывают, что provider health можно оценивать по real protocol checks, predictive failure scoring и weighted SLA metrics. Для D03 важен именно synthesis: централизованный observability plane + decentralized health hints. [C25]

### 7.3 Elastic provisioning

`G13` закрывается через уже mature autoscaling stack, а не через speculative future tooling. Corpus подтверждает, что Kubernetes-based elastic provisioning для AI workloads поддерживается `Karpenter`, NVIDIA Grove / DRA и `KAITO` + `KEDA`, с fast reaction and topology-aware GPU scheduling. [C21]

Predictive layer добавляет Kedify-style and related forecasting systems: scaling уже не обязан быть purely reactive, а capacity можно поднимать до того, как latency деградировала. Для D03 это означает простой target pattern: reactive autoscaling через `Karpenter` / `KAITO`, GPU-aware scheduling через NVIDIA DRA / Grove-like patterns и predictive extension для нагрузки с понятным seasonality or queue profile. [C21][C22]

### 7.4 Decentralized capacity expansion

Elastic provisioning не замыкается на hyperscalers. Corpus отдельно поддерживает тезис, что decentralized GPU markets могут быть used as elastic overflow capacity: `OpenGPU`, `Aethir` и `io.net` демонстрируют globally distributed supply, task-based billing и agent-driven provisioning. [C23]

Значение этого тезиса для D03 таково: масштабирование среды не обязано означать "добавим ещё один cloud account". Оно может означать:

- перенести часть inference burst в Tier 2;
- временно расширить regional pool через decentralized supply;
- снизить idle cost за счёт task-based billing;
- удержать Tier 1 только для workloads, где SLA или compliance важнее цены. [C23]

### 7.5 Networking economics как часть reliability

`G12` показывает, что inter-tier networking costs - не вторичная бухгалтерия, а архитектурный constraint. Hyperscaler egress и cross-zone / cross-region transfer charges уже достаточно хорошо quantified, чтобы topology-aware routing, compression, caching и locality-aware placement считались обязательными, а не "оптимизацией на потом". [C26]

Одновременно `G12` закрыт только частично. Для AWS / Azure / GCP мы имеем хорошие ориентиры, но egress and inter-provider transfer economics у `Akash` и `io.net` остаются публично непрозрачными. Это значит, что для decentralized tiers среда должна закладывать более консервативную margin policy и не считать networking economics fully observable. [C27]

### 7.6 Итоговый reliability posture

Надёжность и масштабируемость среды в D03 обеспечиваются не одной "магической" технологией, а композицией семи слоёв:

1. явный settlement boundary на Base Stage 1;
2. hybrid compute tiers;
3. efficient inference stack;
4. confidential-compute path для sensitive execution;
5. multi-tier storage with placement discipline;
6. verifiable routing and enforcement;
7. observability + autoscaling + topology-aware networking. [C1][C5][C7][C10][C17][C18][C21][C24][C26]

Именно в этой композиции D03 даёт прямой ответ на ключевой вопрос: среда построена не на одном протоколе и не на одном провайдере, а на управляемом, проверяемом и поэтапно масштабируемом инфраструктурном стеке. [C1][C17][C23]

---

## Связь с другими измерениями

| Измерение | Связь |
|-----------|-------|
| **L1/D03 (Материальное)** | `L1/D03` отвечает на вопрос доступа Логоса к ресурсам, а `L2/D03` - на вопрос, на какой инфраструктуре эти ресурсы вообще становятся доступными. Типология `Compute / Storage / Network / Tooling` и agent-side selection остаются проекцией `→ L1/D03 §1-§4`. |
| **L2/D01 (Онтология)** | `L2/D01` владеет boundary-логикой среды и `Microsandbox` как объектом границы; `L2/D03` раскрывает вычислительный substrate, storage, contracts и routing, которые технически поддерживают эту границу. См. `→ L2/D01 §2`, `→ L2/D01 §4`. |
| **L2/D02 (Инструменты)** | `AGC`, `Qi Energy`, dual-token architecture и payment economics не принадлежат D03. Здесь они используются только как projection of operational settlement over infrastructure rails. Подробности `→ L2/D02 §4`. |
| **L2/D06 (Координация)** | D04 владеет CNP, ADS и sandbox execution semantics; D03 владеет compute fabric, router plumbing и infrastructural enforcement вокруг provider choice. Подробности `→ L2/D06 §3-§5`. |
| **L2/D07 (Безопасность)** | Security playbooks, systemic-risk response, smart-contract security и defense-in-depth принадлежат D06; D03 даёт техническую substrate-side часть для `TEE`, observability и breaker-capable infrastructure. См. `→ L2/D07 §6-§9`. |
| **L2/D08 (Регуляторное)** | GDPR/data-minimization и legal qualification smart-contract operations не описываются здесь полно. D03 фиксирует инфраструктурные предпосылки (`TEE`, storage placement, attestation), а правовые обязательства раскрываются в `→ L2/D08 §4-§5`. |
| **L3/D04 (Контракты)** | SLA как формализованный тип договорных отношений не owned в D03. Здесь описан только infrastructural enforcement path; сами SLA metrics, tiers и penalties принадлежат `→ L3/D04 §4`. |
| **L3/D06 (Конкуренция)** | Reverse auctions и иные auction mechanics используются D03 как market-routing precedent, но их полное описание принадлежит конкурентному уровню. См. `→ L3/D06 §2`. |

---

## Открытые вопросы

### Требуют решения (Critical / High)

1. **[High] G10: финальные веса `Resource Router`.** Evidence поддерживает hard-constraint gating и weighted scoring, но фиксирует variation between concrete schemas.
> **Resolved (Phase 8):** В `v4.0` принят рабочий draft-only профиль весов для маршрутизации; он пригоден для архитектурного описания, но остаётся governance-tunable и не считается окончательным закрытием вопроса. [C18][C19]

### Требуют исследования (Medium)

2. **[Medium] OI-067: placement policy для Episodic Memory и RAG-data.** Архитектура `on-chain / hot / warm / permanent` уже определена, но финальная раскладка конкретных knowledge-state классов по слоям не закрыта.
> **Open.** Нужен отдельный design decision по retention horizon, deletion semantics и proximity requirements для retrieval-sensitive data. [C10]

3. **[Medium] OI-068: thresholds и порядок failover для LLM routing.** Observability and failover patterns закрыты на уровне architecture, но не на уровне exact Logos-specific switching thresholds.
> **Open.** Требуются concrete timeout, retry budget, model-cascade и provider-priority rules. [C24][C25]

4. **[Medium] G12: egress и inter-provider transfer economics для decentralized Tier 2.** Для hyperscalers тарифы достаточно прозрачны, для `Akash` и `io.net` - нет.
> **Open.** До появления better public visibility следует использовать conservative network-cost buffers и учитывать риск скрытой стоимости при Tier 2 failover. [C26][C27]
