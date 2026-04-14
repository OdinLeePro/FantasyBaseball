# MachineLearningFantasyBaseballDraftEngine

MachineLearningFantasyBaseballDraftEngine is an end to end data science and predictive analytics pipeline designed to optimize fantasy baseball draft strategy. The project ingests historical and projected player statistics, reverse engineers custom league scoring systems using Lasso Regression, calculates true Value Over Replacement Player (VORP), and utilizes K-Means clustering to generate a tiered, draft ready "Big Board." It emphasizes structured data cleaning, automated feature selection, mathematical replacement level modeling, and actionable draft day insights.

### Overview

This project was created as a comprehensive personal draft preparation tool to transform raw historical and projected baseball data into a clean, enriched, and analytics ready model. The final outputs include cleaned player projections, machine learning models that isolate precise fantasy point weights, historical replacement level calculations, and a clustered draft board ranking players by true mathematical value over their Average Draft Position (ADP).

The workflow includes:

- Pulling and curating historical data using R ('baseballr', 'dplyr') and standardizing innings notation
- Cleaning and merging 2026 player projections with current Average Draft Position (ADP) metrics
- Applying Lasso Regression with 5-fold Cross Validation to calculate point weights for Hitters, Starters, and Relievers
- Constructing a historical baseline for "Replacement Level" players based on true fantasy roster limitations
- Implementing K-Means clustering to group players into strategic drafting tiers
- Producing a master "Big Board" sorted by Position, Tier, and VORP to identify high value draft targets

### Features

- **Curation Layer Construction** — Generates:
  - Standardized historical datasets with normalized innings pitched and standardized naming conventions
  - Cleaned 2026 player projections merged seamlessly with ADP data
  - Separate, analytics ready datasets for Hitters, Starting Pitchers, and Relief Pitchers

- **Machine Learning (Scoring Weights)** — Adds:
 - Z-score standardization to ensure features are appropriately scaled
 - Automated feature selection driving irrelevant stats to zero
 - Precise coefficient calculations mapping exact fantasy point values to events (e.g., weighting Innings Pitched vs. Earned Runs)

- **Draft Strategy (VORP & Big Board)** — Produces:
 - Dynamic replacement level baselines (calculated at 120% of rostered players per position)
 - K-Means generated draft tiers for rapid visual grouping
 - A final, comprehensive dataset highlighting players whose mathematical VORP significantly outpaces their ADP

### Project Structure

```
/MachineLearningFantasyBaseballDraftEngine
├── HistoricalData/
│   ├── HistoricalDataCollectClean.qmd
│   ├── HittersHistorical.csv
│   ├── PitchersHistorical.csv
│   ├── RelieversHistorical.csv
│   └── StartersHistorical.csv
├── MachineLearningNotebooks/
│   ├── LinearRegressionHitters.ipynb
│   ├── LinearRegressionRelievers.ipynb
│   └── LinearRegressionStarters.ipynb
├── ProjectedData/
│   ├── AverageDraftPositionData/
│   │   ├── AverageDraftPositionDataClean/
│   │   │   ├── HittersADP2026Clean.csv
│   │   │   ├── PitchersADP2026Clean.csv
│   │   ├── AverageDraftPositionDataRAW/
│   │   │   ├── HittersADP2026RAW.csv
│   │   │   ├── PitchersADP2026RAW.csv
│   │   ├── ADPDataCleaning.ipynb
│   ├── ProjectedDataClean/
│   │   ├── HittersProj2026Clean.csv
│   │   ├── PitchersProj2026Clean.csv
│   ├── ProjectedDataRAW/
│   │   ├── HittersProj2026RAW.csv
│   │   ├── PitchersProj2026RAW.csv
│   └── ProjectedDataCleaning.ipynb
├── BigBoard.csv
├── BigBoard.ipynb
├── BigBoard.xlsx
├── LICENSE
├── README.md
└── TeamScoresAnalysis.xlsx
```

## Data Sources

This project utilizes multi source baseball data spanning historical records and future forecasts. The data includes:

