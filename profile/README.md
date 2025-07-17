# Choice Exchange

## Decentralised swaps on Injective, powered by open‑source smart contracts

> **Smart‑contract repo:** [https://github.com/choice-exchange/choice_exchange](https://github.com/choice-exchange/choice_exchange)

---

## 🚀 What is Choice?

Choice Exchange is a community‑focused DEX on the **Injective** blockchain. Under the hood we run a battle‑tested fork of the original **Terraswap** contracts, refactored for Injective’s CosmWasm 2.0 environment and upgraded security libraries.
Today we ship a lean **constant‑product AMM** while we work on concentrated‑liquidity and hybrid pool models.

### Key tweaks from the Terraswap base

* **Injective native tokens & IBC assets** – seamless `inj` gas and cross‑chain denominations
* **0.05 % protocol fee → weekly burn auction** – every trade contributes to sustainable INJ tokenomics
* Hard‑coded fee caps & panic‑on‑overflow math for extra safety

---

## 🛠️ Public Code

| Repository                        | Description                                                            |
| --------------------------------- | ---------------------------------------------------------------------- |
| [https://github.com/choice-exchange/choice_exchange](https://github.com/choice-exchange/choice_exchange) | CosmWasm contracts for pool creation, swaps, LP tokens and fee routing |

*All other infra (aggregator service, indexer and front‑end dApp) is currently closed‑source while we iterate quickly.*

---

## 📊 Fee Flow & Burn Mechanism

```
Trader swap amount
  └─▶ 0.30 % total swap fee
       ├─▶ 0.20 % to LPs (liquidity providers)
       ├─▶ 0.05 % to Choice Treasury
       └─▶ 0.05 % accumulated in Fee Collector
                 └─▶ Weekly on‑chain burn auction sub account 🔥
```

The weekly burn auction permanently removes collected fees from circulation, delivering long‑term value to all INJ holders.

---

## 🗺️ Road ahead



---

## 🌐 Links & Community

* Website: **[https://choice.exchange](https://choice.exchange)**
* X (Twitter): **[https://x.com/ChoiceXchange](https://x.com/ChoiceXchange)**
* Discord: **[https://discord.gg/BSsqGaD3ZG](https://discord.gg/BSsqGaD3ZG)**

Stay tuned for dev updates and governance discussions on Discord!

---

> © 2025 Choice Exchange
