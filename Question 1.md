# Soru 1) 1980’den itibaren spor grubu bazında en çok madalya alan 1. 3. 5. ülkeyi bulalım.

```SQL
WITH t1 as(
SELECT
  Country,
  Sport,
  ROW_NUMBER() OVER (PARTITION BY Sport ORDER BY COUNT(*) DESC) rank
FROM
  `dsmbootcamp.ahmet_lekesiz.summer_medals`
WHERE
  Country IS NOT NULL
  AND Year>=1980
GROUP BY Sport, Country
)
SELECT 
  Sport, first_country, third_country, fifth_country FROM (
SELECT 
  Sport, 
  rank,
  first_value(Country) over(partition by Sport order by rank) as first_country,
  nth_value(Country, 3) over(partition by Sport order by rank) as third_country,
  nth_value(Country, 5) over(partition by Sport order by rank) as fifth_country,
FROM 
  t1
order by Sport, rank
)
WHERE rank = 5
```
