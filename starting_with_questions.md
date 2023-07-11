Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


***SQL Queries:
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

***Answer:

USA has the highest level of transaction revenues on the site.
San Francisco has the highest level of transaction reveneues on the site.

**Question 2: What is the average number of products ordered from visitors in each city and country?**


SQL Queries:
_This would be for the breakdown by country._
```
SELECT als.country, CAST(AVG(p."orderedQuantity") AS Numeric(14,2)) AS AvgQtyOrdered
FROM all_sessions als
FULL OUTER JOIN products p
	ON p."SKU" = als."productSKU"
WHERE city != 'not available in demo dataset' AND city != '(not set)'
GROUP BY als.country
HAVING AVG(p."orderedQuantity") IS NOT NULL
ORDER BY als.country
```
_This would be for the breakdown by city._
```
SELECT als.city, CAST(AVG(p."orderedQuantity") AS Numeric(14,2)) AS AvgQtyOrdered
FROM all_sessions als
FULL OUTER JOIN products p
	ON p."SKU" = als."productSKU"
WHERE city != 'not available in demo dataset' AND city != '(not set)'
GROUP BY als.city
HAVING AVG(p."orderedQuantity") IS NOT NULL
ORDER BY als.city
```

Answer:





**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:



Answer:





**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:



Answer:





**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:



Answer:







