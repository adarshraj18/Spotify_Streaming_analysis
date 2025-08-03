Overview
This project involves analyzing a Spotify dataset with various attributes about tracks, albums, and artists using SQL. It covers an end-to-end process of normalizing a denormalized dataset, performing SQL queries of varying complexity (easy, medium, and advanced), and optimizing query performance. The primary goals of the project are to practice advanced SQL skills and generate valuable insights from the dataset.


Project Steps
1. Data Exploration
Before diving into SQL, it’s important to understand the dataset thoroughly. The dataset contains attributes such as:

Artist: The performer of the track.
Track: The name of the song.
Album: The album to which the track belongs.
Album_type: The type of album (e.g., single or album).
Various metrics such as danceability, energy, loudness, tempo, and more.

2. Querying the Data
Retrieving the names of all tracks that have more than 1 billion streams.
Listing down all albums along with their respective artists.
Getting the total number of comments for tracks where licensed = TRUE.
Finding out all tracks that belong to the album type single.
Counting the total number of tracks by each artist.
Calculating the average danceability of tracks in each album.
Finding the top 5 tracks with the highest energy values.
Listing down all tracks along with their views and likes where official_video = TRUE.
For each album, calculating the total views of all associated tracks.
Retrieving the track names that have been streamed on Spotify more than YouTube.
Find the top 3 most-viewed tracks for each artist using window functions.
Write a query to find tracks where the liveness score is above the average.
Use a WITH clause to calculate the difference between the highest and lowest energy values for tracks in each album.

3. Query Optimization
In advanced stages, the focus shifts to improving query performance. Some optimization strategies include:



Database: PostgreSQL
SQL Queries: DDL, DML, Aggregations, Joins, Subqueries, Window Functions
Tools: pgAdmin 4 (or any SQL editor), PostgreSQL (via Homebrew, Docker, or direct installation)
