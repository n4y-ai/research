# УРОВЕНЬ 1 / ИЗМЕРЕНИЕ 09: СОЦИАЛЬНОЕ (РЕПУТАЦИОННОЕ)

> **Уровень:** L1 — Агент (свойства Логоса)  
> **Измерение:** D06 — Reputation / Social  
> **Версия:** 4.0  
> **Статус:** Финал v4.0 | Дата: 02.04.2026  
> **Зависимости:** L1/D01, L1/D02, L1/D04, L1/D05, L1/D07, L3/D09

---

## Ключевой вопрос

**Как тебя оценивает сообщество и на основе чего строится доверие других к тебе?**

---

## Аннотация

Прямой ответ на вопрос измерения таков: сообщество оценивает Логос не по одному "рейтингу", а по репутационному профилю из пяти компонентов: `TP`, `PF`, `CC`, `ER`, `DE`. Публикуемая L1-форма этого профиля должна оставаться интерпретируемой и переносимой: `Performance VC` выступают канонической единицей репутационных данных, `EAS` и `ERC-8004` дают on-chain якорь и discovery-friendly публикацию, `ERC-5192` даёт непередаваемый tier-badge, а richer contextual scoring и trust interpretation могут происходить вне одной цепочки. [C1][C2][C3][C6][C10]

По сравнению с `v0_legacy` этот черновик сохраняет базовое ядро прежней версии: мультикомпонентную модель, lifecycle репутации, anti-manipulation, cold start, tiers и privacy-boundary. Но теперь всё уложено в новый template и ownership-дисциплину: `ESq` остаётся owned concept `→ L1/D02 §5`, `LOA` и earned autonomy остаются owned concept `→ L1/D07 §3`, доверительный propagation-алгоритм и physics trust-сети вынесены в `→ L3/D09 §2-§4`, а криптографический privacy stack для selective disclosure остаётся проекцией `→ L2/D07 §8`. [C6][C8][C9][C12][C15]

---

## 1. Мультикомпонентная репутационная модель

### 1.1 Почему D06 не сводится к одному числу

Корпус для `L1/D09` последовательно показывает, что один универсальный scalar score недостаточен для ответа на ключевой вопрос измерения. Production scoring, vector reputation, multidimensional incentive mechanisms и context-dependent operational tracking все сходятся в одном: доверие к агенту формируется из разных типов сигналов, которые нельзя без потерь схлопнуть в "среднюю температуру". Поэтому D06 фиксирует мультикомпонентную модель как канонический L1-ответ, а не как optional enhancement. [C3][C6][C16]

Для L1 при этом нужен и второй принцип: сложность внутреннего расчёта не должна ломать portability и читаемость внешнего профиля. Поэтому текущий draft разделяет публикуемый L1-profile в шкале `0-100` и более богатые internal/off-chain engines, которые могут считать промежуточные values в иной шкале до нормализации.

> **OI-043 note.** Конфликт между `0-100` и `0-1000` не считается закрытым: included evidence реально поддерживает оба паттерна. В этом черновике D06 использует `0-100` как current publication layer, потому что она согласуется с `ERC-8004`, текущим glossary baseline и tier logic; governance всё ещё должен решить, является ли это окончательной end-to-end шкалой или только публичной нормализацией более богатого внутреннего расчёта. [C1][C4][C6]

### 1.2 Пять компонентов репутации

Репутационный профиль Логоса собирается из пяти компонент, каждая из которых отвечает на отдельный аспект социального доверия:

| Компонент | Что измеряет | Основной тип сигнала | Роль в доверии |
|-----------|--------------|----------------------|----------------|
| **TP — Task Performance** | Насколько надёжно и качественно Логос выполняет задачи | `Execution Trace`, objective outcomes | Базовый behavioral record |
| **PF — Peer Feedback** | Как оценивают взаимодействие контрагенты | `A2A`-связанный feedback, post-task review | Социальный слой отзывов |
| **CC — Certified Capability** | Какие capabilities подтверждены внешними аттестациями | benchmark, audit, capability attestations | Сертифицированный слой доверия |
| **ER — Economic Reliability** | Насколько Логос надёжен в платёжно-договорной дисциплине | settlement/dispute/payment signals | Контрактно-экономическая надёжность |
| **DE — Domain Expertise** | Насколько глубока репутация внутри конкретного домена | domain-bounded history and credentials | Специализация, а не общий prestige |

