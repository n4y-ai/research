# УРОВЕНЬ 3 / ИЗМЕРЕНИЕ 07: ОРГАНИЗАЦИОННО-ЭМЕРДЖЕНТНОЕ

> **Уровень:** L3 — Межагентные взаимодействия  
> **Измерение:** D07 — Emergence  
> **Версия:** 4.0  
> **Статус:** Финал v4.0 | Дата: 04.04.2026  
> **Зависимости:** L2/D07, L3/D06, L3/D05, L2/D06, L1/D07, L2/D09

---

## Ключевой вопрос

**Какие устойчивые надорганизационные формы (виртуальные компании, гильдии, картели) возникают из хаоса взаимодействий?**

---

## Аннотация

Этот черновик переводит D07 с описательных тезисов на evidence-grounded синтез. Корпус подтверждает, что эмерджентность в агентных экономиках проявляется не как единичный феномен, а как набор устойчивых режимов: полезные организационные формы, коллективный интеллект, нормы и Knowledge Networks с одной стороны, и flash crash, collusion, cascade failure и herding с другой. Ключевой вывод D07: главным рычагом устойчивости оказывается не жёсткая централизация, а управляемое разнообразие агентов, раннее обнаружение фазовых переходов и ограничение каналов распространения негативной динамики. [E1][E3][E9][E16][E17][E32][E37][E49]

По сравнению с `v3.1` этот текст делает три жёстких исправления. Во-первых, не принадлежащие D07 концепты переведены в проекции: `AI Watcher` → `L2/D07 §3`, `Circuit Breaker` → `L2/D07 §6`, `Microsandbox` и `CSB` → `L2/D06 §5`, `LOA` → `L1/D07 §1`, platform governance → `L2/D09 §1` и `§5`. Во-вторых, Metcalfe/Reed и фиксированные пороги network effects больше не подаются как закрытое знание: эмпирическая валидация для агентной экономики остаётся открытой. В-третьих, `Collective LOA` явно отмечается как design issue, а не как доказанный режим автономии. [E20][E23][E25][E39]

---

## Содержание

