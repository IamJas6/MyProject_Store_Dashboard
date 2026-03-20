# 📊 Super Store Interactive Dashboard — Analysis Report

## 1. Project Overview

This dashboard is built on the **Sample Superstore dataset** and represents the most technically advanced dashboard in this portfolio. It goes beyond static visualization — every chart on the dashboard is **fully dynamic**, driven by interactive parameters, calculated fields, and parameter actions that respond in real time to user selections.

The goal is to allow business users to explore Sales, Profit, and Quantity performance across multiple dimensions — Category, Segment, Ship Mode, Sub-Category, and State — all from a single, unified view.

---

## 2. 🔧 Interactive Features — How the Dashboard Works

This section explains the technical building blocks used to make the dashboard interactive. Understanding these features is key to appreciating the complexity behind what appears to be a simple, clean interface.

---

### ⚙️ Feature 1: Metric Parameter (Parameter 1)

**What it is:**
A **Tableau Parameter** that stores the currently selected metric — either `Sales`, `Profit`, or `Quantity`. The default value is `Sales`.

**How it works:**
The `Select A Metric` sheet acts as a clickable button strip. When the user clicks any metric name, a **Parameter Action** fires and updates `Parameter 1` to the selected value. Every chart on the dashboard is built on a **Calculated Field** that reads this parameter:

```
CASE [Parameter 1]
  WHEN "Profit"   THEN [Profit]
  WHEN "Sales"    THEN [Sales]
  WHEN "Quantity" THEN [Quantity]
END
```

**Impact:**
Clicking `Profit` instantly switches ALL 8 charts to show Profit data. Clicking `Quantity` switches everything to Quantity. One click — entire dashboard redraws. This eliminates the need for separate dashboards per metric.

---

### ⚙️ Feature 2: State Parameter (State Parameter)

**What it is:**
A **Tableau Parameter** that stores the currently selected US State. The default value is `Illinois`.

**How it works:**
The `Select A State` sheet acts as a state selector. Clicking a state triggers a **Parameter Action** that updates `State Parameter`. A **Calculated Field** then evaluates:

```
IF [State] = [State Parameter] THEN "select"
ELSE [Parameter 1]
END
```

**Impact:**
The selected state gets highlighted with a distinct color across all relevant charts — visually separating it from the rest. This allows users to **benchmark a specific state** against the broader dataset without applying a hard filter that removes other data.

---

### ⚙️ Feature 3: Parameter Actions

**What it is:**
Parameter Actions are **click-based triggers** that update parameter values when a user interacts with a chart. Unlike filter actions (which show/hide data), parameter actions **change the calculation logic itself**.

**Used in this dashboard:**
- Clicking `Select A Metric` → updates `Parameter 1` → redraws all metric-based charts
- Clicking `Select A State` → updates `State Parameter` → highlights the selected state

**Why it matters:**
Parameter Actions are an advanced Tableau feature that most beginners never use. They allow the dashboard to behave like a **custom-built web application** — fully interactive, context-aware, and responsive — without any external code.

---

### ⚙️ Feature 4: Calculated Fields

Three key calculated fields power the entire dashboard:

| Calculated Field | Formula | Purpose |
|---|---|---|
| **Dynamic Metric** | `CASE [Parameter 1] WHEN "Profit" THEN [Profit] WHEN "Sales" THEN [Sales] WHEN "Quantity" THEN [Quantity] END` | Single field that dynamically returns the selected metric |
| **State Highlight** | `IF [State] = [State Parameter] THEN "select" ELSE [Parameter 1] END` | Applies distinct color to the selected state |
| **State Match** | `[State Parameter] = [State]` | Boolean true/false used for conditional formatting |

---

### ⚙️ Feature 5: KPI Cards with Trend Lines

The dashboard includes **3 KPI metric cards** — Sales, Profit, and Quantity — each paired with a dedicated **trend line chart** showing performance over time (by Order Date). This gives users an immediate at-a-glance health check of the business before diving into dimensional breakdowns.

---

## 3. Dashboard Charts & Analysis

---

### 📌 Chart Group 1: KPI Cards — Sales, Profit, Quantity

**Business Question:** What is the overall performance of the business across the three key metrics?

#### 🔍 What it Shows
- Three headline KPI numbers displayed prominently — total Sales, total Profit, and total Quantity
- Each KPI is paired with a **trend line** (Profit Trend, Sales Trend, Quantity Trend) showing the metric over time by Order Date
- Provides instant business health snapshot before any dimensional breakdown

#### 💡 Insight
- The trend lines reveal whether Sales and Profit are growing in tandem or diverging — divergence signals rising costs or discount erosion
- A declining Profit trend alongside stable or growing Sales is a **red flag for margin compression**
- Quantity trend shows whether volume growth is actually driving revenue, or whether discounting is inflating volume at the cost of profit

