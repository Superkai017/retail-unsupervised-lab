# Retail unsupervised learning lab

A hands-on lab for practicing unsupervised learning — customer segmentation on real e-commerce transaction data using RFM feature engineering and K-Means clustering.

## Objective

This is a learning exercise, not a production system. The goal is to get comfortable with the unsupervised workflow (clean → engineer features → scale → cluster → visualize → interpret) the same way earlier work covered the supervised workflow (train/test split → feature selection → model → evaluate).

## Dataset

**Online Retail** (UCI Machine Learning Repository) — ~542K invoice-line transactions from a UK-based online retailer, December 2010 to December 2011.

| Column | Description |
|---|---|
| `InvoiceNo` | Invoice ID (prefixed `C` for cancellations) |
| `StockCode` | Product code |
| `Description` | Product name |
| `Quantity` | Units purchased |
| `InvoiceDate` | Date and time of transaction |
| `UnitPrice` | Price per unit (£) |
| `CustomerID` | Customer identifier (~25% missing) |
| `Country` | Customer's country |

## Project structure

```
retail-unsupervised-lab/
├── data/
│   └── online_retail.csv      # raw dataset, untouched
├── unsupervised_lab.ipynb     # everything happens here: clean → RFM → scale → KMeans → visualize
├── requirements.txt           # pandas, scikit-learn, matplotlib
└── README.md                  # this file
```

Kept intentionally flat — one notebook, one data folder. No `src/`, no config files, no Docker. That structure comes later, once the concepts are second nature and the project needs to be reused or deployed.

## Setup

```bash
python -m venv venv
source venv/bin/activate      # Windows: venv\Scripts\activate
pip install -r requirements.txt
jupyter notebook unsupervised_lab.ipynb
```

## Workflow

1. **Clean** — drop cancelled orders, missing `CustomerID`, negative/zero quantity and price.
2. **Feature engineer (RFM)** — Recency (days since last order), Frequency (order count), Monetary (total spend), per customer.
3. **Scale** — StandardScaler, since K-Means is distance-based.
4. **Choose k** — elbow method (inertia) + silhouette score.
5. **Cluster** — fit K-Means, assign each customer to a segment.
6. **Visualize** — PCA down to 2D to plot the clusters.
7. **Interpret** — profile each cluster's average R/F/M to label it (e.g. loyal high-spenders, at-risk, new).

## What I did

*(fill in after completing the notebook — dataset size after cleaning, k chosen and why, silhouette score achieved)*

## What I learned

*(fill in — what unsupervised learning felt like compared to supervised, what surprised you, what you'd try next: hierarchical clustering, DBSCAN, market basket analysis)*
