# 📚 Bookshop Analytics Dashboard
### A Tableau Business Intelligence Portfolio Project

> **Analyst:** TAIYESE OLAMILEKAN
> **Tool:** Tableau Desktop  
> **Dataset:** Bookshop.xlsx — Fictional bookshop operational data  
> **Scope:** Full-year sales, reader engagement, catalogue analysis, author profiling, and awards intelligence  
> **Dashboards:** 5 interactive dashboards + 1 guided Tableau Story  *WORK IN PROGRESS*

---

## Table of Contents

1. [Project Overview](#project-overview)
2. [Business Context](#business-context)
3. [Project Structure](#project-structure)
4. [Data Source Overview](#data-source-overview)
5. [Data Model](#data-model)
6. [Dashboard Overview](#dashboard-overview)
7. [Key Findings](#key-findings)
8. [Calculated Fields & Methodology](#calculated-fields--methodology)
9. [Tools Used](#tools-used)
10. [Getting Started](#getting-started)
11. [Design Principles](#design-principles)
12. [Known Limitations](#known-limitations)
13. [Future Enhancements](#future-enhancements)

---

## Project Overview

This project delivers a fully interactive Tableau business intelligence solution for a fictional bookshop operating in the year 2193. The workbook consolidates data from 12 source tables spanning sales transactions, reader ratings, library checkouts, publishing metadata, author demographics, and award records into five purpose-built dashboards and a narrated story.

The project demonstrates end-to-end BI skills including data modelling with relationships and unions, LOD expression authoring, cross-dashboard filter management, KPI card design, and executive-level data storytelling — all within a single Tableau workbook.

---

## Business Context

The bookshop stakeholders require a consolidated view of performance across four key business domains:

| Domain | Business Question |
|---|---|
| **Sales** | How did revenue and transaction volume evolve across the year? Which formats and titles drove performance? |
| **Reader Engagement** | How satisfied are readers? Which genres and titles earn the highest ratings and repeat borrowing? |
| **Catalogue & Publishing** | How is the catalogue distributed across genres, formats, and publishers? Where is the pricing power? |
| **Author Intelligence** | Where are our authors based? Does writing output correlate with reader satisfaction? |
| **Awards & Series** | Which titles carry prestige? Do award winners outperform in borrowing activity? |

The dashboards are designed for consumption by both operational managers (who need granular filters and drill-down) and senior executives (who need clear KPI headlines and a guided narrative via the Story tab).

---

## Project Structure

```
bookshop-tableau-dashboard/
│
├── README.md                        ← Project documentation (this file)
├── Bookshop_Dashboard.twbx          ← Packaged Tableau workbook (data embedded)
├── Bookshop.xlsx                    ← Source data file (12 sheets)
│
├── docs/
│   ├── data_dictionary.md           ← Field definitions for all 12 source sheets
│   ├── dashboard_guide.md           ← Worksheet specs, chart types, filter design
│   └── calculated_fields.md         ← All formulas with explanations
│
└── presentation/
    └── presentation_script.md       ← ~20-minute narrated findings script
```

> **Note:** The `.twbx` file is a Tableau Packaged Workbook. It contains the Tableau workbook and the embedded data source, so no separate database connection is required to open it.

---

## Data Source Overview

The source file `Bookshop.xlsx` contains **12 sheets** representing different operational domains of the bookshop. All connections are established in Tableau's logical layer using relationships and a manual union.

| Sheet | Row Count | Description |
|---|---|---|
| Book | 58 | Master catalogue — BookID, Title, AuthorID |
| Author | 41 | Author demographics — country, birthday, daily writing hours |
| Info | 58 | Genre, series membership, staff comments per title |
| Edition | 95 | Format, price, publication date, print run per edition |
| Publisher | 4 | Publisher details, headquarters, annual marketing spend |
| Award | 18 | Literary award wins by title and year |
| Checkouts | 668 | Monthly checkout volumes per book across 12 months |
| Ratings | 50,330 | Individual reader star ratings (1–5) with reviewer IDs |
| Series | 6 | Series metadata — planned volumes, book tour event count |
| Sales Q1 | ~14,000 | Q1 transaction records — Sale Date, ISBN, Discount, ItemID |
| Sales Q2 | ~13,000 | Q2 transaction records |
| Sales Q3 | ~22,000 | Q3 transaction records |
| Sales Q4 | ~7,000 | Q4 transaction records |

**Combined sales dataset: 56,350 rows** after unioning all four quarterly sheets.

### Key Identifiers

| Key Field | Tables Connected | Notes |
|---|---|---|
| `BookID` | Book, Info, Checkouts, Ratings, Edition | Primary book identifier |
| `ISBN` | Edition, Sales Q1–Q4 | Edition-level identifier; links price to transactions |
| `AuthID` | Book, Author | Author identifier |
| `PubID` | Edition, Publisher | Publisher identifier |
| `SeriesID` | Info, Series | Series membership link |

> ⚠️ **Data modelling note:** The Info sheet stores BookID split across two columns (`BookID1` as string, `BookID2` as integer). A join calculation `STR([BookID1]) + STR([BookID2])` is required to reconstruct the full key. The `STR()` cast on `BookID2` is critical to prevent a type mismatch that would silently break genre joins.



## Dashboard Overview

### Dashboard 1 — Sales Performance

**Audience:** Sales managers, operations leads  
**Purpose:** Full-year sales analysis — volume trends, format performance, bestsellers, and discount activity

| Worksheet | Chart Type | Primary Insight |
|---|---|---|
| Sales by Quarter | Bar chart | Q3 dominates with 22,118 transactions — nearly 3× Q1 |
| Monthly Sales Trend | Line chart | August peaks at 8,621 sales; clear summer surge pattern |
| Top 10 Best-Selling Titles | Horizontal bar | The Mallemaroking leads with 7,334 units — 58% ahead of #2 |
| Sales by Format | Treemap | Mass market paperback leads volume at 47.7% of all sales |
| Discount Split | Stacked bar | 5.9% of sales (3,314 transactions) carried a discount |

**Filters:** Genre · Format · Quarter  
**KPI Cards:** Total Sales · Gross Revenue · Avg Book Price · Total Checkouts

---

### Dashboard 2 — Reader Engagement

**Audience:** Acquisition editors, marketing team, library coordinators  
**Purpose:** Reader sentiment and borrowing behaviour — ratings, satisfaction distribution, checkout trends

| Worksheet | Chart Type | Primary Insight |
|---|---|---|
| Avg Rating by Genre | Horizontal bar | Young Adult leads at 4.24; Nonfiction lowest at 3.73 |
| Rating Distribution | Bar chart | 41.9% of all 50,330 reviews are 5-star — strongly positive skew |
| Monthly Checkout Heatmap | Heatmap | Rystwyth and The Mallemaroking dominate year-round |
| Top 10 Most Checked-Out | Horizontal bar | Rystwyth leads checkouts at 1,332 — top of both sales and borrowing |
| Checkouts vs Avg Rating | Scatter plot | Positive correlation — high checkout titles also rate highest |

**Filters:** Genre · Rating slider (1–5)  
**KPI Cards:** Total Reviews · Average Rating · 5-Star Share · Total Checkouts · Total Sales · Gross Revenue · Avg Book Price

---

### Dashboard 3 — Catalogue & Publishing

**Audience:** Buyers, acquisition team, publisher relations  
**Purpose:** Catalogue shape, pricing intelligence, publisher market share and investment analysis

| Worksheet | Chart Type | Primary Insight |
|---|---|---|
| Catalogue by Genre | Donut chart | Fiction (26%) and SciFi/Fantasy (19%) dominate the catalogue |
| Editions by Format | Bar chart | Hardcover has the most editions (36) but lowest sales share |
| Price Distribution by Format | Box plot | Hardcovers range $24–$48; board books cluster at $5–$12 |
| Publisher Market Share | Bar chart | Etaoin Shrdlu Press publishes the most editions |
| Publisher Marketing Spend | Bar chart | ESP spends $2.32M — 15× more than the next highest publisher |
| Print Run Size by Genre | Bar chart | SciFi/Fantasy and Fiction command the largest print runs |

**Filters:** Publisher · Format · Publication Year

---

### Dashboard 4 — Author Insights

**Audience:** Acquisition editors, diversity and inclusion leads, talent scouts  
**Purpose:** Author geographic spread, productivity analysis, and output profiling

| Worksheet | Chart Type | Primary Insight |
|---|---|---|
| Authors by Country | Filled map | US (9) and New Zealand (7) supply the largest author cohorts |
| Writing Hours Distribution | Histogram | Most authors write 4–12 hours daily |
| Writing Hours vs Books Published | Scatter plot | Weak positive correlation — career length is a stronger output predictor |
| Books per Author (Top 10) | Horizontal bar | Most prolific authors have 2–3 titles in the catalogue |
| Author Age Distribution | Histogram | Broad age spread — no dominant career stage |

**Filters:** Country of Residence · Genre

---

### Dashboard 5 — Awards & Series

**Audience:** Marketing team, PR, series acquisition managers  
**Purpose:** Prestige and serialisation intelligence — award impact on borrowing, series completion status

| Worksheet | Chart Type | Primary Insight |
|---|---|---|
| Awards by Title | Horizontal bar | Multiple titles carry more than one award — strong prestige concentration |
| Awards by Year | Line chart | Award wins distributed across multiple years |
| Award Type Breakdown | Bar chart | Hugo Award is the most frequently won award in the catalogue |
| Series Completion Status | Bar chart | Planned vs published volumes gap visible across all 6 series |
| Book Tour Events by Series | Bar chart | The Mallemaroking Saga leads tour activity at 32 events |
| Checkout Uplift: Award vs Non-Award | Grouped bar | Award winners show measurably higher average monthly checkouts |

**Filters:** Award Name · Series

---

### Story — Bookshop Annual Report 2193

A 10-point guided narrative built in Tableau Story format, designed for executive presentation. Each story point includes annotated commentary and is narrated in full in `presentation/presentation_script.md`.

| Point | Title | Key Message |
|---|---|---|
| 1 | Setting the Scene | Introduce the bookshop and data scope |
| 2 | A Record-Breaking Year | Q3 drove nearly half of all annual sales |
| 3 | What Sold and How | Mass market paperback dominates; The Mallemaroking is the standout title |
| 4 | Readers Speak | 41.9% five-star reviews — overwhelmingly positive sentiment |
| 5 | Checkout Patterns | Summer peaks mirror sales; Rystwyth and Mallemaroking dominate borrowing |
| 6 | Inside the Catalogue | Fiction and SciFi/Fantasy anchor the catalogue |
| 7 | Publisher Power | Etaoin Shrdlu Press outspends competitors by a factor of 15 |
| 8 | Meet the Authors | International spread across 12 countries; productive but diverse writing habits |
| 9 | Awards & Recognition | Hugo Award dominates; award winners show measurable checkout uplift |
| 10 | Key Takeaways | Four strategic recommendations for the year ahead |

---

## Key Findings

### 1. Sales are heavily seasonal — Q3 is the growth engine
Q3 recorded 22,118 transactions, accounting for **39.3% of the full year's volume**. August alone peaked at 8,621 sales. Q1 was the weakest quarter at 7,785 transactions. This seasonal pattern suggests promotional activity or organic summer demand that the business should plan inventory and staffing around.

| Quarter | Transactions | Share of Annual Total |
|---|---|---|
| Q1 | 7,785 | 13.8% |
| Q2 | 13,354 | 23.7% |
| Q3 | 22,118 | 39.3% |
| Q4 | 13,093 | 23.2% |
| **Total** | **56,350** | **100%** |

---

### 2. Mass market paperback drives volume; hardcover drives catalogue depth
Despite hardcover having the most editions (36 of 95), mass market paperback accounts for **47.7% of all sales transactions** — nearly double hardcover's share. This suggests a price-sensitive customer base where accessibility drives volume. Hardcover likely contributes disproportionate revenue per unit but requires deeper margin analysis.

---

### 3. Reader satisfaction is exceptionally high across the catalogue
With an average rating of **4.12 out of 5** across 50,330 reviews, reader sentiment is strongly positive. Five-star reviews alone account for 41.9% of all ratings. Young Adult leads genre satisfaction at 4.24 while Nonfiction trails at 3.73 — a 13% satisfaction gap that warrants acquisition strategy review.

---

### 4. Checkout activity and ratings are positively correlated
Titles with the highest checkout volumes also consistently earn the highest average ratings. Rystwyth (4.46 avg rating, 1,332 checkouts) and The Mallemaroking (4.66 avg rating, 1,243 checkouts) sit in the top-right quadrant of the scatter plot — confirming that quality and popularity reinforce each other. These titles represent the highest-value assets in the catalogue.

---

### 5. Etaoin Shrdlu Press has an outsized marketing investment
ESP's annual marketing spend of **$2,320,000** dwarfs all other publishers combined (Cedar House $72K · Sound & Seas $151K · Palimpsest Printing $48K). Whether this investment produces proportional sales lift is the single most important unanswered question for a future revenue-by-publisher analysis.

---

### 6. Award winners outperform in checkout frequency
Award-winning titles show measurably higher average monthly checkout rates compared to non-awarded titles. The Hugo Award is the most commonly held award in the catalogue. This data supports prioritising award-nominated titles in acquisition, stocking, and promotional strategies.

---

### 7. The catalogue is internationally authored but US and NZ concentrated
41 authors span 12 countries, but the United States (9 authors) and New Zealand (7 authors) supply 39% of the authorship base. Regions including Southeast Asia, South America, and continental Europe are underrepresented relative to their global publishing output — a potential diversity gap in acquisition strategy.

---

### 8. Discounting is minimal and targeted
Only **5.9% of transactions (3,314 sales)** carried a discount. Discount rates used were 5%, 10%, 13%, 15%, and 20%. The concentration of discounting in specific titles and quarters suggests deliberate promotional activity rather than broad markdown — a healthy pricing discipline signal.

---



### Revenue LOD Rationale

The Revenue metric required a Level of Detail expression rather than a simple `SUM([Price])` because of how Tableau's relationship model activates tables. `Price` lives in the Edition table. Without forcing Sales into scope via the LOD, Tableau computes the sum at the Edition grain (95 rows × average price ≈ $1,597) rather than the transaction grain (56,350 rows × respective prices = $699,885). The `FIXED` LOD anchors the calculation to ISBN and explicitly brings `COUNT([Item ID])` from Sales into the same expression, resolving the cross-table grain mismatch.

---

## Tools Used

| Tool | Purpose |
|---|---|
| **Tableau Desktop 2024.x** | Primary dashboard development, data modelling, story authoring |
| **Microsoft Excel** | Source data format (Bookshop.xlsx) |
| **GitHub** | Version control and portfolio hosting |
| **Markdown** | Documentation authoring |

---

## Getting Started

### Prerequisites
- Tableau Desktop (2022.1 or later recommended)
- No database connection required — data is embedded in the `.twbx` file

### Opening the workbook
1. Clone or download this repository
2. Open `Bookshop_Dashboard.twbx` in Tableau Desktop
3. All data is pre-embedded — no re-pointing of data source required
4. Navigate dashboards using the tabs at the bottom of the screen
5. Open the **Story** tab for the guided executive presentation

### Exploring the documentation
| If you want to... | Go to... |
|---|---|
| Understand every data field | `docs/data_dictionary.md` |
| See how each dashboard was built | `docs/dashboard_guide.md` |
| Review all calculated field formulas | `docs/calculated_fields.md` |
| Read the full presentation narrative | `presentation/presentation_script.md` |

### Interacting with the dashboards
- **Click any bar, tile, or data point** to filter all other views on that dashboard
- **Use the Genre dropdown** to filter all views by genre simultaneously
- **Use the Rating slider** on Dashboard 2 to filter by star rating range
- **Hover over any mark** for a customised tooltip with full detail
- **Click the Story tab** for a guided 10-point narrative walkthrough

---

## Design Principles

This workbook was built following industry-standard BI dashboard design principles appropriate for executive-level consumption:

**Clarity over decoration** — every chart earns its place by answering a specific business question. No decorative elements that do not carry analytical meaning.

**KPI cards first** — each dashboard leads with a strip of headline metrics so executives can assess performance at a glance before exploring detail.

**Consistent colour encoding** — Genre is encoded with the same colour palette across all dashboards so viewers build intuition once and apply it everywhere.

**Filter scope discipline** — all filters are scoped to Selected Worksheets to prevent cross-dashboard contamination. Each dashboard's filters affect only its own views.

**Annotated outliers** — significant data points (August peak, The Mallemaroking dominance, ESP marketing spend) are annotated directly on the chart so the insight is visible without requiring the viewer to search for it.

**Verified against source** — all KPI card values are verified against direct Python aggregations of the source Excel file before publication. Expected values are documented in `docs/dashboard_guide.md` for QA purposes.

**Accessible axis ranges** — rating charts start at 3.5 rather than 0 to make genre differences visible. Reference lines show overall averages so individual values have meaningful context.

---

## Known Limitations

**Fictional dataset** — all data is fictional and set in the year 2193. Patterns and findings are illustrative and not representative of real bookshop economics.

**No real-time connection** — the workbook uses a static extract of the Excel source. Any updates to the source data require re-publishing the `.twbx` file.

**Award table joins on Title string** — the Award sheet has no BookID and joins to Book via a string title match. Any title formatting inconsistency in the source data would cause award rows to silently drop from the join.

**Revenue is gross, not net** — the Revenue metric reflects full retail price multiplied by unit count. It does not account for discounts applied (5.9% of transactions), returns, or publisher margins.

**Multi-genre filter label** — the dynamic KPI subtitle shows the genre name when one genre is selected, and the count (e.g. "3 Genres Selected") when multiple are chosen. It cannot display multiple genre names simultaneously due to Tableau's lack of a native string aggregation function.

---

## Future Enhancements

Given additional data or development time, the following analyses would add material value:

- **Net revenue analysis** — apply discount rates to Revenue for a net figure; calculate margin by publisher
- **Cohort analysis** — track whether readers who rate highly in one genre cross-purchase into others
- **Predictive checkout model** — use historical checkout trends to forecast demand and inform stock levels
- **Author productivity over time** — requires publication date data per author to track output trajectory
- **Return on marketing spend** — correlate ESP's $2.32M marketing investment against actual sales volume per publisher to measure ROI
- **Series completion forecasting** — model expected revenue from planned but unpublished series volumes

---

## Repository

| Link | Description |
|---|---|
| [Dashboard Guide](docs/dashboard_guide.md) | Full worksheet and layout specifications |
| [Data Dictionary](docs/data_dictionary.md) | All field definitions across 12 source sheets |
| [Calculated Fields](docs/calculated_fields.md) | Every formula used with plain-language explanation |
| [Presentation Script](presentation/presentation_script.md) | 20-minute narrated findings script |

---

*Project completed: June 2026 · Dataset: Fictional · Tool: Tableau Desktop · Author: [Your Name]*
