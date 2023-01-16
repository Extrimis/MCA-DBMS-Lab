
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
insert into employee values(2, 'Jane Doe', 2);
insert into employee values(3, 'Bob Johnson', 2);
insert into employee values(4, 'Samantha Williams', 3);
insert into employee values(5, 'Michael Brown', 1);
```


*pay_details*
```sql
insert into pay_details values(1, 1, 50000, '2003-01-01');
insert into pay_details values(2, 2, 55000, '2027-02-01');
insert into pay_details values(3, 2, 60000, '2025-03-01');
insert into pay_details values(4, 3, 65000, '2021-04-01');
insert into pay_details values(5, 1, 70000, '2020-05-01');
```

