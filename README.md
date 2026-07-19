# falconbi · articles

Open, vendor-agnostic notes on enterprise planning, EPM and FP&A — written to make the choices legible to the people who sign off on them, not just the people who build them.

Published via GitHub Pages: **https://falconbi.github.io/articles/**

## Articles

### Cost Apportionment & Reciprocal Allocation
- **[Reciprocal apportionment — when cost flows both ways](https://falconbi.github.io/articles/reciprocal-apportionment.html)** — deep dive into mutual service departments, the three allocation methods (direct / step-down / reciprocal), Leontief origins, worked example, and how modern planning systems handle the circular problem.
- **[Cost apportionment, explained — executive briefing](https://falconbi.github.io/articles/cost-apportionment-executive.html)** — what each service really costs. Five-stage overview, assurance mechanisms, and why the numbers can be trusted — framed for decision-makers.
- **[Cost Apportionment in Pigment — a five-stage engine](https://falconbi.github.io/articles/cost-apportionment.html)** — full technical reference: dimension model, transaction lists, five-stage chain (including reciprocal settlements), control layer, boards, and data flow in Pigment.
- **[Activity Based Cost Apportionment in TM1](https://falconbi.github.io/tm1_apportionment/)** — full TM1 implementation: 10-cube data model, five-stage chain (TM1 rules + Python reciprocal iteration), VAL validation gate, and RC reconciliation checks.

### Financial Consolidation
- **[Financial Consolidation Hierarchy Design in TM1](https://falconbi.github.io/tm1_consolidation_design/)** — design notes covering consolidation dimension structure, ownership hierarchies, minority interest, intercompany eliminations, and the translation stage in Planning Analytics.

### Cash Flow
- **[Forecast Cash Flow & Balance Sheet — Design](https://falconbi.github.io/Cash-Flow-Direct-Method/)** — a posting-engine approach for integrated direct-method cash flow and balance sheet forecasting. Platform-agnostic design with TM1 implementation notes.

### Pigment Patterns & Hierarchy Design
- **[IF() Dimensional Scope Contamination — Case Study](https://falconbi.github.io/articles/pigment-if-scope.html)** — a 13-branch IF formula silently inflated reconciliation totals by exactly 12×. How eager evaluation, dimension alignment, and default aggregation combine to produce a silent multiplier.
- **[Ragged Hierarchies — Concepts & Patterns](https://falconbi.github.io/articles/Hierarchy%20Patterns/00_ragged_hierarchies_concepts.html)** — what ragged hierarchies are, why they break standard consolidation logic, and the three principal patterns for handling them in Pigment.
- **[Balanced Hierarchy Pattern](https://falconbi.github.io/articles/Hierarchy%20Patterns/balanced_hierarchy_pattern.html)** — design pattern for equal-depth hierarchies with predictable roll-up behaviour and clean formula paths.
- **[Lean Hierarchy Pattern](https://falconbi.github.io/articles/Hierarchy%20Patterns/lean_hierarchy_pattern.html)** — a leaner approach that preserves correct aggregation while reducing dimension maintenance cost.
- **[COA Leaf-Gate Correction](https://falconbi.github.io/articles/Hierarchy%20Patterns/COA_Leaf_Gate_Correction.html)** — isolating and correcting double-count errors at the chart-of-accounts leaf level.

### Platform Selection
- **[IBM Planning Analytics vs Pigment — Weighted Decision Scorecard](https://falconbi.github.io/articles/planning-platform-scorecard.html)** — an interactive scorecard for the AI-era planning-platform choice. Adjust the weights, or switch lens, and watch the winner change.

## About

Part of [falconbi](https://falconbi.github.io) — independent EPM/FP&A methodology, published in the open.
