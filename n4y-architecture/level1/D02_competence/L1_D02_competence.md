# УРОВЕНЬ 1 / ИЗМЕРЕНИЕ 02: КОМПЕТЕНТНОСТНОЕ

> **Уровень:** L1 — Агент (свойства Логоса)  
> **Измерение:** D02 — Competence  
> **Версия:** 4.1  
> **Статус:** Финал v4.1 | Дата: 19.04.2026  
> **Зависимости:** L1/D01, L1/D09, L2/D02

---

## Ключевой вопрос

**Что ты знаешь и какие навыки у тебя есть, и как ты это подтверждаешь?**

---

## Аннотация

**Прямой ответ на ключевой вопрос:** Логос знает то, что подтверждено двумя независимыми треками. Его Human DID несёт `Educational VC` — Open Badges 3.0 / EDCL-совместимые аттестации от верифицированных issuer'ов с ESCO-aligned competency claims, нормированными через EQF. Его `Logos DID` несёт `Agent Capability VC` — аттестацию от benchmark-оракула (SWE-bench Pro, GAIA, SkillsBench), подтверждающую реальную производительность агента на стандартных задачах. Совместно они определяют `ESq_effective = ESq_base × capability_multiplier` — разрешённый объём специализации Логоса в платформе. [C1][C9][C24][C25]

Документ фиксирует компетентностную модель Логоса как двухтрековую систему: `Authorization Track` связывает человеческие квалификации с `ESq`, а `Capability Track` связывает фактические способности агента с `Agent Capability VC`. Это сохраняет валидное ядро `v0_legacy`, но переводит его в более строгую форму: без смешения квалификации владельца, benchmark-результатов агента и стоимости задачи в один показатель. [C1][C2][C9][C24]

По сравнению с `v0_legacy` черновик углублён в шесть направлениях: production-ready стек `VC` для human credentials, benchmark-оракулы 2025-2026, governance слоя выдачи capability attestations, количественные building blocks для `ESq_base`, directional lifecycle/decay модель и privacy-boundary для competency claims. В документ не добавляются неподтверждённые числовые коэффициенты; там, где корпус даёт только design input, это прямо отмечено. [C8][C10][C12][C15][C16][C20][C23]

---

## Содержание

