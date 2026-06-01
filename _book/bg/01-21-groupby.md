---
lang: bg
layout: article
title: Групиране
---

Получихме списък на полетите за нашия модел самолет:

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

Полетите със същия номер отлитат на различни дни. Нека изберем само уникалните номера на полетите. За да направим това, след `SELECT` напишете `DISTINCT`:

    SELECT DISTINCT
      aircrafts.model,
      flights.flight_no
    FROM
      bookings.flights
      JOIN bookings.aircrafts
        ON aircrafts.aircraft_code = flights.aircraft_code
    WHERE
      aircrafts.model = 'Airbus A321-200'

Същите данни могат да се получат чрез групиране на селекцията по полетата `model` и `flight_no`:

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

Това изглежда по-многословно, но групирането дава допълнителни възможности. Например, можем да преброим колко пъти самолета е летял по един маршрут. Използвайте **функцията за агрегиране** `COUNT`:

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

И изведете датата и часа на отправление на първия и последния полет в нашата база данни. Нека също да си припомним псевдонимите:

    SELECT
      aircrafts.model "Модел",
      flights.flight_no "Полет",
      COUNT(*) "Полети",
      MIN(flights.scheduled_departure) "Първи",
      MAX(flights.scheduled_departure) "Последен"
    FROM
      bookings.flights
      JOIN bookings.aircrafts
        ON aircrafts.aircraft_code = flights.aircraft_code
    WHERE
      aircrafts.model = 'Airbus A321-200'
    GROUP BY
      aircrafts.model,
      flights.flight_no

Помните ли, когато извеждахме всички места на самолета:

	SELECT
		aircrafts.model,
		seats.seat_no
	FROM
		bookings.aircrafts
		JOIN bookings.seats
			ON seats.aircraft_code = aircrafts.aircraft_code
	WHERE
		aircrafts.model = 'Airbus A321-200'

Нека ги преброим:

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

И за всички модели самолети?

	SELECT
		aircrafts.model,
		COUNT(seats.seat_no) "Места"
	FROM
		bookings.aircrafts
		JOIN bookings.seats
			ON seats.aircraft_code = aircrafts.aircraft_code
	GROUP BY
		aircrafts.model
