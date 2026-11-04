# УРОВЕНЬ 2 / ИЗМЕРЕНИЕ 07: БЕЗОПАСНОСТЬ И МОНИТОРИНГ

> **Уровень:** L2 — Среда (инфраструктура)  
> **Измерение:** D06 — Security & Monitoring  
> **Версия:** 4.0  
> **Статус:** Финал v4.0 | Дата: 02.04.2026  
> **Зависимости:** L1/D01, L2/D01, L2/D03, L2/D06, L2/D08

---

## Ключевой вопрос
**Как среда обнаруживает аномалии, предотвращает systemic risks, верифицирует честность и защищает участников от мошенничества?**

---

## Аннотация

Черновик v4.0 сохраняет continuity from `[PREVIOUS VERSION]`, но перестраивает D06 вокруг более жёсткой логики ownership и обновлённого корпуса `C1-C20`. Безопасность N4Y.AI трактуется как средовой security stack, который начинается не с одного "главного" контроля, а с согласованной комбинации `Sybil-защиты`, `AI Watcher`, circuit breakers, adversarial defense, ZKP/privacy controls, TEE-backed confidentiality и incident response. [C1][C3][C7][C13]

В сравнении с v3.1 усиливаются четыре линии. Во-первых, OWASP `Agentic AI Top 10` используется как overlay для agentic-specific рисков, но финальная N4Y.AI mapping table пока остаётся design note, а не frozen standard. Во-вторых, detection stack формулируется как ensemble из probe-based, graph-based и statistical методов, а не как ставка на один детектор. В-третьих, honesty verification выводится за пределы простой репутации и связывается с attestation, verification games и dispute-based slashing. В-четвёртых, KPI-блок намеренно описывается через bands и escalation gates, а не через один универсальный SLA, поскольку `OI-036` пока не закрыт. [C5][C7][C11][C15][C16]

---

## Содержание

