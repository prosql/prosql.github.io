---
lang: bg
layout: article
title: Подзапроси
---

Нека изведем всички полети от Ростов:

    SELECT
    	flights.flight_id,
      flights.flight_no,
      flights.scheduled_departure
    FROM
    	bookings.flights
    WHERE
    	departure_airport='ROV'

Имаме също таблица с информация за това на кои полети са продадени билети `ticket_flights`. Нека си припомним групирането и получим броя на продадените билети за всеки полет:

    SELECT
    	ticket_flights.flight_id,
      count(*) "Билети"
    FROM
    	bookings.ticket_flights
    GROUP BY
    	ticket_flights.flight_id

Както присоединихме таблица с помощта на `JOIN`, можем също да присоединим цял запрос. Такъв присоединен запрос ще се нарича *подзапрос*:

    SELECT
      flights.flight_id,
      flights.flight_no,
      flights.scheduled_departure,
      flights.aircraft_code,
      tickets_count."Билети"
    FROM
      bookings.flights
      JOIN
        (
          SELECT
          	ticket_flights.flight_id,
            count(*) "Билети"
          FROM
          	bookings.ticket_flights
          GROUP BY
          	ticket_flights.flight_id
        ) AS tickets_count
          ON tickets_count.flight_id=flights.flight_id
    WHERE
      departure_airport='ROV'

Подзапросът трябва да бъде затворен в скобки и даден псевдоним. През този псевдоним се обръщаме към резултатите на подзапроса по същия начин, както към полетата на обикновена таблица.

Същото нещо може да бъде написано различно, използвайки конструкцията `WITH`:

    WITH
      tickets_count AS
        (
          SELECT
            ticket_flights.flight_id,
            count(*) "Билети"
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
      tickets_count."Билети"
    FROM
      bookings.flights
      JOIN tickets_count
          ON tickets_count.flight_id=flights.flight_id
    WHERE
      departure_airport='ROV'

Тук, след `WITH` деклариме подзапроса `tickets_count`, който използваме по-късно след `FROM`.

Нека добавим информация за броя на издадените посадъчни талони за полетите:

    WITH
      tickets_count AS
        (
          SELECT
            ticket_flights.flight_id,
            count(*) "Билети"
          FROM
            bookings.ticket_flights
          GROUP BY
            ticket_flights.flight_id
        ),
      passes_count AS
        (
          SELECT
            boarding_passes.flight_id,
            count(*) "Посадъчни талони"
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
      tickets_count."Билети",
      passes_count."Посадъчни талони"
    FROM
      bookings.flights
      JOIN tickets_count
        ON tickets_count.flight_id=flights.flight_id
      JOIN passes_count
        ON passes_count.flight_id=flights.flight_id
    WHERE
      departure_airport='ROV'

Броят на посадъчните талони съответства на броя на билетите за полет, ако всички пътници са се регистрирали.

По този начин можете да създадете доста сложни запросите.
