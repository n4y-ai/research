# УРОВЕНЬ 1 / ИЗМЕРЕНИЕ 07: АВТОНОМНОСТЬ

> **Уровень:** L1 — Агент (свойства Логоса)  
> **Измерение:** D07 — Autonomy  
> **Версия:** 4.0  
> **Статус:** Финал v4.0 | Дата: 02.04.2026  
> **Зависимости:** L1/D01, L1/D04, L1/D09, L1/D06, L2/D08, L2/D09

---

## Ключевой вопрос

**Насколько ты независим в принятии решений и какие механизмы это обеспечивают?**

---

## Аннотация

Этот черновик фиксирует автономность Логоса как управляемую policy-конфигурацию, а не как синоним модельной "силы". Ядро D08 составляют шкала `LOA`, структура `Delegation VC`, механизм `Earned autonomy`, `Non-delegable core`, `HITL`-правила, `Emergency brake` и метрики наблюдаемой автономности. [C1][C2][C5]

По сравнению с финальной версией `v3.1` здесь сохранено прежнее operational core, но обновлены три критические зоны: правило приоритета `loa_ceiling` над domain override (`G9`), правовая проекция `LOA -> liability` в согласии с `L1/D06 v4.0` (`OI-110`), и все ссылки на `HITL Gate` теперь указывают на `→ L1/D04 §6.3`, а не на legacy-секцию. Числовые thresholds для earned autonomy и части runtime-порогов сохраняются как архитектурный baseline и явно помечаются как illustrative governance choices там, где внешний корпус не даёт универсального стандарта. [C6][C10][C11]

---

## Содержание

