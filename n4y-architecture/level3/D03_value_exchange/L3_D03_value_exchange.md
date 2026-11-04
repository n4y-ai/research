# УРОВЕНЬ 3 / ИЗМЕРЕНИЕ 03: ТРАНЗАКЦИОННО-ОБМЕННОЕ

> **Уровень:** L3 — Межагентные взаимодействия  
> **Измерение:** D03 — Value Exchange  
> **Версия:** 4.0  
> **Статус:** Финал v4.0 | Дата: 04.04.2026  
> **Зависимости:** L2/D06, L2/D02, L3/D02, L2/D08, L1/D08, L2/D03, L2/D07, L3/D05, L3/D06

---

## Ключевой вопрос

**Что является предметом обмена (данные, услуги, вычислительная мощность, капитал) и как происходит передача ценности?**

---

## Аннотация

Этот hard re-run перестраивает `D03` вокруг подтверждённого ядра обмена ценностью: `ERC-8183` как базовый escrow/lifecycle стандарт, 7-фазный N4Y lifecycle поверх него, стоимость settlement как лестницу из четырёх режимов (`SimpleEscrow -> SubEscrow -> State Channels -> Merkle batch`), layered dispute stack и dual-scenario design для `EU Data Act Article 36`. Ключевое отличие от `v3.1`: документ жёстче соблюдает ownership. `D03` полно описывает эскроу и settlement flow, но проецирует `AGC/Qi`, `MiCA`, `State Channels`, `Oracle stack`, `ZKP`, `Circuit Breaker`, `Shapley Value` и auction-механику обратно к их владельцам.

Документ отвечает на вопрос "как передаётся ценность" через четыре опорных блока: что именно передаётся, как escrow и verification делают обмен условным, когда выгоден off-chain rail, и какие legal/regulatory constraints ограничивают design. Числа по газу и долларовым эквивалентам читаются как engineering guidance, а не как обещание fixed cost: evidence pack прямо фиксирует расхождения между gas-based и USD-based оценками.

---

## Содержание

