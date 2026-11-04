# УРОВЕНЬ 2 / ИЗМЕРЕНИЕ 05: ЭКОНОМИКО-СТИМУЛИРУЮЩЕЕ

> **Уровень:** L2 - Среда (инфраструктура)  
> **Измерение:** D05 - Economic Incentives  
> **Версия:** 4.2  
> **Статус:** Финал v4.2 | Дата: 19.04.2026  
> **Зависимости:** L2/D02, L2/D03, L2/D08, L2/D09, L1/D08, L1/D09, L3/D04, L3/D05, L3/D06

---

## Ключевой вопрос

**Какие стимулы и экономическую выгоду получает каждый участник (владелец Логоса, заказчик, платформа, другие агенты), как устроены токеномика, системы комиссий и перераспределение ценности и как обеспечивается баланс интересов между ними?**

---

## Аннотация

Этот draft отвечает на вопрос `L2/D05` как на вопрос о согласовании многосторонней экономики N4Y.AI, а не только о "доходности токена". В центре находятся карта участников, value flows, dual-token архитектура, revenue distribution, конфликт между краткосрочным удобством пользователей и долгосрочным интересом стейкеров, cold-start bootstrapping, фазовая комиссия, sanctions stack, public-goods контур и MiCA compliance. [C2][C6][C10][C13]

Ключевой тезис `v4.1` таков: устойчивые стимулы в N4Y.AI должны строиться вокруг реального использования, fee-linked value capture и ограниченных по времени bootstrap-субсидий. `D05` владеет dual-token экономикой (`AGC`, `Qi`, `veQi`), механизмом `Revenue Distribution` (`40/30/20/10`) и правовым статусом токенов по MiCA (§2, §3, §9). Governance-оболочка `Qi Chamber` и `Citizens' Chamber` остается owned layer `→ L2/D09 §1–§6`. `D05` владеет картой выгод, incentive conflicts и design rules, по которым эти owner-слои складываются в incentive-compatible экономику. [C1][C3][C7][C10][C13]

---

## Содержание

