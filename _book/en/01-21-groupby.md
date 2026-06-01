---
lang: en
layout: article
title: Grouping
---

We got a list of flights for our aircraft model:

    SELECT
      aircrafts.model,
      flights.flight_no,
      flights.scheduled_departure
    FROM
      bookings.flights
      JOIN bookings.aircrafts
      ON aircrafts.aircraft_code = flights.aircraft_code
    WHERE
      aircrafts.model = 'Airbus A321-200'

Flights with the same number depart on different days. Let's select only the unique flight numbers. To do this, after `SELECT` write `DISTINCT`:

    SELECT DISTINCT
      aircrafts.model,
      flights.flight_no
    FROM
      bookings.flights
      JOIN bookings.aircrafts
        ON aircrafts.aircraft_code = flights.aircraft_code
    WHERE
      aircrafts.model = 'Airbus A321-200'

The same data can be obtained by grouping the selection by the `model` and `flight_no` fields:

    SELECT
      aircrafts.model,
      flights.flight_no
    FROM
      bookings.flights
      JOIN bookings.aircrafts
        ON aircrafts.aircraft_code = flights.aircraft_code
    WHERE
      aircrafts.model = 'Airbus A321-200'
    GROUP BY
      aircrafts.model,
      flights.flight_no

This looks more verbose, but grouping provides additional capabilities. For example, we can count how many times the aircraft flew on a route. Use the **aggregate function** `COUNT`:

	SELECT
	  aircrafts.model,
	  flights.flight_no,
	  COUNT(*)
	FROM
	  bookings.flights
	  JOIN bookings.aircrafts
	    ON aircrafts.aircraft_code = flights.aircraft_code
	WHERE
	  aircrafts.model = 'Airbus A321-200'
	GROUP BY
	  aircrafts.model,
	  flights.flight_no

And output the date and time of departure of the first and last flights in our database. Let's also remember aliases:

    SELECT
      aircrafts.model "Model",
      flights.flight_no "Flight",
      COUNT(*) "Flights",
      MIN(flights.scheduled_departure) "First",
      MAX(flights.scheduled_departure) "Last"
    FROM
      bookings.flights
      JOIN bookings.aircrafts
        ON aircrafts.aircraft_code = flights.aircraft_code
    WHERE
      aircrafts.model = 'Airbus A321-200'
    GROUP BY
      aircrafts.model,
      flights.flight_no

Remember when we output all seats on an aircraft:

	SELECT
		aircrafts.model,
		seats.seat_no
	FROM
		bookings.aircrafts
		JOIN bookings.seats
			ON seats.aircraft_code = aircrafts.aircraft_code
	WHERE
		aircrafts.model = 'Airbus A321-200'

Let's count them:

	SELECT
		aircrafts.model,
		COUNT(seats.seat_no) seats_count
	FROM
		bookings.aircrafts
		JOIN bookings.seats
			ON seats.aircraft_code = aircrafts.aircraft_code
	WHERE
		aircrafts.model = 'Airbus A321-200'
	GROUP BY
		aircrafts.model

And for all aircraft models?

	SELECT
		aircrafts.model,
		COUNT(seats.seat_no) "Seats"
	FROM
		bookings.aircrafts
		JOIN bookings.seats
			ON seats.aircraft_code = aircrafts.aircraft_code
	GROUP BY
		aircrafts.model
