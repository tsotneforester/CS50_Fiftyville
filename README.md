<a href="https://cdn.cs50.net/2022/fall/psets/7/fiftyville.zip">DB</a>

<h1 align="center">A Mystery in Fiftyville</h1>
<div align="center">
 <img src="https://github-production-user-asset-6210df.s3.amazonaws.com/79293287/286924597-7b39d1e2-0ada-4656-9a3d-4ff453fb805a.png" alt="logo">
 <br/>
 
</div>

The CS50 Duck has been stolen! The town of Fiftyville has called upon you to solve the mystery of the stolen duck. Authorities believe that the thief stole the duck and then, shortly afterwards, took a flight out of town with the help of an accomplice.

### Your goal is to identify:

<br/>

![thief](https://img.shields.io/badge/-Who%20the%20thief%20is-f4cf0c "image")<br/>
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

### Prerequisites

npm (for React Projects)

```sh
npm install npm@latest -g
```

### Installation & Usage

For React Projects

1. In REACT folder install NPM package

Get Crime log

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

witness interviews

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

ID - ATM Withdraval

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

Phone Calls

```sql
SELECT
  id
FROM
  phone_calls
WHERE
  month = 7
  AND day = 28
  AND duration < 60;
```

Number Plates

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

Earliest Flight (36)

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

Thief ESCAPED TO:

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

Passport Numbers from flight

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

THIEF is:

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

ACCOMPLICE Phone Number

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

ACCOMPLICE is:

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
