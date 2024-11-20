# 🎥 Netflix Movies and TV Shows Data Analysis Using SQL

![Netflix Logo](https://github.com/Ashokkumar5003/Netflix_SQL_Project/blob/main/logo.png)

## 📌 Project Overview  

This project uses SQL to analyze the **Netflix Movies and TV Shows dataset**, uncovering key insights into content distribution, ratings, genres, and trends. The goal is to extract actionable insights for content strategies, audience preferences, and regional trends.  


## 🗃️ Dataset  

The dataset for this project simulates Netflix's catalog, containing details like **type, title, director, cast, country, release year, ratings, and duration**.  

### Dataset Schema  

```sql  
CREATE TABLE netflix (  
    show_id      VARCHAR(6),  
    type         VARCHAR(10),  
    title        VARCHAR(150),  
    director     VARCHAR(208),  
    casts        VARCHAR(1000),  
    country      VARCHAR(150),  
    date_added   VARCHAR(150),  
    release_year INT,  
    rating       VARCHAR(10),  
    duration     VARCHAR(15),  
    listed_in    VARCHAR(100),  
    description  VARCHAR(250)  
);  
```  

## 📊 Analysis Objectives  

This analysis focuses on answering **15 business problems**, such as:  
1. Count the distribution of movies vs. TV shows.  
2. Identify the most common ratings.  
3. Explore regional content distribution.  
4. Highlight content trends by genre and duration.  
5. Detect patterns in director and actor appearances.  
6. Categorize content based on thematic keywords.  

## 🛠️ SQL Solutions  

### 1. **Count the Number of Movies vs. TV Shows**  
```sql  
SELECT type, COUNT(*) AS total_content  
FROM netflix  
GROUP BY type;  
```  
**Objective**: Understand the distribution of content types.  


### 2. **Find the Most Common Rating**  
```sql  
SELECT  
    type, rating  
FROM (  
    SELECT  
        type,  
        rating,  
        COUNT(*),  
        RANK() OVER(PARTITION BY type ORDER BY COUNT(*) DESC) AS ranking  
    FROM netflix  
    GROUP BY 1, 2  
) AS t1  
WHERE ranking = 1;  
```  
**Objective**: Discover the most frequent ratings.  


### 3. **List All Movies Released in 2020**  
```sql  
SELECT title, release_year  
FROM netflix  
WHERE release_year = 2020 AND type = 'Movie';  
```  
**Objective**: Filter movies by specific release years.  

### 4. **Top 5 Countries with the Most Content**  
```sql  
SELECT  
    UNNEST(STRING_TO_ARRAY(country, ',')) AS new_country,  
    COUNT(show_id) AS total_content  
FROM netflix  
GROUP BY 1  
ORDER BY 2 DESC  
LIMIT 5;  
```  
**Objective**: Identify regions dominating Netflix's catalog.  

### 5. **Identify the Longest Movie**  
```sql  
SELECT *  
FROM netflix  
WHERE type = 'Movie' AND duration = (SELECT MAX(duration) FROM netflix);  
```  
**Objective**: Highlight the longest movie by duration.  


### 6. **Find Movies Added in the Last Year**  
```sql  
SELECT type, date_added  
FROM netflix  
WHERE TO_DATE(date_added, 'Month DD, YYYY') >= CURRENT_DATE - INTERVAL '5 years' AND type = 'Movie';  
```  
**Objective**: Identify recently added movies.  


### 7. **Content by Director 'Rajiv Chilaka'**  
```sql  
SELECT *  
FROM netflix  
WHERE director ILIKE '%Rajiv Chilaka%';  
```  
**Objective**: List all movies/TV shows directed by a specific individual.  


### 8. **List All TV Shows with More Than 5 Seasons**  
```sql  
SELECT *  
FROM netflix  
WHERE type = 'TV Show' AND SPLIT_PART(duration, ' ', 1)::NUMERIC > 5;  
```  
**Objective**: Identify long-running TV shows.  


### 9. **Content Count by Genre**  
```sql  
SELECT  
    UNNEST(STRING_TO_ARRAY(listed_in, ',')) AS genre,  
    COUNT(show_id) AS total_content  
FROM netflix  
GROUP BY 1;  
```  
**Objective**: Count content across genres.  


### 10. **Top 5 Years by Average Content Released in India**  
```sql  
SELECT  
    EXTRACT(YEAR FROM TO_DATE(date_added, 'Month DD, YYYY')) AS year,  
    COUNT(*) AS yearly_content,  
    ROUND(COUNT(*)::NUMERIC / (SELECT COUNT(*) FROM netflix WHERE country = 'India')::NUMERIC * 100, 2) AS avg_content_per_year  
FROM netflix  
WHERE country = 'India'  
GROUP BY 1  
ORDER BY avg_content_per_year DESC  
LIMIT 5;  
```  
**Objective**: Analyze trends in Indian content production.  

### 11. **List All Documentaries**  
```sql  
SELECT title AS movies, listed_in AS genre  
FROM netflix  
WHERE listed_in ILIKE '%Documentaries%';  
```  
**Objective**: Filter documentaries from the dataset.  


### 12. **Find Content Without a Director**  
```sql  
SELECT show_id, title, director  
FROM netflix  
WHERE director IS NULL;  
```  
**Objective**: Highlight content lacking directorial information.  


### 13. **Count Salman Khan Movies in the Last 10 Years**  
```sql  
SELECT casts, release_year  
FROM netflix  
WHERE casts ILIKE '%Salman Khan%' AND release_year > EXTRACT(YEAR FROM CURRENT_DATE) - 10;  
```  
**Objective**: Determine Salman Khan's recent contributions.  


### 14. **Top 10 Actors in Indian Movies**  
```sql  
SELECT  
    UNNEST(STRING_TO_ARRAY(casts, ',')) AS actors,  
    COUNT(*) AS total_content  
FROM netflix  
WHERE country ILIKE '%India%'  
GROUP BY 1  
ORDER BY 2 DESC  
LIMIT 10;  
```  
**Objective**: Rank actors by appearances in Indian productions.  


### 15. **Categorize Content Based on Keywords ('Kill' and 'Violence')**  
```sql  
WITH new_table AS (  
    SELECT *,  
           CASE  
               WHEN description ILIKE '%kill%' OR description ILIKE '%violence%' THEN 'Bad Content'  
               ELSE 'Good Content'  
           END AS category  
    FROM netflix  
)  
SELECT category, COUNT(*) AS total_content  
FROM new_table  
GROUP BY 1;  
```  
**Objective**: Categorize content as "Good" or "Bad" based on keywords in the description.  


## 📈 Insights and Findings  

1. **Content Distribution**: Netflix has a balanced mix of TV shows and movies.  
2. **Regional Trends**: India, the USA, and Japan contribute significantly to the catalog.  
3. **Popular Ratings**: TV-MA and PG are frequent, reflecting a mix of mature and family-friendly content.  
4. **Content Categorization**: Thematic analysis reveals "Good" and "Bad" content patterns.  
5. **Notable Contributors**: Directors and actors like Salman Khan play significant roles in Netflix’s offerings.  


## 👤 Author  

**Ashok Kumar S**  
- GitHub   : [Ashokkumar5003](https://github.com/Ashokkumar5003)  
- Email Id : ashokit1012@gmail.com  
- Mobile No: 6374517308