- Raw historical player statistics sourced via the baseballr R package
- Consensus 2026 player statistical projections via FantasyPros.com
- Average Draft Position (ADP) consensus data via FantasyPros.com
- Historical fantasy league matchup scores from the 2023 and 2024 seasons

Fantasy baseball domain knowledge (e.g., standardizing partial innings from decimals to fractions, understanding positional scarcity, and streaming strategies) was used to enrich the data, filter out extreme outliers, and establish logical replacement level thresholds.

## Key Outputs

### 1. Curated Data Models

Analytics ready, highly cleaned datasets including:

- **Historical_Data** — stripped of extreme outliers and formatted for machine learning ingestion
- **Hitters_Projections** — cleaned projections merged with 2026 ADP
- **Starters_Projections** — cleaned projections merged with 2026 ADP
- **Relievers_Projections** — cleaned projections merged with 2026 ADP

### 2. Predictive Scoring Models

Three highly accurate Lasso Regression models ($R^2$ ~ 1.0) that successfully isolate the custom league scoring system:

- **Hitters** feature coefficients (e.g., heavily weighting Total Bases, Walks)
- **Starters** feature coefficients (e.g., heavily weighting Innings Pitched, Earned Runs)
- **Relievers** feature coefficients (e.g., heavily weighting Saves, Holds)

### 3. True Replacement Level Analysis

A data driven historical baseline utilizing:

- 12-team matchup scores from 2023-2025
- Roster position limit calculations (set at 120% of rostered players per position) to determine the true value of a bench level replacement player

### 4. The Big Board (Draft Optimizer)

A clustered, performance-optimized master spreadsheet that:

- Ranks every eligible player by their Value Over Replacement Player (VORP)
- Groups players into specific actionable tiers using K-Means clustering
- Contrasts mathematical value against public draft sentiment (ADP) to expose draft day steals

### 5. Visual Documentation

Embedded directly within the machine learning Jupyter notebooks:

- Horizontal bar charts displaying precise positive and negative coefficient weights for Hitters, Starters, and Relievers derived from the machine learning models.

## Pipeline Execution (How to Run)

To replicate this data pipeline or update it for a new fantasy season, execute the following steps in order:

**1. Calculate Base Replacement Level**
* **File:** `TeamScoresAnalysis.xlsx`
* **Action:** Update the *Number of Players on Roster* calculations.
* **Frequency:** Once at the start of the new year.

**2. Collect & Clean Historical Data**
* **File:** `HistoricalDataCollectClean.qmd`
* **Action:** Select the target year and filter the relevant rows.
* **Frequency:** Once at the start of the new year.

**3. Update Average Draft Position (ADP) Data**
* **Files:** `HittersADP2026RAW.csv` & `PitchersADP2026RAW.csv`
* **Action:** Export the latest consensus ADP from FantasyPros. Edit in Excel to match previous column structures.
* **Frequency:** Continuously updated (pull as close to draft day as possible).

**4. Update Player Projections (Hitters)**
* **File:** `HittersProj2026RAW.csv`
* **Action:** Export 'The Bat X' (Standard) batter projections from FanGraphs. Edit in Excel to match previous column structures.
* **Frequency:** Continuously updated (pull as close to draft day as possible).

**5. Update Player Projections (Pitchers)**
* **File:** `PitchersProj2026RAW.csv`
* **Action:** Export 'ATC' (Standard) pitcher projections from FanGraphs. Edit in Excel to match previous column structures.
* **Frequency:** Continuously updated (pull as close to draft day as possible).

**6. Apply Machine Learning Weights**
* **File:** `BigBoard.ipynb`
* **Action:** Define Weights by updating the Theta values (coefficients) derived from the ML models.
* **Frequency:** Once at the start of the new year.

**7. Finalize the Big Board**
* **File:** `BigBoard.ipynb`
* **Action:** Update the Roster Count in the ML Points Section to match the established Replacement Level (x 1.2).
* **Frequency:** Once per year.

## License
This project is licensed under the MIT License.
