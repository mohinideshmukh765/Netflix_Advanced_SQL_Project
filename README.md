# 📺 Netflix Data Analysis Project (PostgreSQL)
![Netflix_Logo]{https://github.com/mohinideshmukh765/Netflix_Advanced_SQL_Project/blob/main/Netflix_Logo.png}

📌 Overview

This project focuses on analyzing the Netflix dataset using PostgreSQL. The goal is to extract meaningful insights from raw CSV data by performing data cleaning, transformation, and SQL-based analysis.

The dataset contains information about movies and TV shows available on Netflix, including attributes such as title, director, cast, country, release year, rating, duration, and genre.

🛠️ Tech Stack

Database: PostgreSQL

Tool: pgAdmin

Language: SQL

📂 Dataset

File: netflix_titles.csv  Link: https://www.kaggle.com/datasets/shivamb/netflix-shows

Contains:

Movies and TV Shows

Metadata such as cast, director, genre, duration, and release details

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
⚠️ Data Import Issue & Solution
Problem:

Direct CSV import in pgAdmin failed due to:

File path issues

Encoding or delimiter mismatch

Solution:

Used COPY command for importing data:

COPY netflix(show_id, type, title, director, casts, country, date_added, release_year, rating, duration, listed_in, description)
FROM 'C:\\Users\\Mohin\\OneDrive\\Desktop\\Placements\\Projects\\Netflix\\netflix_titles.csv'
DELIMITER ','
CSV HEADER
QUOTE '"'
NULL '';
📊 Business Problems Solved
1. Count Movies vs TV Shows

Analyzed distribution of content types.

2. Most Common Rating

Used window functions (RANK()) to identify most frequent ratings.

3. Movies Released in 2020

Filtered dataset by release year.

4. Top 5 Countries with Most Content

Split multi-country entries using STRING_TO_ARRAY() and UNNEST().

5. Longest Movie

Extracted numeric duration and sorted results.

6. Content Added in Last 5 Years

Converted text date using TO_DATE().

7. Content by Specific Director

Handled multiple directors using array unnesting.

8. TV Shows with More Than 5 Seasons

Parsed duration field.

9. Content Count by Genre

Extracted genres from comma-separated values.

10. Year-wise Content Release in India

Calculated percentage contribution per year.

11. Documentary Movies

Used LIKE filter on genre.

12. Content Without Director

Identified missing values.

13. Movies Featuring Salman Khan (Last 10 Years)

Filtered using LIKE and dynamic year calculation.

14. Top 10 Actors in Indian Content

Extracted actors using UNNEST() and counted frequency.

15. Content Categorization (Violence/Kill Keywords)

Classified content into “Good” and “Bad” using CASE.

🔍 Key SQL Concepts Used

Aggregate Functions (COUNT, MAX)

Window Functions (RANK)

String Functions (STRING_TO_ARRAY, SPLIT_PART)

Date Functions (TO_DATE, CURRENT_DATE)

Conditional Logic (CASE)

Subqueries and CTEs

Data Cleaning Techniques

🚀 Key Learnings

Handling real-world messy data (comma-separated fields)

Working with CSV imports in PostgreSQL

Writing optimized SQL queries for analytics

Using advanced SQL functions for transformation
