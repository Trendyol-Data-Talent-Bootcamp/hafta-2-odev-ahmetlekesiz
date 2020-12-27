# Question 3

```SQL
-- Data Review
SELECT * FROM `dsmbootcamp.ahmet_lekesiz.pageview` LIMIT 100 

-- Question 3
SELECT view_period,
  (SELECT HLL_COUNT.MERGE(sketch) FROM UNNEST(device_in_5_min) sketch)  active_user_count
FROM (
  SELECT 
    view_period,
    ARRAY_AGG(device_in_each_min) OVER(ROWS BETWEEN 4 PRECEDING AND CURRENT ROW) device_in_5_min 
  FROM (
    SELECT 
      timestamp_trunc(view_ts, minute) as view_period,
      HLL_COUNT.init(deviceid) as device_in_each_min 
    FROM 
      `dsmbootcamp.ahmet_lekesiz.pageview` 
    GROUP BY view_period
    ORDER BY view_period
  )
)
```
