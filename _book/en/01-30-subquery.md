---
lang: en
layout: article
title: Subqueries
---

Let's output all flights from Rostov:

    SELECT
    	flights.flight_id,
      flights.flight_no,
      flights.scheduled_departure
    FROM
    	bookings.flights
    WHERE
    	departure_airport='ROV'

We also have a table with information about which flights have sold tickets `ticket_flights`. Let's remember grouping and get the number of sold tickets for each flight:

    SELECT
    	ticket_flights.flight_id,
      count(*) "Tickets"
    FROM
    	bookings.ticket_flights
    GROUP BY
    	ticket_flights.flight_id

Just as we attached a table using `JOIN`, we can also attach an entire query. Such an attached query will be called *subquery*:

    SELECT
      flights.flight_id,
      flights.flight_no,
      flights.scheduled_departure,
      flights.aircraft_code,
      tickets_count."Tickets"
    FROM
      bookings.flights
      JOIN
        (
          SELECT
          	ticket_flights.flight_id,
            count(*) "Tickets"
          FROM
          	bookings.ticket_flights
          GROUP BY
          	ticket_flights.flight_id
        ) AS tickets_count
          ON tickets_count.flight_id=flights.flight_id
    WHERE
      departure_airport='ROV'

A subquery must be enclosed in parentheses and given an alias. Through this alias we refer to the results of the subquery in the same way as to the fields of an ordinary table.

The same thing can be written differently using the `WITH` construction:

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

Here, after `WITH` we declare the subquery `tickets_count`, which we use later after `FROM`.

Let's add information about the number of boarding passes issued for flights:

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
        ),
      passes_count AS
        (
          SELECT
            boarding_passes.flight_id,
            count(*) "Boarding Passes"
          FROM
            bookings.boarding_passes
          GROUP BY
            boarding_passes.flight_id
        )
    SELECT
      flights.flight_id,
      flights.flight_no,
      flights.scheduled_departure,
      flights.aircraft_code,
      tickets_count."Tickets",
      passes_count."Boarding Passes"
    FROM
      bookings.flights
      JOIN tickets_count
        ON tickets_count.flight_id=flights.flight_id
      JOIN passes_count
        ON passes_count.flight_id=flights.flight_id
    WHERE
      departure_airport='ROV'

The number of boarding passes matches the number of tickets for a flight if all passengers have checked in.

Thus you can create quite complex queries.
