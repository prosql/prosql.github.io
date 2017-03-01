---
layout: article
title: Подзапросы
---
Выведем все рейсы из Ростова:

    SELECT
    	flights.flight_id,
      flights.flight_no,
      flights.scheduled_departure
    FROM
    	bookings.flights
    WHERE
    	departure_airport='ROV'

У нас также есть таблица с информацией о том, на какие рейсы проданы билеты `ticket_flights`. Вспомним про группировку и получим количество проданных билетов на каждый из рейсов:

    SELECT
    	ticket_flights.flight_id,
      count(*) "Билетов"
    FROM
    	bookings.ticket_flights
    GROUP BY
    	ticket_flights.flight_id

Так же, как мы присоединяли таблицу при помощи `JOIN`, можно присоединить и целый запрос. Такой присоединённый запрос будем называть *подзапросом*:

    SELECT
      flights.flight_id,
      flights.flight_no,
      flights.scheduled_departure,
      flights.aircraft_code,
      tickets_count."Билетов"
    FROM
      bookings.flights
      JOIN
        (
          SELECT
          	ticket_flights.flight_id,
            count(*) "Билетов"
          FROM
          	bookings.ticket_flights
          GROUP BY
          	ticket_flights.flight_id
        ) AS tickets_count
          ON tickets_count.flight_id=flights.flight_id
    WHERE
      departure_airport='ROV'

Подзапрос обязательно нужно заключить в скобки и присвоить ему псевдоним. По этому псевдониму мы обращаемся с результатами подзапроса так же, как и с полями обычной таблицы.

То же самое можно записать иначе при помощи конструкции `WITH`:

    WITH
      tickets_count AS
        (
          SELECT
            ticket_flights.flight_id,
            count(*) "Билетов"
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
      tickets_count."Билетов"
    FROM
      bookings.flights
      JOIN tickets_count
          ON tickets_count.flight_id=flights.flight_id
    WHERE
      departure_airport='ROV'

Здесь мы после `WITH` объявляем подзапрос `tickets_count`, который используем дальше после `FROM`.

Добавим сюда информацию о количестве выданных посадочных талонов на рейсы:

    WITH
      tickets_count AS
        (
          SELECT
            ticket_flights.flight_id,
            count(*) "Билетов"
          FROM
            bookings.ticket_flights
          GROUP BY
            ticket_flights.flight_id
        ),
      passes_count AS
        (
          SELECT
            boarding_passes.flight_id,
            count(*) "Посадочных талонов"
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
      tickets_count."Билетов",
      passes_count."Посадочных талонов"
    FROM
      bookings.flights
      JOIN tickets_count
        ON tickets_count.flight_id=flights.flight_id
      JOIN passes_count
        ON passes_count.flight_id=flights.flight_id
    WHERE
      departure_airport='ROV'

Количество посадочных талонов совпадает с количеством билетов на рейс, если все пассажиры прошли регистрацию.

Таким образом можно создавать довольно сложные запросы.
