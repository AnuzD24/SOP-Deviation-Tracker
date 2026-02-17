# SOP Deviation Tracker

A personal portfolio project built to practice product compliance review skills.
Tracks deviations found when cross-checking product information against Standard
Operating Procedure (SOP) rules and regulatory documentation.

## What it does

When a product is submitted for listing on an e-commerce platform, it needs to
meet specific regulatory and labelling requirements. This tracker logs cases where
a product's actual label or documentation didn't match what the SOP requires —
and records what action was taken.

For example:
- A baby toy missing a choking hazard warning (BIS SOP018)
- A food product with allergens not declared in bold (FSSAI SOP010)
- A CE-marked cookware set with no Declaration of Conformity document (SOP023)
- A cosmetic product using German INCI names on an India import batch (SOP014)

## Files

| File | Description |
|------|-------------|
| `SOP_Deviation_Tracker.xlsx` | Main Excel file — 5 sheets |
| `sop_deviation_log.csv` | 50 deviation records |
| `sop_rules.csv` | 24 SOP rules across 5 product categories |
| `sop_tracker.db` | SQLite database for running queries |

## Excel Sheets

1. **Dashboard** — KPIs: total deviations, resolved, rejected, escalated, pending
2. **Deviation Log** — Full log with expected vs actual finding, severity, status
3. **SOP Rules Reference** — 24 rules mapped to category and regulatory document
4. **High Priority Open** — 18 unresolved high-severity items
5. **SQL Queries** — 15 ready-to-run queries

## Data Structure

Each deviation record contains:
- Product name, category, country, regulatory document
- SOP ID and rule that was checked
- Expected finding (what the SOP requires)
- Actual finding (what was found on the product)
- Deviation type, severity, reviewer, date
- Status (Resolved / Rejected / Escalated / Pending Review)
- Action taken

## SOP Rules Covered

| Category | Regulatory Doc | Examples |
|----------|---------------|---------|
| Electronics | BIS, FCC, CE-Mark | MRP on outer packaging, FCC ID visible, COO label in English |
| Food & Beverages | FSSAI, FDA | Allergen in bold, date in DD/MM/YYYY, FSSAI number on front |
| Cosmetics | CDSCO, FDA | Batch number present, INCI names used, PAO symbol on label |
| Toys | BIS, CPSC, CE-Mark | Age recommendation printed, choking hazard warning, ASTM F963 |
| Home & Kitchen | BIS, CE-Mark, FDA | ISI mark on product, wattage visible, Declaration of Conformity |

## SQL Queries (sample)
```sql
-- High severity deviations not yet resolved
SELECT Log_ID, Product_Name, Deviation_Type, Status
FROM deviation_log
WHERE Severity = 'High' AND Status != 'Resolved';

-- Cross-check deviations with SOP rules
SELECT d.Product_Name, d.Deviation_Type, s.Rule_Description
FROM deviation_log d
JOIN sop_rules s ON d.SOP_ID = s.SOP_ID
WHERE d.Severity = 'High';

-- Reviewer workload summary
SELECT Reviewer_Name, COUNT(*) as Total,
  SUM(CASE WHEN Status='Resolved' THEN 1 ELSE 0 END) as Resolved
FROM deviation_log GROUP BY Reviewer_Name;
```

## Related Project

[Product Compliance Tracker] — tracks overall
compliance status of 215 Indian and international products.

## Note

Built as a portfolio project to demonstrate skills relevant to product compliance
and data operations roles. All product names and deviation scenarios are
realistic but simulated.
