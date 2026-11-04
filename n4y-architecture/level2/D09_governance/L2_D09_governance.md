# УРОВЕНЬ 2 / ИЗМЕРЕНИЕ 09: УПРАВЛЕНЧЕСКОЕ (GOVERNANCE)

> **Уровень:** L2 — Среда (инфраструктура)  
> **Измерение:** D09 — Governance  
> **Версия:** 4.0  
> **Статус:** Финал v4.0 | Дата: 02.04.2026  
> **Зависимости:** L2/D01, L2/D02, L2/D05, L2/D08, L1/D09, L1/D07

---

## Ключевой вопрос

**Кто и как управляет правилами среды, их изменениями и экстренными мерами?**

---

## Аннотация

Этот черновик собирает owner-слой governance архитектуры N4Y.AI вокруг восьми тем: бикамеральной DAO-модели, `veQi`, `Security Council`, `AIP`-процесса, Citizens' Chamber с Sybil-resistance, межпалатного разрешения конфликтов, делегированных governance-органов и roadmap `Progressive Decentralization`. В режиме hard carry-over документ сохраняет конструктивное ядро [PREVIOUS VERSION] там, где оно согласуется с новым корпусом, но переносит содержание в template `v2.0`, явнее проводит ownership-границы и отдельно помечает design-решения, которые всё ещё не закрыты evidence. [C1][C3][C4][C5]

Ключевой тезис `v1` таков: governance N4Y.AI не должно опираться ни на один источник легитимности. `Qi Chamber` нужен для stake-weighted economic coordination, `Citizens' Chamber` - для public-goods, safety и sandbox-policy легитимации, а `Security Council` - как узкий emergency-backstop на ранних фазах. При этом ни `PoP`-стек, ни legal wrapper, ни economics `Qi` не переписываются здесь заново: они используются как зависимые owner-проекции `→ L1/D01 §3`, `→ L2/D07 §2`, `→ L2/D05 §2–§3`, `→ L2/D08 §7.5` (DAO legal wrapper jurisdiction projection). [C1][C5][C9][C10]

---

## Содержание

