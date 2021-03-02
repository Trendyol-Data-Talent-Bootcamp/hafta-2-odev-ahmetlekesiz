# Soru 2) 1980’den itibaren herhangi bir spor grubunda üst üste 3 veya daha fazla madalya almış atletleri bulalım.

```SQL
WITH t2 as(
SELECT Sport, Athlete, Year,
  LEAD(Year) OVER (PARTITION BY Sport, Athlete ORDER BY Year) second_year,
  LEAD(Year, 2) OVER (PARTITION BY Sport, Athlete ORDER BY Year) as third_year
  --Year,
  --Sport,
  --Athlete,
  --DENSE_RANK() OVER (PARTITION BY Sport, Year ORDER BY Athlete) dense_rank
FROM
  `dsmbootcamp.ahmet_lekesiz.summer_medals`
WHERE
  Country IS NOT NULL
  AND Year>=1980
Group by Sport, Year, Athlete
)
SELECT 
  *
FROM 
  t2
WHERE 
  second_year is not null
AND
  third_year is not null
AND
  second_year - year = 4
AND
  third_year - second_year = 4
```
