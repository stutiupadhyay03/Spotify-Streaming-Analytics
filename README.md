# Spotify Global Streaming Charts Analysis

A large-scale data engineering and analytics project analyzing Spotify's global Top 200 and Viral 50 streaming charts using **Apache Spark** on **Databricks**. The project processes approximately 9 million records across global markets to uncover streaming trends, regional performance patterns, and artist collaboration networks.

**Authors:** Stuti Upadhyay and Philip Appiah | UMBC DATA603 — Platforms for Big Data Processing | December 2024

---

## Dataset

**Source:** Kaggle — Spotify Top 200 and Viral 50 Charts

- **Size:** 1.46 GB
- **Records:** ~9 million streaming records
- **Time period:** January 2020 to December 2021
- **Coverage:** Global Top 200 and Viral 50 charts across country-specific markets
- **Key fields:** Track name, artist, streams, chart position, country, date

---

## Key Findings

| Analysis | Finding |
|---|---|
| Top artist by total streams | The Weeknd — 9.88 billion streams |
| Top streaming market | United States, followed by Brazil and Mexico |
| Peak streaming day | Fridays and Saturdays consistently show highest stream counts |
| Rank vs. streams correlation | r = -0.13 (weak negative — chart position has limited predictive power over stream volume) |
| GraphFrames PageRank | Identified key hub artists in the global collaboration network |

---

## Analysis Structure

### 1. Data Ingestion and Pipeline Setup
- Load 1.46 GB Spotify dataset into Databricks
- Schema enforcement and validation
- Data type casting and null handling
- Partitioning strategy for efficient Spark processing

### 2. Exploratory Data Analysis
- Top 10 artists by total global streams
- Top 10 most-streamed tracks overall
- Country-level streaming volume comparison
- Distribution of stream counts across chart positions

### 3. Time-Series Streaming Trend Analysis
- Monthly streaming volume trends (Jan 2020 to Dec 2021)
- Pandemic-era streaming behavior analysis (2020 lockdown surge)
- Year-over-year comparison (2020 vs. 2021)

### 4. Regional Performance Analysis
- Country-by-country streaming volume ranking
- US dominance vs. emerging markets (Brazil, Mexico)
- Regional chart composition — Top 200 vs. Viral 50 breakdown by country

### 5. Day-of-Week Behavioral Analysis
- Average stream counts by day of week
- Friday and Saturday peak pattern identification
- Weekday vs. weekend streaming behavior

### 6. Rank vs. Streams Correlation
- Pearson correlation between chart rank and stream count
- r = -0.13 across full dataset
- Scatter plot with regression line

### 7. Track-Level Trend Analysis
- Deep dive on individual track performance over time
- Blinding Lights (The Weeknd) streaming trajectory analysis
- Chart longevity vs. initial peak performance

### 8. GraphFrames Artist Collaboration Network
- Build artist co-occurrence graph from shared chart appearances
- Apply GraphFrames **PageRank** algorithm to identify influential hub artists
- Visualize network centrality

---

## Technical Stack

| Component | Technology |
|---|---|
| Distributed compute | Apache Spark (PySpark) |
| Platform | Databricks |
| Query language | Spark SQL |
| Graph analytics | GraphFrames (PageRank) |
| Data manipulation | PySpark DataFrame API, Pandas |
| Visualization | Matplotlib, Seaborn |
| Language | Python 3 |

---

## Setup

This project was built and run on Databricks. To reproduce:

**Option 1 — Databricks (recommended)**
1. Upload the dataset to DBFS or mount an S3/Azure storage bucket
2. Import the notebook into your Databricks workspace
3. Attach to a cluster with GraphFrames installed (`com.databricks:graphframes:0.8.2-spark3.2-s_2.12`)
4. Run all cells

**Option 2 — Local (limited)**
```bash
pip install pyspark pandas matplotlib seaborn
# Note: GraphFrames requires a running Spark cluster and cannot be run locally without setup
```

---

## Repository Structure

```
Spotify-Streaming-Analytics/
├── Spotify_Streaming_Analysis.ipynb    # Full PySpark analysis notebook
├── Spotify_Streaming_Presentation.pptx # Project presentation slides
└── README.md
```

---

## Sample Spark Operations

```python
# Schema enforcement on load
schema = StructType([
    StructField("Position", IntegerType(), True),
    StructField("Track Name", StringType(), True),
    StructField("Artist", StringType(), True),
    StructField("Streams", LongType(), True),
    StructField("Date", DateType(), True),
    StructField("Region", StringType(), True),
])

# Top artists by total streams
top_artists = df.groupBy("Artist") \
    .agg(F.sum("Streams").alias("Total_Streams")) \
    .orderBy(F.desc("Total_Streams")) \
    .limit(10)

# Day-of-week analysis
df_with_day = df.withColumn("DayOfWeek", F.dayofweek("Date"))

# GraphFrames PageRank
graph = GraphFrame(vertices, edges)
pagerank_results = graph.pageRank(resetProbability=0.15, tol=0.01)
```

---

## Limitations

- Dataset limited to 2020-2021 — does not reflect post-pandemic streaming behavior
- Country coverage varies — some markets have incomplete chart data
- GraphFrames co-occurrence network uses chart co-appearance as a proxy for collaboration, not verified artist relationships
- Rank-streams correlation (r=-0.13) is weak, suggesting chart position alone is a poor predictor of streaming volume

---

## Stack

`PySpark` · `Databricks` · `GraphFrames` · `Spark SQL` · `Pandas` · `Matplotlib` · `Seaborn` · `Python`