1. [Карта участников и value flows](#1-карта-участников-и-value-flows)
2. [Dual-token экономика](#2-dual-token-экономика)
3. [Revenue distribution и incentive compatibility](#3-revenue-distribution-и-incentive-compatibility)
4. [Баланс между стейкером и пользователем](#4-баланс-между-стейкером-и-пользователем)
5. [Cold start bootstrapping](#5-cold-start-bootstrapping)
6. [Incentives по типам участников](#6-incentives-по-типам-участников)
   - 6.4 [Logos owners как knowledge sellers](#64-logos-owners-как-knowledge-sellers)
7. [Dynamic commissions и slashing](#7-dynamic-commissions-и-slashing)
8. [Public goods и anti-gaming](#8-public-goods-и-anti-gaming)
9. [MiCA compliance и правовой статус токенов](#9-mica-compliance-и-правовой-статус-токенов)

---

## 1. Карта участников и value flows

### 1.1 Кто участвует в экономике N4Y.AI

`D05` отвечает не только на вопрос "кто зарабатывает", но и на вопрос "какая форма выгоды закрепляет нужное поведение". В N4Y.AI рациональный участник должен видеть не абстрактную "экосистемную ценность", а конкретный пакет выгод: немедленный доход, снижение издержек, рост доступа, долгосрочную доходность, governance-право или public-goods leverage. Именно поэтому incentive map строится как многосторонняя матрица, а не как одна кривая APY. [C5][C8][C11][C18]

| Участник | Что платит / вкладывает | Что получает | Какое поведение стимулируется |
|----------|-------------------------|--------------|-------------------------------|
| **Заказчик / владелец Логоса как buyer** | `AGC`, task demand, данные, повторный спрос | полезный результат, SLA, снижение transaction cost, loyalty benefits | возвращаться в маркетплейс, консолидировать спрос, давать повторяемый cash flow |
| **Логос-исполнитель** | время, compute, компетенции, reputation risk | `AGC`-выручку, reputation accumulation, доступ к лучшим задачам | выполнять задачи качественно и предсказуемо |
| **Logos owner как Knowledge Seller** | Experience Base, экспертиза, время на структурирование знаний | `AGC` per download / subscription / royalty, reputation в Knowledge Marketplace | вести и агрегировать знания точно и структурированно |
| **Провайдер ресурсов** | compute / storage / inference capacity, uptime commitment | оплату за verified contribution, priority routing, margin при хорошем SLA | держать capacity доступной и не нарушать SLA |
| **Qi-staker / veQi-holder** | lock `Qi`, ликвидность и время | revenue-linked yield, governance weight, long-horizon upside | поддерживать долгий горизонт и не извлекать value однократно |
| **Платформа / treasury** | разработку, risk reserve, enforcement, GTM | комиссионный поток, runway, funding for development and stability | поддерживать качество рынка, а не только volume |
| **Citizens' Chamber / public-goods layer** | governance attention, human legitimacy, review labor | grants budget, общественный мандат, funding for shared infrastructure | финансировать то, что рынок недодает напрямую |
| **Внешние агенты и внешние сети** | интеграционные затраты, переходные риски | доступ к новому спросу, cross-platform monetization, wider market reach | приносить supply и capability без полной миграции в один стек |

Эта карта intentionally шире, чем buyer-seller pair. Если оставить в модели только заказчика и исполнителя, исчезают три системно важные incentive surfaces: долгосрочный капитал, public goods и внешний supply. Тогда зрелая экономика распадается на локальные сделки без источника устойчивого роста, anti-gaming контроля и shared infrastructure funding. [C7][C11][C13][C17]

### 1.2 Value flows

Высокоуровневый поток ценности в `D05` читается так:

```text
Buyer pays AGC
  -> platform captures commission
  -> net execution payout goes to Logos / providers
  -> commission contour is split into stakers / grants / treasury / ops-stability
  -> resulting yield and grants feed long-term participation
  -> better participation improves supply quality
  -> better supply quality raises retained demand
```

Это не просто accounting. Для `D05` важно, что каждый downstream payment создает upstream incentive: buyer demand питает staker yield; staker patience финансирует liquidity commitment; public-goods contour поддерживает shared infrastructure; treasury contour дает runway для product investment; resource-side payouts удерживают supply. Если один из этих links обрывается, система начинает либо недофинансировать качество, либо переизвлекать value из пользователей. [C3][C7][C8][C13]

### 1.3 Методология incentive compatibility для N4Y

`G1` не закрывается формальным доказательством exact Nash equilibrium для N4Y split, и это должно быть сказано прямо. Retained corpus показывает не proof самой схемы `40/30/20/10`, а рабочий путь к proof: `Specification -> Modeling -> Verification -> Refinement`, supplemented by approximate-IC analysis и adjacent validator-game results. Для `D05` этого достаточно, чтобы зафиксировать methodology baseline, но недостаточно, чтобы утверждать "схема уже доказана". [C2][C16]

Рабочий interim rule для этого draft-а такой: incentive map считается приемлемой, если у каждого ключевого класса участников ожидаемая полезность честного поведения выше или не ниже полезности основного отклонения. Для buyers это повторный спрос вместо churn, для providers - корректное выполнение вместо SLA gaming, для stakers - долгий lock вместо extraction through fee spikes, для public-goods layer - проверяемый impact вместо grants capture. Это design criterion, а не завершенный theorem. [C2][C5][C10][C13]

### 1.4 Граница владения: D05, D02 и D09

> **Допущение [A3, A18, A47, A48]:** `AGC` проектируется как EMT-bound operational instrument с primary fiat/HQLA reserve path; `Qi` допускается только как supplementary stability/incentive buffer; canonical revenue contour remains `40/30/20/10`.

`D05` является единственным canonical owner для dual-token архитектуры среды: `AGC`, `Qi`, `veQi` как economic layer, `Revenue Distribution`, peg discipline и reserve boundary описываются здесь полно. Именно `D05` отвечает на вопрос, зачем эти инструменты нужны участникам и как они складываются в incentive-compatible экономику.

`D02` владеет функциональной типологией среды, маркетплейсами и mission-level design (`→ L2/D02 §1–§3`). В актуальном каноне `L2/D02` содержит только `§4 Токеномика и стимулы (проекция)`; отдельных tokenomics owner-sections `§5–§6` в `L2/D02` нет. Поэтому `D02` использует dual-token и revenue distribution только как projection (`→ L2/D02 §4`), не переписывая их mechanics.

`D09` владеет governance-механикой: `Qi Chamber`, `Citizens' Chamber`, `veQi` voting semantics, `Security Council`, `AIP`-процесс и `Progressive Decentralization` (`→ L2/D09 §1–§8`). `D05` использует governance как incentive control surface, но не переписывает его конструкцию. Эта граница важна для cross-document consistency: без нее `D05` начал бы дублировать governance design из `D09`, а `D02` — пересказывать token mechanics. [C4][C10]

---

## 2. Dual-token экономика

### 2.1 Зачем среде нужны два токена

Базовая логика dual-token architecture проста: операционная валюта не должна одновременно быть и daily payment rail, и governance/investment asset, иначе low-friction usage начинает конфликтовать с value accrual и speculative holding. [C1]

Поэтому `AGC` и `Qi Energy` разделяют роли. `AGC` — это `Operations Layer`: stable operational instrument для расчётов, pricing, escrow-facing flows и сервисных микроплатежей. `Qi Energy` — это `Value & Governance Layer`: базовый резервный, инвестиционный и governance-связанный актив экосистемы. `veQi` возникает как lock-based form `Qi`, но полная voting semantics этого механизма принадлежит `→ L2/D09 §2`; здесь важна только его экономическая функция как средства фиксации долгосрочного участия и распределения real yield. [C1][C4]

| Инструмент | Роль | MiCA | Supply | Ценообразование |
|------------|------|------|--------|-----------------|
| **AGC** (AgentCredits) | Стабильная операционная валюта (1 AGC ≈ 1 USD) | EMT (Title IV) | Эластичный: mint через fiat gateway (USD → AGC 1:1), burn через redemption (AGC → USD) | Стабильная: привязка к USD, обеспечение ≥100% fiat/гособлигациями (MiCA Art. 54) |
| **Qi Energy** | Value & governance asset | OCA (Title II) | Фиксированный: 333M токенов (предварительно), never more | Рыночная: формируется спросом на revenue share, governance power и scarcity |

### 2.2 AGC как операционный инструмент

`AGC` поддерживается как `EMT`-bound operational instrument: с par-value expectation, reserve discipline и регулируемой цепочкой выпуска/обслуживания. `AGC` проектируется не как волатильный utility token, а как расчётная единица с predictable pricing, payroll-like flows, escrow settlement и throughput-sensitive micropayments. [A3][A18]

**AGC:** единственный путь создания — fiat gateway (вносишь USD → получаешь AGC). Единственный путь уничтожения — redemption (отдаёшь AGC → получаешь USD). Staking rewards Qi-холдерам — это перераспределение существующих AGC из комиссий платформы, не mint новых.

Supplementary `Qi` stability buffer задаётся как governance-tunable precedent band, а не как primary reserve layer: рабочий диапазон размера — `5-15%` liquid buffer envelope relative to circulating `AGC` float, deploy cap — `10-20%` of buffer size за `24h`, activation band — `0.5-2%` sustained de-peg after oracle-confirmed divergence. Параметры deliberately остаются governance-tunable внутри этого precedent band и не отменяют reserve discipline как основной peg-anchor. [A3][A18][A47][A48]

Replenishment path prioritises platform stability reserve, revenue-fed refill and explicit restoration of Qi isolation after stress event; `Qi` cannot become standing primary collateral or permanent substitute for fiat/HQLA reserves. Negative lesson from comparable systems is straightforward: relaxation without preserved repurchase discipline deepens de-peg risk, so N4Y keeps reserve-first logic and supplementary-buffer semantics together. [A18][A47][A48]

### 2.3 Пег, оркестрация резервов и смерть алгоритмической рефлексивности

`Oracle` plumbing описывается полностью в `→ L2/D03 §5.4`: multi-source stack `Chainlink` + `Pyth` + median/TWAP validation, stale-feed handling, fallback order и divergence checks принадлежат owner-layer инфраструктуры. Здесь фиксируется только economic dependency: `AGC` mint/redemption brakes и supplementary `Qi` deployment могут потреблять сигналы только из этого layered stack, а не из singleton feed. [A3][A47]

Если surviving feeds расходятся materially (`>20%`) или теряется quorum/freshness, oracle layer передаёт управление в halt / peg-defense path; в `D05` это читается как reserve-first response и только затем как optional supplementary `Qi` buffer deployment. [A3][A47]

### 2.4 Qi Energy, veQi и доходность 8-12%

`Qi Energy` — базовый value & governance asset с fixed supply 333M (предварительно). Locking `Qi` в `veQi` связывает governance participation и economic patience: shorter-term liquidity sacrifice обменивается на более высокий weight в распределении доходов и, через `→ L2/D09 §2`, на вес голоса. [C1][C4]

Три фактора ценности Qi:
1. **Revenue share** — 40% дохода платформы → Qi stakers в AGC (real yield, не инфляционный)
2. **Scarcity** — fixed supply 333M + staking locks (veQi 1-4 года) уменьшают float
3. **Governance premium** — veQi даёт голос в управлении параметрами платформы

Целевой launch-range для `veQi` в 8-12% APY допустим только как `Real Yield`, а не как голая эмиссионная субсидия. Полная governance mechanics `veQi` принадлежит `→ L2/D09 §2`. [C4]

### 2.5 Burn-Mint Equilibrium: только для Qi

`Burn-Mint Equilibrium` не должен применяться к `AGC`. Для `AGC` используется peg-механика с резервной дисциплиной, redemption expectations и stabilization tools. Если используется язык burn/mint, он относится только к `Qi` как value-layer instrument. **Qi:** fixed supply, mint не предусмотрен. Burn возможен через governance vote (buyback + burn из treasury), но не является автоматическим механизмом. [C1][C6]

### 2.6 Payment flows и контроль скорости оборота AGC

Для `AGC` предпочтителен подход off-chain accumulation или channel-style execution с периодическим settlement. Если среда хочет обслуживать agentic micropayments, `AGC` нужен velocity-aware payment stack, где часть транзакций агрегируется, часть обслуживается каналами, а on-chain footprint редуцируется до периодического settlement. [C8]

### 2.7 Death Spiral prevention и изоляция рисков

`Death Spiral` — режим, в котором instability operational token и падение доверия к value layer начинают подпитывать друг друга. Предотвращение: reserve discipline, minting brakes, circuit breakers, oracle redundancy. Всё, что делает peg зависимым от непрерывного роста `Qi`, противоречит логике `EMT`-style instrument. [C1][C6][C7]

Qi и AGC **изолированы**: падение цены Qi не ломает AGC-пег, потому что AGC обеспечен fiat-резервами, а не Qi. Qi может выступать только как supplementary stability buffer, но не как primary collateral (§2.2). После stress deployment restore sequence обязан вернуть систему к reserve-first posture: сначала восстанавливаются par-value redemption и reserve coverage, затем `Qi`-экспозиция снижается до standby-buffer режима и replenishment идёт через stability contour, а не через превращение `Qi` в постоянное обеспечение. [A3][A18][A47][A48]

---

## 3. Revenue distribution и incentive compatibility

### 3.1 Базовый механизм split 40/30/20/10

> **Допущение [A48]:** Базовый contour `Revenue Distribution` сохраняется как `40/30/20/10`: veQi reward pool, Treasury/Operations, Public Goods Grants, Platform Stability Reserve.

Платформа зарабатывает на: транзакционных комиссиях, fiat gateway, premium-сервисах и корпоративных подписках. Доходы распределяются по четырём контурам, каждый из которых решает отдельную incentive task и deliberately не смешивается с соседним. [C1][C3][C6]

| Контур | Доля | Incentive effect | Failure mode if underfunded |
|--------|------|------------------|-----------------------------|
| **Long-term alignment** (veQi stakers) | **40%** | делает lock `Qi` экономически рациональным и связывает long-term capital с usage growth | `Qi` превращается в спекулятивный asset без терпеливого capital base |
| **Platform operations / treasury** | **30%** | дает ресурсы на GTM, product iteration, reserves и growth investments | платформа начинает либо стагнировать, либо доизвлекать комиссии |
| **Public goods / Citizens' Chamber** | **20%** | финансирует shared goods, которые не монетизируются каждой сделкой напрямую | инфраструктура, безопасность и ecosystem tooling недофинансируются |
| **Stability / ecosystem reserve** | **10%** | финансирует compliance, settlement, buffers, emergency maintenance | любой stress event начинает разъедать основную value pool |

Canonical contour names: "veQi reward pool" (40%), "Treasury/Operations" (30%), "Public Goods Grants" (20%), "Platform Stability Reserve" (10%). [OI-200: canonical contour names]

`Public Goods Grants` contour остаётся экономически фиксированным на уровне `20%`, но распределяется не как one-shot popularity vote, а как повторяемый grants program с quarterly cadence, отдельным proactive window для инфраструктурных потребностей и retroactive window для доказанного ecosystem impact. Такой mixed contour удерживает предсказуемость funding и одновременно избегает pure retro-only модели. [A48]

Для этого контура `D05` фиксирует экономические guardrails: auditable rounds, hybrid badge-holder / grant-committee review, anti-concentration cap на одного получателя в рамках одного раунда, conflict-of-interest disclosure и carry-over rule для неиспользованного бюджета. Governance authority, Citizens' Chamber mandate и AIP-class для изменения allocation rules принадлежат `→ L2/D09 §5`; здесь описывается только economic contour и его anti-capture logic. [A48]

В терминах mechanism design N4Y здесь может утверждать не full theorem, а explicit no-profitable-deviation argument under fixed contours [A48]. Игроки: buyer, execution-side supply, governance-aligned capital (`veQi`) и rule-bound treasury/public-goods/stability recipients. Компоненты payoff: полезность выполненной задачи net of fees, verified execution payout, repeat-demand value, long-horizon yield и устойчивость reserve/public-goods funding. Основной deviation set: попытка перетянуть больше value в один contour, краткосрочно завысить take rate, недофинансировать reserve/public-goods слой или вынести value capture в непрозрачный side-deal. При фиксированном contour `40/30/20/10` [A48] ни один из этих unilateral deviators не получает устойчивого expected gain: buyer теряет retention and matching upside, provider теряет future verified flow, staker не может законно увеличить свою долю без отдельного governance path, а treasury-side actor не может изъять reserve/public-goods share without breaking the published rule. [C1][C2][C3][C6]

Adjacent results for percentage-fee IC mechanisms and IC plus IR plus strong budget balance in expectation give N4Y only an approximation-support boundary, while the classical impossibility line associated with fixed-split budget-balance problems explains why this document cannot honestly claim a public formal theorem for an exact N-party `40/30/20/10` split. Therefore `D05` treats `40/30/20/10` as an IC-oriented N4Y baseline under [A48], not as a proven globally optimal mechanism; full closure still requires N4Y-specific payoff calibration, elasticity estimates and explicit deviation modeling. [C2][C3][C6]

> **Допущение [A88, position]:** 40/30/20/10 revenue split satisfies approximate incentive-compatibility posture under N4Y's assumption set: the mechanism is transparent, rule-based, and consistent with IC+BB+IR in expectation (Osogami et al. 2024) and percentage-fee IC models (Dobzinski et al. 2024), but a full formal proof for fixed-proportion N-party splits is not available in public mechanism-design literature. N4Y adopts best-effort IC posture pending calibrated payoff-model evidence. [A88][A48]

### 3.2 Что уже можно утверждать по механизму распределения

Из retained evidence следует более узкий и более честный тезис: N4Y уже может строить распределение так, чтобы оно было проверяемо, on-chain enforceable и не зависело от discretionary off-chain payout logic. Snapshot-based claims, pull-based redemption, burn/redemption exits и verifiable usage-linked allocation дают достаточно примитивов, чтобы распределение было rule-based. [C3]

Отсюда следует resolve-in-draft для `G1`: в `v4.1` принимается, что payment rail может быть реализован формально и прозрачно, а сама оптимальность split должна дальше проверяться отдельным modeling pass. То есть proof infrastructure есть, proof of final parameters еще нет. [C2][C3]

### 3.3 Nash-compatible baseline

Практически `D05` должен читать Nash-compatible baseline не как "никто никогда не отклонится", а как "основные отклонения становятся хуже при соблюдении правил". Для N4Y это значит следующее:

1. buyer не выигрывает от однократного использования платформы, если repeat-demand benefits и quality matching превышают switching gains; [C8][C18]
2. provider не выигрывает от систематического ухудшения SLA, если будущие выплаты и routing priority дороже краткосрочного обмана; [C5][C15]
3. staker не должен выигрывать от постоянного завышения take rate, если fee-setting authority ограничена corridor-based governance; [C10]
4. grants layer не должна выигрывать от pure popularity capture, если funding идет через anti-sybil и impact-aware routing. [C13][C14]

Это не theorem, но это корректная mechanism-design reading текущего корпуса. Более сильное утверждение требовало бы N4Y-specific payoff model, calibration of elasticities и explicit deviation set; именно эта работа остается в open questions. [C2][C8][C10]

### 3.4 Citizens' Chamber как адресат public goods

`Citizens' Chamber` полностью принадлежит `→ L2/D09 §1`; здесь важна только её инструментальная роль как адресата 20% контура. Public goods allocation не должна быть побочным режимом `Qi Chamber` — она требует отдельной legitimacy surface. Полная конструкция → `L2/D09 §1`, `→ L2/D09 §5`. [C11][C12]

### 3.5 Progressive Decentralization как управленческая проекция

`Progressive Decentralization` принадлежит `→ L2/D09 §6`. В `D05` этот концепт фиксируется только как ответ на вопрос, когда economic instruments переходят под более распределённый governance control. Полная детализация → `L2/D09 §6`. [C11][C12]

### 3.6 Связь распределения с децентрализацией

Revenue design и decentralization roadmap неразделимы. Если `40/30/20/10` существует только на бумаге без распределения полномочий — `Qi` теряет governance-смысл. Если полномочия передаются слишком рано — public goods и peg defense зависят от незрелой политической координации. [C5][C11][C12]

---

## 4. Баланс между стейкером и пользователем

### 4.1 Почему конфликт реален

Retained evidence прямо показывает, что fee-sharing creates durable tension between holders and users: первая группа хочет больший extractable cash flow, вторая - более низкие комиссии и более высокую integrity of service. Это не теоретическая придирка, а production pattern, уже наблюдавшийся в live governance и fee-sharing systems. [C10]

Для N4Y конфликт особенно чувствителен, потому что `Qi` и `veQi` обещают long-term alignment, но могут стать и механизмом давления на short-run users. Если не развести fee rights, payout rights и integrity controls, система будет вознаграждать стейкера за рост extraction even when that hurts retained demand. [C1][C10]

### 4.2 Design decision G5

`v4.1` принимает явное design decision: **`Qi Chamber` не получает неограниченного права напрямую максимизировать platform take rate.** Fee-setting делится на три слоя:

| Слой | Кто контролирует | Что можно менять |
|------|------------------|------------------|
| **Fee corridor** | constitutional / bicameral governance | верхние и нижние границы комиссий |
| **Operational tuning** | operator -> later parameter governance | текущие значения внутри corridor |
| **Staker payout share** | owner-side economics (§3) | распределение уже captured revenue |

Это решение нужно, чтобы отделить право участвовать в upside от права бесконтрольно повышать extraction. По смыслу это ближе к bounded governance, чем к прямой fee maximization by stakers. [C4][C10]

### 4.3 Staged governance вместо "fee vote every epoch"

`v4.1` также принимает staged governance path:

1. **Bootstrap phase:** комиссии и rebates задаются operator-led policy внутри заранее объявленного corridor. [C8][C10]
2. **Growth phase:** `Qi Chamber` получает право на периодическую корректировку параметров, но не на изменение constitutional bounds. [C10]
3. **Mature phase:** изменения corridor и payout-sensitive rules требуют межпалатного или конституционного маршрута `→ L2/D09 §4`, `→ L2/D09 §6`. [C4][C10]

Такой подход опирается на two-sided market logic и снижает риск, что `veQi` holders начнут оптимизировать quarter-by-quarter yield за счет долгосрочного churn. Для N4Y это важнее, чем "чистая" токен-демократия без guardrails. [C8][C10]

### 4.4 `veQi`, `OI-023` и `OI-034`

Полная governance mechanics `veQi` принадлежит `→ L2/D09 §2`. Здесь важно лишь следующее: vote-escrow alignment полезен как filter for patience, но сам по себе не решает, как выражать preference intensity и какова цена концентрации. Поэтому `OI-023` остается открытым: `veQi` time-weighting и quadratic overlay решают разные задачи и не должны смешиваться автоматически. [C1][C4]

Параллельно `OI-034` остается открытым как economic boundary: чем сильнее lock-up, тем выше вероятность снижения ликвидности `Qi`, роста концентрации и competition between staking yield and liquid utility. Для `D05` это означает простой policy rule: доходность `veQi` допустима только при контролируемом liquidity sacrifice, а не при полном вытеснении liquid `Qi` из оборота. [C1][C4]

### 4.5 Что считается успехом баланса

Balanced outcome для `D05` выглядит так: stakers видят credible yield и long-term upside, buyers - predictable effective take rate и retention benefits, platform - runway, а governance - не путает payout entitlement с правом ухудшать market fairness. Если один из этих outputs исчезает, `D05` считает конфликт неразрешенным. [C8][C10]

---

## 5. Cold start bootstrapping

### 5.1 Основной урок retained corpus

Cold start incentives работают, когда они создают supply, который потом встречает реальный спрос. Они проваливаются, когда emissions и subsidy привлекают mercenary capital without durable usage. Этот вывод повторяется и в DePIN comparators, и в more general token bootstrapping cases. [C6][C7]

Для N4Y это означает, что bootstrap incentives должны вести к спросу, а не заменять его. Экономика, которая может жить только пока льется субсидия, для `D05` считается не growth engine, а delayed failure mode. [C6][C7]

### 5.2 Трехфазный playbook G3

`v4.1` принимает трехфазный bootstrapping baseline:

| Фаза | Цель | Основной инструмент |
|------|------|---------------------|
| **Phase 1 - Build Magnet** | создать убедительный use case | корпоративные пилоты, flagship workloads, quality supply |
| **Phase 2 - User Loop** | запустить повторный спрос | retention incentives, fast matching, early loyalty benefits |
| **Phase 3 - Incentivize Behavior** | закрепить устойчивые привычки | ограниченные rewards, governance broadening, fee normalization |

Этот playbook лучше соответствует corpus, чем perpetual subsidy. Он также хорошо стыкуется с `Progressive Decentralization` как governance overlay, хотя owner-layer roadmap остается в `→ L2/D09 §8`. [C6][C7][C8]

### 5.3 N4Y baseline decision

Carry-over из [PREVIOUS VERSION] сохраняется, но переводится в более строгий режим:

1. **`100%` commission subsidy** допускается только как early launch tool для первых `1000` задач или до достижения заранее объявленного demand milestone. [C6][C8]
2. **`3-5` корпоративных пилотов** остаются допустимым GTM instrument, если они создают real recurring demand rather than PR volume. [C6][C18]
3. **Subsidies sunset by rule**, а не по усмотрению; переход в следующую фазу должен быть привязан к usage and retention signals. [C6][C7]

Это design choice важен, потому что N4Y не должен повторять pattern, где supply-side actors приходят за временной доходностью и уходят в момент нормализации take rate. [C6]

### 5.4 Helium, SubQuery и DePIN flywheel

Comparator lesson from `Helium` состоит не в том, что любую сеть можно бесконечно накачивать токеном, а в том, что usage-linked burn and real demand eventually matter more than raw node count. Lesson from `SubQuery` еще жестче: переход к zero-inflation и usage-tied rewards важен как maturity signal. [C6][C7]

Для N4Y это превращается в простой rule: launch incentives могут prepay network formation, но mature economics должны сводиться к real usage, fee-linked capture и selective sinks. Именно поэтому bootstrap не должен объясняться через постоянный emission culture. [C6][C7]

### 5.5 Что bootstrap не должен делать

Cold-start policy в `D05` не должна:

1. обещать бессрочную high APY без связи с revenue; [C6][C7]
2. субсидировать обе стороны рынка одновременно без понимания elasticity; [C8]
3. использовать broad buyer subsidy как permanent demand engine; [C18]
4. подменять product-market fit incentives-only growth model. [C6][C18]

Именно здесь `D05` жёстко связывает bootstrap с later commission normalization: early generosity допустима только как мост к экономике, которая уже может стоять на собственном usage. [C7][C8]

---

## 6. Incentives по типам участников

### 6.1 Resource providers

Resource-side markets почти всегда платят не за сам факт владения capacity, а за verified contribution: доступную мощность, соблюдённый SLA и корректно исполненный workload. Для `D05` это означает, что provider economics должна вознаграждать usable service, а не пассивный rent extraction. [C22][C23]

| Элемент | Что дает провайдеру | Зачем это системе |
|---------|---------------------|-------------------|
| **Usage-linked payout** | доход за реально отработанную capacity | не платить за пустую "готовность" |
| **Routing priority** | больше заказов при хорошем SLA | reward quality without permanent monopoly |
| **Reputation carry-over inside N4Y** | рост доверия и price power | стимулировать долгий track record |
| **Staking / bond requirement** | доступ к premium jobs и higher trust tier | создать stake-at-risk against misbehavior |

`LLM` pricing decomposition описывается полно в `→ L1/D03 §1.2`. Здесь важна только economic consequence: tier-specific model quote задаёт тот минимум payout discipline, ниже которого `Premium`, `Open` и `Specialized` supply перестают рационально обслуживать спрос. [C17][A35]

Tier 2 egress и inter-provider transfer pricing описываются полно в `→ L2/D03 §6.2-§6.4`. Здесь: hidden network cost должен учитываться как реальная provider cost component до payout optimization, иначе `Resource Router` будет поощрять низкий sticker price, который потом возвращается как opaque transfer loss under hard-constraint-first routing. [C22][A35][A54]

Полные SLA-contract semantics принадлежат `→ L3/D04 §4`, а exact resource-market mechanics остаются вне `D05`. Здесь owner-answer состоит в том, что provider economics должна вознаграждать usable service, а не static token position. [C22][C23]

### 6.2 External agents и ASI-style interoperability

External agents заходят в экосистему не ради символической "интеграции", а ради immediate monetization plus lower switching cost. Corpus показывает, что interoperability, unified access and credit-like pricing уменьшают friction для participation сильнее, чем обещание одного еще одного isolated token. [C11][C17]

Для `v4.1` принимается следующий onboarding package:

1. внешние агенты получают **доступ к спросу без полного экосистемного lock-in**; [C11]
2. imported reputation допускается только как **discovery aid**, а не как full substitute for native trust; [C12]
3. базовая monetization должна работать через **`AGC` / credit-like settlement rail**, а не через отдельные side rewards; [C11][C17]
4. для high-value participation допускаются **capability tiers и probation windows** before full routing priority. [C11][C12]

Это решение важно потому, что `35%`-style portability caveat из comparator space делает наивную репутационную совместимость опасной. N4Y должен поощрять внешнее supply, но не считать импортированную оценку равной нативному track record. [C12]

### 6.3 Logos owners как buyers

> **Допущение [A48, A49]:** Buyer-side incentives допустимы только тогда, когда они повышают quality-adjusted repeat demand быстрее, чем расходуют commission room.

Buyer-side incentives не должны строиться как indefinite mass subsidy. Устойчивый пакет для `D05` - loyalty benefits, lower future transaction cost, better matching and selective access. В steady state buyer остаётся в среде не потому, что каждая задача субсидируется, а потому, что effective experience лучше outside option. [C23][C24][C25][C26]

Для E2 buyer-side mix считается валидным только при одновременном движении трёх KPI: task completion rate `>85%`, post-task quality score `>4.0/5` и quality-adjusted repeat engagement relative to prior-phase baseline или holdout. Rebate cost и revenue-quality impact выступают veto metrics: если redemption растёт, а completion / quality-adjusted repeat не растут, incentive классифицируется как leakage, а не как retention investment. [C24][C25][C26][A48][A49]

### 6.4 Logos owners как knowledge sellers

`D05` фиксирует Knowledge Marketplace как **дополнительный** revenue stream для владельца Логоса — существующий параллельно task-execution payouts, а не вместо них. Владелец может одновременно выполнять задачи как исполнитель (`AGC`-выручка) и продавать накопленные знания как Knowledge Seller через `Knowledge License (KL)` (`→ L3/D04 §5`). Одно другому не мешает.

**Базовая модель — двусторонняя:** `Knowledge Seller ↔ Knowledge Buyer`. Покупателями выступают другие Логосы, корпоративные заказчики или, что принципиально расширяет рынок, **производители LLM-инференса** (OpenAI, Anthropic, Mistral, локальные inference-провайдеры), заинтересованные в верифицированных domain-specific данных для fine-tuning и дообучения моделей.

**Опциональный верификационный слой.** Покупатель по своему усмотрению может привлечь **институционального верификатора** — ВУЗ, министерство образования, лицензированную организацию — который аттестует качество и происхождение знаний и выдаёт `Knowledge Quality VC`, прикреплённый к KTP. Это повышает рыночную цену актива. Верификация не является обязательным условием сделки — решение остаётся за покупателем. Это не создаёт конфликт интересов: частные компании вправе обращаться за аттестацией к любым организациям, имеющим соответствующие полномочия или лицензию. Если единого режима верификации ещё не существует — его создание является частью эволюции самой экосистемы N4Y.AI.

| Участник | Что вкладывает | Что получает | Incentive |
|----------|---------------|--------------|-----------|
| **Logos owner (Knowledge Seller)** | Experience Base, экспертиза, структурирование | `AGC` per download / subscription / royalty + рост `ESq` в домене | вести знания точно, а не накапливать мусор |
| **LLM-провайдер / корпорация (Buyer)** | `AGC`-оплата | верифицированные данные с provenance для fine-tuning | снижение стоимости качественных обучающих данных |
| **Институциональный верификатор** (опц.) | экспертиза, репутация, лицензия | комиссия в `AGC` за аттестацию | монетизация доверия без физических ограничений масштаба |

**ESq как второй актив продажи знаний.** Knowledge Seller получает не только `AGC` — продажа верифицированных знаний является одним из основных способов пополнения собственного пула `ESq`. Когда институциональный верификатор выдаёт `Knowledge Quality VC`, эта аттестация служит доказательством domain-компетенции и может засчитываться в `ESq_base` наравне с Educational VC (`→ L1/D02 §5`). Это создаёт замкнутый цикл: производство качественных знаний → институциональная верификация → рост `ESq` в домене → доступ к более сложным и высокооплачиваемым задачам → накопление ещё более глубокой экспертизы.

> **Архитектурная заметка.** Механизм зачёта `Knowledge Quality VC` в `ESq_base` требует явного design-решения в `→ L1/D02 §5`: является ли такой VC полноправным источником `ESq_base` наравне с Educational VC или действует как отдельный additive-компонент.

Ключевое incentive-следствие для `D05`: Knowledge Marketplace стимулирует **качество, а не объём**. Покупатели платят за provenance и структурированность; верификаторы аттестуют только достоверные знания; следовательно, Logos-владелец заинтересован вести Experience Base точно и аккуратно — ради двойного вознаграждения: `AGC` сейчас и расширения `ESq` на перспективу. Это естественно согласуется с warranty block в KTP (`→ L3/D04 §5`) и не требует отдельного anti-gaming механизма.

Контрактная упаковка (KTP, license logic, royalty routing) принадлежит `→ L3/D04 §5`. Рыночная инфраструктура принадлежит `→ L2/D02 §3.4`. Здесь `D05` фиксирует только incentive-перспективу: зачем это экономически выгодно каждой из сторон.

### 6.5 Design decision G8

`v4.2` фиксирует явное buyer-side решение:

| Инструмент | Базовый baseline | KPI link |
|------------|------------------|----------|
| **Loyalty rebate** | `0.5%-2%` по lifetime spend tiers; финансируется из `Treasury/Operations` contour, а не из `veQi` pool [OI-201] | должен повышать quality-adjusted repeat engagement без ухудшения completion / quality |
| **Priority matching** | repeat buyers получают доступ к top-performing supply раньше | должен сокращать time-to-match и повышать completion rate |
| **Early access** | `48h` early window на новые premium capability pools | должен конвертироваться в paid repeat usage, а не в разовый opportunistic traffic |
| **Batch discount** | скидка для bundled demand `>=10` задач | должен снижать acquisition friction при сохранении positive contribution margin |

Пакет остаётся валидным только пока completion удерживается выше `85%`, средний quality score - выше `4.0/5`, quality-adjusted repeat engagement растёт, а rebate cost остаётся внутри текущего commission room. Если два review windows подряд эти условия не соблюдаются, rebate снижается на один tier либо sunset'ится до повторной валидации. [C24][C25][C26][A48][A49]

### 6.6 Почему buyer incentives не должны конфликтовать с staker yield

Buyer benefits не конфликтуют со staker yield тогда, когда работают как ROI-positive retention spend, а не как unlimited giveaway. Под `[A48]` и `[A49]` корректная проверка выглядит так: incremental net revenue от более высокой completion, лучшего quality score и repeat usage должен превышать rebate / discount cost в том же review window или в следующем phase-adjacent окне. [C24][C25][C26][A48][A49]

Если quality-adjusted repeat engagement стагнирует, completion падает ниже `85%`, средний quality score уходит ниже `4.0/5` или rebate cost продолжает расти без соответствующего uplift, buyer incentive package уменьшается до следующего lower tier раньше, чем governance вообще рассматривает расширение corridor. Именно так `D05` удерживает buyer loyalty logic совместимой с долгосрочным yield discipline. [C24][C25][C26][A48][A49]

### 6.7 Граница с L1/D08 и L3/D05

Индивидуальная финансовая устойчивость конкретного Логоса как buyer or operator принадлежит `→ L1/D08 §5–§6`; кооперативные reward structures и free-rider dynamics внутри временных команд принадлежат `→ L3/D05 §4`. Здесь `D05` использует их как downstream constraints: buyer cannot remain loyal if his own `AGC` runway is unstable, and cooperative supply cannot remain reliable if coalition economics is broken. [C5][C18]

---

## 7. Dynamic commissions и slashing

### 7.1 Почему flat fee недостаточен

Retained comparators показывают, что фиксированная комиссия "навсегда" редко является economic optimum. Разные стадии роста требуют разных take-rate trade-offs, а market regimes могут отличаться по volatility, congestion и elasticity. Поэтому `D05` принимает dynamic commissions как canonical direction, even though exact coefficients remain calibratable. [C8][C9]

### 7.2 Design decision G6

> **Допущение [A49]:** Phase-based commission schedule сохраняется как draft economic baseline, но требует benchmark- и elasticity-based calibration rather than optimality claims.

`v4.2` калибрует phase-based commission baseline against three benchmark bands: mature infrastructure distribution near `~3%`, broad gig / app marketplaces `~5-15%`, and AI / agent service layers `~6-12%`. Следствие для N4Y простое: launch phase может удерживать более высокий seller-side take rate only while curated onboarding, manual QA, routing fallback и compliance overhead ещё materially дороги; по мере роста объёма ставка должна переходить к moderate, а затем stable band, а не застывать в permanently extractive режиме. [C28][C29][C30][C31][C32][C33][A49]

| Фаза | Buyer fee | Seller / execution fee | Смысл |
|------|-----------|------------------------|-------|
| **Bootstrap (0-6 мес.)** | `0%-0.5%` | `10%-12%` | higher take-rate band допустим только пока платформа финансирует supply curation, fallback capacity и expensive launch support |
| **Growth (6-24 мес.)** | `0%-1%` | `8%-10%` | moderate band monetizes matching quality и снижает dependence on launch-era manual intervention |
| **Mature (24+ мес.)** | `0.5%-1.5%` | `6%-8%` | stable band сближает N4Y с mature service-market comparators, но остаётся выше pure cloud floor, потому что всё ещё финансирует routing, dispute/SLA plumbing и reserve discipline |

Это не universal theorem, а explicit N4Y baseline. Evidence поддерживает именно calibration protocol: benchmark bands + monitored elasticity + phase transition checks, а не утверждение, будто одна fixed schedule оптимальна для всех market regimes. [C29][C30][C31][C32][C33][A49]

### 7.3 Update formula внутри corridor

Внутри phase corridor `D05` принимает следующее bounded правило:

```text
Commission(t) = Base(phase)
  + Congestion_Adjustment
  + Volatility_Adjustment
  + Compliance_or_Verification_Premium
  - Loyalty_Rebate
```

Routine retuning допускается только в пределах `+/-20%` от baseline текущей phase и не чаще одного планового review window в квартал; baseline означает operator-set initial rate внутри опубликованного phase range на момент входа в фазу. Изменение самого constitutional envelope допускается только отдельным governance route и не должно расширять published band более чем на `+/-30%` без явного higher-order decision. Так N4Y получает numeric guardrails, а не "fee vote every epoch". [C7][C8][C9][C10][C11][C12][A49]

Emergency override существует только как bounded safety path: multisig `3/5` может временно зафиксировать комиссию на уже разрешённой ступени corridor или заморозить очередной retune при market dislocation, systemic provider outage либо observability-confirmed fee anomaly. Override замораживает full effective commission (base + active sandbox premium), идёт через `48h` timelock pattern и не может использоваться как скрытый канал перманентного повышения take rate; любое emergency изменение после стабилизации должно вернуться в ordinary governance review. [C8][C10][C11][C12][A49]

### 7.4 Slashing как экономический механизм

Slashing в `D05` читается не как набор штрафов "по вкусу", а как часть reward-penalty mechanism. Corpus поддерживает distinction between liveness faults and safety or collusion faults, а также идею, что penalty design должен учитывать severity and correlation. [C15][C16]

Следовательно, N4Y не должен иметь одну линейную таблицу "любой сбой = X%". Honest behavior становится incentive-compatible только когда reward and penalty model сконструированы совместно. [C15][C16]

### 7.5 Working offense taxonomy

`v4.1` принимает такой baseline:

| Нарушение | Тип санкции | Рабочий диапазон |
|-----------|-------------|------------------|
| **Minor liveness / SLA miss** | reward deduction | `0.1%-0.5%` |
| **Repeated availability fault** | higher deduction / small slash | `0.5%-1%` |
| **Provable omission / incorrect execution** | stake slash | `2%-5%` |
| **Collusion / fraud / coordinated manipulation** | hard slash + routing exclusion | `10%-25%` |

Числа выше фиксируются как N4Y draft baseline, а не как доказанный optimum. Их смысл - различать качество сервиса и злонамеренное поведение, а не наказывать все сбои одинаково. [C15][C16]

### 7.6 EIP-7716 as analogy, not transplant

`EIP-7716` важен для `D05` не как готовая формула для агентного рынка, а как design lesson: correlated failures должны стоить дороже isolated faults. Если десять providers падают по одной общей причине или одна коалиция coordinatedly manipulates service quality, penalty must scale with correlation, because systemic risk is higher. [C15]

Рабочая N4Y reading therefore выглядит так:

```text
Slash = Base * Severity * Correlation_Factor * Volatility_Modifier
```

Это draft heuristic, grounded in retained slashing evidence. Exact factor weights остаются отдельным modeling task. [C15][C16]

### 7.7 Связь с конкуренцией и market fairness

Комиссии и slashing связаны между собой через competition quality. Если fee policy слишком агрессивна, providers and buyers ищут обходные пути; если penalty policy слишком мягкая, рынок засоряется low-integrity supply; если слишком жесткая, new entrants не заходят. Поэтому `D05` считает commission policy и sanctions policy двумя сторонами одного incentive surface, а не разными governance queues. [C8][C9][C15]

### 7.8 Cross-sandbox pricing как bounded commission overlay

Cross-sandbox pricing в `D05` не должна превращаться в open-ended price discrimination. Базовое правило: для sandbox-классов с materially higher compliance, verification, locality или SLA cost допустим `base commission + 5-15% of base (multiplicative) premium` относительно текущего phase baseline, но только по именованным differentiators. Разрешённые differentiators: verification level, SLA / health-score class, environment-specific service cost, data-residency burden и confidentiality overhead. Всё остальное считается forbidden ad hoc repricing. [C23][C27][A49]

Этот premium остаётся governance-tunable внутри corridor, emergency override из §7.3 применяется к full effective commission (base + active premium), и он не переписывает auction design. Competitive and price-discovery semantics принадлежат `→ L3/D06 §4.4`; здесь `D05` фиксирует только policy surface: когда premium допустим, какими классами затрат он объясняется и почему он не должен использоваться для произвольной сегментации спроса. [C23][C27][A49]

## 8. Public goods и anti-gaming

### 8.1 Почему public goods принадлежат incentive layer

Public goods в N4Y - это не "благотворительность после прибыли", а часть самой incentive architecture. Общая инфраструктура, tooling, audits, education, dependency maintenance и safety commons не монетизируются в каждой отдельной сделке, но без них частный рынок деградирует. Поэтому `20%` grants contour в `D05` читается как structural incentive for ecosystem maintenance. [C13]

### 8.2 Projection boundary

`Citizens' Chamber` и полная governance-механика grants описываются owner-side в `→ L2/D09 §1` и `→ L2/D09 §5`. Здесь `D05` использует их только как economic recipient and anti-capture layer: зачем public-goods stream выделен отдельно и почему он не должен растворяться в обычной token-holder логике. [C13][C14]

### 8.3 Deep Funding, pluralism и ex-post logic

Retained evidence показывает, что grant DAO не должен полагаться на один allocator forever. Ex-post funding, dependency-aware routing, mechanism pluralism и yield-funded pools дают более устойчивую картину, чем pure token voting or one-shot quadratic-only governance. [C13]

Для N4Y это означает следующее baseline rule:

1. grants не должны распределяться только через popularity voting; [C13]
2. часть public-goods pool должна оценивать **observable impact** and ecosystem dependency; [C13]
3. один механизм не должен монополизировать allocation path. [C13]

### 8.4 Anti-sybil как экономическая защита

Anti-Sybil design здесь нужен не только ради governance purity. Weak identity filters ухудшают качество matching, эффективность subsidies и качество public-goods allocation. Поэтому `D05` прямо трактует Sybil resistance как часть economics, even though owner-side security stack lives elsewhere. [C14]

Рабочий минимальный пакет для `v4.1`:

| Layer | Роль в incentives |
|-------|-------------------|
| **Proof-of-personhood** | повышает цену массового fake participation |
| **Private / anti-bribery voting** | снижает доказуемость купленного голоса |
| **Impact review / sampling** | уменьшает reward for fake grants farming |
| **Funding mechanism pluralism** | не дает одному виду атаки захватить весь pool |

### 8.5 Free-rider prevention

`D05` считает free-riding не моральной, а строго экономической проблемой: если лучшие общие блага финансируют немногие, а выгоду получают все, рациональные участники будут недовносить средства или effort. Именно поэтому grants contour должен быть rule-based and visibly valuable for the ecosystem. [C13][C14]

На практике это значит:

1. ex-post rewards лучше привязывать к observable contribution, чем к обещаниям; [C13]
2. dependency-heavy projects deserve funding even when their user-facing visibility low; [C13]
3. для явно манипулятивных rounds должны существовать tighter apertures and challenge procedures. [C13][C14]

### 8.6 Что считается зрелым public-goods режимом

Mature regime для N4Y - это не максимальный размер grant pool, а credible ability финансировать общесредовые блага without fresh inflation and without easy capture. Yield-funded or revenue-funded grants лучше соответствуют этой цели, чем perpetual emissions. Именно здесь public goods замыкают ответ `D05`: многосторонняя экономика устойчива только тогда, когда она умеет платить не только за частный output, но и за shared substrate. [C7][C13]

---

## 9. MiCA compliance и правовой статус токенов

### 9.1 Design constraints от MiCA

Регуляторное владение `MiCA`, `EMT`, `OCA`, `CASP` и PSD2-MiCA interplay находится в `→ L2/D08 §3`. Но legal classification влияет непосредственно на design instruments: reserve discipline `AGC`, допустимые payment flows, issuance model и способ отделения operational token от governance/investment token. [C1][C2]

### 9.2 AGC как EMT-bound operational instrument

`AGC` следует проектировать как `EMT`-bound operational token: single-currency peg, par-value expectations, reserve controls и regulated issuance/service chain. `AGC` должен быть boring, redeemable, settlement-friendly and auditable. Design choice "выпустить stable operational token" — это выбор в пользу более тяжёлой legal/institutional architecture. [C1][C2][C3]

### 9.3 Qi как governance/value token

Для `Qi Energy` текущая canonical owner-проекция в `L2/D08` использует `OCA` как основной рабочий режим: governance token с fixed supply, без обязательной single-currency stabilization. Корректная формула: `AGC = EMT`; `Qi = OCA`, но требует аккуратного сопровождения на границе `ART/OCA`. `→ L2/D08 §3` [C1][C2][C3]

### 9.4 Deadline 01.07.2026 и dual authorization pressure

Production rollout `AGC` должен учитывать дедлайн `01.07.2026` и то, что custody-plus-transfer flows for EMTs подпадают под PSD2-like payment-services scope. Полное owner-описание → `L2/D08 §3`. [C2][C3]

---

## Связь с другими измерениями

| Измерение | Связь |
|-----------|-------|
| **L2/D02 (Функционально-целевое)** | Четыре маркетплейса и типология агентов определяют функциональную структуру среды, в которой работают экономические стимулы D05. `→ L2/D02 §1–§3`. |
| **L2/D03 (Инфраструктурное)** | Oracle stack и settlement infrastructure обеспечивают verifiable enforcement экономических правил: `→ L2/D03 §5.4`. |
| **L2/D08 (Нормативно-правовое)** | Регуляторная рамка MiCA для среды как платформы, PSD2-MiCA interplay и compliance infrastructure: `→ L2/D08 §2–§5`. D05 §9 описывает правовой статус токенов AGC и Qi. |
| **L2/D09 (Governance)** | `Qi Chamber`, `Citizens' Chamber`, `veQi` voting mechanics и межпалатные guardrails принадлежат governance owner-слою. `D05` использует их как incentive control surface: `→ L2/D09 §1–§2`, `→ L2/D09 §5–§6`. |
| **L1/D08 (Экономическое)** | Индивидуальная финансовая устойчивость конкретного Логоса, `Budget Controller`, KPI и `AGC = 0` behavior: `→ L1/D08 §5–§6`; `D05` использует их как agent-facing constraint. |
| **L1/D09 (Репутационное)** | Buyer retention, provider priority и external-agent onboarding завязаны на reputation quality: `→ L1/D09 §1`. |
| **L3/D04 (Контрактное)** | SLA-contract semantics и enforcement patterns для provider-side incentives: `→ L3/D04 §4`. Knowledge License (KL), Knowledge Transfer Package, royalty routing и опциональный верификационный слой: `→ L3/D04 §5`. |
| **L3/D05 (Кооперация)** | Team-level free-rider dynamics, coalition reward stack и cooperative fairness: `→ L3/D05 §4`; `D05` использует как supply-side continuation of marketplace incentives. |
| **L3/D06 (Конкурентно-рыночное)** | Auction switching, market arenas и competition policy задают market structure, внутри которого работают комиссии и take rates: `→ L3/D06 §2`. |

---

## Открытые вопросы

### Требуют решения (Critical / High)

1. **[High] OI-023: `veQi` vs `Quadratic Voting` в `Qi Chamber`.** Draft принимает `veQi` как baseline time-lock mechanism, но не фиксирует hybrid rule for preference intensity.  
> **Open.** Для `D05` это важно как ограничение на staker-user balance: payout logic и governance-expression logic нельзя смешивать автоматически. Требуются attack-cost modeling и turnout sensitivity analysis совместно с `→ L2/D09 §2`, `§6`. [C1][C4][C10]

2. **[High] OI-034: impact `veQi` lock-up на ликвидность `Qi`.** Draft допускает lock-based alignment, но не считает ликвидностный trade-off закрытым.  
> **Open.** Нужно отдельное моделирование competition between liquid `Qi`, staking yield и governance demand, чтобы `veQi` не вытеснял полезную ликвидность. Связанные owner-sections: §2.4, `→ L2/D09 §2`. [C1][C4]

3. **[High] G1: формальное доказательство incentive compatibility для N4Y split.** `v4.1` фиксирует methodology, но не theorem.  
> **Deferred.** В draft принят rule-based baseline `40/30/20/10` и formal-methods path `Specification -> Modeling -> Verification -> Refinement`, однако собственный proof для N4Y payoff matrix еще не выполнен. [C2][C3][C16]

### Требуют исследования (Medium)

4. **[Medium] G5 design decision: bounded fee governance уже выбран, но не откалиброван.** Draft принимает corridor-based fee setting with staged governance instead of direct staker maximization.  
> **Open.** Нужны точные corridor widths, retuning frequency и emergency override rules before constitutional freeze. [C8][C10]

5. **[Medium] G6 design decision: phase-based dynamic commissions уже выбраны, но не доказаны как оптимум.** Draft принимает `0-6 / 6-24 / 24+` month fee schedule and bounded update formula.  
> **Open.** Требуются elasticity estimates, churn thresholds и market-regime simulations для final calibration. [C8][C9]

6. **[Medium] G8 design decision: buyer-side incentives уже выбраны, но требуют KPI validation.** Draft принимает loyalty rebates, priority matching, early access and batch discounts вместо broad permanent subsidies.  
> **Open.** Нужно измерить, какой набор действительно повышает retention and LTV without eroding revenue quality. [C8][C18]

---

## Ownership Changelog

| Дата | Версия | Изменения |
|------|--------|-----------|
| 02.04.2026 | v4.0 | Финал v4.0. Секции §8-§10 перенесены из L2/D02 (MIG-004, MIG-005, MIG-006). Bulk rename D09→D05 (MIG-002, MIG-020). |
| 19.04.2026 | v4.1 | Структурная реорганизация: удалён дублирующий executive summary; §8 Dual-token → §2; §2+§9 Revenue Distribution merged → §3; §10 MiCA → §9. Все self-references D09→D05. Обновлены cross-references (L2/D02 §4-§6 → внутренние §2, §3, §9). Обновлена таблица «Связь с другими измерениями». |
| 19.04.2026 | v4.2 | Добавлен Knowledge Marketplace как дополнительный revenue stream: строка Knowledge Seller в §1.1; новый §6.4 «Logos owners как knowledge sellers» (двусторонняя базовая модель + опциональный институциональный верификатор + двойное вознаграждение AGC и ESq; архитектурная заметка о зачёте Knowledge Quality VC в ESq_base → L1/D02 §5); нумерация §6.4–§6.6 сдвинута на §6.5–§6.7; обновлена кросс-ссылка L3/D04 (добавлен §5). |
