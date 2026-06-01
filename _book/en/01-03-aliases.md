---
lang: en
layout: article
title: Aliases
---

Output data can be given aliases

	SELECT 'Elena' AS first_name

will output the text `Elena` with the header `first_name`

	SELECT 'Elena' AS first_name, 18 AS age

will output the text `Elena` with the header `first_name`, and the number `18` with the header `age`.

The keyword `AS` can be omitted:

	SELECT 'Elena' first_name, 18 age

will give the same result.

In aliases you can use not only Latin letters, but also spaces and Cyrillic. In this case the alias must be enclosed in double quotes:

	SELECT
		'Elena' "Name",
		9*2 "Age",
		'red' || 'car' "color of auto"

By the way, in Microsoft SQL Server, square brackets are used for aliases instead of double quotes:

	SELECT
		'Elena' [Name],
		9*2 [Age],
		'red' || 'car' [color of auto]

And in MySQL - backticks:

	SELECT
		'Elena' `Name`,
		9*2 `Age`,
		'red' || 'car' `color of auto`
