# Market Basket Analysis - Grocery Data Using Apriori Model

A data mining project that uncovers hidden purchasing patterns in grocery transaction data using the **Apriori algorithm** and **association rule mining**. The analysis identifies products frequently bought together, enabling actionable retail strategies like product placement, promotional bundling, and cross-selling.

![Association Rules Flow](Market%20Basket%20Anaylsis%20-%20Grocery%20Data%20usign%20Apriori%20Model.png)

---

## Table of Contents

- [Overview](#overview)
- [Dataset](#dataset)
- [Methodology](#methodology)
- [Key Findings](#key-findings)
- [Technologies Used](#technologies-used)
- [Getting Started](#getting-started)
- [Project Structure](#project-structure)
- [License](#license)

---

## Overview

**Market Basket Analysis (MBA)** is a technique used by retailers to understand customer purchasing behavior by discovering co-occurrence relationships between products in transactional data. This project applies the Apriori algorithm to a grocery store dataset to extract frequent itemsets and generate association rules that reveal which products are most likely to be bought together.

### Core Concepts

- **Affinity Analysis** - Discovers co-occurrence relationships among items recorded in transactions, enabling retailers to understand customer purchase behavior.
- **Association Rule Mining** - Identifies combinations of items that occur together frequently and expresses them as "if → then" rules (e.g., *if yogurt, then whole milk*).
- **Apriori Algorithm** - Iteratively identifies frequent individual items and extends them to larger itemsets, pruning infrequent candidates at each level using the Apriori property: *if an itemset is infrequent, all its supersets are also infrequent*.

### Metrics

| Metric | Description | Formula |
|---|---|---|
| **Support** | How popular an itemset is, measured by the proportion of transactions containing it | `P(A ∩ B)` |
| **Confidence** | How likely item B is purchased when item A is purchased | `P(B\|A) = Support(A ∩ B) / Support(A)` |
| **Lift** | How much more likely B is purchased when A is purchased, controlling for B's popularity | `Confidence(A → B) / Support(B)` |

A **lift > 1** indicates a positive association (items are bought together more often than by chance), **lift = 1** indicates independence, and **lift < 1** indicates a negative association.

---

## Dataset

The project uses the [Groceries Dataset](https://www.kaggle.com/datasets/heeraldedhia/groceries-dataset) containing transaction records from a grocery store.

| Property | Value |
|---|---|
| Total purchase records | 38,765 |
| Unique customers | 3,898 |
| Unique products | 167 |
| Unique transactions (baskets) | 14,963 |
| Date range | January 2014 – December 2015 |

Each row represents a single item purchased by a customer on a given date. The three columns are:

- `Member_number` - Unique customer identifier
- `Date` - Date of purchase
- `itemDescription` - Name of the grocery item

### Top 10 Most Purchased Items

| Rank | Item | Purchases |
|---|---|---|
| 1 | Whole milk | 2,363 |
| 2 | Other vegetables | 1,827 |
| 3 | Rolls/buns | 1,646 |
| 4 | Soda | 1,453 |
| 5 | Yogurt | 1,285 |
| 6 | Root vegetables | 1,041 |
| 7 | Tropical fruit | 1,014 |
| 8 | Bottled water | 908 |
| 9 | Sausage | 903 |
| 10 | Citrus fruit | 795 |

---

## Methodology

The notebook follows a structured pipeline:

### 1. Data Loading & Exploration
Load the CSV and explore customer purchase patterns, date ranges, and item distributions.

### 2. Transaction Creation
Group individual item records into baskets - each unique combination of `Member_number` and `Date` forms one transaction (a list of items bought together in a single trip).

### 3. One-Hot Encoding
Transform the list of transactions into a binary matrix (14,963 × 167) using `TransactionEncoder` from mlxtend, where each row is a transaction and each column is a product (`True` = purchased, `False` = not purchased).

### 4. Frequency Analysis & Treemap Visualization
Compute item purchase frequencies and visualize the top 50 items using a treemap.

### 5. Apriori Algorithm
Apply the Apriori algorithm with `min_support=0.001` and `max_len=5` to discover frequent itemsets.

**Results:**
- 149 frequent single items
- 649 frequent 2-item pairs
- 13 frequent 3-item triples
- **811 total frequent itemsets**

### 6. Association Rule Generation
Generate association rules from the frequent itemsets using lift as the evaluation metric, producing **1,376 association rules**.

### 7. Visualization
Visualize the rules using PyARMViz's parallel category (Sankey-style) plot, showing the flow of associations between antecedent items and consequent items.

---

## Key Findings

### Strongest Association Rules (by Lift)

| Rule | Support | Confidence | Lift |
|---|---|---|---|
| {whole milk, yogurt} → {sausage} | 0.0015 | 0.1317 | 2.18 |
| {sausage, whole milk} → {yogurt} | 0.0015 | 0.1642 | 1.91 |
| {specialty chocolate} → {citrus fruit} | 0.0014 | 0.0879 | 1.65 |
| {sausage, yogurt} → {whole milk} | 0.0015 | 0.2558 | 1.62 |
| {tropical fruit} → {flour} | 0.0011 | 0.0158 | 1.62 |

### Insights

1. **Whole milk is the anchor product** - present in 15.8% of all transactions and involved in more association rules than any other item.
2. **Core product clusters** identified: whole milk ↔ other vegetables, rolls/buns ↔ sausage, yogurt ↔ whole milk, soda ↔ rolls/buns.
3. **Three-item combinations** like {whole milk, yogurt, sausage} show the strongest lift values, indicating customers who buy two of these are highly likely to buy the third.

### Business Applications

- **Shelf Placement** - Place strongly associated items (e.g., sausage near yogurt and whole milk) in adjacent aisles to encourage cross-purchases.
- **Promotional Bundling** - Offer combo discounts on high-lift item pairs to increase basket size.
- **Targeted Advertising** - Recommend associated items to customers based on their current basket contents.
- **Store Layout Optimization** - Use the Sankey flow diagram to inform how product categories should flow through the store.

---

## Technologies Used

- **Python 3**
- **pandas** - Data manipulation and analysis
- **NumPy** - Numerical computation
- **mlxtend** - Apriori algorithm and association rule mining
- **matplotlib / seaborn** - Data visualization
- **squarify** - Treemap visualization
- **PyARMViz** - Association rule visualization (Sankey/parallel category plots)

---

## Getting Started

### Prerequisites

```bash
pip install pandas numpy matplotlib seaborn mlxtend squarify PyARMViz
```

### Running the Analysis

1. Clone the repository:
   ```bash
   git clone https://github.com/keithfernandes12/Market-Basket-Analysis.git
   cd Market-Basket-Analysis
   ```

2. Launch Jupyter Notebook:
   ```bash
   jupyter notebook "Market Basket Analysis - Grocery Data usign Apriori Model.ipynb"
   ```

3. Run all cells sequentially. The notebook will:
   - Load and preprocess the grocery dataset
   - Build transaction baskets and encode them
   - Run the Apriori algorithm to find frequent itemsets
   - Generate and rank association rules
   - Produce visualizations including the Sankey flow diagram

---


