---
lang: bg
layout: article
title: Сортиране на резултатите
---

По подразбиране, редовете в резултата от запрос не са подредени. За да ги изведем в конкретен ред, използваме фразата `ORDER BY`. След нея изредяваме изрази, разделени със запетаи, според които трябва да сортираме резултата. Например, списък на самолетите по наименование на модела:

    SELECT *
    FROM bookings.aircrafts
    ORDER BY model

или по код:

    SELECT *
    FROM bookings.aircrafts
    ORDER BY aircraft_code

Нека първо изведем тези самолети, чийто обхват е по-малко от 6000 км. Изразът `(range > 6000)` връща `ИСТИНА` или `ЛЪЖА`. При сортиране първо ще бъдат изведени редовете, където този израз е лъжа, след това тези, където е истина:

    SELECT *
    FROM bookings.aircrafts
    ORDER BY (range > 6000)

Сред самолетите с обхват до 6000 и над 6000, нека да сортираме по модел. Тоест, първо сортирайте по това дали самолета може да лети до 6000 км или повече от 6000, след това по модел:

    SELECT *
    FROM bookings.aircrafts
    ORDER BY
      (range > 6000),
      model

За да обърнете редът на сортиране след израз, използвайте думата `DESC`:

    SELECT *
    FROM bookings.aircrafts
    ORDER BY
      (range > 6000) DESC,
      model

ще изведе първо тези самолети с обхват повече от 6000 км.

    SELECT *
    FROM bookings.aircrafts
    ORDER BY
      (range > 6000),
      model DESC

ще сортира моделите в обратен ред. Можете да използвате `DESC` и в двата израза:

    SELECT *
    FROM bookings.aircrafts
    ORDER BY
      (range > 6000) DESC,
      model DESC

Фразата `ORDER BY` се поставя след фразите `WHERE` и `GROUP BY`:

    SELECT *
    FROM bookings.aircrafts
    WHERE range > 2000
    ORDER BY aircraft_code

Нека сортираме резултатите от запроса от предишния урок. Първо по код на самолета, след това между еднакви самолети - по номер на полета в обратен ред, и между еднакви полети - по дата на отправление:

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
    ORDER BY
      aircraft_code,
      flight_no DESC,
      scheduled_departure
