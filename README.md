# LEGO Value Engine

The LEGO Value Engine is an end-to-end analytics pipeline built on Databricks Free Edition that helps collectors mathematically identify the best-value LEGO sets. Utilizing a Medallion Architecture, Spark SQL, and PySpark KMeans clustering, the engine scores and segments the LEGO catalog based on price, piece counts, and community ratings. The final result is an interactive Databricks Dashboard and Genie AI space where collectors can visually explore the data and ask questions to get set recommendations.

Built for the DAIS 2026 Community Virtual Challenge.

## The Problem

When people evaluate LEGO sets, they usually look at one factor at a time:

- Price
- Piece Count
- Average Rating
- Number of Reviews

The challenge is that none of these metrics tell the full story on their own. A set might have a great rating but be extremely expensive. Another might have thousands of pieces but poor reviews. This engine evaluates all of these factors together to identify which sets provide the most value for the money.

## Pipeline Architecture

The project follows the Medallion Architecture with Bronze, Silver, and Gold layers, all backed by Delta Lake.

### Bronze Layer
Stores the raw LEGO catalog dataset exactly as received as a Delta table. This acts as the immutable source of truth for the entire pipeline.

### Silver Layer
Cleans the data and calculates three custom scoring metrics using Spark SQL:

- **Smart Value Index** = (Piece Count x Average Rating) / Price — the primary metric, estimating how much value a collector receives per dollar spent
- **Collector Potential Score** = (Average Rating x Number of Reviews) / Price — highlights sets with broad community consensus, not just a few high ratings
- **Price Efficiency** = Piece Count / Price — pure bricks per dollar

### Gold Layer
Generates final insights including:

- Theme rankings based on average value index
- The top 10 most undervalued sets in the catalog
- A premium segment of sets priced over $200 that justify their price through strong community ratings

## Databricks Features Used

- Medallion Architecture
- Delta Lake
- Databricks SQL
- Delta Time Travel
- PySpark Machine Learning (KMeans clustering)
- Databricks AI/BI Dashboards
- Databricks Genie AI

## Delta Time Travel

LEGO prices change frequently due to promotions and holiday sales. By querying historical versions of the Delta tables, a user can compare previous and current prices and observe exactly how a price change impacts a set's value score over time.

## Machine Learning

A PySpark KMeans clustering model automatically groups the catalog based on price, ratings, piece count, and overall value. Instead of relying on manual categories, the model discovers four distinct market segments from the data:

| Segment | Description |
|---|---|
| Premium Collector | Flagship sets with massive piece counts and near-perfect ratings |
| High Value | Hidden gems that over-deliver on value relative to cost |
| Casual Buyers | Standard mid-range sets that form the bulk of the catalog |
| Budget-Friendly | Accessible low-cost sets with solid community approval |

## Dashboard and Genie AI

The Databricks Dashboard allows users to explore which themes provide the most value, analyze the relationship between price and ratings, and view the machine learning segments across the catalog.

The Gold tables are also connected to Databricks Genie AI, so users can ask plain English questions like "Show me the top 5 undervalued sets under $100" and immediately receive set recommendations without writing any SQL.

## Key Findings

- Star Wars and Harry Potter, two of LEGO's most marketed themes, both rank near the bottom of the value index
- Central Perk at $59.99 scores 85 on the value index while the Colosseum at $549.99 scores 75 — a $60 set mathematically beats a $550 set
- Four of the top five undervalued sets under $100 are botanical sets that receive very little marketing attention

## Getting Started

1. Upload `lego_data.xlsx` to Databricks using the Data Ingestion tool (Catalog → Add Data → Create or Modify Table)
2. Open `Lego_Value_Recommendations.ipynb` in a Databricks notebook
3. Attach a cluster and run all cells top to bottom

## Repository Contents

- `Lego_Value_Recommendations.ipynb` — the full Databricks notebook containing all pipeline code
- `lego_data.xlsx` — the raw LEGO catalog dataset used in the pipeline
