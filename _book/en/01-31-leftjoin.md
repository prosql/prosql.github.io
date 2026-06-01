---
lang: en
layout: article
title: Non-Strict Join
---

You may have noticed that the query

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
      JOIN tickets_count
          ON tickets_count.flight_id=flights.flight_id
    WHERE
      departure_airport='ROV'

returns only those flights for which tickets were sold. What if we need to output *all* flights, even if no tickets were sold for them? In this case we need to join the `flights` table with the `tickets_count` subquery using `LEFT JOIN`:

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

Look, for example, for `flight_id` 13029, the expression `"Tickets"` is empty (`null`), because there was no record for this flight in the `tickets_count` subquery.

With `LEFT JOIN`, *all* rows from the left table (above, before) this phrase are output, even if no corresponding (according to the `ON` condition) record is found in the attached table (subquery).

With `JOIN`, from the left table only *those* records are output for which a match is found in the attached table.
