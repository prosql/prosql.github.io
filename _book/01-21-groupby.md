---
layout: article
title: Группировка
---

Мы получили перечень рейсов, в которые летает наша модель самолёта:

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

В разные дни отправляются рейсы с одним номером. Выберем только уникальные номера рейсов. Для этого после `SELECT` напишем `DISTINCT`:

    SELECT DISTINCT
      aircrafts.model,
      flights.flight_no
    FROM
      bookings.flights
      JOIN bookings.aircrafts
        ON aircrafts.aircraft_code = flights.aircraft_code
    WHERE
      aircrafts.model = 'Airbus A321-200'

Те же данные можно получить, сгруппировав выборку по полям `model` и `flight_no`:

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

Выглядит более многословно, но группировка даёт дополнительные возможности. Например, можем посчитать сколько раз вообще летал самолёт по рейсу. Используем **агрегатную функцию** `COUNT`:

```SQL
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
```

И вывести дату и время отправления первого и последнего рейсов, занесённых в нашу базу. Давайте ещё вспомним про псевдонимы:

    SELECT
      aircrafts.model "Модель",
      flights.flight_no "Рейс",
      COUNT(*) "Рейсов",
      MIN(flights.scheduled_departure) "Первый",
      MAX(flights.scheduled_departure) "Последний"
    FROM
      bookings.flights
      JOIN bookings.aircrafts
        ON aircrafts.aircraft_code = flights.aircraft_code
    WHERE
      aircrafts.model = 'Airbus A321-200'
    GROUP BY
      aircrafts.model,
      flights.flight_no