Такое разбиение сохраняет архитектурное зерно `v0_legacy`, но переводит его в более строгую форму. `TP` и `PF` отвечают за историческое исполнение и социальную оценку, `CC` не даёт смешивать сертификацию с crowd feedback, `ER` не растворяет contractual discipline в общей "успешности", а `DE` защищает систему от ложного переноса репутации между несвязанными доменами. [C3][C5][C6][C10]

### 1.3 Базовая формула и веса

Практический L1 baseline должен быть интерпретируемым и governance-tunable, поэтому D06 фиксирует простую weighted model как publication logic:

```text
ReputationScore(domain) =
  0.30 * TP +
  0.20 * PF +
  0.20 * CC +
  0.15 * ER +
  0.15 * DE
```

Эти веса не претендуют на final mathematical optimum; они лишь выражают L1-principle: наибольший вес получает observed performance, certified evidence и social feedback остаются самостоятельными слоями, economic discipline не доминирует над task quality, а domain expertise не подменяет recent execution. Более сложная trust propagation и weighting of raters не описываются здесь fully, потому что physics сетевого доверия принадлежат `→ L3/D09 §2-§4`. Здесь D06 владеет не алгоритмом распространения trust по графу, а semantic anatomy входного reputation profile. [C4][C5][C6]

### 1.4 Доменно-специфический характер score

Логос не должен получать безусловно один и тот же "социальный вес" во всех контекстах. Доверие в travel, devops, research или moderation должно читаться как разные проекции одного агента, а не как один transferable prestige-token. Именно поэтому `DE` и component isolation встроены в ядро D06, а не вынесены в optional analytics. [C3][C5][C16]

Практический вывод таков: базовая репутация публикуется как domain-bounded profile, cross-domain transfer допускается только как ограниченная проекция, а reputation outside domain never substitutes current evidence inside domain. Эта граница важна и для downstream-измерений: `L1/D07` использует tier thresholds как один из сигналов earned autonomy, но сами thresholds должны читаться как track-record within domain, а не как универсальная лицензия на автономию "везде"; `L1/D02` использует certified capability как вход в competence layer, но D06 не становится владельцем `ESq` или competence semantics. Подробности: `→ L1/D07 §3`, `→ L1/D02 §5`. [C3][C13][C16]

---

## 2. Performance VC и on-chain layer

### 2.1 Performance VC как каноническая единица репутации

Если ключевой вопрос D06 звучит как "на основе чего строится доверие других", то базовая единица ответа должна быть не сам итоговый score, а проверяемое evidence of performance. В `v1` такой единицей остаётся `Performance VC`: репутация должна строиться из переносимых attestations and status-aware records, а не только из локальной базы платформы. Это прямо поддерживается included evidence по attestation infrastructure и credential status management. [C2][C9][C12]

Минимальный практический набор типов `Performance VC` для D06:

| Тип | Триггер | Что фиксирует | Куда идёт |
|-----|---------|---------------|-----------|
| **Task Completion VC** | завершение задачи | objective execution outcome, trace hash, context | input в `TP` |
| **Peer Feedback VC** | post-task review | оценки контрагента и review metadata | input в `PF` |
| **Benchmark VC** | внешний capability test | результат benchmark run | input в `CC` и затем в `Agent Capability VC` |
| **Safety / Audit VC** | periodic audit | verified safety/capability findings | input в `CC` |
| **Incident VC** | dispute, failure, санкция | negative signal, corrective status | input в `ER` и penalties |

