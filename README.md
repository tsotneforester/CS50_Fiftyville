pu<div align="center">
<img src="https://github-production-user-asset-6210df.s3.amazonaws.com/79293287/287023363-0c72c852-3d13-4af6-b543-aa7dfe8bcf27.png" alt="logo">
<br/> <br/>

</div>

Provided to you is a file called movies.db, a SQLite database that stores data from <a href="https://www.imdb.com/">IMDb</a> about movies, the people who directed and starred in them, and their ratings.

### Your goal is to identify:

For each of the following problems, you should write a single SQL query that outputs the results specified by each problem. Your response must take the form of a single SQL query, though you may nest other queries inside of your query.

<br/>

### Built With

<p align="center">
  <a href="https://skillicons.dev">
    <img src="https://skills.thijs.gg/icons?i=sqlite" />
  </a>
</p>

### Installation & Usage

- Set up SQLite Environment
- Download <a href="https://cdn.cs50.net/2022/fall/psets/7/movies.zip">files</a>
- write queries in appropriate n.sql file
- run query in terminal

```sh
cat n.sql | sqlite3 movies.db
```

<h1 align="center">Solution</h1>

1. In 1.sql, write a SQL query to list the titles of all movies released in 2008. (1 column and 10,050 rows.)

```sql
SELECT title
  FROM movies
 WHERE year = 2008
```

2. In 2.sql, write a SQL query to determine the birth year of Emma Stone. (1 column and 1 row - 1988)

```sql
   SELECT birth
     FROM people
    WHERE name = "Emma Stone"
    LIMIT 10
```

3. In 3.sql, write a SQL query to list the titles of all movies with a release date on or after 2018, in alphabetical order. (1 column and 88,918 rows.)

```sql
   SELECT title
     FROM movies
    WHERE year >= 2018
 ORDER BY title
```

4. In 4.sql, write a SQL query to determine the number of movies with an IMDb rating of 10.0 (1 column and 1 row - 132)

```sql
   SELECT count(*) as rating
     FROM ratings
    WHERE rating = 10.0
```

5. In 5.sql, write a SQL query to list the titles and release years of all Harry Potter movies, in chronological order. (2 columns and 12 rows.)

```sql
   SELECT title,
          year
     FROM movies
    WHERE title LIKE "Harry Potter%"
 ORDER BY year
```

6. In 6.sql, write a SQL query to determine the average rating of all movies released in 2012. (1 column and 1 row - 6.29787154592979)

```sql
   SELECT AVG(rating) as average
     FROM ratings
    WHERE movie_id IN (
             SELECT id
               FROM movies
              WHERE year = 2012
          )
```

7. In 7.sql, write a SQL query to list all movies released in 2010 and their ratings, in descending order by rating. For movies with the same rating, order them alphabetically by title. (2 columns and 7,085 rows.)

```sql
   SELECT movies.title,
          ratings.rating
     FROM movies,
          ratings
    WHERE movies.id = ratings.movie_id
      AND movies.year = 2010
 ORDER BY ratings.rating DESC,
          movies.title
```

8. In 8.sql, write a SQL query to list the names of all people who starred in Toy Story (1 column and 4 rows.)

```sql
   SELECT name
     FROM people
    WHERE id IN (
             SELECT person_id
               FROM stars
              WHERE movie_id IN (
                       SELECT id
                         FROM movies
                        WHERE title = "Toy Story"
                    )
          )
```

9. In 9.sql, write a SQL query to list the names of all people who starred in a movie released in 2004, ordered by birth year. (1 column and 18,946 rows.)

```sql
   SELECT name
     FROM people
    WHERE id IN (
             SELECT person_id
               FROM stars
              WHERE movie_id IN (
                       SELECT id
                         FROM movies
                        WHERE year = 2004
                    )
          )
 ORDER BY birth
```

10. In 10.sql, write a SQL query to list the names of all people who have directed a movie that received a rating of at least 9.0. (1 column and 3,392 rows.)

```sql
   SELECT name
     FROM people
    WHERE id IN (
             SELECT DISTINCT person_id
               FROM directors
              WHERE movie_id IN (
                       SELECT movie_id
                         FROM ratings
                        WHERE rating >= 9.0
                    )
           ORDER BY person_id
          )
```

11. In 11.sql, write a SQL query to list the titles of the five highest rated movies (in order) that Chadwick Boseman starred in, starting with the highest rated. (1 column and 5 rows.)

```sql
   SELECT title
     FROM (
             SELECT movie_id
               FROM ratings
              WHERE movie_id IN (
                       SELECT movie_id
                         FROM stars
                        WHERE person_id IN (
                                 SELECT id
                                   FROM people
                                  WHERE name = "Chadwick Boseman"
                              )
                    )
           ORDER BY rating DESC
              LIMIT 5
          ) as b
     JOIN movies ON b.movie_id = movies.id;
```

12. In 12.sql, write a SQL query to list the titles of all movies in which both Bradley Cooper and Jennifer Lawrence starred (1 column and 4 rows.)

```sql
   SELECT title
     FROM movies
    WHERE id IN (
             SELECT movie_id
               FROM stars
              WHERE person_id IN (
                       SELECT id
                         FROM people
                        WHERE name = "Bradley Cooper"
                           OR name = "Jennifer Lawrence"
                    )
           GROUP BY movie_id
             HAVING COUNT(DISTINCT person_id) = 2
          )
```

13. In 13.sql, write a SQL query to list the names of all people who starred in a movie in which Kevin Bacon also starred. (1 column and 182 rows.)

```sql
   SELECT name
     FROM people
    WHERE id IN (
             SELECT DISTINCT person_id
               FROM stars
              WHERE movie_id IN (
                       SELECT movie_id
                         FROM stars
                        WHERE person_id IN (
                                 SELECT id
                                   FROM people
                                  WHERE name = "Kevin Bacon"
                                    AND birth = 1958
                              )
                    )
                AND person_id != (
                       SELECT id
                         FROM people
                        WHERE name = "Kevin Bacon"
                          AND birth = 1958
                    )
          )
```
