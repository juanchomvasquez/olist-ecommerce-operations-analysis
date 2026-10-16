# Olist | E-Commerce Operations & Customer Experience Analytics

**Portfolio Project 04** · SQL / DuckDB / Python / pandas / Matplotlib  
**Sector:** E-commerce, marketplace operations, logistics, customer analytics  
**Data:** Historical anonymized Brazilian marketplace transactions, 2016–2018

## The business question

**Where is delivery reliability associated with poor customer experience, and which marketplace operations should leadership investigate first?**

This independent case study turns a multi-table transaction dataset into an order-level analytical model, examines operational risks, and translates findings into a proposed 90-day investigation and pilot plan. It does **not** imply access to Olist's internal operations or financial records.

## Executive highlights

| Finding | Observed result | Why it matters |
|---|---:|---|
| Analyzed delivered orders with usable dates | **96,470** | Denominator for delivery reliability |
| Delivered after promised date | **6.77%** | Delivery-reliability baseline |
| Reviewed late orders with low ratings (average score ≤ 2) | **62.36%** | Strong association with dissatisfaction |
| Reviewed on-time orders with low ratings | **9.23%** | Dissatisfaction also exists without delays |
| Rio de Janeiro (RJ) | **1,495 late orders / 12.11%** | High late-order volume **and** high late rate |
| São Paulo (SP) | **1,820 late orders / 4.49%** | Highest number of late orders, but below-average rate |
| Delivered-order volume, Jan–Aug 2018 vs 2017 | **+139.94%** | Commercial expansion was largely volume-driven |
| Delivered-order merchandise value, same periods | **+141.13%** | Merchandise value is **not** company revenue |
| Observed 180-day repeat-purchase rate | **3.11%** | Low observed repeat incidence in eligible cohorts |

**Operational screening:** Seller `4a3ca931...` had **168** late deliveries from **1,673** single-seller orders, versus approximately **110.5** expected given its customer-state distribution (**+57.5** excess relative to this benchmark). These are descriptive comparisons, not evidence that a seller caused the delays.

## Decision recommendation

1. Prioritize targeted delivery-performance reviews in **Rio de Janeiro (RJ)** and **Bahia (BA)**, accounting for both lateness rates and late-order volumes.
2. Investigate selected seller / route / carrier relationships with observed lateness above destination-state benchmarks before attributing responsibility.
3. Monitor **São Paulo (SP)** as the largest absolute concentration of late deliveries despite its lower rate.
4. Use a **30/60/90-day diagnose–pilot–evaluate framework**, tracking punctuality, customer reviews, freight economics and verified implementation costs.

See the **[Executive Operations Brief](Olist_Executive_Operations_Brief.pdf)** for a concise leadership-facing summary and action plan.

## Analysis and methodology

The main deliverable is the [**Google Colab notebook**](04_ecommerce_operations_analysis.ipynb). It contains saved charts, SQL queries, validation checks and the full executive memo.

| Notebook section | Analytical technique |
|---|---|
| 1. Data exploration | Coverage, order-status and missingness checks |
| 2. Order-level model | SQL CTEs; pre-aggregated items, payments and reviews; join-grain checks |
| 3. Delivery performance | Date-based lateness, mean/median/p90 delivery duration |
| 4. Customer satisfaction | Reviewed-order low-rating share by delay severity |
| 5. Geographic analysis | Destination-state late-order rate vs absolute volume |
| 6. Seller comparison | Single-seller orders and destination-state mix benchmark |
| 7. Commercial analysis | Monthly delivered-order merchandise value, matching-period YoY decomposition |
| 8. Purchasing behavior | Unique-customer order frequency and 180-day first-observed-purchase cohorts |
| 9–10. Leadership package | Dashboard, priority scorecard, limitations and 90-day management plan |

**Data-model integrity:** The principal `order_fact` model has exactly one record per `order_id` (**99,441 orders**). One-to-many item, payment and review tables are aggregated by order **before** joining, preventing multiplied rows and inflated transaction values. The seller table is introduced separately for the single-seller comparison.

## Data source and reproducibility

The notebook downloads CSVs from a specific revision of the public [Olist `work-at-olist-data` repository](https://github.com/olist/work-at-olist-data/tree/d9e49802f3e92d09ee94ab9ccc5e457f207a8959/datasets):

- `olist_orders_dataset.csv`
- `olist_order_items_dataset.csv`
- `olist_customers_dataset.csv`
- `olist_order_payments_dataset.csv`
- `olist_order_reviews_dataset.csv`
- `olist_sellers_dataset.csv`

The dataset is also described on [Kaggle: Brazilian E-Commerce Public Dataset by Olist](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce).

**To reproduce:**

1. Open `04_ecommerce_operations_analysis.ipynb` in **Google Colab**.
2. Run the cells top-to-bottom with an internet connection. The first cell installs `duckdb` and fetches the public CSVs; Colab provides pandas, numpy and Matplotlib.
3. Confirm the primary validation results: **99,441** unique order rows, **96,470** analyzed deliveries, and the saved summary KPIs.

The public-source Git commit is fixed; third-party package versions are not pinned in this notebook. The saved outputs permit reviewing the analysis without re-running it.

## Important limitations

- Estimated delivery **dates** are used as the delivery-promise benchmark; this is not a verified contractual service level.
- Regional groupings use **customer destination state**; they do not identify responsible carriers, warehouses or sellers.
- Seller comparisons use single-seller orders and only destination-state mix adjustment; they are **not** causal estimates. Their expected counts also include each seller's records in the state benchmark.
- Customer review scores may be missing; an order with multiple reviews is represented by its **average** review score.
- January–August commercial comparisons cover **purchases later classified as delivered**, grouped by **purchase month**. This is not a complete gross-merchandise-value or accounting revenue statement.
- The 180-day repeat metric reflects subsequent **observed delivered purchases** for eligible first-observed-purchase cohorts; it does not measure full customer retention or churn.
- No carrier cost ledger, returns/refunds, contribution margin or internal service targets are available; intervention ROI is **not estimated**.
- Findings are historical and should not be read as a description of Olist's current performance.

## Deliverables

- [Reproducible analysis notebook](04_ecommerce_operations_analysis.ipynb)
- [Executive Operations Brief (PDF)](Olist_Executive_Operations_Brief.pdf)

*Independent portfolio case study prepared from public, anonymized records. Not affiliated with or commissioned by Olist.*
