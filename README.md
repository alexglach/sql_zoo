# sql_zoo

Alex and CJ


SELECT BASICS

1. SELECT population FROM world
  WHERE name = 'Germany'

2. SELECT name, population FROM world
  WHERE name IN ('Ireland', 'Iceland', 'Denmark');

3. SELECT name, area FROM world
  WHERE area BETWEEN 200000 AND 250000

SELECT WORLD

1. N/A

2. SELECT name FROM world
WHERE population>=200000000

3. SELECT name, (gdp/population) AS gdp_per_capita
FROM world
WHERE population >= 200000000

4. SELECT name, (population/1000000) AS population_in_millions
FROM world
WHERE continent='South America'

5. SELECT name,population
FROM world
WHERE name IN ('France', 'Germany', 'Italy')

6. SELECT name
FROM world
WHERE name like '%United%'

7. SELECT name, population, area
FROM world
WHERE area > 3000000 OR population > 250000000

8. SELECT name, population, area
FROM world
WHERE area > 3000000 XOR population > 250000000

9. SELECT name, ROUND(population/1000000, 2), ROUND(gdp/1000000000, 2)
FROM world
WHERE continent='South America'

10. SELECT world.name, ROUND((world.gdp/population) / 1000)*1000 as gdp_rounded
FROM world
WHERE world.gdp >= 1000000000000

11. SELECT name,
       CASE WHEN continent='Oceania' THEN 'Australasia'
            ELSE continent END AS continent
  FROM world
 WHERE name LIKE 'N%'