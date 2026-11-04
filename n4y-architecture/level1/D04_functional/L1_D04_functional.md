# УРОВЕНЬ 1 / ИЗМЕРЕНИЕ 04: ФУНКЦИОНАЛЬНО-ИНСТРУМЕНТАЛЬНОЕ

> **Уровень:** L1 — Агент (свойства Логоса)  
> **Измерение:** D04 — Functional-Instrumental  
> **Версия:** 4.0  
> **Статус:** Финал v4.0 | Дата: 02.04.2026  
> **Зависимости:** L1/D01, L1/D02, L1/D03, L1/D05, L1/D07, L1/D08

---

## Ключевой вопрос

**Какие прикладные задачи ты способен решать и как ты их выполняешь?**

---

## Аннотация

Прямой ответ на вопрос измерения таков: Логос решает прикладные задачи через `Orchestrator`, который классифицирует запрос, строит план, удерживает рабочий контекст, вызывает инструменты через `MCP`, управляет ошибками и оставляет `Execution Trace`. Внутри D04 это оформляется как таксономия задач A-D, архитектура `Task Router -> Planner -> Context Manager -> Execution Engine`, набор оркестрационных паттернов и контрольный слой `HITL Gate` для случаев, где автономное исполнение должно быть остановлено или подтверждено. [C2][C3][C4][C5][C8][C13]

Документ фиксирует именно функциональный контур выполнения. `DID`-аутентификация остаётся проекцией идентификационного слоя `→ L1/D01 §1, §6`; смысл `ESq`/`ESc` и competence-side gate остаются проекцией `→ L1/D02 §5.5`; resource substrate, storage/backends и failover остаются проекцией `→ L1/D03 §3-§7`; `A2A` и межагентная семантика остаются проекцией `→ L1/D05 §4-§6`; `LOA` и policy thresholds остаются проекцией `→ L1/D07 §1-§2`; `Budget Controller` и `Dual Constraint` остаются проекцией `→ L1/D08 §2, §6`. Числа из стратегических примеров, если они появляются как иллюстрация масштаба миссий, трактуются только как `[стратегия: вектор]`. [C8][C14][C16][C21]

---

## 1. Таксономия задач Логоса A-D

### 1.1 Почему таксономия здесь является authored synthesis

Корпус не показывает одного внешнего стандарта, который уже дал бы готовую четырёхклассовую схему `A/B/C/D` именно для Логоса. Поэтому D04 фиксирует её как собственный рабочий синтез поверх современных routing patterns: от атомарных задач к многошаговым, иерархическим и long-running миссиям. Это не конфликт с evidence, а требуемое проектное решение уровня D04. [C3][C4]

### 1.2 Decision tree для `Task Router`

`Task Router` должен отвечать на пять вопросов в указанном порядке:

1. Можно ли закрыть запрос одним действием или одним tool call без явного плана?
2. Нужны ли несколько шагов, но внутри одного исполнителя и одного thread state?
3. Нужны ли несколько исполнителей, независимые ветви или `Structured Handoff`?
4. Требуется ли cross-session state, checkpointing, длительное ожидание или повторный вход в задачу?
5. Есть ли условия, которые делают маршрут cost-aware: budget cap, quality floor, tool availability, policy limits?

Если ответы заканчиваются на вопросах 1-2, задача попадает в класс `A` или `B`. Если требуется координация нескольких исполнителей, задача переходит в `C`. Если нужен долгоживущий mission loop с checkpoint/resume, задача классифицируется как `D`. Cost/quality-aware routing включается поверх всех классов, а не вместо них. [C3][C4][C5][C21]

### 1.3 Классы задач, примеры и критерии успеха

