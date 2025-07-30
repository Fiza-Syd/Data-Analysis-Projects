# Indian Wedding Data Cleaning & Analysis

## Key Steps

### Data Import:
Initially struggled with `LOAD DATA INFILE` due to `secure_file_priv` and PATH issues. Resolved by using MySQL Workbench’s Table Data Import Wizard, successfully loading `weddings.csv`, `expenses.csv`, and `vendors.csv` into the `indian_weddings` database.

### Data Cleaning:
- Inspected for NULLs and duplicates using `SELECT` queries.
- Standardized `city_tier`, `wedding_type`, `category`, and `vendor_type` with `UPPER(TRIM())`, and corrected negative values with `ABS()`.
- Added a `guest_category` column based on `guest_count` ranges (Small ≤ 200, Medium ≤ 500, Large).

### Table Joins:
- Created a **"Basic Join for Total Costs"** CTE (`WeddingCosts`) to calculate total cost per wedding (budget + expenses + vendor prices), grouping by `wedding_id`, `year`, `city`, `region`, `budget`.
- Developed a **"Join with Filters"** CTE (`AdditionalCosts`) to analyze average budget and additional costs by `region` and `wedding_type`, fixing invalid aggregation nesting.

---

## Challenges and Resolutions

- **Import Errors:** Faced Error 1064 (syntax error) and Error 2003 (connection failure). Resolved by using Workbench’s GUI import and restarting the MySQL service.
- **Credential Issues:** Unknown username/password fixed by resetting the root password in safe mode.
- **Grouping Errors:** Original queries failed with Error 1111 (invalid group function) due to nested aggregates and `ONLY_FULL_GROUP_BY` mode. CTEs separated aggregation levels, resolving the issue by structuring logic hierarchically.

---

## Current Status

- Data is imported and cleaned in the `indian_weddings` database.
- CTEs (`WeddingCosts`, `AdditionalCosts`) enable efficient joins and aggregations, ready for further analysis or export to Power BI.

---

## Data Cleaning Process Documentation

```sql
-- Step 1: Check for NULL values and duplicates in all tables

-- Weddings table checks
SELECT COUNT(*) - COUNT(wedding_id) AS null_wedding_ids, 
       COUNT(*) - COUNT(year) AS null_years, 
       COUNT(*) - COUNT(city) AS null_cities
FROM weddings;

-- Check for duplicate wedding_ids
SELECT wedding_id, COUNT(*) 
FROM weddings 
GROUP BY wedding_id 
HAVING COUNT(*) > 1;

-- Expenses table checks
SELECT COUNT(*) - COUNT(wedding_id) AS null_wedding_ids, 
       COUNT(*) - COUNT(amount) AS null_amounts
FROM expenses;

-- Vendors table checks
SELECT COUNT(*) - COUNT(wedding_id) AS null_wedding_ids, 
       COUNT(*) - COUNT(price) AS null_prices
FROM vendors;

/* 
Analysis: No NULL values or duplicates found, but we'll proceed with cleaning
to ensure dataset integrity.
*/

-- Step 2: Handle NULL values by setting them to 0 where appropriate
UPDATE weddings SET budget = 0 WHERE budget IS NULL;
UPDATE expenses SET amount = 0 WHERE amount IS NULL;
UPDATE vendors SET price = 0 WHERE price IS NULL;

-- Step 3: Handle potential duplicates (example query - not needed in this case)
-- DELETE t1 FROM weddings t1
-- INNER JOIN weddings t2 
-- WHERE t1.wedding_id = t2.wedding_id AND t1.id > t2.id;

-- Step 4: Standardize text data
-- Note: Had to disable safe update mode temporarily for these operations
UPDATE weddings 
SET city_tier = UPPER(TRIM(city_tier))
WHERE city_tier IS NOT NULL AND city_tier != UPPER(TRIM(city_tier));

UPDATE weddings SET wedding_type = LOWER(TRIM(wedding_type));
UPDATE expenses SET category = UPPER(TRIM(category));
UPDATE vendors SET vendor_type = UPPER(TRIM(vendor_type));

-- Step 5: Fix invalid numeric values (negative numbers)
UPDATE weddings SET budget = ABS(budget) WHERE budget < 0;
UPDATE expenses SET amount = ABS(amount) WHERE amount < 0;
UPDATE vendors SET price = ABS(price) WHERE price < 0;

-- Step 6: Add calculated fields
-- Categorize guest counts (note initial naming issue corrected)
ALTER TABLE weddings
ADD COLUMN guest_category_type VARCHAR(10);

UPDATE weddings
SET guest_category_type = 
   CASE 
       WHEN guest_count <= 200 THEN 'Small'
       WHEN guest_count <= 500 THEN 'Medium'
       ELSE 'Large'
   END;
```

```text
Important Notes:
1. Safe update mode had to be disabled temporarily for bulk updates
2. Initial column naming conflict occurred ('guest_category' already existed)
3. All changes were carefully documented to ensure reproducibility
4. Safe update mode was re-enabled after completing the updates
5. As an analyst, DML operations (INSERT, UPDATE, DELETE) are crucial for data cleaning
```
