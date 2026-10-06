# Cultfit Multi-Channel Financial Reconciliation & Revenue Leakage Dashboard

## Executive Summary
High-growth multi-channel fitness operations process thousands of daily membership transactions across fragmented payment rails (UPI, Credit Cards, Debit Cards, Net Banking and Front-Desk Cash), Unmonitored payment gateway failures, untracked Merchant Discount Rates (MDR) and settlement lag float load to hidden revenue leakage.

This project delivers an end-to-end financial reconciliation engine. Built using backward-compatible **Advanced Excel** modeling and an executive **Power BI** monitoring suite, it audits 2,000 transaction events to quantify the variance between gross captured sales and net settle bank deposits.

---

## Technology Stack & Analytical Implementation
* **Microsoft Excek (Auditing & Modeling):** Built using legacy-compatible formulas (`INDEX/MATCH`, `IFERROR`, `TRIM`, `LOWER`, and nested conditionals) to eliminate `#NAME?` and array errors across older corporate Excel installations.
* **POWER BI Desktop:** Star schema data modeling, context-transition DAX measures with explicit `COALESCE` exception handling to prevent `(Blank)` metric states and interactive cross-filtering.
* **Master Rate Card Architecture:** Decouples gateway fee rules from transactional logs, ensuring modular adaptability to changing aggregator contracts.

---

## Relational Architecture & Formula Reference

### Fee Mater Contractual Benchmarks 
Gateway contracts are managed through a dedicated master rate card (`Fee_Master`):
* **Credit Cards:** 2.0% MDR + Rs.2.50 Fixed Fee ($T+3$ Settlement SLA)
* **Debit Cards:** 1.0% MDR + Rs.1.00 Fixed Fee ($T+2$ Settlement SLA)
* **Net Banking:** 1.5% MDR + Rs.3.00 Fixed Fee ($T+1$ Settlement SLA)
* **UPI / Cash:** 0.00% MDR + Rs.0.00 Fixed Fee ($T+0$ Instant Settlement)

### Formula Mechanics: Legacy vs Modern Syntax
| Operational Metric | Modern Syntax (Excel 365) | Enterprise production Formula (Legacy Compatible) |
| :--- | :--- | :--- |
| **MDR Fee Derivation** | `=ROUND(F2 * XLOOKUP(E2, Fee!A:A, Fee!B:B), 2)` | `=ROUND(F2 * IFERROR(INDEX(Fee_Master!$B$2:$B$6, MATCH(TRIM(E2), Fee_Master!$A$2:$A$6, 0)), 0), 2)` |
| **Net Realized Cash** | `=IF(G2="Settled", F2-H2-I2, 0)` | `=IF(TRIM(LOWER(G2))="settled", F2-H2-I2, 0)` |
| **Exception Triage Flag** | `=IFS(G2="Settled", "Clean", ...)` | `=IF(TRIM(LOWER(G2))="settled", "Clean: Settled", IF(TRIM(LOWER(G2))="failed", "Leakage: Gateway Drop", "At-Risk: In-Transit Float"))` |

---

## key Business Findings 
* **Quantified Revenue Leakage:** Gateway filaure drops accounted for **~13.9%** of gross checkout volume, representing immediate uncaptured revenue requiring operational recovery triage.
* **Channel Cost Impact:** Credit Cards accounted for over **~64%** of total MDR fee deductions due to higher percentage rates coupled with fixed flat-fee structures.
* **Working Capital Float:** **~16.2%** of monthly transaction volume was stalled in $T+2$ and $T+3$ banking transit, impacting short-term liquidity management.
* **Center Concentration:** **Cult Koramangala** and **Cult Indiranagar** exhibited the highest nominal leakage volume due to high concentrations of premium Cultpass ELITE subscriptions.

* ---

* ## Strategic Recommendations
* 1. **Automated Gateway Failover:** Configure POS and web checkout to initiate secondary aggregator failover whenever primary card processing latency exceeds 3 seconds.
  2. **UPI Adoption Incentivization:** Promote zero-MDR UPI payment methods during app checkouts to preserve operating margins on entry-level packs (Cultpass LIVE).
  3. **Daily Exception Triage:** Mandate daily finance review of the embedded **Audit Exception Grid** to trigger automated customer recovery notifications for failed transactions withing 24 hours.
