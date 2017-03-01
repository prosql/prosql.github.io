---
layout: article
title: Нестрогое соединение
---
Возможно, вы заметили, что запрос

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

возвращает только те рейсы, на которые были проданы билеты. А если нужно вывести *все* рейсы, пусть даже на них не было продано билетов? В таком случае нам нужно к таблице `flights` присоединить подзапрос `tickets_count` при помощи `LEFT JOIN`:

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
      LEFT JOIN tickets_count
          ON tickets_count.flight_id=flights.flight_id
    WHERE
      departure_airport='ROV'

Посмотрите, например, для `flight_id` 13029 в выражении `"Билетов"` стоит пусто (`null`), так как в подзапросе `tickets_count` не нашлось записи для этого рейса.

При `LEFT JOIN` выводятся *все* строки из таблицы слева (выше, до) этой фразы, даже если в присоединяемой таблице (подзапросе) не находится соответствующей (по условию `ON`) записи.

При `JOIN` из таблицы слева выводятся *только* те записи, которым найдено соответствие в присоединяемой таблице.
