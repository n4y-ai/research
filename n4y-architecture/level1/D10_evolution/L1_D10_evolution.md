# УРОВЕНЬ 1 / ИЗМЕРЕНИЕ 10: ЭВОЛЮЦИОННОЕ
> **Уровень:** L1 — Агент (свойства Логоса)  
> **Измерение:** D10 — Evolution  
> **Версия:** 4.0  
> **Статус:** Финал v4.0 | Дата: 02.04.2026  
> **Зависимости:** L1/D02, L1/D04, L1/D09, L1/D07, L3/D04, L2/D08

---
## Ключевой вопрос
**Как ты развиваешься, учишься и адаптируешься к новым условиям и какие механизмы обучения у тебя есть?**

---
## Аннотация
Этот черновик фиксирует эволюцию Логоса как непрерывный operational process, а не как редкое переобучение модели. Базовый ответ D10 состоит из двух независимых потоков: `Human Evolution` повышает компетентностный потолок владельца через human credentials и `ESq`, а `Agent Evolution` повышает фактическую capability Логоса через опыт, benchmarks, memory updates, controlled versioning и повторную сертификацию. Оба потока влияют на допустимый operating envelope, но не должны смешиваться в один индикатор. [C1][C6][C13][C18]

По сравнению с `v3.1` прежнее ядро сохраняется, но собирается в более строгий ownership-compliant каркас: `Agent Capability VC` остаётся owner-концептом `→ L1/D02 §3-§6`, `Execution Trace` остаётся owner-концептом `→ L1/D04 §7`, `LOA` и `Earned autonomy` остаются owner-концептами `→ L1/D07 §1-§5`, а regulatory analysis остаётся проекцией на `→ L2/D08 §1`, `§4`, `§5`. Внутри D10 fully описываются только эволюционные loop'ы: Experience Base, четырёхслойное versioning, canary rollout, bounded self-update guardrails, контроль negative transfer и governance-метрики. [C2][C4][C5][C7][C9][C16][C17]

