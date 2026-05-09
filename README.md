# 🔄 Dormant Borrower Reactivation Campaign — Digital Lending | Emerging African Markets

## 📌 Overview

In digital lending platforms, borrower dormancy is one of the most significant threats to portfolio health and revenue sustainability. Once customers stop borrowing, conversion rates decline sharply with every passing day of inactivity — and the cost of re-engagement grows.

This project documents the end-to-end design, analysis, and pilot execution of a **data-driven Reactivation Campaign** targeting dormant borrowers across two emerging African markets, using **targeted fee waiver incentives** delivered via SMS to bring lapsed customers back into the active borrowing lifecycle.

---

## 🔍 Problem Statement

Analysis of the borrower base revealed a critical dormancy problem across both markets:

| Market | Total Dormant Base | % Not Availed Loan (60D) | Avg Drop-off Rate |
|--------|--------------------|--------------------------|-------------------|
| **Uganda** | **2M+** | **~72%** | **71.7%** |
| **Tanzania** | **1M+** | **~67%** | **65.7%** |

**Conversion rates decline sharply as dormancy deepens:**

### Uganda
| Segment | Conversion (30D) | Drop-off (60D) |
|---------|-----------------|----------------|
| 30D Dormant | 28.5% | 58.2% |
| 60D Dormant | 24.5% | 62.3% |
| 90D Dormant | 19.4% | 69.0% |
| 120D Dormant | 14.7% | 75.8% |
| 120D+ Dormant | 11.9% | 80.2% |

### Tanzania
| Segment | Conversion (30D) | Drop-off (60D) |
|---------|-----------------|----------------|
| 30D Dormant | 31.4% | 55.9% |
| 60D Dormant | 27.0% | 60.5% |
| 90D Dormant | 23.1% | 66.0% |
| 120D Dormant | 21.1% | 68.0% |
| 120D+ Dormant | 13.5% | 78.0% |

**Key Insight:** Conversion drops from ~28-31% at 30D dormancy to ~12-14% at 120D+ — meaning **every day of inactivity makes reactivation harder and more expensive.**

**Key Question:**
> *Can a targeted fee waiver offer, delivered at the right time to the right dormancy cohort, reverse the conversion decline and restore repeat borrowing behavior profitably?*

---

## 💡 Campaign Design

### Offer Structure
Eligible dormant borrowers received a **loan application fee waiver** — conditional on full repayment within the loan tenure — delivered via SMS.

| Market | Offer Condition | Repayment Window |
|--------|----------------|------------------|
| **Uganda** | Fee waiver on next loan | **15 days** |
| **Tanzania** | Fee waiver on next loan | **30 days** |

> *Different repayment windows reflect market-specific product constraints and borrower behavior profiles in each market.*

### A/B Test Design — Two Offer Variants Tested Simultaneously

| Parameter | Offer A — Long-term Play | Offer B — Conversion Play |
|-----------|--------------------------|---------------------------|
| **Discount** | 50% Fee Waiver | 100% Fee Waiver |
| **Applicable On** | Next 2 Loans | Next 1 Loan |
| **Condition** | Full repayment within tenure | Full repayment within tenure |
| **Strategic Focus** | Higher CLTV & repeat engagement | Higher initial conversion |

**Goal:** Identify the optimal offer that balances **conversion uplift** with **long-term CLTV** — not just the highest converting offer, but the most profitable one.

---

## 🔧 Methodology

### Step 1 — Dormancy Analysis & Opportunity Sizing
- Pulled borrower activity data to segment the entire base by days since last loan
- Mapped **conversion rates and drop-off rates** across dormancy cohorts (30D, 60D, 90D, 120D, 120D+)
- Quantified the revenue opportunity from reactivating each cohort
- Identified that conversion decline is **steepest between 90D and 120D+** — making early intervention critical

### Step 2 — Base Generation & Segmentation
Segmentation pipeline built using **PySpark (SQL + Python)**:

```
Full Borrower Base
    └── Filter: Has taken at least 1 loan previously (existing borrowers)
        └── Filter: No loan activity in last 30D / 60D / 90D / 120D+
            └── Filter: No active outstanding loan
                └── Segment by Dormancy Cohort
                    └── Final Target Base per Cohort
```

### Step 3 — Pilot Design & Sampling

**Pilot Structure (Per Market):**
| Group | Size | Split |
|-------|------|-------|
| **Pilot (Test) Group** | 20,000 users | 5,000 per dormancy cohort |
| **Control Group** | 20,000 users | 5,000 per dormancy cohort (same split) |

- Equal representation across all 4 dormancy cohorts in both test and control
- Within test group: **A/B split of 10,000 users per offer variant**
- Control group received the offer but no marketing outreach — establishing a clean baseline to isolate marketing-driven lift from organic conversion
- Stratified design ensures **clean incrementality measurement** across cohorts

### Step 4 — Campaign Execution
- Final segmented base handed off to the **marketing team** for SMS outreach
- Offer A and Offer B variants messaged to respective test sub-groups
- Control group received no communication

### Step 5 — Performance Monitoring & Tracking
- Tracked **reactivation rates** per cohort — % dormant users who took a new loan
- Monitored **closure rates** — whether reactivated borrowers repaid within the tenure
- Measured **incremental lift** — test conversion vs. control baseline per cohort
- Tracked **CLTV over 3 months** — to capture repeat borrowing value beyond first loan
- Monitored **ROI** — incremental revenue generated per dollar of fee waived

---

## 📊 ROI Methodology

```
ROI = Incremental Revenue (3M) / Campaign Cost

Campaign Cost     = Total application fees waived across reactivated customers
Incremental Revenue = Interest + fees collected from reactivated borrowers (3M horizon)
Incremental Lift  = Test Conversion Rate - Control (Baseline) Conversion Rate
```

**Baseline assumption:** +20% incremental lift over baseline conversion rates applied conservatively across all cohorts.

---

## 🛠️ Tools & Technologies

| Tool | Usage |
|------|-------|
| **PySpark (SQL + Python)** | Base generation, dormancy segmentation, cohort filtering |
| **Excel** | Pilot sizing, financial modelling, ROI projections |
| **Borrower Transaction Data** | Loan history, activity timestamps, repayment behavior |
| **Mobile Money Wallet Data** | Supporting activity signals for eligibility validation |

---

## 🧠 Key Learnings

1. **Dormancy is a spectrum, not a binary state** — conversion rates decline continuously with each passing day; treating all dormant users the same destroys targeting precision and ROI
2. **Earlier intervention = higher ROI** — 30D dormant cohorts show 2-3x better conversion than 120D+ cohorts; the optimal reactivation window is within the first 60 days of inactivity
3. **A/B testing offer variants is critical** — a 100% fee waiver drives higher initial conversion but a 50% waiver on 2 loans drives better CLTV; the right choice depends on portfolio strategy
4. **Cohort-stratified pilot design ensures clean measurement** — equal representation across dormancy segments prevents any single cohort from dominating results and masking poor performance elsewhere
5. **Repayment window design is market-specific** — Uganda's 15-day and Tanzania's 30-day windows reflect different product constraints and borrower behavior profiles; a one-size-fits-all approach would underperform in both markets
6. **Control groups are non-negotiable** — the control group received the offer but without any marketing outreach, enabling clean separation of **marketing-driven lift** from organic conversion; without this design, true campaign incrementality cannot be measured reliably.

---
