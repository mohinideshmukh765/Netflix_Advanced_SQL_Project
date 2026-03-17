📺 Netflix Data Analysis Project (PostgreSQL)

![Netflix_Logo](https://github.com/mohinideshmukh765/Netflix_Advanced_SQL_Project/blob/main/Netflix_Logo.png)

##📌 Overview
This project analyzes the Netflix dataset using SQL in PostgreSQL. It focuses on extracting meaningful insights from raw CSV data by performing data cleaning, transformation, and analytical querying. The dataset includes information about movies and TV shows such as title, director, cast, country, release year, rating, and genre.

##🛠️ Tech Stack
###Database: PostgreSQL
###Tool: pgAdmin
###Language: SQL

##📂 Dataset
###File: netflix_titles.csv
###Source: Kaggle – Netflix Movies and TV Shows Dataset
###Link: https://www.kaggle.com/datasets/shivamb/netflix-shows

##Description:
The dataset contains:
Movies and TV Shows available on Netflix
Metadata fields:
Title, Director, Cast
Country, Release Year
Rating, Duration
Genre (listed_in)

##Description
🧱 Database Schema
CREATE TABLE netflix ( 
    show_id VARCHAR(6),
    type VARCHAR(10),    
    title VARCHAR(150),
    director VARCHAR(208),
    casts VARCHAR(1000),
    country VARCHAR(150),
    date_added VARCHAR(50),
    release_year INT,
    rating VARCHAR(10),
    duration VARCHAR(10),
    listed_in VARCHAR(100),
    description VARCHAR(250)
);


###Used PostgreSQL COPY command:
COPY netflix(show_id, type, title, director, casts, country, date_added, release_year, rating, duration, listed_in, description)
FROM 'C:\\Users\\Mohin\\OneDrive\\Desktop\\Placements\\Projects\\Netflix\\netflix_titles.csv'
DELIMITER ','
CSV HEADER
QUOTE '"'
NULL '';


##📊 Business Problems & SQL Queries

1. Count Movies vs TV Shows
SELECT type, COUNT(*) AS total_content
FROM netflix
GROUP BY type;

2. Most Common Rating per Type
WITH RatingCounts AS (
    SELECT type, rating, COUNT(*) AS rating_count
    FROM netflix
    GROUP BY type, rating
),
RankedRatings AS (
    SELECT type, rating, rating_count,
           RANK() OVER (PARTITION BY type ORDER BY rating_count DESC) AS rank
    FROM RatingCounts
)
SELECT type, rating AS most_frequent_rating
FROM RankedRatings
WHERE rank = 1;

3. Movies Released in 2020
SELECT * 
FROM netflix
WHERE release_year = 2020;

4. Top 5 Countries with Most Content
SELECT *
FROM (
    SELECT UNNEST(STRING_TO_ARRAY(country, ',')) AS country,
           COUNT(*) AS total_content
    FROM netflix
    GROUP BY 1
) AS t1
WHERE country IS NOT NULL
ORDER BY total_content DESC
LIMIT 5;

5. Longest Movie
SELECT *
FROM netflix
WHERE type = 'Movie'
ORDER BY SPLIT_PART(duration, ' ', 1)::INT DESC;

6. Content Added in Last 5 Years
SELECT *
FROM netflix
WHERE TO_DATE(date_added, 'Month DD, YYYY') >= CURRENT_DATE - INTERVAL '5 years';

7. Content by Director 'Rajiv Chilaka'
SELECT *
FROM (
    SELECT *, UNNEST(STRING_TO_ARRAY(director, ',')) AS director_name
    FROM netflix
) AS t
WHERE director_name = 'Rajiv Chilaka';

8. TV Shows with More Than 5 Seasons
SELECT *
FROM netflix
WHERE type = 'TV Show'
AND SPLIT_PART(duration, ' ', 1)::INT > 5;

9. Content Count by Genre
SELECT UNNEST(STRING_TO_ARRAY(listed_in, ',')) AS genre,
       COUNT(*) AS total_content
FROM netflix
GROUP BY 1;

10. Year-wise Content Release in India
SELECT country, release_year,
       COUNT(show_id) AS total_release,
       ROUND(
           COUNT(show_id)::numeric /
           (SELECT COUNT(show_id) FROM netflix WHERE country = 'India')::numeric * 100, 2
       ) AS avg_release
FROM netflix
WHERE country = 'India'
GROUP BY country, release_year
ORDER BY avg_release DESC
LIMIT 5;

11. Documentary Movies
SELECT *
FROM netflix
WHERE listed_in LIKE '%Documentaries';

12. Content Without Director
SELECT *
FROM netflix
WHERE director IS NULL;

13. Movies Featuring Salman Khan (Last 10 Years)
SELECT *
FROM netflix
WHERE casts LIKE '%Salman Khan%'
AND release_year > EXTRACT(YEAR FROM CURRENT_DATE) - 10;


14. Top 10 Actors in Indian Content
SELECT UNNEST(STRING_TO_ARRAY(casts, ',')) AS actor,
       COUNT(*)
FROM netflix
WHERE country = 'India'
GROUP BY actor
ORDER BY COUNT(*) DESC
LIMIT 10;

15. Content Categorization (Violence/Kill Keywords)
SELECT category, COUNT(*) AS content_count
FROM (
    SELECT CASE 
        WHEN description ILIKE '%kill%' OR description ILIKE '%violence%' THEN 'Bad'
        ELSE 'Good'
    END AS category
    FROM netflix
) AS categorized_content
GROUP BY category;


🔍 Key SQL Concepts Used
Aggregate Functions (COUNT)
Window Functions (RANK)
String Functions (STRING_TO_ARRAY, SPLIT_PART)
Date Functions (TO_DATE, CURRENT_DATE)
Conditional Logic (CASE)
CTEs (Common Table Expressions)
Data Cleaning & Transformation

