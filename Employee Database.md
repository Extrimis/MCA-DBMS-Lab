
## Employee Database
department(dep_id,dept_name)<br>
employee(emp_id,emp_name,dept_id)<br>
pay_details(emp_id,dept_id,basic_salary,date_of_joining)<br>

### Creating database
```bash
create database employee;
```
### Creating tables
```sql
create table department(dept_id int primary key,dept_name varchar(30));
create table employee(emp_id int primary key,emp_name varchar(30),dept_id int, foreign key(dept_id) references department(dept_id));
create table pay_details(emp_id int,dept_id int,basic_salary int,date_of_joining date, foreign key(emp_id) references employee(emp_id), foreign key(dept_id) references department(dept_id));
```
### Inserting data
*department*
```sql
insert into department values(1, 'IT');
insert into department values(2, 'HR'); 
insert into department values(3, 'Finance'); 
insert into department values(4, 'Marketing'); 
insert into department values(5, 'Operations');
```

*employee*
```sql
insert into employee values(1, 'John Smith', 1);
insert into employee values(2, 'Arcane', 2);
insert into employee values(3, 'Bob Johnson', 2);
insert into employee values(4, 'Samantha Williams', 3);
insert into employee values(5, 'Michael Brown', 1);
```


*pay_details*
```sql
insert into pay_details values(1, 1, 7000, '2003-01-01');
insert into pay_details values(2, 2, 12000, '2004-02-01');
insert into pay_details values(3, 2, 14000, '2005-03-01');
insert into pay_details values(4, 3, 35000, '2021-04-01');
insert into pay_details values(5, 1, 70000, '2020-05-01');
```
## Problem Statements
**1. List the employee's name which starts with 'A'.**
```sql
select emp_name from employee where emp_name like 'A%';
```
```
 emp_name 
----------
 Arcane
(1 row)
```
**2. List the employee details along with department.**
```sql
select e.*,d.dept_name from employee e,department d where d.dept_id = e.dept_id;
```
```
 emp_id |     emp_name      | dept_id | dept_name 
--------+-------------------+---------+-----------
      1 | John Smith        |       1 | IT
      3 | Bob Johnson       |       2 | HR
      4 | Samantha Williams |       3 | Finance
      5 | Michael Brown     |       1 | IT
      2 | Arcane            |       2 | HR
(5 rows)
```

**3. List the employee names that joined after a particular date**
```sql
select e.emp_name,p.date_of_joining from employee e,pay_details p where p.date_of_joining > '2005-03-01' and e.emp_id=p.emp_id;
```
```
     emp_name      | date_of_joining 
-------------------+-----------------
 Samantha Williams | 2021-04-01
 Michael Brown     | 2020-05-01
(2 rows)
```

**4. List the details of employees whode basic salary is between 10,000 and 20,000**
```sql
select e.*,p.basic_salary from employee e,pay_details p where (p.basic_salary between 10000 and 20000) and e.emp_id=p.emp_id;
```
```
emp_id |  emp_name   | dept_id | basic_salary 
--------+-------------+---------+--------------
      3 | Bob Johnson |       2 |        14000
      2 | Arcane      |       2 |        12000
(2 rows)
```

**5. Create a trigger which will display a warning message while you enter salary less than 1000**
*Create the function*
``` bash
employee=# \q
postgres@extrimis:~$ vi function.sql
```
*function.sql*
```sql
create or replace function salary() returns trigger as
$$ begin
if(new.basic_salary<1000) then
raise exception 'Salary amount is low !!!';
end if;
return new;
end
$$ language plpgsql
```
```
*Import the function*
```bash
postgres@extrimis:~$ psql employee;
psql (14.5 (Ubuntu 14.5-1ubuntu1))
Type "help" for help.

employee=# \i function.sql
CREATE FUNCTION
```
*Create the trigger*
```bash
employee=# \q
postgres@extrimis:~$ vi trigger.sql
```
```sql
create trigger salary_check before insert on pay_details
for each row 
execute procedure salary()
```
*Import the trigger*
```bash
postgres@extrimis:~$ psql employee;
psql (14.5 (Ubuntu 14.5-1ubuntu1))
Type "help" for help.

university=# \i trigger.sql;
CREATE TRIGGER
```
*Testing the function and trigger*
```sql
insert into pay_details values(5, 1, 700, '2020-05-01');
```
```
ERROR:  Salary amount is low !!!
```
