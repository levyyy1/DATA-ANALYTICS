🍕 Pizza Sales Performance Analysis (SQL Server + Excel)

An end-to-end data analysis project that turns 150,000+ raw pizza order
records into actionable business insights — using SQL Server for data 
storage and querying, and **Excel** for KPI reporting and dashboard visualization.


 📌 Project Overview

The goal of this project was to analyze a pizzeria's transactional sales data to answer key business questions:

- How much revenue and how many orders are we generating?
- When are our busiest days and hours?
- Which pizza categories, sizes, and products sell best (and worst)?
- How can this data support decisions on staffing, inventory, and menu strategy?


 🛠️ Tools & Tech Stack

| Tool | Purpose |
| SQL Server Management Studio (SSMS) | Database creation, table design, data import, querying |
| T-SQL | Aggregation, filtering, and ranking queries for KPIs and trends |
| Microsoft Excel | PivotTables, PivotCharts, and dashboard design |

---

## 🗄️ Data

- **Source:** Raw pizza order-level transactional data (order ID, pizza ID, quantity, price, date, time, size, category, ingredients, pizza name)
- **Volume:** 150,000+ rows imported into a SQL Server table (`pizza_sales`)
- **Process:** Created the database and table schema in SSMS, imported the raw dataset, then queried it directly for analysis before visualizing results in Excel.

---

## 📈 Key Performance Indicators

| Metric | Value |
|---|---|
| Total Revenue | **$817,860.05** |
| Total Orders | **21,350** |
| Total Pizzas Sold | **49,574** |
| Average Order Value | **$38.31** |
| Average Pizzas per Order | **2.32** |

---

## 🔍 SQL Queries Used

All queries were written and executed in SQL Server Management Studio against the `pizza_sales` table.

**KPIs**
```sql
-- Total Revenue
SELECT SUM(total_price) AS TOTAL_REVENUE FROM pizza_sales;

-- Average Order Value
SELECT SUM(total_price) / COUNT(DISTINCT order_id) AS AVERAGE_ORDER_VALUE FROM pizza_sales;

-- Total Pizzas Sold
SELECT SUM(quantity) AS Total_pizzas_sold FROM pizza_sales;

-- Total Orders
SELECT COUNT(DISTINCT order_id) AS Total_order FROM pizza_sales;

-- Average Pizzas per Order
SELECT CAST(CAST(SUM(quantity) AS decimal(10,2)) / CAST(COUNT(DISTINCT order_id) AS decimal(10,2)) AS decimal(10,2))
AS total_pizzas_per_order FROM pizza_sales;
```

**Daily Order Trend**
```sql
SELECT DATENAME(DW, order_date) AS Order_day, COUNT(DISTINCT order_id) AS Total_orders
FROM pizza_sales
GROUP BY DATENAME(DW, order_date);
```

**Time-Based Filtering (Month/Quarter)**
```sql
-- Example: Orders by day of week, filtered to Quarter 1
SELECT DATENAME(DW, order_date) AS order_day, COUNT(DISTINCT order_id) AS total_orders
FROM pizza_sales
WHERE DATEPART(QUARTER, order_date) = 1
GROUP BY DATENAME(DW, order_date);
```

**Sales % by Pizza Size**
```sql
SELECT pizza_size, SUM(total_price) AS total_sales,
       SUM(total_price) * 100 / (SELECT SUM(total_price) FROM pizza_sales WHERE DATEPART(QUARTER, order_date) = 1) AS Percentage_total_sales
FROM pizza_sales
WHERE DATEPART(QUARTER, order_date) = 1
GROUP BY pizza_size
ORDER BY Percentage_total_sales DESC;
```

**Total Pizzas Sold by Category**
```sql
SELECT pizza_category, SUM(quantity) AS Total_pizza_sold
FROM pizza_sales
GROUP BY pizza_category;
```

**Top 5 Bestsellers**
```sql
SELECT TOP 5 pizza_name, SUM(quantity) AS Total_pizzas_sold
FROM pizza_sales
GROUP BY pizza_name
ORDER BY Total_pizzas_sold DESC;
```

**Bottom 5 Worst Sellers**
```sql
SELECT TOP 5 pizza_name, SUM(quantity) AS Total_pizzas_sold
FROM pizza_sales
GROUP BY pizza_name
ORDER BY Total_pizzas_sold ASC;
```

*(Full query set with screenshots is included in `PIZZA_SALES_SQL_QUERIES.docx` in this repo.)*

---

## 📊 Excel Dashboard

The query outputs were brought into Excel and built into a single-page interactive dashboard using **PivotTables and PivotCharts**, including:

- **KPI cards:** Revenue, Orders, Pizzas Sold, Avg Order Value, Avg Pizzas per Order
- **Daily trend chart:** Order volume by day of week (Friday = peak day)
- **Hourly trend chart:** Order volume by hour (lunch and dinner rush clearly visible)
- **Category & size breakdown:** % of sales and quantity sold by pizza category (Classic, Supreme, Veggie, Chicken) and size (Regular, Medium, Large, X-Large, XX-Large)
- **Best & worst sellers:** Top 5 and bottom 5 pizzas by quantity sold

📁 See `Pizza_Sales_Dashboard.xlsx` in this repo.

---

## 💡 Key Insights

- **Fridays** are the busiest day of the week; **Sundays** the slowest.
- Order volume peaks around **12–1 PM (lunch)** and **6 PM (dinner)**.
- **Classic** pizzas lead in both revenue share (~27%) and volume (14,888 units).
- **Large** pizzas account for the largest share of sales (~46%) by size.
- The Thai Chicken, Pepperoni, Hawaiian, Barbecue Chicken, and Classic Deluxe pizzas are the top 5 bestsellers.

---

## 📂 Repo Contents

```
├── Pizza_Sales_Dashboard.xlsx        # Final Excel dashboard
├── PIZZA_SALES_SQL_QUERIES.docx      # All SQL queries with screenshots
├── README.md                         # Project overview (this file)
```


If you'd like to discuss the methodology, queries, or dashboard design behind this project, feel free to reach out!
