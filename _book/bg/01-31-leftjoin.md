---
lang: bg
layout: article
title: Нестрого съединение
---

Може да сте забелязали, че запросът

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

връща само тези полети, за които са продадени билети. Какво ако трябва да изведем *всички* полети, дори и ако нямаше продадени билети за тях? В този случай трябва да присоединим таблицата `flights` с подзапроса `tickets_count`, използвайки `LEFT JOIN`:

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
      LEFT JOIN tickets_count
          ON tickets_count.flight_id=flights.flight_id
    WHERE
      departure_airport='ROV'

Вижте, например, за `flight_id` 13029 изразът `"Билети"` е празен (`null`), защото нямаше запис за този полет в подзапроса `tickets_count`.

С `LEFT JOIN`, *всички* редове от лявата таблица (горе, преди) тази фраза се извеждат, дори и ако няма намерено съответстващ (по условието `ON`) запис в присоединяваната таблица (подзапрос).

С `JOIN`, от лявата таблица се извеждат само *тези* записи, за които се намери съответствие в присоединяваната таблица.