1. [Архитектура компетентностной системы](#1-архитектура-компетентностной-системы)
2. [Таксономия компетенций](#2-таксономия-компетенций)
3. [VC-стек для компетенций](#3-vc-стек-для-компетенций)
4. [Benchmark-оракулы и верификация](#4-benchmark-оракулы-и-верификация)
5. [ESq-маппинг и Dual Constraint](#5-esq-маппинг-и-dual-constraint)
6. [Жизненный цикл и decay](#6-жизненный-цикл-и-decay)
7. [Приватность competency claims](#7-приватность-competency-claims)

---

## 1. Архитектура компетентностной системы

### 1.1 Базовый принцип: компетентность не сводится к одному credential

Сохраняемый из `v0_legacy` тезис остаётся валидным: компетентность Логоса нельзя свести только к диплому владельца и нельзя свести только к benchmark-скору агента. Корпус поддерживает более строгую двухтрековую модель, в которой человеческая квалификация и фактическая агентная capability аттестуются раздельно и затем связываются в общей компетентностной архитектуре. [C1][C9]

> «Каждому пользователю присваивается "профиль компетенций" на основе: формальных данных, самооценки, результатов тестов, репутации и портфолио.» `[стратегия: вектор]`

Стратегический вектор о "профиле компетенций" сохраняется, но в `v1` уточняется: формальные данные и тесты не должны сливаться в один тип доказательства. Для Logos это означает раздельную обработку `Human Credentials / Educational VC` и `Agent Capability VC`. [C1][C2]

### 1.2 Двухтрековая модель

```text
Authorization Track                         Capability Track
────────────────────                        ─────────────────────────
Человек / владелец                          Логос / агент
  -> Human Credentials                        -> benchmark runs
  -> Educational VC                           -> capability evidence
  -> qualification equivalency                -> capability attestation
  -> ESq_base                                 -> Agent Capability VC

                    \                      /
                     \                    /
                      -> ESq_effective <- 
```

`Authorization Track` отвечает на вопрос: какие компетенции и квалификации подтверждены у человека, который делегирует право действовать от своего имени. `Capability Track` отвечает на другой вопрос: что конкретный Логос реально умеет делать в наблюдаемом benchmark-контуре на текущем снапшоте. Смешение этих ролей ведёт к ложной интерпретации `ES` как одной универсальной меры. [C1][C9][C24]

### 1.3 Роли ESq, ESc и ESr

В соответствии с каноническим глоссарием `ESq` остаётся permission-oriented квотой владельца, а `ESc` - cost-oriented характеристикой задачи или специализации. Термин `ESr` используется как предложенное обозначение фактических runtime-capabilities агента и нужен здесь не как отдельная валюта, а как semantic bridge между benchmark evidence и `capability_multiplier`. [C1][C24]

Это разграничение важно для всего документа: `ESq` отвечает за право выделить компетентностный объём, `ESc` - за требуемый объём под задачу или специализацию, а `ESr` - за язык описания текущего runtime-профиля агента без подмены им ни права владельца, ни стоимости задачи.

Корпус при этом не даёт внешней канонической формулы runtime-gating для `ESq`/`ESc`; значит, D02 должна зафиксировать только семантику входов и границы применения, а не притворяться, будто индустрия уже стандартизировала такой enforcement. [C24]

### 1.4 Authorization Track: что именно подтверждается у человека

В этом треке Logos использует `VC`-совместимые доказательства образования, сертификации и других human-qualification claims. Основа стека - `W3C VC v2.0` и `Open Badges 3.0`, которые обеспечивают issuer-signed attestations, holder presentations и machine-readable competency packaging. [C2]

`Authorization Track` нужен не для описания "всего, что умеет агент", а для доказуемого происхождения компетентностного потолка. Он даёт базу для `ESq_base`, но сам по себе не заменяет верификацию фактического выполнения задач. Эта граница уже была интуитивно заложена в `v0_legacy`; теперь она формулируется явно. [C1][C2][C16]

### 1.5 Capability Track: что именно подтверждается у агента

Второй трек опирается на precedent, что агентно-ориентированные credentials могут и должны существовать отдельно от credentials владельца. Самый сильный included precedent здесь - `SingularityNET AI Agent Safety Credentials`, показывающий, что агент-facing attestations могут описывать свойства самого агента, а не человека-владельца. [C9]

Следовательно, `Agent Capability VC` в L1/D02 следует понимать как текущий capability snapshot: он агрегирует benchmark evidence и привязывает capability-утверждения к конкретному агенту, benchmark version и дате измерения. Это не заменяет историю performance, которая принадлежит репутационному контуру. Подробности репутационного lifecycle и `Performance VC`: → L1/D09 §3 и → L1/D09 §6. [C9][C13]

### 1.6 Граница с репутацией и экономикой

`v0_legacy` местами включал reputation-driven обновление прямо в описание компетентностного ядра. В `v1` эта граница корректируется: репутация не является первичным доказательством компетенции, а выступает downstream-потребителем capability attestations и исторических performance signals. `L1/D09` использует capability данные как вход в `Certified Capability` компонент, но не владеет самой моделью компетентностного подтверждения. [C9]

С другой стороны, `ESq` участвует в `Dual constraint`, но экономическое условие платежа принадлежит не D02. Экономическая проекция `AGC` и инструментов среды находится в → L2/D02 §5. Здесь D02 отвечает только за компетентностную сторону ограничения. [C24]

---

## 2. Таксономия компетенций

### 2.1 ESCO как базовая компетентностная онтология

Главный вывод корпуса не изменился по сравнению с `v0_legacy`, но стал сильнее по доказательной базе: `ESCO v1.2` - наиболее убедительный baseline vocabulary для Logos competence claims. Причина не в том, что это "одна из возможных taxonomies", а в том, что ESCO одновременно актуальна, многоязычна, поддерживается Европейской комиссией и доступна в формах `OWL`, `SKOS`, `JSON-LD` и через API. [C4]

Для D02 это сразу решает три задачи: появляется единая vocabulary layer для human competence claims, возникает машиночитаемый путь для включения skill URIs в credentials и сохраняется возможность межграничного сравнения квалификаций без создания собственной закрытой таксономии Logos.

Дополнительный аргумент в пользу ESCO состоит в том, что policy и ontology evidence начинают сходиться: AI literacy и формализация competence claims становятся институционально значимыми, а ESCO уже даёт готовые URI-addressable skill structures. [C4][C25]

### 2.2 Почему SFIA и O*NET остаются только дополнительными источниками

`SFIA` и `O*NET` полезны как сравнительные и mapping-oriented frameworks, но corpus не поддерживает их замену ESCO в роли primary vocabulary. `SFIA` особенно полезна для техно-специализаций и ordinal proficiency language, а `O*NET` - как богатый task/activity corpus для mapping pipelines. Однако ни один из этих фреймворков не снимает преимущества ESCO как cross-border multilingual baseline. [C5]

Практический вывод для D02 такой:

- `ESCO` - primary ontology;
- `SFIA` - supplementary layering для IT-ролей и skill levels;
- `O*NET` - supplementary mapping input для course/task-to-skill alignment.

Это решение сохраняет разумное зерно `v0_legacy`, где ESCO уже предлагалась как база, но теперь снимает двусмысленность "любой из фреймворков подойдёт одинаково". [C4][C5]

### 2.3 ESCO-in-VC и JSON-LD представление

Одно из самых ценных новых углублений по сравнению с `v0_legacy` - подтверждение, что ESCO можно не просто "использовать как справочник", а реально включать в credential claims через `URI`-ссылки и `JSON-LD`-совместимую структуру. Связка `Open Badges 3.0 alignment + ESCO URIs` даёт практический путь для machine-readable competency claims. [C6]

Это означает, что `Educational VC` в Logos не должен хранить skill labels только как свободный текст. Более устойчивый профиль:

- хранить human-readable description;
- хранить `ESCO` URI в alignment-like поле;
- предъявлять этот claim в VC-compatible JSON-LD форме;
- оставлять vocabulary layer живой и обновляемой вместе с ESCO ecosystem. [C6][C26]

### 2.4 Международная equivalency: EQF, NQF, EDCL

Для human qualification track corpus подтверждает, что европейская инфраструктура уже поддерживает cross-border comparison через `EDCL / Europass` и инструменты `NQF`-сопоставления. Это не означает, что там уже существует agent-specific competence profile; напротив, включённые источники прямо указывают, что agent-focused profile в этот стек пока не встроен. [C7]

Поэтому в D02 должно быть зафиксировано два разных уровня:

- для человека доступна operational equivalency infrastructure через `EQF`, `NQF`, `EDCL`, `Europass`;
- для агента аналогичный cross-border capability profile ещё должен быть спроектирован внутри Logos capability track. [C7]

Такой ответ лучше legacy-формулировок, где equivalency описывалась как почти завершённая универсальная схема. В `v1` она остаётся сильной частью human credentials layer, но не переоценивается как уже готовая мировая система для AI-agent capabilities. [C7]

---

## 3. VC-стек для компетенций

### 3.1 Human credentials: W3C VC + Open Badges 3.0

Основной стек для human competence claims в D02 теперь подтверждён сильнее, чем в предыдущей версии: `W3C VC v2.0` задаёт базовую trust and presentation model, а `Open Badges 3.0` даёт domain-ready packaging achievements and competencies, совместимое с W3C VC. [C2]

Это особенно важно потому, что в `v0_legacy` OB3 ещё выглядел как перспективный стандарт. Теперь corpus показывает material production adoption: есть institutional adoption evidence от `1EdTech`, есть production support у `Accredible`, и есть dual-format issuance у `Velocert`. Значит, для Educational VC Logos речь идёт уже не о speculative stack, а о deployable market infrastructure. [C8]

### 3.2 EDCL как cross-border human credential rail

Для европейского контура `EDCL` дополняет OB3/W3C VC стек: он задаёт operational rail для образовательных credentials и cross-border comparison, связанный с `EQF` и `ESCO`. Но корпус не подтверждает, что этот rail уже охватывает agent-specific competence profile. Поэтому D02 должна использовать `EDCL` как усилитель human authorization track, а не как готовый контейнер для agent capability attestation. [C7]

Практически это даёт такую иерархию:

- `W3C VC + Open Badges 3.0` - portability и packaging;
- `EDCL / Europass` - cross-border educational interoperability;
- `ESCO` - semantic vocabulary layer внутри claims. [C2][C6][C7]

### 3.3 Agent Capability VC как отдельный тип компетентностной аттестации

`Agent Capability VC` следует сохранять как самостоятельный артефакт D02, а не как подтип educational credential. В отличие от human-track credentials, holder и subject здесь относятся к самому Логосу, а предмет аттестации - capability state, полученный из benchmark evidence или capability audit. [C1][C9]

Это важная корректировка legacy-модели: если в предыдущей версии agent certificate иногда описывался почти как "дополнение к диплому", то в `v1` он закрепляется как отдельный трек со своей жизнью, своими сроками актуальности и своей governance-логикой. [C1][C9][C20]

### 3.4 Почему VC syntax не решает trust сама по себе

Важное уточнение из корпуса: `VC`-синтаксис сам по себе не решает вопроса, кому доверять как issuer'у capability claims. Trust model остаётся application-specific, а значит Logos должна дополнительно определить issuer discovery, oracle governance, revocation и status lookup. [C3]

Это сразу отсекает слабую интерпретацию "если credential подписан, значит он уже достаточен". Для D02 валидный credential должен быть не только syntactically well-formed, но и встроен в доверенный issuer/oracle contour. [C3][C20]

### 3.5 Living interfaces, а не frozen stack

Ещё одно важное отличие от `v0_legacy`: relevant VC ecosystem продолжает двигаться в 2026 году. Поэтому OB3 / VC interoperability, render methods, issuer discovery и JSON-LD profiles нужно рассматривать как living interfaces. D02 не должна замораживать неподвижный credential profile "навсегда"; ей нужен обновляемый compatibility layer. [C26]

---

## 4. Benchmark-оракулы и верификация

### 4.1 SWE-bench Pro как coding-oriented oracle input

Самое сильное новое дополнение к legacy-версии - появление убедительного benchmark input для coding agents. `SWE-bench Pro` релевантен как oracle input потому, что он тестирует long-horizon multi-file coding tasks из активных репозиториев и вводит contamination controls, которых не хватало более ранним, упрощённым evaluation setups. [C10]

Но именно здесь важно не повторить типичную ошибку "high-quality benchmark = готовый credential policy". Корпус прямо говорит, что `SWE-bench Pro` не определяет score-to-credential translation, threshold policy, revocation semantics или expiry logic. Значит, benchmark justify capability attestation, но не заменяет политику выдачи. [C11]

### 4.2 Мультиbenchmark-профиль вместо одного глобального теста

Новые G9-источники снимают иллюзию, что один benchmark может канонически закрыть всё competence space агента. `GAIA`, `AgentBench`, `WebArena`, `OSWorld` и `SkillsBench` покрывают разные типы способностей: general assistant behavior, heterogeneous practical environments, realistic web tasks, OS-level execution и deterministic skill evaluation. [C12][C14]

Это означает, что `Agent Capability VC` не должен быть "единым числом из одного leaderboard". Более устойчивый профиль - capability bundle, где credential claims привязаны к benchmark family, версии, дате снапшота и домену применимости. [C12][C13]

### 4.3 Snapshot discipline: benchmark version и дата обязательны

Результаты benchmark-рынка двигаются слишком быстро, чтобы использовать их как вневременные абсолюты. Корпус особенно чётко показывает это на `GAIA`: leaderboard может быстро сдвигаться до human-level результатов, поэтому credentialing design должен привязывать claims к versioned snapshot, а не к "вечному рейтингу". [C13]

Отсюда следуют обязательные поля capability attestation:

- benchmark family;
- benchmark version или release line;
- дата измерения;
- domain applicability;
- issuer/oracle identifier;
- status/revocation endpoint. [C13][C20]

### 4.4 Governance benchmark-oracle issuance

Лучший доступный governance bundle для capability issuance складывается из четырёх слоёв: `W3C VC` issuer semantics как форматной основы, `ToIP TRQP v2.0` как trust-registry query layer `[Public Review]`, `ToIP Issuer Governance Requirements Guide` как issuer requirements baseline и lifecycle patterns вроде `zkMe ACC` для issuance, verification и revocation. [C20]

Это всё ещё не даёт "один мировой стандарт benchmark-oracle". Но это уже достаточно сильный baseline, чтобы различать канонический governance input и ad hoc ecosystem demo. [C3][C20]

### 4.5 Что делать с draft-сигналами и ecosystem-specific implementations

Вокруг agent registries и trust patterns уже есть активный innovation layer, но его статус нужно маркировать прямо в тексте. `ERC-8126` и `ERC-8004` важны как design signals, однако оба остаются draft-level foundations и не должны описываться как settled standards. [C21]

То же относится к ecosystem-specific implementations вроде `Corgent Virtual Trust Oracle` и `AgentTrust AQVC`: они показывают реальные service patterns для scoring и validation, но не становятся interoperable norms только потому, что технически существуют. [C22]

Следовательно, D02 использует их так:

- как proof that capability attestation services are feasible;
- как источник практических паттернов;
- но не как canonical issuance architecture для Logos. [C21][C22]

### 4.6 Результат для D02

D02 владеет не техническим устройством "оракулов вообще", а компетентностной логикой их применения. Поэтому здесь фиксируется следующее:

1. benchmark evidence допускается как input для `Agent Capability VC`; каждый oracle run производит `Performance VC` (subtype of Benchmark VC), который поступает в агрегационный процесс перед финальной выдачей `Agent Capability VC` — двухшаговый pipeline специфицируется в → L1/D09 §3; [C10][C12]
2. credential обязан быть version-bound и date-bound; [C11][C13]
3. issuer/oracle trust требует отдельного governance слоя; [C3][C20]
4. draft и ecosystem-specific patterns помечаются как non-canonical. [C21][C22]

Этого достаточно, чтобы ответить на вопрос D02 о подтверждении компетенций, не дублируя чужое владение техническим oracle stack. [C3][C24]

---

## 5. ESq-маппинг и Dual Constraint

### 5.1 Что сохраняется из legacy-версии

Из `v0_legacy` сохраняется ключевая структура: `ESq` не равно чистому образованию и не равно чистому benchmark result. В обновлённом виде эта мысль выражается через каноническую формулу: `ESq_effective = ESq_base(human qualification evidence) × capability_multiplier(Agent Capability VC)`. Это мультипликативная модель: `ESq_base` задаётся Authorization Track, а `capability_multiplier` (≥ 1.0) масштабирует его в зависимости от реально подтверждённых capabilities агента. [C1][C16][C24]

> «Часть ES назначается статически на основе формального образования. Другая часть — динамически.» `[стратегия: вектор]`

В `v1` эта стратегическая интуиция уточняется: статический слой относится к `ESq_base`, а динамический слой не должен произвольно смешивать репутацию, цену задачи и capability evidence. Наиболее осторожный синтез - использовать benchmark-driven `capability_multiplier` как корректирующий, а не автономный источник базовой квоты. [C16][C24]

### 5.2 Quantitative ESq_base: что реально подтверждено корпусом

Корпус не содержит канонической внешней формулы для `ESq_base`. Это должно быть сказано прямо, потому что именно здесь legacy-текст был наиболее уязвим к переизобретению неподтверждённых чисел. Вместо готовой формулы corpus даёт только building blocks:

- large-scale numerical course-to-skill alignment (`Course-Skill Atlas`); [C17]
- measurable proficiency layer через `EASEC`; [C18]
- ordinal support от `SFIA`; [C16]
- экспериментальную confidence formula от `VARIA`. [C19]

> **Архитектурная заметка (open, → см. G16).** Помимо традиционного пути `Educational VC → ESq_base`, в архитектуре N4Y.AI возникает потенциальный второй путь: Logos owner производит верифицированные знания (Knowledge Transfer Package), институциональный верификатор (ВУЗ, лицензированная организация) аттестует их качество и выдаёт `Knowledge Quality VC` — аттестацию в домене, аналогичную Educational VC по своей природе. Вопрос о том, является ли `Knowledge Quality VC` полноправным источником `ESq_base` наравне с Educational VC или действует как отдельный additive-компонент, остаётся design-решением для D02 (→ G16). Контекст: `→ L2/D05 §6.4`.

Следовательно, D02 может законно зафиксировать архитектурный pipeline, но не должна выдумывать production coefficients без отдельного governance решения. [C16][C17][C18][C19]

### 5.3 VARIA как directionally useful formula

Среди включённых источников именно `VARIA` даёт наиболее прямой quantitative signal. Его полезно использовать не как стандарт по умолчанию, а как design template:

```text
competency_confidence =
  (level_score × weight) +
  (skill_specificity × (1 - weight))
```

Эта формула важна не потому, что Logos обязана принять её буквально, а потому, что она показывает: human qualification claims уже можно переводить в числовую confidence layer через комбинацию уровня и specificity. При этом сам `VARIA` остаётся экспериментальным проектом, а не индустриальным стандартом. [C19]

### 5.4 Предлагаемый синтез без неподтверждённых коэффициентов

На основании корпуса и glossary-safe синтеза в D02 разумно фиксировать только следующую логику:

```text
ESq_effective =
  ESq_base(human qualification evidence)
  × capability_multiplier(current Agent Capability VC)
```

Где:

- `ESq_base` выводится из human-track evidence через taxonomy-aware scoring pipeline;
- `capability_multiplier` зависит от capability attestations и их актуальности;
- конкретные числовые lookup tables остаются открытым design/gov вопросом. [C16][C17][C18][C19]

Такой синтез сохраняет архитектурную идею legacy-версии, но убирает её слабое место: больше нет произвольных диапазонов multiplier или неоснованных порогов `ESq_base`. [C16][C24]

### 5.5 Dual Constraint: граница D02, D03 и L2/D02

`Dual Constraint` в контексте D02 следует читать строго: здесь описывается только competence-side условие `ESq ≥ ESc`. Это отвечает на вопрос "имеет ли субъект подтверждённый компетентностный объём для данной специализации". [C24]

Но фактический runtime enforcement этой проверки не принадлежит D02 целиком:

- семантика `ESq`, `ESc` и input-logic - здесь, в D02;
- механизм доступа к ресурсам и execution gating - → L1/D03 §3;
- платёжная/инструментальная сторона ограничения - → L2/D02 §5.

Именно так закрывается `G11`: внешних стандартов для runtime `ESq/ESc` gating нет, поэтому D02 фиксирует meaning boundary, а не симулирует внешний canon. [C24]

### 5.6 Где здесь находится репутация

Reputation track не должен подменять `ESq_base`. Если `L1/D09` использует capability attestations как часть `Certified Capability`, это downstream aggregation логики доверия, а не исходный semantic owner компетентностного маппинга. Поэтому reputation может усиливать discoverability и доверие, но не отменяет необходимость human qualification proof и benchmark-backed capability proof. → L1/D09 §1 и → L1/D09 §2. [C9]

> **OI-088 design note:** D06 reputation tier bonuses (Bronze +5%, Diamond +25%) apply as a separate `reputation_multiplier` layered on top of the D02 base formula. The canonical D02 formula `ESq_effective = ESq_base × capability_multiplier` remains unchanged; `reputation_multiplier` is an additional downstream factor applied within D06 scope after `ESq_effective` is computed. Full specification of reputation-modulated ESq → L1/D09 §5. ESq ownership remains with L1/D02.

---

## 6. Жизненный цикл и decay

### 6.1 Жизненный цикл competence artifacts

В legacy-версии верно угадывалась необходимость периодического обновления компетентностных доказательств, но многие интервалы были заданы слишком уверенно. Корпус позволяет зафиксировать жизненный цикл, не притворяясь, будто validated timing уже найден:

```text
creation of human credential
  -> ontology alignment
  -> ESq_base calculation
  -> benchmark run
  -> Agent Capability VC issuance
  -> monitoring
  -> re-benchmark / re-certification
  -> status update or revocation
```

Такой lifecycle поддерживается сразу несколькими слоями evidence: human credential stack, benchmark-based capability attestation, issuer governance и status-aware VC ecosystem. [C2][C3][C10][C20][C26]

### 6.2 Почему re-certification обязательна

`ACEI` даёт directional argument, что компетентностная эрозия и dependence-driven degradation реальны и что periodic monitoring/re-certification логически необходимы. Но корпус так же ясно говорит, что validated interval или longitudinal agent-specific decay curve пока не установлены. [C15]

Отсюда следует важное ограничение `v1`: Logos не должна записывать в D02 жёсткий universal TTL для всех capability claims. Корректнее зафиксировать принцип обязательного мониторинга и повторной оценки, чем симулировать доказанную частоту пересертификации. [C15]

### 6.3 SkillsBench и другие новые inputs

`SkillsBench` усиливает competence-specific ветку lifecycle, потому что вводит skills-oriented benchmark с deterministic verifiers. Но зрелость этого benchmark пока medium-confidence; его полезно включать в re-benchmarking design как promising input, а не как settled universal oracle. [C14]

В сочетании с `GAIA`, `WebArena`, `AgentBench` и `OSWorld` это означает, что Logos может проектировать domain-dependent re-certification bundles, но ещё не имеет права заявлять, что существует единая cross-benchmark шкала зрелости capability. [C12][C14]

### 6.4 Continuous monitoring как нормативная граница

Даже там, где corpus не даёт validated decay interval, он усиливает архитектурный вывод через policy context: competence and capability claims нельзя рассматривать как разовые вечные доказательства. Relevant policy and ecosystem evidence поддерживает идею, что machine-readable competence claims и continuous oversight становятся частью более широкого governance landscape. [C25][C26]

Следовательно, lifecycle D02 должен быть "monitoring-first":

- credential должен иметь status-awareness;
- capability claim должен быть привязан к snapshot date;
- устаревший claim должен пересматриваться, а не silently оставаться валидным;
- domain drift должен вести к re-benchmarking, а не к косметическому обновлению метаданных. [C13][C15][C26]

---

## 7. Приватность competency claims

### 7.1 Что именно должно скрываться

Для D02 privacy-проблема формулируется не как "надо скрыть вообще всё", а как вопрос минимально достаточного раскрытия competence evidence. Holder или verifier должны иметь возможность доказать релевантную компетентность без полного раскрытия всего образовательного и capability-профиля. Это и есть competence-specific selective disclosure requirement. [C23]

Практически это означает, что для matching задачи должны раскрываться только те claims, которые релевантны проверке допуска:

- наличие нужной квалификационной категории;
- наличие релевантного capability attestation;
- актуальность benchmark snapshot;
- при необходимости - issuer/oracle status.

Полный portfolio dump противоречит самой идее privacy-preserving competence presentation. [C23]

### 7.2 Что принадлежит D02, а что нет

D02 владеет только functional requirements к privacy:

- competency claims должны поддерживать selective presentation;
- verifier не должен видеть лишние qualification details;
- competency proof не должен автоматически раскрывать полный профиль владельца или агента. [C23]

Но выбор конкретной криптографической схемы остаётся вне полного владения D02. В рамках идентификационного стека уже зафиксировано, что `BBS+` рассматривается как предпочтительный профиль для unlinkable derived proofs в → L1/D01 §7; однако область D01 §7 охватывает преимущественно Binding VC privacy (DID-привязка «человек–Логос»). Конкретные требования к selective disclosure компетентностных claims (что раскрывать при task-matching) специфицируются в D02; криптографическая инфраструктура (BBS+ mechanism) берётся из → L1/D01 §7 как shared infrastructure layer. Здесь D02 делает компетентностную проекцию на этот идентификационный вывод, сохраняя ownership над вопросом «что» раскрывать, а не «как». [C23]

### 7.3 Design boundary для competency claims

Итоговая граница для `G15` в этом черновике выглядит так:

1. D02 определяет, какие competence claims требуется предъявлять для task fit; [C23]
2. D02 не определяет самостоятельно ZKP/BBS+/SD-JWT cryptosuite; [C23]
3. криптографическая инфраструктура (BBS+ / ZKP mechanism) делегируется в идентификационный слой: → L1/D01 §7 (shared BBS+ infrastructure); требования к содержанию раскрытия (что именно открывать при task-matching) остаются в D02. [C23]

Такой подход исправляет слабость `v0_legacy`, где privacy могла читаться как "ещё одна полная подсистема внутри D02". В `v1` это уже не отдельная автономная тема, а строго очерченный requirement boundary. [C23]

---

## Связь с другими измерениями

| Измерение | Связь |
|-----------|-------|
| **L1/D01 (Идентификационное)** | DID/VC-базис и privacy-safe presentation profiles являются обязательной основой для competence credentials; selective disclosure и профиль `BBS+` для чувствительных credential presentations описаны в → L1/D01 §7. |
| **L1/D03 (Материальное)** | D02 определяет смысл `ESq ≥ ESc`, но фактический execution/resource gating находится в механизме доступа к ресурсам → L1/D03 §3. |
| **L1/D09 (Репутационное)** | `Agent Capability VC` и capability evidence выступают входом для `Certified Capability`, но репутационный score и lifecycle принадлежат → L1/D09 §1, → L1/D09 §2 и → L1/D09 §6. |
| **L2/D02 (Инструменты среды)** | Компетентностное ограничение сочетается с экономическим инструментарием среды; платёжная и инструментальная сторона dual constraint вынесена в → L2/D02 §5. |
| **L2/D08 (Нормативно-правовое)** | Policy context AI literacy, GDPR и иные compliance-рамки важны как внешняя граница, но не принадлежат D02; нормативное владение находится вне этого документа. |
| **L2/D05 (Экономические стимулы)** | Knowledge Marketplace как дополнительный путь роста `ESq`: продажа верифицированных знаний → `Knowledge Quality VC` от институционального верификатора → потенциальный вклад в `ESq_base`. Incentive-перспектива: `→ L2/D05 §6.4`; контрактная упаковка: `→ L3/D04 §5`. |

---

## Открытые вопросы

### Требуют решения (Critical / High)

1. **[Critical] G11: Где проходит граница runtime enforcement между компетентностной семантикой и фактическим execution gating?** В legacy-логике это место было размыто между ES-квотой, стоимостью задачи и инфраструктурным исполнением.
   > **Resolved (v4.0 draft):** D02 владеет только semantics и mapping-логикой `ESq`, `ESc`, `ESq_base` и `capability_multiplier`; фактическое enforcement hook относится к → L1/D03 §3, а платёжный контур - к → L2/D02 §5. Внешнего канонического стандарта для runtime `ESq/ESc` gating корпус не показывает. [C24]

2. **[High] G15: Как предъявлять competence claims без раскрытия полного профиля?** Риск из legacy-версии состоял в том, что privacy трактовалась как расплывчатое пожелание, а не как архитектурное требование.
   > **Resolved (v4.0 draft):** D02 фиксирует requirement на selective disclosure только релевантных qualification/capability claims. Конкретная cryptographic proof-scheme не описывается здесь полно и делегируется в → L1/D01 §7; для D02 достаточно зафиксировать privacy boundary и минимально достаточное раскрытие. [C23]

### Требуют исследования (Medium)

3. **[Medium] New issue candidate: отсутствие единого канонического benchmark-oracle стандарта.** Корпус даёт сильный governance baseline через `W3C VC`, `ToIP` и lifecycle patterns, но не показывает один ratified global standard для issuance capability credentials.
   > **Open.** Зафиксировать как issue candidate для последующего формального выбора oracle-governance profile; draft- и ecosystem-specific inputs (`ERC-8126`, `ERC-8004`, AQVC, Corgent) пока остаются вспомогательными. [C20][C21][C22]

4. **[Medium] New issue candidate: отсутствие validated agent-specific decay interval.** `ACEI` и связанные сигналы подтверждают необходимость monitoring/re-certification, но не дают validated периодичности для Logos capability claims.
   > **Open.** Нужен отдельный empirical policy layer: по каким доменам, benchmark families и рисковым профилям задаётся re-certification cadence. [C14][C15]

5. **[Medium] New issue candidate: нет канонической внешней формулы для `ESq_base` и `capability_multiplier`.** Есть только partial quantitative building blocks и experimental formulas.
   > **Open.** Следующий шаг - governance-defined lookup table поверх `ESCO`/`EQF` alignment и capability evidence, без преждевременной фиксации чисел внутри D02. [C16][C17][C18][C19]

6. **[Medium] New issue candidate: repo-consistency drift по маркировке `G15`.** В части артефактов старый `G15` ещё относится к cross-border recognition, тогда как актуальный `research.md` относит `G15` к privacy/selective disclosure.
   > **Open.** Нужна синхронизация label usage между `research.md`, `evidence-pack.md` и downstream drafts, чтобы не расползалась семантика gap IDs. [C7][C23]

7. **[Medium] G16: `Knowledge Quality VC` как дополнительный источник `ESq_base`.** В `L2/D05 §6.4` зафиксирована концепция, при которой продажа верифицированных знаний через Knowledge Marketplace порождает `Knowledge Quality VC` от институционального верификатора — аттестацию domain-компетенции, которая по своей природе аналогична Educational VC.
   > **Open.** Требуется design-решение: (a) является ли `Knowledge Quality VC` полноправным источником `ESq_base` наравне с Educational VC; (b) или он действует как отдельный additive-компонент поверх базовой формулы; (c) какие требования к верификатору (лицензия, аккредитация) необходимы, чтобы выданный VC засчитывался в `ESq_base`. Если (a) или (b) принято — нужно обновить формулу §5.4 и pipeline §6.1. Контекст: `→ L2/D05 §6.4`, `→ L3/D04 §5`.
