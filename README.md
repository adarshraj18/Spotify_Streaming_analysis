# **Spotify Streaming Data Analysis (Advanced SQL)**

## **Overview**

This project explores global music streaming trends using advanced SQL techniques. By analyzing a dataset of tracks, artists, and platform engagement metrics, the project uncovers insights into listener behavior, platform preferences, and track performance.

---

## **Objectives**

* Extract key performance indicators (KPIs) from streaming data.
* Analyze patterns in artist popularity, track characteristics, and platform dominance.
* Apply advanced SQL techniques (Window Functions, CTEs, Indexing) to derive actionable insights.
* Optimize query performance for large-scale datasets.

---

## **Dataset**

The dataset contains **25+ attributes**, including:

* **Artist, Track, Album, Album Type**
* **Streams, Views, Likes, Comments**
* **Audio Features**: Danceability, Energy, Loudness, Acousticness, Liveness, Valence, Tempo
* **Platform Indicators**: Most Played Platform (Spotify vs YouTube), Official Video Flag

---

## **Key Analyses & SQL Techniques**

1. **Exploratory Data Analysis (EDA)**

   * Count of tracks, albums, and artists.
   * Track duration checks and data cleaning.

2. **KPIs & Insights**

   * Top 3 most-viewed tracks per artist *(Window Functions)*
   * Platform segmentation: Spotify vs YouTube streams.
   * Tracks with liveness above average.
   * Energy variation across albums *(CTEs)*.
   * Danceability trends by album.

3. **Performance Optimization**

   * Indexed artist column to reduce execution time by **85%**.

---

## **Sample Queries**

```sql
-- Top 3 most-viewed tracks per artist
WITH ranking_artist AS (
    SELECT
        artist,
        track,
        SUM(views) AS total_view,
        DENSE_RANK() OVER (PARTITION BY artist ORDER BY SUM(views) DESC) AS rank
    FROM spotify
    GROUP BY artist, track
)
SELECT * FROM ranking_artist
WHERE rank <= 3;
```

---

## **Future Enhancements**

* Create a **Power BI Dashboard** to visualize KPIs and trends.
* Implement a **Recommendation Engine** based on audio features.
* Automate data ingestion for real-time streaming insights.

---

## **Tools & Skills Used**

* **SQL**: Aggregations, Window Functions, CTEs, Indexing, Query Optimization.
* **Data Analysis Concepts**: KPIs, Trend Analysis, Platform Segmentation.
* (Upcoming) **Power BI / Excel Dashboard** for visualization.

---

## **Conclusion**

The analysis highlights how advanced SQL can uncover valuable insights from streaming data, enabling music platforms and record labels to optimize playlists, target marketing strategies, and enhance listener engagement.

---

