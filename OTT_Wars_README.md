# 🎬 India OTT Wars — Competitive Market Analysis (2019–2024)

> A 5-page interactive Power BI dashboard analysing India's ₹10,900 Cr OTT market across 5 platforms over 6 years

[![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)](https://powerbi.microsoft.com)
[![DAX](https://img.shields.io/badge/DAX-0078D4?style=for-the-badge&logo=microsoft&logoColor=white)](https://learn.microsoft.com/en-us/dax/)
[![Excel](https://img.shields.io/badge/Excel-217346?style=for-the-badge&logo=microsoft-excel&logoColor=white)](https://www.microsoft.com/excel)

---

## 📊 Live Dashboard

🔗 [View the Dashboard (PDF)](https://drive.google.com/file/d/1ftkKIEYwm5-XdtSm4A-fHWIOoUSCZYHw/view)

---

## 🎯 Project Overview

India's OTT market is one of the fastest-growing streaming markets globally — yet deeply misunderstood. This project breaks down 6 years of subscriber, revenue, content, and engagement data across 5 major platforms to surface the real competitive dynamics behind India's streaming wars.

**Platforms covered:** Netflix India · Amazon Prime Video · JioCinema · Disney+ Hotstar · SonyLIV

**Time period:** 2019 – 2024E

**Market size:** ₹10,900 Cr (2024)

---

## 🔍 Key Business Insights

| Insight | Finding |
|---|---|
| 🚀 Fastest grower | JioCinema: ₹150 Cr → ₹4,000 Cr revenue (4x) post IPL acquisition |
| 📉 Biggest loser | Disney+ Hotstar: 24% subscriber decline after losing IPL rights |
| 💰 Best content ROI | Amazon Prime Video: 1.25x — highest revenue per ₹ spent on content |
| 👑 Premium leader | Netflix India: ₹499/month ARPU — highest in market |
| 📱 Mass market king | JioCinema: 450 Mn MAU in 2024, mobile-first at 82% |

---

## 🗂️ Dashboard Pages

| Page | What it shows |
|---|---|
| **1 — Overview** | KPI cards, 2024 market share donut, MAU vs ARPU bubble, revenue vs content spend |
| **2 — Growth Race** | Subscriber line chart 2019–2024, market share shift stacked area |
| **3 — Revenue Deep Dive** | Revenue trends, YoY% growth, ARPU comparison, content spend vs revenue scatter |
| **4 — Content Strategy** | Content investment race, library composition, content ROI by platform |
| **5 — Engagement & Ads** | MAU growth, CPM comparison, ad revenue trend |

---

## 🏗️ Data Model — Star Schema

```
                    ┌─────────────┐
                    │  Platforms  │  ← Dimension
                    │  (5 rows)   │
                    └──────┬──────┘
                           │ 1:Many
          ┌────────────────┼────────────────┐
          │                │                │
    ┌─────▼──────┐  ┌──────▼─────┐  ┌──────▼──────┐
    │Subscribers │  │  Revenue   │  │Content_Inv  │  ← Fact tables
    └────────────┘  └────────────┘  └─────────────┘
          │                │                │
          └────────────────┼────────────────┘
                    ┌──────▼──────┐
                    │  DateTable  │  ← Date Dimension
                    │ 2019–2024   │
                    └─────────────┘

    Also connected to Platforms:
    ├── Content_Library  (static snapshot)
    └── MAU_Engagement   (unpivoted 2022–2024)
```

**6 fact tables · 2 dimension tables · Star schema design**

---

## ⚙️ Technical Implementation

### Power Query Transformations
- Unpivoted year columns (2019–2024E) across 4 fact tables to convert wide → tall format for time-series analysis
- Removed source footnote rows and null platform entries
- Created calculated `Date` columns (`DATE([Year], 1, 1)`) for DateTable relationship
- Added custom ARPU column via M formula to preserve data lost during unpivot

### DAX Measures (15+)

```dax
-- Year-over-year revenue growth
Revenue YoY% = 
DIVIDE(
    [Total Revenue] - [Revenue LY],
    [Revenue LY],
    0
)

-- Market share by platform
Market Share% = 
DIVIDE(
    [Total Subscribers],
    CALCULATE([Total Subscribers], ALL(Platforms)),
    0
)

-- Content ROI (revenue per ₹ spent on content)
Content ROI = 
DIVIDE(
    CALCULATE(SUM(Revenue[Revenue_Cr]), 
        FILTER(ALL(DateTable), DateTable[Year] = MAX(DateTable[Year]))),
    CALCULATE(SUM(Content_Investment[Total Content Spend (₹ Cr)]), 
        FILTER(ALL(DateTable), DateTable[Year] = MAX(DateTable[Year]))),
    0
)

-- Previous year revenue for time intelligence
Revenue LY = 
CALCULATE(
    [Total Revenue],
    FILTER(ALL(DateTable), 
        DateTable[Year] = MAX(DateTable[Year]) - 1)
)
```

### Data Sources
All data manually compiled from public industry reports:
- FICCI-EY Media & Entertainment Report 2024
- MCA/RoC filings
- Mint, Economic Times, Business Standard
- Platform investor relations disclosures

---

## 📁 Repository Structure

```
india-ott-wars/
├── data/
│   └── India_OTT_Wars_Dataset.xlsx    # Source data (6 sheets)
├── dashboard/
│   └── OTT_Wars_Dashboard.pbix        # Power BI file
├── exports/
│   └── OTT_Wars_Dashboard.pdf         # PDF export (all 5 pages)
├── theme/
│   └── OTT_Wars_Theme.json            # Custom dark theme file
└── README.md
```

---

## 🛠️ Tools & Skills Demonstrated

| Skill | Application |
|---|---|
| **Power BI Desktop** | 5-page dashboard, custom visuals, slicers, annotations |
| **Power Query (M)** | Unpivot, null handling, custom columns, date transformation |
| **DAX** | Time intelligence, CALCULATE, FILTER, DIVIDE, SUMMARIZE |
| **Data Modelling** | Star schema, 1:Many relationships, dimension/fact separation |
| **Data Visualisation** | Line, donut, scatter, stacked area, clustered bar, waterfall |
| **Business Analysis** | OTT market dynamics, content ROI, ARPU benchmarking |

---

## 💡 Why This Project

As a data analyst working in sports media analytics at YouGov Sport, I analyse broadcast audience data and sponsorship ROI daily. The IPL rights shift from Disney+ Hotstar to JioCinema in 2023 was one of the most seismic events in Indian media — I wanted to quantify exactly what that meant in subscriber, revenue, and engagement terms across the entire OTT landscape.

---

## 👩‍💻 About

**Netra Pawar** — Data Analyst | Sports Technology & Analytics

🔗 [LinkedIn](https://www.linkedin.com/in/netrap18/) · [GitHub](https://github.com/netrap18) · netrap181@gmail.com