Такой pipeline согласуется с уже зафиксированным ремонтом границы между `D02` и `D06`: benchmark и audit events могут жить как исторические `Performance VC`, а агрегированный competence snapshot остаётся в `Agent Capability VC` у владельца `→ L1/D02 §4-§5`. D06 использует эти historical records как certified input, но не владеет competence ontology. [C2][C9]

### 2.2 Lifecycle: от события к публикуемой репутации

Репутационные данные должны иметь lifecycle, а не существовать как вечные неизменяемые marks. Поэтому D06 фиксирует короткий baseline: событие исполнения, взаимодействия или аудита порождает raw evidence, raw evidence оформляется как `Performance VC`, on-chain публикуется не весь payload, а anchor / attestation / score reference, off-chain хранится полный контекст, а устаревшие, оспоренные или revoked записи теряют влияние через status и decay. Здесь важны два ownership-boundary: privacy-preserving status handling поддерживается через `Bitstring Status List`, но детальный crypto/profile reasoning не раскрывается fully внутри D06; dispute-resolution mechanics как самостоятельный межагентный процесс тоже не принадлежат D06. D06 отвечает только за то, как эти события меняют reputation state. [C2][C12][C15]

### 2.3 Роли EAS, ERC-5192 и ERC-8004

В `v1` on-chain layer не выбирается как one-size-fits-all механизм. Корпус поддерживает dual-layer architecture, где разные стандарты решают разные подзадачи: [C1][C2][C6][C10]

| Слой | Что делает | Почему нужен |
|------|------------|--------------|
| **EAS** | schema-based attestation rail для score anchors, VC references и auxiliary attestations | production-proven attestation backend, пригодный для lifecycle и portability |
| **ERC-5192** | non-transferable tier badge | делает tier видимым как непередаваемый social badge |
| **ERC-8004** | lightweight public discovery / reputation registry | совместим с bounded on-chain score и multi-agent discovery |

Практически это означает следующее. `EAS` удобен как основной attestation substrate, потому что позволяет schema-based and high-scale publication. `ERC-5192` не заменяет score и не хранит весь profile; он лишь визуализирует текущий tier и тем самым превращает reputation в machine-readable и human-readable badge. `ERC-8004` нужен не как единственное хранилище всей репутационной логики, а как discovery-friendly публичный anchor для bounded score, registry semantics и межсетевого распознавания агента. [C1][C2][C6]

### 2.4 cheqd Trust Graph как динамический слой

Included evidence не поддерживает идею, что одна on-chain запись достаточно описывает социальное доверие к агенту. Поэтому поверх simple registry layer нужен richer contextual layer, и наиболее сильный included pattern здесь даёт cheqd: `ERC-8004` и `EAS` дают discovery and anchoring, `Performance VC` дают переносимые evidence bundles, а `Trust Graph` даёт contextual reading of who trusts whom and why. Отсюда и storage boundary: on-chain лучше держать attestation id, score anchor, tier marker, status pointer и evidence hash, а полный `Performance VC`, review payload, extended trace и dispute context - off-chain. Такой anchor-first подход лучше соответствует нынешнему evidence, чем full on-chain publication of every social signal. [C6][C9][C10][C12][C15]

---

## 3. Сигналы и агрегация

### 3.1 Базовое правило G12: агрегировать нужно сигналы, а не мнения "вообще"

`G12` был критическим именно потому, что прежняя модель оставляла неявным вопрос: из каких полей и по каким правилам строятся `TP`, `PF`, `CC`, `ER`, `DE`. Fresh evidence показывает более зрелый ответ: полезный reputation engine начинается с signal taxonomy, explicit normalization и publication thresholds, а не с opaque score. [C4][C5]

Для `L1/D09` это означает простую цепочку: исходные события нормализуются в signal classes, signals кладутся только в релевантный компонент, cross-component correlations допускаются только там, где они объяснимы, а final score публикуется лишь после достижения минимальной evidence density. Такой подход совместим и с production composite scoring, и с topic-aware vector models, и с context-dependent tracking; он также оставляет D06 explainable для downstream-измерений. [C3][C4][C5][C16]

### 3.2 Маппинг сигналов в пять компонентов

