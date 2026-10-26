# Database notes

## Source and scope

The preserved assignment document is the source of truth. It is titled “MySQL Subquery [22 exercises]” and contains 22 numbered prompts, followed by “More to Come!” and an HR database diagram. Every `sql/NN.sql` file reproduces its corresponding prompt in a comment. The source's stray replacement glyph between each number and “Write” is a Word formatting artifact; the question wording itself is preserved.

The diagram lists the familiar HR tables: `regions`, `countries`, `locations`, `departments`, `employees`, `jobs`, `job_history`, and `job_grades`. The exercises require country, location, department, employee, job, and salary-grade data. `00_schema.sql` creates these tables and their relevant foreign keys with MySQL types. `00_sample_data.sql` loads a deterministic synthetic dataset including `Bull`, `Bell`, IT departments, US locations, supervisors, departments without employees, and salary ties. It is intentionally smaller than Oracle's full HR sample data.

## Query interpretations

- Q3 asks for employees with a manager in a USA-based department and explicitly hints at single-row and multiple-row subqueries. The answer uses a scalar lookup of the US country key by its unique name plus `IN` subqueries for matching US locations and departments.
- Q4 identifies managers as employees referenced by another employee's `manager_id`.
- Q6 maps “job grade” to `job_grades`: salary locates the employee in the inclusive grade range, and the answer returns employees whose salary equals that grade row’s `lowest_sal` threshold. The diagram does not store a separate grade key on `employees`.
- Q8 selects employees whose salary exceeds the salary of the employee whose last name is `Bell`; if there are multiple Bells, the query uses `MAX` to avoid a scalar-subquery error.
- Q9 means employees paid the minimum salary value appearing in every department's employee group; the query compares the employee salary with the minimum department-level minimum. It does not require the employee to belong to every department.
- Q10 compares each salary to the average of all departmental average salaries (each department has equal weight). This makes “average salary of all departments” explicit.
- Q11's “higher than the salary of all” means greater than the maximum salary among `SH_CLERK` employees.
- Q12 means employee IDs never appear as another employee's `manager_id`.
- Q13 includes employees whose department is missing as well, showing a `NULL` department name via `LEFT JOIN`.
- Q15 interprets “even numbered records” as even `employee_id` values. Tables have no natural record order.
- Q16 and Q17 use distinct salary ranks, so ties do not consume additional rank positions.
- Q18 returns the last ten rows by descending `employee_id` (then displays them ascending). Without an ordering key, “last records” has no portable SQL meaning.
- Q20 and Q21 return exactly the three highest/lowest distinct salary values. If the intent is three employee rows with ties retained, a `DENSE_RANK` variant would be appropriate instead.
- Q22 returns the nth distinct salary value(s), with `@n` set by the caller; ties at that rank are all returned.

## Subquery concepts

A non-correlated subquery does not refer to the outer query. For example, Q5 compares every employee with one independently computed average. A correlated subquery refers to an outer row; Q14 computes the average for the current employee's department, and Q19 checks whether the current department has any employees. The inner expression therefore depends on which outer row is being considered.

`IN` checks membership in a result set; `EXISTS` checks whether at least one related row exists. `NOT EXISTS` expresses absence safely even when a candidate column might contain `NULL`. Scalar subqueries should return at most one row; aggregates such as `MAX` or `MIN` can enforce that where appropriate.

## Schema and execution

Run setup from the repository root:

```sh
mysql -u root -p < sql/00_schema.sql
mysql -u root -p hr_assignment5 < sql/00_sample_data.sql
```

Then run any one exercise with `mysql -u root -p hr_assignment5 < sql/NN.sql`. SQL files are independent read-only `SELECT` exercises. Q22 requires `SET @n := 3;` (replace 3 with the desired positive rank) in the same MySQL session before the query.
