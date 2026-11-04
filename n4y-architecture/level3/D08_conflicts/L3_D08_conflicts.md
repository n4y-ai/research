# УРОВЕНЬ 3 / ИЗМЕРЕНИЕ 08: КОНФЛИКТНО-АРБИТРАЖНОЕ

> **Уровень:** L3 — Межагентные взаимодействия  
> **Измерение:** D08 — Conflicts & Arbitration  
> **Версия:** 4.0  
> **Статус:** Финал v4.0 | Дата: 04.04.2026  
> **Зависимости:** L3/D03, L2/D08, L2/D09, L1/D09

---

## Ключевой вопрос

**Как возникают споры и конфликты интересов между Логосами и какие динамики их эскалации или разрешения реализуются?**

---

## Аннотация

Это измерение описывает не "суд" как изолированный модуль, а конфликтный слой N4Y.AI как последовательность из трёх взаимосвязанных частей: рабочая таксономия конфликтов, маршрутизация по 5-уровневой лестнице разрешения и граница между допустимой автоматизацией и обязательным human oversight. Для `D08` каноничен именно этот interaction-level взгляд: какие споры возникают, как они классифицируются, когда остаются в машинном разрешении, когда переходят к коллективному или человеческому арбитражу, и каким образом outcome влияет на последующие отношения между сторонами. [C9][C13]

Новый `v4.0` делает две принципиальные коррекции относительно `v3.1`. Во-первых, правовой блок приведён в соответствие с корпусом: для AI-enabled ADR ключевыми нормами выступают `Annex III`, `Recital 61`, `Art. 85`, `Art. 86`, `Art. 27` и `Art. 26` EU AI Act, а не `Art. 47`, который относится к `EU declaration of conformity`. Во-вторых, ownership-границы сделаны жёстче: детали escrow-routing остаются в `→ L3/D03 §6`, регуляторные рамки GDPR / AI Act проецируются из `→ L2/D08 §1` и `→ L2/D08 §4`, полномочия `Security Council` проецируются из `→ L2/D09 §4`, а репутационные последствия конфликта проецируются из `→ L1/D09 §2` и `→ L1/D09 §6.4`. [C1][C3][C4][C11]

---

## Содержание

