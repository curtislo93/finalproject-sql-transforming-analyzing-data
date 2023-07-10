What issues will you address by cleaning the data?



Queries:
Below, provide the SQL queries you used to clean your data.

--- Update the unit_price in analytics from varchar to numeric (14,2)
ALTER TABLE analytics
ALTER COLUMN unit_price TYPE decimal(14,2)
USING (unit_price::decimal(14,2))

--- Divide the unit_price by 1,000,000
UPDATE analytics
SET unit_price = unit_price * 0.000001

---Update date from varchar to date
ALTER TABLE analytics
ALTER COLUMN date TYPE date
USING (date::date)
