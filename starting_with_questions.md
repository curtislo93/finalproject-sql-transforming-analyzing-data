Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


SQL Queries:
```
SELECT country, sum("totalTransactionRevenue") AS TotalTransactionRevenue
FROM all_sessions 
WHERE "totalTransactionRevenue" IS NOT NULL
GROUP BY country
ORDER BY TotalTransactionRevenue DESC
```
```
SELECT city, sum("totalTransactionRevenue") AS TotalTransactionRevenue
FROM all_sessions 
WHERE 
	"totalTransactionRevenue" IS NOT NULL AND
	city != 'not available in demo dataset'
GROUP BY city
ORDER BY TotalTransactionRevenue DESC
LIMIT 5
```

Answer:

USA has the highest level of transaction revenues on the site.
San Francisco has the highest level of transaction reveneues on the site.

**Question 2: What is the average number of products ordered from visitors in each city and country?**


SQL Queries:

_This would be for the breakdown by country._
```
SELECT als.country, CAST(AVG(s.total_ordered) AS Numeric(14,2)) AS AvgQtyOrdered
FROM all_sessions als
FULL OUTER JOIN sales_by_sku s
	ON s."productSKU" = als."productSKU"
WHERE city != 'not available in demo dataset' AND city != '(not set)'
GROUP BY als.country
HAVING AVG(s.total_ordered) IS NOT NULL
ORDER BY als.country
```
_This would be for the breakdown by city._
```
SELECT als.city, CAST(AVG(s.total_ordered) AS Numeric(14,2)) AS AvgQtyOrdered
FROM all_sessions als
FULL OUTER JOIN sales_by_sku s
	ON s."productSKU" = als."productSKU"
WHERE city != 'not available in demo dataset' AND city != '(not set)'
GROUP BY als.city
HAVING AVG(s.total_ordered) IS NOT NULL
ORDER BY als.city
```

Answer:





**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:
```
SELECT 
	als."v2ProductCategory", als.country, als.city,
	sum(s.total_ordered) AS TotalQtyOrdered
FROM all_sessions als
FULL OUTER JOIN sales_by_sku s
	ON s."productSKU" = als."productSKU"
WHERE city != 'not available in demo dataset' AND city != '(not set)'
GROUP BY als."v2ProductCategory", als.country, als.city
HAVING sum(s.total_ordered) IS NOT NULL
ORDER BY TotalQtyOrdered DESC
```


Answer:
Looks like the types of product categories people are interested in are Nest and Office. Most are located in California (Sunnyvale, Palo Alto, San Francisco, Mountain View...)


**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:
```
WITH cte1 AS	
(	SELECT 
		als."v2ProductName", als.country, als.city,
		sum(s.total_ordered) AS TotalQtyOrdered
	FROM all_sessions als
	FULL OUTER JOIN sales_by_sku s
		ON s."productSKU" = als."productSKU"
 	FULL OUTER JOIN products p
 		ON p."SKU" = s."productSKU"
	WHERE city != 'not available in demo dataset' AND city != '(not set)'
	GROUP BY als."v2ProductName", als.country, als.city
	HAVING sum(s.total_ordered) IS NOT NULL
),
cte2 AS
( 	SELECT 
		country,
	 	MAX(TotalQtyOrdered) AS TopQtyOrdered
	 FROM cte1
	 GROUP BY country
)

SELECT cte1."v2ProductName", cte1.country, cte2.TopQtyOrdered 
FROM cte1
INNER JOIN cte2
	 ON cte1.country = cte2.country AND cte1.TotalQtyOrdered = cte2.TopQtyOrdered
ORDER BY cte2.TopQtyOrdered DESC
```
_For cities, replace 'country' with 'city' and rerun the code._

Top selling products:
```
SELECT 
	als."v2ProductName",
	sum(s.total_ordered) AS TotalQtyOrdered
FROM all_sessions als
FULL OUTER JOIN sales_by_sku s
	ON s."productSKU" = als."productSKU"
FULL OUTER JOIN products p
	ON p."SKU" = s."productSKU"
WHERE city != 'not available in demo dataset' AND city != '(not set)'
GROUP BY als."v2ProductName"
HAVING sum(s.total_ordered) IS NOT NULL
ORDER BY TotalQtyOrdered DESC
```

Answer:

Most countries and cities' top-selling product is the Nest® Cam Indoor Security Camera - USA. A further analysis on this item would show that this is one of the more profitable items overall (step 4, question 2).

Overall, the sport bottle sold the most while the cameras were 3rd and 4th best selling.


**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:

_I am using the same query as Question 1_
```
SELECT country, sum("totalTransactionRevenue") AS TotalTransactionRevenue
FROM all_sessions 
WHERE "totalTransactionRevenue" IS NOT NULL
GROUP BY country
ORDER BY TotalTransactionRevenue DESC
```
```
SELECT city, sum("totalTransactionRevenue") AS TotalTransactionRevenue
FROM all_sessions 
WHERE "totalTransactionRevenue" IS NOT NULL
GROUP BY city
ORDER BY TotalTransactionRevenue DESC
```
Answer:

We can summarize it as being most profitable in the United States, however the summary of the total transaction revenue cannot be grouped by each city as a lot of the results are not listed (not available in demo dataset).




