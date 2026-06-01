---
lang: en
layout: article
title: Conditions
---

For the examples below, I will use the educational database [Flights](https://edu.postgrespro.ru/bookings.pdf).

    SELECT *
    FROM bookings.aircrafts

outputs all contents of the `aircrafts` table of the `bookings` schema

To output data for only a specific aircraft, use the `WHERE` keyword, after which a condition is specified:

    SELECT *
    FROM bookings.aircrafts
    WHERE aircrafts.model = 'Airbus A321-200'

or

    SELECT *
    FROM bookings.aircrafts
    WHERE model = 'Airbus A321-200'

will output data for the `Airbus A321-200` aircraft

    SELECT *
    FROM bookings.aircrafts
    WHERE range > 5000

will output aircraft that can fly more than 5000 km.

Conditions are expressions that apply to each row of the table. If for a row the expression is **true**, then that row will be output in the result. If the expression for that row is **false**, then the row is not output.

    SELECT *
    FROM bookings.aircrafts
    WHERE 'Airbus A321-200'

will give an error, because the expression after `WHERE` cannot be calculated as **true** or **false**.
Check:

    SELECT
      aircrafts.model,
      aircrafts.model = 'Airbus A321-200'
    FROM
      bookings.aircrafts

will give the aircraft models and the result of the expression `aircrafts.model = 'Airbus A321-200'` for each model. You see, `true` or `false`.

But if you write like this:

    SELECT
      aircrafts.model,
      'Airbus A321-200'
    FROM
      bookings.aircrafts

the second expression will not give us either `true` or `false`, it returns text. It cannot be used in this form in the `WHERE` condition.

Conditions can be more complex

    SELECT *
    FROM bookings.aircrafts
    WHERE
      (model = 'Airbus A321-200')
      OR (range > 6000)

will output Airbus A321-200, as well as aircraft that fly more than 6000 km.

    SELECT *
    FROM bookings.aircrafts
    WHERE
      (model = 'Airbus A321-200')
      AND (range > 6000)

will return an empty result, because we don't have any aircraft of the 'Airbus A321-200' model that can fly more than 6000 km.