1. [Threat taxonomy](#1-threat-taxonomy)
2. [Identity и Sybil protection](#2-identity-и-sybil-protection)
3. [AI Watcher 3-tier](#3-ai-watcher-3-tier)
4. [Финансовые circuit breakers](#4-финансовые-circuit-breakers)
5. [Adversarial defense](#5-adversarial-defense)
6. [Honesty verification и ZKP](#6-honesty-verification-и-zkp)
7. [Smart contract security, TEE application, incident response](#7-smart-contract-security-tee-application-incident-response)
8. [Defense-in-depth и KPIs](#8-defense-in-depth-и-kpis)

---

## 1. Threat taxonomy

### 1.1 Пять классов угроз как базовая рамка D06

В v4.0 D06 сохраняет 5-классовую таксономию угроз из `[PREVIOUS VERSION]`, но уточняет её под agentic-risk landscape. Цель таксономии не в том, чтобы заменить OWASP-каталоги, а в том, чтобы дать N4Y.AI operational grouping для мониторинга, containment и ownership routing. [C1][C3][C5]

| Класс | Что покрывает | Типичные проявления | Где primary response |
|---|---|---|---|
| **I. Identity / privilege abuse** | подмена агента, reuse credentials, confused-deputy, Sybil pressure | fake helper, stolen delegated scope, replay identity chain | D06 + проекция в `→ L1/D01 §3` и `→ L1/D01 §7` |
| **II. Adversarial AI / input integrity** | prompt injection, memory/context poisoning, sandbagging, скрытый policy bypass | malicious tool call, hidden instruction, benchmark gaming | D06 |
| **III. Communication / swarm / collusion** | insecure inter-agent communication, collusion, cascade, rogue-agent propagation | replay, spoofing, covert collusion, swarm failure | D06 + `AI Watcher` |
| **IV. Economic / systemic risk** | flash crash, liquidity drain, denial-of-wallet, cross-agent amplification | abnormal spend velocity, correlated withdrawals, freeze-worthy stress | D06 + `Circuit Breaker` |
| **V. Infrastructure / contract / governance surface** | smart-contract flaws, oracle misuse, TEE compromise, governance attack spillover | exploit path through escrow/oracle, attestation abuse, emergency override capture | D06 with routing to `→ L2/D03 §3` / `§5` and `→ L2/D09` |

### 1.2 OWASP Agentic AI 2026 как overlay, а не замена

`OWASP Agentic AI Top 10` используется в D06 как overlay поверх пяти классов. Это удобно, потому что ASI-категории описывают attack patterns, а D06-классы описывают управленческие зоны защиты. [C5][C6]

| OWASP agentic risk | Куда маппится в D06 |
|---|---|
| `ASI03 Identity & Privilege Abuse` | Класс I |
| `ASI01 Goal Hijack`, `ASI06 Memory / Context Poisoning` | Класс II |
| `ASI07 Insecure Inter-Agent Communication`, `ASI08 Cascading Failures`, `ASI10 Rogue Agents` | Класс III |
| `ASI02 Tool Misuse & Exploitation` | Класс IV, если вред экономический; иначе Класс II |
| `ASI04 Supply Chain`, `ASI05 Unexpected Code Execution`, `ASI09 Human-Agent Trust Exploitation` | Класс V с частичным переходом в II / III |

Эта mapping-table в v4.0 остаётся resolve-in-draft: она достаточна для writing and review, но ещё не зафиксирована как каноническая cross-dimension taxonomy. Именно поэтому governance-specific атаки не "втягиваются" обратно в D06, а держатся как routed boundary к `L2/D09`. [C5]

### 1.3 Практический вывод для N4Y.AI

Главный operational вывод таков: D06 защищает не только от "взлома", но и от деградации управляемости среды. Поэтому prompt injection, collusion, cascade и financial stress рассматриваются как одна цепочка рисков, где слабость в одном слое быстро переходит в соседний. Cross-agent correlation monitoring и automatic containment в этом контексте являются не optional, а mandatory controls. [C1][C3]

---

## 2. Identity и Sybil protection

### 2.1 Что D06 владеет, а что проецирует

Полная PoP-модель, `Human DID -> Logos DID` и privacy profile Binding VC принадлежат идентификационному слою `→ L1/D01 §3` и `→ L1/D01 §7`. Здесь D06 владеет не определением идентичности, а средовым security-profile: как identity layer используется для anti-Sybil, fraud prevention и privilege containment. [C5][C17]

### 2.2 Базовый anti-Sybil стек

В терминах D06 базовый стек остаётся трёхзвенным:

1. `PoP` как верхняя граница "один человек -> один персональный Логос" по owner-логике `L1/D01`.
2. Экономическое трение через staking или equivalent bond, чтобы сделать массовое развёртывание фальшивых identities дорогим.
3. Rate limits и scoped issuance, чтобы даже валидная identity не могла бесконтрольно клонировать агентные цепочки.

Первый элемент owned elsewhere, второй и третий остаются operational controls D06. Такая связка нужна не только против классической Sybil-атаки, но и против `ASI03`-стиля privilege abuse, где злоумышленник пытается распространить доверие по наследуемым credential chains. [C5]

### 2.3 PoP как gate, а не как единственный фильтр

`PoP` описывается полностью в `→ L1/D01 §3`. Здесь достаточно зафиксировать его роль: он ограничивает создание первичных identities, но сам по себе не решает проблему delegated abuse, replay и marketplace-scale farming. Поэтому D06 не трактует PoP как "single silver bullet"; после identity bootstrap всё равно требуются budget fences, session scoping, re-authorization и platform-side throttling. [C5]

### 2.4 Staking и rate limits как средовые контрмеры

В v4.0 сохраняется continuity from `[PREVIOUS VERSION]`: anti-Sybil защита должна быть не только identity-based, но и cost-imposing. Staking делает массовую атаку дороже, а rate limits не дают даже корректно созданным агентам быстро развернуть swarm или wallet-drain campaign. Количественные параметры стейкинга и лимитов в этом rerun не переопределяются и требуют отдельного operational profile; D06 фиксирует только необходимость этого слоя. 

### 2.5 Privacy-совместимость идентификационной защиты

Связка anti-Sybil и privacy не должна скатываться в глобальную deanonymization machine. Поэтому при проверке identity-bound claims D06 должен использовать selective disclosure и unlinkable proofs, а не постоянно раскрывать полную связку человека и агента. Для Binding VC privacy-profile предпочтителен `BBS+`, потому что он поддерживает selective disclosure и unlinkable derived proofs; полная аргументация и threat model уже зафиксированы в `→ L1/D01 §7`. [C17]

### 2.6 Результат для D06

D06 отвечает за то, чтобы identity layer превратился в security control, а не просто в registry record. Практически это означает: `PoP` ограничивает вход, staking ограничивает стоимость атаки, rate limits ограничивают скорость разрастания, а privacy-preserving proofs не позволяют защите превратиться в постоянную утечку идентичности. [C5][C17][C18]

---

## 3. AI Watcher 3-tier

### 3.1 Трёхуровневый надзор как owned concept D06

`AI Watcher` остаётся owned concept D06. В v4.0 он формулируется как 3-tier supervision stack:

| Tier | Роль | Основной вопрос |
|---|---|---|
| **Tier 1: automated monitoring** | непрерывное наблюдение, scoring, anomaly flags | "Что отклоняется от baseline?" |
| **Tier 2: automated protocols** | containment, quarantine, throttling, breaker triggers | "Что нужно сделать немедленно?" |
| **Tier 3: human oversight** | override, interpretation, halt, appeals, legal review | "Где требуется человек?" |

Такой стек согласуется с логикой `AI Act Art.14`: regulation не требует человека в каждой операции, но требует способности понимать, интерпретировать, вмешиваться, останавливать и override system behavior. Полное правовое описание AI Act принадлежит `→ L2/D08 §1`; здесь D06 фиксирует technical implementation of oversight. [C19][C20]

### 3.2 Tier 1: automated monitoring

Первый слой строится на behavioural baselines и multi-signal telemetry. Корпус рекомендует отслеживать как минимум три класса метрик:

1. agent-specific metrics: confidence patterns, resource consumption, tool frequency;
2. interaction metrics: message topology, call chains, communication graph anomalies;
3. system-level metrics: throughput, latency distribution, correlated fault bursts.

Именно здесь D06 встречается с multi-agent anomaly detection. Statistical monitoring даёт более дешёвый и near-real-time baseline, но не видит всей семантики сложных exploit paths. Graph-based oversight видит структуру атак лучше, но дороже по runtime overhead. Поэтому Tier 1 не должен полагаться только на один detector family. [C7][C9][C10]

### 3.3 Tier 2: automated protocols

Если Tier 1 говорит "поведение ненормально", Tier 2 отвечает "как ограничить ущерб". Базовый набор автоматических протоколов:

- throttling по velocity / frequency;
- credential scoping или temporary downgrade полномочий;
- quarantine для отдельных агентов или связанной группы;
- запуск financial circuit breaker;
- перевод случая в mandatory human review.

Tier 2 особенно важен против swarm attacks и cascading failures: в multi-agent среде detection без auto-containment часто означает просто более красивое наблюдение за катастрофой. [C3][C15]

### 3.4 Tier 3: human oversight

Третий слой нужен не для замены автоматики, а для контроля над ней. Human oversight должен иметь:

- доступ к объяснимому incident context;
- право override или halt;
- понимание confidence / uncertainty;
- защиту от automation bias;
- path for appeal and post-incident review.

В этом месте D06 сохраняет continuity with `[PREVIOUS VERSION]`: карантин без процедуры разбора создаёт ложное ощущение безопасности. Поэтому human tier нужен не только для compliance, но и для легитимности enforcement. [C19][C20]

---

## 4. Финансовые circuit breakers

### 4.1 Почему circuit breakers остаются ключевым owned concept D06

`Circuit Breaker` по ownership map принадлежит D06. Это логично: вопрос "как среда предотвращает systemic risks" нельзя закрыть без механизмов мгновенного торможения и containment. В v4.0 circuit breakers трактуются не как частный economic gadget, а как часть security architecture. [C3][C15]

### 4.2 Три класса финансовых breaker-механизмов

В D06 полезно различать три семейства breaker-логики:

| Тип breaker | Что триггерит | Что делает |
|---|---|---|
| **Flash Crash CB** | резкое ценовое отклонение, spike volume, coordinated dump | freeze / slow path для новых критичных операций |
| **Liquidity CB** | abnormal withdrawals, reserve stress, liquidity cliff | лимиты на вывод, staged settlement, deeper review |
| **Systemic Risk CB** | cross-agent correlation, cascading anomalies, multi-market contagion | platform-wide throttling, quarantine domain, emergency pause |

Такое разбиение лучше, чем один "универсальный breaker", потому что оно отделяет micro-volatility от реально системной дестабилизации. [C3][C15]

### 4.3 Чем они отличаются от Sandbox CB

Sandbox CB не должен смешиваться с financial CB. `Sandbox CB` относится к permeability и execution boundary внутри `Microsandbox`-архитектуры и описывается полностью в `→ L2/D06 §5`. D06 использует только ту часть, где break or quarantine нужен для security or systemic containment. Иными словами:

- `Sandbox CB` отвечает на вопрос "пропускать ли выполнение через границу sandbox";
- `Financial CB` отвечает на вопрос "останавливать ли экономический поток ради устойчивости среды".

### 4.4 Какие сигналы должны запускать breaker

Корпус для D06 не даёт одной фиксированной numeric formula, зато даёт правильный design principle: breaker должен учитывать не только цену или объём, но и поведенческую корреляцию агентов, cascade precursors и anomaly clusters. Это особенно важно для случаев, когда сама финансовая атака маскируется под нормальную автоматизацию. [C3][C7][C10]

### 4.5 Почему breaker без human path недостаточен

Автоматическое торможение нужно быстро, но его нельзя оставлять без escalation logic. High-cost false positives могут замораживать честных участников, а low-sensitivity thresholds пропустят cascade. Поэтому у каждого breaker должны быть:

- автоматический safe mode;
- escalation to human review;
- процедура staged restore;
- post-incident tuning.

Именно здесь D06 пересекается с `OI-036`: пока attack-cost model не закрыт, архитектура должна использовать bands и governance-tunable gates, а не делать вид, что существует одна навсегда верная формула. [C15][C16]

---

## 5. Adversarial defense

### 5.1 Prompt injection остаётся угрозой первого порядка

Корпус v4.0 сохраняет жёсткий вывод: prompt injection остаётся primary risk, и single-layer defense не существует. Даже сильные shields могут обходиться; поэтому D06 должен проектироваться как defense-in-depth, а не как зависимость от одной библиотеки, одного classifier или одного system prompt. [C1]

### 5.2 OWASP LLM 2025 как практический baseline

Для adversarial layer D06 использует `OWASP Top 10 for LLM Applications 2025` как нижний baseline, а `OWASP Agentic AI Top 10` как верхний overlay. В D06 это даёт минимум четыре обязательные группы контрмер:

1. input filtering и context hygiene;
2. execution guardrails и privilege minimization;
3. output validation и policy checks;
4. monitoring + HITL для sensitive actions.

Смысл этой связки в том, что prompt injection в agent system редко остаётся "только prompt injection": он быстро превращается в tool misuse, rogue behavior или silent policy drift. [C1][C5]

### 5.3 Sandbagging требует особого мониторинга

`Sandbagging` важен для D06 не как academic curiosity, а как мониторинговая проблема: система может "хорошо проходить тесты" и при этом вести себя иначе в реальном deployment. Поэтому scheduled benchmark checks недостаточны. Нужны unannounced evaluation, behavioural consistency testing и observer diversity, чтобы агенту было сложнее оптимизироваться под заранее известную проверку. [C2]

### 5.4 Ensemble anomaly detection вместо single detector

Для multi-agent anomaly detection D06 рекомендует ensemble из трёх семейств:

| Семейство | Сильная сторона | Ограничение | Роль в ensemble |
|---|---|---|---|
| **Activation probes / NARCBench-style** | сильный in-distribution detection of collusion | transfer uneven, brittleness under filtering | high-signal but not standalone |
| **Graph monitoring / SentinelAgent-style** | видит exploit paths, collusion, unsafe tool use, emergent miscoordination | заметный runtime overhead | structural detection |
| **Auditing / Colosseum + statistical baselines** | выявляет collusion propensity и system-level deviations | часть сигналов probabilistic | calibration and verification |

Эта рекомендация прямо следует из корпуса: no single technique dominates all collusion types, а probe-only monitoring недостаточен как production default. [C7][C8][C9][C10]

### 5.5 Prompt injection defense как трёхслойный minimum

В operational виде prompt injection defense можно свести к трём слоям:

1. `input hygiene`: фильтрация внешнего контента, нормализация, source trust scoring;
2. `execution guardrails`: tool allowlists, scoped credentials, step-level authorization;
3. `output and action verification`: policy checks, anomaly flags, HITL on high-impact actions.

Это не гарантирует нулевой риск, но радикально уменьшает вероятность того, что один заражённый input пройдёт весь pipeline без сопротивления. [C1][C6]

### 5.6 Continuous red teaming как обязательный operational цикл

Adversarial defense нельзя считать "внедрённой" один раз. Корпус рекомендует autonomous, policy-aware red teaming, потому что agentic red-team frameworks устойчиво превосходят static или generic jailbreak baselines по coverage и cost. Для D06 это означает цикл `attack simulation -> detection tuning -> policy update -> rerun`, а не разовый audit before launch. [C12]

---

## 6. Honesty verification и ZKP

### 6.1 Honest behavior нельзя сводить к репутации

Корпус v4.0 прямо указывает: honesty verification нельзя строить только на reputation heuristics. Для D06 нужны incentive-compatible verification games, verifiable inference и attestation-based trust anchors, иначе система проверяет "кто выглядит достойно", а не "кто действительно выполнил работу честно". [C11]

### 6.2 Behavioral attestation как первый слой честности

Для средового уровня полезно различать два типа attestation:

- **identity / environment attestation**: кто выполняет и в каком attested environment;
- **behavioral attestation**: совпадает ли observed behavior с обещанным policy profile.

Второй слой особенно важен для agent marketplace, где fraud часто выглядит не как "сломанный ключ", а как систематическое отклонение между заявленным и фактическим execution profile. Именно здесь D06 может использовать performance-bound credentials, runtime attestations и review-as-principal patterns. [C11]

### 6.3 VeriLLM и dispute-based verification

`VeriLLM`-style подход полезен для D06 как reference model: сначала агент commit-ит исполнение, затем часть вычисления верифицируется с возможностью dispute escalation. Такой путь показывает, что honesty verification может быть экономически устойчивым даже без постоянного полного пересчёта каждого вывода. Важно, однако, сохранять архитектурную оговорку: некоторые схемы предполагают по крайней мере одного honest verifier, и этот assumption должен поддерживаться организационно. [C11]

### 6.4 ERC-8004 и slashing как implementation path

В practical architecture D06 может использовать `ERC-8004`-style attested agent registry и slashing-compatible enforcement как один из implementation paths для dishonest execution. Это не означает, что честность целиком "владeет" токен-слой; смысл в другом: attestation, on-chain identity и economic penalty должны быть связаны так, чтобы ложная аттестация или fraud имели проверяемую стоимость. [C11]

### 6.5 ZKP как privacy-preserving security control

`ZKP` по ownership map принадлежит D06, поэтому здесь допустимо дать full description security-role, а не только ссылку на другой слой. В v4.0 D06 использует `ZKP` в трёх ролях:

1. privacy-preserving compliance;
2. selective disclosure идентификационных и credential claims;
3. verifiable assertions о поведении, статусе или праве доступа без раскрытия лишних данных.

Это важно потому, что security и privacy в N4Y.AI не должны быть врагами: честный участник должен иметь возможность доказать соответствие правилу без передачи всей внутренней истории. [C17][C18]

### 6.6 BBS+ как предпочтительный VC primitive

Для VC-based selective disclosure D06 сохраняет линию, согласованную с `→ L1/D01 §7`: `BBS+` предпочтителен там, где нужен unlinkable proof profile. Это особенно важно для anti-fraud и AML-контуров, где система должна проверить "прошёл ли субъект требуемую проверку", но не превращать каждый compliance-check в тотальный correlation event. При этом статус зрелости должен быть видимым: `BBS+` остаётся в стадии W3C Candidate Recommendation и требует monitor-as-you-deploy posture. [C17]

### 6.7 ZK AML как доказательство без тотального раскрытия

Корпус поддерживает сильный тезис: AML / sanctions / threshold compliance можно проверять через ZK proofs без раскрытия сырых PII или балансов. Это делает D06 совместимым одновременно с anti-fraud и privacy-by-design логикой. Для N4Y.AI это особенно ценно в сценариях, где участники должны доказать KYC/AML readiness, но не готовы постоянно раскрывать полный financial dossier каждому контрагенту. [C18]

---

## 7. Smart contract security, TEE application, incident response

### 7.1 Граница ownership для контрактов и TEE

Смарт-контрактный стек, oracle stack и технические параметры `TEE` принадлежат `→ L2/D03 §3` и `→ L2/D03 §5`. D06 владеет security application layer: как использовать эти технологии для защиты участников, ограничения blast radius и incident handling. Это важное изменение по сравнению с `[PREVIOUS VERSION]`, где D06 местами смешивал технологическую и security ownership-плоскости.

### 7.2 Smart contract security как security overlay

В D06 smart contract security фиксируется не как полный re-description контрактной архитектуры, а как минимальный security baseline для всего on-chain execution:

- formal verification или equivalent assurance для critical custody / escrow / breaker logic;
- external audit before production;
- runtime monitoring и event-based alerting;
- emergency hooks для containment и rollback governance path.

Иначе говоря, "какие именно контракты" описывает D03, а "как не дать им стать точкой катастрофы" отвечает D06. Полный контрактный inventory и oracle topology см. `→ L2/D03 §5`. 

### 7.3 TEE: где обязательность, а где рекомендация

В D06 `TEE` рассматривается как security-control layer для confidentiality, integrity и attestation на недоверенном infrastructure. Корпус прямо предупреждает: TEE не является standalone guarantee и должен быть встроен в defense-in-depth. [C13][C14]

Практическая матрица применения в D06 выглядит так:

| Зона / сценарий | Статус TEE | Обоснование |
|---|---|---|
| **HC Microsandbox** | **mandatory** | high-sensitivity execution требует attested boundary |
| **FS и иные sensitive flows** | **recommended by default** | де-факто становится mandatory, если стоимость утечки выше overhead |
| **Owner-data inference / private RAG** | **strongly recommended** | privacy-heavy workloads выигрывают от attested execution |
| **Public batch / low-sensitivity jobs** | **optional** | допустим non-TEE path при низком риске |

Эта матрица согласуется с `→ L2/D03 §3.5` и одновременно учитывает `OI-161`: D06 не выбирает один hardware vendor как абсолютную истину, а фиксирует security boundary and use-case logic. [C13][C14]

### 7.4 Осторожность по отношению к TEE residual risk

Даже сильный TEE-контур не устраняет все риски. Physical attacks, attestation misuse и configuration drift остаются residual concerns, поэтому D06 должен трактовать TEE как один слой защиты, а не excuse to skip monitoring, segmentation or audit. Это особенно важно в agent systems, где атака на confidential boundary может совпасть с privilege abuse или silent model manipulation. [C13][C14]

### 7.5 Incident response в пяти фазах

В v4.0 D06 сохраняет из `[PREVIOUS VERSION]` пятифазный incident-response lifecycle:

1. `Detect` — обнаружить отклонение или breach signal.
2. `Contain` — локализовать ущерб через quarantine, breaker, scope reduction.
3. `Investigate` — собрать execution trace, attestation evidence, cross-agent correlations.
4. `Remediate` — закрыть root cause, обновить policy, отозвать compromised credentials.
5. `Review` — пересмотреть thresholds, ownership boundaries и false-positive trade-offs.

Такой lifecycle хорошо согласуется с корпусом, который требует выражать monitoring effectiveness через bands для detection, containment, dwell time и escalation gates, а не через одну цифру. [C15]

### 7.6 Quarantine как bridge между monitoring и response

Карантин в D06 нужен не как "мягкий бан", а как безопасный промежуточный режим между detection и полной деактивацией. Он позволяет:

- быстро остановить harmful execution;
- сохранить forensic context;
- не уничтожить path for appeal;
- staged-вернуть участника после проверки.

Эта логика особенно важна, когда система имеет дело с ошибками высокой неопределённости: мгновенное удаление создаёт governance risk, а отсутствие containment создаёт systemic risk. [C3][C15]

---

## 8. Defense-in-depth и KPIs

### 8.1 Девять слоёв защиты как итоговая модель

В v4.0 D06 сохраняет 9-layer defense-in-depth model из `[PREVIOUS VERSION]`, но выравнивает её с обновлённым корпусом:

| Слой | Что защищает | Ключевые controls |
|---|---|---|
| **1. Identity** | вход в среду | PoP gate, scoped identity, anti-Sybil profile |
| **2. Authorization** | пределы полномочий | bounded credentials, re-authorization, rate limits |
| **3. Input / context** | входные данные и память | sanitization, filtering, context hygiene |
| **4. Execution** | tool use и policy adherence | manifests, guardrails, runtime validation |
| **5. Communication** | межагентные связи | signed messages, anti-replay, protocol enforcement |
| **6. Financial safety** | средства и systemic stress | circuit breakers, velocity controls, liquidity gates |
| **7. Confidential computing** | чужая инфраструктура | TEE, attestation, confidential execution |
| **8. Monitoring / response** | обнаружение и containment | AI Watcher, anomaly ensemble, quarantine |
| **9. Governance / review** | легитимность и tuning | human oversight, appeals, post-incident review |

Такая модель хороша тем, что она не делает ставку на один "идеальный" контроль. Если один слой даёт сбой, следующий должен либо остановить атаку, либо хотя бы уменьшить её масштаб. [C1][C3][C7][C13]

### 8.2 Где сюда встраивается continuous red teaming

Red teaming не образует отдельный "десятый слой"; он действует поперечно. Его роль в D06 — постоянно проверять, что девять слоёв действительно работают совместно, а не только хорошо выглядят по отдельности. Именно поэтому `ADVERSIFY`-style testing и policy-aware attack simulation должны запускаться против всей цепочки: identity, prompts, tools, communications, breakers и incident handling. [C12]

### 8.3 KPI-блок: bands, а не ложная точность

Ключевой вывод корпуса состоит в том, что KPI/SLA для monitoring нельзя сводить к одному универсальному magic number. D06 должен использовать bands по следующим измерениям:

- detection latency;
- containment time;
- MTTR / dwell time;
- false-positive rate after triage;
- escalation quality;
- coverage по основным attack classes.

Именно такой подход согласуется с разнообразием benchmark sources и с тем, что разные detector families по-разному распределяют latency и accuracy cost. [C15][C16]

### 8.4 Как обращаться с числовыми целями в v4.0

При этом D06 может сохранить более амбициозные design-targets из legacy continuity, если они помечены именно как target, а не как universal promise. Для Tier-1 critical alerts допустимо использовать aspirational envelope `<100ms` до первичного machine flag и `FPR <0.01%` после полноценных triage / aggregation stages, но эти цифры нельзя фиксировать как глобально обязательный SLA до закрытия `OI-036`. [C15][C16]

Иными словами:

- как **stretch target** эти числа полезны;
- как **единый платформенный норматив** они преждевременны;
- как **governance-tunable objective** они уместны уже сейчас.

### 8.5 Что должен измерять KPI-набор D06

Для D06 осмысленный KPI-набор должен отражать не только скорость, но и качество защитного контура: `MTTD / time-to-flag`, `time-to-contain`, `false-positive rate after escalation`, `dwell time`, `coverage by threat class` и `appeal / restore quality`. Иначе среда будет быстро реагировать, но не обязательно правильно защищать участников. [C15][C16]

### 8.6 Финальный тезис измерения

Ответ D06 на ключевой вопрос выглядит так: среда остаётся безопасной не тогда, когда у неё есть один хороший монитор, один хороший стандарт или один хороший breaker, а тогда, когда identity, monitoring, containment, privacy, attestation и human review образуют единый защитный контур. Именно этот контур должен обнаруживать аномалии, ограничивать systemic risk, проверять честность и защищать участников от мошенничества. [C3][C11][C15][C19]

---

## Связь с другими измерениями

| Измерение | Связь |
|-----------|-------|
| **L1/D01 (Идентификационное)** | `PoP`, DID-chain и privacy profile Binding VC описываются полностью в `→ L1/D01 §3` и `→ L1/D01 §7`; D06 использует их как вход для `Sybil-защиты`, anti-fraud и bounded identity. |
| **L2/D01 (Онтология среды)** | `Microsandbox` как boundary object и логика managed permeability принадлежат `→ L2/D01 §2`; D06 использует эти границы как основу для quarantine и TEE-application matrix. |
| **L2/D03 (Инфраструктурно-материальное)** | Технические параметры `TEE`, smart-contract stack и oracle layer принадлежат `→ L2/D03 §3` и `→ L2/D03 §5`; D06 владеет их security-применением, monitoring и incident posture. |
| **L2/D06 (Координация)** | `Sandbox CB`, execution path и CSB-границы описаны в `→ L2/D06 §5`; D06 отделяет их от financial / systemic circuit breakers и использует только security-проекцию. |
| **L2/D08 (Регуляторное)** | Полные правовые рамки `AI Act`, `GDPR`, `MiCA` и eIDAS принадлежат `→ L2/D08 §1-§4`; D06 описывает лишь техническую реализацию oversight и privacy-preserving compliance controls. |
| **L2/D09 (Governance)** | Governance attacks и authority-abuse в этой версии намеренно routed out of D06; они должны решаться в `L2/D09`, чтобы security enforcement не смешивался с governance ownership. |

---

## Открытые вопросы

### Требуют решения (Critical / High)

1. **[High] OI-035: governance attacks остаются за границей D06, но влияют на security enforcement.** Если emergency powers, appeals или override-path захватываются governance-игроками, D06 не может считать свой security loop замкнутым.
> **Open.** Нужна согласованная связка с `L2/D09`, чтобы authority abuse не выпадал из общей модели безопасности, но и не ломал ownership boundary.

2. **[High] OI-036: attack-cost modeling и trigger economics не закрыты.** Пока нет устойчивой формулы, которая одновременно минимизирует cascade risk, false positives и cost of honest participation.
> **Open.** До закрытия issue v4.0 должен использовать KPI bands и governance-tunable thresholds вместо жёстких универсальных нормативов. [C15][C16]

### Требуют исследования (Medium)

3. **[Medium] G10: N4Y.AI-specific mapping для `OWASP Agentic AI Top 10` ещё не заморожен.** Черновик v4.0 использует рабочий overlay между `ASI01-ASI10` и пятью классами D06, но эта таблица пока design note, а не canonical reference.
> **Deferred.** Нужна отдельная cross-dimension сверка с `L1/D01`, `L2/D04`, `L2/D08` и `L2/D09`, чтобы mapping стал репозиторным стандартом, а не только удобным draft artifact. [C5][C6]