| Класс | Функциональный профиль | Типичный паттерн | Пример | Критерий успеха |
|------|-------------------------|------------------|--------|-----------------|
| **A** | Атомарная задача: один исполнитель, один основной вызов, почти нет ветвления | `direct invoke` / короткий `ReAct` loop | получить статус заказа, сделать поиск, извлечь один факт, вызвать один API | ответ корректен, schema-valid, tool result подтверждён, лишний план не строится |
| **B** | Композитная задача: один исполнитель, несколько шагов или tool calls, но без межагентной координации | `plan-and-execute` / последовательный pipeline | собрать данные, нормализовать, затем сгенерировать отчёт; подготовить бронирование через несколько API | все обязательные шаги выполнены, промежуточные результаты валидированы, контекст удержан в одном thread |
| **C** | Оркестрированная задача: несколько исполнителей, DAG зависимостей, handoff между ролями | hierarchical orchestration + parallel branches | исследование -> анализ -> drafting -> review; travel mission с несколькими микроагентами | завершены все критичные subtask nodes, handoff валиден, failures локализованы, итог собирается в единый результат |
| **D** | Автономная миссия: long-running, cross-session, с checkpoint/resume, re-planning и ожиданием внешних событий | adaptive hierarchical mission loop | многодневный workflow, корпоративное сопровождение, фоновые проверки с повторным входом | задача проходит через несколько сессий без потери state, checkpoints консистентны, условия `HITL Gate` соблюдены, mission можно безопасно возобновить |

Эта шкала не равна шкале `LOA`: класс задачи отвечает на вопрос "какой execution shape нужен?", а `LOA` отвечает на вопрос "какой уровень самостоятельности разрешён?". Полная шкала `LOA` и policy ownership находятся в `→ L1/D07 §1-§2`. [C3][C4][C8]

### 1.4 Практический operational baseline

Типовые примеры читаются так: `A` — один retrieval/status/tool call; `B` — один исполнитель с несколькими последовательными шагами; `C` — несколько исполнителей с handoff и aggregation; `D` — long-running mission с pause/resume. Неуспехом для D04 считается не только явный `failed`, но и неверная классификация пути, избыточно дорогой маршрут, запуск без checkpoint discipline или пропуск обязательного `HITL Gate`. [C1][C3][C4][C5][C6][C8][C19][C21]

---

## 2. Архитектура Orchestrator

### 2.1 Базовый тезис

`Orchestrator` в D04 определяется как связка четырёх обязательных частей: `Task Router`, `Planner`, `Context Manager`, `Execution Engine`. Именно эта связка превращает Логос из набора отдельных умений в исполняющую систему, способную принять произвольный запрос, выбрать класс задачи, удержать состояние и довести выполнение до результата или управляемого отказа. [C2][C5][C6]

### 2.2 Четыре компонента

| Компонент | Что делает | Что не принадлежит компоненту |
|----------|------------|-------------------------------|
| **Task Router** | Классифицирует запрос, оценивает сложность, budget/quality envelope, выбирает класс `A-D` и стартовый execution path | не владеет `LOA` policy и не определяет legal scope; это проекция `→ L1/D07 §1-§2` |
| **Planner** | Строит DAG, определяет dependency edges, условия параллелизма, критерии re-plan и verification checkpoints | не владеет benchmark semantics и `ESq/ESc` mapping; это проекция `→ L1/D02 §5.5` |
| **Context Manager** | Удерживает thread state, summaries, episodic/semantic abstractions, cross-session continuity | не владеет storage substrate и provider internals; это проекция `→ L1/D03 §4-§7` |
| **Execution Engine** | Вызывает tools/agents, контролирует status, retries, compensations, audit trail и итоговый `Execution Trace` | не владеет `Budget Controller`; бюджетный контроль остаётся в `→ L1/D08 §2` |

### 2.3 Базовый flow данных и сигналов

Поток исполнения в D04 должен быть понятен как цепочка сигналов, а не как "магия агента":

1. `Task Router` получает входной запрос и собирает routing context: intent, constraints, budget envelope, available tools, allowed autonomy. [C4][C16]
2. На основе routing outcome задача либо идёт сразу в `Execution Engine` (`A`), либо в `Planner` (`B-D`). [C3][C5]
3. `Planner` формирует DAG, помечает обязательные проверки, потенциальные `HITL Gate` и точки `checkpoint`. [C5][C7][C8]
4. `Context Manager` выделяет нужный рабочий state: thread buffer, summary, episodic memory, retrieved semantics. [C9][C10][C11]
5. `Execution Engine` вызывает `MCP` tools, запускает workers, применяет retries, compensations и handoff validation. [C1][C7][C18][C19]
6. Наблюдаемость и trace-поток пишутся по ходу исполнения, а не только в конце: это нужно для debugging, audit и user explanation. [C13][C15]

