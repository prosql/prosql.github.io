---
lang: en
layout: article
title: Output Data
---

The query begins with the `SELECT` keyword. After it, the data we want to see in the query results are listed separated by commas.

Examples:

	SELECT 258

will output the number `258`

	SELECT 'Elena'

will output the text `Elena`. Note that text must be enclosed in single quotes.

	SELECT current_timestamp

but here `current_timestamp` without quotes. This is a variable name. The `current_timestamp` variable always contains the current date and time, which will be output as the result.

We can also calculate expressions:

	SELECT 11*5

will output `55`

	SELECT 'Road ' || '60'

will output `Road 60`. The operator `||` concatenates text.

	SELECT 'Road ' || 60

will give an error. Did you notice the difference? `60` is not enclosed in quotes and is treated as a number. You cannot concatenate a number to text.

	SELECT 2.5

will output the number `2.5`

	SELECT 2,5

will output two numbers: `2` and `5`. Remember that comma is a data separator?

	SELECT 2, 5, 84, 3.7

will output 4 numbers

	SELECT 3, 'river', 'Omsk region', current_timestamp

will output a number, two text values and a date with time.
