

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
### Add foreign keys
```sql
alter table employee add foreign key(dept_no) references department(dept_no);
alter table department add foreign key(emp_id) references employee(emp_id);
alter table project add foreign key(emp_id) references employee(emp_id);
alter table project add foreign key(dept_no) references department(dept_no);
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