---
## Содержание
1. [Двойной поток эволюции](#1-двойной-поток-эволюции)
2. [Experience Base](#2-experience-base)
3. [4-слойное версионирование](#3-4-слойное-версионирование)
4. [Canary deployment и re-certification](#4-canary-deployment-и-re-certification)
5. [Safety guardrails при эволюции](#5-safety-guardrails-при-эволюции)
6. [Negative transfer и preference contamination](#6-negative-transfer-и-preference-contamination)
7. [Human oversight, метрики и governance](#7-human-oversight-метрики-и-governance)

---
## 1. Двойной поток эволюции
### 1.1 Базовый принцип
D10 отвечает на вопрос развития Логоса через два независимых, но связанных потока: `Human Evolution` поднимает разрешённую границу специализации через human qualification evidence и `ESq`, а `Agent Evolution` меняет фактическую operational capability через опыт, benchmark-backed capability updates, memory consolidation и controlled deployment. Эти потоки нельзя сливать в один показатель без потери смысла: человек может иметь более высокий competence ceiling, чем текущий агент реально демонстрирует, а агент может накапливать опыт, не меняя human credentials владельца. Поэтому D10 принимает двухпотоковую модель как owner-level synthesis и опирается на competence owner `→ L1/D02 §1`, `§3`, `§6` для semantics `ESq_effective = ESq_base × capability_multiplier`. [C1][C6][C18]

### 1.2 Верхняя граница и operational growth
Полное описание human competence track не принадлежит D10 и находится в `→ L1/D02 §1-§3`. Для D10 достаточно зафиксировать, что новые `Educational VC` и domain claims поднимают `ESq_base`, но не доказывают сами по себе, что агент уже умеет безопасно выполнять новые задачи. Agent-side рост, напротив, строится не вокруг one-shot retraining, а вокруг циклов `plan -> act -> reflect -> update`, где изменения могут происходить на prompt/policy, memory, knowledge и tool layers без обязательного full fine-tuning. Поэтому D10 рассматривает эволюцию как continuous operational adaptation, а не как редкое событие "обновили модель и всё изменилось". [C1][C2][C3][C6][C14][C18]

### 1.3 Четыре оси `G10`
Для `v4.0` D10 фиксирует четыре оси, по которым evolution должна читаться в L1-слое:

| Ось | Что растёт | Основной owner/source | Как проявляется в D10 |
|-----|------------|-----------------------|-----------------------|
| **Ось 1. Компетенции владельца** | `ESq_base`, domain credentials, permission envelope | `→ L1/D02 §1-§3` | расширяет допустимую область специализации |
| **Ось 2. Capabilities агента** | benchmark-backed capability state, `Agent Capability VC`, quality of execution | `→ L1/D02 §3-§6` | повышает capability multiplier и re-cert scope |
| **Ось 3. Автономность** | допустимый `LOA`, density of oversight, exception-driven operation | `→ L1/D07 §1-§5` | меняет, сколько эволюционных шагов допускается без ручного approve |
| **Ось 4. Экономическая устойчивость** | способность финансировать обновления, benchmarks, memory retention, rollback buffers | reputation/economic downstream | определяет sustainable cadence эволюции, но не заменяет capability evidence |

Эти оси намеренно не сводятся к одной шкале зрелости. D10 фиксирует их как orthogonal control surface: агент может расти по capability без немедленного роста `LOA`, а автономность не должна повышаться только потому, что вырос economic throughput. [C5][C13][C17]

### 1.4 Независимость потоков и синхронизация
Минимальная operational синхронизация между двумя потоками выглядит так:
```text
Human Evolution:
credential update -> ESq review -> permission envelope expands

Agent Evolution:
experience / benchmark / rollout -> capability review -> safe deployment

Joint effect:
new operating envelope only after both permission and capability remain valid
```

Практический смысл схемы прост: эволюция не считается завершённой, пока новые capabilities не подтверждены, а новый human-side scope не отражён в governance и credentials. Это сохраняет границу между "разрешено" и "реально доказано". [C5][C6][C17]

### 1.5 `G10` design note
> **`G10` resolve-in-draft note.** D10 фиксирует оси и их роли, но не притворяется, будто literature уже дала окончательные межосевые веса. Inter-axis weights и tradeoff policy остаются governance question; в operational baseline действует правило: ни одна ось не может автоматически повышать другую без отдельного validation step. [C13][C17]

---
## 2. Experience Base
### 2.1 Что именно D10 называет `Experience Base`
В общем глоссарии `Experience Base` описан как persistent knowledge store Логоса. Для `v4.0` D10 стабилизирует его через три рабочих компонента: `Episodic Memory`, `Pattern Library` и `Reflection Engine`. `Preference Model` при этом не объявляется здесь стабилизированным core-компонентом; он остаётся adjacent layer и обсуждается отдельно в §6, потому что protocol против contamination пока не закрыт. `Execution Trace` как артефакт исполнения fully описан в `→ L1/D04 §7`; для D10 trace важен как сырьё для learning loop, retrieval, re-certification и rollback analysis, а не как owner-defined observability object. [C2][C5][C14][C16][C17]

### 2.2 Место Experience Base в архитектуре Логоса

Experience Base занимает позицию между Execution Engine (производящим Traces) и Orchestrator (потребляющим накопленные паттерны при планировании). Он не является частью ни inference pipeline, ни session context — это отдельный persistent layer с собственным lifecycle. [C2][C14]

### 2.3 Три компонента `Experience Base`
| Компонент | Что хранит / делает | Временной горизонт | Роль в эволюции |
|-----------|---------------------|--------------------|-----------------|
| **Episodic Memory** | raw traces, summaries, success/failure cases, contextual constraints | кратко- и среднесрочный | даёт фактический material для reflection и retrieval |
| **Pattern Library** | validated strategies, anti-patterns, failure signatures, reusable heuristics | долгосрочный | переводит частный опыт в повторно используемое знание |
| **Reflection Engine** | post-run analysis, proposal generation, validation prompts, consolidation jobs | event-driven + batch | связывает прошлый опыт с future policy/memory updates |

Такая архитектура согласуется с evidence по elastic memory, graph-native memory, dual-outcome experience stores и online memory evolution. Главное свойство не в том, что память "большая", а в том, что она умеет различать raw evidence, reusable abstraction и update proposal. [C2][C14][C15][C18]

### 2.4 Накопление -> retrieval -> update
D10 принимает следующий базовый loop:
1. Миссия завершается и оставляет `Execution Trace`.
2. `Episodic Memory` сохраняет сырой эпизод, ключевые решения, side effects и outcome markers.
3. `Reflection Engine` выделяет success patterns, repeated errors, missed checkpoints и promising corrections.
4. Выделенные паттерны попадают в `Pattern Library` только после validation, а не автоматически.
5. При новой задаче planner и runtime retrieval обращаются сначала к релевантным patterns, затем к эпизодам, если нужна конкретика.
6. После нового исполнения loop повторяется и обновляет confidence тех паттернов, которые подтвердились или провалились.

Именно этот переход от "накопили историю" к "извлекли полезное" и затем к "обновили guidance" делает `Experience Base` механизмом evolution, а не архивом логов. [C1][C2][C14][C18]

### 2.5 Retrieval discipline
С ростом опыта memory layer легко становится либо слишком дорогой, либо слишком шумной. Поэтому `G11` в `v4.0` закрывается не обещанием бесконечной памяти, а правилами memory management:
- retrieval должен быть bounded-cost;
- успехи и провалы должны храниться раздельно, чтобы система не училась только на победах;
- repeated low-value episodes должны сжиматься сильнее, чем high-value failures и high-signal decisions;
- graph relations и typed links повышают auditability и rollback readiness;
- асинхронная consolidation допустима только при сохранении provenance. [C2][C14][C15]

Это означает, что D10 предпочитает hybrid memory architecture с разными уровнями абстракции, а не "длинный контекст вместо архитектуры памяти". [C14][C15]

### 2.6 `Reflection Engine` как bounded update layer
`Reflection Engine` не должен silently переписывать active policy. Его корректная роль в `v4.0`:
- формировать proposal;
- привязывать proposal к конкретным traces и failure patterns;
- запускать validation against safety, regressions и recent constraints;
- только после этого обновлять memory, knowledge layer или rollout candidate.

Такой proposal-first режим особенно важен, потому что evidence по self-evolving agents одновременно подтверждает пользу reflection loops и показывает риск alignment drift при неконтролируемых post-training updates. D10 не запрещает model-layer updates, но не считает их единственным или даже основным механизмом эволюции: практический приоритет здесь идёт от memory/policy/knowledge updates к tool/workflow evolution и только затем к model-layer update, если выгода превышает risk and re-cert cost. [C1][C3][C9][C10][C11][C18]

---
## 3. 4-слойное версионирование
### 3.1 Почему versioning является отдельным owner-блоком D10
Эволюция без versioning быстро превращается в неразличимую смесь изменений. D10 поэтому фиксирует versioning как control plane, который делает рост агента наблюдаемым, сравнимым и обратимым. Главное здесь не numbering ради numbering, а явное разделение behavior-bearing изменений по слоям. [C4][C15]

### 3.2 Четыре слоя
```text
Logos Version = Cognitive.Model.Knowledge.Tool
```
| Слой | Что меняется | Пример изменений | Почему выносится отдельно |
|------|--------------|------------------|---------------------------|
| **Cognitive** | reasoning policy, prompts, reflection heuristics, decision rules | новая planning heuristic, revised safety prompt, updated critique rubric | сильнее всего меняет поведение без смены base model |
| **Model** | provider/model family/adapters/weights | смена frontier model, новый adapter, LoRA refresh | самый дорогой по re-cert и alignment risk слой |
| **Knowledge** | memory structures, retrieval indexes, Pattern Library, domain bundles | новая consolidation logic, graph schema update, curated pattern set | stateful слой, критичен для continuity |
| **Tool** | tool manifests, bindings, versions, permissions, runtime contracts | новый MCP integration, changed tool schema, deprecated connector | меняет action surface и external effects |

Такой расклад поддержан evidence по formal memory/version semantics и agent versioning patterns, а также хорошо стыкуется с тем фактом, что agent evolution идёт не только через model updates. [C3][C4][C15]

### 3.3 Правило backward compatibility
`v4.0` использует следующее baseline-правило:
- minor update внутри слоя допустим, если совместим input/output contract и trace comparability не ломается;
- breaking change требует нового layer version и shadow validation;
- knowledge migration должна сохранять read access к предыдущим snapshots хотя бы для audit and rollback;
- tool updates не считаются безопасными "по умолчанию" только потому, что модель не менялась. [C3][C4][C15]

Именно knowledge and tool layers часто меняют фактическое поведение сильнее, чем косметический model refresh, поэтому D10 не ограничивает re-cert triggers только model-layer change. [C4][C5]

### 3.4 Immutable revisions и mutable pointers
Для stateful слоёв D10 предпочитает модель `immutable revisions + mutable pointers + typed dependency edges + provenance link от deployed state к validated evidence`. Это особенно полезно для knowledge layer, где rollback не означает "стереть историю", а означает "вернуться к прошлому валидному pointer при сохранении аудита происходившего". [C14][C15]

### 3.5 Rollback triggers
Rollback и freeze в D10 должны запускаться по явным условиям:

| Trigger | Что откатывается | Комментарий |
|---------|------------------|-------------|
| benchmark regression above tolerance | `Cognitive` / `Model` candidate | затрагивает capability evidence и re-cert pipeline |
| рост repeated error rate по уже решённым кейсам | `Knowledge` candidate | признак degraded retrieval или плохой consolidation |
| safety refusal / oversight metrics просели | `Cognitive` / `Model` / `Tool` | alignment drift важнее локального прироста utility |
| tool contract mismatch or unsafe permission expansion | `Tool` | откат обязателен до восстановления allowlisted surface |
| unexplained trace divergence | любой слой | отсутствие traceability делает update несертифицируемым |

Такие триггеры увязывают versioning с D04 observability и D02 re-certification, не смешивая ownership. [C4][C5][C15]

### 3.6 `G5` deferred note
> **`G5` deferred note.** Knowledge transfer между Логосами остаётся реальным эволюционным каналом, но D10 не раскрывает здесь его full contract semantics. Детали `Knowledge Transfer Package`, license types и exchange contract принадлежат `→ L3/D04 §5`. В D10 допустима только проекция: внешний knowledge import влияет на evolution pipeline только после provenance check, compatibility review и отдельной validation/rollout процедуры. [C3]

### 3.7 Что D10 не должен обещать
D10 не должен утверждать, что четырёхслойное versioning само по себе гарантирует безопасность. Оно лишь создаёт структуру, внутри которой можно сравнивать кандидат и baseline, доказуемо связывать update с evidence, откатывать или замораживать слой отдельно и запускать targeted re-certification вместо blind full restart. [C4][C5][C15]

---
## 4. Canary deployment и re-certification
### 4.1 Почему rollout и re-certification связаны
Если эволюция происходит непрерывно, то deploy и certification уже нельзя считать разными мирами. Rollout определяет, как новый state входит в production, а re-certification определяет, когда старый `Agent Capability VC` перестаёт быть достаточным evidence для нового состояния. Полная owner-семантика `Agent Capability VC` остаётся в `→ L1/D02 §3-§6`; D10 владеет только правилом, когда evolution invalidates or narrows its applicability. [C5][C6][C7]

### 4.2 Четыре фазы canary deployment
| Фаза | Объём трафика / задач | Что проверяется | Условие перехода |
|------|------------------------|-----------------|------------------|
| **Phase 1. Shadow** | 0% user-facing effects, parallel replay | trace parity, benchmark parity, no hidden schema breaks | candidate объяснимо повторяет baseline или улучшает его |
| **Phase 2. Canary** | малая доля low-risk workload | regressions, repeated failures, retrieval quality, oversight triggers | нет существенного ухудшения safety and quality envelope |
| **Phase 3. Graduated** | bounded domain scale-up | устойчивость при росте разнообразия и нагрузки | metrics держатся в agreed band across domains |
| **Phase 4. Full** | production default | retention, post-deploy monitoring, re-cert evidence completeness | candidate становится новым baseline только после retention window |

Эта схема ближе к agent systems, чем к plain web rollout: в каждой фазе нужно смотреть не только success rate, но и trace completeness, plan quality, oversight pressure и rollback readiness. [C4][C5][C17]

### 4.3 Re-certification matrix по типу изменений
| Тип изменения | Основной затронутый слой | Формат re-certification | Owner ref |
|---------------|--------------------------|-------------------------|-----------|
| **смена base model / adapter** | `Model` | полная re-certification capability bundle + safety re-check | `→ L1/D02 §3-§6` |
| **изменение reasoning policy / reflection heuristics** | `Cognitive` | domain-targeted benchmark rerun + shadow validation | `→ L1/D02 §4`, `§6` |
| **крупный memory schema или retrieval update** | `Knowledge` | selective re-certification на affected domains + regression pack | `→ L1/D02 §6` |
| **новый high-impact tool или permission expansion** | `Tool` | selective plus safety-focused re-certification до rollout | `→ L1/D02 §4-§6` |
| **чисто editorial knowledge refresh без behavior change evidence** | `Knowledge` | monitoring-first, с повышенной observability | `→ L1/D02 §6` |
| **incident / unexplained drift** | любой | emergency re-certification или capability freeze | `→ L1/D02 §6` |

Таблица намеренно не вводит universal TTL. D02 прямо фиксирует, что validated agent-specific decay interval пока не установлен, поэтому корректная логика в D10 — trigger-based and monitoring-first, а не "всегда каждые N дней". [C5][C6][C17]

### 4.4 Когда старый `Agent Capability VC` уже недостаточен
Старый capability credential следует считать ограниченно применимым или требующим повторной проверки, если новый state меняет benchmark family relevance, update затрагивает safety profile или planning behavior, changed tool surface радикально расширяет action space, production behavior больше не сопоставимо со snapshot, либо drift фиксируется в метриках и инцидентах. Иначе D10 создаёт ложную ситуацию, где credential описывает прошлый агентный state, а production уже давно работает как другой агент. [C5][C6][C17]

### 4.5 Continuous attestation и bounded certification
Корпус показывает convergence к двум рельсам: frequent attestation / monitoring и более редкая формальная re-certification. Практический baseline D10 отсюда такой: каждое значимое обновление должно оставлять attestation-friendly evidence chain, capability credential должен указывать snapshot date и benchmark scope, canary не завершается без evidence для возможной повторной выдачи credential, а runtime monitoring не заменяет сертификацию, но предотвращает long silent drift. [C5][C6][C17]

### 4.6 EU AI Act Art.12 / Art.17
Полный regulatory owner остаётся в `→ L2/D08 §1`, `§4`, `§5`. В D10 важна только проекция:
- `Art.12` усиливает requirement на record-keeping, а значит every evolution step должен быть traceable;
- `Art.17` усиливает requirement на quality-management discipline, а значит rollout and re-cert cannot be ad hoc;
- если adaptive changes были заранее определены и оценены в initial conformity envelope, они могут оставаться внутри существующего assessment boundary;
- если self-update выходит за pre-determined and pre-assessed envelope, D10 должен поднимать regulatory reassessment flag и эскалировать на owner-layer `→ L2/D08 §1`, `§4`, `§5`. [C7]

Значит, "непрерывно обучается" не является индульгенцией. Для high-risk posture continuous adaptation допустима только вместе с предопределёнными пределами, auditability и явной точкой эскалации на reassessment. [C7][C17]

---
## 5. Safety guardrails при эволюции
### 5.1 Почему D10 не может обещать safe growth by default
Evidence по self-modification и post-training safety идёт в двух направлениях одновременно: controlled reflective updates действительно способны повышать utility и resilience, но unsafe or narrow adaptation может порождать generalized misalignment, которая не остаётся локальной для одного домена. Поэтому D10 в `v4.0` исходит не из тезиса "эволюция полезна", а из более строгой формулы: эволюция допустима только как bounded, typed and reversible process. [C8][C9][C10][C11][C12]

### 5.2 Defense-in-depth around updates
Минимальный guardrail stack для любого эволюционного candidate:

| Слой | Что проверяет | Зачем нужен |
|------|---------------|-------------|
| **proposal gate** | есть ли trace-backed rationale и expected benefit | отсекает произвольные self-updates |
| **policy gate** | нарушает ли update owner values, forbidden actions, domain bounds | удерживает value anchoring и `Non-delegable core` |
| **safety gate** | ухудшились ли refusal, moderation, harmful-task metrics | ловит alignment drift до production |
| **rollout gate** | допускается ли canary scope и rollback plan | не даёт risky change попасть сразу в full production |
| **monitoring gate** | есть ли post-deploy drift signals и escalation hooks | превращает guardrails в continuous process |

Здесь D10 опирается на `HITL Gate` policy from `→ L1/D07 §5` и execution traceability from `→ L1/D04 §7`, не перехватывая их ownership. [C5][C9][C17]

### 5.3 `G12` conflict note: `Two-Gate` vs impossibility
> **`G12` conflict note (explicit).** Корпус не даёт права утверждать универсальную безопасность self-modification. С одной стороны, bounded-capacity `Two-Gate` conditions показывают, что safe self-update возможен, если proposals проходят validation и reachable model family остаётся capacity-bounded. С другой стороны, impossibility results показывают, что classifier-based gates не могут безопасно разрешать unbounded beneficial self-modification under power-law risk schedules. D10 therefore keeps both claims simultaneously and treats the conflict as real, not as a wording issue. [C8][C12]

Практическое следствие для `v4.0` жёсткое:
- D10 допускает только bounded self-update families;
- любое утверждение про "безопасное неограниченное самосовершенствование" считается unsupported;
- если candidate требует выхода за bounds, он попадает не в обычный rollout, а в research-only PoC posture. [C8][C12]

### 5.4 Value anchoring
Даже полезный update неприемлем, если он смещает ценностные ограничения владельца или платформы. Поэтому D10 фиксирует три anchors: owner intent and domain scope from delegated autonomy envelope `→ L1/D07 §2-§5`, preserved traceability of decisions and update rationale `→ L1/D04 §7`, а также no-update zone around `Non-delegable core` and policy-forbidden behaviors `→ L1/D07 §4`. Это важно, потому что alignment drift чаще проявляется не как мгновенный полный отказ от правил, а как локальная оптимизация, которая постепенно размывает refusal, oversight sensitivity или scope discipline. [C5][C9][C10][C11]

### 5.5 Alignment drift detection
D10 считает обязательными минимум следующие drift signals:
- неожиданное падение refusal quality на harmful or out-of-scope tasks;
- рост override rate со стороны человека при сопоставимых задачах;
- рост unexplained plan divergence relative to previous validated baseline;
- повторное появление уже закрытых unsafe patterns;
- улучшение utility metrics ценой ухудшения safety metrics. [C5][C10][C11]

Такие сигналы должны идти в same dashboard and rollout loop, а не быть manual afterthought. [C5][C17]

### 5.6 Design note for `v4.0`
Для текущего черновика безопасный эволюционный baseline формулируется так:
```text
safe evolution = proposal-first
               + bounded update family
               + trace-backed validation
               + canary rollout
               + rollbackability
               + human/governance escalation
```

Это не математическое доказательство полной безопасности. Это минимально честная архитектурная позиция, совместимая и с позитивными результатами по guarded self-update, и с негативными результатами про невозможность универсального gate-based safety для unbounded self-modification. [C8][C9][C12]

---
## 6. Negative transfer и preference contamination
### 6.1 Почему это отдельная секция
Эволюция ломается не только через явные safety breaches. Есть более тихий failure mode: агент улучшается в одном месте, но деградирует в другом. D10 разделяет два риска: `negative transfer`, где новый adaptation path ухудшает downstream performance outside target setting, и `preference contamination`, где post-training or RL process повреждает preference-sensitive behavior, policy entropy или alignment profile. Эти два риска нельзя лечить одним словом "retraining"; им нужны разные контрольные механизмы. [C10][C11][C16]

### 6.2 Два базовых контроллера
| Риск | Baseline control | Почему именно он |
|------|------------------|------------------|
| **negative transfer** | no-worse-than-scratch style update screening | evolution не должна считаться успехом, если вне target domain всё стало хуже |
| **preference contamination** | entropy / collapse monitoring during RL or post-training | contamination может проявиться как policy collapse before visible catastrophic failure |

Evidence по `REFINE` поддерживает идею updates, которые как минимум не должны быть хуже scratch baseline, а evidence по `Self-Critique` показывает, что contamination detection требует специальных signals, а не только task score. [C16]

### 6.3 Operational protocol for `negative transfer`
В `v4.0` D10 принимает такой baseline:
1. каждый candidate update объявляет целевой домен улучшения;
2. перед rollout проверяется no-regression set по смежным доменам;
3. если target improvement получен ценой падения в safety-critical adjacent domain, candidate блокируется или дробится на narrower scope;
4. pattern extracted from one domain получает bounded applicability tag, пока не накоплено cross-domain evidence. [C5][C16]

Это особенно важно для `Pattern Library`: reusable pattern без domain tags легко превращается в источник ложных переносов. [C2][C16]

### 6.4 Preference contamination and unresolved `Preference Model`
`Preference Model` присутствует в historical continuity D10, но в `v4.0` не считается fully stabilized component `Experience Base`, потому что corpus показывает риск contamination without giving a settled Logos-specific protocol. Отсюда позиция D10: preference updates должны versioned separately, RL/post-training updates обязаны сопровождаться contamination checks, owner values и stable safety preferences не должны silently переписываться reflective loop'ом, а preference-sensitive updates без explicit audit не допускаются в full rollout. [C10][C11][C16]

### 6.5 Inter-axis tradeoffs
Негативный перенос часто выглядит как tradeoff между четырьмя осями из §1:

| Tradeoff | Типичный ложный выигрыш | Чем грозит |
|----------|-------------------------|------------|
| **capability vs autonomy** | агент решает больше задач и кажется "готовым" к higher `LOA` | рост capability не доказывает готовность к более редкому oversight |
| **memory richness vs safety** | больше retrieval coverage повышает success rate | retrieval может тащить устаревшие или unsafe patterns |
| **economic efficiency vs re-cert discipline** | редкие проверки снижают cost | drift остаётся незамеченным дольше |
| **fast rollout vs compatibility** | обновление быстро доходит до production | ломаются rollback and audit guarantees |

Таблица намеренно не задаёт формулу весов. Она лишь фиксирует, что "улучшение" без axis-aware tradeoff review не должно считаться полным эволюционным прогрессом. [C5][C13][C16][C17]

### 6.6 `G13` resolve-in-draft note
> **`G13` resolve-in-draft note.** В `v4.0` D10 закрывает только baseline distinction and controls: `negative transfer` требует no-regression discipline, `preference contamination` требует dedicated detection signals. Но полноценный Logos-specific protocol для `Preference Model`, threshold values, remediation workflow и quarantine path пока не завершён и выносится в `Открытые вопросы`. [C16]

### 6.7 Допустимые remediation paths
Если contamination или transfer failure detected, D10 допускает четыре response types: откат affected layer version, сужение domain scope candidate update, перевод candidate обратно в canary-only posture и emergency freeze с повторной сертификацией capability-sensitive surfaces. Это подчёркивает: эволюция в D10 должна быть reversible enough to stop, not only powerful enough to change. [C4][C5][C16]

---
## 7. Human oversight, метрики и governance
### 7.1 Oversight boundary
В D10 human oversight не означает ручное одобрение каждого learning event. По мере роста зрелости корректный режим смещается от dense approvals к exception-driven oversight, но человек и governance не исчезают из control loop. Полная owner-логика `LOA`, `HITL Gate` и `Earned autonomy` находится в `→ L1/D07 §3-§5`; D10 использует её как рамку для того, какие evolution events могут идти автоматически, а какие требуют escalation. [C5][C17]

### 7.2 Три роли oversight в эволюции
| Роль | Что делает | Где owner semantics |
|------|------------|--------------------|
| **Владелец / deployer** | подтверждает high-impact evolution decisions, rollback acceptance, scope expansion | `→ L1/D07 §2-§5` |
| **Assessor / oracle** | запускает benchmarks, attestation and re-cert procedures | `→ L1/D02 §4-§6` |
| **Platform governance / compliance** | следит за reassessment boundaries, registry hygiene, policy escalation | `→ L2/D08 §1`, `§4`, `§5` |

Эта тройка нужна, чтобы evolution governance не свелась либо к "всё решает модель", либо к "всё делает человек вручную". [C5][C6][C7][C17]

### 7.3 `ESCALATE` / `OLEA` posture
Как design input для `v4.0`, D10 использует два governance patterns: `ESCALATE`-style explicit approval triggers для production deployments, privilege changes, high-cost or risky updates и `OLEA`-style lifecycle governance, где registry и stage transitions не дают agent sprawl размыть ownership and monitoring. Внутри D10 это означает, что evolution events должны иметь явные escalation conditions, stage transitions и registry-visible state, а не происходить как скрытая фоновая "самонастройка". [C17]

### 7.4 Evolution Dashboard: 13 KPI
Для `G8` D10 фиксирует `Evolution Dashboard` как owner-level aggregation surface из 13 KPI:

| № | KPI | Что показывает |
|---|-----|----------------|
| **1** | `benchmark_delta` | насколько candidate реально улучшает capability relative to baseline |
| **2** | `domain_coverage_growth` | в скольких доменах capability подтверждена, а не только заявлена |
| **3** | `pattern_reuse_rate` | как часто validated patterns действительно помогают в новых задачах |
| **4** | `repeat_error_rate` | возвращаются ли уже известные ошибки после updates |
| **5** | `retrieval_hit_quality` | насколько retrieval доставляет релевантный опыт вместо шума |
| **6** | `memory_staleness_ratio` | доля устаревших patterns и episodes в active retrieval set |
| **7** | `canary_survival_rate` | какая доля candidates проходит все 4 фазы rollout |
| **8** | `rollback_frequency` | как часто updates приходится откатывать |
| **9** | `re_cert_cycle_time` | сколько времени занимает подтверждение новой capability state |
| **10** | `human_override_rate` | как часто evolution-driven behavior требует ручного вмешательства |
| **11** | `autonomy_incident_rate` | растут ли инциденты при тех же `LOA` и scope settings |
| **12** | `regulatory_reassessment_count` | как часто updates выходят за pre-assessed compliance envelope |
| **13** | `safety_drift_index` | меняется ли safety / refusal / harmful-task profile после updates |

Эти KPI смотрят не только на success. D10 считает эволюцию качественной лишь тогда, когда рост capability совместим с rollbackability, oversight, traceability and compliance posture. [C5][C7][C17]

### 7.5 Bridge к `Earned autonomy`
`Earned autonomy` fully принадлежит `→ L1/D07 §3`, но D10 добавляет эволюционную проекцию: устойчивый рост capability и memory maturity может быть input в повышение eligibility, но не может автоматически повышать `LOA`. Любой bridge должен проходить через reputation and oversight side:
- capability growth как необходимое, но недостаточное условие;
- history of safe rollouts and low override rate как дополнительный input;
- final decision всё равно остаётся актом делегирования, а не актом самообновления агента. [C5][C17]

### 7.6 Regulatory re-assessment `G9`
`G9` в `v4.0` закрывается как trigger map, а не как full legal doctrine:

| Событие эволюции | Что делает D10 | Куда эскалирует |
|------------------|----------------|-----------------|
| update остаётся внутри pre-defined adaptation envelope | продолжает canary + monitoring | без обязательной новой legal ветки |
| update меняет risk posture или human-oversight assumptions | поднимает reassessment flag | `→ L2/D08 §1`, `§4`, `§5` |
| update затрагивает high-impact domains / automated decisions | требует stricter review before full rollout | `→ L2/D08 §1`, `§4` |
| update меняет contract / smart tool semantics materially | добавляет product-side and contract-side review | `→ L2/D08 §5` |

Таким образом, D10 не дублирует legal framework, а делает его operationally usable внутри эволюционного pipeline. [C7][C17]

### 7.7 Governance baseline
Сводный governance baseline для D10 `v4.0` выглядит так:
1. каждый evolution step versioned;
2. каждый candidate trace-backed;
3. каждый significant update проходит canary;
4. re-certification запускается trigger-based, not folklore-based;
5. high-risk drift ведёт к rollback or freeze;
6. compliance boundary имеет явную точку эскалации;
7. рост capability не равен автоматическому росту автономии. [C4][C5][C6][C7][C17]

Именно эта семёрка даёт D10 практическую форму ответа на ключевой вопрос: Логос развивается не потому, что "умеет учиться вообще", а потому, что у него есть наблюдаемый, versioned, bounded and governable learning loop. [C1][C2][C4][C18]

---
## Связь с другими измерениями
| Измерение | Связь |
|-----------|-------|
| **L1/D02 (Компетентностное)** | `Human Evolution` и `Agent Evolution` сходятся в competence architecture, но `Agent Capability VC`, benchmark semantics и re-certification ownership принадлежат `→ L1/D02 §3-§6`. |
| **L1/D04 (Функциональное)** | `Execution Trace` fully принадлежит `→ L1/D04 §7`; в D10 он выступает сырьём для `Experience Base`, reflection и rollback analysis. |
| **L1/D09 (Репутационное)** | reputation decay и tiers влияют на устойчивость эволюции и downstream trust, но не подменяют capability evidence; reputational bridge к autonomy находится в `→ L1/D09 §5`. |
| **L1/D07 (Автономность)** | `LOA`, `HITL Gate` и `Earned autonomy` задают допустимый режим эволюции и oversight density, но owner-описание остаётся в `→ L1/D07 §1-§5`. |
| **L2/D08 (Нормативно-правовое)** | D10 operationalizes regulatory triggers для evolution pipeline, но full analysis AI Act / GDPR / PLD остаётся в `→ L2/D08 §1`, `§4`, `§5`. |
| **L3/D04 (Контрактно-договорное)** | Knowledge transfer between agents влияет на эволюцию как внешний input, но `Knowledge Transfer Package` fully описан в `→ L3/D04 §5`. |

---
## Открытые вопросы
### Требуют решения (Critical / High)
1. **[Critical] `G12`: нужен production-grade PoC для bounded self-modification safety.** Сейчас D10 честно удерживает и позитивный `Two-Gate` result, и impossibility bound, но это ещё не превращено в platform-ready validation pipeline.
   > **Open.** Нужен PoC с типизированными update families, explicit capacity bounds, zero-ambiguity rollback path и empirical falsification tests before any broader self-update posture. [C8][C12]

2. **[High] Не зафиксирован единый trigger policy для перехода от monitoring к full re-certification.** Корпус поддерживает monitoring-first posture, но не даёт validated universal cadence.
   > **Open.** Требуется governance profile по risk tier, layer type и domain criticality поверх `→ L1/D02 §6`. [C5][C6]

### Требуют исследования (Medium)
3. **[Medium] `G13`: Logos-specific protocol для `Preference Model` и contamination remediation остаётся незавершённым.**
   > **Open.** Нужны threshold rules, quarantine path, version isolation и explicit owner review semantics для preference-sensitive updates. [C10][C11][C16]

4. **[Medium] `G10`: inter-axis weights и tradeoff policy пока не имеют канонической формулы.**
   > **Open.** В `v4.0` оси определены, но weighting, tie-break rules и multi-axis promotion criteria должны быть вынесены в governance layer, а не импровизироваться per update. [C13][C17]

5. **[Medium] `G5`: knowledge transfer details намеренно отложены.**
   > **Deferred:** D10 использует only projection на внешний knowledge import; full lifecycle, contract semantics и licensing принадлежат `→ L3/D04 §5`. [C3]