Ниже зафиксирован минимальный signal map для `v1`:

| Источник | Нормализуемый сигнал | Компонент | Cross-reference |
|----------|----------------------|-----------|-----------------|
| `Execution Trace` | success/failure, completion discipline, retries, outcome quality, trace completeness | **TP** | `→ L1/D04 §7` |
| `A2A`-связанный feedback | post-task ratings, structured review, vouch-like updates | **PF** | `→ L1/D05 §1`, `→ L1/D05 §4` |
| benchmark / audit attestation | capability proof, safety proof, recertification | **CC** | `→ L1/D02 §4-§5` |
| settlement / dispute history | payment discipline, breach frequency, dispute burden | **ER** | projection to contractual/economic layers |
| domain-bounded history | repeated success in same domain, long-tail specialization | **DE** | tied to local domain profile |

Здесь есть важная ownership-granularity. `Execution Trace` fully owned in `D04`, но его interpretive contribution к social trust fully owned here. Аналогично, `A2A` fully belongs to communication stack, но вопрос "как отзывы после A2A-взаимодействия меняют репутацию" принадлежит D06, а не D05. [C4][C5][C10]

### 3.3 Нормализация и publication thresholds

Практический scoring baseline должен сначала привести heterogeneous signals к общей форме, а потом уже умножать на веса. Для `v1` достаточно зафиксировать следующую normalization discipline:

```text
raw signal -> domain/context normalization -> component score 0..100 -> weighted publication score
```

Эта дисциплина означает:

- `TP` и `PF` не сравниваются в raw units;
- domain-specific variance сглаживается до component-local range;
- old and new evidence участвуют через decay, а не через вечное накопление;
- public score выпускается только после достаточного минимального объёма evidence.

В качестве текущего design note D06 принимает production-ориентированный порог публикации: публичный score должен показываться только после достижения meaningful evidence density, а не после первых одной-двух транзакций. Included evidence прямо поддерживает такую threshold-aware публикацию и frequent recomputation cadence. [C4][C11]

### 3.4 Component isolation и cadence пересчёта

Одна из самых сильных свежих идей для D06 - не просто multi-component score, а active component isolation. `DE` всегда считается domain-bounded, `PF` от одной domain cluster не должен без проверки отравлять другую, а cross-component correlation допустима только когда она объяснима. Одновременно D06 фиксирует смешанный cadence пересчёта: `TP` и `ER` могут обновляться near-real-time, `PF` лучше считать батчами после review window, а `CC` и `DE` требуют более редкого update cadence. Так D06 избегает двух крайностей: "универсального рейтинга на всё" и "застывшей репутации", не реагирующей на новые события. [C3][C4][C5][C10][C16]

---

## 4. Anti-manipulation и cold start

### 4.1 Defense-in-depth вместо одного "антифрода"

Fresh included evidence не подтверждает существование единственного механизма, который сам по себе делает reputation system нечувствительной к gaming, Sybil или collusion. Поэтому `v1` принимает layered anti-manipulation baseline: разные атаки адресуются разными слоями, а D06 сознательно избегает обещания "полной защищённости". [C7]

Минимальный defensive stack для D06 выглядит так:

| Атака | Базовый ответ |
|-------|---------------|
| **Sybil inflation** | economic friction, bounded benefit, decay-aware propagation |
| **collusive review clusters** | graph monitoring, cluster discount, pruning/detection |
| **vote rings / laundering** | topic-aware propagation and bounded transfer |
| **feedback gaming** | structured review window, thresholding, anti-spam controls |
| **cold-start exploitation** | bootstrap signals, referral discipline, limited initial scope |

Такой стек сохраняет прагматизм прежней версии: анти-манипуляция - это не один toggle, а архитектурный слой поверх scoring, publication и trust interpretation. [C7][C11]

### 4.2 COCM, commit-reveal и GroupGuard

Из `v0_legacy` в `v1` сохраняется полезная интуиция, что взаимные накрутки нельзя оставлять на уровне "мы потом посмотрим". Но теперь она ужесточается доказательной дисциплиной:

