**Question 1: Find the number of unique product views by each visitor. What can this tell us?**

SQL Queries:
```
SELECT DISTINCT(a."fullvisitorId"), COUNT(als."productSKU") AS UniqueProduct
FROM analytics a
FULL OUTER JOIN all_sessions als
	ON als."visitId" = a."visitId"
WHERE 
	als."productSKU" IS NOT NULL AND
	a."fullvisitorId" IS NOT NULL
GROUP BY a."fullvisitorId"
ORDER BY UniqueProduct DESC
LIMIT 100
```
Answer: 
There are many shoppers that like to browse all different products.

**Question 2: What are the 5 best selling products in terms of total revenue, by product name?**

SQL Queries:
```
SELECT 
	als."v2ProductName" AS ProductName,
	sum(sr.total_ordered * als."productPrice") AS TotalOrderRevenue
FROM sales_report sr
JOIN all_sessions als ON als."productSKU" = sr."productSKU"
GROUP BY ProductName
ORDER BY TotalOrderRevenue DESC
LIMIT 5
```
Answer:
"Nest® Cam Outdoor Security Camera - USA"	1434496.00
"Nest® Learning Thermostat 3rd Gen-USA - Stainless Steel"	1417802.00
"Nest® Cam Indoor Security Camera - USA"	1359150.00
"Google 17oz Stainless Steel Sport Bottle"	386898.92
"Nest® Protect Smoke + CO White Wired Alarm-USA"	262794.00

**Question 3: Find the top selling product by month.**

SQL Queries:
```
WITH cte1 AS
(	SELECT 
		als."v2ProductName",
		als."v2ProductCategory",
		EXTRACT(MONTH FROM date) AS Month,
		sum(p."orderedQuantity") AS TotalQtyOrdered
	FROM all_sessions als
	FULL OUTER JOIN products p
		ON p."SKU" = als."productSKU"
	WHERE 
		city != 'not available in demo dataset' AND
		city != '(not set)' 
	GROUP BY als."v2ProductName", als."v2ProductCategory", als.date
	HAVING SUM(p."orderedQuantity") IS NOT NULL
),
cte2 AS 
( 	SELECT 
		Month,
	 	MAX(TotalQtyOrdered) AS TopQtyOrdered
	 FROM cte1
	 GROUP BY Month
)

SELECT cte1."v2ProductName", cte1.month, cte2.TopQtyOrdered 
FROM cte1
INNER JOIN cte2
	 ON cte1.month = cte2.month AND cte1.TotalQtyOrdered = cte2.TopQtyOrdered
GROUP BY cte1.Month, cte1."v2ProductName", cte2.TopQtyOrdered 
ORDER BY cte1.Month
```

Answer:
With the exception of August, the Google Kick Ball was the top selling product in each month.


Question 4: 

SQL Queries:

Answer:



Question 5: 

SQL Queries:

Answer:
