
## University Database
department(dept_no,dept_name,phone_no,email)<br>
student(student_no,student_name,address,age,dept_no)<br>
faculty(faculty_id,faculty_name,dept_no,course)<br>

### Creating database
```bash
create database university;
```
### Creating tables
```sql
create table department(dept_no int primary key,dept_name varchar(30),phone_no varchar(20),email varchar(20));
create table student(student_no int primary key,student_name varchar(30),address text,age int,dept_no int,foreign key(dept_no) references department(dept_no));
create table faculty(faculty_id int primary key,faculty_name varchar(30),dept_no int,course varchar(20), foreign key(dept_no) references department(dept_no));
```
### Inserting data
*department*
```sql
insert into department values (1, 'MCA', '9195520044', 'mca@dbc.in');
insert into department values (2, 'BBA', '7697054977', 'bba@dbc.in');
insert into department values (3, 'BCOM', '5083318228', 'bcom@dbc.in');
insert into department values (4, 'BA', '2178366030', 'ba@dbc.in');
insert into department values (5, 'BCA', '8282098458', 'bca@dbc.in');
```

```
 dept_no | dept_name |  phone_no  |    email    
---------+-----------+------------+-------------
       1 | MCA       | 9195520044 | mca@dbc.in
       2 | BBA       | 7697054977 | bba@dbc.in
       3 | BCOM      | 5083318228 | bcom@dbc.in
       4 | BA        | 2178366030 | ba@dbc.in
       5 | BCA       | 8282098458 | bca@dbc.in
(5 rows)

```
*student*
```sql
insert into student values (1, 'Peter Parker', 'Queens, New York', 20,1);
insert into student values (2, 'Tony Stark', 'Manhattan, New York', 24,1);
insert into student values (3, 'Bruce Banner', 'Brooklyn, New York', 22,3);
insert into student values (4, 'Steve Rogers', 'Los Angeles', 25,4);
insert into student values (5, 'Sam Wilson', 'Brooklyn, New York',23,5);
```
```
 student_no | student_name |       address       | age | dept_no 
------------+--------------+---------------------+-----+---------
          1 | Peter Parker | Queens, New York    |  20 |       1
          2 | Tony Stark   | Manhattan, New York |  24 |       1
          3 | Bruce Banner | Brooklyn, New York  |  22 |       3
          4 | Steve Rogers | Los Angeles         |  25 |       4
          5 | Sam Wilson   | Brooklyn, New York  |  23 |       5
(5 rows)
```
*faculty*
```sql
insert into faculty values (1, 'Howard Stark', 1,'Java');
insert into faculty values(2,'Barry Allen',1,'DBMS');
insert into faculty values (3, 'Ebard Thawnr',3,'Accounting');
insert into faculty values (4, 'Harisson Wells', 4,'English');
insert into faculty values (5, 'Cisco Ramon', 5,'Digital Electronics');
```
```
 faculty_id |  faculty_name  | dept_no |       course        
------------+----------------+---------+---------------------
          1 | Howard Stark   |       1 | Java
          2 | Barry Allen    |       1 | DBMS
          3 | Ebard Thawnr   |       3 | Accounting
          4 | Harisson Wells |       4 | English
          5 | Cisco Ramon    |       5 | Digital Electronics
(5 rows)
```
## Problem Statements
**1. List the name of student along with the department name.**
```sql
select s.student_name,d.dept_name from student s,department d where s.dept_no=d.dept_no;
```
```
 student_name | dept_name 
--------------+-----------
 Peter Parker | MCA
 Tony Stark   | MCA
 Bruce Banner | BCOM
 Steve Rogers | BA
 Sam Wilson   | BCA
(5 rows)
```

**2. List the department name and number of students in each department.**
```sql
select d.dept_name,count(s.student_no) from department d,student s where s.dept_no=d.dept_no group by d.dept_no;
```
```
 dept_name | count 
-----------+-------
 BCA       |     1
 BA        |     1
 MCA       |     2
 BCOM      |     1
(4 rows)
```
**3. List name of the student who was taught by a particular faculty.**
```sql
select student_name from student where dept_no in (select dept_no from faculty where faculty_name='Barry Allen');
```
```
 student_name 
--------------
 Peter Parker
 Tony Stark
(2 rows)
```
**4. List name of the faculty who teaches in particular department**
```sql
select faculty_name from faculty where dept_no in(select dept_no from department where dept_name='MCA');
```
```
faculty_name 
--------------
 Howard Stark
 Barry Allen
(2 rows)
```
**5. Create a trigger which will display a warning message while you entering age less than zero**

*Create the function*
``` bash
university=# \q
postgres@extrimis:~$ vi function.sql
```
```sql
create or replace function age() returns trigger as
$$ begin
if(new.age<0) then
raise exception 'Invalid age !!!';
end if;
return new;
end
$$ language plpgsql
```
*Import the function*
```bash
postgres@extrimis:~$ psql university;
psql (14.5 (Ubuntu 14.5-1ubuntu1))
Type "help" for help.

university=# \i function.sql
CREATE FUNCTION
```
*Create the trigger*
```bash
university=# \q
postgres@extrimis:~$ vi trigger.sql
```
```sql
create trigger age_check before insert on student
for each row 
execute procedure age()
```
*Import the trigger*
```bash
postgres@extrimis:~$ psql university;
psql (14.5 (Ubuntu 14.5-1ubuntu1))
Type "help" for help.

university=# \i trigger.sql;
CREATE TRIGGER
```
*Testing the function and trigger*
```sql
insert into student values (1, 'Thanos', 'Titan', -20,1);
```
```
ERROR:  Invalid age !!!
```
