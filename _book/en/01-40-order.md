---
lang: en
layout: article
title: Sorting Results
---

By default, rows in a query result are not ordered. To output them in a specific order, we use the `ORDER BY` phrase. After it, we list expressions separated by commas according to which we need to sort the result. For example, a list of aircraft by model name:

    SELECT *
    FROM bookings.aircrafts
    ORDER BY model

or by code:

    SELECT *
    FROM bookings.aircrafts
    ORDER BY aircraft_code

Let's first output those aircraft whose range is less than 6000 km. The expression `(range > 6000)` returns `TRUE` or `FALSE`. When sorting, rows where this expression is false will be output first, then those where it is true:

    SELECT *
    FROM bookings.aircrafts
    ORDER BY (range > 6000)

Among aircraft with range up to 6000 and over 6000, let's sort by model. That is, first sort by whether the aircraft can fly up to 6000 km or more than 6000, then by model:

    SELECT *
    FROM bookings.aircrafts
    ORDER BY
      (range > 6000),
      model

To reverse the sort order after an expression, use the word `DESC`:

    SELECT *
    FROM bookings.aircrafts
    ORDER BY
      (range > 6000) DESC,
      model

will output first those aircraft with a range of more than 6000 km.

    SELECT *
    FROM bookings.aircrafts
    ORDER BY
      (range > 6000),
      model DESC

will sort models in reverse order. You can use `DESC` in both expressions:

    SELECT *
    FROM bookings.aircrafts
    ORDER BY
      (range > 6000) DESC,
      model DESC

The `ORDER BY` phrase is placed after the `WHERE` and `GROUP BY` phrases:

    SELECT *
    FROM bookings.aircrafts
    WHERE range > 2000
    ORDER BY aircraft_code

Let's sort the results of the query from the previous lesson. First by aircraft code, then among identical aircraft - by flight number in reverse order, and among identical flights - by departure date:

    WITH
      tickets_count AS
        (
          SELECT
            ticket_flights.flight_id,
            count(*) "Tickets"
          FROM
            bookings.ticket_flights
          GROUP BY
            ticket_flights.flight_id
        )
    SELECT
      flights.flight_id,
      flights.flight_no,
      flights.scheduled_departure,
      flights.aircraft_code,
      tickets_count."Tickets"
    FROM
      bookings.flights
      LEFT JOIN tickets_count
          ON tickets_count.flight_id=flights.flight_id
    WHERE
      departure_airport='ROV'
    ORDER BY
      aircraft_code,
      flight_no DESC,
      scheduled_departure
