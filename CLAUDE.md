# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A single-notebook learning lab for unsupervised customer segmentation on the UCI **Online Retail** dataset (~542K invoice-line transactions, UK online retailer, Dec 2010–Dec 2011). It is a hands-on exercise, not a production system. The whole workflow lives in one notebook — keep it that way unless asked; there is intentionally no `src/`, no config, no Docker.

## Actual file layout (differs from README)

The README describes an idealized layout; the real files are:

- **`unsupervised.ipynb`** — the notebook where everything happens (README calls it `unsupervised_lab.ipynb`).
- **`data/Online Retail - Online Retail.csv`** — the raw 44 MB dataset (README calls it `data/online_retail.csv`). The notebook loads it via a hardcoded absolute Windows path: `r'D:\Unsupervised_lab\retail-unsupervised-lab\data\...'`. Update this path if the repo moves.
- **No `requirements.txt` exists** despite the README referencing one. Dependencies used by the notebook: `numpy`, `pandas`, `matplotlib`, `seaborn`, `scikit-learn`, `joblib`, `jupyter`.

## Running

Open and run the notebook top-to-bottom:

```bash
jupyter notebook unsupervised.ipynb
```

There is no test suite, linter, or build step. "Running the code" means executing notebook cells.

## Notebook conventions that matter

- **Cell order is load-bearing.** The pipeline mutates a single `df` in place across cells (drop duplicates → drop null `CustomerID` → filter `Quantity>0` & `UnitPrice>0` → RFM). Re-running an individual cell out of order re-filters an already-filtered `df` or hits type errors. After any edit, the correct fix is **Kernel → Restart & Run All**, not re-running one cell.
- **`InvoiceDate` must be parsed to datetime at load time.** It is converted in the `read_csv` cell with the explicit format `pd.to_datetime(df['InvoiceDate'], format='%m/%d/%y %H:%M')` — note the 2-digit year (`%y`) and month-first order (source strings look like `12/1/10 8:26`). Downstream RFM math (`reference_date = df['InvoiceDate'].max() + pd.Timedelta(days=1)`) fails with a string-concatenation TypeError if this conversion hasn't run.

## Pipeline (the "big picture")

clean → RFM feature engineering → StandardScaler → choose k (elbow + silhouette) → K-Means → PCA-to-2D for visualization → interpret clusters. RFM is computed per `CustomerID` by grouping the cleaned transactions: Recency = days from each customer's last order to a reference date (last invoice + 1 day), Frequency = count of distinct `InvoiceNo`, Monetary = sum of `Quantity * UnitPrice`.