### 2.4 Границы ответственности

В v4.0 `Orchestrator` больше не смешивается с одним `Router`: выбор пути, построение плана, удержание state и фактическое исполнение разнесены по четырём компонентам, потому что routing errors, state drift и side-effect failures происходят на разных стадиях. При этом `DID`-auth потребляется из `→ L1/D01 §6`, competence-side gate `ESq >= ESc` — из `→ L1/D02 §5.5`, material routing и storage internals — из `→ L1/D03 §3-§7`, а versioning микроагентов по `G9` сознательно deferred в `→ L1/D10 §4.3`. [C2][C4][C6][C7][C9][C16][C21]

---

## 3. MCP tool integration и security

### 3.1 Почему `MCP` является каноническим tool protocol для D04

Для D04 `MCP` остаётся каноническим протоколом agent-to-tool взаимодействия: он даёт клиент-серверную модель, primitives `Tools / Resources / Prompts`, transport/runtime evolution и единый operational surface для работы `Execution Engine`. При этом он не заменяет `A2A`: взаимодействие agent-to-agent остаётся отдельным контуром `→ L1/D05 §4-§6`. [C1][C14]

### 3.2 Что именно берётся из spec `2025-11-25`

Для D04 operationally значимы следующие зафиксированные элементы:

- `Streamable HTTP` как основной шаг к более масштабируемому remote transport; [C1]
- JSON-RPC batching для снижения orchestration overhead при сериях вызовов; [C1]
- formalized authorization model с внешним OAuth 2.1 слоем; [C16]
- tool annotations как risk vocabulary, но не как security guarantee. [C17]

Важно различать ratified и forward-looking части. Всё, что относится к `Server Cards`, `.well-known/mcp.json`, `Tasks` primitive и части roadmap вокруг enterprise/gateway maturation, следует считать только направлением развития, а не уже обязательным базисом. Это нужно помечать прямо в тексте, потому что соответствующие части `E31` содержат roadmap material. [C1][C20]

### 3.3 Security baseline: OAuth 2.1 и доверенный вызов

`MCP` сам по себе не даёт встроенной caller identity; поэтому D04 должен считать обязательным внешний authorization layer:

- `OAuth 2.1`;
- `Protected Resource Metadata`;
- `Resource Indicators`;
- `PKCE`;
- `HTTPS` для remote servers.

Из этого следует важная граница: `Tool Manifest` и `readOnlyHint`/`destructiveHint` помогают принять решение об исполнении, но не подменяют собой authentication, authorization и policy checks. [C16][C17]

### 3.4 Локальная шкала trust levels для `Execution Engine`

Протокол `MCP` не навязывает единую шкалу доверия к tool servers, поэтому D04 фиксирует локальный runtime profile из пяти уровней:

| Trust level | Operational meaning | Допуск в D04 |
|------------|---------------------|--------------|
| **0** | Unknown / ad-hoc server без проверенного owner и без устойчивого auth profile | только isolated discovery, без production execution |
| **1** | Read-mostly / sandbox server | допустим для retrieval и non-destructive read paths |
| **2** | Authenticated external server | допустим для bounded write operations при явной policy проверке |
| **3** | Approved partner server | допустим для domain workflows с audit и rollback discipline |
| **4** | Internal / mission-critical server | допустим для critical orchestration paths, если есть trace, policy и fallback |

Эта шкала является design policy Логоса, а не свойством самого стандарта `MCP`; она выводится из требований к OAuth-based authorization, advisory annotations и registry-backed trust mediation. [C16][C17][C20]

### 3.5 Lifecycle и discovery

