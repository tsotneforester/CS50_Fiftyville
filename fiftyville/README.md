<h1 align="center">Mystery in Fiftyville</h1>
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

1. Get Crime log of 28th of July

```sql
SELECT
  description
FROM
  crime_scene_reports
WHERE
  month = 7
  AND day = 28
  AND street = 'Humphrey Street';
```

2. Get witnesses interviews where `bakery` keyword presents

```sql
SELECT
  transcript
FROM
  interviews
WHERE
  month = 7
  AND day = 28
  AND transcript LIKE "%bakery%";
```

3. Get Person IDs as thief withdrew money from ATM on Legget Str.

```sql
SELECT
  person_id
FROM
  bank_accounts
WHERE
  account_number IN (
    SELECT
      account_number
    FROM
      atm_transactions
    WHERE
      month = 7
      AND day = 28
      AND atm_location LIKE "%leggett%"
      AND transaction_type = "withdraw"
  );
```

4. As thief called up his accomplice with call duration less then minute, let's get phone numbers, one of which is thief's

```sql
SELECT
  caller
FROM
  phone_calls
WHERE
  month = 7
  AND day = 28
  AND duration < 60;
```

5. As thief left parking within 10 minutes, let's find out all number plates of appropriate cars

```sql
SELECT
  license_plate
FROM
  bakery_security_logs
WHERE
  month = 7
  AND day = 28
  AND hour = 10
  AND minute < 25
  AND activity = "exit";
```

6. Get ID of Earliest Flight out of Fiftyville next day of crime (36)

```sql
SELECT
  id
FROM
  flights
WHERE
  month = 7
  AND day = 29
  AND origin_airport_id IN (
    SELECT
      id
    FROM
      airports
    WHERE
      city = "fiftyville"
  )
ORDER BY
  hour
LIMIT
  1;
```

![city](https://img.shields.io/badge/-What%20city%20the%20thief%20escaped%20to-3e54a3 "image")

```sql
SELECT
  city
FROM
  airports
WHERE
  id IN (
    SELECT
      destination_airport_id
    FROM
      flights
    WHERE
      month = 7
      AND day = 29
      AND origin_airport_id IN (
        SELECT
          id
        FROM
          airports
        WHERE
          city = "fiftyville"
      )
    ORDER BY
      hour
    LIMIT
      1
  );
```

7. Get Passport Numbers of N 36 flight passengers

```sql
SELECT
  passport_number
FROM
  passengers
WHERE
  flight_id IN (
    SELECT
      id
    FROM
      flights
    WHERE
      month = 7
      AND day = 29
      AND origin_airport_id IN (
        SELECT
          id
        FROM
          airports
        WHERE
          city = "fiftyville"
      )
    ORDER BY
      hour
    LIMIT
      1
  );
```

As we allready know possible phone numbers, license plate numbers, id-s, passport numbers, filter out `people` table and learn<br/>
![thief](https://img.shields.io/badge/-Who%20the%20thief%20is-006400 "image")

```sql
SELECT
  name
FROM
  people
WHERE
  license_plate IN (
    SELECT
      license_plate
    FROM
      bakery_security_logs
    WHERE
      month = 7
      AND day = 28
      AND hour = 10
      AND minute < 25
      AND activity = "exit"
  )
  AND passport_number IN (
    SELECT
      passport_number
    FROM
      passengers
    WHERE
      flight_id = 36 -- type dynamically
      )
  AND id IN (
    SELECT
      person_id
    FROM
      bank_accounts
    WHERE
      account_number IN (
        SELECT
          account_number
        FROM
          atm_transactions
        WHERE
          month = 7
          AND day = 28
          AND atm_location LIKE "%leggett%"
          AND transaction_type = "withdraw"
      )
  )
  AND phone_number IN (
    SELECT
      caller
    FROM
      phone_calls
    WHERE
      month = 7
      AND day = 28
      AND duration < 60
  );
```

8. As we already know who the thief is, let's find out ACCOMPLICE Phone Number, as they talked to each other by phone on crime day

```sql
 SELECT
  receiver
FROM
  phone_calls
WHERE
  month = 7
  AND day = 28
  AND duration < 60
  AND caller IN (
    SELECT
      phone_number
    FROM
      people
    WHERE
      license_plate IN (
        SELECT
          license_plate
        FROM
          bakery_security_logs
        WHERE
          month = 7
          AND day = 28
          AND hour = 10
          AND minute < 25
          AND activity = "exit"
      )
      AND passport_number IN (
        SELECT
          passport_number
        FROM
          passengers
        WHERE
          flight_id = 36 -- type dynamically
          )
      AND id IN (
        SELECT
          person_id
        FROM
          bank_accounts
        WHERE
          account_number IN (
            SELECT
              account_number
            FROM
              atm_transactions
            WHERE
              month = 7
              AND day = 28
              AND atm_location LIKE "%leggett%"
              AND transaction_type = "withdraw"
          )
      )
      AND phone_number IN (
        SELECT
          caller
        FROM
          phone_calls
        WHERE
          month = 7
          AND day = 28
          AND duration < 60
      )
  );
```

Find accomplice by phone number <br/>
![accomplice](https://img.shields.io/badge/-Who%20the%20thief’s%20accomplice%20is-FF0000 "image")

```sql
SELECT
  NAME
FROM
  people
WHERE
  phone_number IN (
    SELECT
      receiver
    FROM
      phone_calls
    WHERE
      month = 7
      AND day = 28
      AND duration < 60
      AND caller IN (
        SELECT
          phone_number
        FROM
          people
        WHERE
          license_plate IN (
            SELECT
              license_plate
            FROM
              bakery_security_logs
            WHERE
              month = 7
              AND day = 28
              AND hour = 10
              AND minute < 25
              AND activity = "exit"
          )
          AND passport_number IN (
            SELECT
              passport_number
            FROM
              passengers
            WHERE
              flight_id = 36
          )
          AND id IN (
            SELECT
              person_id
            FROM
              bank_accounts
            WHERE
              account_number IN (
                SELECT
                  account_number
                FROM
                  atm_transactions
                WHERE
                  month = 7
                  AND day = 28
                  AND atm_location LIKE "%leggett%"
                  AND transaction_type = "withdraw"
              )
          )
          AND phone_number IN (
            SELECT
              caller
            FROM
              phone_calls
            WHERE
              month = 7
              AND day = 28
              AND duration < 60
          )
      )
  )
```
