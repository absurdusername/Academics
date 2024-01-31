# Nested Queries

### Set membership

```sql
select ...
where col in (select ...);
```

### Set comparision

#### Some

To find instructors with salary greater than that of at least one instructor in the Biology department.

```sql
select name
from instructor
where salary > some (
    select salary
    from instructor
    where dept_name = 'Biology'
);
```
Also supported with `some()` are `>=`, `<`, `<=`, `<>`, and `=`.

`= some()` is identical to `in` and `<> some()` is identical to `not in`.

#### Any

To find the department with the highest average salary

```sql
select dept_name 
from instructor
group by dept_name
having avg(salary) > all(
    select avg(salary) 
    from instructor
    group by dept_name
);
```

#### Exists

```sql
select course
from section S
where semester = 'Fall' and year = 2009 and
    exists(
        select * 
        from section T
        where semester = 'Spring' and year = 2010 and
            S.course_id = T.course_id
    );
```

This is a correlation subquery, because `S` from the outer query is used in the subquery.

#### Test for uniqueness

```sql
select course_id
from course
where course_id in (
    select course_id 
    from section
    group by course_id
    having count(*) > 1
);
```

#### Lateral joins

[Explanation](https://docs.snowflake.com/en/sql-reference/constructs/join-lateral) by Snowflake

```sql
select I1.name, I1.salary, dept_avg_salary
from instructor I1, lateral (
    select avg(salary) as dept_avg_salary
    from instructor I2
    where I1.dept_name = I2.dept_name
);
```