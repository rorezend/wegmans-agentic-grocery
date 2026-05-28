# Wegmans Agentic Grocery

**AI agents for grocery retail operations — inventory, demand, merchandising, and supply chain.**

This prototype demonstrates how five specialized Copilot agents can analyze Wegmans store-level data and deliver actionable recommendations for store managers and merchandising teams. All data is synthetic mock data — no live systems are connected.

## Agents

| Agent | Prompt File | Purpose |
|---|---|---|
| Store Status | `agent-store-status.prompt.md` | Operational snapshot of a store |
| Recommend Actions | `agent-recommend-actions.prompt.md` | Prioritized action list for store managers |
| Merch Optimize | `agent-merch-optimize.prompt.md` | Product placement and assortment optimization |
| Build Basket | `agent-build-basket.prompt.md` | Customer shopping basket builder |
| Supply Risk | `agent-supply-risk.prompt.md` | Supply chain risk identification |

## Data

All agents operate on three shared mock datasets in `mock-data/`:

- **`store.json`** — Store-level operations (departments, staffing, alerts)
- **`inventory.json`** — Product inventory across stores (on-hand, reorder points, vendor info)
- **`demand.json`** — Historical sales and demand forecasts per product

## Usage

Open this repo in VS Code / GitHub Copilot and invoke any agent via the prompt files in `.github/prompts/`. Each prompt is self-contained — it tells the agent what data to read and what output to produce.

## Specification

See [`spec.md`](spec.md) for the full product specification including data model details and agent interaction patterns.