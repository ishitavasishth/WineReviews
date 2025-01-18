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


🚀 Impact and Value
By leveraging SQL-based insights, this analysis supports data-driven decision-making in the wine industry. It provides strategies to increase profit margins, optimize product offerings, and target high-value customers, ultimately driving sustainable business growth.


1. Data Cleaning and Importing

-- Convert the price column to FLOAT for numerical analysis
ALTER TABLE wine
ALTER COLUMN price TYPE FLOAT;

-- Drop the existing table if it exists to avoid conflicts
DROP TABLE IF EXISTS wine;

-- Create the wine table with appropriate data types
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

-- Increase the description column size for longer texts
ALTER TABLE wine
ALTER COLUMN description TYPE VARCHAR(1000);

-- Convert description to TEXT for better storage of long descriptions
ALTER TABLE wine
ALTER COLUMN description TYPE TEXT;



