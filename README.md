# Wine Market Analysis Project

📊 **Project Overview**
This project focuses on performing a comprehensive analysis of wine data to uncover actionable insights that can maximize profits and identify high-performing customers. Using SQL for data manipulation and querying, this project explores trends in wine pricing, customer preferences, and product performance to guide strategic business decisions.

🎯 **Objectives**
Increase Profitability: Identify the best-performing wines, regions, and wineries to optimize sales strategies.
Customer Segmentation: Discover high-value customer segments to enable personalized marketing and upselling opportunities.
Data-Driven Insights: Analyze wine ratings, pricing strategies, and production trends to support growth initiatives.
Product Optimization: Determine the most popular and profitable wine varieties for better inventory and marketing decisions.


🛠 **Tech Stack**
Database: PostgreSQL
Query Language: SQL
Data Source: Cleaned and structured wine dataset


🔍 **Key Analyses Conducted**
Descriptive Analysis: Identify top-performing wineries, best-selling wine varieties, and regional performance.
Pricing Strategy Evaluation: Analyze pricing distribution, average price trends, and premium pricing opportunities.
Customer Insights: Segment customers based on preferences and purchase behaviors.
Profitability Analysis: Discover high-margin products and evaluate factors impacting sales performance.
Advanced Analytics: Correlation analysis between wine ratings and pricing, and product diversification impact.


💡 **Business Questions Answered**
Which wine varieties are driving the most sales and revenue?
How does wine pricing vary across countries, regions, and wine ratings?
Which wineries consistently produce high-rated and profitable wines?
What are the most profitable customer segments based on purchasing behavior?
How can product diversification be optimized to increase profitability?


🚀 **Impact and Value**
By leveraging SQL-based insights, this analysis supports data-driven decision-making in the wine industry. It provides strategies to increase profit margins, optimize product offerings, and target high-value customers, ultimately driving sustainable business growth.


1.
-- Convert the price column to FLOAT for numerical analysis
```sql
ALTER TABLE wine
ALTER COLUMN price TYPE FLOAT;
```

-- Drop the existing table if it exists to avoid conflicts
```sql
DROP TABLE IF EXISTS wine;
```
-- Create the wine table with appropriate data types
```sql
CREATE TABLE wine (
    country VARCHAR(50),
    description VARCHAR(500),
    designation VARCHAR(100),
    points INT,
    price FLOAT,
    province VARCHAR(400),
    region_1 VARCHAR(50),
    region_2 VARCHAR(50),
    variety VARCHAR(100),
    winery VARCHAR(100)
);
```
-- Increase the description column size for longer texts
```sql
ALTER TABLE wine
ALTER COLUMN description TYPE VARCHAR(1000);
```
-- Convert description to TEXT for better storage of long descriptions
```sql
ALTER TABLE wine
ALTER COLUMN description TYPE TEXT;
```

2. **Exploratory Data Analysis (EDA)**

-- Total number of rows in the dataset
```sql
SELECT COUNT(*) FROM wine;
```
-- Number of unique countries in the dataset
```sql
SELECT COUNT(DISTINCT country) FROM wine;
```
-- Number of distinct provinces
```sql
SELECT COUNT(DISTINCT province) FROM wine;
```
-- Number of different wine varieties
```sql
SELECT COUNT(DISTINCT variety) FROM wine;
```
-- Number of unique wineries
```sql
SELECT COUNT(DISTINCT winery) FROM wine;
```
-- Maximum and minimum wine prices
```sql
SELECT MAX(price) FROM wine;
SELECT MIN(price) FROM wine;
```

 3.**Data Quality Check**

    -- Check for NULL values in the country column
```sql
SELECT country FROM wine WHERE country IS NULL;
```
-- Check for NULL values in the description column
```sql
SELECT description FROM wine WHERE description IS NULL;
```
-- Check for NULL values in the price column
```sql
SELECT price FROM wine WHERE price IS NULL;
```

4. **Business Questions and Analysis**

   -- Which wine variety has the highest number of sales?
```sql
SELECT variety, COUNT(*) AS frequency, AVG(price) AS avg_price, AVG(points) AS avg_points
FROM wine
GROUP BY variety
ORDER BY frequency DESC;
```
-- What is the average price of wines by country?
```sql
SELECT country, AVG(price) AS avg_price
FROM wine
GROUP BY country;
```
-- Which province produces the most wines in the dataset?
```sql
SELECT province, COUNT(*) AS count_of_wine
FROM wine
WHERE province IS NOT NULL
GROUP BY province
ORDER BY count_of_wine DESC;
```
-- What is the most common price point for wines?
```sql
SELECT price, COUNT(*) AS price_count
FROM wine
WHERE price IS NOT NULL
GROUP BY price
ORDER BY price_count DESC;
```

-- Which combination of region and variety generates the highest average price?
SELECT region_1, variety, AVG(price) AS avg_price
FROM wine
GROUP BY region_1, variety
ORDER BY avg_price DESC
LIMIT 1;

-- The combination is "Montrachet" and "Chardonnay" with an avg_price of 601.18
```sql

-- How does wine pricing vary across different points (ratings)?
SELECT points, AVG(price)
FROM wine
WHERE price IS NOT NULL
GROUP BY points
ORDER BY points DESC;
```

-- Identify the provinces where wines are highly rated and priced above average.
```sql
SELECT province, AVG(points) AS avg_points
FROM wine
WHERE points > 90 AND price > (SELECT AVG(price) FROM wine)
GROUP BY province
ORDER BY avg_points DESC;
```
-- Points above 90 are considered highly rated in this analysis
Advanced Level
```sql
-- What is the correlation between wine price and rating across different varieties?
SELECT variety, CORR(price, points)
FROM wine
WHERE price IS NOT NULL AND points IS NOT NULL
GROUP BY variety;
```
-- This shows if higher prices are linked to better ratings
```sql
-- Which regions show potential for premium pricing despite lower wine ratings?
SELECT region_1, 
       COUNT(*) AS total_wines, 
       COUNT(CASE WHEN price > 200 AND points < 90 THEN 1 END) AS high_price_low_rating,
       ROUND((COUNT(CASE WHEN price > 200 AND points < 90 THEN 1 END) * 100.0) / COUNT(*), 2) AS high_price_low_rating_percentage,
       RANK() OVER (ORDER BY (COUNT(CASE WHEN price > 200 AND points < 90 THEN 1 END) * 1.0) / COUNT(*) DESC) AS rank
FROM wine
WHERE region_1 IS NOT NULL
GROUP BY region_1
ORDER BY high_price_low_rating_percentage DESC;
```
-- "Pompeiano" is ranked the highest for premium pricing potential


-- Which wineries have the highest potential for upselling based on their product mix and pricing trends?
```sql
SELECT winery, 
       COUNT(DISTINCT variety) AS variety_count, 
       AVG(price) AS avg_price, 
       (MAX(price) - MIN(price)) AS price_gap,
       RANK() OVER (ORDER BY COUNT(DISTINCT variety) * (MAX(price) - MIN(price)) DESC) AS rank
FROM wine
WHERE price IS NOT NULL AND winery IS NOT NULL
GROUP BY winery
HAVING (MAX(price) - MIN(price)) > 2000
ORDER BY rank;
```
-- Château Latour has the highest upselling potential based on variety and pricing


5.**Key Insights**
Top-Selling Wine Variety: Chardonnay is the most sold and highly rated variety.

Top Producing Province: California leads wine production.

Premium Pricing Potential: Pompeiano shows potential for premium pricing despite lower ratings.

Upselling Opportunities: Château Latour leads in upselling potential due to product variety and pricing.
