---
lang: bg
layout: article
title: Условия
---

За примерите по-долу ще използвам образователната база данни [Полети](https://edu.postgrespro.ru/bookings.pdf).

    SELECT *
    FROM bookings.aircrafts

извежда всички съдържание на таблицата `aircrafts` на схемата `bookings`

За да изведете данни само за конкретен самолет, използвайте ключовата дума `WHERE`, след която се посочва условие:

    SELECT *
    FROM bookings.aircrafts
    WHERE aircrafts.model = 'Airbus A321-200'

или

    SELECT *
    FROM bookings.aircrafts
    WHERE model = 'Airbus A321-200'

ще изведе данни за самолета `Airbus A321-200`

    SELECT *
    FROM bookings.aircrafts
    WHERE range > 5000

ще изведе самолети, които могат да летят повече от 5000 км.

Условията са изрази, които се прилагат към всеки ред на таблицата. Ако за един ред изразът е **истинит**, тогава този ред ще бъде изведен в резултата. Ако изразът за този ред е **лъжив**, тогава редът не се извежда.

    SELECT *
    FROM bookings.aircrafts
    WHERE 'Airbus A321-200'

ще даде грешка, защото изразът след `WHERE` не може да бъде изчислен като **истинит** или **лъжив**.
Проверете:

    SELECT
      aircrafts.model,
      aircrafts.model = 'Airbus A321-200'
    FROM
      bookings.aircrafts

ще даде моделите на самолетите и резултата от израза `aircrafts.model = 'Airbus A321-200'` за всеки модел. Виждате, `true` или `false`.

Но ако напишете по този начин:

    SELECT
      aircrafts.model,
      'Airbus A321-200'
    FROM
      bookings.aircrafts

вторият израз няма да ни даде нито `true`, нито `false`, той връща текст. Не може да бъде използван в този вид в условието `WHERE`.

Условията могат да бъдат по-сложни

    SELECT *
    FROM bookings.aircrafts
    WHERE
      (model = 'Airbus A321-200')
      OR (range > 6000)

ще изведе Airbus A321-200, както и самолети, които летят повече от 6000 км.

    SELECT *
    FROM bookings.aircrafts
    WHERE
      (model = 'Airbus A321-200')
      AND (range > 6000)

ще върне празен резултат, защото нямаме самолет на модел 'Airbus A321-200', който да може да лети повече от 6000 км.