#### ✅ Recommendation
- Monitor the **Sales vs Profit trend gap** closely — if Sales grows faster than Profit, a discount audit is overdue
- Use the KPI cards as a **daily executive summary** — any sharp drop in any metric should trigger an immediate drill-down into the dimensional charts below

---

### 📌 Chart Group 2: Dynamic Dimension Charts (Category, Segment, Ship Mode, Sub-Category)

**Business Question:** Which categories, segments, ship modes, and sub-categories are performing best — for whichever metric the user selects?

#### 🔍 What it Shows
- **Category X** — Bar chart: 3 product categories (Furniture, Office Supplies, Technology) ranked by the selected metric
- **Segment X** — Bar chart: 3 customer segments (Consumer, Corporate, Home Office) ranked by the selected metric
- **Ship Mode X** — Bar chart: 4 shipping methods ranked by the selected metric
- **Sub Category X** — Bar chart: All 17 sub-categories ranked by the selected metric
- All 4 charts are sorted in **descending order** and switch simultaneously when the metric parameter changes

#### 💡 Insight (Sales view — default):
- **Technology** leads in Sales among categories, while **Furniture** lags despite being a high-ticket category — suggesting lower volume or heavy discounting
- **Consumer segment** drives the most Sales — it is the largest and most active buyer group
- **Standard Class** is the dominant ship mode by volume — most customers choose economy shipping
- **Phones and Chairs** top the sub-category sales rankings — high-demand, high-ticket items

#### 💡 Insight (Profit view):
- **Profit by category** often reveals a different story than Sales — Technology maintains strong margins while Furniture frequently shows compressed profit
- **Tables and Bookcases** in the sub-category profit view are known loss-makers — their large size in the Sales chart combined with small or negative Profit bars is a critical signal
- Switching to Profit view exposes which segments and ship modes are actually profitable vs just high-revenue

#### ✅ Recommendation
- Always toggle between **Sales and Profit views** before making inventory or marketing decisions — a top-selling sub-category with negative profit needs immediate discounting intervention
- Use the **Quantity view** to identify volume leaders that may not be revenue leaders — these could be candidates for price increases

---

### 📌 Chart Group 3: Select A State (State Highlighting)

**Business Question:** How does a specific state compare to others on the selected metric?

#### 🔍 What it Shows
- A ranked bar chart of all US states by the selected metric
- The **currently selected state** (default: Illinois) is highlighted in a distinct color
- All other states remain visible — the selection does NOT filter out other data
- Clicking any state immediately re-highlights it across the dashboard

#### 💡 Insight
- State-level analysis reveals **geographic performance concentration** — a few states typically account for disproportionate sales
- The highlight feature enables quick **state benchmarking** — comparing one state's bar against its neighbors without losing context of the full picture
- States with high Sales but low Profit in the Profit view are likely markets with heavy discounting or high logistics costs

#### ✅ Recommendation
- Use state highlighting to **build region-specific sales strategies** — underperforming states near high-performing ones may benefit from targeted promotions
- Identify states where Profit per Sale is low and investigate whether **local competitors, discount pressure, or shipping costs** are the root cause

---

## 4. Technical Summary — What Makes This Dashboard Advanced

| Feature | Complexity Level | Description |
|---|---|---|
| **Parameter 1 (Metric Switcher)** | Advanced | Drives all charts simultaneously with one click |
| **State Parameter** | Advanced | Enables state-level highlighting without filtering |
| **Parameter Actions** | Expert | Click-to-update parameters — no filter dialogs needed |
| **Dynamic Calculated Fields** | Advanced | CASE-based formula dynamically returns selected metric |
| **Conditional State Coloring** | Advanced | IF-THEN logic highlights selected state across charts |
| **KPI Cards + Trend Lines** | Intermediate | Paired metric + trend for executive-level summary |
| **Synchronized Chart Updates** | Expert | 8+ charts redraw simultaneously on single interaction |

---

## 5. Tools & Dataset

| Item | Details |
|---|---|
| **Tool** | Tableau Desktop / Tableau Public |
| **Dataset** | Sample Superstore (`Sample - Superstore.xls`) |
| **Key Fields** | Order Date, Category, Sub-Category, Segment, Ship Mode, State, Sales, Profit, Quantity |
| **Parameters Used** | Parameter 1 (Metric), State Parameter |
| **Calculated Fields** | Dynamic Metric, State Highlight, State Match |
| **Actions Used** | Parameter Actions (click-to-update) |

---

*Analysis by Shubham | Super Store Interactive Dashboard Project*