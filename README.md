# MySQL DBMS Assignment 5: Subqueries

This repository rebuilds the supplied Assignment 5 from its original Word document. The source contains **22 numbered exercises** (the trailing “More to Come!” line is not a question). Each exercise has a standalone numbered query file, and a compact MySQL HR sample database makes the queries reproducible.

## Contents

- `assignment/dbms assignment 5.docx` — preserved copy of the supplied assignment.
- `sql/01.sql` through `sql/22.sql` — one answer per original exercise. Each file begins with the original prompt.
- `sql/00_schema.sql` and `sql/00_sample_data.sql` — schema and deterministic sample rows.
- `docs/database-notes.md` — schema, assumptions, subquery concepts, and question-by-question notes.

## Requirements and setup

Use MySQL 8.0 or later. In a fresh local server, run:

```sh
mysql -u root -p < sql/00_schema.sql
mysql -u root -p hr_assignment5 < sql/00_sample_data.sql
```

The schema script creates and selects the `hr_assignment5` database. The sample-data script may be re-run after resetting the schema; it inserts fixed sample rows and is not designed as an idempotent migration.

Run an exercise, for example:

```sh
mysql -u root -p hr_assignment5 < sql/03.sql
```

Question 22 uses a session variable as the requested *n*; set it before the query, for example `SET @n := 3;`. Question 18 orders by `employee_id` as a stable proxy for “last”; relational tables have no inherent row order.

## Correlated and non-correlated subqueries

A non-correlated subquery runs independently of the outer query and supplies a value or set, such as the company-wide average salary in `05.sql`. A correlated subquery refers to a row from its outer query and is evaluated in that row’s context, such as the department average in `14.sql` or the employee-to-department check in `19.sql`. `EXISTS` and `NOT EXISTS` are useful for testing related rows without depending on nullable `NOT IN` values.

## Scope and interpretation

The provided assignment describes the Oracle HR sample schema, but the Word document does not include its rows. The included MySQL schema retains the relevant HR table and column names and uses a compact, clearly synthetic dataset. It is sufficient to exercise the prompts, not a copy of Oracle's complete sample dataset. See `docs/database-notes.md` for choices made where the wording leaves room for interpretation.

The source wording is kept in each SQL file; spelling and grammar are intentionally not silently rewritten. “5th maximum” and “4th minimum” mean the fifth and fourth **distinct** salary values. “3 maximum/minimum salaries” returns the three highest/lowest **distinct** values. Question 15 means employees with even employee IDs. Question 20/21 list salary values rather than employee rows. Question 8 treats Mr. Bell as the employee with last name `Bell` and uses the greater-than predicate as written.
