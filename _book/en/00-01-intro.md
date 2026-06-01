---
lang: en
layout: article
title: Introduction to Databases and SQL
---

The goal of [SQL](https://en.wikipedia.org/wiki/SQL) development was to create a simple language that any user could use, even without programming experience.

Example of SQL construction:

    SELECT region_code, name
    FROM cities

In [relational databases](https://en.wikipedia.org/wiki/Relational_database), data is placed in **tables** (*table*). Tables consist of **records** (rows), each having its own set of **fields** (columns) for each table. In our case, the `cities` table has the fields `region_code` and `name`.

SQL from different vendors: Microsoft SQL, Oracle, MySQL, PostgreSQL -- has its own features. Here we will use the PostgreSQL dialect. In the future, I plan to make notes on the features of other dialects.
