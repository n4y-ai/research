# УРОВЕНЬ 3 / ИЗМЕРЕНИЕ 09: ДОВЕРИТЕЛЬНО-РЕПУТАЦИОННОЕ

> **Уровень:** L3 — Межагентные взаимодействия  
> **Измерение:** D09 — Trust  
> **Версия:** 4.0  
> **Статус:** Финал v4.0 | Дата: 04.04.2026  
> **Зависимости:** L1/D09, L1/D01, L3/D08, L3/D10

---

## Ключевой вопрос

**Как формируется локальное точечное доверие конкретного Логоса к другим партнерам на основе прямого опыта взаимодействия?**

---

## Аннотация

`L3/D09` отвечает не за анатомию публичной репутации отдельного Логоса, а за физику межагентного доверия: как локальный опыт превращается в направленные trust-edges, как эти edges распространяются по `Trust Graph`, как ограничивается cross-domain leakage и как система сопротивляется whitewashing, Sybil, bad-mouthing и collusion attacks. Полное описание `Reputation Score` остаётся у владельца `→ L1/D09 §1-§3`; в этом измерении он используется только как входной или агрегированный сигнал более высокого уровня. [C1][C3][C8][C9][C10]

Hard re-run фиксирует три ключевых изменения по сравнению с `v3.1`: во-первых, direct experience вынесен в явный bilateral Beta-layer с асимметрией `A→B != B→A`; во-вторых, `TrustFlow` сохранён как сильный кандидат на upgrade, но помечен как **preprint / not peer-reviewed** и не заменяет автоматически classical `EigenTrust` baseline; в-третьих, архитектурные claims про storage backend и fixed cross-domain matrices очищены до evidence-grounded формулировок и открытых calibration gaps (`OI-021`, `OI-022`). [C2][C4][C5][C8][C12][C15]

---

## Содержание

