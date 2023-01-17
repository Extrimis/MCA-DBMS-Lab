
## Customer Database
salesman(salesman_id,name,city,commission)<br>
customer(customer_id,customer_name,city,grade,salesman_id)<br>
orders(order_no,purchase_amount,order_date,customer_id,salesman_id)<br>

### Creating database
```bash
create database customer;
```
### Creating tables            
```sql
create table salesman(salesman_id int primary key,name varchar(30),city varchar(30),commission int);
create table customer(customer_id int primary key,customer_name varchar(30),city varchar(30),grade varchar(20),salesman_id int, foreign key(salesman_id) references salesman(salesman_id));
create table orders(order_no int primary key,purchase_amount int,order_date date,customer_id int,salesman_id int,foreign key(salesman_id) references salesman(salesman_id),foreign key(customer_id) references customer(customer_id));
```
### Inserting data
*salesman*
```sql
insert into salesman values(1, 'John Smith', 'New York', 500);
insert into salesman values(2, 'Jane Doe', 'Los Angeles', 700);
insert into salesman values(3, 'Bob Johnson', 'Chicago', 600);
insert into salesman values(4, 'Samantha Williams', 'Houston', 800);
insert into salesman values(5, 'Michael Brown', 'Philadelphia', 900);
```
*customer*
```sql
insert into customer values(1, 'Emily Williams', 'New York', 'A', 1);
insert into customer values(2, 'Michael Johnson', 'Los Angeles', 'B', 5);
insert into customer values(3, 'Jacob Smith', 'Chicago', 'A', 3);
insert into customer values(4, 'Olivia Jones', 'Houston', 'C', 2);
insert into customer values(5, 'Ethan Brown', 'Philadelphia', 'A', 4);
```
*orders*
```sql
insert into orders values(1, 5000, '2022-03-02', 1, 1);
insert into orders values(2, 2500, '2022-02-24', 5, 3);
insert into orders values(3, 1500, '2022-03-02', 3, 5);
insert into orders values(4, 1700, '2022-04-01', 2, 2);
insert into orders values(5, 1300, '2022-05-06', 4, 4);
```

## Problem Statements
**1. Write a SQL statement to prepare a list with salesman name,customer name and their cities for the salesman and customer who belongs to the same city**
```sql
select s.name,c.customer_name,s.city,c.city from salesman s,customer c where c.city = s.city;
```
or
```sql
select s.name,c.customer_name,s.city as "salesman city",c.city as "customer city" from salesman s,customer c where c.city = s.city;
```
```
       name        |  customer_name  | salesman city | customer city 
-------------------+-----------------+---------------+---------------
 Jane Doe          | Michael Johnson | Los Angeles   | Los Angeles
 Samantha Williams | Olivia Jones    | Houston       | Houston
(2 rows)
```

**2. Write SQL statement to make a list with order no,purchase amount,customer name and their cities for those orders which order amount between 500 and 2000**
```sql
select o.order_no,o.purchase_amount,c.customer_name,c.city from orders o,customer c where (o.purchase_amount between 500 and 2000) and o.customer_id = c.customer_id;
```
```
 order_no | purchase_amount |  customer_name  |    city     
----------+-----------------+-----------------+-------------
        4 |            1700 | Michael Johnson | Los Angeles
        3 |            1500 | Jacob Smith     | Tokyo
        5 |            1300 | Olivia Jones    | Houston
(3 rows)
```
**3. Write an sql statement to know which salesman are working for which customer**
```sql
select c.customer_name, s.name from customer c,salesman s where s.salesman_id=c.salesman_id;
```
```
  customer_name  |       name        
-----------------+-------------------
 Emily Williams  | John Smith
 Michael Johnson | Michael Brown
 Jacob Smith     | Bob Johnson
 Olivia Jones    | Jane Doe
 Ethan Brown     | Samantha Williams
(5 rows)
```
**4. List the name of customer and their cities in uppercase**
```sql
select upper(customer_name) as "customer name",upper(city) as "customer city" from customer;
```
or
```sql
select upper(customer_name),upper(city) from customer;
```
```
  customer name  | customer city 
-----------------+---------------
 EMILY WILLIAMS  | CAIRO
 MICHAEL JOHNSON | LOS ANGELES
 JACOB SMITH     | TOKYO
 OLIVIA JONES    | HOUSTON
 ETHAN BROWN     | TORONTO
(5 rows)
```
**5. Write a function to calculate total number of orders made on particular date**

*Create the function*
``` bash
customer=# \q
postgres@extrimis:~$ vi function.sql
```
```sql
create or replace function total_orders(dt date) returns integer as
$$ declare total integer;
begin
select count(order_no) into total from orders where order_date=dt;
return total;
end
$$ language plpgsql
```
*Import the function*
```bash
postgres@extrimis:~$ psql customer;
psql (14.5 (Ubuntu 14.5-1ubuntu1))
Type "help" for help.

customer=# \i function.sql
CREATE FUNCTION
```
*Testing the function*
```sql
select total_orders('2022-03-02');
```
```
 total_orders 
--------------
            2
(1 row)
```
