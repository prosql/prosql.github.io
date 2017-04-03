---
layout: article
title: Сортировка результата
---
Строки в результате запроса по умолчанию не упроядочены. Чтобы их вывести в определённом порядке, используем фразу `ORDER BY`. После неё через запятую перечислим выражения, в соответствии с которыми надо отсортировать результат. Например, список самолётов по наименованию модели:

    SELECT *
    FROM bookings.aircrafts
    ORDER BY model

или по коду:

    SELECT *
    FROM bookings.aircrafts
    ORDER BY aircraft_code

Давайте сначала выведем те самолёты, у которых дальность менее 3000 км. Выражение `(range > 6000)` возвращает `ИСТИНА` или `ЛОЖЬ`. При сортировке сначала выведутся строки, у которых это выражение ложно, а после них - истинно:

    SELECT *
    FROM bookings.aircrafts
    ORDER BY (range > 6000)

Среди самолётов с дальностью до 6000 и более 6000 сделаем сортировку по модели. То есть, сначала отсортируем по признаку до 6000 км или дальше может летать самолёт, а затем по модели:

    SELECT *
    FROM bookings.aircrafts
    ORDER BY
      (range > 6000),
      model

Чтобы изменить порядок сортировки после выражения ставится слово `DESC`:

  SELECT *
  FROM bookings.aircrafts
  ORDER BY
    (range > 6000) DESC,
    model

выведет сначала самолёты с дальностью более 6000 км.

  SELECT *
  FROM bookings.aircrafts
  ORDER BY
    (range > 6000),
    model DESC

отсортирует модели в обратном порядке. Можно использовать `DESC` в обоих выражениях:

    SELECT *
    FROM bookings.aircrafts
    ORDER BY
      (range > 6000) DESC,
      model DESC

Фраза `ORDER BY` ставится после фразы `WHERE`:

    SELECT *
    FROM bookings.aircrafts
    WHERE range > 2000
    ORDER BY aircraft_code

Давайте упорядочим результаты запроса из предыдущего занятия. Сначала по коду самолёта, затем среди одинаковых самолётов - по номеру рейса в обратном порядке, а среди одинаковых рейсов - по дате вылета:

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
    ORDER BY
      aircraft_code,
      flight_no DESC,
      scheduled_departure
