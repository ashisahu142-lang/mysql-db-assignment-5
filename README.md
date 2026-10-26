# MySQL DBMS Assignment 5: Subqueries

This repository rebuilds the supplied Assignment 5 from its original Word document. The source contains **22 numbered exercises** (the trailing “More to Come!” line is not a question). Each exercise has a standalone numbered query file, and a compact MySQL HR sample database makes the queries reproducible.

## Contents

- `assignment/dbms assignment 5.docx` — preserved copy of the supplied assignment.
- `sql/01.sql` through `sql/22.sql` — one answer per original exercise. Each file begins with the original prompt.
- `sql/00_schema.sql` and `sql/00_sample_data.sql` — schema and deterministic sample rows.
- The schema script is non-destructive but expects an empty database; it does not drop or replace existing tables.
- `docs/database-notes.md` — schema, assumptions, subquery concepts, and question-by-question notes.

## Requirements and setup

Use MySQL 8.0 or later. In a fresh local server, run:

```sh
mysql -u root -p < sql/00_schema.sql
mysql -u root -p hr_assignment5 < sql/00_sample_data.sql
```

The schema script creates and selects `hr_assignment5` without dropping databases or tables. Run it once against a fresh local database; if its tables already exist, choose a separate empty test database rather than deleting data. The sample-data script inserts fixed sample rows and is intended to be run once after schema creation.

Run an exercise, for example:

```sh
mysql -u root -p hr_assignment5 < sql/03.sql
```

Question 22 uses a session variable as the requested *n*; set it before the query, for example `SET @n := 3;`. Question 18 orders by `employee_id` as a stable proxy for “last”; relational tables have no inherent row order.

## Question-wise solution index

| Question | Original wording | Script |
|---:|---|---|
| 1 | Write a query to find the name (first_name, last_name) and the salary of the employees who have a higher salary than the employee whose last_name='Bull'. | [`01.sql`](sql/01.sql) |
| 2 | Write a query to find the name (first_name, last_name) of all employees who works in the IT department. | [`02.sql`](sql/02.sql) |
| 3 | Write a query to find the name (first_name, last_name) of the employees who have a manager and worked in a USA based department. Hint : Write single-row and multiple-row subqueries | [`03.sql`](sql/03.sql) |
| 4 | Write a query to find the name (first_name, last_name) of the employees who are managers. | [`04.sql`](sql/04.sql) |
| 5 | Write a query to find the name (first_name, last_name), and salary of the employees whose salary is greater than the average salary. | [`05.sql`](sql/05.sql) |
| 6 | Write a query to find the name (first_name, last_name), and salary of the employees whose salary is equal to the minimum salary for their job grade. | [`06.sql`](sql/06.sql) |
| 7 | Write a query to find the name (first_name, last_name), and salary of the employees who earns more than the average salary and works in any of the IT departments. | [`07.sql`](sql/07.sql) |
| 8 | Write a query to find the name (first_name, last_name), and salary of the employees who earns more than the earning of Mr. Bell. | [`08.sql`](sql/08.sql) |
| 9 | Write a query to find the name (first_name, last_name), and salary of the employees who earn the same salary as the minimum salary for all departments. | [`09.sql`](sql/09.sql) |
| 10 | Write a query to find the name (first_name, last_name), and salary of the employees whose salary is greater than the average salary of all departments. | [`10.sql`](sql/10.sql) |
| 11 | Write a query to find the name (first_name, last_name) and salary of the employees who earn a salary that is higher than the salary of all the Shipping Clerk (JOB_ID = 'SH_CLERK'). Sort the results of the salary of the lowest to highest. | [`11.sql`](sql/11.sql) |
| 12 | Write a query to find the name (first_name, last_name) of the employees who are not supervisors. | [`12.sql`](sql/12.sql) |
| 13 | Write a query to display the employee ID, first name, last name, and department names of all employees. | [`13.sql`](sql/13.sql) |
| 14 | Write a query to display the employee ID, first name, last name, salary of all employees whose salary is above average for their departments. | [`14.sql`](sql/14.sql) |
| 15 | Write a query to fetch even numbered records from employees table. | [`15.sql`](sql/15.sql) |
| 16 | Write a query to find the 5th maximum salary in the employees table. | [`16.sql`](sql/16.sql) |
| 17 | Write a query to find the 4th minimum salary in the employees table. | [`17.sql`](sql/17.sql) |
| 18 | Write a query to select last 10 records from a table. | [`18.sql`](sql/18.sql) |
| 19 | Write a query to list the department ID and name of all the departments where no employee is working. | [`19.sql`](sql/19.sql) |
| 20 | Write a query to get 3 maximum salaries. | [`20.sql`](sql/20.sql) |
| 21 | Write a query to get 3 minimum salaries. | [`21.sql`](sql/21.sql) |
| 22 | Write a query to get nth max salaries of employees. | [`22.sql`](sql/22.sql) |

## Correlated and non-correlated subqueries

A non-correlated subquery runs independently of the outer query and supplies a value or set, such as the company-wide average salary in `05.sql`. A correlated subquery refers to a row from its outer query and is evaluated in that row’s context, such as the department average in `14.sql` or the employee-to-department check in `19.sql`. `EXISTS` and `NOT EXISTS` are useful for testing related rows without depending on nullable `NOT IN` values.

## Scope and interpretation

The provided assignment describes the Oracle HR sample schema, but the Word document does not include its rows. The included MySQL schema retains the relevant HR table and column names and uses a compact, clearly synthetic dataset. It is sufficient to exercise the prompts, not a copy of Oracle's complete sample dataset. See `docs/database-notes.md` for choices made where the wording leaves room for interpretation.

The source wording is kept in each SQL file; spelling and grammar are intentionally not silently rewritten. “5th maximum” and “4th minimum” mean the fifth and fourth **distinct** salary values. “3 maximum/minimum salaries” returns the three highest/lowest **distinct** values. Question 15 means employees with even employee IDs. Question 20/21 list salary values rather than employee rows. Question 8 treats Mr. Bell as the employee with last name `Bell` and uses the greater-than predicate as written.
