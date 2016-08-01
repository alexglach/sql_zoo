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

12. SELECT name, 
      CASE WHEN continent='Europe' THEN 'Eurasia'
          WHEN continent='Asia' THEN 'Eurasia'
          WHEN continent='North America' THEN 'America'
         WHEN continent='South America' THEN 'America'
          WHEN continent='Caribbean' THEN 'America'
           ELSE continent END
 FROM world
WHERE  name LIKE 'A%' 
OR name LIKE  'B%'

 13. SELECT name, continent,
     CASE WHEN continent = 'Oceania' THEN 'Australasia'
    WHEN continent = 'Eurasia' THEN 'Europe/Asia'
    WHEN continent = 'Caribbean' AND name LIKE 'B%' THEN 'North America'
    WHEN continent = 'Caribbean' THEN 'South America'
    ELSE continent END AS new_continent
FROM world
ORDER BY name


NOBEL

1. SELECT yr, subject, winner
  FROM nobel
 WHERE yr = 1950

 2. SELECT winner
  FROM nobel
 WHERE yr = 1962
   AND subject = 'Literature'

3. SELECT yr, subject
FROM nobel
WHERE winner = 'Albert Einstein'


4. SELECT winner
FROM nobel
WHERE yr >= 2000 AND subject = 'Peace'

5. SELECT yr, subject, winner
FROM nobel
WHERE yr BETWEEN 1980 AND 1989
AND subject = 'Literature'

6. SELECT * FROM nobel
 WHERE winner IN ('Theodore Roosevelt', 'Woodrow Wilson', 'Jimmy Carter')


7. SELECT winner
FROM nobel
WHERE winner LIKE 'John%'

8. SELECT *
FROM nobel
WHERE (subject = 'Physics' AND yr = 1980) OR (subject = 'Chemistry' AND yr = 1984)


9. SELECT *
FROM nobel
WHERE yr = 1980 
AND subject NOT IN ('Chemistry', 'Medicine')

10. SELECT * FROM nobel
WHERE (subject = 'Medicine' AND yr < 1910) OR (subject = 'Literature' AND yr >= 2004)

11. SELECT * FROM nobel
WHERE winner = 'Peter Grünberg'

12. SELECT * FROM nobel
WHERE winner = 'Eugene O''Neill'

13. SELECT winner, yr, subject
FROM nobel
WHERE winner like 'Sir%'
ORDER BY yr DESC, winner;

14. SELECT winner, subject AS
  FROM nobel
 WHERE yr=1984
 ORDER BY subject IN ('Physics', 'Chemistry'), subject, winner

JOIN
1. SELECT goal.matchid, goal.player
FROM goal
WHERE goal.teamid = 'GER'

2. SELECT game.id, game.stadium, game.team1, game.team2
FROM game
WHERE game.id = 1012

3. SELECT goal.player, goal.teamid, game.stadium, game.mdate
FROM goal JOIN game ON goal.matchid = game.id
WHERE goal.teamid = 'GER'

4. SELECT game.team1, game.team2, goal.player
FROM goal JOIN game ON goal.matchid = game.id
WHERE goal.player LIKE 'Mario%'

5. SELECT goal.player, goal.teamid, eteam.coach, goal.gtime
FROM goal JOIN eteam ON goal.teamid = eteam.id
WHERE goal.gtime <= 10

6. SELECT game.mdate, eteam.teamname
FROM game JOIN eteam ON game.team1 = eteam.id
WHERE eteam.coach = 'Fernando Santos'

7. SELECT goal.player
FROM goal JOIN game ON goal.matchid = game.id
WHERE game.stadium = 'National Stadium, Warsaw'

8. SELECT DISTINCT goal.player
FROM goal JOIN game ON goal.matchid = game.id
WHERE (goal.teamid != 'GER') AND (game.team1 = 'GER' OR game.team2 = 'GER')

9. SELECT eteam.teamname, COUNT(*) AS goals
FROM goal JOIN eteam ON goal.teamid = eteam.id
GROUP BY eteam.teamname

10. SELECT game.stadium, COUNT(*) AS goals_scored
FROM goal JOIN game ON goal.matchid = game.id
GROUP BY game.stadium

11. SELECT game.id, game.mdate, COUNT(*)
FROM game JOIN goal ON game.id = goal.matchid
WHERE game.team1 = 'POL' OR game.team2 = 'POL'
GROUP BY game.id, game.mdate

12. SELECT game.id, game.mdate, COUNT(*)
FROM game JOIN goal ON game.id = goal.matchid
WHERE goal.teamid = 'GER'
GROUP BY game.id, game.mdate

13. SELECT mdate,
  team1,
  SUM(CASE WHEN teamid=team1 THEN 1  ELSE 0 END) score1,
  team2,
  SUM( CASE WHEN teamid = team2 THEN 1 ELSE 0 END) score2
  FROM game LEFT OUTER JOIN goal ON matchid = id
GROUP BY mdate, team1, team2

MORE JOINS

1. SELECT id, title
 FROM movie
 WHERE yr=1962

2. SELECT yr
FROM movie
WHERE title='Citizen Kane'

3. SELECT id, title, yr
FROM movie
WHERE title LIKE '%Star Trek%'
ORDER BY yr

4. SELECT title
FROM movie
WHERE id IN ('11768', '11955', '21191')

5. SELECT id
FROM actor
WHERE name = 'Glenn Close'

6. SELECT movie.id
FROM movie
WHERE movie.title = 'Casablanca'

