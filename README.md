# Trader Behavior vs Market Sentiment Analysis

This project studies how trader behavior and profitability change across market sentiment regimes using two datasets:
- historical trade-level activity
- daily Fear & Greed sentiment index

The analysis is implemented in a single exploratory notebook and focuses on turning raw trading logs into daily behavior/performance signals.

## Project Goal

Understand whether trading outcomes and behavior are materially different on Fear days vs Greed days.

Key questions:
- Are traders more profitable in fearful or greedy markets?
- Does behavior (trade count, size, leverage) shift with sentiment?
- Which trader segments perform better across sentiment conditions?

## Data Used

1. `Data/historical_data.csv`
- 211,224 trade rows
- 16 columns (price, size, side, account, realized PnL, timestamps, etc.)
- Date coverage used in analysis: May 1, 2023 to May 1, 2025 (IST)
- 32 unique accounts

2. `Data/fear_greed_index.csv`
- 2,644 daily rows
- columns: timestamp, value, classification, date

## Methodology (Notebook Workflow)

Notebook: `Notebook/trader_behavior_sentiment_analysis.ipynb`

Main steps:
1. Load and validate both datasets.
2. Run quality checks (nulls/duplicates).
3. Verify timestamp reliability and choose `Timestamp IST` for day-level analysis.
4. Build daily market metrics (PnL, trades, active accounts, win rate, drawdown proxy).
5. Merge with sentiment labels and create Fear/Neutral/Greed grouping.
6. Compare performance on Fear vs Greed days.
7. Use bootstrap confidence intervals for non-normal PnL comparisons.
8. Analyze behavior shifts (trade frequency, size, leverage proxy, directional bias).
9. Segment traders by leverage, frequency, and consistency.

### Leverage proxy
The raw data does not provide explicit leverage. A proxy is estimated as:

`trade notional / starting position notional`

This is clipped at the 99th percentile to reduce outlier distortion.

## Key Findings

From the executed notebook output:

- Mean daily PnL on Fear days: 39,012
- Mean daily PnL on Greed days: 15,848
- Fear/Greed PnL ratio: 2.5x

- Average trades per day:
  - Fear: 793
  - Greed: 294

- Average leverage proxy:
  - Fear: 0.053
  - Greed: 0.184

Interpretation: in this dataset, Fear days are more active and more profitable on average, while Greed days show lower activity but higher leverage usage.

Additional segmentation snapshot:
- Frequent traders average daily PnL: 14,129
- Infrequent traders average daily PnL: 3,486

## Project Structure

```text
trader-behavior-sentiment-analysis/
|-- Data/
|   |-- historical_data.csv
|   `-- fear_greed_index.csv
|-- Notebook/
|   `-- trader_behavior_sentiment_analysis.ipynb
|-- Requirements.txt
`-- README.md
```

## Setup

Python used locally: 3.13

1. Create and activate a virtual environment
```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```

2. Install dependencies
```powershell
pip install -r Requirements.txt
```

3. (If needed) install Jupyter
```powershell
pip install notebook
```

4. Run the notebook
```powershell
jupyter notebook Notebook/trader_behavior_sentiment_analysis.ipynb
```

## Reproducibility Notes

- Analysis is notebook-based (no packaged pipeline yet).
- Date parsing is done with `dayfirst=True` for `Timestamp IST`.
- Fear/Greed matching coverage after merge is ~99.8%.