- `COCM`-подобный cluster discount сохраняется как design pattern для снижения веса тесно коррелированных review clusters;
- `GroupGuard` даёт более сильный modern precedent для explicit collusion detection и structural pruning;
- `commit-reveal` допустимо сохранять как anti-strategic disclosure pattern для bilateral feedback, но как implementation-level control, требующий собственного PoC и не заменяющий graph defense.

Иными словами, `COCM` отвечает за discount suspicious social topology, `GroupGuard` отвечает за detection and pruning, а `commit-reveal` отвечает только за sequencing of feedback disclosure. Вместе они образуют layered anti-gaming bundle, а не три конкурирующих ответа. [C7]

### 4.3 Reputation Stake, Bootstrap и Referral

Cold start нельзя решать ожиданием "сначала заработай репутацию, потом получишь работу", потому что такая схема блокирует новых агентов логически. Included evidence поддерживает более зрелый ответ: начальное доверие должно приходить из переносимых credential signals, prior activity, attested bootstrap paths и ограниченно рискованных первых взаимодействий. [C11]

Для `v1` это раскладывается на три основных инструмента:

| Механизм | Функция | Ограничение |
|----------|---------|-------------|
| **Reputation Stake** | создаёт skin-in-the-game для первых сделок | stake economics и token mechanics проектируются не здесь, а в `→ L2/D05 §2.4` |
| **Certified Bootstrap** | использует ранние `CC` signals через benchmark/audit credentials | не заменяет будущий `TP`/`PF`, а лишь открывает вход |
| **Referral / prior trust import** | переносит ограниченный prior trust через credentials или vouch-like signals | вес должен быть ограничен и decay-aware |

Это даёт D06 минимальный safe-onboarding baseline: новый агент может стартовать не с абсолютного нуля, но и не с полным social authority. Reputation stake создаёт economic commitment, certified bootstrap даёт early trust evidence, referral даёт ограниченный перенос внешнего доверия. [C8][C11]

### 4.4 A2A trust signals как cold-start inputs

Особенно полезно для cold start то, что `A2A trust.signals[]` уже задаёт reusable taxonomy начальных сигналов: on-chain credentials, on-chain activity, vouch chains и behavioral evidence. Даже если часть этого стека ещё proposal-level, он достаточно силён как design input для D06, потому что помогает не выдумывать собственный ad hoc envelope для стартового доверия. [C5][C11]

В терминах D06 это означает:

- до первой длинной task history агент может предъявлять credentials and activity signals;
- эти сигналы допускают старт в низкорисковых взаимодействиях;
- полноценный tier не должен выдаваться только по bootstrap-сигналам без накопления `TP` и `PF`.

Именно так D06 отделяет cold-start trust от earned reputation: первое открывает вход, второе строит устойчивое положение в сообществе. [C11]

### 4.5 Ограничения anti-manipulation слоя

Даже layered defense не снимает всех рисков. Included evidence прямо подсказывает три ограничения, которые должны быть названы в `v1` явно:

- нет одного complete protection mechanism;
- часть design controls ещё требует production validation в AI-agent settings;
- стоимость защиты должна соотноситься с value-at-risk, иначе система становится либо слишком дорогой, либо слишком слабой.

Поэтому D06 использует graduated security posture: low-stake interactions могут довольствоваться lightweight registry and threshold controls, тогда как high-stake relationships требуют более сильного graph/incentive/privacy bundle. Подробности trust propagation and attack resistance выше behavioral layer находятся в `→ L3/D09 §6`. [C7][C8]

---

## 5. Decay, тиры и LOA-переходы

### 5.1 Почему decay обязателен

Репутация без decay превращается в архив заслуг, а не в актуальный social signal. Included evidence прямо поддерживает модели, где свежесть сигнала критична, а старые данные должны либо терять вес постепенно, либо быть вытеснены более актуальными. Для D06 это означает, что reputation is always time-sensitive, even when it remains portable. [C4][C9][C16]

