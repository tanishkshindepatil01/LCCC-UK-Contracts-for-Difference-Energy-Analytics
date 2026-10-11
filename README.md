# Low Carbon Contracts Company (LCCC) | UK Contracts for Difference Energy Analytics

### Renewable Generation, CfD Financial Performance & Net Zero Analysis

An **independent energy analytics portfolio project** built using open datasets published by the **Low Carbon Contracts Company (LCCC)**. I use Python to examine how the UK's **Contracts for Difference (CfD)** scheme relates to renewable electricity generation, financial settlement flows, market reference prices, auction awards, contract capacity and estimated avoided greenhouse gas emissions.

**Tools:** Python · pandas · NumPy · Matplotlib · Seaborn · Jupyter Notebook · Tableau-ready CSV

> **Independent project notice:** This is not an official LCCC report and is not affiliated with, commissioned by or endorsed by LCCC. Calculations and interpretations are my own and use public information.

## Project questions

- How has CfD-supported electricity generation changed across years and technologies?
- How do net CfD payments vary with reference prices, strike prices and generation?
- What does the published contract portfolio tell us about capacity and project status?
- How have auction awards and strike prices differed across allocation rounds?
- What do current payment forecast *revisions* show, and what can we responsibly conclude from them?
- How can generation, financial and estimated carbon-impact metrics be presented in a single Tableau reporting workflow?

## Data sources

The analysis uses six public CSV extracts from the [LCCC Data Portal](https://dp.lowcarboncontracts.uk/):

| File in `data/raw/` | Role in this analysis |
| --- | --- |
| `actual_cfd_generation_and_avoided_ghg_emissions.csv` | Historical eligible generation, CfD payments, price measures and estimated avoided emissions |
| `cfd_contract_portfolio_status.csv` | Contract-level capacity, status and project information |
| `auction_outcomes.csv` | CfD auction awards, capacity and published strike prices |
| `forecast_average_strike_price_and_market_price.csv` | Financial-year price forecasts and strike-price summaries |
| `forecast_market_reference_prices.csv` | Forward-looking market reference price forecasts |
| `in_period_tracking.csv` | Initial and latest in-period CfD payment forecasts |

The historical generation records span **June 2016 to September 2026** in the supplied extract. Other datasets have different publication windows and reporting levels. Results describe these downloaded snapshots rather than a live feed.

## What I did

1. **Load and inspect:** Read the six raw CSV files; inspect field types, row counts, missingness and reporting periods.
2. **Clean with care:** Standardise dates and labels and remove exact duplicates. Keep valid zero and negative financial flows; do not invent missing observations.
3. **Explore and visualise:** Build **18 visualisations** covering energy production, technology mix, net payments, price relationships, avoided emissions, portfolio capacity, auction awards and forecasts.
4. **Analyse finance and forecasts:** Compare prices and net settlements, and measure revisions between paired initial and latest daily forecasts.
5. **Prepare Tableau data:** Export one consolidated, **long-format** CSV with metric names, units, source identifiers and aggregation guidance.
6. **Validate outputs:** Reconcile important totals in the Tableau export against the source tables.

The notebook includes written context before each analysis section, chart outputs, and short interpretations afterwards.

## Project structure

```text
UK_CfD_Analytics_Project/
├── data/
│   ├── raw/
│   │   ├── actual_cfd_generation_and_avoided_ghg_emissions.csv
│   │   ├── auction_outcomes.csv
│   │   ├── cfd_contract_portfolio_status.csv
│   │   ├── forecast_average_strike_price_and_market_price.csv
│   │   ├── forecast_market_reference_prices.csv
│   │   └── in_period_tracking.csv
│   └── processed/
│       └── LCCC_CfD_Tableau_Consolidated.csv
├── notebooks/
│   └── UK_CfD_Analytics_EDA.ipynb
└── README.md
```

## Run the analysis

1. Download or clone the repository and keep all six original CSV files in `data/raw/`.
2. Install Python 3 and the packages used by the notebook:

   ```bash
   python -m pip install pandas numpy matplotlib seaborn ipython jupyter
   ```

3. Open `notebooks/UK_CfD_Analytics_EDA.ipynb` in JupyterLab, Jupyter Notebook or VS Code.
4. Select **Run All**. The notebook supports execution from the project root or from `notebooks/`.
5. Find the consolidated Tableau file in `data/processed/LCCC_CfD_Tableau_Consolidated.csv`.

The included notebook has **already been executed**, with plots and cell outputs saved for review. It reads local files from `data/raw/` and does not require an API connection.

## Tableau reporting

Use **`data/processed/LCCC_CfD_Tableau_Consolidated.csv`** as the single input to Tableau. It contains **47,361 records** covering **26 metrics** and the relevant descriptive dimensions. Because the six sources use different levels of detail, I have **stacked them in a long-format metric table** instead of joining contract, award and daily observation rows into one wide dataset.

Important fields include:

- `record_group`, `grain` and `source_file` — identify the source and level of detail.
- `date`, `year`, `month`, `technology`, `project`, `status` and other dimensions — support time-series and segmentation views where relevant.
- `metric`, `value`, `unit` and `aggregation_hint` — tell Tableau which values to display and how to aggregate them.

### Proposed dashboards

**1. Renewable Generation & Carbon Impact**  
Eligible generation over time, technology mix and modelled avoided GHG emissions.

**2. CfD Financial Performance & Market Prices**  
Net payment flows, strike and reference price context, and technology-level comparisons.

**3. Contract Portfolio, Auctions & Forecast Revisions**  
Contract capacity and status, auction-awarded capacity and changes between initial and latest payment forecasts.

**Tableau setup:** Filter to the relevant `record_group` and `metric` for each sheet, and use `unit` and `aggregation_hint` to avoid summing prices, rates or differently-grained observations. The notebook's final section provides chart-building suggestions.

## Interpretation notes and limitations

- **CfD generation is not total UK renewable generation.** It covers eligible volumes in the provided CfD data.
- **Payments can be negative.** These are meaningful settlement outcomes and should not be dropped or converted to absolute values.
- **2016 and 2026 are partial years** in the historical extract; treat full-year comparisons carefully.
- **Payment forecast accuracy cannot be assessed from this tracking extract:** the actual-payment field has no populated values. I therefore analyse **forecast revisions**, not forecast-versus-actual error.
- **Contract portfolio capacity is a snapshot,** including categories that are not operating. Contracted MW must not be labelled as realised generation MWh.
- **Auction capacity is awarded, not necessarily installed or operating.** Historic strike prices may be quoted on different price bases; nominal trends alone are not inflation-adjusted comparisons.
- **Avoided emissions are modelled estimates,** not experimentally observed counterfactual reductions.
- **Price measures require appropriate aggregation.** Weighted price calculations should use the corresponding energy weights rather than a simple sum of prices.

## Source and attribution

Data source: [Low Carbon Contracts Company — Public Data Portal](https://dp.lowcarboncontracts.uk/). Please review dataset definitions, publication dates and licensing terms at source before redistributing the underlying data.

**Project scope:** Independent portfolio analysis aligned with UK renewable electricity and CfD market analytics; not an official company assessment.
