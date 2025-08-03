-- Adavanced SQL Project -- Spotify Datasets
-- create table
DROP TABLE IF EXISTS spotify;
CREATE TABLE spotify (
    artist VARCHAR(255),
    track VARCHAR(255),
    album VARCHAR(255),
    album_type VARCHAR(50),
    danceability FLOAT,
    energy FLOAT,
    loudness FLOAT,
    speechiness FLOAT,
    acousticness FLOAT,
    instrumentalness FLOAT,
    liveness FLOAT,
    valence FLOAT,
    tempo FLOAT,
    duration_min FLOAT,
    title VARCHAR(255),
    channel VARCHAR(255),
    views FLOAT,
    likes BIGINT,
    comments BIGINT,
    licensed BOOLEAN,
    official_video BOOLEAN,
    stream BIGINT,
    energy_liveness FLOAT,
    most_played_on VARCHAR(50)
);
SELECT * FROM spotify

-- EDA
SELECT COUNT(*) FROM spotify;
SELECT COUNT(DISTINCT artist) FROM spotify;
SELECT COUNT(DISTINCT album) FROM spotify;
SELECT DISTINCT album_type FROM spotify;
SELECT duration_min FROM spotify;
SELECT MAX(duration_min) FROM spotify;
SELECT MIN(duration_min) FROM spotify;
SELECT * FROM spotify
WHERE duration_min = 0;
DELETE FROM spotify
WHERE duration_min = 0;
SELECT DISTINCT channel FROM spotify;
SELECT DISTINCT most_played_on FROM spotify;

---------------------------------------------------------------------
---------------------------------------------------------------------

/* Retrieving the names of all tracks that have more than 1 billion streams.

Listing down all albums along with their respective artists.

Getting the total number of comments for tracks where licensed = TRUE.

Finding out all tracks that belong to the album type single.

Counting the total number of tracks by each artist. */

----------------------------------------------------------------------
----------------------------------------------------------------------
--Retrieving the names of all tracks that have more than 1 billion streams.

SELECT * FROM spotify
WHERE stream > 1000000000

-- Listing down all albums along with their respective artists.
SELECT
	album, artist
FROM spotify;

SELECT
	DISTINCT album
FROM spotify
ORDER BY 1 ;

--Getting the total number of comments for tracks where licensed = TRUE.

SELECT DISTINCT licensed FROM spotify
SELECT * FROM spotify
WHERE licensed = 'true' ;

SELECT
	SUM(comments) as total_comments
FROM spotify
WHERE licensed = 'true' ;

--Finding out all tracks that belong to the album type single.
SELECT * FROM spotify
WHERE album_type = 'single' ;

--Counting the total number of tracks by each artist.

SELECT 
	artist,
	COUNT(*) as total_number_of_songs
FROM spotify
GROUP BY artist
ORDER BY 2;
---------------------------------------------------------------------
---------------------------------------------------------------------
/*
Calculating the average danceability of tracks in each album.

Finding the top 5 tracks with the highest energy values.

Listing down all tracks along with their views and likes where official_video = TRUE.

For each album, calculating the total views of all associated tracks.

Retrieving the track names that have been streamed on Spotify more than YouTube.
*/
---------------------------------------------------------------------
---------------------------------------------------------------------
--Calculating the average danceability of tracks in each album.
SELECT
	album,
	avg(danceability) as avg_danceability
FROM spotify
GROUP BY 1
ORDER BY 2 DESC;

--Finding the top 5 tracks with the highest energy values.

SELECT 
	DISTINCT track
FROM spotify;

SELECT
	track,
	MAX(energy)
FROM spotify
GROUP BY 1
ORDER BY 2 DESC
LIMIT 5;

--Listing down all tracks along with their views and likes where official_video = TRUE.
SELECT
	track,
	SUM(views) as total_views,
	SUM(likes) as total_likes
FROM spotify
WHERE official_video = 'true'
GROUP BY 1
ORDER BY 2 DESC
LIMIT 5;

--For each album, calculating the total views of all associated tracks.
SELECT
	album,
	track,
	SUM(views)
FROM spotify
GROUP BY 1, 2;

--Retrieving the track names that have been streamed on Spotify more than YouTube.
SELECT*FROM
(SELECT
	track,
	
	--most_played_on,
	COALESCE(SUM(CASE WHEN most_played_on = 'Youtube' THEN stream END),0)  as streamed_on_youtube,
	COALESCE(SUM(CASE WHEN most_played_on = 'Spotify' THEN stream END),0)  as streamed_on_spotify
FROM spotify
GROUP BY 1
) as t1
WHERE 
	streamed_on_spotify > streamed_on_youtube
	AND
	streamed_on_youtube <> 0;

---------------------------------------------------------------------
---------------------------------------------------------------------
/*Find the top 3 most-viewed tracks for each artist using window functions.

Write a query to find tracks where the liveness score is above the average.

Use a WITH clause to calculate the difference between the highest and lowest energy values for tracks in each album.*/
---------------------------------------------------------------------
---------------------------------------------------------------------
--Find the top 3 most-viewed tracks for each artist using window functions.
--Each artist and total view of each track
--Track the highest view for each artist(we need top)
-- Dense rank
WITH ranking_artist
AS
(SELECT
	artist,
	track,
	SUM(views) as total_view,
	DENSE_RANK() OVER(PARTITION BY artist ORDER BY SUM(views) DESC) as rank
FROM spotify
GROUP BY 1, 2
ORDER BY 1, 3 DESC)
SELECT * FROM ranking_artist
WHERE rank <= 3;

--Write a query to find tracks where the liveness score is above the average.

SELECT
	track,
	artist,
	liveness
FROM spotify
WHERE liveness > (SELECT AVG(liveness) FROM spotify);

--Use a WITH clause to calculate the difference between the 
--highest and lowest energy values for tracks in each album

WITH cte
AS
(SELECT
	album,
	MAX(energy) as highest_energy,
	MIN(energy)as lowest_energy
FROM spotify
GROUP BY 1)

SELECT 
	album,
	highest_energy - lowest_energy as energy_difference
FROM cte
ORDER BY 2 DESC;

--Query Optimization

EXPLAIN ANALYZE --Execution Time 12.109 ms Planning time 0.446 ms
SELECT
	artist,
	track,
	views
FROM spotify
WHERE artist = 'Gorillaz'
	AND
	most_played_on = 'Youtube'
ORDER BY stream DESC LIMIT 25
------------------------------------------------------------------
CREATE INDEX artist_index ON spotify(artist); --Execution Time 0.072 ms Planning time 0.304 ms





	