1. [Trust Stack и границы уровней](#1-trust-stack-и-границы-уровней)
2. [Локальное доверие: direct experience и bilateral asymmetry](#2-локальное-доверие-direct-experience-и-bilateral-asymmetry)
3. [Транзитивное распространение доверия](#3-транзитивное-распространение-доверия)
4. [Trust Graph, cross-domain transfer и privacy boundary](#4-trust-graph-cross-domain-transfer-и-privacy-boundary)
5. [Cold start и защита от атак](#5-cold-start-и-защита-от-атак)
6. [Восстановление доверия после конфликта](#6-восстановление-доверия-после-конфликта)
7. [Операционные пределы и критерии применения](#7-операционные-пределы-и-критерии-применения)

---

## 1. Trust Stack и границы уровней

### 1.1 Операционное разбиение стека

Для `L3/D09` trust computation удобно раскладывать в пять операционных уровней, идущих от first-party evidence к network-wide и platform-enforced constraints:

| Уровень | Что является сигналом | Ownership статус |
|---------|------------------------|------------------|
| **1. Direct experience** | Прямой исход собственных взаимодействий `A→B`, хранимый как bilateral history | **FULL** в `L3/D09` |
| **2. Credential attestation** | `DID`, `VC`, issuer trust, proof of control, PoP-bound claims | ПРОЕКЦИЯ: identity layer полностью в `→ L1/D01 §1`, `§3`, `§5` |
| **3. Propagated trust** | Сетевой вывод по directed trust graph через `EigenTrust-Logos`, `TNA-SL` и candidate-upgrade `TrustFlow` | **FULL** в `L3/D09` |
| **4. Aggregate reputation signal** | Публикуемые reputation bundles, tiers и attestation-derived summaries | ПРОЕКЦИЯ: публикация и score anatomy полностью в `→ L1/D09 §1-§5` |
| **5. Platform invariants** | Hard rules: identity continuity, dispute outcomes, anti-Sybil gates, deny/limit policies | Смешанный слой: identity anchors `→ L1/D01 §3`, security controls `→ L2/D07 §3-§4, §8`, dispute enforcement `→ L3/D08 §4-§5` |

Такое разбиение отвечает evidence pack лучше, чем старая схема `v3.1`: локальный trust больше не смешивается с публичной репутацией, а hard rules не маскируются под обычный graph score. Это **операционное проявление** канонического `Trust Stack`, а не новая glossary-definition: термины центрального глоссария сохраняются, а таблица выше показывает, как эти уровни работают в trust computation pipeline. [C3][C8][C10][C11]

> **Примечание — соответствие уровней каноническому глоссарию:** Уровни 1–5 выше соответствуют канонической пятиуровневой схеме `Trust Stack` следующим образом: уровень 1 (direct experience / Beta/bilateral) = **Identity Trust**; уровень 2 (credential attestation / VC/DID) = **Capability Trust**; уровень 3 (propagated / EigenTrust-Logos/TrustFlow) = **Performance Trust**; уровень 4 (aggregate / Reputation Score L1/D09) = **Relationship Trust**; уровень 5 (platform invariants / non-delegable core L1/D07) = **Network Trust**. Имена в таблице — операционные метки pipeline; canonical glossary-определения остаются в `consistency_glossary.md → Trust Stack`.

### 1.2 Boundary definition между уровнями

Базовое правило стека в `L3/D09` таково:

- Если у Логоса есть собственная history по контрагенту, decision starts at level 1.
- Если history sparse или отсутствует, он опирается на level 2 credential signals и только затем на level 3 propagation.
- Level 4 нужен не для подмены bilateral trust, а для публичного summary и faster discovery.
- Level 5 применяется как veto / floor / ceiling: он ограничивает, но не заменяет ordinary trust computation.

Это особенно важно для ownership discipline. `Reputation Score` описывается полностью в `→ L1/D09 §1-§3`; здесь он выступает как один из входов для discovery и aggregation, но не как полный ответ на вопрос измерения. Аналогично, `DID` и `PoP` принадлежат `→ L1/D01`, а в `D09` нужны только как trust anchors, не как самостоятельный identity subsystem. [C4][C8][C10][C13]

---

## 2. Локальное доверие: direct experience и bilateral asymmetry

### 2.1 Базовая модель direct experience

Локальное доверие начинается не с global score, а с ordered pair `A→B`. Для каждого направления хранится отдельное распределение `Beta(alpha, beta)`, и именно оно отвечает на вопрос: насколько Логос `A` считает Логос `B` надёжным на основании собственного опыта. Это соединяет Bayesian reputation и subjective trust without forcing symmetry. [C3][C4][C9]

Практически это выглядит так:

```text
Start: B_A→B ~ Beta(1, 1)          // uniform prior when no evidence
Success: B_A→B := Beta(alpha+1, beta)
Failure: B_A→B := Beta(alpha, beta+1)
Expected trust: E[p_A→B] = alpha / (alpha + beta)
```

Если `A` и `B` наблюдали один и тот же event, они всё равно обновляют trust independently. Поэтому `B_A→B` и `B_B→A` - разные состояния; это не defect, а базовая directed property trust graph. [C4][C9]

### 2.2 Bilateral asymmetry как обязательное свойство

`A` может доверять `B` как исполнителю, но `B` не обязан симметрично доверять `A` как заказчику, арбитру или рекомендателю. Поэтому в hard re-run draft запрещается любое неявное averaging вида "одна общая trust score на пару". Минимальная каноническая структура - две ordered пары параметров:

```text
(alpha_A→B, beta_A→B)
(alpha_B→A, beta_B→A)
```

Поверх них могут строиться derived views, например private bilateral history или direction-specific confidence bands, но первичный слой остаётся ordered. Это согласуется и с modern asymmetric trust literature, и с TrustFlow's directed-edge design. [C6][C9]

### 2.3 Temporal decay и recency weighting

Evidence pack показывает, что temporal weighting необходимо: raw `EigenTrust` explicit decay не содержит, тогда как adaptive trust models consistently требуют recency sensitivity. Для локального direct experience наиболее сильный empirical fit даёт hyperbolic decay:

```text
R(t) = R0 / (1 + beta * t)
```

По compiled evidence hyperbolic fit превосходит exponential и linear alternatives (`R² = 0.89`) и потому принимается в этом draft как **preferred default for direct experience**, но не как окончательно закрытый governance law. Параметр `beta` остаётся calibratable. Для propagated trust допускается более простое hop/time dampening, если production simplicity окажется важнее empirical optimality. [C14]

### 2.4 Что D09 не владеет в этом слое

`L3/D09` не владеет публичной шкалой `Reputation Score`, tier labels, `Reputation SBT` или publication policy. Всё это описывается полностью в `→ L1/D09 §1-§5`. Здесь: direct experience - приватный bilateral layer, который может feed'ить более высокие reputation and graph layers, но не тождественен им. [C4][C13]

---

## 3. Транзитивное распространение доверия

### 3.1 Classical baseline: `EigenTrust` + `TNA-SL`

С точки зрения hard re-run production baseline должен оставаться консервативным:

- `EigenTrust` даёт проверенный network-level propagation baseline при stochastic / aperiodic trust matrix и power-iteration aggregation. [C1]
- `TNA-SL` даёт формальную обработку uncertainty и transitive recommendation chains, особенно там, где graph sparse и доверие нельзя редуцировать к одному scalar score. [C3]

Эта связка закрывает основной required baseline уже сейчас: `EigenTrust` агрегирует directed local trust, а `TNA-SL` даёт fallback semantics для uncertain multi-hop recommendations и non-trivial opinion fusion. [C1][C3]

### 3.2 Candidate upgrade: `TrustFlow` as EigenTrust-Logos vNext

`TrustFlow` заслуживает отдельного места, но должен быть помечен строго: это **preprint / not peer-reviewed**. Evidence supports it as a credible upgrade candidate, not as already-settled production truth. [C2][C15]

Что именно поддержано evidence:

- Theorem 1: convergence to a unique fixed point via contraction mapping for any Lipschitz-1 transfer operator and `alpha in (0,1)`.
- Theorem 2: with negative trust edges, convergence requires `alpha(1+beta) < 1`.
- Transfer operators family: projection, squared gating, scalar-gated, hybrid - all designed to preserve bounded propagation.
- Sparse benchmark signal exists, but only on small-scale settings; large-N validation remains absent. [C2][C15]

Поэтому hard re-run draft фиксирует rule:

1. `EigenTrust-Logos` = current proven baseline.
2. `TrustFlow` = candidate upgrade for pilot / research branch.
3. Until `OI-021` is closed, any production adoption of `TrustFlow` requires fallback to classical `EigenTrust` + `TNA-SL`.

### 3.3 Hop depth, damping и centralization trade-off

Evidence across the pack converges on a practical constraint: deep trust chains quickly accumulate noise. For N4Y this implies:

- Default practical depth: **3 hops**.
- Optional 4th hop: only for high-value or manually reviewable contexts.
- 5+ hops: treated as weak evidence, not as ordinary production trust. [C3][C14]

`EigenTrust` also retains its classic trade-off: pre-trusted peers improve robustness but create compromise and centralization risk. In N4Y this should be read as a bounded root set of `Trust Anchors`, not as an unlimited privileged class. [C5]

### 3.4 Dynamic and directed graph caveat

Modern directed/dynamic trust models support the general shape of N4Y's graph design, but the evidence base is still immature compared with what would be needed for 100K+ agent production networks. This matters operationally: the direction of travel is sound, but not every new graph-theoretic mechanism is ready for default deployment. [C6][C15]

---

## 4. Trust Graph, cross-domain transfer и privacy boundary

### 4.1 Trust Graph как архитектурная форма

`Trust Graph` в `L3/D09` - это directed, context-sensitive graph of trust relations, not merely a registry of attestations. cheqd-backed direction plus `TRAIN` validation gives a credible architectural basis for DID-linked trust pathways and recursive issuer validation. Полная identity/credential stack принадлежит `→ L1/D01 §1` и `§5`; здесь важна именно graph physics: who trusts whom, through which paths, with what bounded propagation. [C10]

`TRAIN` не навязывает один storage backend и не закрывает вопрос indexing. Следовательно, hard re-run сознательно не фиксирует `Neo4j`, `TigerGraph` или иной concrete backend как settled architecture claim - evidence для такого closure нет. [C10][C15]

### 4.2 Cross-domain transfer: allowed, but topic-gated

Cross-domain transfer поддерживается evidence, но только в similarity-weighted form. Domain pair coefficients cannot be hard-coded once and forever. Instead, transfer should be topic-gated and bounded by semantic distance between source and target domains. [C8]

`TrustFlow` strengthens this with topic-aware leakage control: transfer operators remain Lipschitz-1, while gating suppresses illegitimate spillover from one domain into another. Это хороший кандидат для `EigenTrust-Logos` evolution path, но `OI-022` остаётся открытым, потому что конкретные coefficients и calibration routines для N4Y не выведены из evidence pack автоматически. [C2][C8]

Практический вывод для draft:

- Strongly related domains may inherit partial trust.
- Weakly related domains should receive only shallow prior, not full carry-over.
- Cross-domain transfer always remains subordinate to fresh direct evidence from level 1. [C8][C13]

### 4.3 Privacy boundary и ZKP projection

Privacy-preserving trust proofs поддерживаются несколькими viable paths: `ZKCreds`, anonymous-credential reputation / `zk-promises`, selective-disclosure schemes (`BLS-MT-ZKP` style), and graph-oriented ZK proof patterns (`ZK Graph` / zkSNARK reputation). Но ownership криптографического proof stack остаётся у `→ L2/D07 §8`. [C11]

Здесь фиксируется только L3/D09 requirement:

- verifier may need to know that threshold trust holds,
- but should not automatically learn the entire interaction history,
- and publication of trust edges must not force full disclosure of bilateral history.

Иными словами: `D09` owns the trust question; `L2/D07` owns the proof machinery that hides sensitive evidence while preserving verifiability. [C11]

---

## 5. Cold start и защита от атак

### 5.1 Cold start: сначала low confidence, потом earned trust

Evidence strongly disfavors synthetic default reputation. New Logos should begin from explicit low-confidence or null-trust states and then earn trust through direct interactions or trusted credentials. [C13]

Для N4Y это означает двухканальный bootstrap:

- **Path A:** no history -> `null` / very-low-confidence direct trust, limited-value interactions, gradual accumulation of bilateral evidence.
- **Path B:** strong `DID` / `VC` support -> initial credential-assisted prior, but still with low confidence until direct evidence arrives. Полный identity anchor layer находится в `→ L1/D01 §1-§3`. [C13]

Cold start therefore does not abolish uncertainty; it only changes how uncertainty is surfaced and governed.

### 5.2 Attack taxonomy

Hard re-run retains four canonical attack families:

| Атака | Основной риск | Primary mitigation logic |
|-------|---------------|--------------------------|
| **Whitewashing** | Identity reset to escape bad history | continuity checks, `PoP`/identity binding, no automatic full reset [C7] |
| **Sybil** | Many fake identities amplify trust | cost-of-identity + graph analytics + bounded propagation [C7] |
| **Bad-mouthing** | False negative feedback suppresses honest peer | rater-weighting, anomaly detection, bounded influence [C7] |
| **Collusion ring** | Mutual reinforcement inflates in-group trust | path divergence checks, cycle bounds, directed anomaly analysis [C7] |

### 5.3 Defense-in-depth rule

No single mechanism closes the threat model. Production posture must combine:

- identity continuity anchors from `→ L1/D01 §3`,
- `AI Watcher` / anomaly detection and security controls from `→ L2/D07 §3-§4`,
- graph-bounded propagation from `EigenTrust-Logos`,
- and privacy-preserving evidence presentation from `→ L2/D07 §8`. [C5][C7][C11]

### 5.4 Whitewashing and partial carry-over

Evidence strongly supports persistent identity binding as the core whitewashing defense, but does not support a naive "start fresh and forget everything" policy. Accordingly, hard re-run adopts the following N4Y draft rule:

- Negative trust history is **not** wiped simply by changing presentation or agent instance.
- Positive carry-over, if any, is only partial and only after verified identity continuity and dispute closure.

This is a design synthesis rather than a settled theorem, but it is stricter and more evidence-consistent than the legacy assumption that a new identifier can reset trust state. [C7][C13]

---

## 6. Восстановление доверия после конфликта

### 6.1 Evidence strength: weakest cluster in the dimension

Trust restoration is the least mature evidence cluster in `D09`. The pack supports the existence of recovery formulas, privacy-preserving dispute processes and reputation-governed arbitration, but it does **not** derive one validated N4Y-ready recovery protocol by itself. Any final recovery lifecycle remains partly synthetic design work. [C12]

### 6.2 Minimal N4Y synthesis for Phase 8

The strongest evidence-grounded synthesis is:

1. Conflict is adjudicated through `→ L3/D08 §4-§5`.
2. Financial / delivery closure is reflected in escrow or settlement pathways from `→ L3/D03 §2`, `§6`.
3. Only after adjudication plus remediation evidence can reputation points begin to recover.
4. Recovery is gradual, not instantaneous, and should pass through a probationary phase. [C12]

This allows a conservative `Trust Recovery Protocol` for draft purposes:

| Phase | Condition | Effect on trust |
|------|-----------|-----------------|
| **Containment** | Incident confirmed or dispute opened | freeze further positive trust amplification |
| **Assessment** | `L3/D08` determines liability and required remediation | classify which trust edges were affected |
| **Probation** | escrow settled / remedy completed / no repeat offense | allow only partial recovery |
| **Rehabilitation** | sustained clean interactions after closure | gradual point recovery |
| **Full recovery** | long enough post-incident consistency | restore ordinary propagation eligibility |

Формально арбитраж, escrow semantics и jury logic не принадлежат `D09`; они проектируются из `D08` и `D03`. Здесь owned concept - только how trust should react after those external events. [C12]

### 6.3 What remains unresolved

Draft can justify **that** trust restoration should exist, but not yet exactly:

- how much reputation is frozen,
- how quickly points recover,
- whether domain-specific incidents contaminate all domains equally,
- and which governance body can override automated recovery. [C12]

Those questions remain open because the evidence supports restoration as a pattern, but not one single validated parameterization.

---

## 7. Операционные пределы и критерии применения

### 7.1 What is already strong enough

The current evidence is strong enough to fix the following as draft-level architecture:

- directed bilateral direct trust via Beta updates,
- asymmetric trust representation,
- `EigenTrust` as baseline propagation engine,
- `TrustFlow` as candidate upgrade with explicit preprint warning,
- topic-gated cross-domain transfer instead of universal coefficient tables,
- layered attack defense instead of single-score optimism. [C1][C2][C4][C7][C8][C9]

### 7.2 What is not yet strong enough

The evidence is **not** strong enough to close:

- production calibration of `EigenTrust-Logos` parameters and TrustFlow promotion criteria,
- N4Y-specific cross-domain coefficient matrix,
- backend / index architecture for very large trust graphs,
- large-scale validation beyond small research benchmarks,
- exact trust-recovery milestones and governance overrides. [C2][C12][C15]

### 7.3 Deployment posture implied by this draft

Accordingly, the prudent posture is:

- baseline today: `EigenTrust-Logos` + `TNA-SL`,
- pilot lane: `TrustFlow` in bounded experimental deployment,
- hard requirement: keep direct experience and public reputation analytically distinct,
- hard requirement: treat all cross-domain transfer coefficients as provisional until `OI-022` is closed. [C1][C2][C3][C8][C15]

### 7.4 Trust-collapse cascade и cross-dimension resilience interface

Системные атаки (массовые `Sybil`, `collusion rings`), охватывающие порог критической плотности (`~0.23` по `D07`), могут инициировать trust-collapse cascade с реверсией к более ранней фазе `BELC` (`→ L3/D10 §4.2`, `§4.3`). При обнаружении такого каскада `D10 EWS` должен триггерить heightened defense posture в `D09 Trust Stack`: усиление anti-Sybil gates, anomaly monitoring и ограничение positive trust amplification.

Формальный протокол этого feedback loop пока остаётся open design question: `D09` владеет trust-side detection/defense posture, а `L3/D10` - phase-regression semantics и ecosystem-level early warning.

---

## Связь с другими измерениями

| Измерение | Связь |
|-----------|-------|
| **L1/D09 (Социальное / Репутационное)** | `Reputation Score` описывается полностью в `→ L1/D09 §1-§5`. В `D09` он выступает как публичный aggregate signal и один из inputs для discovery/propagation, но не заменяет bilateral trust. |
| **L1/D01 (Идентификационное)** | `DID`, `VC`, `PoP` и credential-chain validation принадлежат `→ L1/D01 §1`, `§3`, `§5`. В `D09` они используются как trust anchors и credential-level evidence. |
| **L1/D07 (Автономность)** | `LOA` не принадлежит trust layer; здесь он может использоваться только как downstream policy gate или contextual signal. Полная логика earned autonomy находится в `→ L1/D07 §3`. |
| **L2/D07 (Безопасность)** | `AI Watcher`, adversarial monitoring и `ZKP Privacy Infrastructure` остаются проекциями владельца `→ L2/D07 §3-§4`, `§8`. `D09` задаёт only trust-side requirements к этим механизмам. |
| **L3/D03 (Ценностный обмен)** | Эскроу и settlement pathways не принадлежат `D09`, но их закрытие влияет на trust restoration. Детали settlement/dispute routing находятся в `→ L3/D03 §2`, `§6`. |
| **L3/D08 (Конфликтно-арбитражное)** | Арбитраж, liability determination и reputation disputes находятся в `→ L3/D08 §4-§5`; `D09` использует их результаты как external events для trust freeze / recovery logic. |
| **L3/D10 (Эволюционно-сетевое)** | Trust collapse cascade -> trigger `BELC` phase regression `EWS` (`→ L3/D10 §4.2`, `§4.3`). Уровень `BELC` phase определяет background trust defense posture. |
| **L3/D05 (Кооперация)** | Selection of partners and repeated cooperation depend on bilateral and propagated trust outputs from `D09`, но сами cooperation models остаются owned concept `L3/D05`. |

---

## Открытые вопросы

### Требуют решения (Critical / High)

1. **[High] OI-021: promotion criteria for `TrustFlow` over classical `EigenTrust`.** Evidence supports `TrustFlow` as a promising upgrade, but not as an automatically safe replacement.
   > **Resolved (Phase 8):** Draft fixes a conservative baseline: classical `EigenTrust` + `TNA-SL` remain default; `TrustFlow` is marked **preprint / not peer-reviewed** and treated as a pilot candidate, not a closed production choice. [C1][C2][C3]

2. **[High] OI-022: N4Y-specific calibration of cross-domain coefficients and graph indexing.** Topic-gated transfer is justified, but concrete coefficient matrices and indexing/storage choices are still design work.
   > **Open.** Нужны domain-pair calibration data, governance thresholds for leakage tolerance and implementation benchmarks on realistic N4Y graph sizes. [C8][C10][C15]

3. **[High] Trust restoration policy after adjudicated conflicts.** Evidence supports restoration as a pattern, but not a single validated milestone system.
   > **Open.** Нужно закрепить recovery schedule, domain contamination rules, probation thresholds and override authority before this layer can be considered operationally closed. [C12]

### Требуют исследования (Medium)

4. **[Medium] Large-scale validation of directed trust propagation.** Sparse-graph tractability is plausible, but the pack does not validate `TrustFlow` or cheqd-backed trust graph operation at 100K+ scale.
   > **Open.** Нужны stress tests for convergence time, graph-update latency and attack resistance on production-like workloads. [C15]

5. **[Medium] Selection of privacy proof stack for trust queries.** Several ZKP paths are viable, but the evidence does not force one canonical implementation.
   > **Open.** `D09` can specify disclosure requirements now, but stack choice should be finalized jointly with `→ L2/D07 §8` after performance and usability benchmarking. [C11]