1. [Бикамеральная DAO-архитектура](#1-бикамеральная-dao-архитектура)
2. [veQi как механизм Qi Chamber](#2-veqi-как-механизм-qi-chamber)
3. [Security Council](#3-security-council)
4. [AIP-процесс и типы решений](#4-aip-процесс-и-типы-решений)
5. [Citizens' Chamber и Sybil resistance](#5-citizens-chamber-и-sybil-resistance)
6. [Межпалатные конфликты и governance-атаки](#6-межпалатные-конфликты-и-governance-атаки)
7. [Sandbox Councils, TAC и Conviction Voting](#7-sandbox-councils-tac-и-conviction-voting)
8. [Progressive Decentralization roadmap](#8-progressive-decentralization-roadmap)

---

## 1. Бикамеральная DAO-архитектура

### 1.1 Почему N4Y.AI не должен сводить governance к одной палате

Нормализованный корпус поддерживает исходную интуицию [PREVIOUS VERSION]: closest production precedent для N4Y.AI - не однопалатный token vote, а bicameral или stakeholder-based governance, где token-holders и non-token legitimacy surfaces не смешиваются в один агрегат. Именно поэтому `Qi Chamber` и `Citizens' Chamber` должны читаться не как cosmetic duplication, а как два разных источника authority. [C1][C2]

`Qi Chamber` отвечает за экономически чувствительные и protocol-level вопросы, где цена ошибки и стоимость координации напрямую связаны с locked capital. `Citizens' Chamber` нужна там, где governance не должно полностью зависеть от размера позиции в `Qi`: public goods, safety priorities, sandbox-policy и отдельные constitutional veto cases. Это совпадает и с проектной логикой N4Y.AI, и с тем, как production governance 2025-2026 всё чаще отделяет economic legitimacy от user or stakeholder legitimacy. [C1][C2][C5]

### 1.2 Базовая раскладка палат

| Палата | Источник легитимности | Базовая функция | Что не должно решаться в одиночку |
|--------|-----------------------|-----------------|-----------------------------------|
| **Qi Chamber** | locked `Qi` через `veQi` | параметрическая настройка, экономические контуры, выбор ordinary governance course | constitutional changes с human-legitimacy следствием |
| **Citizens' Chamber** | `PoP`-верифицированные human-backed участники | public goods, safety, sandbox policy, social legitimacy backstop | treasury-wide tokenomics и operator-critical hotfixes |

Эта раскладка не означает симметрию "50/50 по всем вопросам". Ordinary parameter updates должны проходить быстрее и дешевле, тогда как изменения архитектуры среды и emergency authority требуют более широкой легитимации; именно поэтому далее `AIP` делятся на типы `A/B/C`, а не сводятся к одному шаблону proposal. [C4]

`Optimism` остаётся лучшим структурным ориентиром для этой секции, но не шаблоном-копией: production governance там уже сочетает token-weighted layer, stakeholder citizenship, optimistic approval и veto windows, тогда как chamber boundaries N4Y.AI ещё нужно закрепить отдельно. Полная governance ownership остаётся здесь, в `L2/D09`, а зависимые контуры не переписываются заново: `Qi Energy` и `Revenue Distribution` остаются в `→ L2/D05 §2–§3`, managed boundary среды - в `→ L2/D01 §2`, legal wrapper - в `→ L2/D08 §7.5`. [C1][C11]

Безопасный стартовый режим therefore выглядит так: сначала разворачивается `Qi Chamber`, затем ограниченная по scope `Citizens' Chamber`, а `Security Council` остаётся emergency-anchor до перехода к более глубокой bicameralism через `Progressive Decentralization`. [C5][C12][C13]

### 1.3 DAO wrapper and monetary-policy authority

> **Допущение [A24, A25, A86]:** N4Y.AI сохраняет бикамеральную `Governance DAO` как источник ordinary governance-legitimacy, а `Security Council` - как emergency-only backstop. До финального юридического заключения day-to-day monetary-policy authority для `AGC` исполняется через accountable off-chain legal-wrapper layer, тогда как DAO удерживает appointment, mandate и constitutional control, а дальнейшая передача полномочий ограничена stop-rules `§8.5-§8.6`.

`MiCA`-периметр для `AGC` не даёт evidence-backed пути, в котором pure on-chain DAO сама по себе становится допустимым bearer of EMT-facing governance obligations: текущий корпус требует идентифицируемую legal entity с accountable management/governance structure, а не только token-holder vote. Поэтому для N4Y.AI ordinary monetary-policy authority должна быть отделена и от `Security Council`, и от идеи "DAO без legal wrapper". `Security Council` остаётся emergency-only органом, а не центром day-to-day monetary-policy decisions. [C1][C2][C3][C14]

Полное описание `AGC` как инструмента среды остаётся в `→ L2/D05 §2–§3`, а regulatory perimeter `EMT` issuer path - в `→ L2/D08 §3.1`. Здесь фиксируется governance-слой: кто именно получает правомочие представлять monetary-policy decisions вовне, кто назначает и ограничивает этот слой, и при каких условиях authority может постепенно сдвигаться в сторону DAO without bypassing external accountability. [A24][A25][A86][C4][C5][C12]

| Wrapper candidate | Recognition of DAO governance | EU-facing `MiCA` credibility | Bankability | Governance flexibility | Tax / nonprofit fit | Residual risk |
|-------------------|-------------------------------|-------------------------------|-------------|----------------------|---------------------|---------------|
| **Wyoming DAO LLC** | явное statutory recognition для algorithmic/member-managed governance | ограниченная пригодность для EU-facing `EMT` perimeter; US-centric structure | умеренная в evidence pack | высокая | pass-through или corporate US tax posture | слабее precedent density для MiCA-facing wrapper path [C6][C11] |
| **Marshall Islands DAO LLC / MIDAO** | явное statutory recognition для token-based governance | ограниченная EU-facing credibility | конфликтное evidence: legal capacity есть, практическая bankability остаётся слабой | высокая | offshore / tax-light posture возможен | bankability и correspondent-banking risk остаются открытыми [C7][C10] |
| **Switzerland Foundation / Verein** | governance можно связать с DAO через bylaws / charter, но структура менее natively DAO-shaped | сильная institutional credibility для EU-facing posture | сильнее других вариантов по evidence pack | средняя: Foundation жёстче, Verein гибче | non-profit fit и institutional legitimacy сильны | direct Swiss P1 statute text в этом пакете не был independently fetched [C8][C15] |
| **Cayman Foundation Company** | strongest practice precedent для DAO-directed constitutional control | пригодна как bounded wrapper candidate для MiCA-facing path, но offshore reading требует counsel validation | хорошая по evidence pack | высокая: directors/supervisor structure хорошо сочетается с DAO mandate | tax-neutral | direct Cayman P1 statute text в этом пакете не был independently fetched; final regulatory fit needs legal opinion [C9][C15] |

Для bounded governance posture operationally strongest reading сейчас выглядит так: legal-wrapper layer исполняет внешние legal acts и day-to-day monetary-policy authority в пределах DAO-defined mandate, а сама DAO сохраняет appointment, removal и constitutional-control surface вместо direct operational execution [A24][A25][A86]. Если N4Y.AI оптимизирует выбор под precedent density для MiCA-facing `EMT` path, наиболее сильным кандидатом выглядит `Cayman Foundation Company`; если приоритетом становится EU-facing institutional credibility и bankability, наиболее сильной альтернативой остаётся Switzerland. Ни один из этих вариантов не должен считаться committed incorporation choice до внешнего юридического заключения. [C5][C8][C9][C10][C15]

> **Допущение [A87, proposal]:** Preferred jurisdiction для N4Y.AI DAO legal wrapper = **Cayman Islands Foundation Company** (strongest precedent density: ENS, MakerDAO, Optimism; permissive governance; no distribution mandate; global banking access; supports DAO appointment/constitutional control without individual UBO disclosure requirements). Switzerland Foundation = EU-credibility alternative if MiCA EMT issuer requires EU-physical-presence governing body. Marshall Islands MIDAO и Wyoming DAO LLC = secondary options с material bankability and regulatory-recognition limitations. Final jurisdiction selection requires external legal counsel confirmation before moving to commit. [A87]

---

## 2. veQi как механизм Qi Chamber

### 2.1 Что именно принадлежит D08, а что нет

`veQi` как governance-механизм принадлежит `L2/D09`, потому что отвечает на вопрос, как именно распределяется voting power в `Qi Chamber`. Но сам актив `Qi Energy`, его issuance, dual-token boundary и `Revenue Distribution` принадлежат `→ L2/D05 §2–§3`. Поэтому в этой секции описывается не весь economics stack, а только transformation rule: как `Qi` превращается в governance weight. [C3][C4]

### 2.2 Базовая механика

В `v1` сохраняется базовая формула [PREVIOUS VERSION], потому что она согласуется с retained corpus и glossary:

```text
veQi = Qi_locked × (time_remaining / max_lock)
max_lock = 4 года
min_lock = 1 месяц
```

Это означает три governance-последствия:

1. voting power является time-weighted, а не только capital-weighted;
2. право голоса линейно убывает к дате разблокировки;
3. власть в `Qi Chamber` становится дороже для краткосрочного заимствования и spot-acquisition. [C3]

`veQi` канонически остаётся non-transferable, а значит не может использоваться как легко перемещаемый governance-instrument поверх краткоживущих позиций. Это главный reason, по которому corpus считает vote-escrow baseline наиболее правдоподобной защитой от flash-loan style governance acquisition. [C3][C8][C9]

### 2.3 Lock, snapshot-защита и открытые пределы

Для continuity from [PREVIOUS VERSION] в draft сохраняются `activation delay = 48h` после lock и привязка reward-side логики к реальному cash-flow среды, а не к чисто инфляционному mining. Под "реальной доходностью" здесь понимается не monetary policy, а связь locked governance power с реальным потоком ценности owner-слоя `→ L2/D05 §3`, где уже описан long-term alignment contour для locked `Qi` / `veQi`. [C3][C4][C5]

Retained evidence не доказывает, что `veQi` автоматически устраняет все capture-risks, но подтверждает более узкий тезис: vote-locking повышает acquisition cost и даёт governance системе "память" о времени, а не только о размере позиции. Поэтому `veQi` остаётся baseline `Qi Chamber`, тогда как `OI-023` оставляет открытым спор о quadratic overlay, а `OI-034` - вопрос liquidity impact, dual lock competition и attack-cost profile при разных распределениях lock durations. [C3][C7][C8][C13]

---

## 3. Security Council

### 3.1 Роль Security Council в governance-стеке

`Security Council` в N4Y.AI не должен быть третьей обычной палатой и не должен подменять DAO. Его функция уже и жёстче: это emergency-backstop для периода, в котором среда ещё не достигла зрелой distributed operability. Именно поэтому retained evidence поддерживает модель "узкий совет с сильными emergency powers, но не с открытым ordinary mandate". [C5][C9][C12]

В `v1` базовая конструкция сохраняет ядро [PREVIOUS VERSION] и glossary:

- **12 членов**, разделённых на **2 когорты по 6**;
- **9-of-12** для emergency actions;
- **7-of-12** для non-emergency operational actions;
- staggered rotation как защита от одномоментной полной замены состава. [C12]

### 3.2 Состав, полномочия и lifecycle

Корпус показывает, что production evidence по `Security Council` стало заметно зрелее: у внешних референсов появились key-rotation pathways, lower nomination barriers, crisis drills и более явные role-diverse membership rules. Но сам N4Y-specific threshold всё равно остаётся design choice; выбор `12` и `9-of-12` в `v1` оправдан тем, что emergency authority не должна концентрироваться в совсем малой группе, простой majority недостаточен для protocol-wide effect, а реагировать всё равно нужно быстрее, чем full bicameral process. [C9][C12]

Draft сохраняет рабочую идею двух когорт, но делает её более нейтральной к конкретным именованным квотам. Критичен не столько exact constituency math, сколько composition rule:

| Принцип состава | Зачем нужен |
|-----------------|-------------|
| **техническая компетенция** | emergency-патчи и incident judgment не должны зависеть только от политического веса |
| **governance legitimacy** | совет не должен превращаться в чисто operator-appointed комитет |
| **role diversity** | нужен mix безопасности, операций, legal/compliance и ecosystem-представителей |
| **географическая и организационная диверсификация** | снижает correlated compromise и reputational capture |

Отсюда следует рекомендуемая формула отбора для `v1`: dual-chamber election or ratification, ограничения на концентрацию членов из одной организации, публичные conflict-of-interest disclosures и обязательная ротация signing keys. Легитимная область совета уже ordinary governance: emergency pause, hotfix execution, emergency veto/backstop в пределах timelock и принудительная эскалация proposal в более высокий governance-class, если риск был занижен. Обычная legislative function у совета отсутствует; иначе `Progressive Decentralization` превращается в декларацию без реальной передачи власти. [C5][C8][C9][C12]

По `G11` retained evidence достаточно сильна, чтобы зафиксировать baseline lifecycle: две когорты избираются со смещением по времени, ранний cadence review составляет **6 месяцев**, а removal идёт либо через governance vote с повышенным participation bar, либо через internal council supermajority в случае compromise. Эта конструкция соответствует логике Phase 1 anchor, где важнее частая переоценка состава и key hygiene, чем максимальная продолжительность мандата. [C5][C12]

### 3.3 Классы действий Security Council

Чтобы совет не расползался в обычный policy-center, `v1` полезно зафиксировать минимальное деление его действий:

| Класс | Примеры | Кто подтверждает постфактум |
|-------|---------|-----------------------------|
| **Emergency action** | pause, hotfix, freeze emergency pathway | публичный incident report + последующий governance review |
| **Emergency veto** | остановка proposal в timelock при доказанном critical risk | обе палаты в следующем governance-цикле |
| **Operational maintenance** | узкие non-emergency security chores без смены конституции | `Qi Chamber` или designated review path |

Такое деление помогает удержать совет в исходной роли: не "правительство среды", а ограниченный контур экстренной стабилизации. [C9][C12]

---

## 4. AIP-процесс и типы решений

### 4.1 Почему N4Y.AI нужен не один proposal type

Нормализованный корпус прямо отмечает, что конкретная `AIP`-таксономия N4Y.AI остаётся в значительной степени internal design choice, хотя и согласуется с production practice: низкорисковые изменения должны проходить проще, а high-impact changes - через более широкую legitimacy surface и более длинные timelocks. [C4]

Поэтому `v1` сохраняет три типа `AIP` из [PREVIOUS VERSION]: `Parameter (A)`, `Structural (B)` и `Constitutional (C)`. Это не "три слова для одного и того же", а способ разделить скорость governance, breadth of approval и rollback risk. [C4]

### 4.2 Рабочая матрица AIP

| Тип | Что меняется | Инициатор | Палаты | Порог / кворум | Timelock |
|-----|--------------|-----------|--------|----------------|----------|
| **Parameter (A)** | настраиваемые параметры, не меняющие конституцию среды | `veQi`-holder с минимальным порогом подачи | `Qi Chamber` | `Qi quorum 4%` | `48h` |
| **Structural (B)** | структура программ, routing funds, scope палат, sandbox-authority | повышенный threshold подачи | обе палаты | `Qi quorum 10% + >50%`, `Citizens' quorum 5%` | `7d` |
| **Constitutional (C)** | core governance architecture, emergency powers, constitutional invariants | самый высокий threshold подачи | обе палаты | `Qi quorum 20% + >66%`, `Citizens' quorum 10% + >50%` | `14d` |

Эти thresholds в `v1` следует читать как draft-stage baseline, а не как externally validated optimum. Corpus поддерживает differentiated governance, но не даёт production DAO с идентичной three-tier stack; значит, сами числа ещё должны пройти threat-model и participation-model проверку. [C4][C8][C13]

### 4.3 Proposal flow и границы суверена

Для continuity from [PREVIOUS VERSION] proposal pipeline фиксируется как `forum discussion -> классификация A/B/C -> chamber vote -> timelock -> execution или veto/escalation`. Здесь важен не exact tooling, а rule of escalation: proposal не должен сам выбирать себе "удобный" класс риска, и при споре о классификации `Security Council` или chamber chairs должны иметь право отправить его в более высокий governance-class. [C1][C4][C8][C9]

При этом `AIP` не следует читать как абсолютный суверен: economic owner-детали `Qi`, `AGC` и `Revenue Distribution` закрепляются в `→ L2/D05 §2–§3`, legal wrapper и boundary с on-chain governance - в `→ L2/D08 §7.5`. Ordinary protocol rules меняются через `AIP`, emergency containment идёт через `Security Council`, а supervisory constraints не отменяются simple majority голосованием. [C4][C5]

---

## 5. Citizens' Chamber и Sybil resistance

### 5.1 Что Citizens' Chamber должна добавлять governance, а не дублировать

`Citizens' Chamber` не нужна для того, чтобы просто "повторить" `Qi Chamber` другими участниками. Её задача - ввести non-token legitimacy layer там, где среда принимает решения о public goods, safety priorities и sandbox-policy, а не только о распределении economic incentives. Это и есть governance role второй палаты. [C1][C2][C10]

Полное описание `PoP` как identity-layer принадлежит `→ L1/D01 §3`, а security-side Sybil protection принадлежит `→ L2/D07 §2`. Здесь, в `D08`, фиксируется только управленческая функция этой идентичности: кто получает право на `1 человек = 1 голос` и при каких условиях этот голос становится конституционно значимым. [C10]

### 5.2 Working design note: World ID 4.0 + MACI v3

С точки зрения available building blocks `v1` исходит из того, что наиболее сильный текущий набор компонентов выглядит так:

- `World ID 4.0` как production-scale PoP building block;
- `MACI v3` как privacy and anti-collusion voting layer;
- credential-aggregation fallback or supplement через `Human Passport`-style stack. [C10]

Это не означает, что N4Y.AI уже закрыла выбор final stack. Напротив, retained corpus подчёркивает, что building blocks имеют разные trust assumptions, UX-costs, regulatory exposure и anti-Sybil strength. Поэтому `G9` должен считаться **resolve-in-draft, but not finalized**. [C10]

### 5.3 Дизайн-решение G9, которое ещё не закрыто

Для governance `Citizens' Chamber` мало просто проверить "один человек = один аккаунт". Credible chamber membership требует personhood verification, privacy-preserving vote submission, revocation path и защиты от bribery/collusion; именно поэтому `MACI` важен не как optional privacy feature, а как отдельный слой governance-integrity поверх `PoP`. [C9][C10]

`v1` сознательно не объявляет окончательную PoP-конфигурацию. Вместо этого фиксируется design note:

| Компонент | В draft считается предпочтительным | Что остаётся открытым |
|-----------|------------------------------------|-----------------------|
| **PoP bootstrap** | `World ID 4.0`-class system | biometric and jurisdictional exposure |
| **fallback / aggregation** | `Human Passport`-style credential aggregation | false positives, farming, UX friction |
| **private voting** | `MACI v3` | cost, operational complexity, final gatekeeper choice |

Следовательно, Citizens' Chamber в `v1` не должна считаться fully general-purpose house from day one. Более безопасный baseline - **Phase 2 launch с ограниченным scope**, сначала вокруг grants/public goods и safety-linked veto surfaces, а расширение полномочий выносить в `Structural AIP`. Это напрямую связано с `OI-065`. [C10][C11]

---

## 6. Межпалатные конфликты и governance-атаки

### 6.1 Почему G10 остаётся отдельным design problem

Нормализованный корпус даёт необычно ясный вывод: production evidence уже показывает veto windows, optimistic approval и adaptive quorum, но **не показывает устойчивый precedent repeated bilateral deadlock resolution между двумя truly co-equal chambers**. Поэтому `G10` нельзя считать "закрытым внешним примером". Он закрыт только как informed design space, а не как copied mechanism. [C11]

Из этого следует главный методический вывод `v1`: межпалатный конфликт нельзя оставлять на уровне "разберёмся политически". Он должен быть заранее разложен по процедурам, даже если сами thresholds ещё остаются открытыми. [C11]

### 6.2 Рабочая схема разрешения конфликта и layered defense

Для N4Y.AI рекомендуемая baseline-схема выглядит так: `optimistic approval` для routine decisions, `veto window` для второй палаты, затем `conference committee` как обязательная стадия компромисса; fallback default задаётся жёстко: routine and reversible changes -> default action, constitutional or safety-critical changes -> default inaction. `Conference committee` здесь - сознательная design adaptation из классического bicameral logic, а не claimed production-standard DAO feature. [C1][C11]

Корпус для `G8` подчёркивает, что governance-атаки не исчезли вместе с ростом зрелости DAO: credible failure modes включают whale capture, low-turnout capture, signer social engineering, governance-parameter drift и patient accumulation. Поэтому защита должна быть layered: `veQi` повышает acquisition cost, `MACI` уменьшает выгоду bribery and collusion, timelocks дают время на реакцию, `Security Council` остаётся emergency veto backstop, а signer-opsec и key-rotation защищают emergency layer. [C8][C9][C12]

### 6.3 Два кейса, которые нельзя игнорировать

Для `v1` достаточно зафиксировать два свежих и показательных failure modes:

| Кейс | Что произошло | Вывод для N4Y.AI |
|------|---------------|------------------|
| **Moonwell (2026)** | дёшево накопленная governance-позиция позволила провести враждебное предложение с disproportionate payoff | низкая стоимость захвата при слабом turnout делает timelock без backstop недостаточным |
| **Drift (2026)** | атака на signer/process layer показала, что social engineering и zero-timelock migration могут быть опаснее, чем обычный smart-contract bug | governance security = не только voting math, но и opsec emergency actors |

Эти примеры важны именно потому, что показывают разные поверхности угрозы. `Moonwell` - про acquisition economics и turnout. `Drift` - про signer compromise, process integrity и риск "обхода" формального governance через атакованный emergency path. [C8][C9]

### 6.5 OI-035 как незакрытый риск

> **Scoping note (OI-035/OI-036):** OI-035 (governance attacks threat model) и OI-036 (attack cost modeling / quorum calibration) зарегистрированы как open. Они не блокируют публикацию D08 v4.0 — D08 описывает текущий design baseline с обоснованными draft-значениями. Quorum числа в §4 помечены как `"draft baseline (pending attack-cost modeling per OI-036)"`. Финализация thresholds через governance (AIP-A) после закрытия OI-036. Аналогично OI-023 (veQi vs QV) и OI-025 (DAO legal wrapper) — open research/expert questions, не design blockers.

`OI-035` требует отдельного анализа attack-cost modeling и quorum calibration. До его закрытия конкретные threshold числа остаются draft baseline, не финальными значениями:

- quorum values для `AIP-B` и `AIP-C`;
- minimum proposer thresholds;
- delegated veto logic;
- attack-cost assumptions для low-turnout windows. [C8][C11]

Отдельно связан и `OI-036`: even strong governance structure нуждается в attack-cost modeling, а не только в качественном описании угроз. [C8]

---

## 7. Sandbox Councils, TAC и Conviction Voting

### 7.1 Sandbox Councils и TAC

Retained evidence поддерживает общий тезис: specialist governance bodies defensible, но внешне валидированы лишь частично. Это значит, что `Sandbox Councils` и `TAC` допустимы как N4Y-specific design, но их authority должна быть узкой, отчуждаемой и подотчётной. `Microsandbox` как объект принадлежит `→ L2/D01 §2`; здесь фиксируется только governance projection: каждый отраслевой контур среды может иметь свой `Sandbox Council`, если требуется domain-specific policy and escalation. [C6]

Для `v1` мандат `Sandbox Councils` сводится к локальным sandbox-parameters, domain guardrails, incident escalation и управлению делегированными public-goods budgets в пределах выданного мандата; такой council не должен становиться "мини-DAO внутри DAO", потому что его authority делегирована сверху, ограничена по домену, обратима через `Structural AIP` и не отменяет полномочий палат и `Security Council`. [C4][C6]

`TAC` в `v1` читается как экспертный фильтр, а не как скрытая верхняя палата: structured technical judgment по safety benchmarks и AI-specific review допустим, самостоятельный legislative veto - нет. Поэтому каноническое положение органов в этом draft-е такое:

| Орган | Роль | Почему не является owner of final authority |
|-------|------|----------------------------------------------|
| **TAC** | recommendation, review, benchmark interpretation | экспертность не заменяет legitimacy |
| **Sandbox Councils** | delegated domain policy | authority выдана сверху и может быть отозвана |
| **Security Council** | emergency containment | narrow emergency mandate, not ordinary policymaking |

### 7.2 Conviction Voting и G12

`Conviction Voting` в retained pack поддерживается как plausible mechanism для non-binary and lower-salience decisions, но evidence сильнее по delegation/incentive design, чем по свежим production deployments. Поэтому `v1` использует его ограниченно: grants, public-goods allocation и sandbox-budget prioritization, но не `AIP-C`. В лучшем случае он работает как pre-legislative signal или budget-routing mechanism, особенно внутри `Citizens' Chamber`. [C4][C7][C13]

Корпус по `G12` при этом даёт практический вывод: turnout улучшается не одной наградой, а связкой delegation, predictable cadence, focal governance events, infrastructure support и carefully bounded incentives. Значит, `Qi Chamber` должна поощрять регулярное участие без превращения voting в mercenary yield game, `Citizens' Chamber` нуждается в low-friction tooling, а любые прямые rewards следует привязывать к governance-quality metrics, а не только к факту клика "за/против". [C13][C14]

---

## 8. Progressive Decentralization roadmap

### 8.1 Почему roadmap принадлежит именно D08

`Progressive Decentralization` по ownership-map принадлежит `L2/D09`, потому что отвечает не просто на вопрос "насколько децентрализована технология", а на вопрос "кто именно принимает решения на разных стадиях зрелости среды". Технические аспекты децентрализации могут жить и в других измерениях, но governance transfer logic владеется здесь. [C5]

### 8.2 Фазы 1 -> 4

В `v1` сохраняется четырёхфазный baseline [PREVIOUS VERSION], но он переписан под evidence language:

| Фаза | Кто реально управляет | Что уже decentralizes | Что ещё остаётся узким местом |
|------|------------------------|-----------------------|-------------------------------|
| **Phase 1** | founder/operator core + `Security Council` + ранний `Qi Chamber` | базовое vote layer, emergency transparency, limited delegation | emergency authority, membership design, legal wrapper |
| **Phase 2** | `Qi Chamber` + ограниченный `Citizens' Chamber` + councils | grants/public goods, часть structural governance, chamber legitimacy experiments | PoP stack selection, chamber scope, attack-cost calibration |
| **Phase 3** | полноценная bicameral DAO | ordinary `A/B/C` governance, stable delegated organs, richer veto logic | constitutional hardening, anti-capture monitoring |
| **Phase 4** | constitutional DAO with minimized operator role | core rule changes through mature governance, ossification of selected invariants | over-ossification risk, rare emergency fallback |

Главный смысл этой таблицы в том, что децентрализация здесь не one-shot event. В production governance 2025-2026 authority обычно переходит по слоям: сначала появляются stake-weighted controls, потом усиливается community legitimacy, и лишь затем emergency/operator powers сжимаются до исключительного режима. [C5][C12]

### 8.3 Сигналы перехода и comparative patterns

Корпус не даёт универсального KPI, который автоматически "включает" следующую фазу, поэтому `v1` использует пакет критериев: распределение и lock-depth `veQi`, устойчивый turnout в `Qi Chamber`, доказанную operability `Citizens' Chamber`, incident-response maturity `Security Council` и governance-attack resilience без частых ручных вмешательств. Это согласуется и с более общим project-level сравнением decentralization trajectories: кейсы вроде `XMTP`, `Starknet` и `The Graph` показывают, что реальный переход обычно идёт не "по одной кнопке", а как пакет отдельных lanes - operators, staking/economic security, governance and service modularity. Для `L2/D09` вывод не в копировании этих roadmaps, а в том, что `Phase 1` с узким backstop не противоречит decentralization trajectory, если у неё есть понятные conditions for transfer and shrinkage of special powers. [C5][C8][C12][C13]

### 8.5 Что должно быть явно отложено

В `v1` нельзя делать вид, что `Progressive Decentralization` автоматически переносит external legal authority внутрь on-chain governance. Как минимум должны оставаться explicitly open:

- final timing и scope `Citizens' Chamber`;
- final jurisdiction choice для legal-wrapper layer, пока не закрыты вопросы regulatory credibility, bankability и governance enforceability;
- любой тезис о том, что `EMT` authorization или связанные supervisory obligations автоматически переходят от legal-wrapper layer к DAO successor;
- final supremacy order между bicameral DAO, legal-wrapper layer и `Security Council` вне narrow emergency context. [A24][A25][A86][C10][C12][C13][C15]

Именно поэтому `Progressive Decentralization` в этом draft-е формулируется не как self-executing обещание "wrapper исчезнет сам", а как управленческая траектория передачи authority, где каждый следующий шаг допустим только после closure юридической continuity boundary. [C12][C13]

### 8.6 Stop-rules для перехода между фазами

Чтобы roadmap не превращался в narrative without brakes, `v1` фиксирует и юридические stop-rules перехода:

- среда **не** должна переходить к следующей фазе, если day-to-day `AGC` monetary-policy authority ещё не опирается на legal-wrapper layer с traceable DAO appointment/mandate chain [A24][A86];
- среда **не** должна переходить к следующей фазе, если выбранная юрисдикция ещё не подтверждена внешним юридическим заключением по representational capacity, bankability и MiCA-facing accountability path [A86][C8][C9][C10][C15];
- среда **не** должна переходить к следующей фазе, если transition logic предполагает automatic transfer of `EMT` authorization, supervisory duties или contractual obligations к DAO без formal successor or re-authorization path; current evidence, напротив, подтверждает отсутствие такого механизма [C12][C13];
- среда **не** должна переходить к следующей фазе при повторяющихся emergency interventions `Security Council`, потому что это означает, что ordinary authority ещё не стабилизировалась внутри bicameral governance [A25][C13];
- среда **не** должна переходить к следующей фазе при устойчиво низкой явке в `Qi Chamber`, формальности `Citizens' Chamber` или незакрытых high-severity governance-issues по `OI-023` / `OI-035` / `OI-036`. [C13]

Эти stop-rules важны потому, что premature transfer of authority сам по себе является governance-risk, а не доказательством зрелости. Иными словами, roadmap задаёт не только траекторию передачи власти, но и условия допустимой паузы, пока legal continuity не стала управляемой, а не подразумеваемой. [C12][C13]

---

## Связь с другими измерениями

| Измерение | Связь |
|-----------|-------|
| **L2/D01 (Онтология)** | `L2/D01` владеет managed boundary среды и `Microsandbox`-логикой. В `D08` они используются как governance-context: кто задаёт правила среды и кто делегирует sandbox-policy. Подробности среды и границ: `→ L2/D01 §2`. |
| **L2/D02 (Функционально-целевое)** | Маркетплейсы и типология агентов определяют функциональную структуру среды: `→ L2/D02 §1–§3`. |
| **L2/D05 (Экономико-стимулирующее)** | `Qi Energy`, `Revenue Distribution` и economic alignment потоки принадлежат `L2/D05`. `D09` использует их как governance substrate: `veQi` превращает `Qi` в voting weight, а public-goods funding и long-term yield привязаны к owner-логике `→ L2/D05 §3`. |
| **L2/D07 (Безопасность)** | `Sybil`-protection, private voting security layers и defense-in-depth не описываются здесь полно. `D08` использует их только как governance safeguards для chamber integrity и anti-capture. Подробности: `→ L2/D07 §2`, `→ L2/D07 §8`. |
| **L2/D08 (Регуляторное)** | Legal wrapper DAO, bounded decentralization reading и supervisory boundary принадлежат `L2/D08`. В `D08` governance описывается как owner-layer правил среды, но не как произвольный override над правовым периметром. Подробности: `→ L2/D08 §7.5` (DAO legal wrapper jurisdiction projection). |
| **L1/D01 (Идентификационное)** | `PoP` и personhood-binding для `Citizens' Chamber` опираются на owner-слой идентичности `→ L1/D01 §3`. `D08` использует этот слой только как критерий политической легитимности второй палаты. |
| **L1/D09 (Репутационное)** | Репутация не заменяет governance weight, но влияет на participation quality, delegation trust и доменно-специфическую легитимность участников. Переходы между качеством участия и reputation-profile описываются в `→ L1/D09 §1` и `→ L1/D09 §5`. |
| **L1/D07 (Автономность)** | Governance non-delegable actions и человеческий контроль над голосованием нельзя описывать без owner-границы автономности. Поэтому любые попытки делегировать chamber voting, Security Council elections или self-authority expansion должны читаться через `→ L1/D07 §4`. |

---

## Открытые вопросы

### Требуют решения (Critical / High)

1. **[High] OI-023: veQi vs Quadratic Voting в Qi Chamber.** Draft фиксирует `veQi` как baseline, но не закрывает вопрос о quadratic overlay или cap.
> **Open.** Нужны attack-cost modeling и turnout sensitivity analysis, прежде чем превращать hybrid design в canonical rule. Связанные секции: `§2`, `§4`. [C3][C8]

2. **[High] OI-025: юридическая оболочка DAO и выбор юрисдикции.** Governance architecture не может считаться финальной без правовой формы, через которую она будет распознана внешним правом.
> **Open.** Draft удерживает governance как owner-layer среды, но legal wrapper boundary сознательно вынесен в `→ L2/D08 §7.5`. До formal legal opinion final constitutional scope и jurisdiction choice остаётся условным (ESC-027/028). [C4][C5]

3. **[High] OI-035: governance-атаки должны рассматриваться как P1 threat model.** После свежих кейсов считать их "вторичным риском" нельзя.
> **Open.** Нужно синхронизировать governance design с security owner-слоем: acquisition cost, signer compromise, low-turnout capture, emergency-path abuse. Связанные секции: `§6`, `→ L2/D07 §8`. [C8][C9]

4. **[High] OI-036: математическое моделирование attack cost и quorum calibration.** Без него thresholds `AIP-B/C` остаются только draft baseline.
> **Open.** Для финализации нужны сценарии по распределению `veQi`, turnout bands и стоимости hostile accumulation. Связанные секции: `§2`, `§4`, `§6`. [C8][C13]

### Требуют исследования (Medium)

5. **[Medium] OI-062: binding authority hierarchy.** Кто имеет окончательную binding authority при конфликте между DAO, `Security Council`, operator-layer и legal wrapper?
> **Open.** `v1` описывает роли, но не закрывает final supremacy order. Это отдельное constitutional решение на стыке `D08` и `D07`. [C4][C5]

6. **[Medium] OI-063: governance non-delegable core.** Список неделегируемых governance-действий ещё должен быть синхронизирован с owner-логикой автономности.
> **Open.** Пока безопасный минимум такой: chamber voting, delegation of voting power, участие в выборах `Security Council` и саморасширение governance authority не должны становиться autonomous-by-default. Полная owner-граница: `→ L1/D07 §4`. [C9]

7. **[Medium] OI-064: форма участия ИИ-агентов в governance.** Должны ли агентные системы иметь advisory role, execution role или быть полностью исключены из direct voting?
> **Open.** Current draft исходит из human-backed governance legitimacy, а не из самостоятельного voting права Логоса. Но advisory analytics and tooling остаются допустимым design space. [C10][C13]

8. **[Medium] OI-065: timing и scope Citizens' Chamber.** Запускать ли её сразу, с grants-only scope в `Phase 2`, или только после большего числа устойчиво верифицированных участников?
> **Open.** Текущий draft выбирает консервативную линию: limited launch с public-goods и safety-linked scope, расширение через `Structural AIP`. Связанные секции: `§5`, `§8`. [C10][C11]

9. **[Medium] G9 design decision: final PoP stack selection.** Какой набор `PoP + private voting + fallback credentials` станет production baseline для Citizens' Chamber?
> **Open.** `World ID 4.0 + MACI v3` выглядит сильным working candidate, но regulatory exposure, operational complexity и fallback logic ещё не закрыты. [C10]

10. **[Medium] G10 design decision: final deadlock procedure.** Как exactly codify conference committee, veto windows и fallback default?
> **Open.** Production precedent для recurring bilateral deadlock остаётся слабым, поэтому точный порядок needs explicit constitutional drafting before finalization. [C11]
