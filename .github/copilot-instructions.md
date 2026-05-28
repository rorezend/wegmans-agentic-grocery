# Wegmans Agentic Grocery — Copilot Instructions

## Architecture

This is a prompt-driven agent prototype with no application code. Five specialized agents analyze synthetic grocery retail data via Copilot prompt files.

**Structure:**
- `mock-data/` — Three JSON datasets (store operations, inventory, demand forecasts) shared by all agents
- `.github/prompts/` — Five agent prompt files, each self-contained with instructions and data references
- `spec.md` — Full product specification with data model schemas and agent behavior definitions

## Data Model

All agents consume from `mock-data/`. Key identifiers:
- **Store IDs:** `WEG-XXX` format (e.g., `WEG-001`)
- **SKUs:** `SKU-XXXXX` format (e.g., `SKU-10001`)
- **Vendor IDs:** `V-XXX` or `V-INHOUSE` / `V-WPROD` for Wegmans-produced items

Three stores exist: WEG-001 (Pittsford), WEG-002 (East Ave), WEG-003 (Hunt Valley). Twelve inventory items span Produce, Dairy, Bakery, Meat & Seafood, Grocery, Prepared Foods.

Key inventory fields: `onHand`, `daysOfSupply`, `reorderPoint`, `vendorReliability` (0–1 score), `perishable`, `expirationDate`.

Key demand fields: `weeklyDemand[]` (last 8 weeks), `forecastNextWeek`, `seasonalityIndex` (1.0 = normal), `trendDirection` (rising/stable/declining).

## Conventions

- Currency is USD; all prices in dollars
- Dates use ISO 8601 (`YYYY-MM-DD`)
- Demand units are individual sellable units, not cases
- Departments follow Wegmans structure: Produce, Dairy, Bakery, Meat & Seafood, Grocery, Frozen, Deli, Prepared Foods, Pharmacy, Floral
- Agents are invoked independently — no agent-to-agent orchestration
- No persistent state between agent invocations; each run reads fresh from mock-data JSON files

## Agent Output Expectations

All agents should:
- Reference specific SKUs, store IDs, and product names (not vague generalities)
- Include quantitative data (units, dollars, days of supply) in recommendations
- Prioritize actions by urgency and business impact when producing lists
- Flag perishable items approaching expiration as high-priority
