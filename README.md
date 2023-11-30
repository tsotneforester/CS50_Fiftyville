<a name="readme-top"></a>

<div align="center">
 <img src="https://github-production-user-asset-6210df.s3.amazonaws.com/79293287/286924597-7b39d1e2-0ada-4656-9a3d-4ff453fb805a.png" alt="logo">
 <br/>
 <h3 align="center">A Mystery in Fiftyville</h3>
 <br/>
</div>

<h1 align="center"> About The Project </h1>

_**"The root of learning is bitter, but the crown is sweet"**_.... - Ancient Georgian (:bow_and_arrow:ქართული:crossed_swords:) proverb.

After first steps in Web Development (HTML+CSS), it was Javascript, which really drove my nuts, but now thing are getting better as experience growth. On my Js journey, I've come up to many interesting tasks and I am going to share it to you! :partying_face::partying_face::partying_face:

## Getting Started

- Project GIF
- Project Name
- Main build language  
  ![html](https://img.shields.io/badge/-HTML-6abecd "image")
  ![css](https://img.shields.io/badge/-CSS-3e54a3 "image")
  ![js](https://img.shields.io/badge/-Vanilla%20JS-cf6390 "image")
  ![react](https://img.shields.io/badge/-React-f4cf0c "image")
  ![api](https://img.shields.io/badge/-API-aad742 "image")
- Difficulty Level  
  ![newbie](https://img.shields.io/badge/%201%20-newbie-white?labelColor=6abecd "image")
  ![junior](https://img.shields.io/badge/%202%20-junior-white?labelColor=aad742 "image")
  ![intermediate](https://img.shields.io/badge/%203%20-intermediate-white?labelColor=f1b604 "image")
  ![advanced](https://img.shields.io/badge/%204%20-advanced-white?labelColor=bf4605 "image")
  ![guru](https://img.shields.io/badge/%205%20-guru-white?labelColor=ed2c49 "image")
- Project Links
- Summery

### Built With

<p align="center">
  <a href="https://skillicons.dev">
    <img src="https://skills.thijs.gg/icons?i=js,html,css,sass,styledcomponents,react,codepen,figma,git,ps,vscode" />
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