Для D04 жизненный цикл инструмента читается как `register/discover -> auth bind -> Tool Manifest -> runtime use -> suspend/revoke`. `Tool Manifest` нужен именно затем, чтобы `Execution Engine` не работал с произвольным найденным tool. Устойчивое направление развития — переход от static configs к registry-backed discovery и gateway mediation, но `.well-known` metadata, `Server Cards` и `Tasks` primitive пока нужно описывать как forward-looking discovery layer, а не как уже обязательный baseline. [C17][C18][C20] *(часть `E31` — early-stage / roadmap)*.

---

## 4. Оркестрационные паттерны

### 4.1 Матрица паттернов по классам задач

| Класс | Основной паттерн | Когда он уместен | Что ломает паттерн |
|------|-------------------|------------------|--------------------|
| **A** | direct execution / micro-`ReAct` | одна цель, один основной вызов, минимум state | лишний planning overhead и ненужный delegation |
| **B** | sequential pipeline | несколько шагов одного исполнителя, порядок важен | попытка вести много ветвей без DAG discipline |
| **C** | hierarchical orchestration | есть набор ролей, handoff, aggregation, shared constraints | отсутствие `Structured Handoff` и status reconciliation |
| **D** | adaptive hierarchical mission | long-running workflow, waiting states, re-entry, external events | запуск без checkpointing и persistent state |

Паттерн выбирается не по "красоте архитектуры", а по структуре задачи. D04 не должен принуждать все задачи к multi-agent execution только потому, что это выглядит более агентно. [C3][C4][C5]

### 4.2 DAG как каноническая форма плана

Для классов `B-D` план должен читаться как DAG:

- node = атомарный шаг, worker subtask или critical checkpoint;
- edge = dependency / precedence / data requirement;
- branch = параллельная ветка, если нет hard dependency;
- barrier = точка синхронизации перед aggregation;
- interrupt node = обязательная остановка на `HITL Gate`.

Это позволяет явно выражать parallelism, а не держать его "в голове модели". Именно поэтому `Planner` и `Execution Engine` разделены: один управляет графом, другой управляет фактическим прогрессом узлов графа. [C5][C6][C8][C12]

### 4.3 Параллелизм и `Structured Handoff`

Параллелизм в D04 допустим, только если соблюдены три условия:

1. ветви не конкурируют за один и тот же mutable state;
2. их outputs имеют явный merge contract;
3. handoff между ветвями проходит через `Structured Handoff`, а не через неформальный текстовый пересказ.

`Structured Handoff` нужен потому, что cascade failures в agent systems часто начинаются не в конце, а на ранней передаче контекста между частями графа. Поэтому handoff должен переносить не только payload, но и provenance: что было вызвано, чем подтверждено, какие ограничения ещё действуют. [C14][C18][C19]

### 4.4 `Saga Pattern`, checkpoint и rollback

`Saga Pattern` остаётся каноническим error-handling pattern D04 для многошаговых задач с побочными эффектами. Его смысл в том, что Логос не надеется на "магический atomic commit" across tools, а заранее проектирует compensating actions. [C7]

Практическая дисциплина выглядит так:

- перед необратимым или дорогостоящим действием ставится `checkpoint`; [C6][C7]
- после действия в trace фиксируется его side effect и возможная compensation; [C13]
- при ошибке выполняется либо local retry/backoff, либо compensating action, либо re-plan с последнего валидного checkpoint; [C7][C12]
- `rollback` означает возврат orchestration state, но не отменяет физически уже совершённый внешний side effect без явной compensation. [C7]

Одна и та же миссия может по ходу исполнения пройти путь `A -> B -> C -> D`: сначала как простой вызов, затем как pipeline, потом как multi-agent DAG и, наконец, как long-running mission. Поэтому класс задачи не является вечной меткой; `Planner` должен переоценивать execution shape по новым observations, а `Context Manager` — удерживать continuity между такими переходами. [C3][C5][C9][C12]

---

## 5. Context Manager и state

### 5.1 Почему контекст в D04 - это не просто conversation buffer

