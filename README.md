# AML Transaction Detection

Exploratory data analysis and machine-learning pipeline for the
**IBM Transactions for Anti-Money Laundering (AML)** synthetic dataset.

Dataset: <https://www.kaggle.com/datasets/ealtman2019/ibm-transactions-for-anti-money-laundering-aml>

## Quick start

```bash
# 1. Install uv (if not present)
curl -LsSf https://astral.sh/uv/install.sh | sh

# 2. Create virtual environment and install dependencies
uv sync

# 3. Download dataset (requires Kaggle API token at ~/.kaggle/kaggle.json)
uv run python -c "
import kaggle
kaggle.api.authenticate()
kaggle.api.dataset_download_files(
    'ealtman2019/ibm-transactions-for-anti-money-laundering-aml',
    path='data/raw', unzip=True
)
"

# 4. Launch Jupyter
uv run jupyter lab
```

## Dataset

Six independent CSV files across two groups:

| Group | Size   | Approx illicit ratio |
|-------|--------|----------------------|
| HI    | Small  | ~5 %                 |
| HI    | Medium | ~5 %                 |
| HI    | Large  | ~5 %                 |
| LI    | Small  | ~0.1 %               |
| LI    | Medium | ~0.1 %               |
| LI    | Large  | ~0.1 %               |

### Columns

| Column             | Description                                   |
|--------------------|-----------------------------------------------|
| Timestamp          | Date-time of the transaction                  |
| From Bank          | Originating bank ID                           |
| Account (from)     | Originating account ID                        |
| To Bank            | Receiving bank ID                             |
| Account (to)       | Receiving account ID                          |
| Amount Received    | Amount received (in Receiving Currency)       |
| Receiving Currency | ISO currency code at destination              |
| Amount Paid        | Amount paid (in Payment Currency)             |
| Payment Currency   | ISO currency code at source                   |
| Payment Format     | Wire, Cheque, Credit Card, ACH, etc.          |
| Is Laundering      | Binary label – 1 = laundering, 0 = legitimate |

### Laundering patterns

- **Fan-out** – One account rapidly sends to many recipients
- **Fan-in** – Many accounts consolidate to one
- **Cycle** – Money circulates through a closed loop of accounts
- **Gather-Scatter** – Aggregate then disperse through layering
- **Scatter-Gather** – Disperse then re-aggregate
- **Stack** – Layered pass-through chains
- **Random** – Irregular mixing pattern

## Project layout

```
aml-transaction-detection/
├── data/
│   ├── raw/          <- original Kaggle CSVs
│   └── processed/    <- parquet caches
├── notebooks/
│   └── 01_eda.ipynb  <- main EDA notebook
├── src/              <- helper modules (future)
├── pyproject.toml
└── README.md
```