Практический baseline `v1` сохраняет параметры, уже подготовленные на уровне framing и legacy continuity:

- активный decay: `1%` в неделю для продолжающейся деятельности;
- неактивный decay: `5%` в неделю при длительном отсутствии релевантных событий.

Это не математическая истина "на все времена", а текущий policy-ready стартовый профиль. Он хорошо соответствует L1-идее: recent work должна весить больше, чем далёкое прошлое, но score не должен схлопываться в ноль мгновенно. [C4][C6]

### 5.2 Тиры как social shorthand

Поскольку полный репутационный профиль слишком богат для every discovery interaction, D06 использует tier layer как сокращённую социальную форму репутации. В `v1` сохраняются четыре основных положительных tier plus baseline unrated state:

| Tier | Current L1 threshold | Функция |
|------|----------------------|---------|
| **Unrated** | `0` или недостаточно evidence | discovery без earned reputation |
| **Bronze** | `1-30` | ранний подтверждённый trust baseline |
| **Silver** | `31-55` | устойчивое рабочее доверие |
| **Gold** | `56-75` | высокий domain-relevant track record |
| **Platinum** | `76-90` | высокий earned autonomy tier |
| **Diamond** | `91-100` | верхний tier: Trust Anchor, Mentor Sponsorship eligibility |

Эти tiers существуют не для эстетики badge'ей. Они сокращают поведенческий профиль до понятного сигнала для discovery, routing и access decisions, оставаясь совместимыми с on-chain anchor layer и `ERC-5192`-presentation model. [C1][C2][C6]

### 5.3 Связь tier с LOA transitions

Ключевая downstream-связь D06 - это не "репутация ради репутации", а earned autonomy. Здесь действует уже не собственное владение D06, а точная проекция на `L1/D07`: tier thresholds D06 служат репутационной частью переходов между уровнями автономности, но сами правила LOA принадлежат `→ L1/D07 §3`. [C13]

Текущий согласованный bridge выглядит так:

| Reputation Tier | Репутационный порог | LOA-переход |
|-----------------|---------------------|-------------|
| **Bronze** | `>=1` | eligibility for `LOA-1 -> LOA-2` |
| **Silver** | `>=31` | eligibility for `LOA-2 -> LOA-3` |
| **Gold** | `>=56` | eligibility for `LOA-3 -> LOA-4` |
| **Platinum** | `>=76` | eligibility for `LOA-4 -> LOA-5` |
| **Diamond** | `>=91` | Trust Anchor eligibility, Mentor Sponsorship; LOA-5 consolidated |

Эта таблица намеренно читается как projection table, а не как повторное полное описание автономности. D06 отвечает за репутационный сигнал, D08 отвечает за full promotion logic, mission counts, override rates и governance conditions. Подробности: `→ L1/D07 §3`. [C13]

### 5.4 OI-088: reputation_multiplier не равен ESq_base

В предыдущей версии D06 репутационные тиры были слишком близко подведены к `ESq`, что создало реальный ownership conflict с `D02`. После ремонта `OI-088` в `v1` фиксируется более строгая формулировка:

> **OI-088 note.** Любой reputation-driven competence bonus читается только как отдельный `reputation_multiplier`, накладываемый downstream поверх `ESq_effective`, а не как изменение `ESq_base`. Canonical owner `ESq/ESc` — `→ L1/D02 §5`. Ориентировочные значения `reputation_multiplier` по тирам (illustrative, pending governance finalization): Bronze +0%, Silver +2%, Gold +5%, Platinum +12%, Diamond +25%. Эти проценты не являются частью формулы ESq — они описывают downstream discoverability boost и операционный кредит доверия за пределами competence semantics. [C8][C13] [OI-043: финализация шкалы влияет на эти значения]

Это важно и практически. Социальное доверие действительно может усиливать willingness других работать с агентом, но не должно переписывать доказанный competence baseline владельца и агента. Таким образом сохраняется чистая граница: `D02` отвечает на вопрос "что ты умеешь", `D06` - на вопрос "как сообщество оценивает твою надёжность". [C3][C8]

