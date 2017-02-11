---
layout: article
title: Условия
---

Дальше для примеров буду использовать учебную базу [Полёты](https://edu.postgrespro.ru/bookings.pdf).

  SELECT *
  FROM bookings.aircrafts

выводит всё содержимое таблицы `aircrafts` схемы `bookings`

Чтобы вывести данные только по отдельному самолёту, используем ключевое слово `WHERE`, после которого указывается условие:

  SELECT *
  FROM bookings.aircrafts
  WHERE aircrafts.model = 'Airbus A321-200'

или

  SELECT *
  FROM bookings.aircrafts
  WHERE model = 'Airbus A321-200'

выведет данные по самолёту `Airbus A321-200`

  SELECT *
  FROM bookings.aircrafts
  WHERE range > 5000

выведет самолёты, которые могут летать на расстояние более 5000 км.

Условия - это выражения, которые применяются для каждой строки таблицы. Если для строки выражение **истинно**, то эта строка будет выведена в результате. Если выражение для этой строки **ложно**, то строка не выводится.

  SELECT *
  FROM bookings.aircrafts
  WHERE 'Airbus A321-200'

выдаст ошибку, так как выражение после `WHERE` не может быть рассчитано как **истинное** или **ложное**.
Проверьте:

  SELECT aircrafts.model, aircrafts.model = 'Airbus A321-200'
  FROM bookings.aircrafts

выдаст модели самолётов и результат выражения `aircrafts.model = 'Airbus A321-200'` для каждой модели. Видите, `true` или  `false`.

А если написать так:

  SELECT aircrafts.model, 'Airbus A321-200'
  FROM bookings.aircrafts

второе выражение не даст нам ни `true` или  `false`, оно возвращает текст. Его нельзя а таком виде использовать в условии `WHERE`.

Условия могут быть более сложными

  SELECT *
  FROM bookings.aircrafts
  WHERE (model = 'Airbus A321-200') OR (range > 6000)

выведет Airbus A321-200, а также самолёты, которые летают более чем на 6000 км.

  SELECT *
  FROM bookings.aircrafts
  WHERE (model = 'Airbus A321-200') AND (range > 6000)

выведет пустой результат, так как у нас нет самолётов модели 'Airbus A321-200', которые летают более чем на 6000 км.