1. [Что именно возникает в D07](#1-что-именно-возникает-в-d07)
2. [Позитивная эмерджентность и коллективный интеллект](#2-позитивная-эмерджентность-и-коллективный-интеллект)
3. [Гетерогенность как главный рычаг устойчивости](#3-гетерогенность-как-главный-рычаг-устойчивости)
4. [Негативная эмерджентность: collusion, flash crash, cascades](#4-негативная-эмерджентность-collusion-flash-crash-cascades)
5. [Фазовые переходы и раннее обнаружение](#5-фазовые-переходы-и-раннее-обнаружение)
6. [Containment и управление эмерджентными формами](#6-containment-и-управление-эмерджентными-формами)
7. [Collective LOA как открытая граница](#7-collective-loa-как-открытая-граница)

---

## 1. Что именно возникает в D07

### 1.1 Рабочее определение

Для D07 эмерджентность означает устойчивые макроформы и режимы, которые не проектируются напрямую одним контрактом или одним агентом, а возникают из повторяющихся взаимодействий, feedback loops, репутационных сигналов, обмена знаниями и плотности координации. Важна не любая спонтанность, а только та, которая переживает отдельные транзакции и начинает влиять обратно на поведение участников: через path dependence, hysteresis, threshold effects и downward causation. [E10][E39][E40][E44][E45]

### 1.2 Таксономия устойчивых форм

Ниже перечислены не "готовые юридические сущности", а N4Y-синтез организационных режимов, собранный из screened evidence. Корпус лучше подтверждает условия их появления и деградации, чем сами ярлыки; поэтому названия форм здесь служат рабочей типологией D07. [E3][E15][E20][E32][E33][E42]

| Форма | Что возникает | Условия устойчивости | Как деградирует | Опора |
|------|---------------|----------------------|-----------------|------|
| **Виртуальная компания** | Временная многоагентная "фирма" под миссию или рыночную возможность | Повторяемый поток задач, распределение ролей, достаточное доверие и предсказуемый settlement | Распадается при падении trust/reputation signals и разрыве task flow | [E3][E15][E33][E42] |
| **Гильдия** | Устойчивый кластер специализации с фильтром входа по качеству и репутации | Доменные нормы, reputational screening, shared standards, связь с platform governance без поглощения ею | Деградирует в закрытый клуб или rent-seeking при низкой прозрачности и плохой калибровке репутации | [E19][E20][E23][E36] |
| **Картель** | Координация ради подавления конкуренции и supra-competitive outcomes | Достаточная согласованность стратегий, наблюдаемость поведения друг друга, слабые deterrence mechanisms | Разрушается через GNN detection, enforcement, diversification и anti-collusion constraints | [E32][E37][E38] |
| **Knowledge Network** | Граф экспертизы, в котором ценность создаётся повторной валидацией и маршрутизацией знаний | Специализация, память, reputational legibility, умеренная плотность координации | Деградирует в echo chamber или в слабосвязанный каталог при падении trust/validation quality | [E17][E33][E35][E36] |

Типология выше важна для разграничения D07 и `L3/D05`. `Coalition` и `Alliance` описываются fully в `→ L3/D05 §1` и `→ L3/D05 §4`; здесь они выступают как микромеханизмы, из которых могут кристаллизоваться более устойчивые надорганизационные формы. D07 отвечает не за lifecycle команды, а за переход от повторяющихся локальных коопераций к макрорежимам сети. [E33][E39]

---

## 2. Позитивная эмерджентность и коллективный интеллект

### 2.1 Когда возникает полезная самоорганизация

Корпус не подтверждает тезис "чем больше агентов, тем умнее система". Позитивная эмерджентность появляется только тогда, когда diversity, memory и layered coordination поддерживают друг друга. Самый сильный screened вывод здесь такой: crowd effects без памяти и без структурированной координации остаются хрупкими, а не интеллектуальными. И наоборот, разнообразие агентов, Theory-of-Mind-like coordination, повторяющиеся сигналы и отсутствие полной монокультуры повышают шансы на collective intelligence. [E14][E16][E17][E19]

Практически это означает для N4Y.AI три критерия:

1. В системе должна сохраняться `Coordination Entropy`, а не сводиться к одному доминирующему шаблону решений.
2. Обратная связь должна быть достаточно быстрой, чтобы полезные паттерны закреплялись, но не настолько жёсткой, чтобы каждый локальный успех немедленно превращался в herding.
3. Коммуникация и environmental traces должны быть богатыми настолько, чтобы агенты координировались, но не настолько централизованными, чтобы вся сеть стала одной коррелированной машиной ошибки. [E17][E30][E39]

### 2.2 Stigmergy как неявная координация

`Stigmergy` в D07 не заменяет прямую коммуникацию, а объясняет, как полезная самоорганизация сохраняется даже без постоянного переговорного канала. Сильнейшее screened наблюдение задаёт порог порядка `ρ_c ≈ 0.23`: ниже него traces остаются слишком разрежёнными, выше него сеть переходит от memory-dominated поведения к self-organized coordination. При этом та же работа показывает, что одна только среда недостаточна: добавление индивидуальной памяти даёт `68.7%` improvement, а полностью "безголовая" среда не удерживает координацию стабильно. [E16][E30][E31]

Для N4Y.AI стигмергические сигналы могут проявляться через price traces, публичную репутацию и shared artifacts. Полное описание `Репутации` находится в `→ L1/D09 §1`, а не здесь; D07 использует её только как один из environmental traces, через которые сеть обучается локальным правилам без общего диспетчера. [E19][E30][E36]

### 2.3 Micro-macro loop

Позитивная эмерджентность не бывает строго bottom-up. Macro states затем начинают ограничивать micro behavior: успешная Knowledge Network меняет выбор задач, reputational stratification меняет партнёрства, а доминирующие нормы меняют вероятность отклонения. Именно поэтому D07 должен думать не только о появлении полезного порядка, но и о том, как он закрепляется и когда становится self-locking equilibrium. [E10][E39][E40][E44][E45]

---

## 3. Гетерогенность как главный рычаг устойчивости

Самый практически полезный вывод всего корпуса: гетерогенные популяции устойчивее гомогенных почти по всем критическим метрикам D07. В evidence pack это сводится к трём повторяющимся наблюдениям: `3-8×` lower collapse risk, `4-6×` smaller cascade amplitude и `60-80%` lower manipulation vulnerability. Эти числа пока остаются transfer estimates, а не N4Y-specific calibration, но направление эффекта подтверждается независимо несколькими линиями evidence. [E3][E17][E45][E48][E49]

| Параметр | Гомогенная популяция | Гетерогенная популяция | Вывод для D07 |
|----------|----------------------|-------------------------|---------------|
| Синхронный сбой | Быстрый и коррелированный | Более поздний и фрагментированный | Diversity уменьшает systemic coupling |
| Манипулируемость | Высокая при одинаковых policy/model priors | Ниже из-за contestability | Сетевые атаки хуже масштабируются |
| Скорость самоорганизации | Быстрее | Медленнее | D07 принимает slower coordination как цену устойчивости |
| Качество режима | Хрупкое winner-takes-most | Более robust multi-cluster order | Лучше для positive emergence |

Здесь важно не перепутать capability diversity с governance chaos. D07 не доказывает, что "чем больше различий, тем лучше"; он показывает, что controlled heterogeneity снижает риск collapse, но увеличивает coordination overhead. Следовательно, D07 должен проектировать heterogeneity как safety lever, а не как эстетическое предпочтение. [E3][E17][E48][E49]

---

## 4. Негативная эмерджентность: collusion, flash crash, cascades

### 4.1 Collusion как полноценная эмерджентная форма

Корпус закрывает спор о том, возможен ли cartel-like режим без явного центра. Да: LLM и agent populations способны drift into tacit, explicit и covert collusion; enforcement and market remedies для таких режимов описываются в `→ L3/D06 §5`. В screened evidence это выглядит как три разных механизма:

1. **Tacit coordination**: агенты сходятся к supra-competitive equilibrium даже без прямой коммуникации. [E32]
2. **Explicit natural-language cartel**: координация через обычные текстовые сообщения. [E38]
3. **Covert / steganographic collaboration**: вредоносная координация прячется в seemingly benign exchange. [E38]

Для обнаружения strongest evidence указывает на graph-based detection: GNN-based detectors лучше standard neural baselines переносят collusive patterns across markets. Полный anti-collusion stack принадлежит `→ L3/D06 §3`; в D07 этот материал важен потому, что картель здесь понимается как emergent organizational pathology, а не просто нарушение правил торгов. [E37][E38]

### 4.2 Flash crash и cascade failure

Flash crash в D07 нельзя трактовать как "редкую аномалию". Корпус описывает его как структурное следствие высокой связности, leverage, portfolio overlap и положительной обратной связи между локальными стратегиями. Из этого же следует неприятный вывод: compartmentalization помогает, но не гарантирует полной локализации. Если сеть остаётся tightly coupled через bridges и общие сигналы, contagion переходит границы контейнеров. [E4][E5][E6][E7][E8][E49]

Особенно важен здесь конфликт evidence, который D07 не должен замазывать. С одной стороны, compartmentalization снижает системный риск. С другой, сложные interconnections способны обойти boundary assumptions. Поэтому для N4Y.AI правильный тезис звучит так: `Microsandbox` уменьшает blast radius, но не отменяет cross-boundary contagion через `CSB`. Полное описание `Microsandbox`, `CSB` и `Sandbox Circuit Breaker` находится в `→ L2/D06 §5`; D07 фиксирует только emergent consequence этого выбора. [E6][E7][E27][E29]

### 4.3 Граница между кооперацией и вредной координацией

Для D07 это отдельная проблема классификации. `Alliance` как полезная долгосрочная кооперация описывается fully в `→ L3/D05 §1`, тогда как cartel подавляет competition. На практике же один и тот же cluster behavior может выглядеть одинаково на уровне raw telemetry. Поэтому D07 должен требовать не только detectors, но и semantic criteria: что именно координируется, подавляет ли это contestability, есть ли price/output harm, можно ли объяснить паттерн benign division of labor. Эта граница остаётся открытой. [E32][E34][E37][E38]

---

## 5. Фазовые переходы и раннее обнаружение

### 5.0 ABM-sourced early-warning signals (projection from L3_D06)

Агент-based calibration многорыночных dynamics N4Y.AI описана в `→ L3_D06 §5.2-§5.3`; D07 фиксирует только emergent читку: ABM-output metrics из `→ L3_D06 §5.3` (`HHI trajectory`, `challenger win share`, `price dispersion`, `cross-market dependency score`, `convergence diagnostics`) служат входными сигналами раннего предупреждения для phase-transition detection framework этого раздела. Параметры и governance-tunable пороги — в `→ L3_D06 §5.2-§5.3`. [A90]

### 5.1 Базовый набор ранних сигналов

Нормальная starting point для D07 — classical early-warning signals: rising variance, autocorrelation, slower recovery. Но screened corpus показывает, что ими нельзя ограничиваться, потому что многие реальные transitions выглядят как non-equilibrium or first-order-like phenomena. Поэтому detection stack должен объединять classical CSD indicators, multivariate recurrence features и non-equilibrium metrics вроде entropy production или time-irreversibility proxies. [E1][E2][E10][E11][E12][E13]

### 5.2 Что даёт ML

Сильнейший внешний результат в корпусе — PNAS 2026: ML detector поймал `50%` transitions within a short lead window при `3.6%` false-positive rate на большой социальной системе (`[C3]` — скринированный включённый источник). В evidence pack это трактуется как достаточное основание для ensemble approach, а не для слепой веры в один индикатор. Research materials D07 дополнительно фиксируют design band, где ML-ensemble может работать в low-80s sensitivity range (`82-89%`) при хорошем feature engineering (`[C-survey]` — ensemble survey, нескринированные research materials; другой дизайн исследования). Эти два числа нельзя смешивать: PNAS-результат — валидированная нижняя граница из included source; диапазон 82-89% — transfer estimate из research materials, который пока не получил статуса screened evidence. Оба числа требуют локальной калибровки на данных N4Y.AI. [E9][E13][E50]

Следствие для D07: фазовый переход должен детектироваться как согласованность нескольких слабых сигналов, а не как single-threshold event. Практически это означает ensemble из:

1. classical CSD (`variance`, `autocorrelation`, recovery lag),
2. entropy / irreversibility signals,
3. Bayesian changepoint logic,
4. ML anomaly layer для noisy heterogeneous populations. [E1][E9][E11][E13][E50]

### 5.3 Проекция на AI Watcher

`AI Watcher` описывается fully в `→ L2/D07 §3`, а platform-level `Circuit Breaker` — в `→ L2/D07 §6`. Для D07 важно не их внутреннее устройство, а то, что emergence monitoring должно идти по трём уровням: individual deviation, pair/group coordination и whole-network regime shift. Эта трёхслойность логически согласуется с corpus: опасные transitions редко возникают одновременно во всех точках сети; сначала меняется локальная структура корреляций, затем меняется макрорежим. [E1][E9][E39]

В текущем included corpus нет достаточного основания, чтобы фиксировать строгий закон числа sentinel nodes или topology formula наподобие `√ln N`. Следовательно, `v4.0` сохраняет идею distributed monitoring, но не закрепляет неподтверждённую числовую sufficiency rule.

---

## 6. Containment и управление эмерджентными формами

### 6.1 Containment как многоуровневый режим

D07 не может опираться на один boundary mechanism. Нормализованный корпус прямо говорит, что effective containment для emergent failures должно комбинировать circuit breakers, scoped communication и protective interventions; ни одна граница сама по себе не останавливает cascade propagation. [E24][E25][E27][E29]

Отсюда возникает трёхчастная логика D07:

1. **Разрешать** positive emergence внутри локальных сред.
2. **Локализовать** negative emergence до того, как она приобретёт межсистемный характер.
3. **Разрывать** каналы распространения, а не только "лечить" отдельного агента.

Архитектурный слой этого механизма принадлежит `→ L2/D06 §5`. D07 формулирует только emergent reading: containment полезен не потому, что запрещает самоорганизацию, а потому, что делает её domain-bounded и reversible.

### 6.2 Governance emergent structures

Полная DAO/governance architecture принадлежит `→ L2/D09 §1` и `→ L2/D09 §5`. В D07 governance нужно в более узком смысле: как сеть признаёт, ограничивает и использует уже возникшие формы. Из evidence следует умеренный, а не романтический вывод: реальные DAOs часто страдают от token concentration и de facto centralization; agentic governance promising, but early-stage. [E20][E23][E25]

Практический вывод D07:

1. Гильдии и Knowledge Networks можно признавать как полезные emergent clusters только при наблюдаемой accountability.
2. Platform governance не должна смешиваться с governance самой emergent формы: первое принадлежит L2, второе D07 описывает как объект наблюдения.
3. Sandbox Councils и TAC из `→ L2/D09 §5` должны играть роль interface layer, где emergent structures встречаются с formal governance, не превращаясь автоматически в owner-level institutions.

---

## 7. Collective LOA как открытая граница

`LOA` как шкала автономии принадлежит `→ L1/D07 §1`; D07 не переопределяет её. Но D07 обязан ответить на вопрос, может ли группа действовать "автономнее", чем любой из её участников. Current screened corpus отвечает осторожно: коллектив может outperform individual members за счёт decomposition, delegation и specialization, однако ни один included source не даёт формальной модели, в которой группа легитимно превышает автономный предел собственных членов. [E17][E23][E41][E47]

Поэтому `Collective LOA` в `v4.0` фиксируется как unresolved design issue:

1. **Что поддержано evidence:** группа может показывать выше performance и problem-solving breadth, чем отдельный агент. [E17][E23][E47]
2. **Что не поддержано evidence:** группа не получила доказанного права превышать autonomy envelope, выданный её участникам. [E41][E47]
3. **Рабочее правило N4Y.AI для черновика:** пока не создана собственная теория, D07 должен считать, что коллективное действие может превосходить индивидуальную capability, но не должно превосходить максимально допустимый delegated authority участников и platform constraints из `→ L1/D07 §1` и `→ L2/D09 §1`.

Это самый сильный оставшийся conceptual gap D07 и именно здесь требуется original N4Y theoretical work, а не только очередной search pass.

---

## Связь с другими измерениями

| Измерение | Связь |
|-----------|-------|
| **L2/D07 (Безопасность и мониторинг)** | `AI Watcher` и platform `Circuit Breaker` дают механизм раннего обнаружения и остановки негативной эмерджентности; D07 задаёт, что именно надо мониторить как macro-regime shifts. |
| **L2/D06 (Координация и исполнение)** | `Microsandbox`, `CSB` и Sandbox CB определяют архитектурный blast radius; D07 описывает, как через эти границы распространяются или локализуются emergent patterns. |
| **L3/D06 (Конкурентно-рыночное)** | Cartel detection и `HHI` monitoring `D06 §5` = enforcement response к cartels `D07`. Anti-collusion mechanisms `D06 §3` → стимулы против cartel formation. `→ L3/D06 §3`, `§5` |
| **L3/D08 (Конфликтно-арбитражное)** | Alliance vs Cartel classification disputes → `D08` dispute resolution `§1`, `§3`. Flash crash cascade disputes → `D08 §6-§7`. `→ L3/D08 §1`, `§3`, `§6`, `§7` |
| **L3/D05 (Кооперация)** | `Coalition` и `Alliance` выступают micro-level building blocks, из которых со временем могут кристаллизоваться гильдии, виртуальные компании и другие устойчивые формы. |
| **L1/D07 (Автономность)** | D07 зависит от owner-описания `LOA`: group performance можно анализировать здесь, но каноническую autonomy scale D07 не меняет. |
| **L2/D09 (Governance)** | Platform governance определяет formal recognition, thresholds и emergency powers по отношению к emergent structures; сами эти структуры остаются объектом D07-наблюдения. |
| **L3/D09 (Доверие)** | Trust dynamics и reputation-mediated partner choice определяют, какие emergent clusters становятся устойчивыми, а какие распадаются. |

---

## Открытые вопросы

### Требуют решения (Critical / High)

1. **[High] Collective LOA.** Нет формальной модели, по которой группа агентов легитимно превышает autonomy bound собственных участников.
> **Open.** Нужна оригинальная N4Y-модель: отличие collective capability от collective authority, правила composition/decomposition и проверяемый upper bound относительно `→ L1/D07 §1`.

2. **[High] Cross-sandbox contagion через CSB.** Microsandbox помогает containment, но corpus не позволяет утверждать полную изоляцию каскадов.
> **Open.** Нужны simulations или formal bounds для contagion across `→ L2/D06 §5`, особенно при shared signals, portfolio overlap и cross-sandbox routing.

3. **[High] Граница Alliance vs Cartel.** Tacit collusion и legitimate long-term coordination telemetry-wise могут выглядеть одинаково.
> **Open.** D07 нужен классификатор вреда: price/output harm, loss of contestability, exclusion effects и semantic intent поверх detector output. [E32][E37][E38]

### Требуют исследования (Medium)

4. **[Medium] Network effects validation.** Metcalfe/Reed удобно объясняют intuition роста, но прямой empirical validation для AI agent economies нет.
> **Open.** G-08 остаётся открытым; нужен ABM/production telemetry, а не только аналогии с соцсетями и финансами.

5. **[Medium] Переносимость порога `ρ_c ≈ 0.23`.** Самый сильный screened результат получен на одной архитектуре stigmergic coordination.
> **Open.** Нужно проверить, как меняется threshold при другом memory depth, trace persistence и communication topology. [E16][E30][E31]

6. **[Medium] Калибровка ensemble detection.** В research materials фигурирует promising low-80s sensitivity band, но из screened evidence напрямую закреплён только результат PNAS 2026.
> **Open.** Нужна локальная калибровка признаков, thresholds и detector mix для N4Y populations; неподтверждённые sufficiency rules для sentinel topology пока не фиксировать. [E9][E13][E50]

---

## Источники

> Все цитаты `[E#]` трассируются через [`evidence/evidence-pack.md`](evidence/evidence-pack.md). Количественные параметры (3–8× collapse risk, 4–6× cascade amplitude, ρ_c ≈ 0.23, 50% PNAS 2026 [C3]) — transfer estimates из screened corpus; диапазон 82–89% [C-survey] — из research materials. Все числа требуют локальной калибровки для N4Y.AI.
