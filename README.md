# blinkit-analysis
BlinkIT Grocery Sales Analysis using SQL, Python, and Power BI

Conducted end-to-end data analysis on 8,523 retail records
Cleaned and transformed data using SQL (handling nulls, standardization)
Developed KPI-based analysis (Sales, Ratings, Item Count)
Built interactive dashboard in Power BI with filters and visual insights
Identified top-performing categories and outlet segments


sql quries 
-- =========================
-- 1. VIEW DATA
-- =========================
SELECT * FROM grocery_sales;


-- =========================
-- 2. DATA CLEANING
-- =========================

-- Standardize Fat Content
UPDATE grocery_sales
SET Item_Fat_Content = 
    CASE 
        WHEN Item_Fat_Content IN ('LF', 'low fat') THEN 'Low Fat'
        WHEN Item_Fat_Content = 'reg' THEN 'Regular'
        ELSE Item_Fat_Content
    END;

-- Verify Cleaning
SELECT DISTINCT Item_Fat_Content FROM grocery_sales;


-- =========================
-- 3. KPI QUERIES
-- =========================

-- 3.1 Total Sales (in Millions)
SELECT ROUND(SUM(Sales)/1000000, 2) AS Total_Sales_Million
FROM grocery_sales;

-- 3.2 Average Sales
SELECT ROUND(AVG(Sales), 0) AS Avg_Sales
FROM grocery_sales;

-- 3.3 Number of Items
SELECT COUNT(*) AS No_of_Items
FROM grocery_sales;

-- 3.4 Average Rating
SELECT ROUND(AVG(Rating), 2) AS Avg_Rating
FROM grocery_sales;


-- =========================
-- 4. ANALYSIS QUERIES
-- =========================

-- 4.1 Sales by Fat Content
SELECT 
    Item_Fat_Content, 
    ROUND(SUM(Sales), 2) AS Total_Sales
FROM grocery_sales
GROUP BY Item_Fat_Content
ORDER BY Total_Sales DESC;


-- 4.2 Sales by Item Type
SELECT 
    Item_Type, 
    ROUND(SUM(Sales), 2) AS Total_Sales
FROM grocery_sales
GROUP BY Item_Type
ORDER BY Total_Sales DESC;


-- 4.3 Fat Content by Outlet Location (Pivot Style)
SELECT 
    Outlet_Location_Type,
    SUM(CASE WHEN Item_Fat_Content = 'Low Fat' THEN Sales ELSE 0 END) AS Low_Fat,
    SUM(CASE WHEN Item_Fat_Content = 'Regular' THEN Sales ELSE 0 END) AS Regular
FROM grocery_sales
GROUP BY Outlet_Location_Type
ORDER BY Outlet_Location_Type;


-- 4.4 Sales by Outlet Establishment Year
SELECT 
    Outlet_Establishment_Year, 
    ROUND(SUM(Sales), 2) AS Total_Sales
FROM grocery_sales
GROUP BY Outlet_Establishment_Year
ORDER BY Outlet_Establishment_Year;


-- 4.5 Percentage of Sales by Outlet Size
SELECT 
    Outlet_Size,
    ROUND(SUM(Sales), 2) AS Total_Sales,
    ROUND(SUM(Sales) * 100 / (SELECT SUM(Sales) FROM grocery_sales), 2) AS Sales_Percentage
FROM grocery_sales
GROUP BY Outlet_Size
ORDER BY Total_Sales DESC;


-- 4.6 Sales by Outlet Location
SELECT 
    Outlet_Location_Type,
    ROUND(SUM(Sales), 2) AS Total_Sales
FROM grocery_sales
GROUP BY Outlet_Location_Type
ORDER BY Total_Sales DESC;


-- 4.7 All Metrics by Outlet Type
SELECT 
    Outlet_Type,
    ROUND(SUM(Sales), 2) AS Total_Sales,
    ROUND(AVG(Sales), 0) AS Avg_Sales,
    COUNT(*) AS No_of_Items,
    ROUND(AVG(Rating), 2) AS Avg_Rating,
    ROUND(AVG(Item_Visibility), 2) AS Avg_Visibility
FROM grocery_sales
GROUP BY Outlet_Type
ORDER BY Total_Sales DESC;



python load and exploration

import pandas as pd

df = pd.read_csv('BlinkIT_Clean.csv')

print(df.head())     # Display first 5 rows
print(df.shape)      # Check dataset size (rows, columns)
print(df.columns)    # View all column names
print(df.dtypes)     # Check data types of each column









