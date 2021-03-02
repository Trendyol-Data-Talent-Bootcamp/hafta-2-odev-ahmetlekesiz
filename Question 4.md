# Question 4

```SQL
-- Get changes by using Merge
MERGE ahmet_lekesiz.content_category f1
USING ahmet_lekesiz.content_category_20201222_00_59 f2 
ON f1.id = f2.id
WHEN MATCHED AND f1.cdc_date != f2.cdc_date THEN
  UPDATE SET f1.category = f2.category, f1.cdc_date = f2.cdc_date
WHEN NOT MATCHED BY TARGET THEN
  INSERT(cdc_date, is_deleted, id, category) VALUES(f2.cdc_date, f2.is_deleted, f2.id, f2.category)
WHEN NOT MATCHED BY SOURCE THEN
  UPDATE SET f1.is_deleted = true


-- Make sure that you have get every changes
-- This query should return 0 row if you have got every changes
WITH t1 as(
SELECT
  *,
  farm_fingerprint(to_json_string( CONCAT(cdc_date, is_deleted, id, category) )) as _hash1
FROM
  ahmet_lekesiz.content_category
)

SELECT *
FROM t1
RIGHT JOIN (SELECT
  *,
  farm_fingerprint(to_json_string( CONCAT(cdc_date, is_deleted, id, category) )) as _hash1
FROM
  ahmet_lekesiz.content_category_20201222_00_59) t2
ON t1.id = t2.id
WHERE t1._hash1 <> t2._hash1
```
