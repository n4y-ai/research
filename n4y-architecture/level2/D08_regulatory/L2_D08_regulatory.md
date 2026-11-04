# УРОВЕНЬ 2 / ИЗМЕРЕНИЕ 08: НОРМАТИВНО-ПРАВОВОЕ
> **Уровень:** L2 — Среда (инфраструктура)  
> **Измерение:** D08 — Regulatory  
> **Версия:** 4.1  
> **Статус:** Финал v4.1 | Дата: 10.05.2026  
> **Зависимости:** L1/D06, L2/D01, L2/D02, L2/D09
---
## Ключевой вопрос
**Какие правовые и регуляторные рамки зашиты в среду и как они соотносятся с внешними юридическими системами?**
---
## Аннотация
Этот черновик собирает owner-слой регуляторной архитектуры N4Y.AI именно как среды. В отличие от `L1/D06`, где право выступает как agent-level projection, здесь полное описание принадлежит самим режимам: `AI Act`, `MiCA`, `eIDAS 2.0`, `GDPR`, `PLD 2024/2853`, `AMLR 2024/1624`, `NIS2`, `CRA`, Data Act и supervisory coordination. [C1][C3][C6][C7][C10][C11][C12][C16]
По сравнению с [PREVIOUS VERSION] `v4.0` усиливает пять вещей: фазированную шкалу `GPAI`/high-risk enforcement, жёсткую фиксацию `AGC = EMT` и `Qi = OCA`, явную controller-архитектуру с `DPIA`, отдельный cyber-law слой `NIS2 + CRA`, а также оговорку, что `G13` по multi-agent liability пока остаётся главным образом доктринальной конструкцией без устойчивой case law. [C2][C4][C7][C10][C11][C17][C18][C19]
> «Регулятивное соответствие: KYC/AML, соответствие ценных бумаг для Qi, международная координация.» `[стратегия: вектор]`
---
## Содержание
1. [Регуляторный обзор](#1-регуляторный-обзор)
2. [AI Act: среда как AI-система и downstream GPAI-слой](#2-ai-act-среда-как-ai-система-и-downstream-gpai-слой)
3. [MiCA: AGC как EMT, Qi как OCA](#3-mica-agc-как-emt-qi-как-oca)
4. [eIDAS 2.0 и GDPR](#4-eidas-20-и-gdpr)
5. [PLD 2024/2853 и AMLR](#5-pld-20242853-и-amlr)
6. [NIS2 и CRA](#6-nis2-и-cra)
7. [Smart contracts и кросс-юрисдикционное сравнение](#7-smart-contracts-и-кросс-юрисдикционное-сравнение)
8. [Regulatory interplay и conflict resolution](#8-regulatory-interplay-и-conflict-resolution)
---

> **Scoping note:** OI-003 (MiCA CASP лицензия) и OI-007 (AGC EMT лицензия) имеют статус `architectural-direction-resolved` / `architectural-perimeter-resolved`: D08 фиксирует regulatory framework, compliance obligations, deployment baseline и acceptance criteria, при этом финальный transactional licensing остаётся `awaiting external legal counsel for licensing transaction`. OI-059 (Qi/MiFID II classification) — `monitoring-posture-resolved`: explicit re-evaluation triggers зафиксированы в §3.2, formal ESMA RTS не опубликован. Все claim-теги `[C{N}]` ссылаются на `evidence/normalized-corpus.md` (C1–C19). G13 (multi-agent PLD liability) = doctrinal; zero published case law на дату выпуска.

## 1. Регуляторный обзор
`L2/D01` отвечает на вопрос, что это за среда и каков её статус; полный ontology-side ответ закреплён в `→ L2/D01 §3`. Здесь фиксируется другой слой: какие внешние режимы превращаются во внутренние design constraints среды и как они накладываются друг на друга. [C1][C3][C6][C7][C10][C11][C12][C16]
Для N4Y.AI карта правового режима многослойна:
| Режим | Что он квалифицирует | Что становится design constraint |
|------|----------------------|----------------------------------|
| `AI Act` | AI-system lifecycle, provider/deployer duties, transparency, high-risk cases | documentation, monitoring, sandbox, logging, oversight [C1][C13][C15] |
| `MiCA` | token issuance и crypto-service layer | `AGC` issuer path, `Qi` white paper path, `CASP` pressure [C3][C4][C5] |
| `eIDAS 2.0` | legal identity rails и evidentiary force | `EUDI Wallet`, `QEAA`, `QES`, qualified seals [C6] |
| `GDPR` | controller architecture и lawful processing | `joint controller`, `DPIA`, minimization, Art.22 gates [C7][C12] |
| `PLD 2024/2853` | strict liability for software/AI | cloud/SaaS exposure, ongoing defect risk, multi-actor chain [C16][C17][C18][C19] |
| `AMLR 2024/1624` | anonymous-account prohibition | no pseudonymous service layer, beneficial-owner linkage |
| `NIS2` / `CRA` | cyber-law and product-security baseline | staged reporting, secure-by-design, vulnerability handling, SBOM [C10][C11] |
| `Data Act` | smart-contract reversibility | kill switch / interruption capacity [C8][C12] |
Один и тот же узел может подпадать сразу под несколько режимов. Execution/logging контур для среды одновременно является доказательственным слоем по `AI Act`, источником персональных данных по `GDPR`, потенциальным disclosure-object для `PLD` и incident artifact для `NIS2`/`CRA`. [C10][C11][C12][C13][C18]
Поэтому обзор нельзя строить вокруг вопроса "какой закон главный". Корректный вопрос другой: какая комбинация обязанностей активируется в конкретном сценарии использования. [C12]
Для ориентира полезно держать короткую карту "режим -> главный тип риска":
| Режим | Главный risk vector для среды |
|------|-------------------------------|
| `AI Act` | неверная risk-classification и слабая post-market governance [C1][C13] |
| `MiCA` | незаконный issuer/service setup и просрочка transition windows [C3][C4][C5] |
| `eIDAS 2.0` | недостаточная юридическая сила identity/attestation chain [C6] |
| `GDPR` | неоформленный controller split и отсутствие `DPIA` [C7] |
| `PLD` | post-launch defect exposure при updates и orchestration failures [C16][C18][C19] |
| `AMLR` | недопустимая псевдонимность financial surface [C12] |
| `NIS2` / `CRA` | несвоевременный reporting и слабая cyber-governance дисциплина [C10][C11] |
Минимальная unresolved-карта, которую `v1` не имеет права закрывать формально:
- `OI-003` — `CASP` path и дедлайн `01.07.2026` остаются **pending legal opinion**. [C5]
- `OI-007` — `EMI`/issuer path для `AGC` остаётся **pending legal opinion**. [C3]
- `OI-010`–`OI-015` — AI Act / GDPR implementation gaps закрыты: OI-010/011/012/027/041 — F7 cluster-AI-Act-compliance (`§2.3-§2.7`); OI-013/014/015 — F8 cluster-GDPR-data-protection (`§4.4`, `§4.6`, `§4.7`). [C7][C12][C13]
- `OI-059` — `Qi = OCA` канонично, но MiCA/MiFID II boundary остаётся **pending legal opinion**. [C4]
- `G11` — supervisory coordination между режимами остаётся незавершённой. [C12]
Читательская оговорка для `v1` проста:
- если вопрос касается owned legal regime, документ даёт полный environment-level answer;
- если вопрос касается токеномики, smart-contract implementation или governance mechanics, здесь остаётся только правовая проекция;
- если вопрос упирается в unsettled authority practice, документ фиксирует risk posture, а не изображает ложную окончательность. [C12]
Это особенно важно для `OI-003`, `OI-007`, `OI-059`, `G11` и `G13`, где доказательственная база уже сильна, но institutional closure ещё отсутствует. [C5][C12][C17]
В отличие от [PREVIOUS VERSION], этот обзор работает не как перечень актов, а как карта наложения режимов на архитектуру среды. [C12]
---
## 2. AI Act: среда как AI-система и downstream GPAI-слой
`AI Act` — owner-концепт этого измерения. Полное agent-level применение к конкретному Логосу описано в `→ L1/D06 §3`; здесь фиксируется environment-level baseline, в котором N4Y.AI выступает как system-layer actor с provider/deployer responsibilities и с downstream зависимостью от `GPAI` supply chain. [C1][C2][C13][C14]
### 2.1 Базовая квалификация среды
По текущему корпусу `AI Act` задаёт базовый compliance framework для среды: risk-based logic, provider duties, deployer duties, transparency duties, `GPAI` obligations на уровне базовых моделей и sandbox support. [C1]
Практический смысл:
- N4Y.AI нельзя описывать как "просто платформу", если она проектирует, интегрирует и развёртывает agentic AI-system с предсказуемым operational impact. [C1][C13]
- Для базовых моделей релевантно не только "какую LLM мы используем", но и как N4Y.AI принимает downstream documentation, instructions и risk-signals от upstream `GPAI` provider. [C2][C14]
- Если сама среда не выпускает собственную `GPAI` model, прямые Chapter V duties лежат прежде всего на upstream provider; но это не отменяет system-level обязанностей среды как deployer/consumer этого слоя. [C1][C2][C14]
Следовательно, "`GPAI`" в этом документе означает не автоматический provider status среды, а её операционную зависимость от чужих GPAI-обязательств и документации. [C2][C14]
### 2.2 Временная шкала и GPAI layer
| Дата | Что меняется | Почему это важно для среды |
|------|--------------|----------------------------|
| `02.02.2025` | запретные практики AI Act в силе (Art.5) | unacceptable-risk AI systems запрещены на EU рынке |
| `02.08.2025` | старт `GPAI` obligations | downstream chain должен уметь принимать documentation, copyright и transparency inputs [C2][C14] |
| `02.08.2026` | Commission enforcement powers по `GPAI`; у MS должны быть sandboxes | regulatory comfort заканчивается, governance становится hard-enforcement topic [C2][C15] |
| `02.08.2027` | закрывается legacy window для pre-2025 models | временной люфт исчезает [C2] |
Поэтому формула "полный AI Act только в 2027" неверна для N4Y.AI: часть обязанностей уже operationally значима в `2025-2026`. [C2][C14][C15]
### 2.3 Art.10 Data Governance
`Art.10` задаёт provider-side требования к data governance для high-risk AI systems; для N4Y.AI этот слой становится обязательным с `02.08.2026` и читается через high-risk posture среды и tri-party bearer split `[A13][A14][A82]`. Базовый bearer здесь — provider (N4Y.AI at system/model layer per Art. 25(1)), а deployer-side context не отменяет обязанность вести quality, provenance и bias-controls на стороне system layer. [C1][C4][C17][C19]
Минимальный contour для среды:
| Контроль | Что должно быть обеспечено | Required artifact |
|----------|----------------------------|-------------------|
| relevance / representativeness / error-completeness | training, validation и testing datasets должны быть релевантны intended purpose, sufficiently representative и очищены от известных quality gaps | dataset inventory + quality criteria register |
| bias examination | процедуры выявления, prevention и mitigation bias должны быть задокументированы до training и на этапе regression testing | bias assessment log + corrective-action record |
| provenance and preparation | collection sources, selection logic, labeling and cleaning steps должны быть traceable | provenance register + data preparation SOP |
| special-category data | special categories of personal data допускаются только в узких случаях `Art.10(5)` и требуют necessity-based justification | lawful-basis memo + restricted-access evidence pack |
| testing-only systems | even when no training/validation datasets are used, testing-data controls still apply under `Art.10(6)` | test dataset dossier + evaluation protocol | [C1][C2][C3]
`Annex IV` связывает этот блок с technical documentation: N4Y.AI должен хранить dataset provenance, collection/selection methods, labeling/cleaning procedures и ключевые dataset characteristics в compliance file среды. [C3]
Поэтому Article-level answer для N4Y.AI такой: provider-side data governance выполняется centrally в environment layer, а deployer-side data-protection overlays остаются смежным режимом, не подменяющим `Art.10`. Article-specific implementing detail beyond the Regulation text в current corpus не подтверждён; действующая безопасная позиция — исполнять прямой текст `Art.10` + `Annex IV` и вести negative-finding log по secondary acts. [C18][C19]
### 2.4 Art.17 Quality Management System
`Art.17` требует документированную `QMS` для providers high-risk AI systems; для N4Y.AI обязательность приходит `02.08.2026`, а bearer-side split должен быть прозрачно разведен между provider controls и deployer operations `[A14][A82]`. `QMS` здесь принадлежит owner-слою `L2/D08`, потому что она охватывает не только модель, но и orchestration, updates, incidents и post-market governance среды. [C4][C5][C6][C17]
`QMS` minimum для среды:
| Contour | Что должно быть обеспечено | Required artifact |
|---------|----------------------------|-------------------|
| regulatory strategy | mapping `Art.8-15`, role allocation, acceptance gates и accountability path | compliance matrix + role assignment register |
| design / development / validation | change-controlled development, testing, validation and release criteria for model and orchestration layers | SDLC/QMS procedure set + validation records |
| data and risk management | связка с `Art.10` data governance и с lifecycle risk controls | data-governance linkage note + risk register |
| incident and corrective action | intake, root-cause analysis, CAPA and serious-incident escalation | incident SOP + CAPA log |
| post-market monitoring | interface between `QMS`, `Annex IV` technical documentation and `Art.72` ongoing monitoring | monitoring plan + evidence retention schedule | [C5][C6]
`ISO 42001` может помогать operationalize management discipline, но не даёт AI Act presumption of conformity; standards-track posture корпуса проходит через Commission standardisation request и `prEN 18286`, который ещё не опубликован как harmonised standard in the OJ. [C7][C18]
Практический вывод: N4Y.AI нужен один environment-level `QMS` file, который связывает design controls, documentation, change management, monitoring и incident handling, а не набор несвязанных policy notes. `Annex IV` и `Art.72` остаются встроенными interfaces этой системы, а не отдельными substitute-документами. [C6]
### 2.5 Art.27 Fundamental Rights Impact Assessment (FRIA)
`FRIA` (Fundamental Rights Impact Assessment) относится к deployer-side high-risk use contexts, а не к provider-side baseline по умолчанию; в текущем bearer split `[A14]` + `[A83]` provider N4Y.AI обязан подготовить deployer-ready package и trigger criteria, но сам `FRIA` выполняет actor, входящий в роль deployer в конкретном deployment context. Связка с `DPIA` допустима по coordination path, но не превращает две оценки в одну юридическую форму. [A83][C8][C17]
Для N4Y.AI `Art.27` materially важен с `02.08.2026` как часть high-risk chapter: к моменту production deployment должен существовать repeatable `FRIA` workflow для всех covered contexts. Конкретные категории deployer-side высокорисковых use cases перечислены в `Annex III AI Act` (биометрическая идентификация, критическая инфраструктура, education, employment, essential services, law enforcement, migration, justice). N4Y.AI как marketplace-level operator и sandbox deployer должен проверить какие операции попадают в эти категории. [A83][C4][C8]
Минимальный `FRIA` checklist:
| FRIA element | Что должно быть обеспечено | Required artifact |
|--------------|----------------------------|-------------------|
| use-case context | deployment purpose, operational context и affected groups должны быть explicitly scoped | FRIA scoping sheet |
| rights-risk analysis | конкретные risks of harm to fundamental rights must be identified per use context | rights-risk register |
| human oversight | oversight, intervention and halt measures must be documented before go-live | oversight control map |
| complaint / redress path | subjects and operators need a documented complaint and escalation route | complaint-handling procedure |
| review triggers | reassessment is required when deployment context or risk profile materially changes | FRIA review trigger log |
| DPIA coordination | overlap with `DPIA` may be coordinated, but duties remain legally distinct | FRIA-DPIA linkage memo | [C8][C9]
Official Commission / AI Office `FRIA` template is not evidenced in the corpus as of May 2026; until such template exists, N4Y.AI must keep an internal structured `FRIA` form rather than waiting for secondary guidance. [C10][C18]
### 2.6 Art.50 AI Disclosure
`Art.50` вводит transparency duties, которые выходят за пределы high-risk chapter alone и применяются с `02.08.2026`; в tri-party split `[A14][A82]` provider/deployer обязан обеспечить notice или marking в зависимости от interaction/content pattern (provider at system layer per A82 + deployer at deployment context), while Logos remains the instrument of disclosure rather than statutory bearer. [C11][C13][C17]
Минимальный disclosure contour:
| Scenario | Что должно быть обеспечено | Required artifact |
|----------|----------------------------|-------------------|
| person interacts with an AI system | user-facing notice must appear before or at the point of meaningful interaction | UI disclosure rule + evidence screenshot set |
| AI-generated or manipulated content | outputs that fall under `Art.50` marking duties require machine-readable marker and human-readable disclosure where supported | content-marking policy + metadata specification |
| deepfake / synthetic media | manipulated or synthetic content needs explicit disclosure treatment, subject to statutory exceptions | synthetic-content disclosure SOP |
| public-interest text use cases | deployer-side disclosure must be added where the use context activates public-interest publication duties | publication checklist + review log |
| channel governance | disclosure has to be consistent across UI, API and mediated interaction surfaces | channel-control matrix | [C11][C12]
Legal ownership of the duty stays here, but implementation surfaces are projection-only to `→ L1/D05 §1.2`, `→ L1/D05 §3` and `→ L2/D04 §3`. Current Commission signal supports layered metadata + watermarking patterns, yet no binding single machine-readable format is evidenced in force. [C12][C18]
Следовательно, N4Y.AI должен проектировать `Art.50` как mandatory disclosure baseline across human-facing and content-bearing channels, а не как optional UX enhancement. [C11][C12][C13]
### 2.7 Art.57 Regulatory Sandbox
`Regulatory sandbox` в final `AI Act` закреплён в `Art.57`, а не в proposal-era `Art.53`; participation path therefore must be mapped to the final numbering and member-state authority route `[A14]`. Для N4Y.AI это design route for supervised experimentation, а не waiver of other regimes: sandbox participation does not suspend `GDPR`, `PLD`, `AMLR` or other applicable duties. [C14][C15][C17][C19]
Sandbox availability becomes operationally relevant by `02.08.2026`, when Member States must ensure at least one sandbox route, while detailed participation decisions run through competent authorities and should be handled within the statutory process windows. [C15]
Минимальный sandbox contour:
| Step | Что должно быть обеспечено | Required artifact |
|------|----------------------------|-------------------|
| eligibility and entry | intended use case, innovation rationale and compliance-learning objective must be scoped before application | sandbox eligibility memo |
| authority path | application must go through the competent national authority / designated sandbox route | application package + authority mapping |
| participation duties | testing conditions, cooperation duties, evidence logging and safeguards must be documented during participation | sandbox participation plan + test protocol |
| exit and follow-up | exit report, remediation actions and go/no-go decision for production must be explicit | exit report + remediation tracker |
| jurisdiction scan | route availability must be tracked per Member State | member-state sandbox map | [C15][C16]
Current corpus supports an active Spanish route, a German pilot route and no clearly evidenced active French `Art.57` route as of May 2026; this is enough for routing posture, but not for assuming a single EU-wide interface is already operational. [C16][C18]
> **Допущение [A82, position]:** N4Y.AI acts as **provider** at the system/model layer (qualifying as provider when the system is placed on the market or put into service, per Art. 25(1)) and as **deployer** in operated production-sandbox contexts under Art. 26; Logos remains an instrument, not a statutory bearer. This supplements `[A14]` (tri-party responsibility) with AI Act statutory role labels.

> **Допущение [A83, position]:** FRIA (Art. 27) triggers for N4Y.AI only where it acts as **deployer** in a high-risk use context listed in Art. 27(1) Annex III categories (biometric identification, critical infrastructure, education, employment, essential services, law enforcement, migration, justice). It does NOT trigger at the platform/provider layer absent a concrete high-risk deployment decision, per Art. 27(1) literal scope.

Сводная bearer matrix below uses `[A14]` + `[A82]` + `[A83]` as the working allocation across the five article sections:
| Article | Primary bearer | Deployer-side role | Logos role | Evidence artifact | Operational owner | Residual ambiguity |
|---------|----------------|--------------------|------------|-------------------|-------------------|--------------------|
| `Art.10` | provider | supplies deployment context constraints and local data-handling overlays | instrument | dataset governance file + Annex IV data dossier | N4Y.AI compliance / data governance | exact secondary acts remain absent [C18] |
| `Art.17` | provider | must follow released controls and feed incidents / monitoring events upstream | instrument | environment `QMS` + CAPA / monitoring records | N4Y.AI compliance / operations | harmonised standard not yet published [C18] |
| `Art.27` | deployer in covered high-risk context | performs `FRIA` before go-live and on material change | instrument | `FRIA` file + review log | deployer compliance owner | official template not yet published [C10] |
| `Art.50` | provider and/or deployer depending on use pattern | ensures channel-specific disclosure at point of interaction or publication | instrument of notice | disclosure matrix + marking evidence | product / channel owner under compliance control | binding single format not yet fixed [C12][C18] |
| `Art.57` | provider / prospective provider for application, with competent authority oversight | cooperates with sandbox conditions in the deployment context | instrument inside test environment | sandbox application + exit report | regulatory affairs / compliance | member-state routes remain fragmented [C16][C18] | [C17]
---
## 3. MiCA: AGC как EMT, Qi как OCA
Экономическая природа `AGC` и `Qi` описывается полностью в `→ L2/D02 §4` и `→ L2/D02 §6`. Здесь остаётся только тот слой, который отвечает на вопрос о regulatory perimeter токенов среды. [C3][C4][C5]
### 3.1 AGC: EMT-bound baseline
`AGC` описывается полностью в `→ L2/D02 §4` и `→ L2/D02 §6.2`. Здесь: `AGC` фиксируется как `EMT`-bound operational instrument среды, то есть **AGC = EMT canonically**. [A3][C3]
Из этой квалификации следуют жёсткие последствия:
- нужен licensed issuer model, а не light-touch token release; [C3]
- нужен white paper, reserve/redemption discipline и prudential contour; [C3]
- launch `AGC` становится вопросом institutional architecture, а не одной tokenomics. [C3]
Per `Delegated Regulation (EU) 2025/415` (in force с `13.04.2025`), capital adequacy для `EMT` issuer operationally читается через тесты `Method A` / `Method B` / `Method C` и фиксированный минимум собственных средств; для tokenized e-money baseline этой секции значимы thresholds `2%` от outstanding balance `EMT` либо `3%` для `EMT` в распределении, с prudential adjustment по institutional structure и supervisory discretion. [A3][A14][C3]
Per `Delegated Regulation (EU) 2025/1264` (in force с `23.10.2025`), reserve assets должны соответствовать `HQLA`-классам: fiat, государственные облигации и qualifying central bank deposits; crypto-assets из primary reserve явно исключены. `Qi` сохраняется как supplementary stability buffer over fiat/`HQLA` primary, а не как primary collateral. [A18][A47][C5][E8]
Production `EMT`-issuance для `AGC` имеет два архитектурно допустимых пути: (a) `partnered EMI` — авторизованная `e-money institution` в EU + N4Y.AI как technical operator; (b) `own EMI license` — N4Y.AI как лицензированный `e-money issuer`, что требует minimum capital и governance setup. Финальный выбор зависит от `external legal counsel` и corporate structure decision; архитектурное направление и compliance gates зафиксированы, но transactional licensing остаётся `awaiting external legal counsel for licensing transaction`. [A3][A14][C2]
Это продолжает линию [PREVIOUS VERSION], но в `v4.0` формулировка должна быть строже: production launch для `AGC` требует licensed issuer architecture, а не light-touch token release. [A3][A14][A18][A47][C2][C3][C5]
### 3.2 Qi: OCA, не ART
`Qi Energy` описывается полностью в `→ L2/D02 §4.4` и `→ L2/D02 §6.3`. Здесь: фиксируется только regulatory boundary. [C4]
Текущая canonical квалификация для `Qi` — `OCA` по MiCA Title II, а не `ART`. В этом draft-е используется только `OCA`. [C4]
Практические выводы:
- базовый режим требует white paper path, а не EMT/ART-style issuer authorization; [C4]
- governance/value features сами по себе не делают `Qi` автоматически `EMT`; [C4]
- boundary `OCA` ↔ `MiFID II financial instrument` формально не закрыта отдельным `ESMA RTS`. [C8]
Boundary `OCA` ↔ `MiFID II financial instrument` для `Qi` не закрыта формальным `ESMA RTS` по состоянию на даты текущего research. N4Y.AI применяет monitoring posture: периодическая re-evaluation квалификации `Qi` против emerging `ESMA` guidance, `EBA` Q&A и национальных interpretations; явные re-evaluation triggers — (1) публикация `ESMA RTS` по governance tokens, (2) изменение `Qi` staking yield mechanism с прохождением threshold-индикаторов securities-like behaviour, (3) cross-border enforcement action в EU member state по similar tokens. [A18][A47][C8]
Иными словами: `Qi = OCA` — текущий canonical answer; вопрос закрывается как monitoring posture с negative finding по отсутствию formal `ESMA RTS`, а не как основание объявить финальный investment-qualification verdict. [A18][A47][C8]
### 3.3 CASP layer и дедлайн 01.07.2026
Отдельно от token classification стоит service layer. Если N4Y.AI оказывает marketplace, exchange, custody или сходные crypto-asset services, среда входит в `CASP` perimeter. [C5]
Ключевые тезисы:
- `01.07.2026` — hard stop для transitional logic; [C5]
- merely pending application не сохраняет право продолжать operations после дедлайна. [C5]
N4Y.AI marketplace operations попадают в `CASP` perimeter per `MiCA Title V` как минимум через три класса услуг: (a) `exchange of crypto-assets for funds or other crypto-assets` для settlement-контуров с `AGC` ↔ fiat off-ramp; (b) `custody and administration of crypto-assets on behalf of clients`, если N4Y.AI hosts user wallets; (c) `transmission of orders for crypto-assets on behalf of clients`, если Task/Knowledge Marketplace действует как broker-layer. Минимальный compliance contour требует `CASP` authorization в одной EU member state + `passporting`; после `01.07.2026` production EU crypto-asset services без valid authorization или partner route недопустимы. Финальный transactional authorization остаётся `awaiting external legal counsel for licensing transaction`. [A14][A18][C4][C5]
Следовательно, `MiCA` для среды состоит из трёх связанных, но нетождественных вопросов:
| Узел | Что проверяется | Статус в `v1` |
|------|-----------------|---------------|
| `AGC` issuer path | `EMT` / `EMI` boundary | **Architectural direction resolved**: two-path model (`partnered EMI` как launch default, `own EMI` как long-horizon route); transactional licensing остаётся `awaiting external legal counsel for licensing transaction`. [C2][C3] |
| service layer | marketplace / custody / exchange as `CASP` | **Architectural perimeter resolved**: marketplace operations mapped to `CASP` service classes, `CASP` authorization + `passporting` обязательны; transactional authorization остаётся `awaiting external legal counsel for licensing transaction`. [C4][C5] |
| `Qi` classification boundary | `OCA` vs MiFID II spillover | **Monitoring posture resolved**: explicit re-evaluation triggers зафиксированы; formal `ESMA RTS` по состоянию current research не опубликован. [C8] |
### 3.4 Почему D02 и D08 нужно читать вместе
Владелец токенов как инструментов — `L2/D02`; владелец `MiCA` как правового режима — `L2/D08`. Корректная projection formula в этой секции такова:
`AGC` описывается полностью в `→ L2/D02 §4`, `→ L2/D02 §6.2`. Здесь: фиксируется, что operational usefulness `AGC` покупается ценой более тяжёлой legal architecture `EMT`-issuer level. [C3]
`Qi` описывается полностью в `→ L2/D02 §4.4`, `→ L2/D02 §6.3`. Здесь: фиксируется, что governance/value role пока укладывается в `OCA`, но boundary monitoring по `OI-059` остаётся обязательным. [C4]
Итог секции: **MiCA не внешнее примечание к токенам среды; она встроена в design space инструментов.** [C3][C4][C5]
---
## 4. eIDAS 2.0 и GDPR
`eIDAS 2.0` и `GDPR` вместе образуют identity-and-data backbone среды: один режим отвечает за legal/evidentiary rails, другой — за controller architecture и lawful processing. [C6][C7][C12]
### 4.1 eIDAS 2.0: legal identity baseline
Для `v1` достаточно зафиксировать четыре опорных вывода:
- rollout `EUDI Wallet` обязателен к `12.2026`; [C6]
- `QEAA` имеет paper-equivalent evidentiary force; [C6]
- `QES` имеет handwritten-signature equivalence; [C6]
- qualified electronic seals критичны для legal-person attestations платформенного уровня. [C6]
Следовательно, среда не должна строить legal identity rail на "внутренних attestations по умолчанию", если ей нужен cross-MS evidentiary effect. [C6]
В текущем research framing интеграционный путь читается через wallet-compatible VC rails (`OID4VCI` / `OID4VP`) и transition layer между существующей DID/VC-моделью и `EUDI Wallet`. В `v1` это сохраняется как implementation direction, но не превращается в protocol spec. Базовая правовая точка прежняя: юридическая сила возникает не из VC-format itself, а из eIDAS-qualified trust chain. [C6]
### 4.2 QEAA, QES и граница с agent-level legal force
Agent-level legal meaning делегирования и мандата принадлежит `→ L1/D06 §6`. Здесь достаточно зафиксировать:
- `QES` связывает человеческое волеизъявление с юридически значимым действием; [C6]
- `QEAA` усиливает trust-chain атрибутов, статуса и representation facts; [C6]
- среда может опираться на qualified seals там, где нужно удостоверять действия юридического лица или оператора. [C6]
Для N4Y.AI `eIDAS` — это не "ещё один identity standard", а механизм перевода части attestations из технической сферы в юридически признаваемую. [C6]
### 4.3 GDPR: controller-centric baseline
По текущему корпусу `GDPR` остаётся controller-centric even in agentic setting:
- платформа несёт core controller obligations; [C7]
- owner Логоса может оказаться в `joint controller` posture; [C7]
- `DPIA` обязательна для high-risk processing; [C7]
- Art.22 / erasure / minimization tensions остаются material, особенно при blockchain traces и automated decisions. [C7]
Это усиливает базовый тезис [PREVIOUS VERSION]: среда не может спрятать data governance за формулу "агент действовал автономно". С точки зрения `GDPR` важно, кто определяет цели и средства обработки, кто контролирует tooling и кто может остановить либо ограничить processing chain. [C7]
### 4.4 DPIA и joint-controller architecture

> **Допущение [A84, position]:** N4Y.AI lawful basis for agent-loop processing: Art.6(1)(b) GDPR (contractual necessity) for user-directed task execution; Art.6(1)(f) GDPR (legitimate interests, passed balancing test) for platform monitoring/safety; Art.6(1)(a) GDPR (explicit consent) preferred for sensitive use cases and special-category adjacent processing. N4Y.AI acts as data controller for platform-level processing. [A84]

> **Допущение [A85, position]:** N4Y.AI (provider) and Logos owner (deployer) = joint controllers under Art.26 GDPR for processing in operated production sandboxes where both determine purposes and means. DPIA responsibility is shared per Art.26 arrangement; N4Y.AI bears primary responsibility in the Art.26 joint-controller arrangement as the co-determining infrastructure and platform operator; Logos owner bears deployment-context processing responsibility. [A85]

`DPIA` остаётся обязательным pre-deployment gate для processing, которое с высокой вероятностью создаёт высокий риск для прав и свобод физических лиц. Базовая controller-centric логика `§4.3` сохраняется: среда не может ссылаться на автономность Логоса как на основание не проводить `DPIA`; bearer mapping должен быть зафиксирован через provider / deployer / instrument split `[A14][A84][A85]`, а координация с `FRIA` допустима только как соседний workflow, но не как замена `GDPR Art.35` `[A82]`. [C7][C12]
Для N4Y.AI practical trigger matrix выглядит так:
| Сценарий processing | Почему возникает high-risk posture | Что требуется в `DPIA` |
|------|-------------------------------|--------------------------|
| agent-generated scoring, ranking или recommendation, от которых downstream actor materially зависит при доступе к услуге, цене, приоритизации или ограничении пользователя | systematic and extensive evaluation с similarly significant effects; после `SCHUFA (C-634/21)` под риск-периметр попадает не только финальное решение, но и preparatory scoring, если на него существенно опираются | описать decision path, affected persons, error/bias vectors, safeguards, human-review route и evidence that automated output is not treated as unquestionable default |
| cross-sandbox profiling, trust/risk aggregation и persistent behavioral telemetry по нескольким контекстам использования | сочетание profiling / evaluation, масштабности и долговременного накопления данных по критериям `WP248rev01` | зафиксировать categories of data, subjects, geographic scope, retention, profiling logic, false-positive / exclusion risks и minimization controls |
| `HC Microsandbox` и иные scenarios со special-category data или данными повышенной чувствительности | `Art.35(3)(b)` прямо указывает на large-scale special-category processing как mandatory trigger | отдельно описать lawful-basis posture, access segmentation, purpose limitation, deletion schedule и residual-risk treatment before production use |
| systematic monitoring of user-facing surfaces, публично доступных пространств взаимодействия или постоянного observability layer, если monitoring materially affects individuals | `Art.35(3)(c)` и `WP248rev01` требуют оценивать monitoring как самостоятельный trigger, особенно при соединении telemetry с identity / profiling | описать monitoring perimeter, visibility to data subjects, necessity/proportionality, logging minimization и stop conditions |
| non-EEA processor / subprocessor chain, который получает personal data из EU/EEA routing | transfer exposure не заменяет `Art.35` trigger, но materially повышает risk profile и должен входить в `DPIA` transfer map | включить transfer map, transfer tool, destination-law assessment, supplementary safeguards и re-evaluation trigger при смене compute route |

Минимальный `DPIA` package для owner-level `GDPR` baseline состоит из пяти обязательных блоков: (1) systematic description of processing operations, roles, purposes, data categories и recipients; (2) necessity/proportionality analysis; (3) risk register for data subjects; (4) mitigating controls and residual-risk assessment; (5) approval/go-no-go record before production deployment. Проект harmonized `EDPB` template 2026 полезен как operating scaffold, но пока должен читаться как draft meta-template, а не как единственная binding форма.

Role allocation внутри `DPIA` должна быть явно зафиксирована. `N4Y.AI` не может по умолчанию объявлять себя только processor, если именно среда определяет существенную часть целей и средств обработки; где Logos owner совместно определяет purpose, data flow или automated-decision contour, это должно оформляться как controller / `joint controller` split с Art.26-style allocation of tasks `[A14]`. Если такое распределение не зафиксировано документально, production deployment не проходит `DPIA` gate.

По lifecycle discipline `DPIA` - не одноразовый memo. Она пересматривается при изменении целей обработки, категорий данных, модели/agent loop, processor chain, cross-border routing, retention window, human-review path или residual-risk profile. Если после planned mitigations остаётся высокий residual risk, до production deployment обязателен Art.36 prior consultation; перенос unresolved high-risk processing в production как "потом дооформим" несовместим с этим owner-baseline.

Связь с `FRIA` в `§2.5` и `§8.1` должна быть coordination-only: обе оценки могут использовать общую evidence base, но не схлопываются в один юридический документ. Для текущего контура это означает простое правило: `FRIA` отвечает за фундаментальные права в high-risk AI-use context, `DPIA` - за data-protection risk under `GDPR`; ни одна из двух процедур не закрывает другую. [C7][C12]

Корпус также не подтверждает один универсальный lawful-basis answer для всех N4Y agent loops. Поэтому каждый `DPIA` обязан фиксировать scenario-specific Art.6 posture и отдельно проверять, не пытается ли система использовать contractual necessity или legitimate interests как default shortcut там, где нужен более узкий justification path.
### 4.5 Почему Joint Opinion 1/2026 важен именно здесь
В материалах по `G11` Joint Opinion `1/2026` важна не как отдельный закон, а как сигнал: data-protection layer не растворяется внутри `AI Act` implementation. Линия идёт в сторону строгой accountability и участия `DPA` в sandbox/supervisory coordination. [C12]
Следовательно, N4Y.AI не должен рассчитывать на простое "поглощение" `GDPR`-требований AI-режимом. Практический смысл секции: **eIDAS даёт stronger identity rails, но не снимает `GDPR` burden; AI Act даёт product-safety logic, но не заменяет controller law.** [C6][C7][C12]
### 4.6 GDPR Art.22 automated decision-making rights
`GDPR Art.22` в `N4Y.AI` относится к тем decision paths, где решение принимается solely automatically и создаёт legal effect или similarly significant effect для физического лица. Для bearer model среды это означает, что правами и safeguards обладают люди, а не Логос как самостоятельный юридический bearer; role mapping и implementation responsibility therefore pass through platform / owner allocation `[A13][A14][A84]`. Когда Art.22(2)(b) exception используется (necessary for contract), N4Y.AI должен применять Art.6(1)(b) lawful basis per `[A84]` и обеспечить safeguards (human review + contestation).

После `SCHUFA (C-634/21)` секция не может ограничиваться только "финальной кнопкой отказа". Если automated score, ranking, trust classification или recommendation фактически предопределяет downstream decision because a human normally draws strongly on it, такой preparatory act тоже должен анализироваться как потенциальный Art.22 event. Для N4Y это особенно важно в сценариях доступа к marketplace-функциям, risk-throttling, reputation-based exclusion, персонализированной ценовой/priority логике и иных flows, влияющих на жизненно важные возможности пользователя.

| N4Y decision pattern | Art.22 reading | Required control |
|----------------------|----------------|------------------|
| fully automated denial / suspension / restriction of access to a person-facing service | prohibited unless an Art.22(2) exception applies | default = no production use without documented exception, safeguards and review path |
| automated score or recommendation that a human almost always accepts without real re-assessment | still may fall within Art.22 after `SCHUFA` | redesign review workflow so the human can genuinely overturn, contextualize or disregard the output |
| human-in-the-loop review with real authority, time, context and competence | may fall outside "solely automated" only where the review is meaningful rather than formal | interface, staffing and accountability must evidence genuine re-decision rather than rubber-stamping |

Exception handling требует двойной проверки. Art.22(2) exception и Art.6 lawful basis - не одно и то же. Если processing relies on explicit consent, consent must satisfy the higher `Art.7` standard and remain withdrawable; if processing relies on contractual necessity, the necessity test must be documented against the concrete user-requested service rather than against platform convenience; if a Member-State or Union-law authorization is used, the relevant legal act and its safeguards must be cited explicitly. Until scenario-level legal review closes the remaining ambiguity, production teams must document both the Art.22 exception path and the separate Art.6 basis for each individual-affecting workflow.

Safeguards baseline в owner-section выглядит так:
1. before activation of an Art.22-sensitive workflow, user-facing notice must state that automated decision logic is involved and what category of effect it may produce;
2. there must be a meaningful human review route with authority to change outcome, adequate case context, and sufficient time to reassess the result;
3. the person must be able to express a view, contest the result and request re-evaluation without being trapped inside the same automated loop;
4. explanation/access support must expose the factors, data sources and decision context relevant to the concrete outcome, not only a generic model card;
5. opt-in / opt-out UX must be aligned with the selected legal route: explicit opt-in where consent is the only viable exception, and no hidden coercion where access to the core service is realistically impossible without automated profiling.

Special-category data inside Art.22-sensitive processing remains the narrowest path: absent explicit consent or another specific legal authorization, fully automated decisioning based on such data should not be treated as allowed by default.

Граница этой секции намеренно узкая: здесь фиксируются data-subject rights, exception logic и safeguards design under `GDPR`. Smart-contract execution mechanics, interruption tooling и `Data Act Art.36` trigger taxonomy сюда не входят; contract-level kill-switch mechanics remain in `→ L3/D03 §7.3`, а здесь они появляются только как implementation boundary for `OI-002`.

### 4.7 Cross-border transfers and Chapter V baseline
`GDPR Chapter V` в N4Y.AI должен читаться как routing constraint, а не как post-factum procurement appendix. До выбора transfer tool необходимо зафиксировать, кто выступает controller, `joint controller` или processor в конкретной chain of processing; именно от этой mapping зависит, какой transfer module and documentation package applies `[A14]`.

| Transfer context | Current baseline | Operational posture |
|------------------|------------------|---------------------|
| EEA-internal routing | no Chapter V transfer tool required | keep processor map and intra-EEA routing evidence, but no separate third-country transfer mechanism |
| UK / Switzerland / Japan | adequacy route currently available | allowed under current adequacy posture, with periodic re-check when the legal status changes |
| United States via certified importer | current adequacy route remains available under the EU-US Data Privacy Framework | permitted today, but critical flows should retain Decision (EU) 2021/914 fallback documentation because judicial challenge risk remains live |
| India / Russia / China and other non-adequate destinations | no adequacy route | routing-gate posture: no production transfer without documented transfer tool, destination-law analysis and explicit approval by legal/compliance owners |

Для N4Y.AI contractual baseline under Decision (EU) 2021/914 начинается с module selection by actual role pattern, not by vendor label. Controller-to-processor and processor-to-processor chains are the expected default for external compute and subprocessing; where the chain changes, the module choice must be re-opened rather than copied mechanically from a previous vendor packet. Intra-group rules may support recurring internal transfers, but they do not remove the need to control third-party processor chains.

Documented transfer assessment остаётся обязательным even when contractual clauses are signed. `Schrems II` and subsequent enforcement make the owner-level rule explicit: contractual text alone is insufficient if destination-law exposure undermines essentially equivalent protection. Therefore each non-adequate-country route must record destination-law assessment, importer profile, government-access risk, supplementary technical / contractual / organizational measures, and a re-evaluation trigger when the provider, jurisdiction or processing pattern changes.

Supplementary safeguards для cross-border compute должны проектироваться не как appendix, а как architecture controls: encryption with controlled key management, data minimization, pseudonymisation where feasible, strict role-based access, processor-chain transparency, logging of onward transfers, and the ability to suspend or reroute processing when the legal basis for the route weakens. Enforcement against Meta and Google Analytics is enough to treat "signed clauses only" as an unsafe posture for high-sensitivity workflows.

Current country posture for this document therefore remains conservative: UK, Switzerland and Japan can be treated as adequacy destinations under the current corpus; US certified importers are usable but should keep clause-based fallback and legal watch; India, Russia and China are routing-gate jurisdictions requiring case-specific legal review before production routing. Any non-EEA compute chain selected for deployment must also be reflected back into `§4.4` because transfer exposure changes the `DPIA` risk picture and can re-open the go/no-go gate.
---
## 5. PLD 2024/2853 и AMLR
Эта секция описывает два самых жёстких post-launch режима: один отвечает за ущерб от дефектного AI/software product, другой — за невозможность анонимного service layer в крипто-операциях. [C16][C17][C18][C19][C12]
### 5.1 PLD 2024/2853: software and AI are products
Ключевой сдвиг новой `PLD` состоит в том, что N4Y.AI не может уйти от product-liability perimeter ссылкой на "облачную услугу, а не продукт". `Software`, `AI systems`, cloud/SaaS delivery и failure-to-update scenarios входят в сферу директивы. [C16]
Для архитектуры среды это означает:
- liability может включаться не только на момент initial release, но и при updates, retraining и ongoing control; [C16][C19]
- соответствие и сертификаты не дают иммунитета от strict liability, если defect проявляется post-market; [C16][C18][C19]
- отсутствие или сбой нужных updates само по себе может становиться источником defect claim. [C16][C19]
В current project framing эта директива становится operational baseline с `09.12.2026`; для `v1` это preparation deadline, а не далёкий фон.
### 5.2 G13: multi-agent liability chain
`G13` — одна из самых чувствительных тем измерения. Корпус позволяет описать **доктринальную** картину, но не даёт объявить её эмпирически settled. [C17][C18][C19]
Рабочий вывод:
- substantial modifier может стать manufacturer; [C17]
- несколько акторов могут нести joint-and-several liability; [C17]
- burden of proof заметно смещается в сторону claimant через disclosure duties и presumptions; [C18]
- learning/update dynamics делают возможной defect emergence после запуска. [C19]
Обязательная оговорка для `v1`: **multi-agent liability chain остаётся главным образом doctrinal reading, потому что релевантной case law по автономным многоагентным цепочкам пока нет**. [C17][C18][C19]
Поэтому документ не должен превращать `G13` в псевдо-точную матрицу ответственности между "агентом A", "агентом B" и "платформой". Корректнее фиксировать направленность режима:
| Вопрос | Что уже можно утверждать | Что пока нельзя утверждать |
|--------|--------------------------|----------------------------|
| кто может стать liable actor | chain может включать platform/operator/modifier [C17] | точную judicial apportionment formula |
| как доказывается defect | presumptions и disclosure облегчают claimant path [C18] | как суды реально применят threshold в agentic chains |
| когда liability заканчивается | ongoing control и updates продлевают exposure [C19] | универсальный момент "product left control" для cloud agents |
Минимальная operational triage-логика для среды в этой зоне выглядит так:
| Событие | Что нужно проверить первым |
|--------|----------------------------|
| harm after model update | был ли update / retraining под контролем оператора [C19] |
| autonomous multi-agent failure | кто модифицировал output и кто контролировал orchestration chain [C17] |
| disclosure request in claim | существует ли готовый evidence package и нет ли disclosure-default risk [C18] |
### 5.3 AMLR Art.79: no anonymous agentic finance
`AMLR 2024/1624` важна прежде всего через `Art.79`: анонимные accounts для crypto-service layer запрещены, а значит чисто псевдонимный financial surface для агентов несовместим с целевым режимом. Current research framing фиксирует это как effect с `10.07.2027`, требующий beneficial-owner linkage и KYC/AML-compatible onboarding. [C12]
Практический смысл для среды:
- self-owned/pseudonymous agent as financial actor выглядит несовместимым с AML baseline; [C12]
- если N4Y.AI строит `CASP`-like service layer вокруг `AGC` и `Qi`, она не может полагаться на "wallet-only anonymity" как норму по умолчанию; [C5][C12]
- `AMLR` усиливает связь между identity layer, governance layer и token-service layer. [C6][C12]
### 5.4 Почему PLD и AMLR собраны вместе
На первый взгляд эти акты про разное: один — про ущерб от дефектного продукта, другой — про антиотмывочный perimeter. Для N4Y.AI они сходятся в одном: оба режима делают неприемлемой идею полной юридической "бестелесности" агента.
- `PLD` говорит: если система причиняет вред, цепочка экономических операторов всё равно будет искаться. [C17][C18]
- `AMLR` говорит: если система участвует в финансовом service layer, анонимная цепочка владения недопустима. [C12]
Итоговый design principle: автономность агента не отменяет traceable operator chain.
---
## 6. NIS2 и CRA
В [PREVIOUS VERSION] этот слой ещё не был развёрнут полно. В `v4.0` его уже нельзя оставлять implicit: N4Y.AI как cloud-based AI environment попадает не только под AI/product-law, но и под горизонтальный cybersecurity-law. [C10][C11][C12]
### 6.1 NIS2: probable essential-entity baseline
Нормализованный корпус поддерживает тезис, что N4Y.AI plausibly falls into `NIS2` scope как digital-infrastructure/cloud-style entity, а значит должен читать себя скорее через essential-entity baseline, чем через добровольную "best practice" security. [C10]
Главные последствия:
- risk-management duties по Art.21 становятся обязательными; [C10]
- incident reporting идёт в staged cadence `24h / 72h / 1 month`; [C10]
- variation across member states сохраняется из-за uneven transposition. [C10]
В `v1` безопаснее использовать формулу "highly plausible essential-entity reading", а не выдавать classification за административно подтверждённую во всех юрисдикциях. [C10]
### 6.2 CRA: product-security baseline для AI software
`CRA` работает с другой стороны: не как operator-law, а как product-with-digital-elements regime. Для среды это означает:
- secure-by-design и vulnerability handling становятся правовой обязанностью; [C11]
- notified-body readiness приходит к `06.2026`; [C11]
- vulnerability/incident reporting стартует с `09.2026`; [C11]
- full enforcement приходит `12.2027`. [C11]
Product classification остаётся fact-specific: default / important / critical. В текущем корпусе safest formula — "AI platform is likely at least important-category relevant", но не окончательный административный verdict. [C11]
### 6.3 NIS2 и CRA не дублируют друг друга
| Режим | На что смотрит | Тип обязанности |
|------|-----------------|-----------------|
| `NIS2` | operator / entity resilience | governance, incident reporting, risk management [C10] |
| `CRA` | product with digital elements | secure-by-design, vulnerability reporting, conformity path [C11] |
| `AI Act` | AI-specific product safety | high-risk controls, documentation, monitoring [C1][C13] |
Поэтому один серьёзный инцидент в agentic environment может активировать несколько reporting tracks. Это ожидаемая черта current EU stack, а не ошибка чтения. [C10][C11][C12]
Для planning-level view важен и календарь:
| Дата | Regime event | Значение |
|------|--------------|----------|
| `06.2026` | CRA notified-body readiness | нужно заранее определить conformity path [C11] |
| `09.2026` | CRA vulnerability / incident reporting start | без reporting playbook среда войдёт в режим неподготовленной [C11] |
| `12.2026` | PLD operational baseline | liability preparedness перестаёт быть факультативной [C16][C19] |
| `12.2026` | EUDI Wallet rollout window | identity rails должны стать legally interoperable [C6] |
| `12.2027` | CRA full enforcement | product-security debt становится regulatory debt [C11] |
### 6.4 Практический вывод
Если свести секцию к одному тезису, он будет таким: **для среды уже недостаточно "просто быть безопасной"; нужно быть доказуемо управляемой с точки зрения нескольких cyber-regimes одновременно**. [C10][C11][C12]
Это усиливает важность централизованного incident taxonomy, traceable vulnerability handling, evidence storage, совместимого с `AI Act`/`GDPR`/`PLD`, и предварительного сопоставления national competent authorities по юрисдикциям запуска. [C10][C11][C12]
---
## 7. Smart contracts и кросс-юрисдикционное сравнение
Технический smart-contract stack описывается полностью в `→ L2/D03 §5`. Здесь: только его правовая квалификация и место в сравнительной regulatory карте. [C8][C9]
### 7.1 Smart contracts: не owner-технология, а legal object
Для `L2/D08` smart contracts важны не как Solidity inventory, а как вопрос: какой минимум правовой reversibility и accountability должен быть встроен в автоматизированное исполнение среды. [C8][C12]
Ключевой baseline:
- `Data Act` Art.36 требует kill switch или эквивалентную interruption capacity для covered data-sharing smart contracts; [C8]
- это делает reversibility частью правовой архитектуры среды, а не optional safeguard; [C8]
- одновременно возникает напряжение с `GDPR` Art.17 и broader data-rights logic, когда contract traces связаны с персональными данными или необратимым execution. [C7][C8][C12]
Projection formula для этой секции:
`Smart contracts` описываются полностью в `→ L2/D03 §5`. Здесь: они рассматриваются как правовой узел, где автоматизация допустима только вместе с interruption, accountability и минимизацией необратимых последствий. [C8]
### 7.2 Kill switch как юридическое требование
Это один из ключевых сдвигов относительно [PREVIOUS VERSION]. Раньше kill switch выглядел почти как архитектурное удобство. Теперь его нужно читать жёстче:
- для covered contracts это regulatory requirement; [C8]
- он должен быть совместим с rights-preserving reading `GDPR`; [C7][C8]
- kill-switch execution design уходит в `OI-002` (F6 cluster-smart-contract-legal, `→ L3/D03 §7.3`); `OI-014` (Art.22 ADM rights) закрыт F8 в `§4.6`. [C7][C8]
### 7.3 EU vs Singapore vs UK
Кросс-юрисдикционный слой в текущем корпусе deliberately directional, а не exhaustive:
| Юрисдикция | Базовый подход | Что это значит для N4Y.AI |
|-----------|----------------|---------------------------|
| `EU` | плотный binding stack: `AI Act`, `MiCA`, `GDPR`, `PLD`, `AMLR`, `NIS2`, `CRA` | primary compliance center of gravity [C9] |
| `Singapore` | больше supervisory guidance и proportional sandbox logic | полезен как comparator для staged experimentation, но не заменяет EU hard-law baseline [C9] |
| `UK` | сильнее выражен sandbox / regulator-led iterative approach | полезен как comparator для innovation governance, но не как substitute for EU licensing map [C9] |
Главный вывод не в том, что "EU строже", а в том, что N4Y.AI не может проектировать один универсальный legal operating model без choice-of-law and regulator-mapping work. [C9][C12]
### 7.4 Почему comparative section нужна здесь
`L2/D08` не владеет полным private-international-law analysis, но обязан показать, что среда проектируется не в vacuum. Cross-border deployment меняет reporting obligations, identity/evidence expectations, token-service perimeter и степень reliance на sandboxing как легитимационный механизм. [C5][C6][C9][C12]
Именно поэтому в хвосте остаётся `OI-060` (`OI-015` закрыт F8 в `§4.7`): comparators полезны как architecture guidance, но не дают закрытого transnational operating model.

### 7.5 DAO legal-wrapper jurisdiction projection
Для EU-facing `MiCA` perimeter DAO alone не устраняет требования к идентифицируемому bearer of duties, поэтому N4Y.AI читает DAO legal wrapper только как bounded off-chain legal-wrapper layer, а не как bypass regulatory accountability [A86]. Подробности governance-механики, сравнение юрисдикций (`Cayman Foundation Company`, `Switzerland Foundation / Verein`, `Wyoming DAO LLC`, `Marshall Islands DAO LLC / MIDAO`) и stop-rules `Progressive Decentralization` описываются в `→ L2/D09 §1.3` и `→ L2/D09 §8.5-§8.6`.
---
## 8. Regulatory interplay и conflict resolution
Эта секция отвечает на вопрос, который [PREVIOUS VERSION] только намечала: что происходит, когда все перечисленные режимы активируются одновременно, а не по отдельности. [C12]
### 8.1 AI Act vs GDPR: logging, minimization, FRIA, DPIA
Самый важный конфликтный узел — не "AI law против privacy law" вообще, а столкновение operational duties:
- `AI Act` тянет среду к logging, documentation и monitoring; [C1][C13]
- `GDPR` тянет к minimization, storage limitation и purpose discipline; [C7][C12]
- `FRIA` (Fundamental Rights Impact Assessment — обязательная оценка воздействия на основные права по AI Act Art.27 для deployers высокорисковых систем) и `DPIA` (Data Protection Impact Assessment — GDPR Art.35) логически пересекаются, но юридически не слиты в одну форму. [C7][C12]
Следовательно, N4Y.AI не должен рассчитывать на единый "omnibus compliance file", который автоматически решит обе стороны конфликта. В текущем пакете именно это и остаётся unresolved. [C12]
### 8.2 AI Act vs PLD / Data Act / AMLR
Межрежимная картина не менее жёсткая:
- соответствие `AI Act` не устраняет `PLD` strict-liability exposure; [C16][C18][C19]
- `Data Act` добавляет reversibility / kill-switch layer поверх automation; [C8][C12]
- `AMLR` делает неприемлемой анонимную financial surface даже там, где технически она возможна. [C12]
Иными словами, N4Y.AI не живёт в модели "один регулятор спрашивает одно". Он живёт в модели cumulative obligations with no single hierarchy engine. [C12]
### 8.3 Supervisory coordination
По текущему корпусу это самый недозрелый участок всей карты. Нет единой supervisory architecture, которая заранее разводила бы primacy, sequencing или evidence-sharing между AI authorities, `DPA`, financial regulators и cyber authorities. [C12]
Следовательно, правильная формула для `v1`: **координация ожидается, но не институционально завершена**. [C12]
Минимальная рабочая матрица выглядит так:
| Контур | Кто обычно оказывается первым адресатом |
|--------|-----------------------------------------|
| AI-system safety / classification | AI authority / market-surveillance layer [C12] |
| personal data / lawful processing | `DPA` [C7][C12] |
| token issuance / crypto services | financial / market regulator layer [C3][C5][C12] |
| cyber incident / vulnerability | cyber authority / CSIRT channel [C10][C11][C12] |
### 8.4 Digital Omnibus и Joint Opinion 1/2026
Материалы `G11` показывают два сигнала текущего периода:
- идёт линия на упрощение и согласование AI-rulebook через `Digital Omnibus`;
- одновременно `Joint Opinion 1/2026` настаивает на сохранении accountability и участии `DPA` в чувствительных контурах. [C12]
Для N4Y.AI это означает, что "упрощение" нельзя читать как будущую амнистию от cross-regime burdens. Simplification может снять процедурный шум, но не отменит конфликтов между logging, privacy, product safety, AML и cyber-resilience. [C12]
### 8.5 Практическая схема conflict resolution
Этот документ не закрывает `G11`, но может зафиксировать minimum operating posture:
1. определить, какой use case включает high-risk / token-service / personal-data / cyber-product obligations одновременно; [C1][C3][C7][C10][C11]
2. собрать shared evidence layer, пригодный для нескольких режимов сразу; [C10][C11][C13][C18]
3. отдельно map authority chain по jurisdiction; [C10][C12]
4. unresolved conflicts не затушёвывать, а переводить в explicit legal opinion queue. [C12]
Итог секции: у среды уже есть достаточно evidence, чтобы строить regime-aware architecture, но у неё пока нет основания притворяться, что межрежимные конфликты уже гармонизированы правом или практикой. [C12]
---
## Связь с другими измерениями
| Измерение | Связь |
|-----------|-------|
| **L1/D06 (Правовое)** | `L1/D06` даёт agent-level projection: как эти режимы видит отдельный Логос, owner и delegation chain. Полный environment-level owner-ответ для `AI Act`, `MiCA`, `GDPR`, `eIDAS` и `PLD` закрепляется здесь; обратная ссылка уже проведена в `→ L1/D06 §3-§6`. |
| **L2/D01 (Онтология)** | Статус среды как `Managed Decentralized Digital Economy` и границы managed-permeability принадлежат `→ L2/D01 §3`. Этот документ добавляет поверх онтологии полный regulatory perimeter и показывает, как правовой статус проецируется в design constraints среды. |
| **L2/D02 (Инструменты)** | `AGC` и `Qi` как инструменты среды описываются полностью в `→ L2/D02 §4` и `→ L2/D02 §6`. Здесь фиксируются только их правовые последствия: `AGC = EMT`, `Qi = OCA`, `CASP` pressure и pending MiFID II boundary. |
| **L2/D03 (Инфраструктура)** | Смарт-контрактный стек и oracle layer принадлежат `→ L2/D03 §5`. В `L2/D08` smart contracts описываются только как правовой объект: reversibility, kill switch, GDPR/Data Act tension и contract-level accountability. |
| **L2/D09 (Governance)** | DAO legal wrapper, progressive decentralization и governance-chain остаются owner-слоем `→ L2/D09 §1`, `→ L2/D09 §6`. Здесь governance появляется только как объект регуляторной квалификации: legal wrapper, supervisory expectations и bounded-decentralization reading. |
---
## Открытые вопросы
### Закрыто в текущей версии
1. **[Critical] OI-003: CASP licensing path after 01.07.2026.**
> **Architectural perimeter resolved.** Three `CASP` service classes mapped in `§3.3`; final transactional authorization remains `awaiting external legal counsel for licensing transaction`. [C4][C5]
2. **[Critical] OI-007: AGC issuer / EMI path.**
> **Architectural direction resolved.** Two paths documented (`partnered EMI` / `own EMI`); final transactional licensing remains `awaiting external legal counsel for licensing transaction`. [C2][C3]
3. **[Critical] OI-008: capital adequacy for AGC issuer.**
> **Resolved.** Capital adequacy зафиксирован per `Delegated Regulation (EU) 2025/415` (`Method A` / `Method B` / `Method C`, thresholds `2%` / `3%`) — детали в `§3.1`. [C3]
4. **[High] OI-032: AGC reserve structure и liquidity discipline.**
> **Resolved.** Liquidity discipline зафиксирована per `Delegated Regulation (EU) 2025/1264` (`HQLA` composition, crypto-assets excluded from primary reserve) — детали в `§3.1`. [C4][C5]
5. **[Medium] OI-059: MiCA/MiFID II boundary for Qi.**
> **Monitoring posture resolved.** Formal `ESMA RTS` remains unpublished; explicit re-evaluation triggers зафиксированы в `§3.2`. [C8]
### Требуют решения (Critical / High)
1. **[High] OI-010: EU AI Act Art.10 data governance.**
> **Resolved.** Art.10 Data Governance baseline зафиксирован в `§2.3` (F7 cluster-AI-Act-compliance). [C13]
2. **[High] OI-011: EU AI Act Art.17 Quality Management System.**
> **Resolved.** QMS baseline зафиксирован в `§2.4` (F7 cluster-AI-Act-compliance). [C13]
3. **[High] OI-012: AI disclosure under Art.50.**
> **Resolved.** Art.50 disclosure baseline зафиксирован в `§2.6` (F7 cluster-AI-Act-compliance). [C1]
4. **[High] OI-013: GDPR DPIA.**
> **Resolved (F8 cluster-GDPR-data-protection).** Art.35 trigger matrix, tri-party joint-controller DPIA responsibility, FRIA/DPIA boundary и process cadence зафиксированы в `§4.4`. Kill-switch implementation → `OI-002` (F6 closed, `→ L3/D03 §7.3`). [C7][C12][A85]
5. **[High] OI-014: GDPR Art.22 automated decision-making rights.**
> **Resolved (F8 cluster-GDPR-data-protection).** Art.22 ADM rights design — prohibited ADM, lawful exceptions (contractual necessity Art.22(2)(b) per `[A84]`, explicit consent), safeguards, human review, contestation — зафиксированы в `§4.6`. Kill-switch execution → `OI-002` (F6, не F8 scope).
6. **[High] OI-015: cross-border GDPR and non-EEA compute.**
> **Resolved (F8 cluster-GDPR-data-protection).** Chapter V transfer baseline, SCC 2021 modules, adequacy posture (UK/Switzerland/Japan/US DPF stable; India/Russia/China routing-gate) зафиксированы в `§4.7`. [C7][C12]
### Требуют исследования (Medium)
1. **[Medium] OI-060: implementing acts around qualified electronic ledgers / eIDAS follow-on detail.**
> **Open.** Нужен мониторинг implementing acts и их влияния на wallet/attestation rails среды. [C6]
2. **[Medium] G11 supervisory coordination.**
> **Open.** Пока корректно говорить лишь о fragmented coordination и необходимости jurisdiction-specific regulator mapping. [C12]
3. **[Medium] G13 multi-agent liability chain.**
> **Open.** Следует прямо сохранять пометку: `G13` остаётся **doctrinal, zero case law note**. Нельзя превращать текущие presumptions в псевдо-устоявшуюся case law matrix. [C17][C18][C19]