1. [Таксономия предметов обмена](#1-таксономия-предметов-обмена)
2. [7-фазный платёжный lifecycle](#2-7-фазный-платёжный-lifecycle)
3. [Эскроу-паттерны и газовая экономика](#3-эскроу-паттерны-и-газовая-экономика)
4. [Settlement rails: микроплатежи и cross-sandbox](#4-settlement-rails-микроплатежи-и-cross-sandbox)
5. [Multi-party settlement и pricing rails](#5-multi-party-settlement-и-pricing-rails)
6. [Dispute resolution и anti-manipulation](#6-dispute-resolution-и-anti-manipulation)
7. [Regulatory perimeter и legal constraints](#7-regulatory-perimeter-и-legal-constraints)
8. [Verification, privacy и non-capital exchange](#8-verification-privacy-и-non-capital-exchange)

---

## 1. Таксономия предметов обмена

Нормализованный пакет не даёт готовой единой таксономии объектов обмена, поэтому `D03` вводит её как controlled synthesis поверх retained evidence. Это частично закрывает `G-01`, но не отменяет того, что граница между `data` и `knowledge transfer` остаётся зависимой от contract form. (C12, C14; E28-E33)

| Объект обмена | Что фактически передаётся | Доминирующий rail в D03 | Settlement trigger | Ownership note |
|---------------|---------------------------|--------------------------|--------------------|----------------|
| **Капитал (`AGC`)** | Право на перевод операционной стоимости | on-chain escrow, batched settlement, authorization-based rail | release/refund/dispute decision | `AGC` описывается полностью в `→ L2/D02 §5`. Здесь: `AGC` выступает settlement unit, а не объектом токеномики. |
| **Услуга** | Выполненное действие, API-call, workflow result | one-shot escrow, milestone escrow, pay-per-use rail | attestation/evaluation of completion | Сервисный lifecycle исполнения проецируется к `→ L3/D02 §2` и SLA/contract boundary `→ L3/D04 §4`. |
| **Вычислительная мощность** | Доступ к compute slot, inference job, execution artifact | prepaid tranche, milestone settlement, high-frequency metering | result proof, attestation, SLA acceptance | Инфраструктура и TEE описываются полностью в `→ L2/D03 §3-§5`. Здесь: compute рассматривается как billable exchange object. |
| **Данные** | Off-chain payload, encrypted dataset, access key, commitment | conditional escrow + off-chain delivery | proof of correct disclosure or policy-compliant access | `ZKP` и privacy stack описываются полностью в `→ L2/D07 §8`; TEE details — в `→ L2/D03 §3`. |
| **Знание / KTP** | Лицензируемое know-how, model-derived artifact, contractual right of use | hybrid: contract + escrow + off-chain transfer | contractual acceptance + provenance proof | `Knowledge Transfer` описывается полностью в `→ L3/D04 §5`. Здесь: это особый exchange object, у которого ценность определяется не только delivery, но и reuse rights. |

Практический вывод: `D03` должен оперировать не вопросом "ончейн ли объект", а вопросом "какой минимальный on-chain commitment нужен, чтобы release капитала был проверяемым". Для капитала commitment обычно совпадает с самим settlement. Для данных, услуг и вычислений commitment отделяется от полезной нагрузки. (C11, C12; E1, E28-E33, E38)

---

## 2. 7-фазный платёжный lifecycle

`ERC-8183` даёт strongest retained kernel для agentic commerce: три роли (`Client`, `Provider`, `Evaluator`) и шесть terminal-aware состояний (`Open -> Funded -> Submitted -> Completed/Rejected/Expired`). N4Y не копирует этот lifecycle буквально, а оборачивает его в 7-фазный exchange flow, добавляя negotiation до funding, `AGC`-validation перед блокировкой средств и distribution после settlement. (C1, C14; E1, E2, E9)

| Фаза | Что происходит | D03 ownership |
|------|----------------|---------------|
| **1. Negotiation** | Специфицируются deliverable, цена, timeout, dispute class, evaluator role. | `FIPA CNP` и negotiation semantics описываются полностью в `→ L3/D02 §2-§3` и `→ L2/D06 §3`. Здесь: фиксируется, что exchange contract стал достаточно определённым для funding. |
| **2. AGC Validation** | Проверяется budget envelope, `ESq >= ESc` и наличие достаточного `AGC` для lock/fund. | `Budget Controller` описывается полностью в `→ L1/D08 §2-§3`, `AGC` — в `→ L2/D02 §5`. Здесь: validation является входным условием value transfer. |
| **3. Escrow / Authorization Setup** | Создаётся escrow state или подписывается authorization rail для funding. | Это FULL-часть `D03`: именно здесь ценность переводится в условно заблокированное состояние. |
| **4. Execution** | Поставщик исполняет задачу, передаёт data/service/compute result либо открывает доступ к нему. | Исполнительный lifecycle проецируется к `→ L3/D02 §2` и contract lifecycle `→ L3/D04 §2`. Здесь важен только факт появления deliverable, привязанного к payment condition. |
| **5. Verification** | `Evaluator`, oracle, VC, zk-proof или иной verifier подтверждает result. | `Oracle stack` описывается полностью в `→ L2/D03 §5`, `ZKP` — в `→ L2/D07 §8`. Здесь verification служит gate для release. |
| **6. Settlement / Dispute** | Escrow переходит в `Completed`, `Rejected` или `Expired`; happy path release конкурирует с refund/freeze/escalation. | Это FULL-часть `D03`: именно здесь условная стоимость становится окончательной выплатой или откатом. |
| **7. Distribution** | После gross settlement выполняются platform fee, coalition split и post-settlement routing. | `Revenue Distribution` описывается полностью в `→ L2/D02 §5`; coalition fairness — в `→ L3/D05 §3-§4`. Здесь: distribution трактуется как downstream phase после settlement. |

`Phase 5` и `Phase 6` должны быть идемпотентными и replay-safe. Retained evidence поддерживает эту логику через signed authorizations, nonce-bound payment flow, evaluator attestation hashes и optimistic dispute paths. Это даёт не "абсолютную атомарность" в физическом мире, а атомарность состояния обязательства: средства не должны одновременно считаться и выплаченными, и спорными. (C2, C6, C11, C14; E1-E4, E9, E28, E29, E38)

---

## 3. Эскроу-паттерны и газовая экономика

### 3.1 ERC-8183 как escrow kernel

Для hard re-run базовым escrow pattern принимается не произвольный набор кастомных контрактов, а `ERC-8183`-совместимый kernel: `Client` фондирует обязательство, `Provider` доставляет результат, `Evaluator` подтверждает outcome, а hooks позволяют добавить репутацию, fee routing и dispute modules без переписывания ядра. Live implementation через BNBAgent усиливает этот выбор тем, что уже сочетает `ERC-8183` с `UMA Optimistic Oracle` и `ERC-8004`-composable reputation. (C1, C6; E1, E2)

### 3.2 Лестница стоимости settlement

Evidence pack поддерживает не одну "лучшую" схему, а cost ladder из четырёх режимов. Это важнее старого тезиса "существует 4 паттерна эскроу": часть режимов ниже не являются escrow в строгом смысле, но все они конкурируют как rails для передачи ценности. (C3, C4, C13; synthesis over E4, E5, E34, E35)

| Rail | Когда применять | Индикативная стоимость | Design reading |
|------|------------------|------------------------|----------------|
| **SimpleEscrow** | one-shot задача, единичная поставка, высокая понятность важнее throughput | около `11K gas` на lifecycle; в search synthesis встречается и `$0.75` upper-bound illustration | использовать как baseline для редких и понятных обменов; USD-эквивалент не считать каноническим |
| **SubEscrow / batched milestone** | несколько deliverable внутри одной миссии или пакета | около `190 gas/tx` при пакетировании | снижает marginal cost без потери escrow semantics |
| **MissionEscrow** | coalition escrow для 2–7 участников; budget pool формируется координатором; post-execution distribution по contribution algorithm | аналогично SubEscrow per slot; overhead proportional to coalition size | специализированный multi-party pattern поверх SubEscrow; правила ownership и distribution → `L3/D05 §2-§4` |
| **State Channels** | частые low-value bilateral exchanges | около `10 gas/tx` после open/close; conservative breakeven `~10 transactions` | лучший retained rail для микроплатежей, если сторонам нужен repeated exchange **[gap: L2/D03 §X pending]** |
| **Merkle batch settlement** | массовое пост-фактум закрытие большого числа мелких операций | около `15.5 gas/tx` при batch порядка `1000` платежей | подходит там, где per-item dispute probability низкая и settlement можно отложить |

`Base` как execution environment делает эти rails практичными: evidence pack удерживает ориентир порядка `~$0.005/tx` для средней L2-транзакции, а `x402`-style authorization flow показывает, что HTTP-native settlement может быть ещё дешевле для внешних USDC-like flows. Но проектное решение должно опираться на gas tier и frequency threshold, а не на один "магический" долларовый benchmark. (C2, C13; E4, E35)

---

## 4. Settlement rails: микроплатежи и cross-sandbox

`State Channels` описываются полностью по ownership map в `L2/D03`, но в текущем `L2_D03_infrastructure.md` ещё нет отдельной канонической секции для этого concept. Здесь они используются только как projection: bilateral rail для частых расчётов, у которого setup cost имеет смысл только после накопления повторяющихся exchange events. Этот ownership gap должен быть закрыт в owner artifact отдельно **[gap: L2/D03 §X pending]**. (C3, C4, C13; E5, E34, E35)

Для `D03` достаточно трёх operational rules:

1. Если exchange редкий и outcome дорогой для спора, использовать `SimpleEscrow` или `SubEscrow`.
2. Если exchange повторяющийся и low-value, переключаться на channelized metering после conservative threshold `~10 tx`.
3. Если settlement идёт между sandbox/chain boundary, `CSB` и bridge/intent rail должны оставаться coordination layer, а не логикой самого exchange object.

`CSB` описывается полностью в `→ L2/D06 §5`. Здесь: cross-sandbox payment трактуется как перенос escrow outcome между execution zones. Most mature retained path для chain-agnostic settlement остаётся production bridge/facilitator infrastructure, а не speculative universal rail. Поэтому `D03` должен проектировать cross-sandbox transfers как two-step process: локальное подтверждение обязательства + внешний settlement facilitator. (C10, C14; E6, E36)

`x402 Protocol` не заменяет `AGC`-rail внутри N4Y, но полезен как внешний reference architecture: HTTP-native micropayments, signed authorizations, facilitator model и composition с escrow/dispute modules показывают, как pay-per-use exchange может выглядеть за пределами собственного settlement token. Для N4Y это значит: keep `AGC` as internal settlement unit, but preserve adaptor path for external stablecoin-connected APIs. (C2, C14; E3, E4, E9)

---

## 5. Multi-party settlement и pricing rails

`Shapley Value` описывается полностью в `→ L3/D05 §4`. Здесь: его роль ограничивается ex-post fairness внутри coalition settlement. Retained payment evidence поддерживает split primitives и delegation-aware routing, но не даёт production-ready canonical allocator для full coalition value attribution. Поэтому для `D03` разумен hybrid rule: interim payouts считать пропорционально/по slot price, а mission-end fairness adjustment оставлять для Shapley-like reconciliation или иной off-chain verified formula. (C5; E8-E11)

| Сценарий обмена | Price discovery | Settlement rail | Комментарий |
|-----------------|-----------------|-----------------|-------------|
| **Повторяющийся сервис / data feed** | fixed or standing rate | `SimpleEscrow` или `SubEscrow` | минимизирует overhead и спорность |
| **Редкая уникальная задача** | auction-discovered price | escrow after winner selection | Аукцион описывается полностью в `→ L3/D06 §2`; здесь важен только переход от price discovery к funding |
| **Частые tool/API calls** | pay-per-use | state channel or authorization rail | экономически оправдано после накопления repeated flow |
| **Коалиционная миссия** | negotiated bundle + slot pricing | mission budget + split after settlement | coordinator fee и fairness adjustment не должны смешиваться |
| **Outcome-linked collaboration** | revenue share / contingent payout | deferred split after proof of outcome | требует strongest verification stack и highest dispute readiness |

`AGC/Qi dual-token` архитектура описывается полностью в `→ L2/D02 §5`. Здесь важна только одна граница: settlement obligations между агентами деноминируются в `AGC`, а `Qi` не должен становиться execution-time payment rail. Такая изоляция уменьшает volatility spillover в value exchange и делает regulatory story чище: операционный токен обслуживает обмен, governance/investment token остаётся вне расчётного контура. (C9; E22-E25)

`Pricing mechanisms` как рыночные процедуры не принадлежат `D03`: auction grammar, `Reverse Vickrey`, `VCG` и `Floor Price` описываются полностью в `→ L3/D06 §2` и `→ L3/D06 §4`. Здесь pricing нужен только как mapping from contract type to settlement rail. Это частично закрывает `G-11`: draft now contains a decision matrix, но не претендует на canonical market-design proof. (E26, E27)

---

## 6. Dispute resolution и anti-manipulation

Retained evidence поддерживает layered dispute stack: automated escrow logic для happy path, optimistic challenge для objective claims и внешнюю arbitration layer для дорогих или субъективных конфликтов. (C6; E2, E12-E14)

| Класс спора | Типичный trigger | Базовый resolver | Escalation logic |
|-------------|------------------|------------------|------------------|
| **D1: non-delivery / timeout** | provider не доставил outcome вовремя | expiry / time-lock refund | эскалация нужна редко; default = automatic refund |
| **D2: quality / objective mismatch** | delivered, but quality metric disputed | `Evaluator` + oracle/attestation | optimistic challenge via `UMA`; при неочевидности — external arbitration |
| **D3: settlement / accounting conflict** | разные версии объёма, этапа или суммы выплаты | signed payment trace + escrow state | objective proof first, затем juror/arbitration layer |
| **D4: fraud / subjective dispute** | манипуляция, подмена данных, спор о приемлемости результата | immediate freeze | `Kleros` для production-grade decentralised arbitration; high-value subjective path может требовать richer panel logic |

Research framing упоминает паттерн `Kleros/Aragon/UMA`, но retained normalized evidence напрямую закрепляет именно `UMA` и `Kleros`; Aragon-like governance escalation therefore остаётся здесь design analogue, а не claim of record. Для high-value subjective disputes additional panel layer может быть justified, но её нельзя считать доказанной частью baseline без отдельного retained source. (research.md G-06; E2, E13, E14)

Anti-manipulation слой в `D03` должен быть минимально достаточным, а не encyclopedic:

- `Wash trading detection`: graph-cluster analysis, SCC loops, short-window buy/sell matching и funding-graph heuristics достаточно подтверждены, чтобы считать манипуляцию реальной, а не гипотетической угрозой. (C7; E16, E17, E19)
- `Price manipulation prevention`: multi-source `TWAP`, slippage limits и circuit-breaker logic должны предшествовать settlement of price-sensitive exchanges. `TWAP` и `Circuit Breaker` описываются полностью в `→ L2/D07 §5-§7`; `Oracle stack` — в `→ L2/D03 §5`. Здесь: это gates against bad settlement inputs, а не самостоятельные market modules.
- `Reputation-linked incentives`: retained evidence показывает composability reputation with escrow/dispute, но не закрывает `G-15` как самостоятельную incentive architecture. Поэтому reputation в `D03` пока работает как modifier dispute cost and trust threshold, а не как full anti-fraud regime. (C5, C6; E1, E2, E13, E14)

---

## 7. Regulatory perimeter и legal constraints

`MiCA` описывается полностью в `→ L2/D08 §2`, но для `D03` достаточно жёсткого operational reading: после переходного периода марта 2026 `AGC`-подобный платёжный rail в Европе должен проектироваться как `EMT`-compliant, а fiat-connected payment services в ряде сценариев требуют `PSP/EMI` authorization rather than informal stablecoin usage. Следовательно, `AGC` может быть settlement unit архитектуры только вместе с лицензируемым issuance/ramp model, а не как "потом разберёмся". (C9; E22-E25)

### 7.1 EU Data Act Article 36: dual-scenario design

Для `D03` базовый legal reading теперь строится не вокруг hypothetical repeal-track, а вокруг final текста `EU Data Act`: `Article 36(1)` требует robustness against manipulation, safe termination or interruption, archival/continuity of transactional data вместе с логикой и кодом, access control и согласованность со smart contract agreement. Эти требования относятся только к smart contracts, используемым для исполнения data-sharing agreement; поэтому pure escrow flow нельзя автоматически считать covered без явного scope statement в договорной модели. [A19][A51][A56][C13][C16]

| Требование `Art.36` | Что фиксируется в `D03` |
|----------|----------------------|
| robustness against manipulation | escrow-контур должен ограничивать unilateral override и оставлять auditable trigger path вместо произвольного admin control [C13][C18] |
| safe termination / interruption | owner-layer обязан определить, кто и по какому legal basis может запускать pause/withdraw path; сама taxonomy закрепляется в `§7.3` [C13][C14][C15] |
| archival / continuity of transactional data, logic and code | после interruption или termination должны сохраняться transaction trace, актуальная версия логики и исполняемый код, достаточные для ex post review и lawful continuation / evidence preservation [C13][C19] |
| access control | trigger invocation не может быть public/open; нужен authorisation boundary и проверяемый actor context, а transport-hook остаётся узкой проекцией к `L2/D04 §3.1` [C13][C17] |
| consistency with the data-sharing agreement | kill-switch mechanics должны исполнять условия внешней договорной обёртки, а не подменять её собственной on-chain логикой [C13][C15][C16] |

Следствие для архитектуры жёсткое: если конкретный `Escrow (Smart Contract)` обслуживает data-sharing agreement, compliance burden живёт в owner-layer `D03`, а `L2_D04` получает только узкий ingress hook. Это отдельный контур по отношению к `GDPR Art. 22` и `LOA` emergency brake; их нельзя смешивать с `Kill switch` по `EU Data Act`. [C16][C17]

### 7.2 Legal qualification of escrow

`PLD` относит software к product независимо от способа доставки [C2], но retained legal corpus не подтверждает, что smart contract сам по себе становится autonomous legal contract. Напротив, текущая strongest-reading для N4Y.AI совпадает с `[A19]`: `Escrow (Smart Contract)` — это technical/economic enforcement layer или hybrid mechanism, тогда как юридическая сила обязательства зависит от внешней договорной обёртки и применимого права. `ERC-8183` под `[A56]` остаётся техническим escrow kernel, а не самостоятельным источником contract force. [A19][A56][C3][C4]

> **Допущение [A81, proposal]:** Для EU-facing escrow обязательна off-chain governing-law and forum wrapper: governing-law clause, forum / dispute venue и role allocation должны существовать вне on-chain кода, потому что `Rome I` даёт party autonomy как safest baseline, а smart-contract-specific `CJEU` case law пока отсутствует.
> Применение: on-chain escrow исполняет settlement conditions, но не замещает договор о праве, юрисдикции и remedies. [C5][C6]

Временной периметр тоже должен быть описан явно: recast `PLD` уже in force, но применяется только к products placed on the market or put into service from `2026-12-09`; то, что deployed раньше, не попадает под новый strict-liability regime автоматически. При этом immutable deployment не устраняет ответственность за defects, существовавшие в момент deploy, а defectiveness не требует code malfunction: software может работать "как написано" и всё равно не соответствовать legitimate safety expectations. [C1][C8][C9]

> **Допущение [A77, position]:** Для архитектурного baseline N4Y.AI first-line `PLD` bearer по escrow execution failure считается deployer/operator, если именно он публикует и эксплуатирует конкретный escrow instance; этот baseline отступает только там, где N4Y или иной actor сохраняет substantial modification control, commercial supply responsibility или equivalent manufacturer-like control.
> Применение: owner-layer описывает liability-default как operational posture, а не как settled doctrine; upgrade/admin control повышает риск manufacturer-style attribution per `PLD`. [C7][C12]

`PLD Art. 2(2)` выводит software из scope, если оно developed or supplied outside commercial activity, но эта граница collapse'ится при remuneration, exchange for personal data или ином эквивалентном commercial signal; bright-line rule для indirect monetisation, sponsorship или governance-token economics текущий corpus не даёт. [C10][C11]

> **Допущение [A78, position]:** N4Y reference implementation трактуется как provisionally non-commercial OSS только пока отсутствуют remuneration, exchange for personal data и иные прямые или функционально эквивалентные commercial supply signals; при появлении таких сигналов exclusion under `PLD Art. 2(2)` больше не предполагается по умолчанию.
> Применение: это консервативная product posture для owner-layer и downstream legal review, а не утверждение о settled EU doctrine. [C10][C11][C12]

> **Допущение [A80, assumption]:** До появления Commission guidance или case law on-chain deployment timestamp используется как working proxy для `placing on the market` / `putting into service` по конкретному escrow deployment.
> Применение: этот proxy нужен для temporal scoping между `EU Data Act` и `PLD`, потому что retained corpus подтверждает timeline, но не закрывает deployment-timing question окончательно. [C1][C20]

Итоговая legal posture `D03` therefore такова: escrow code может быть `PLD`-relevant product, но сам по себе не создаёт legal force договора; enforceability, applicable law и remedies живут в off-chain wrapper, а on-chain layer остаётся machine-executable enforcement and evidence trail. Именно поэтому `OI-001` может переходить из pure research в explicit approval path, но не в безусловный `resolved` без фиксации этих product positions. [A19][C4][C5][C12]

### 7.3 Kill-switch governance trigger

`EU Data Act Art. 36` задаёт capability checklist, но не перечисляет готовую trigger taxonomy. Дополнительно `Recital 104` связывает interruption/termination с mutual consent of the parties to the data-sharing agreement, а `Art. 36(2)-(3)` оставляет conformity burden на vendor-side. Поэтому в `D03` trigger logic должна жить в owner-layer и быть уже потом спроецирована как узкий hook в `Agent Gateway`; обратная конструкция, где gateway сам интерпретирует legal basis, для N4Y неприемлема. [A19][A51][A56][C14][C15][C17]

> **Допущение [A79, proposal]:** Default trigger taxonomy для covered escrow ограничивается двумя allowed classes: (1) mutual-party instruction, уже предусмотренная договорной обёрткой, и (2) competent-authority or court-ordered action. Fraud/security incident и timeout/non-performance не являются default-allowed trigger classes и требуют explicit contractual or legal approval before activation.
> Применение: `Kill switch` остаётся compliance mechanism, а не general-purpose admin override. [C14][C15]

| Паттерн | Compliance reading | Abuse / decentralization reading | N4Y fit |
|---------|--------------------|----------------------------------|---------|
| immutable escrow + external operational controls | низкая пригодность для covered `Art.36` cases, потому что native safe interruption отсутствует [C13][C18] | минимальный admin surface, но слабая legal interruptibility [C18] | допустим только там, где flow не подпадает под `Art.36` или pause не требуется |
| role-gated pause primitive | strongest direct fit для interruption requirement и auditable event trail [C13][C18][C19] | умеренный abuse risk, если роль и basis ограничены governance rules [C18][C19] | основной on-chain control candidate для `ERC-8183`-совместимого escrow под `[A56]` |
| upgradeable / admin-controlled proxy | максимальная flexibility для remediation и continuity [C18] | highest centralization and liability exposure, потому что retained control усиливает manufacturer-like reading [C7][C18] | допустим только как exception path для narrowly governed deployments |
| gateway-mediated authorised trigger + bounded on-chain pause/withdraw | `Agent Gateway` даёт auditable ingress и access-control perimeter, а owner-layer хранит legal basis и trigger authority [C17][C18] | сохраняет меньше direct admin power в contract core, чем proxy-wide upgrade [C18][C19] | preferred N4Y composition under `[A51]` + `[A56]`: gateway invokes, escrow core enforces, `D03` owns the legal trigger |

| Trigger class | Baseline verdict | Почему |
|---------------|------------------|--------|
| competent authority / court order | allowed | ближе всего к explicit external legal basis и не конфликтует с `Recital 104` reading [C14][C15] |
| mutual-party instruction under contract terms | allowed | прямо совместимо с party-autonomy и contractual wrapper logic [C5][C14][C15] |
| fraud / security incident | requires user/legal approval | закон не перечисляет такой trigger verbatim; нужен explicit contract / governance basis [C14][C15] |
| timeout / non-performance | requires user/legal approval | допустим только если terms заранее связывают non-performance с pause/termination path [C14][C15] |
| unilateral platform convenience / operator preference | not allowed | противоречит narrow compliance reading и разрушает proportionality of control [C15][C17] |

Практический governance contract therefore минимален и проверяем: trigger request должен записывать actor, legal or contractual basis, affected escrow instance, requested action (`interrupt` vs `terminate`), retained code / logic version и archival pointer, после чего исполняемый path обязан быть audit-ready и replay-auditable. Это даёт `D03` owner-level answer на `OI-002`, но сохраняет narrow hook в `L2_D04 §3.1` как projection-only companion. [C13][C17][C19]

---

## 8. Verification, privacy и non-capital exchange

`D03` не владеет `Oracle stack`, `TEE` или `ZKP`, но value exchange не может быть корректно описан без verification ladder. Нормализованное ядро здесь таково: release денег должен зависеть от cheapest sufficient proof for the object being exchanged. (C11, C12; E1, E28-E33, E38)

| Риск / объект | Предпочтительный verification rail | D03 reading |
|---------------|------------------------------------|-------------|
| **Low-risk service / frequent action** | issuer/evaluator attestation or `VC` | достаточно для дешёвых и частых обменов, где dispute cost важнее абсолютной trustlessness |
| **Medium-risk outcome** | optimistic oracle / challenge window | дешевле full oracle consensus и хорошо сочетается с escrow |
| **High-value objective claim** | oracle computation or attested callback | оправдано, если ошибка release дороже oracle overhead |
| **Compute correctness / sensitive data** | `zk-proof`, zkVM, confidential computing, or both | нужно, когда settlement зависит от результата, который нельзя раскрывать plaintext |

Для data exchange retained evidence уже поддерживает privacy-preserving patterns, в которых сам полезный объект не публикуется on-chain: продавец или провайдер предоставляет commitment, buyer/escrow проверяет proof of property or fair release condition, а plaintext остаётся в TEE/шифрованном канале. Это даёт `D03` practical answer на вопрос "как передавать ценность, если объект нельзя раскрыть". (C12; E30-E33)

Для compute exchange сильнее всего выглядит layered approach: использовать attestation/VC там, где задача routine and auditable, и резервировать `SP1`-class zk-proof или аналогичный prover для дорогих/спорных вычислений. Полная trustless verification остаётся дорогой, поэтому она не должна быть default rail for every exchange. (C11; E28, E29, E38)

Итоговый design principle: чем дешевле и чаще exchange, тем ближе verification к attestation and batching; чем дороже и более privacy-sensitive объект, тем ближе verification к oracle+zk+confidential compute stack. `D03` отвечает не "какая криптография лучшая", а "какой proof suffices to release value without разрушения economics". 

---

## Связь с другими измерениями

| Измерение | Связь |
|-----------|-------|
| **L1/D08 (Экономика)** | `Budget Controller` и `Dual Constraint` дают phase-2 gate для funding. `D03` не владеет spend authority, а использует её как precondition `→ L1/D08 §2-§3`. |
| **L2/D02 (Инструменты)** | `AGC`, `Qi`, dual-token и `Revenue Distribution` описываются полностью в `→ L2/D02 §5`. В `D03` они выступают как rails и boundaries value exchange. |
| **L2/D03 (Инфраструктура)** | `Oracle stack`, `TEE` и canonical ownership of `State Channels` лежат на инфраструктурном уровне. В текущем owner artifact отсутствует отдельная state-channel section; это pending cross-reference gap. |
| **L2/D06 (Координация)** | `CNP`, `ADS`, `CSB` и settlement engine orchestration определяют, как exchange flow запускается и переносится между sandbox boundaries `→ L2/D06 §3-§6`. |
| **L2/D07 (Безопасность)** | `TWAP`, `Circuit Breaker`, `ZKP`, anti-Sybil and KYT controls дают anti-manipulation gates для exchange `→ L2/D07 §5-§8`. |
| **L2/D08 (Регуляторное)** | `MiCA` и legal treatment of smart-contract/payment rails ограничивают допустимые формы settlement `→ L2/D08 §2` и `→ L2/D08 §5`. |
| **L3/D02 (Семантика)** | Negotiation и execution semantics превращают price quote в конкретное обязательство и deliverable `→ L3/D02 §2-§3`. |
| **L3/D05 (Кооперация)** | Coalition fairness и `Shapley Value` объясняют, как распределять ценность после совместного исполнения `→ L3/D05 §3-§4`. |
| **L3/D06 (Конкуренция)** | Auction grammar, `Floor Price` и рыночные механизмы отвечают за price discovery, после чего `D03` принимает цену как input to settlement `→ L3/D06 §2-§4`. |

---

## Открытые вопросы

### Требуют решения (Critical / High)

1. **[Critical] OI-002: финальный статус `EU Data Act Article 36` и границы kill-switch obligations для exchange contracts.**  
   > **Resolved (Phase 8):** В этом черновике принят dual-scenario design: архитектура должна быть совместима и с режимом "Art.36 в силе", и с режимом "статья удалена".  
   > **Open.** Окончательная legal narrow-down невозможна до завершения Omnibus process и привязки к конкретным contract classes.

2. **[High] OI-001: правовая квалификация smart-contract escrow.**  
   > **Open.** Техническая enforceability не эквивалентна юридической квалификации договора; нужен отдельный legal opinion, прежде чем делать stronger claims.

3. **[High] Ownership gap: canonical owner-section for `State Channels` in `L2/D03`.**   **[gap: L2/D03 §X pending]**
   > **Open.** Ownership map назначает владельцем `L2/D03`, но текущий owner artifact не содержит отдельного раздела; поэтому `D03 v4.0` использует только ограниченную проекцию без жёсткой `→ L2/D03 §X` ссылки.

### Требуют исследования (Medium)

4. **[Medium] G-01: полная канонизация таксономии объектов обмена.**  
   > **Resolved (Phase 8):** Черновик вводит operational taxonomy из пяти классов (`capital / service / compute / data / knowledge`).  
   > **Open.** Граница `data` vs `knowledge transfer` остаётся contract-dependent и требует согласования с `L3/D04`.

5. **[Medium] G-11: pricing comparison для N4Y-specific exchange.**  
   > **Resolved (Phase 8):** Добавлена decision matrix между fixed / auction-discovered / pay-per-use / revenue-share rails.  
   > **Deferred.** Каноническая market-design часть и equilibrium claims должны оставаться в `L3/D06`, а absolute tariff decomposition — не в `D03`.

6. **[Medium] G-15: reputation incentives для dispute minimization.**  
   > **Open.** Retained pack показывает composability reputation with escrow and arbitration, но не даёт standalone incentive regime, достаточного для closed claim.