1. [LOA-шкала](#1-loa-шкала)
2. [Delegation VC структура](#2-delegation-vc-структура)
3. [Earned autonomy и тиры](#3-earned-autonomy-и-тиры)
4. [Non-delegable core](#4-non-delegable-core)
5. [HITL activation table и graceful degradation](#5-hitl-activation-table-и-graceful-degradation)
6. [LOA -> liability mapping](#6-loa---liability-mapping)
7. [Emergency brake и метрики](#7-emergency-brake-и-метрики)

---

## 1. LOA-шкала

### 1.1 Базовый принцип

`LOA (Level of Autonomy)` в D08 означает не "насколько умна модель", а насколько широкую операционную свободу владелец разрешил конкретному Логосу в рамках доказуемого мандата. Поэтому высокий `LOA` не отменяет human accountability и не превращает Логоса в отдельный субъект решений; он лишь меняет частоту одобрений, плотность контроля и допустимый диапазон действий. [C1][C2][C3]

Эта формулировка сохраняет преемственность с `v3.1`, но делает её жёстче: автономность задаётся policy-слоем, а capability-слой, identity basis и runtime execution остаются в других измерениях. Identity chain и корневой ceiling делегирования описываются полностью в `→ L1/D01 §1`; исполнение `HITL Gate` как механизма остановки run описывается в `→ L1/D04 §6.3`. [C5][C9][C10]

### 1.2 Пять уровней автономии

| LOA | Роль | Что Логос делает без предварительного одобрения | Что остаётся за человеком | Illustrative threshold baseline |
|-----|------|--------------------------------------------------|---------------------------|---------------------------------|
| **LOA-1** | Исполнитель | Ничего consequential: только подготовка, сбор данных, черновики и симуляции | Человек подтверждает каждый значимый шаг | `HITL` на каждом действии; финансовый auto-run не допускается |
| **LOA-2** | Помощник | Выполняет рутинные обратимые действия внутри narrow scope | Человек утверждает необратимые действия, PII и spend above threshold | auto-run только для low-risk действий; `HITL` при `>50 AGC` |
| **LOA-3** | Советник | Исполняет согласованный план в пределах домена и бюджета | Человек утверждает plan exceptions, irreversible steps и risky branching | `HITL` при `>200 AGC`, scope anomalies и low-confidence escalation |
| **LOA-4** | Поверенный | Ведёт миссию end-to-end в пределах Delegation VC и domain policy | Человек вмешивается по исключениям, конфликтам policy и brake events | approval-by-exception; manual review при anomaly / scope breach |
| **LOA-5** | Доверенный | Работает в устойчивом режиме с periodic oversight и post-hoc accountability | Человек сохраняет право остановки, понижения `LOA` и отзыва делегирования | strategic review only; доступен только при earned + governance constraints |

Таблица выше является owner-описанием D08, потому что именно здесь задаётся каноническая шкала `LOA-1..5`. Числа `50 AGC` и `200 AGC` не претендуют на внешний универсальный стандарт; это Logos operational baseline, перенесённый из `v3.1` и совместимый с corpus-идеей bounded autonomy и meaningful human checkpoints. [C1][C2][C3]

### 1.3 Критерии перехода от уровня к уровню

Переход между уровнями меняет не только количество разрешённых действий, но и тип human posture:

| Переход критерия | LOA-1 | LOA-2 | LOA-3 | LOA-4 | LOA-5 |
|------------------|-------|-------|-------|-------|-------|
| **Планирование** | человек задаёт шаги | Логос предлагает варианты | Логос формирует план | Логос перепланирует по месту | Логос управляет mission loop |
| **Бюджет** | всегда ручной confirm | только low-value auto | bounded delegated spend | exception-driven control | periodic budget governance |
| **Необратимые действия** | всегда запрещены без confirm | только после confirm | selective confirm | по policy exception | только через brake-capable oversight |
| **Доменные границы** | single narrow task | узкий домен | bounded domain | multi-step within domain | multi-domain only if explicitly granted |
| **Ожидание от traceability** | полный pre-approval trail | approval + receipts | plan + receipts + interrupts | exception logs + checkpoints | telemetry-by-design + auditability |

`LOA-5` не означает "полностью без человека". Даже на верхнем уровне должны сохраняться способность мониторить, вмешиваться, override и halt operation. Это согласуется с corpus-формулой meaningful human oversight и прямо исключает чтение `LOA-5` как режима полного отрыва от владельца. [C2][C3][C4]

### 1.4 Выравнивание с Singapore MGF

Singapore Model AI Governance Framework for Agentic AI не задаёт буквальную шкалу `LOA-1..5`, но даёт четыре governance-опоры, с которыми D08 должен быть совместим:

| MGF dimension | Как она проявляется в D08 |
|---------------|---------------------------|
| **Bound autonomy and access** | `Delegation VC`, `loa_ceiling`, domain overrides, spend caps и forbidden actions ограничивают autonomy envelope |
| **Meaningful human accountability** | `HITL`-пороги, earned autonomy, owner override и explicit revocation не дают автономии стать необратимой |
| **Technical safeguards** | baseline testing, anomaly-triggered rollback, graceful degradation, `Emergency brake` и telemetry stack |
| **End-user intervention and transparency** | periodic reports, traceability, право остановки, contested actions и post-hoc audit trail |

Это не означает, что D08 копирует сингапурскую рамку one-to-one. Корректная формула строже: D08 использует `LOA` как внутреннюю шкалу, а MGF служит внешним sanity-check, подтверждающим, что высокая автономность должна оставаться ограниченной, наблюдаемой и останавливаемой. [C2][C3][C12]

---

## 2. Delegation VC структура

### 2.1 Что именно владеет D08

`Delegation VC` является owner-концептом D08: именно он кодирует, сколько автономии делегировано, в каком домене, с какими пределами и при каких условиях возможны escalation, rollback и revocation. При этом идентификационный базис `Human DID -> Logos DID` и Binding VC описываются полностью в `→ L1/D01 §1`, а юридическая функция доказуемого мандата - в `→ L1/D06 §6`. Здесь D08 владеет именно autonomy semantics credential'a. [C5][C9][C10]

### 2.2 Каноническая структура

```yaml
DelegationVC:
  identity:
    human_did: did:cheqd:holder
    logos_did: did:cheqd:logos
    binding_vc_ref: vc:binding:123
    delegation_id: del-2026-04-02-001
    issuer_role: PrimaryLogos | WorkLogos

  autonomy_config:
    global_loa: LOA-3
    loa_ceiling: LOA-4
    domain_overrides:
      travel: LOA-4
      finance: LOA-2
      devops: LOA-3
    emergency_loa: LOA-1
    earned_autonomy_enabled: true

  scope:
    allowed_domains:
      - travel
      - devops
      - analytics
    max_agc_per_transaction: 200
    max_agc_per_day: 1000
    irreversible_actions: confirm_required
    forbidden_actions:
      - modify_identity
      - sign_binding_contract
      - transfer_qi
      - self_raise_loa
      - issue_subdelegation

  hitl_thresholds:
    financial_threshold_agc: 200
    pii_access: confirm_required
    low_confidence_plan: escalate
    policy_conflict: escalate
    irreversible_action: always_confirm

  escalation:
    on_scope_violation: pause_and_notify
    on_anomaly: rollback_to_emergency_loa
    on_low_confidence: request_hitl
    on_conflicting_credentials: lower_wins_then_notify

  revocation:
    owner_revocable: true
    platform_emergency_pause: true
    smart_contract_freeze_hooks: true
    cascade_revocation: within_chain
```

Список полей выше намеренно полный для D08-синтеза `v4.0`: `identity`, `autonomy_config`, `scope`, `hitl_thresholds`, `escalation`, `revocation`. Именно через такую структуру корпус поддерживает идею scoped, auditable delegation вместо broad standing authority. [C5][C9]

### 2.3 Правило приоритета G9

Для `G9` в `v4.0` фиксируется явное вычислительное правило:

```text
LOA_effective = min(loa_ceiling, domain_override.LOA)
```

Смысл правила такой:

1. `loa_ceiling` задаёт абсолютный верхний предел, унаследованный от корневой identity/delegation chain.
2. `domain_overrides` могут только сузить autonomy envelope для конкретного контекста.
3. Попытка поднять доменный `LOA` выше ceiling не создаёт "специального исключения", а либо отвергается валидатором, либо редуцируется к нижней границе.

Это и есть monotonic attenuation invariant, который поддерживается corpus'ом по нескольким семействам делегирования. Поэтому D08 не допускает union semantics для authority: effective autonomy определяется через пересечение ограничений, а не через наибольший grant. [C9][C10]

### 2.4 Что не принадлежит этой секции

D08 не повторяет здесь полный DID/VC privacy stack, не определяет Binding VC presentation profile и не строит legal wrapper для enforceability across jurisdictions. Эти зоны уже принадлежат другим owner-слоям:

| Что не раскрывается fully в D08 | Где owner |
|---------------------------------|-----------|
| Identity basis, Binding VC, `Human DID -> Logos DID` | `→ L1/D01 §1` |
| Runtime pause/resume implementation для `HITL Gate` | `→ L1/D04 §6.3` |
| Legal effect chain и cross-MS legal wrapper | `→ L1/D06 §6` |

Такое разграничение важно, чтобы `Delegation VC` не расползался в три разных "владельца". D08 отвечает за autonomy envelope, а не за всё, что связано с VC вообще. [C5][C12]

---

## 3. Earned autonomy и тиры

### 3.1 Базовая логика

`Earned autonomy` в D08 означает, что повышение `LOA` происходит не автоматически от одной успешной сессии и не только по субъективной уверенности пользователя. Оно требует сочетания track record, reputation signal, низкой частоты overrides, ограниченного incident profile и явного owner decision. Внешний corpus поддерживает сам принцип adaptive trust, но не даёт универсально принятой числовой таблицы миссий; поэтому все mission-count thresholds ниже помечаются как illustrative governance baseline. [C6][C8]

Репутационные тиры при этом не принадлежат D08 полностью: D08 использует их как вход в promotion logic, а полное описание score/tier layer находится в `→ L1/D09 §5.2-§5.3`. D08 отвечает за то, как tier превращается в autonomy eligibility. [C6][C8]

### 3.2 Bridge между тирами и LOA

| Tier | Reputation threshold | Illustrative completed missions | Eligibility effect in D08 | Статус |
|------|----------------------|---------------------------------|---------------------------|--------|
| **Bronze** | `>=1` | `>=10` | открывает `LOA-1 -> LOA-2` | working baseline |
| **Silver** | `>=31` | `>=50` | открывает `LOA-2 -> LOA-3` | working baseline |
| **Gold** | `>=56` | `>=200` | открывает `LOA-3 -> LOA-4` | working baseline |
| **Platinum** | `>=76` | `>=500` | открывает `LOA-4 -> LOA-5` при дополнительных governance checks | working baseline |
| **Diamond** | `>=91` | `>=1000` | не создаёт `LOA-6`; подтверждает устойчивость `LOA-5` и mentor/trust-anchor posture | illustrative extension |

Порог `Diamond` добавлен не как новая ступень автономии, а как слой зрелости поверх уже достигнутого high-autonomy режима. Это сохраняет совместимость с D06, где `Diamond` существует как верхний trust tier, но не ломает пятиуровневую модель `LOA`. [C6][C8]

### 3.3 Promotion logic

Повышение `LOA` допустимо только при одновременном выполнении четырёх условий:

| Условие | Что требуется |
|---------|---------------|
| **Track record** | достаточно завершённых миссий в релевантном домене без критических инцидентов |
| **Reputation** | соответствующий tier из `→ L1/D09 §5.3` |
| **Oversight quality** | устойчиво низкий override / escalation rate |
| **Owner decision** | явное подтверждение владельца; для `LOA-5` также governance boundary |

Важное ограничение: Логос не может "заслужить" себе повышение автономии unilateral act'ом. Earned autonomy меняет eligibility state, но окончательное повышение всегда остаётся act of delegation со стороны владельца. [C5][C6]

### 3.4 Rollback и OI-043 note

Для симметрии promotion/rollback D08 сохраняет operational rollback baseline:

| Trigger | Действие |
|---------|----------|
| `autonomous_error_rate > 5%` на окне контроля | понижение на `-1 LOA` |
| критический инцидент или safety breach | немедленный переход к `LOA-1` |
| `human_override_rate > 20%` на коротком окне | mandatory review и freeze promotion |
| повторное повышение | только после периода стабилизации и owner confirmation |

`OI-043` сохраняется открытым: если D06 пересмотрит итоговую шкалу score, репутационные thresholds для `Bronze/Silver/Gold/Platinum/Diamond` должны быть обновлены синхронно. До этого все numeric bridges следует читать как current working baseline, а не как permanently frozen constitutional constants. [C6]

---

## 4. Non-delegable core

### 4.1 Почему эта граница существует

Higher autonomy не отменяет protected boundaries. Corpus поддерживает два взаимодополняющих тезиса: во-первых, автономность должна масштабироваться вместе с accountability configuration, reversibility и criticality; во-вторых, automated decisions with legal or similarly significant effects остаются юридически чувствительными даже там, где конкретные jurisdictions различаются. Поэтому D08 фиксирует `Non-delegable core` как набор действий, которые не становятся fully autonomous ни при каком `LOA`. [C3][C4][C12]

### 4.2 Шесть категорий non-delegable core

| Категория | Что запрещено делегировать полностью | Почему |
|-----------|--------------------------------------|--------|
| **1. Идентичность и контроль субъекта** | изменение `Human DID`, отзыв/перевыпуск Binding VC, смена root controller, перенос базовой субъектности | эти действия меняют сам источник authority, а не его применение |
| **2. Юридически обязывающие решения** | подписание договоров, согласие с ToS, подача исков, принятие обязательств, создающих прямой legal effect | legal effect требует доказуемой человеческой воли и/или внешнего legal wrapper |
| **3. Высокоимпактные финансовые акты** | перевод `Qi`, залог активов, unrestricted spending, конвертация в off-platform value без отдельного approval rail | высокий economic irreversibility и отдельный compliance tail |
| **4. Значимые data-rights и ADM actions** | consent от имени субъекта, fully automated decisions с legal/similarly significant effect без safeguards, несогласованное раскрытие PII | AI Act Art.14 и GDPR/UK safeguard layer требуют возможность вмешательства и contestation |
| **5. Саморасширение автономии** | повышение собственного `LOA`, расширение `scope`, снятие `forbidden_actions`, отключение `HITL`-порогов | агент не может самостоятельно усиливать свой мандат |
| **6. Репликация, субделегирование и governance-права** | выпуск новых полномочий другим агентам, self-replication с authority transfer, участие в governance vote как замена человеку | delegation chain должна оставаться доказуемой и привязанной к человеку/организации |

Список сохраняет ядро `v3.1`, но уточняет четвертую и шестую категории: `GDPR Art.22`-подобные safeguards и governance-specific non-delegable actions теперь явно вынесены на поверхность, а не растворены в общих запретах. [C4][C12]

### 4.3 AI Act and GDPR boundary conditions

D08 не владеет полным regulatory analysis этих норм; здесь остаётся только autonomy-side projection under `[A14]`. Полные owner-описания AI Act article duties находятся в `→ L2/D08 §2.3-§2.7`, а GDPR/DPIA contour — в `→ L2/D08 §4.3-§4.4`. [C12]

| Нормативный слой | Проекция для D08 |
|------------------|------------------|
| **AI Act Art.10** | High-LOA scenarios требуют dataset traceability, provenance discipline и audit-ready evidence, но полный data-governance regime описан в `→ L2/D08 §2.3`. |
| **AI Act Art.17** | Чем выше autonomy envelope, тем важнее release, monitoring and incident rules среды; полный `QMS` owner-ответ закреплён в `→ L2/D08 §2.4`. |
| **AI Act Art.27** | High-risk deployment не может идти в unrestricted autonomy, если deployer-side `FRIA` не проведён; подробности boundary → `L2/D08 §2.5`. |
| **AI Act Art.50** | Human-facing high-LOA workflows должны сохранять visible AI disclosure and marking obligations, но legal duty and artifacts описаны в `→ L2/D08 §2.6`. |
| **AI Act Art.57** | Sandbox participation может расширять supervised experimentation, но не снимает oversight и halt requirements внутри autonomy design; подробности → `L2/D08 §2.7`. |
| **GDPR Art.22 / DPIA** | Даже при высокой автономности safeguards, intervention and contestation остаются обязательными; полный privacy-owner слой описан в `→ L2/D08 §4.3-§4.4`. |

Следовательно, рост `LOA` усиливает требования к oversight, traceability и reversibility, но не переносит owner-роль regulatory analysis из `L2/D08` в `L1/D07`. [C12]

Для cross-jurisdictional autonomy obligations вне EU (например, UK Data (Use and Access) Act 2025) — см. `→ L1/D06 §3` (comparative jurisdictional baseline); `L1/D07` остаётся EU AI Act + GDPR scoped. [A14]
---
## 5. HITL activation table и graceful degradation

### 5.1 Ownership boundary с D04

`HITL Gate` технически реализуется в owner-слое исполнения `→ L1/D04 §6.3`. D08 владеет не самим механизмом pause/resume, а policy-вопросом: на каких уровнях автономии какие триггеры требуют одобрения, escalations или остановки. Эта граница обязательна для `v4.0`, и все cross-ref в этом черновике сознательно ведут к `→ L1/D04 §6.3`, а не к legacy `§7`. [C2][C3][C8]

### 5.2 HITL activation table

| LOA | Default human posture | Когда `HITL Gate` обязателен | Где исполняется gate |
|-----|-----------------------|------------------------------|----------------------|
| **LOA-1** | confirm-by-default | на каждом consequential action | `→ L1/D04 §6.3` |
| **LOA-2** | pre-approval for exceptions | необратимые действия, PII, `>50 AGC`, policy ambiguity | `→ L1/D04 §6.3` |
| **LOA-3** | plan-approved, action-selective | `>200 AGC`, low-confidence branch, domain exit, irreversible step | `→ L1/D04 §6.3` |
| **LOA-4** | approve-by-exception | scope violation, anomaly, conflicting credentials, brake-precondition | `→ L1/D04 §6.3` |
| **LOA-5** | periodic oversight | emergency, contested action, forced rollback, governance-triggered review | `→ L1/D04 §6.3` |

Эта таблица выражает важный corpus-тезис: meaningful human oversight не равен обязательному просмотру каждого токена или tool call. По мере роста `LOA` человек переходит от micro-approval к exception-driven oversight, но не исчезает из control loop. [C2][C3]

### 5.3 Базовые gate-trigger'ы

В D08 удобно держать короткую policy-карту триггеров, которые затем реализуются D04:

| Trigger family | Policy-решение D08 |
|----------------|--------------------|
| **Irreversibility** | требует confirm или rollback в зависимости от LOA |
| **Spend threshold** | сверяется с domain policy и `hitl_thresholds` |
| **PII / legal-significance** | не переводится в silent auto-run даже на high LOA |
| **Scope/auth conflict** | lower-wins, затем pause-and-notify |
| **Low-confidence planning** | re-plan или `HITL`, но не silent continuation |

Именно здесь применяется `OI-096` fix: любой текст о `HITL Gate` должен ссылаться на `→ L1/D04 §6.3`. D04 владеет runtime-mechanics, D08 - gate policy. [C2][C3][C8]

### 5.4 Четыре уровня graceful degradation

При потере связи, неразрешённом gate или ухудшении runtime confidence D08 не прыгает сразу из "автономен" в "мёртв". Предпочтителен staged degradation:

| Уровень деградации | Триггер | Что делает система |
|--------------------|---------|--------------------|
| **GD-1: pause** | gate без ответа, временная неопределённость | ставит mission на паузу и ждёт owner input |
| **GD-2: safe default** | timeout истёк, но действие обратимо | отменяет или откатывает локальный шаг, сохраняя checkpoint |
| **GD-3: rollback to emergency LOA** | anomaly, repeated policy conflict, rising error rate | снижает autonomy до `emergency_loa = LOA-1` |
| **GD-4: mission freeze** | prolonged outage, severe incident, brake activation | замораживает миссии до явного внешнего решения |

Такая деградация согласуется с corpus-идеей rollback-oriented controls: лучше управляемое ограничение автономии, чем немой collapse или продолжение за пределами governance envelope. [C2][C8]

---

## 6. LOA -> liability mapping

### 6.1 Граница ownership

Полная модель ответственности не принадлежит D08. Её owner-описание находится в `→ L1/D06 §2`, а regulatory substrate - в `→ L2/D08 §1-§5`. В D08 допустима только правовая проекция автономности: как изменение `LOA` влияет на качество доказуемого делегирования, человеческого контроля и governance posture. [C11][C12]

### 6.2 Нормативная формула для OI-110

Для `v4.0` D08 должен использовать именно такую формулу:

> `LOA` влияет на качество доказуемого делегирования и governance, не создавая отдельный режим ответственности `→ L1/D06 §2.3`.

Это исправляет stale framing из `v3.1`, где higher autonomy читалась слишком близко к самостоятельной "лестнице ответственности". После обновления `L1/D06 v4.0` корректнее говорить о перераспределении operational posture, а не о возникновении нового liability regime. [C11][C12]

### 6.3 Mapping table

| LOA tier | Что меняется в D08 | Liability clause для чтения D08 | Owner ref |
|----------|--------------------|---------------------------------|-----------|
| **LOA-1** | почти всё consequential подтверждается человеком | assisted-use posture: владелец ближе всего к конкретному действию, но product-side defect layer не исчезает | `→ L1/D06 §2.3` |
| **LOA-2** | часть low-risk действий делегирована, но approval remains dense | owner/deployer control остаётся плотным; значение scope и limits растёт | `→ L1/D06 §2.3` |
| **LOA-3** | делегирование становится materially meaningful | tri-party reading усиливается: важны formal scope, logs, deployer controls и доказуемость oversight | `→ L1/D06 §2.3` |
| **LOA-4** | autonomy широка, но bound by policy exceptions | повышается значимость provider safeguards и quality of governance, а не только owner intent | `→ L1/D06 §2.3` |
| **LOA-5** | strategic oversight вместо micro-approval | high-autonomy deployment требует strongest evidence of bounded delegation, telemetry and brakeability; D08 не утверждает отдельную "exclusive provider liability" doctrine | `→ L1/D06 §2.3` |

Последняя колонка обязательна не ради формальности, а чтобы D08 не начал конкурировать с D07 за owner-роль. D08 отвечает на вопрос "насколько разрешено действовать", а D07 - "как юридически читать последствия". [C11][C12]

---

## 7. Emergency brake и метрики

### 7.1 Три источника emergency brake

`Emergency brake` в D08 - это немедленное снижение автономии до `LOA-1` с остановкой или резким ограничением миссий. Корпус поддерживает три допустимых источника такого сигнала:

| Источник | Что считается валидным trigger | Действие |
|----------|-------------------------------|----------|
| **Владелец** | явная команда stop, revoke, pause, dispute | `LOA -> LOA-1`, mission pause, уведомление и checkpoint |
| **Платформа** | security incident, anomaly, policy breach, unsafe drift | forced rollback или full freeze в зависимости от серьёзности |
| **Smart contract** | budget breach, escrow timeout, compliance hook, dead-man-style constraint | freeze affected transaction domain, затем notify and review |

Это не три независимых "владельца автономии". Это три разрешённых канала аварийного сужения already delegated authority. Во всех случаях brake должен оставлять audit trail и не должен silently восстанавливать прежний `LOA` без явного review. [C2][C3][C8]

### 7.2 G11 design note: конфликт личной и корпоративной делегации

`G11` остаётся architecture note, а не settled industry standard. Corpus поддерживает жёстко только single-chain attenuation; для dual-root case (personal + corporate credentials) внешний стандарт пока не сложился. Поэтому в D08 `v4.0` фиксируется design recommendation, а не внешнее "требование":

| Случай | Рекомендованное правило |
|--------|-------------------------|
| **Явно личный домен** | применяется personal delegation policy |
| **Явно корпоративный домен** | применяется corporate delegation policy |
| **Амбивалентный домен** | `domain-scoped lower-wins` |
| **Emergency brake от любой стороны** | brake действует немедленно в релевантном домене |
| **Возврат после brake** | только через owner / governance review, без auto-restore |

Формула "lower-wins + owner-veto in ambiguous domains" в этом документе помечается как Logos design choice. Она следует духу least-privilege и monotonic narrowing, но не должна подаваться как завершённый внешний стандарт VC/SSI мира. [C13][C14]

### 7.3 Семь KPI автономности

Для наблюдаемой автономности D08 использует не одну "магическую метрику", а связку operational KPI:

| KPI | Что показывает | Warning | Critical |
|-----|----------------|---------|----------|
| **task_success_rate** | доля успешно завершённых миссий | `<90%` | `<80%` |
| **human_override_rate** | как часто человек отменяет или меняет курс | `>10%` | `>20%` |
| **escalation_rate** | частота `HITL` / anomaly escalations | `>15%` | `>30%` |
| **autonomous_error_rate** | доля автономных ошибок | `>3%` | `>7%` |
| **loa_appropriateness_score** | насколько текущий `LOA` соответствует risk profile и outcomes | `<95%` | `<90%` |
| **emergency_brakes_per_30d** | частота аварийных остановок | `>1` | `>3` |
| **mean_time_to_human_response** | скорость реакции человека на gate / brake events | `>4h` | `>24h` |

Эти thresholds не описываются как окончательный внешний benchmark. Они нужны как operational dashboard baseline и могут быть пересмотрены governance-процессом по мере накопления evidence. [C8]

### 7.4 AURA и многометодная оценка автономности

Corpus поддерживает не одну метрику, а многометодный стек оценки автономности: ex ante scoring, supervised vs asynchronous risk scoring, code-inspection подходы, telemetry/boundary monitoring и measurement of `HITL` fraction. Поэтому KPI-таблица выше должна читаться как surface dashboard, а не как исчерпывающий scientific measure of autonomy. В практическом контуре D08 это означает pre-deployment scoring перед выдачей `LOA`, runtime telemetry по brake/override events и периодический `AURA`-style risk loop, который проверяет, не завышен ли текущий уровень автономии относительно фактического риска. [C8]

---

## Связь с другими измерениями

| Измерение | Связь |
|-----------|-------|
| **L1/D01 (Идентификационное)** | `Binding VC` и DID-связка задают identity basis для delegation chain и поля `loa_ceiling`, но полное описание этой основы находится в `→ L1/D01 §1`. |
| **L1/D04 (Функциональное)** | `HITL Gate` реализуется как execution mechanism в `→ L1/D04 §6.3`; D08 владеет policy-порогами и правилами активации. |
| **L1/D09 (Репутационное)** | reputation tiers `Bronze/Silver/Gold/Platinum/Diamond` поставляют reputation side of earned autonomy, но не заменяют full promotion logic D08: `→ L1/D09 §5.3`. |
| **L1/D06 (Правовое)** | `LOA` влияет на качество доказуемого делегирования и governance, не создавая отдельный режим ответственности: `→ L1/D06 §2.3`. |
| **L2/D08 (Нормативно-правовое)** | AI Act, GDPR и PLD задают regulatory boundary для human oversight, automated decisions и product-side risk, но подробно описываются в `→ L2/D08 §1`, `→ L2/D08 §4`, `→ L2/D08 §5`. |
| **L2/D09 (Governance)** | переход к устойчивому `LOA-5` требует governance boundary и emergency backstop; platform-level emergency powers описаны в `→ L2/D09 §4`. |

---

## Открытые вопросы

### Требуют решения (Critical / High)

Новых unresolved `Critical / High` для шага 8 не добавляется. Правки `OI-096` и `OI-110` встроены в этот черновик как resolved-in-draft.

### Требуют исследования (Medium)

1. **[Medium] OI-043: финализация шкалы reputation score.** Пока `L1/D09` удерживает вопрос открытым, tier thresholds для earned autonomy должны читаться как working baseline, а не как навсегда зафиксированная конституция автономии.  
   > **Open.** После стабилизации D06 требуется синхронный пересмотр таблицы `Bronze -> Diamond`.

2. **[Medium] G11: окончательная политика dual-root delegation.** В `v4.0` принят design note `domain-scoped lower-wins`, но внешний стандарт multi-root conflict resolution пока отсутствует.  
   > **Open.** Нужен дальнейший workstream по ambiguous domains, cross-chain revocation и interaction между personal/corporate override rights.

3. **[Medium] Governance of earned thresholds.** Корпус поддерживает сам принцип earned autonomy, но не даёт универсальных mission-count constants.  
   > **Open.** Нужно решить, какие thresholds остаются на уровне owner defaults, а какие переходят в governance-controlled parameters для `LOA-4 -> LOA-5`.