7. 
(subquery) SELECT name
FROM actor
WHERE id IN (
     SELECT actorid
      FROM casting
      WHERE movieid = 11768)

(join) SELECT actor.name
FROM casting JOIN actor ON casting.actorid = actor.id
WHERE casting.movieid = 11768

8.
SELECT actor.name
FROM casting JOIN actor ON casting.actorid = actor.id
JOIN movie ON casting.movieid = movie.id
WHERE movie.title = 'Alien'

9.
SELECT movie.title
FROM casting JOIN actor ON casting.actorid = actor.id
JOIN movie ON casting.movieid = movie.id
WHERE actor.name = 'Harrison Ford'

10.
SELECT movie.title
FROM casting JOIN actor ON casting.actorid = actor.id
JOIN movie ON casting.movieid = movie.id
WHERE actor.name = 'Harrison Ford' AND casting.ord != 1

11.
SELECT movie.title, actor.name
FROM casting JOIN movie ON casting.movieid = movie.id
JOIN actor ON casting.actorid = actor.id
WHERE movie.yr = 1962 AND casting.ord = 1

12. 
SELECT movie.yr, COUNT(*)
FROM casting JOIN actor ON casting.actorid = actor.id
JOIN movie ON casting.movieid = movie.id
WHERE actor.name = 'John Travolta'
GROUP BY movie.yr
HAVING COUNT(*) > 2

13.
SELECT movie.title, actor.name
FROM casting JOIN actor ON actor.id = casting.actorid
JOIN movie ON movie.id = casting.movieid
WHERE casting.movieid IN (SELECT movie.id 
FROM movie JOIN casting ON movie.id = casting.movieid
JOIN actor ON casting.actorid  = actor.id
WHERE actor.name = 'Julie Andrews') AND casting.ord = 1

14.
SELECT name
FROM (SELECT actor.name, COUNT(*) as starring_roles
FROM casting JOIN actor ON actor.id = casting.actorid
WHERE casting.ord = 1
GROUP BY actor.name) as actors
WHERE starring_roles >= 30

15. 
SELECT movie.title, COUNT(*) as num_actors
FROM casting JOIN movie ON casting.movieid = movie.id
WHERE yr = 1978
GROUP BY movie.title
ORDER BY num_actors DESC

16. 
SELECT actor.name
FROM actor JOIN casting ON actor.id = casting.actorid
WHERE casting.movieid IN (SELECT casting.movieid
FROM casting JOIN actor ON actor.id = casting.actorid
WHERE actor.name = 'Art Garfunkel') AND  actor.name != 'Art Garfunkel'

SUM AND COUNT

1. SELECT SUM(population)
FROM world

2. 
SELECT DISTINCT continent
FROM world

3. 
SELECT SUM(GDP)
FROM world
WHERE continent = 'Africa'

4. 
SELECT COUNT(*)
FROM world
WHERE area >= 1000000

5.
SELECT SUM(population)
FROM world
WHERE name IN ('France', 'Germany', 'Spain')

6. 
SELECT continent, COUNT(*) as num_countries
FROM world
GROUP BY continent

7. 
SELECT world.continent, COUNT(*) AS num_countries
FROM world
WHERE world.population >= 10000000
GROUP BY world.continent

8.
SELECT continents.continent
FROM (SELECT world.continent, SUM(population) AS total_pop
FROM world
GROUP BY world.continent) AS continents
WHERE total_pop >= 100000000

NESTED SELECTS
1.
SELECT world.name
FROM world
WHERE world.population > 
(SELECT world.population
FROM world
WHERE world.name = 'Russia')

2.
SELECT world.name
FROM world
WHERE (world.gdp / world.population) >
(SELECT (world.gdp / world.population) AS uk_gdp_per_capita
FROM world
WHERE world.name = 'United Kingdom')
AND
world.continent = 'Europe'

3. 
SELECT world.name, world.continent
FROM world
WHERE world.continent IN 
(SELECT world.continent
FROM world
WHERE world.name = 'Argentina' OR world.name = 'Australia')
ORDER BY world.name

4.
SELECT world.name, world.population
FROM world
WHERE world.population
>
(SELECT world.population
FROM world
WHERE world.name = 'Canada')
AND
world.population
<
(SELECT world.population
FROM world
WHERE world.name = 'Poland')

5.
SELECT world.name, CONCAT(ROUND((world.population / (SELECT world.population
FROM world
WHERE world.name = 'Germany')) * 100), '%')
FROM world
WHERE world.continent = 'Europe'

6.
SELECT world.name
FROM world
WHERE world.gdp >
(SELECT MAX(world.gdp)
FROM world
WHERE world.continent = 'Europe')

7.
SELECT world.continent, world.name, world.area
FROM world
WHERE world.area IN
(SELECT MAX(world.area)
FROM world
GROUP BY world.continent)

8.
SELECT world.continent, world.name
FROM world
WHERE world.name IN
(SELECT MIN(world.name)
FROM world
GROUP BY world.continent)

9.
SELECT world.name, world.continent, world.population
FROM world
WHERE world.continent IN 
(SELECT max_per_continent.continent
FROM (SELECT world.continent, MAX(world.population) AS max_pop
FROM world
GROUP BY continent) AS max_per_continent
WHERE max_per_continent.max_pop <= 25000000)

10.
<!-- FROM world
WHERE world.population > (3 * min_table.min_pop)
AND
world.continent = min_table.continent
(SELECT world.continent, MIN(world.population) AS min_pop
FROM world
GROUP BY continent) AS min_table -->