Если `Context Manager` хранит только чат-историю, D04 ломается на задачах `B-D`: появляются retrieval collapse, re-summarization overhead, потеря промежуточных решений и невозможность безопасно вернуться в миссию после паузы. Поэтому контекст должен быть layered. [C9][C10][C11]

### 5.2 Иерархия памяти

В D04 полезно разделять operational state и long-term memory hierarchy:

| Слой | Роль |
|------|------|
| **Thread state** | текущий run state, последние messages, current node, pending interrupts |
| **Messages** | сырые сообщения и tool outputs для локального reconstruction |
| **Episodes** | завершённые эпизоды: что произошло, чем закончилось, какие side effects были |
| **Semantics** | обобщённые факты, preferences, reusable constraints |
| **Themes** | долгоживущие паттерны, recurring intents, cross-mission abstractions |

Слои `Messages -> Episodes -> Semantics -> Themes` соответствуют xMemory-style hierarchy и нужны для того, чтобы retrieval шёл не по сплошному списку сообщений, а по progressively abstracted memory. [C9][C10]

### 5.3 Auto-summarization

Для production D04 summary не должен переписывать весь контекст заново на каждом шаге. Вместо этого нужен `RunningSummary`-подход:

- суммаризация запускается threshold-triggered, а не каждый turn; [C11]
- summary хранит только то, что выпало из активного окна; [C11]
- в state фиксируется, что уже было summarised, чтобы избежать повторной компрессии; [C11]
- low-value and redundant history может быть селективно сжата сильнее, чем high-value decisions and constraints. [C10][C11]

Такая схема согласуется и с LangMem-style summarization, и с evidence по memory compression: экономия токенов допустима только тогда, когда summary сохраняет decisions, constraints, unresolved blockers и next steps. [C10][C11]

### 5.4 Cross-session persistence и минимальный state object

Cross-session persistence нужна для mission recovery, continuity задач класса `D` и reuse of stable preferences/patterns; D04 при этом владеет только логикой what-to-persist, а не physical storage substrate `→ L1/D03 §4-§7`. Минимальный state object должен содержать objective, task class, plan snapshot, unresolved blockers, applicable `HITL Gate`, validated receipts/handoffs, summary prior steps и pointer на trace; иначе `Planner` и `Execution Engine` вынуждены заново восстанавливать ситуацию из длинной истории. [C5][C6][C9][C10][C13]

---

## 6. Tool hallucination, HITL и error handling

### 6.1 Tool hallucination как отдельный runtime-risk

Tool hallucination в D04 надо считать отдельной категорией риска: это не просто "модель ошиблась", а ситуация, где execution layer может попытаться вызвать несуществующий tool, сослаться на невыполненное действие или передать downstream ложный результат. Поэтому mitigation должен быть многослойным. [C18][C19]

### 6.2 Многослойная защита

| Слой | Контроль | Что ловит |
|------|----------|-----------|
| **Pre-execution** | schema validation, allowlists, `AEGIS`-style policy firewall | phantom tools, invalid params, policy-breaching calls |
| **Execution-time** | receipt-based verification (`NabaOS` pattern), tool ID binding, registry cross-check | ложные ссылки на якобы выполненные tool calls |
| **Post-execution** | `HaluGate`-style grounding checks, output-to-receipt validation | fabricated references и ungrounded statements в ответе |
| **Boundary / handoff** | handoff validator + schema/receipt checks | cascade failures между агентами и стадиями пайплайна |

Такой стек соответствует корпусу лучше, чем один "универсальный антигаллюцинационный фильтр": разные меры закрывают разные failure modes. [C18][C19]

### 6.3 Baseline и `HITL Gate`

Для `Execution Engine` уже можно фиксировать baseline: любой tool call outside known schema -> reject; любой output без receipt/provenance -> suspect; любой handoff без подтверждённого происхождения -> block or re-plan; любой high-risk tool call без policy pass -> stop before execution. `mcp-hallucination-suite` полезен как pattern library, но не как единственный anchor дизайна. [C16][C17][C18][C19] *(часть `E26` — early-stage open-source evidence; использовать только как supplementary pattern)*.

