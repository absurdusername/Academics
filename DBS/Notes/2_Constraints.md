# Constraints

#### Creating tables with constraints

```sql
create table employees(
    id				int             primary key,
    another_id		int             unique,
    name			varchar(40)     not null,
    department      char(3)         default('DEF') check(department in ('ABC', 'DEF'))
);

create table slaves(
    slave_id	int,
    primary key(slave_id),
    foreign key(slave_id) references employees,
    foreign key(slave_id) references another_table on delete cascade
);

create table another_way(
    id  int references employees(id) on delete cascade
);
```

#### Externally adding named constraints

```sql
alter table slaves
add constraint constraint_name
check (id != 0);

alter table slaves
drop constraint constraint_name;
```

#### Externally adding unnamed constraints
These could have easily been named constrained, by the way.

```sql
alter table slaves
add check(id != 0);

alter table slaves
add primary key(id);

alter table slaves
add foreign key(id) references some_other_table;
```

#### To externally add a not null or default constraint
```sql
alter table slaves
modify id not null;

alter table slaves 
modify salary default 1000;
```