<h1 align="center">A Mystery in Fiftyville</h1>
<div align="center">
 <img src="https://github-production-user-asset-6210df.s3.amazonaws.com/79293287/286924597-7b39d1e2-0ada-4656-9a3d-4ff453fb805a.png" alt="logo">
 <br/>
 
</div>

The CS50 Duck has been stolen! The town of Fiftyville has called upon you to solve the mystery of the stolen duck. Authorities believe that the thief stole the duck and then, shortly afterwards, took a flight out of town with the help of an accomplice.

### Your goal is to identify:

![thief](https://img.shields.io/badge/-Who%20the%20thief%20is-006400 "image")<br/>
![city](https://img.shields.io/badge/-What%20city%20the%20thief%20escaped%20to-3e54a3 "image")<br/>
![accomplice](https://img.shields.io/badge/-Who%20the%20thief’s%20accomplice%20is-FF0000 "image")<br/>

<br/>

All you know is that the theft _**took place on July 28, 2021**_ and that it took place on _**Humphrey Street**_.

### Built With

<p align="center">
  <a href="https://skillicons.dev">
    <img src="https://skills.thijs.gg/icons?i=sqlite" />
  </a>
</p>

### Installation & Usage

- Set up SQLite Environment
- Download <a href="https://cdn.cs50.net/2022/fall/psets/7/fiftyville.zip">files</a>
- write queries in .sql file
- run query in terminal

```sh
cat log.sql | sqlite3 fiftyville.db
```

<h1 align="center">Solution</h1>

1. In 1.sql, write a SQL query to list the titles of all movies released in 2008. (1 column and 10,050 rows.)

```sql
SELECT title
  FROM movies
 WHERE year = 2008
```

2. In 2.sql, write a SQL query to determine the birth year of Emma Stone. (1 column and 1 row.)

```sql
SELECT birth
  FROM people
WHERE name = "Emma Stone"
LIMIT 10
```

3. Get Crime log of 28th of July (1 column and 88,918 rows.)

```sql
   SELECT title
     FROM movies
    WHERE year >= 2018
 ORDER BY title
```

4. Get Crime log of 28th of July (1 column and 1 row.)

```sql

```

5. Get Crime log of 28th of July (2 columns and 12 rows.)

```sql

```

6. Get Crime log of 28th of July (1 column and 1 row.)

```sql

```

7. Get Crime log of 28th of July (2 columns and 7,085 rows.)

```sql

```

8. Get Crime log of 28th of July (1 column and 4 rows.)

```sql

```

9. Get Crime log of 28th of July (1 column and 18,946 rows.)

```sql

```

10. Get Crime log of 28th of July (1 column and 3,392 rows.)

```sql

```

11. Get Crime log of 28th of July (1 column and 5 rows.)

```sql

```

12. Get Crime log of 28th of July (1 column and 4 rows.)

```sql

```

13. Get Crime log of 28th of July (1 column and 182 rows.)

```sql

```