`HITL Gate` в D04 описывается как execution mechanism остановки и возобновления run: gate может ставиться router/planner stage или возникать runtime, но thresholds, domain overrides и шкала `LOA` принадлежат `→ L1/D07 §1-§2, §5`. [C8]

### 6.5 Базовая карта gate-trigger'ов

| Trigger | Что делает D04 | Где owner policy |
|--------|-----------------|------------------|
| **Необратимое действие** | ставит interrupt перед dispatch | `→ L1/D07 §1-§2` |
| **Недостаточная уверенность плана** | запускает verify/re-plan или HITL escalation | `→ L1/D07 §1` |
| **Budget/policy boundary** | останавливает execution и запрашивает внешнее решение | `→ L1/D08 §2, §6` и `→ L1/D07 §2` |
| **Scope/auth problem** | блокирует tool call до повторной авторизации | `→ L1/D01 §6`, `→ L1/D07 §2` |

### 6.6 Таксономия ошибок и recovery

| Тип ошибки | Пример | Нормальный ответ D04 |
|-----------|--------|----------------------|
| **Transient** | timeout, rate limit, temporary network issue | retry + backoff |
| **Tool mismatch** | tool not found, bad schema, annotation conflict | reject call, re-resolve tool, validate manifest |
| **Planning defect** | missing dependency, impossible order, absent intermediate | verify -> re-plan |
| **Business failure** | ресурс недоступен, бронь сорвалась | compensation + re-plan |
| **Policy failure** | нужен `HITL Gate`, but no approval | pause/resume path |
| **State failure** | lost context, inconsistent checkpoint | revert to last valid checkpoint |

D04 intentionally предпочитает управляемое прерывание неуправляемому continuation. Именно поэтому `Saga Pattern`, `checkpoint`, `receipt`, `HITL Gate` и trace рассматриваются как единый recovery stack, а не как отдельные украшения архитектуры. [C6][C7][C8][C18][C19]

---

## 7. Observability и evaluation

### 7.1 `Execution Trace` как обязательный артефакт

`Execution Trace` нужен не только для постфактум-логов. В D04 это основной operational artifact, на который опираются:

- debugging;
- audit/compliance;
- user explanation;
- cost attribution;
- quality evaluation;
- recovery after interruption.

Поэтому trace должен собираться как first-class object на протяжении всего execution graph. [C13][C15]

### 7.2 Что должно попадать в trace

| Trace element | Зачем нужен |
|--------------|-------------|
| task/run identifiers | связать user request, plan и дочерние executions |
| routing decision | объяснить, почему выбран класс `A-D` и какой path стартовал |
| plan/DAG snapshot | видеть expected structure и последующие re-plans |
| tool calls + receipts | подтвердить, что действия реально были исполнены |
| handoff events | локализовать cascade failures |
| `HITL Gate` events | показать, где автономность была остановлена |
| latency / retries / compensations | понять reliability profile |
| cost estimate vs actual | калибровать router/planner heuristics |

### 7.3 Telemetry stack и evaluation

Для trace-модели D04 базовым стандартом выступает `OpenTelemetry GenAI SemConv`, а практический UI/ops layer может строиться через `Langfuse`. Важен не vendor, а нормализованная структура span'ов для reasoning/planning, tool calls, handoff/interrupt/resume и parent-child linkage. [C13]

D04 должен оцениваться system-level metrics, а не только качеством финального текста: success rate по классам `A-D`, latency и fail-storm recovery, unnecessary retries/escalations, handoff integrity и trace completeness. В качестве evaluation precedents подходят `MASEval`, `MultiAgentBench`, `ProtocolBench` и related routing evidence. [C13][C15][C19]

### 7.5 Resource estimation как часть observability loop

Resource estimation в D04 должно происходить дважды:

1. **plan-time estimate** — грубый прогноз tokens, tool calls, compute path и expected latency; [C21]
2. **post-run actuals** — фактические расходы, чтобы калибровать router/planner heuristics через trace. [C13][C21]

Практически router/planner должны работать хотя бы с тремя величинами:

- ожидаемые tokens;
- ожидаемое число tool invocations;
- ожидаемый compute/storage footprint.