1. [Таксономия конфликтов](#1-таксономия-конфликтов)
2. [Лестница разрешения и триггеры эскалации](#2-лестница-разрешения-и-триггеры-эскалации)
3. [Lifecycle конфликта](#3-lifecycle-конфликта)
4. [Архитектурные паттерны резолверов](#4-архитектурные-паттерны-резолверов)
5. [GDPR и AI Act как граница автоматизации](#5-gdpr-и-ai-act-как-граница-автоматизации)
6. [Прецеденты, стимулы и anti-manipulation](#6-прецеденты-стимулы-и-anti-manipulation)
7. [Cross-sandbox и регуляторные конфликты](#7-cross-sandbox-и-регуляторные-конфликты)

---

## 1. Таксономия конфликтов

Ни один included source не даёт готовую "каноническую девятку" конфликтов для N4Y.AI целиком, поэтому ниже зафиксирована рабочая synthesis-версия `Phase 8`: она собирает MAS conflict classes, strategy-selection literature и observed protocol practice в один operational taxonomy. Это закрывает G-01 на уровне draft synthesis, но не снимает необходимость последующего consistency review по naming и governance thresholds. [C9][E28][E29][E31][E54]

Причинная половина key question для `D08` выглядит так: споры возникают не абстрактно, а как сбой или contestation внутри повторяющихся interaction patterns. Если результат выполнения задачи не совпадает с ожидаемым output, возникает `task-contract` failure, который обычно материализуется как `C-SQ`, а при споре о критериях или источнике фактов часто комбинируется с `C-SP` или `C-OF`. Если работа выполнена, но payment or escrow withheld, конфликт стартует как `economic dispute` и попадает в `C-PA`, иногда вместе с `C-DL` или `C-SP`. Если unfairly decremented reputation score меняет будущую contracting position, это `reputation dispute` (`C-RP`). Если доступ denied из-за конфликта sandbox rules or compliance policies, это `regulatory dispute`, обычно выражаемый через `C-RG` и при multi-context routing через `C-CS`. Если coalition member defaults mid-mission, спор возникает как `cooperation dispute` и затем раскладывается по девятикатегорийной таксономии через комбинацию `contract type × failure mode`: задержка даёт `C-DL`, невыполнение обещанного вклада - `C-SP` или `C-SQ`, procedural breakdown - `C-GV`. Неразрешённый спор затем создаёт каскад: dispute outcome or even unresolved status влияет на репутацию, снижает вероятность будущего контрактинга, повышает стимул к эскалации и тем самым может переводить локальный конфликт в повторяющуюся conflict cascade. Если же automated dispute outcome затрагивает legal or financial rights, уже на этапе triage должен активироваться human-review path по логике `GDPR Art. 22`, а не только post hoc appeal. [C1][C3][C4][C9][C13]

| Код | Категория | Типичный trigger | Базовый путь разрешения |
|-----|-----------|------------------|-------------------------|
| **C-SQ** | Качество результата | Итог не соответствует обещанным метрикам, benchmark или acceptance criteria | `OracleResolver`, если критерий объективен; иначе `Verdikta` / jury |
| **C-PA** | Платёж и settlement | Оплата удержана, escrow не разблокирован, payout disputed | Проекция в `→ L3/D03 §6`; здесь только выбор resolver |
| **C-DL** | Срок и expiry | Пропущен deadline, challenge window или evidence window | Автоматический timeout / bounded dispute flow |
| **C-SP** | Scope / provenance | Спор о том, что именно входило в обязательство, была ли поставка complete, корректна ли provenance цепочка | Обычно human-reviewed, так как спор об интерпретации |
| **C-RP** | Репутация и feedback | Contestation `Performance VC`, review, dispute outcome annotation | Проекция последствий в `→ L1/D09 §2` и `→ L1/D09 §6.4` |
| **C-OF** | Oracle / factual disagreement | Источник фактов спорен, AI quality subjective, oracle confidence низок | Эскалация от `OracleResolver` к `Verdikta`, затем к human/community review |
| **C-RG** | Регуляторный / compliance | Outcome конфликтует с GDPR, AI Act, sector rules или sandbox policy | Обязательный compliant hybrid path; при необходимости authority complaint |
| **C-AT** | Антимонопольный / Cartel | Подозрение в collusion или cartel behavior из `D06` competition authority flow | `→ L3/D06 §5` → эскалация к `Security Council` → regulatory referral to external authority |
| **C-CS** | Cross-sandbox / jurisdiction | Стороны и assets распределены по разным sandbox / chain / legal wrapper | `CSB`-routing + jurisdiction selection + external wrapper |
| **C-GV** | Governance / emergency / procedural | Appeal against platform-level intervention, emergency halt, procedural fairness challenge | `Security Council` / judicial escalation |

Эта таксономия intentionally смешивает предмет конфликта и характер required resolver. Для `D08` это правильно: в interaction-layer важно не только "о чём спор", но и "может ли спор быть решён автоматически, community-driven или только через competent humans". Именно на этом стыке и возникает основная динамика эскалации. [C9][C13]

---

## 2. Лестница разрешения и триггеры эскалации

Практически полезная модель для `D08` состоит из `pre-step` переговоров и 5 формальных уровней эскалации. `Pre-step` не входит в лестницу, потому что ещё не запускает formal dispute state: это `A2A Dispute Negotiation`, то есть до трёх раундов структурированного обмена предложениями перед маршрутизацией в `DisputeModule`. Полное описание `A2A` как протокола принадлежит `→ L2/D04`, а в `D08` важна только его роль как zero-load de-escalation surface. [C13]

| Уровень | Resolver | Для каких споров | Почему этого уровня достаточно / недостаточно |
|---------|----------|------------------|-----------------------------------------------|
| **L1** | `OracleResolver` | Детерминированные споры: timeout, objective threshold, machine-checkable condition | Достаточен только когда факт проверяем и contestability сохранена; недостаточен при субъективности или legal significance [C8][C13] |
| **L2** | `Verdikta` / multi-AI jury | Субъективная AI quality evaluation, where objective oracle is absent | Дешёвый и быстрый consensus layer, но evidence mostly protocol-native; не должен быть единственным final authority для legally significant outcomes [C6][C8] |
| **L3** | `Kleros` или domain committee | Community / expert adjudication, appeals from automated layers, mixed factual and normative disputes | Наиболее зрелый production analogue, но token/community review не заменяет competent natural-person oversight там, где этого требуют GDPR / AI Act [C5][C13] |
| **L4** | `Security Council` | Emergency, systemic risk, governance-adjacent conflict, halt decisions | Здесь проектируется полномочие `→ L2/D09 §4`; в `D08` важен только момент, когда спор выходит из normal arbitration в emergency governance [C11] |
| **L5** | Judicial / external arbitration wrapper | Cross-border enforceability, regulatory override, due-process challenge, public-law conflict | Нужен, когда on-chain finality не равна off-chain enforceability или when competent authority can overturn practical effect [C11] |

Наиболее важны не сами уровни, а триггеры перехода между ними. Корпус поддерживает следующие базовые правила эскалации: `authority limit`, `complexity`, `low confidence`, `user request`, `legal significance`, `close or contested vote`, `cross-sandbox jurisdiction mismatch` и `regulatory relevance`. Если любой из этих триггеров срабатывает, system should perform warm handoff instead of silent fallback. [C13][E46][E47]

В рабочем виде это даёт следующий routing principle:

1. Если спор сводится к objective predicate и outcome не закрывает право на contestation, он может стартовать в `L1`.
2. Если predicate частично субъективен, но спор low-stakes и требует only bounded probabilistic judgment, допустим `L2`.
3. Если outcome materially affects rights, money, reputation or access, спор должен пройти через `L3+` с documented human/accountable review path.
4. Если затронуты systemic risk, emergency powers или governance legitimacy, спор идёт в `L4`.
5. Если требуется off-chain recognition, regulatory complaint or court-enforceable effect, нужен `L5`. [C1][C3][C4][C11][C13]

---

## 3. Lifecycle конфликта

Ownership map прямо разрешает каждому измерению владеть lifecycle своего объекта, и для `D08` таким объектом является конфликт. Поэтому lifecycle below описывается здесь full, без проекции.

```text
INITIATED
  ↓
TRIAGED
  ↓
ACTIVE
  ├─→ RESOLVED
  ├─→ ESCALATED
  └─→ EXPIRED
```

### 3.1 Initiated

Состояние `Initiated` начинается не с verdict, а с появления `Dispute Record`: стороны, disputed object, evidence pointer, preliminary category, automation level and proposed resolver. Это минимальный канонический артефакт, который нужен для последующей объяснимости, appealability и логирования. [C13]

### 3.2 Triaged

На этапе `Triaged` система решает три вопроса:

1. Это objective or subjective dispute?
2. Имеет ли outcome legal or similarly significant effect?
3. Достаточен ли текущий resolver по competence / authority / auditability?

Именно здесь происходит главный compliance split: многие споры могут быть автоматически классифицированы, но не все могут быть автоматически завершены. [C1][C2][C3][C4]

### 3.3 Active

`Active` означает, что спор вошёл в formal resolution window: открыты evidence submission, vote / review period, appeal timer и applicable logging obligations. Для bounded dispute systems из корпуса критично не допускать бесконечной эскалации: good design требует timeboxing, bonds, confiscation rules for dishonest escalators and explicit upper bounds. [C12][E41][E42]

### 3.4 Resolved / Escalated / Expired

`Resolved` означает, что outcome принят текущим уровнем и оформлен в settlement / annotation / precedent entry. `Escalated` означает, что текущий resolver признан недостаточным, а handoff зафиксирован с reason code. `Expired` допустим лишь для низкозначимых случаев и не должен скрывать спор с правовыми последствиями за простым таймаутом: для таких случаев expiry может завершать только фазу, но не право на contestation. [C1][C3][C13]

---

## 4. Архитектурные паттерны резолверов

### 4.1 OracleResolver как первый слой

`OracleResolver` описывается полно в словаре `D08` как auto-mechanism для объективных критериев; здесь важно зафиксировать его functional boundary. Он пригоден для deterministic disputes и не должен симулировать понимание там, где спор требует интерпретации контракта, нормы или качества AI-output. Для escrow-specific categories `D1-D4` полная детализация маршрутизации остаётся в `→ L3/D03 §6`; в `D08` этот слой выступает первым rung escalation ladder. [C8][C13]

### 4.2 Verdikta как bounded subjective layer

`Verdikta` даёт plausible architecture для "субъективных, но ещё не судебных" споров: multiple AI arbiters, `commit-reveal`, reputation-weighted random selection, `VDKA` staking (`min 100`, per Verdikta whitepaper; staking requirement may change) и low per-verdict cost около `$0.60` (per Verdikta whitepaper, 2025; gas fees at Base L2 rates). Для непрерывных значений это важно ещё и потому, что whitepaper допускает averaging, а не только binary vote. В `D08` это делает `Verdikta` естественным кандидатом на `L2` для AI-quality conflicts, где deterministic oracle уже недостаточен, а full human panel ещё преждевременна. [C6][E14]

Ограничение принципиально: корпус оценивает `Verdikta` как medium-confidence analogue, потому что evidence largely protocol-native и independent production validation ограничена. Поэтому `Verdikta` хорошо работает как recommendation or bounded binding layer внутри заранее согласованного sandbox, но не как универсальный final authority для legally significant outcomes. [C6][C8]

### 4.3 Kleros / committee как зрелый community-review analogue

`Kleros` остаётся самым зрелым decentralized-arbitration reference в screened set: stake-based juror selection, `commit-reveal`, specialized courts, evidence period and external recognition, включая Mexican court recognition in February 2025 (per Kleros 2026 Update blog, confirmed by Kleros documentation). Это делает `Kleros` strongest production analogue для `L3`, особенно там, где нужен more legible appellate or community layer between automation and governance. [C5][E17][E19]

Однако `Kleros` в `D08` нельзя трактовать как magic compliance bypass. Для disputes with legal effects token jurors сами по себе не равны competent natural persons с authority to halt or override. Поэтому в compliant architecture `Kleros` лучше мыслить как one of two patterns:

1. contractually accepted community adjudication for platform-internal effects;  
2. structured input into a natural-person or institutionally wrapped decision layer. [C2][C4][C5]

### 4.4 Security Council и judicial wrapper как верхние слои

`Security Council` не является "обычным судом" и не должен подменять normal dispute processing. Полное описание его состава и кворумов принадлежит `→ L2/D09 §4`; в `D08` он нужен как emergency / systemic override layer, когда конфликт затрагивает platform safety, governance legitimacy or halt authority. [C11]

Верхний внешний слой нужен по другой причине: enforceability. Корпус показывает, что on-chain verdict strongest when paired with recognized arbitration, e-signatures and conventional legal wrapper. Иначе auto-execution может вступить в конфликт с due-process objections, public policy review и multi-jurisdiction enforcement limits. [C11]

---

## 5. GDPR и AI Act как граница автоматизации

Здесь действует жёсткое ownership правило: full doctrinal description GDPR и EU AI Act принадлежит `→ L2/D08 §1` и `→ L2/D08 §4`. В `D08` допустима только проекция того, что эти рамки делают с конфликтной архитектурой. [C1][C3][C4]

### 5.1 GDPR Art. 22: спор нельзя "спрятать" в псевдоавтоматизацию

Базовое ограничение таково: если automated conflict outcome produces legal effects or similarly significant effects, стороны не должны быть subjected to solely automated decision without the relevant exception and safeguards. После `SCHUFA (C-634/21)` этот тест нельзя читать узко: под него могут попадать и intermediate automated assessments, если они materially shape the final outcome. Для `D08` это означает, что даже предварительный oracle / scoring layer может попасть в scope, если platform фактически следует ему без substantive review. [C1][E3]

Meaningful human involvement в корпусе описано как строгий семикритериальный checklist: reviewer должен быть компетентным, иметь authority to override, участвовать в каждом relevant decision, понимать recommendation и её пределы, проводить active analysis rather than rubber-stamping, учитывать контекст beyond raw inputs и работать в процессе, который допускает несогласие без санкции и сохраняет auditability. Если хотя бы одна из этих опор отсутствует, human review рискует быть performative. [C2][E2][E4]

### 5.2 AI Act: AI-enabled ADR по умолчанию попадает в high-risk frame

Для `D08` критична следующая коррекция: `Art. 47` не является нормой о праве на объяснение или ADR safeguards. Релевантный bundle для AI-enabled ADR состоит из:

- `Annex III` и `Recital 61`: ADR AI with legal effects is high-risk;
- `Art. 85`: right to complaint;
- `Art. 86`: right to explanation of the role of AI and main elements of the decision;
- `Art. 27`: FRIA before deployment;
- `Art. 26`: deployer obligations, competent natural-person oversight, monitoring and log retention for at least 6 months. [C3][C4][E6][E7][E8][E9][E10][E11]

Из этого следует архитектурный вывод для `D08`: дешёвая автоматизация и compliance не являются автоматически совместимыми целями. Если conflict system хочет binding effect, она должна сохранять complaint path, explanation path, competent human halt authority и persistent logs. Именно поэтому legally binding conflict outcomes should remain inside a compliant hybrid pathway rather than pure AI or pure token-vote finality. [C3][C4]

### 5.3 Практическое правило для D08

Практический boundary можно сформулировать так:

- fully automated resolution допустим только для objective, low-discretion and contestable branches;
- `Verdikta` / jury-style automation допустим как intermediate assessment or low-stakes layer;
- disputes with legal significance, regulatory exposure or severe reputation / access consequences требуют documented human oversight path;
- formal explanation, complaint and log retention должны проектироваться в `DisputeModule` с момента `Triaged`, а не добавляться post hoc. [C1][C3][C4][C13]

---

## 6. Прецеденты, стимулы и anti-manipulation

### 6.1 Прецеденты как рекомендательная, а не жёстко обязательная система

Корпус поддерживает precedent systems only as conceptually plausible and still immature. Поэтому для `D08` разумно фиксировать `Precedent Registry` не как `stare decisis`, а как recommendation layer: анонимизированный case summary, category, resolver, rationale class, appeal status and outcome tag. Это даёт consistency and training value without pretending that decentralized precedent already converges like mature court doctrine. [C10]

Наиболее реалистичный design pattern здесь polycentric: precedents are stored, indexed and weighted, but their force remains advisory unless отдельное governance decision поднимает их выше. В этом смысле `D08` отвечает не на вопрос "есть ли полноценное common law?", а на вопрос "как сделать прошлые решения полезными, не обещая больше, чем доказано evidence base". [C10][E34][E43][E44]

### 6.2 Почему token-only truth machine недостаточна

Oracle subjectivity остаётся главным unresolved technical risk этого измерения. Screened incident and analytical evidence показывают, что token-only governance cannot simultaneously solve Sybil resistance and plutocracy; large holders can still manipulate outcomes. Это означает, что D08 не может полагаться на single-token truth mechanism как на универсальный conflict finalizer. [C7][E23][E24]

Mitigation stack выглядит реалистично, но не окончательно:

- multi-agent councils and `commit-reveal-judge` flows;
- bonds and slashing;
- reputation overlays;
- random selection;
- SBT-based juror selection research;
- larger panels and appellate cost. [C8][E18][E25]

Именно "mitigate, not eliminate" должно быть формулой `v4.0`: subjectivity hardening уменьшает incentives for manipulation, но не снимает G-07. [C7][C8]

### 6.3 Griefing, hold-up и bounded escalation

Хорошая conflict architecture должна быть not only fair, but also incentive-compatible. Корпус показывает два устойчивых вывода. Во-первых, perfect anti-griefing defense недостижима в общем случае; задача в том, чтобы понизить griefing factor and bound delay. Во-вторых, bounded dispute games с confiscation from dishonest actors и explicit time limits materially improve robustness. [C12][E40][E41][E42]

Отсюда для `D08` следуют три design rules:

1. У каждого уровня должен быть explicit timebox.
2. Эскалация должна требовать bond or meaningful stake.
3. Appeal path не должен быть бесплатным способом истощать честную сторону.

Это одинаково важно и для AI-jury layers, и для human/community arbitration. [C12]

---

## 7. Cross-sandbox и регуляторные конфликты

### 7.1 Cross-sandbox dispute как routing problem, а не только legal problem

`CSB` полно описывается в `→ L2/D06`; в `D08` он нужен только как projection of routing surface. Cross-sandbox conflict начинается с того, что спор уже не локален: assets, obligations, identity anchors or applicable rules распределены across multiple sandbox or chain contexts. Поэтому первый вопрос здесь не "кто прав?", а "какой resolver and legal wrapper вообще компетентны". [C11]

Практический pattern таков: `CSB` удерживает coordination context, escrow state and audit trail, после чего dispute routing выбирает applicable forum, evidence package and enforcement path. Чем выше cross-border / cross-chain complexity, тем сильнее аргумент в пользу external arbitration or judicial wrapper. [C11]

### 7.2 Регуляторный конфликт не исчерпывается внутренним арбитражем

Регуляторный спор отличается от business dispute тем, что platform cannot fully privatize the final answer. GDPR complaint path, market-surveillance complaint path under AI Act, breach notifications and sectoral authority intervention могут practically override or reshape what platform-level arbitration decided. Поэтому `D08` должен считать regulator interaction частью escalation ladder, а не "внешним шумом". [C3][C11]

Отсюда следуют два ограничения:

1. `Security Council` может временно halt or contain, но не подменяет competent authority.
2. Finality внутри protocol не равна finality в публично-правовом смысле. [C11]

### 7.3 Рабочее правило v4.0

Для `cross-sandbox` и `regulatory` disputes корректный default следующий:

- internal automation handles triage and evidence packaging;
- hybrid arbitration handles contractual interpretation;
- external forum handles recognition, public-law challenge and enforceability.

Это делает `D08` не "замкнутым судом", а dispute-routing architecture over multiple layers of authority. [C11][C13]

---

## Связь с другими измерениями

| Измерение | Связь |
|-----------|-------|
| **L3/D03 (Транзакционно-обменное)** | Полная логика escrow patterns, settlement mechanics и `D1-D4` routing принадлежит `→ L3/D03 §2` и `→ L3/D03 §6`. В `D08` используется только projection: какой конфликтный путь нужен поверх этих платежных механизмов. |
| **L2/D08 (Нормативно-правовое)** | Полное описание GDPR, AI Act, MiCA и related legal obligations принадлежит `→ L2/D08 §1` и `→ L2/D08 §4`. `D08` проецирует из этого только operational constraint: где automation must stop and compliant human review must begin. |
| **L2/D09 (Governance)** | Полномочия, состав и emergency authority `Security Council` принадлежат `→ L2/D09 §4`. В `D08` council выступает как верхний escalation layer для governance-adjacent и systemic conflicts. |
| **L1/D09 (Репутационное)** | Репутационные последствия dispute outcomes, correction of attestations и incentive-compatible reputation design принадлежат `→ L1/D09 §2` и `→ L1/D09 §6.4`. `D08` описывает только когда conflict outcome должен инициировать such downstream effect. |
| **L3/D06 (Конкурентно-рыночное)** | Price manipulation, bid fraud, wash trading = `D06`-originated disputes handled in `D08 §1`. Competition authority enforcement disputes → `D08 §4`. `→ L3/D06 §5`; `→ L3/D08 §1`, `§4` |
| **L3/D07 (Организационно-эмерджентное)** | Cartel formation disputes, Alliance-vs-Cartel classification → `D08 §1` taxonomy. Cascade-related systemic disputes → `D08 §7`. `→ L3/D07 §4.1`; `→ L3/D08 §1`, `§7` |

---

## Открытые вопросы

### Требуют решения (Critical / High)

1. **[Critical] G-07: oracle subjectivity for AI tasks остаётся нерешённой.**
   > **Open.** `OracleResolver`, `Verdikta`, `Kleros` и council-based mechanisms together form a credible ladder, но corpus прямо не поддерживает полное устранение манипуляции для subjective AI-quality disputes. Для `v4.0` принят принцип "mitigate, not eliminate". [C7][C8]

2. **[High] OI-014 / OI-110: meaningful human oversight и liability framing требуют межизмерительного согласования.**
   > **Open.** Для `D08` уже ясно, что legally significant outcomes нельзя замыкать на pure automation or anonymous token finality. Но окончательная формулировка границы между autonomy, liability chain, halt authority и review competence должна быть согласована с `L2/D08` и смежными правовыми workstreams. [C2][C3][C4]

3. **[High] G-01: девятикатегорийная таксономия введена как рабочая, но не зафиксирована как окончательная онтология.**
   > **Resolved (Phase 8):** В `v4.0` собрана operational taxonomy из 9 категорий, достаточная для маршрутизации споров и выбора resolver.
   > **Deferred:** Требуется последующий consistency pass по naming, boundary between `C-SP` and `C-OF`, и governance thresholds. [C9]

### Требуют исследования (Medium)

4. **[Medium] G-05: production-scale precedent convergence не доказана.**
   > **Open.** `Precedent Registry` разумно трактовать как advisory memory layer, а не зрелый decentralized common law. Нужны данные о weighting, deprecation and convergence over time. [C10]

5. **[Medium] Cross-sandbox enforceability остаётся частично открытой.**
   > **Open.** Корпус поддерживает hybrid wrapper model, но не даёт clean solution для multi-chain execution and recognition across many jurisdictions. Это ограничивает силу pure on-chain finality. [C11]

6. **[Medium] Auto-expiry и bounded timing стандартизированы не полностью.**
   > **Open.** Есть сильные образцы bounded dispute design, но нет единого canonical framework для expiry that preserves fairness, contestability and operational efficiency across all categories. [C12][C13]

---

## Источники

> Все цитаты `[C#]` трассируются через [`evidence/evidence-pack.md`](evidence/evidence-pack.md). Числовые данные (VDKA staking min 100, $0.60/verdict, Kleros Mexican court recognition February 2025) атрибутированы источникам в тексте; см. evidence-pack для полных библиографических записей.
