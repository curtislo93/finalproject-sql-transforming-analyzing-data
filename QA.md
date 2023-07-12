**What are your risk areas? Identify and describe them.**


**QA Process:
Describe your QA process and include the SQL queries used to execute it.**

First, we will validate the number of transactions (total ordered) by comparing the results of two queries.

```
SELECT COUNT(total_ordered) 
FROM(
  SELECT sku.total_ordered AS total_ordered
  FROM sales_by_sku sku
  JOIN sales_report sr ON sr."productSKU" = sku."productSKU") tmp
```
Verify with:
```
SELECT COUNT(total_ordered) 
FROM sales_report sr
```

This matches so we know that the number of transactions is accurate from the joined table.