Но D04 не должен сам закрывать вопрос окончательной формулы `ESc <-> resources`: competence semantics находятся в `→ L1/D02 §5.5`, а material substrate — в `→ L1/D03 §3-§7`. В D04 пока фиксируется только необходимость bridge layer в planner. [C21]

### 7.6 Минимальный operational baseline v4.0

Для черновика v4.0 можно считать обязательным следующий baseline:

- любой run класса `B-D` имеет traceable plan state; [C5][C13]
- любой критичный tool call имеет provenance or receipt; [C18]
- любой `HITL Gate` отражён в trace как interrupt/resume pair; [C8][C13]
- любой long-running run имеет checkpoint discipline; [C6]
- router может сравнить estimate vs actual после завершения run. [C21]

---

## Связь с другими измерениями

| Измерение | Связь |
|-----------|-------|
| **L1/D01 (Идентификационное)** | D04 использует runtime identity и `DID`-auth для доверенного tool execution, но не владеет ими. Полный identity/runtime profile находится в `→ L1/D01 §1` и `→ L1/D01 §6`. |
| **L1/D02 (Компетентностное)** | D04 классифицирует и исполняет задачи, но competence semantics `ESq`/`ESc` остаются внешним входом для router/planner. Подробности — `→ L1/D02 §5.5`. |
| **L1/D03 (Материальное)** | `Execution Engine` и `Context Manager` используют material substrate как execution base: compute, storage, tooling access, failover. Полный слой доступа к ресурсам и storage tiering находится в `→ L1/D03 §3-§7`. |
| **L1/D05 (Коммуникационное)** | D04 владеет `MCP` как tool protocol и `Structured Handoff` как intra-Logos execution pattern, но `A2A` и межагентный communication stack принадлежат `→ L1/D05 §4-§6`. |
| **L1/D07 (Автономность)** | `HITL Gate` технически реализуется в D04, однако `LOA`, Delegation VC thresholds и domain-specific autonomy policy принадлежат `→ L1/D07 §1-§2, §5`. |
| **L1/D08 (Экономическое)** | D04 должен учитывать budget limits и cost attribution, но `Budget Controller` и `Dual Constraint` принадлежат `→ L1/D08 §2` и `→ L1/D08 §6`. |

---

## Открытые вопросы

### Требуют решения (Critical / High)

Критические пробелы v3 для D04 закрыты корпусом: `Orchestrator`, `MCP`, `Saga Pattern`, `HITL Gate`, `Execution Trace` и evaluation baseline уже имеют достаточно evidence для архитектурной фиксации в v4.0. [C1][C2][C7][C8][C13][C15]

### Требуют исследования (Medium)

1. **[Medium] G19: как формализовать bridge между `ESc`, resource estimation и material routing в planner?** Корпус подтверждает необходимость plan-time estimation, model-aware cost heuristics и cost-aware routing, но не даёт канонической формулы, связывающей competence-side `ESc`, runtime tokens/tools и material substrate в один governance-ready estimator. [C4][C21]
   > **Open.** Для v4.0 безопаснее фиксировать двухшаговую схему: (a) router/planner строит runtime estimate в своих единицах; (b) отдельный bridge layer later maps estimate к competence/resource policies через `→ L1/D02 §5.5` и `→ L1/D03 §3-§7`. Прямое "ESc = tokens + compute" пока преждевременно.

2. **[Medium] G20: насколько далеко можно продвинуть dynamic tool discovery, не превратив D04 в зависимость от незрелых MCP extensions?** Registry-backed discovery и gateway mediation уже выглядят направлением по умолчанию, но `.well-known` discovery, `Server Cards` и `Tasks` primitive пока частично живут в roadmap / ecosystem proposals. [C20]
   > **Open.** Зафиксировать как target state: discovery через registry/gateway, локальный `Tool Manifest`, auth-first bind и advisory annotations. Всё, что зависит от `E31` roadmap parts, помечать как forward-looking и включать только за feature flag. *(часть `E31` — early-stage / roadmap)*.