### 5.5 Tier decay и пересмотр статуса

Tier никогда не должен быть вечным badge без пересмотра. Поскольку базовый score подвержен decay, tier layer тоже должен пересматриваться автоматически:

- при падении score ниже порога tier понижается;
- при накоплении нового evidence tier может вернуться вверх;
- при длительной неактивности агент сохраняет history, но не весь прежний operational privilege.

Именно это делает связь с автономностью безопасной. Если бы tier не деградировал, агент мог бы удерживать высокий `LOA` только на исторической репутации. D06 поэтому требует, чтобы reputation tiers были живым отражением current state, а не immutable distinction. [C4][C6][C13]

---

## 6. Privacy, portability и incentive design

### 6.1 Privacy boundary: что D06 утверждает, а что нет

На уровне L1 D06 не должен разворачивать полный криптографический учебник по ZKP, BBS+ и unlinkable proofs. Но он обязан зафиксировать privacy-answer на собственный вопрос: репутация должна быть предъявляемой without forced full disclosure. Это означает, что D06 оставляет подробную proof infrastructure в `→ L2/D07 §8`, но требует, чтобы reputation credentials и score proofs поддерживали selective disclosure и privacy-preserving status checks. [C12]

Практический минимум для `v1` таков:

- доказывать tier или threshold можно без раскрытия полного репутационного профиля;
- status/revocation должны оставаться privacy-aware;
- on-chain anchors не должны нести больше персонализированной репутационной информации, чем нужно для проверки.

Таким образом D06 проектирует privacy requirement, но не присваивает себе ownership над криптографической реализацией. [C12][C15]

### 6.2 Портативность: переносить нужно evidence bundles, а не только score

Included evidence значительно сильнее поддерживает переносимость reputation через portable credentials and signal bundles, чем через голый chain-local score. Поэтому `v1` фиксирует следующий baseline:

- переносимым объектом является bundle из attestations, status pointers и bounded summaries;
- chain-local registry score полезен для discovery, но недостаточен как единственный portable artifact;
- cross-ecosystem trust лучше переносится через VC-like evidence и compatible identity rails.

Это объясняет, почему D06 совмещает `EAS`, `ERC-8004` и graph/VC-style layers: один standard слишком narrow для всей задачи portability. Для межсредовой работы Логоса важны не только текущие points, но и provenance of trust signals. [C9][C10][C14]

### 6.3 G15: fragmentation и canonical identity

Самый уязвимый участок portability - не перенос score между chain'ами, а fragmentation across identities, personas and sub-agents. Included evidence поддерживает canonical identity graph, deterministic linking when possible, probabilistic fallback only with confirmation, и progressive disclosure для unresolved identities. Но production-ready rules для hierarchical sub-agent reputation inheritance evidence пока не даёт. [C13][C16]

Следовательно, для `v1` D06 фиксирует три правила:

- репутация должна агрегироваться к одному canonical identity whenever linking is verified;
- неподтверждённые связи не должны silently merge separate personas;
- sub-agent inheritance остаётся open design problem, а не завершённой нормой.

Это особенно важно для multi-agent Logos, где orchestration quality principal agent и task quality delegated subagents могут требовать разных layers of attribution. Полная trust physics таких случаев принадлежит `→ L3/D09 §3-§5`; D06 здесь фиксирует only the reputational boundary and unresolved risk. [C13][C16]

### 6.4 Incentive-compatible reputation

Репутационная система становится устойчивее, когда truthful evaluation выгодна экономически, а dishonest signaling создаёт cost or lost upside. Included evidence поддерживает именно такой direction: reputation and payment functions могут быть связаны так, чтобы cooperation and truthful reporting становились evolutionarily stable or at least materially encouraged. [C8]

Для `v1` отсюда следует практический design bundle:

- evaluation должно быть связано с reward / penalty logic, а не существовать отдельно от economic outcomes;
- truthful peer feedback желательно стимулировать explicit mechanism design;
- honest behavior must improve expected future access and revenue, а dishonest behavior должно создавать measurable downside.

Это не означает, что D06 уже имеет closed production mechanism. Это означает лишь, что репутация должна быть встроена в incentive loop, а не висеть как декоративная метрика рядом с платежами и marketplace behavior. [C8]

### 6.5 G14/G15 design notes

Для конца `v1` полезно зафиксировать две осторожные design notes.

Первая: `Ev-Trust` и родственные mechanism-design patterns делают правдоподобной связку "репутация -> доходы -> устойчивость кооперации", но production AI-agent implementation still needs PoC. Вторая: privacy и portability не отменяют legal/design tension вокруг identity linking и immutable anchors, поэтому safest current posture для D06 - mutable identity resolution off-chain plus on-chain anchors only. [C8][C13][C15]

Именно поэтому D06 в текущем виде сознательно избегает двух крайностей:

- не обещает finished trust economy by default;
- не отказывается от portability/privacy только потому, что perfect architecture ещё не доказана.

---

## Связь с другими измерениями

| Измерение | Связь |
|-----------|-------|
| **L1/D01 (Идентификационное)** | `DID` и `PoP` задают якорь субъекта, к которому вообще может быть привязана репутация. Полная identity-иерархия и privacy-boundary предъявления identity claims находятся в `→ L1/D01 §1`, `→ L1/D01 §3` и `→ L1/D01 §7`. |
| **L1/D02 (Компетентностное)** | `CC` использует capability attestations как certified input, но `ESq`, `ESc` и competence semantics не принадлежат D06. Репутационные бонусы читаются только как downstream `reputation_multiplier`, не переписывающий `ESq_base`: `→ L1/D02 §5`. |
| **L1/D04 (Функциональное)** | `Execution Trace` остаётся главным объективным источником для `TP`; D06 интерпретирует trace как social evidence, но не владеет самим execution artifact: `→ L1/D04 §7`. |
| **L1/D05 (Коммуникационное)** | `A2A`-surface и trust signals дают канал для feedback, vouch-like evidence и discovery, но не владеют репутационной логикой. Коммуникационный baseline находится в `→ L1/D05 §1` и `→ L1/D05 §4`. |
| **L1/D07 (Автономность)** | Reputation tiers используются как earned-autonomy signal, но полная логика переходов `LOA` принадлежит `→ L1/D07 §3`. D06 поставляет reputation side of thresholding, а не саму autonomy policy. |
| **L3/D09 (Доверие)** | D06 описывает анатомию репутации отдельного Логоса, а `L3/D09` описывает физику распространения доверия по сети, propagation, trust graph и attack resistance: `→ L3/D09 §2-§4`, `→ L3/D09 §6`. |

---

## Открытые вопросы

### Требуют решения (Critical / High)

1. **[High] Incentive-compatible feedback PoC.** Корпус поддерживает связку репутации с truthful reporting, reward/penalty loops и cooperative stability, но production-ready agentic mechanism для `D06` ещё не доказан end-to-end.
   > **Open.** Нужен PoC, который соединит payment-aware evaluation, structured peer feedback, anti-collusion controls и measurable reward shaping без разрушения usability. [C8]

### Требуют исследования (Medium)

2. **[Medium] OI-043: финализация шкалы score.** Текущий draft использует `0-100` как publication layer, но conflict with richer internal `0-1000` models остаётся открытым.
   > **Open.** Governance должен решить, считать ли `0-100` итоговой канонической шкалой D06 или внешней нормализацией более богатого внутреннего расчёта. До этого все пороги tier следует трактовать как current working baseline. [C1][C4][C6]

3. **[Medium] G15: fragmentation при multi-persona и sub-agent reputation.** Корпус поддерживает canonical identity resolution, но не даёт production-ready нормы для наследования репутации между главным агентом и субагентами.
   > **Open.** Требуется отдельная design workstream по attribution rules: что относится к orchestration reputation principal agent, что - к domain reputation delegated subagents, и как не допустить ложных merges. [C13][C16]
