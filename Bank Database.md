## Bank Database
customer(cname,street,age)<br>
loan(lid,ac_no,bname,loanamount)<br>
depositor(cname,deposit_amount)<br>
borrower(cname varchar(30),lid int, foreign key(lid) references loan(lid))<br>

### Creating database
```bash
create database bank;
```
### Creating tables
```sql
create table customer(cname varchar(30),street varchar(30),age int);
create table loan(lid int primary key,ac_no int,bname varchar(30),loanamount int);
create table depositor(cname varchar(30),deposit_amount int);
create table borrower(cname varchar(30),lid int, foreign key(lid) references loan(lid));
```

### Inserting data
*customer*
```sql
insert into customer values('John Doe', 'Main St', 35);
insert into customer values('Jane Smith', 'Elm St', 29);
insert into customer values('Bob Johnson', 'Maple Ave', 42);
insert into customer values('Samantha Williams', 'Oak St', 25);
insert into customer values('Michael Brown', 'Pine St', 31);
```
*loan*
```sql
insert into loan values(1, 1013487, 'Perryridge', 50000);
insert into loan values(2, 1043582, 'Midtown Branch', 80000);
insert into loan values(3, 1051215, 'Community Branch', 90000);
```
*depositor*
```sql
insert into depositor values('Jane Smith', 15000);
insert into depositor values('Bob Johnson', 20000);
insert into depositor values('Samantha Williams', 25000);
```
*borrower*
```sql
insert into borrower values('John Doe', 1);
insert into borrower values('Jane Smith', 2);
insert into borrower values('Michael Brown', 3);
```

### Problem Statements
**1. Find the name of all customers who have a loan, an account or both from the bank**
```sql
select cname from depositor union select cname from borrower;
```
```
       cname       
-------------------
 Jane Smith
 Samantha Williams
 Bob Johnson
 Michael Brown
 John Doe
(5 rows)
```

**2. Find the name of all customers who have a loan and an account**
```sql
select cname from borrower where cname in(select cname from depositor);
```
```
   cname    
------------
 Jane Smith
(1 row)
```


**3. Find the name of all customers who have a loan at Perryridge branch**
```sql
select cname from borrower where lid in (select lid from loan where bname='Perryridge');
```
```
  cname   
----------
 John Doe
(1 row)
```

**4. Find name of all customers who have a loan at the bank, along with the loan number and the loan amount**
```sql
select b.cname,l.lid,l.loanamount from borrower b,loan l where b.lid=l.lid;
```
```
     cname     | lid | loanamount 
---------------+-----+------------
 John Doe      |   1 |      50000
 Jane Smith    |   2 |      80000
 Michael Brown |   3 |      90000
(3 rows)
```

