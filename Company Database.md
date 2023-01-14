

## Company Database
employee(emp_id,emp_name,salary,gender,dob,address,dept_no)<br>
department(dept_no,dept_name,start_date,emp_id)<br>
project(project_no,project_name,dept_no,emp_id,hours)<br>

### Creating database
```bash
create database company;
```
### Creating tables
```sql
create table employee(emp_id int primary key,emp_name varchar(30),salary int,gender varchar(10),dob date,address text,dept_no int);
create table department(dept_no int primary key,dept_name varchar(30),start_date date,emp_id int);
create table project(project_no int primary key,project_name varchar(30),dept_no int,emp_id int,hours int);
```

### Inserting data
> **Important Note:** Foreign keys are created at a later stage so make sure when you are inserting data into tables the data must be linked, ie; if you are adding a `dept_no` in department table those id's should be the only one used on the other tables, same for the case of `emp_id`.

*employee*
```sql
insert into employee values (1, 'Tony Stark', 100000, 'male','20-10-1978','Manhattan, New York',1);
insert into employee values (2, 'Peter Parker', 20000, 'male','13-04-1999','Queens, New York',1);
insert into employee values (3, 'Bruce Banner', 80000, 'male','02-05-1987','Los Angeles',2);
insert into employee values (4, 'Steve Rogers', 50000, 'male','30-12-1963','Brooklyn, New York',3);
insert into employee values (5, 'Sam Wilson', 45000, 'male','19-04-1977','Brooklyn, New York',4);
```
*department*
```sql
insert into department values (1, 'Software Testing', '13-04-2007',4);
insert into department values (2, 'System Engineer',  '15-03-2005',1);
insert into department values (3, 'Sales','23-12-2006',5);
insert into department values (4, 'IT Support','21-06-2010',2);
insert into department values (5, 'Software Developer', '03-07-2003',3);
```
*project*
```sql
insert into project values (1, 'Blog Design', 1,2,10);
insert into project values (2, 'Ecommerce Website',  3,3,35);
insert into project values (3, 'Billing Software',5,4,24);
insert into project values (4, 'Project Ultron',1,1,60);
insert into project values (5, 'Chatbot', 4,5,17);
```

### Add foreign keys
```sql
alter table employee add foreign key(dept_no) references department(dept_no);
alter table department add foreign key(emp_id) references employee(emp_id);
alter table project add foreign key(emp_id) references employee(emp_id);
alter table project add foreign key(dept_no) references department(dept_no);
```
### Problem Statements
**1. List employee name from employee table starting with S.** &nbsp;`Question modified`
```sql
select emp_name from employee where emp_name like 'S%';
```
```
   emp_name   
--------------
 Steve Rogers
 Sam Wilson
(2 rows)
```

**2. List the department name and department number along with number of employees working on each department.**
```sql
select d.dept_name,d.dept_no,count(e.emp_id) from department d,employee e where d.dept_no = e.dept_no group by d.dept_no;
```
```
    dept_name     | dept_no | count 
------------------+---------+-------
 IT Support       |       4 |     1
 System Engineer  |       2 |     1
 Software Testing |       1 |     2
 Sales            |       3 |     1
(4 rows)
```


**3. List the employees who are born in the month of april.**
```sql
select emp_name,dob from employee where extract(month from dob) = 4;
```
```
   emp_name   |    dob     
--------------+------------
 Peter Parker | 1999-04-13
 Sam Wilson   | 1977-04-19
(2 rows)
```

**4. List the employees name and employee id along with project number**
```sql
select e.emp_name,e.emp_id,p.project_no from employee e, project p where e.emp_id=p.emp_id;
```
```
   emp_name   | emp_id | project_no 
--------------+--------+------------
 Peter Parker |      2 |          1
 Bruce Banner |      3 |          2
 Steve Rogers |      4 |          3
 Tony Stark   |      1 |          4
 Sam Wilson   |      5 |          5
(5 rows)
```

**5. Create a function to count the number of departments, a given employee works for**

*Create the function*
```bash
company=# \q
postgres@extrimis:~$ vi function.sql
```
```sql
create or replace function dept_count(varchar(30)) returns integer as
$$ declare
total integer;
begin
select into total count(dept_no) from employee where emp_name=$1;
return total;
end
$$ language plpgsql;
```
*Import the function*
```bash
postgres@extrimis:~$ psql company;
psql (14.5 (Ubuntu 14.5-1ubuntu1))
Type "help" for help.

company=# \i function.sql
CREATE FUNCTION
```
*Testing the function*
```sql
select dept_count('Tony Stark');
```
```
 dept_count 
------------
          2
(1 row)
```
