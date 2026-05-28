# Wegmans Agentic Grocery — Product Specification

## 1. Overview

**Wegmans Agentic Grocery** is a prototype demonstrating how AI agents can optimize grocery retail operations at Wegmans Food Markets. The system uses five specialized agents that analyze store-level data — inventory, demand forecasts, and operational status — to deliver actionable recommendations for store managers and merchandising teams.

This is a mock-data demo: no live Wegmans systems are connected. All data is synthetic and illustrative.

## 2. Problem Statement

Grocery retail operations generate enormous volumes of data across stores, categories, and supply chains. Store managers juggle:

- **Inventory imbalances** — overstocked perishables spoil while high-demand items go out of stock
- **Demand unpredictability** — seasonal shifts, promotions, and local events create spikes that manual processes miss
- **Merchandising inefficiency** — planogram decisions are made on gut feel rather than data-driven optimization
- **Supply chain fragility** — vendor delays and logistics disruptions aren't surfaced until they hit the shelf
- **Customer experience gaps** — shoppers can't easily find personalized recommendations or build optimized baskets

## 3. Solution: Five Specialized Agents

Each agent operates on the shared mock dataset and produces structured recommendations.

### 3.1 Store Status Agent
**Purpose:** Provide a real-time operational snapshot of any store.
- Surfaces current inventory levels, out-of-stock items, overstocked perishables
- Highlights staffing coverage and operational alerts
- Produces a summary dashboard suitable for a morning standup

### 3.2 Recommend Actions Agent
**Purpose:** Prioritize the top actions a store manager should take right now.
- Analyzes inventory, demand forecasts, and store status together
- Ranks actions by urgency and business impact (revenue at risk, waste prevention)
- Outputs a prioritized action list with rationale for each item

### 3.3 Merchandising Optimization Agent
**Purpose:** Optimize product placement and assortment for maximum revenue per square foot.
- Identifies underperforming categories and suggests planogram adjustments
- Recommends cross-merchandising opportunities (e.g., chips near salsa, beer near snacks)
- Considers seasonality and local demand patterns from the demand dataset

### 3.4 Build Basket Agent
**Purpose:** Help customers or store associates build an optimized shopping basket.
- Takes a meal plan, recipe list, or budget constraint as input
- Recommends products from current inventory, favoring in-stock and promoted items
- Suggests substitutions when preferred items are unavailable
- Calculates estimated basket cost

### 3.5 Supply Risk Agent
**Purpose:** Identify and flag supply chain risks before they cause stockouts.
- Monitors lead times, vendor reliability scores, and reorder points from inventory data
- Flags items approaching stockout based on demand velocity vs. current inventory
- Recommends emergency reorders or vendor substitutions

## 4. Data Model

All agents consume three shared mock datasets:

### store.json
Store-level operational data:
- `storeId`, `storeName`, `region`, `format` (supermarket, market café, etc.)
- `departments[]` — each with `name`, `staffCount`, `status` (open/understaffed/closed)
- `alerts[]` — operational alerts (equipment issues, safety items)
- `lastUpdated` — timestamp of the snapshot

### inventory.json
Product-level inventory across stores:
- `storeId`, `sku`, `productName`, `category`, `subcategory`
- `onHand`, `onOrder`, `reorderPoint`, `reorderQty`
- `daysOfSupply` — calculated from demand velocity
- `perishable` (boolean), `expirationDate` (if perishable)
- `vendorId`, `vendorName`, `leadTimeDays`, `vendorReliability` (0–1 score)
- `unitCost`, `unitPrice`, `currentPromotion` (null or promo details)
- `shelfLocation` — aisle/section/shelf for planogram context

### demand.json
Historical and forecasted demand:
- `storeId`, `sku`, `productName`
- `weeklyDemand[]` — last 8 weeks of actual unit sales
- `forecastNextWeek`, `forecastNext4Weeks`
- `seasonalityIndex` (1.0 = normal, >1 = seasonal peak)
- `promoLift` — expected demand multiplier if on promotion
- `trendDirection` — `rising`, `stable`, `declining`

## 5. Agent Interaction Pattern

Agents are invoked individually via Copilot prompt files. Each agent:

1. Receives the relevant mock data as context
2. Analyzes the data using its specialized logic
3. Returns structured output (summaries, ranked lists, recommendations)

Agents do not call each other directly. A human orchestrates by running the appropriate prompt.

## 6. Key Conventions

- **Store IDs** use the format `WEG-001`, `WEG-002`, etc.
- **SKUs** use the format `SKU-XXXXX` (5-digit numeric)
- **Categories** follow Wegmans department structure: Produce, Dairy, Bakery, Meat & Seafood, Grocery, Frozen, Deli, Prepared Foods, Pharmacy, Floral
- **Currency** is USD; all prices and costs are in dollars
- **Dates** use ISO 8601 format (`YYYY-MM-DD`)
- **Demand units** are individual sellable units (not cases)

## 7. Non-Goals (for this prototype)

- No live data connections to Wegmans systems
- No authentication or multi-user support
- No persistent state between agent invocations
- No agent-to-agent orchestration (single-agent invocations only)
- No UI — agents are invoked via Copilot prompt files in the IDE
