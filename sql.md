

# PL SQL Assignment
Solve the following
Loops example
1. Print the following patterns using loop :


a.
*
**
***
****
=> Solution

Delimiter //
Create procedure star_pattern(in rows_count INT)
Begin
Declare i int default 1;
Declare j int default 1;
Declare row_str varchar(255);

While i <= row_count do 
	Set j = 1;
	Set row_str=’ ’ ;
	
	While j <= i DO
		Set row_str = concat(row_str, ‘*’);
		Set j = j + 1;
	End while;

	Select row_str as pattern;
	Set i = i + 1;
	END WHILE
END  //













b.
*
***
*****
***
*


=> Solution

delimiter //

create procedure full_triangle(in row_count int)
begin
declare i int default 1;
declare j int default 1;
declare row_str varchar(255);

while i <= row_count do
set j = 1;
set row_str = '';

while j <= i do
set row_str = concat(row_str, '*');
set j = j + 1;
end while;

select row_str as pattern;
set i = i + 2;
end while;


set i = row_count - 2;

while i >= 1 do
set j = 1;
set row_str = '';

while j <= i do
set row_str = concat(row_str, '*');
set j = j + 1;
end while;

select row_str as pattern;
set i = i - 2;
end while;
end //

delimiter ;



c.
1010101
10101
101
1

=>

delimiter //

create procedure pattern_c(in row_count int)
begin
declare i int default 7;
declare j int default 1;
declare row_str varchar(255);

while i >= 1 do
set j = 1;
set row_str = '';

while j <= i do
if j % 2 = 1 then
set row_str = concat(row_str, '1');
else
set row_str = concat(row_str, '0');
end if;
set j = j + 1;
end while;

select row_str as pattern;
set i = i - 2;
end while;
end //

delimiter ;


d.
1
1 2
1 2 3
1 2 3 4
1 2 3 4 5

=> 

delimiter //

create procedure pattern_d(in row_count int)
begin
declare i int default 1;
declare j int default 1;
declare row_str varchar(255);

while i <= row_count do
set j = 1;
set row_str = '';

while j <= i do
if j = 1 then
set row_str = concat(row_str, j);
else
set row_str = concat(row_str, ' ', j);
end if;
set j = j + 1;
end while;

select row_str as pattern;
set i = i + 1;
end while;
end //

delimiter ;






2. write a procedure to insert record into employee table.
the procedure should accept empno, ename, sal, job, hiredate as input parameter
write insert statement inside procedure insert_rec to add one record into table


create procedure insert_rec(peno int,pnm varchar(20),psal decimal(9,2),pjob
varchar(20),phiredate date)
begin
insert into emp(empno,ename,sal,job,hiredate)
values(peno,pnm,psal,pjob,phiredate)
end//

delimiter ;

=> Solution
delimiter //
 create procedure insert_rec(in peno int, in pename varchar(20) , in psal decimal(9,2),in pjob varchar(20), in phiredate date)
 begin
 insert into EMP(empno,ename,sal,job,hiredate)
 values(peno,pename,psal,pjob,phiredate);
 end //

 delimiter ;

3. write a procedure to delete record from employee table.
the procedure should accept empno as input parameter.
write delete statement inside procedure delete_emp to delete one record from emp
Table

=> Solution
delimiter //
 create procedure del_rec(in peno int)
 begin
 delete from EMP where empno = peno;
 end //
delimiter ;






4. write a procedure to display empno,ename,deptno,dname for all employees with sal
> given salary. pass salary as a parameter to procedure

=> Solution

delimiter //

create procedure get_emp_details_by_sal(in p_sal decimal(10,2))
begin
select e.empno, e.ename, e.deptno, d.dname
from emp e
join dept d on e.deptno = d.deptno
where e.sal > p_sal;
end //

delimiter ;


5. write a procedure to find min,max,avg of salary and number of employees in the
given deptno.
deptno --→ in parameter
min,max,avg and count ---→ out type parameter
execute procedure and then display values min,max,avg and count

 => Solution
delimiter //
 create procedure get_arg(in pdeptno int)
 begin
 Select min(sal), max(sal), avg(sal), count(empno) from EMP
 where deptno = pdeptno;
 end //
delimiter ;

6. write a procedure to display all pid,pname,cid,cname and salesman name(use
product,category and salesman table)

=> Solution
delimiter //
create procedure show_info()
begin
select  p.pid , p.pname, c.cid, c.cnam , s.sname from product p 
inner join category c on p.cid = c.cid 
inner join salesman s on p.sid = s.sid;
end //

delimiter ;







7. write a procedure to display all vehicles bought by a customer. pass customer name
as a parameter.(use vehicle,salesman,custome and relation table)
=>
delimiter //

create procedure vehicle_info(in p_cname varchar(50))
begin
select v.vname from vehicle v
join relation r on v.vid = r.vid
join customer c on r.cid = c.cid
where c.cname = p_cname;
end //

delimiter ;


8. Write a procedure that displays the following information of all emp
Empno,Name,job,Salary,Status,deptno
Note: - Status will be (Greater, Lesser or Equal) respective to average salary of their own
department. Display an error message Emp table is empty if there is no matching
Record.
=>
delimiter //
create procedure show_emp()
begin 

if (select count(*) from EMP) = 0 then 
	signal sqlstate'45000'
	set message_text = 'Emp table is empty';

else
	select empno, ename, job, sal, e.deptno,
	case when sal > dept_avg then 'Greater'
		when sal < dept_avg then  'Lesser'
		else 'Equal'
	end as Status
	from EMP e
	inner join (select deptno, avg(sal) as dept_avg from EMP group by deptno) d on e.deptno = d.deptno;
end if;

end //

delimiter ;






9. Write a procedure to update salary in emp table based on following rules.
Exp< =35 then no Update
Exp> 35 and <=38 then 20% of salary
Exp> 38 then 25% of salary

=>
delimiter //
create procedure updt_sal(in exp int) 
begin
 update emp
set sal = 
if 
select datediff(curdate(),hiredate) from EMP 
end //

delimiter ;


10. Write a procedure and a function.
Function: write a function to calculate number of years of experience of employee.(note:
pass hiredate as a parameter)
Procedure: Capture the value returned by the above function to calculate the additional
allowance for the emp based on the experience.
Additional Allowance = Year of experience x 3000
Calculate the additional allowance
and store Empno, ename,Date of Joining, and Experience in
years and additional allowance in Emp_Allowance table.
create table emp_allowance(
empno int,
ename varchar(20),
hiredate date,
experience int,
allowance decimal(9,2));

11. Write a function to compute the following. Function should take sal and hiredate
as i/p and return the cost to company.

DA = 15% Salary, HRA= 20% of Salary, TA= 8% of Salary.
Special Allowance will be decided based on the service in the company.
< 1 Year Nil
>=1 Year< 2 Year 10% of Salary
>=2 Year< 4 Year 20% of Salary
>4 Year 30% of Salary

=> delimiter //

create function get_cost_to_company(p_sal decimal(10,2), p_hiredate date)
returns decimal(10,2)
deterministic
begin
declare v_years int;
declare v_da decimal(10,2);
declare v_hra decimal(10,2);
declare v_ta decimal(10,2);
declare v_special_allowance decimal(10,2);
declare v_ctc decimal(10,2);


set v_da = 0.15 * p_sal;
set v_hra = 0.20 * p_sal;
set v_ta = 0.08 * p_sal;


set v_years = timestampdiff(year, p_hiredate, curdate());

if v_years < 1 then
set v_special_allowance = 0.00;
elseif v_years >= 1 and v_years < 2 then
set v_special_allowance = 0.10 * p_sal;
elseif v_years >= 2 and v_years < 4 then
set v_special_allowance = 0.20 * p_sal;
else
set v_special_allowance = 0.30 * p_sal;
end if;


set v_ctc = p_sal + v_da + v_hra + v_ta + v_special_allowance;

return v_ctc;
end //

delimiter ;


12. Write query to display empno,ename,sal,cost to company for all employees(note:
use function written in question 10)

=>

select empno, ename, sal, get_cost_to_company(sal, hiredate) as cost_to_company
from emp;

Q2. Write trigger

1. Write a tigger to store the old salary details in Emp _Back (Emp _Back has the
same structure as emp table without any
constraint) table.
(note :create emp_back table before writing trigger)
----- to create emp_back table
create table emp_back(
empno int,
ename varchar(20),
oldsal decimal(9,2),
newsal decimal(9,2)
)
(note :
execute procedure written in Q8 and
check the entries in EMP_back table after execution of the procedure)

=>
create trigger sal_log after update on EMP for each row
insert into emp_back values(old.empno, old.ename, old.sal, new.sal);

2. Write a trigger which add entry in audit table when user tries to insert or delete
records in employee table store empno,name,username and date on which
operation performed and which action is done insert or delete. in emp_audit table.
create table before writing trigger.
create table empaudit(
empno int;
ename varchar(20),
username varchar(20);
chdate date;
action varchar(20)
);

=>


For Insertion 

create trigger trg_emp_audit_insert after insert on emp
for each row
begin
insert into empaudit (empno, ename, username, chdate, action)
values (new.empno, new.ename, user(), curdate(), 'insert');
end //

For Deletion

create trigger trg_emp_audit_delete after delete on emp 
for each row 
begin 
insert into empaudit (empno, ename, username, chdate, action) values (old.empno, old.ename, user(), curdate(), 'delete'); 
end // 




3. Create table vehicle_history. Write a trigger to store old vehicleprice and new vehicle
price in history table before you update price in vehicle table
(note: use vehicle table).
create table vehicle_history(
vno int,

vname varchar(20),
oldprice decimal(9,2),
newprice decimal(9,2),
chdate date,
username varchar(20)


=>

create trigger trg_vehicle_price_update before update on vehicle
for each row
begin
if old.price <> new.price then
insert into vehicle_history (vno, vname, oldprice, newprice, chdate, username)
values (old.vno, old.vname, old.price, new.price, curdate(), user());
end if;
end //
delimiter ;

# Index View Assignment

Assignment - Index and View

1. create all given tables
=> Solution

CREATE TABLE vehicle (
    vid INT PRIMARY KEY,
    vname VARCHAR(50) NOT NULL,
    price DECIMAL(10, 2) NOT NULL,
    description TEXT
);

CREATE TABLE customer (
    custid INT PRIMARY KEY,
    cname VARCHAR(50) NOT NULL,
    address VARCHAR(100) NOT NULL
);

CREATE TABLE salesman (
    sid INT PRIMARY KEY,
    sname VARCHAR(50) NOT NULL,
    address VARCHAR(100) NOT NULL
);

CREATE TABLE cust_vehicle (
    custid INT,
    vid INT,
    sid INT,
    buy_price DECIMAL(10, 2) NOT NULL,
    PRIMARY KEY (custid, vid, sid),
    FOREIGN KEY (custid) REFERENCES customer(custid) ON DELETE CASCADE,
    FOREIGN KEY (vid) REFERENCES vehicle(vid) ON DELETE CASCADE,
    FOREIGN KEY (sid) REFERENCES salesman(sid) ON DELETE CASCADE
);

INSERT INTO vehicle (vid, vname, price, description) VALUES
(1, 'Activa', 80000.00, 'ksldjfjksj'),
(2, 'Santro', 800000.00, 'kdjfkjsd'),
(3, 'Motor bike', 100000.00, 'fdkdfj');

INSERT INTO customer (custid, cname, address) VALUES
(1, 'Nilima', 'Pimpari'),
(2, 'Ganesh', 'Pune'),
(3, 'Pankaj', 'Mumbai');


INSERT INTO salesman (sid, sname, address) VALUES
(10, 'Rajesh', 'mumbai'),
(11, 'Seema', 'Pune'),
(13, 'Rakhi', 'pune');

INSERT INTO cust_vehicle (custid, vid, sid, buy_price) VALUES
(1, 1, 10, 75000.00),
(1, 2, 10, 790000.00),
(2, 3, 11, 80000.00),
(3, 3, 11, 75000.00),
(3, 2, 10, 800000.00);


2. create index on vehicle table based on price
=>Solution

create index idx_price 
on vehicle(price);

3. find all customer name,vehicle name, salesman name, discount earn by all customer
=>Solution

Select cname as customer_name, vname as vehicle_name , sname as salesman , round(((v.price - cv.buy_price) / v.price ) * 100, 2) Discount
From cust_vehicle cv
Inner join 
customer c on cv.custid = c.custid
Inner join salesman s
on cv.sid = s.sid
Inner join vehicle v 
On cv.vid = v.vid;


4. find all customer name,vehicle name,salesman name for all salesman who stays in pune
=> Solution 

Select c.cname as customer_name, v.vname as vehicle_name ,c.address  as City, sname as salesman , round(((v.price - cv.buy_price) / v.price ) * 100, 2) Discount
From cust_vehicle cv
Inner join 
customer c on cv.custid = c.custid
Inner join salesman s
on cv.sid = s.sid
Inner join vehicle v 
On cv.vid = v.vid
Where c.address = ‘pune’;


5. find how many customers bought motor bike
=>Solution

Select count(cv.custid)  From cust_vehicle cv
 Inner join 
 customer c on cv.custid = c.custid
 Inner join vehicle v 
 On cv.vid = v.vid
Where v.vname = ‘Motor bike’   

6. create a view find_discount which displays output
-------to create view
=>Solution

create view find_discount
as
select cname,vname,price, buy_price, round(((v.price - cv.buy_price) / v.price ) * 100, 2) Discount 
from customer c inner join cust_vehicle cv 
on c.custid=cv.custid 
inner join vehicle v 
On v.vid=cv.vid

--------to display discount

select * from find_discount;

7. find all customer name, vehicle name, salesman name, discount earn by all customer
=>Solution

Select cname as customer_name, vname as vehicle_name , sname as salesman , round(((v.price - cv.buy_price) / v.price ) * 100, 2) Discount
From cust_vehicle cv
Inner join 
customer c on cv.custid = c.custid
Inner join salesman s
on cv.sid = s.sid
Inner join vehicle v 
On cv.vid = v.vid

8. create view my_hr to display empno,ename,job,comm for all employees who earn
Commission
=>Solution

Create view my_hr 
As 
Select empno, ename, job , comm from EMP where ifnull(comm,0) > 0;

9. create view mgr30 to display all employees from department 30
=>Solution

Create view mgr30
As 
Select * from EMP where deptno = 30;

10. insert 3 employees in view mgr30 check whether insertion is possible
=>Solution

insert into mgr30 (empno, ename, job, mgr, hiredate, sal, comm, deptno) VALUES (7911,'ELON','CEO',7939,'1981-04-21', 3123.13, 1000,40);

11. insert 3 records in dept and display all records from dept
=>Solution 

INSERT INTO dept (deptno, dname, loc) 
VALUES 
(50, 'MARKETING', 'NEW YORK'),
(60, 'HR', 'CHICAGO'),
(70, 'FINANCE', 'BOSTON');

12. use rollback command check what happens
=>Solution 

rollback;

13. do the following


insert row in emp with empno 100
insert row in emp with empno 101
insert row in emp with empno 102
add savepoint A
insert row in emp with empno 103
insert row in emp with empno 104
insert row in emp with empno 105
add savepoint B
delete emp with empno 100
delete emp with emp no 104
rollback upto svaepoint B
check what all records will appear in employee table
rollback upto A
check what all records will appear in employee table
commit all changes
check what all records will appear in employee table
check whether you can roll back the contents.

=> Solution


START TRANSACTION;

INSERT INTO emp (empno) VALUES (100);
INSERT INTO emp (empno) VALUES (101);
INSERT INTO emp (empno) VALUES (102);


SAVEPOINT A;


INSERT INTO emp (empno) VALUES (103);
INSERT INTO emp (empno) VALUES (104);
INSERT INTO emp (empno) VALUES (105);


SAVEPOINT B;


DELETE FROM emp WHERE empno = 100;
DELETE FROM emp WHERE empno = 104;



ROLLBACK TO SAVEPOINT B;


SELECT * FROM emp;

ROLLBACK TO SAVEPOINT A;


SELECT * FROM emp;

COMMIT;

SELECT * FROM emp;

ROLLBACK;

SELECT * FROM emp;


14. create a procedure getMin(deptno,minsal) to find minimum salary of given table.
=>Solution

delimiter //
 create procedure getMin(in pdeptno int,out minsal double(9,2))
 begin
 Select min(sal) as "Minimium Salary" from EMP
 where deptno = pdeptno;
 end //
delimiter ;

# Cassandra Assignment

Cassandra Assignment Questions (Students)

1. Create a keyspace college.
=>
CREATE KEYSPACE college with replication = {'class':'SimpleStrategy','replication_factor':'1'};

2. Create a student table to save following details
Sid int, sname varchar,age, mobile, email text,courseid int, hobbies set of strings,
project is map keys are string and value is number, skill is list of string,marks
tuple(text,int,text,int),
Primary key courseid,sid
=>
create table student (
	sid int,
	sname varchar,
	age int,
	mobile bigint,
	email text,
	courseid int,
	hobbies set<text>,
	project map<text, int>,
	skill list<text>,
	marks tuple<text, int, text, int>,
	Primary key ((courseid),sid)
);

3. Insert 5 student records.
=>
INSERT INTO student (courseid, sid, sname, age, mobile, email, hobbies, project, skill, marks) 
VALUES (101, 1, 'Rahul', 21, 9876543210, 'rahul@mail.com', {'reading', 'gaming'}, {'DBMS': 1, 'AI': 2}, ['python', 'java'], ('DBMS', 85, 'AI', 90));

INSERT INTO student (courseid, sid, sname, age, mobile, email, hobbies, project, skill, marks) 
VALUES (101, 2, 'Priya', 22, 9876543211, 'priya@mail.com', {'dancing'}, {'WebDev': 1}, ['c++', 'python'], ('WebDev', 88, 'OS', 82));

INSERT INTO student (courseid, sid, sname, age, mobile, email, hobbies, project, skill, marks) 
VALUES (102, 1, 'Amit', 23, 9876543212, 'amit@mail.com', {'sports', 'cooking'}, {'Cloud': 3}, ['python', 'sql'], ('Cloud', 78, 'CN', 80));

INSERT INTO student (courseid, sid, sname, age, mobile, email, hobbies, project, skill, marks) 
VALUES (102, 3, 'Neha', 20, 9876543213, 'neha@mail.com', {'singing'}, {'ML': 1}, ['java', 'r'], ('ML', 95, 'Maths', 91));

INSERT INTO student (courseid, sid, sname, age, mobile, email, hobbies, project, skill, marks) 
VALUES (103, 5, 'Rohan', 24, 9876543214, 'rohan@mail.com', {'traveling'}, {'IoT': 2}, ['c', 'python'], ('IoT', 80, 'Embedded', 84));

4. Display all student records.
=>
select * from student;

5. Update marks to 92 of a student with id 2 courseid.
=>
update students set marks = 92
where courseid=2 ;

6. Delete one student record with id 2 and courseid 101.
=>
delete from student where courseid = 101 and sid = 2;

7. Delete email value for student with id 3 and courseid 102
=>
delete email from student where courseid=102 and sid=3; 

8. Add ‘java’,’.net skill for student 1 and courseid 102.
=>
update student on skil= skill + ['java','.net']
where courseid=102 and sid=1;

9. Remove skill python for student with id 1 and courseid 102
=>
update student on skill = skill - ['python'] where courseid=102 and sid=1;

Create a course table to store cid int, cname text, subject is a list of strings,skills is set of
strings, marks is key value pair to store { subject1:marks,subject2:marks}, marks indicates
minimum passing marks for each subject

create table course (
	cid int,
	cname text, 
	subject list<text>,
	skills set<text>,
	marks map<text,int>,
	Primary key (cNAME)
);


10. Insert 5 records in course table table.
=>
-- Record 1: DBDA
INSERT INTO course (cid, cname, subject, skills, marks)
VALUES (
    1, 
    'DBDA', 
    ['SQL', 'Python', 'Java'], 
    {'Database Management', 'Data Analysis', 'Python'}, 
    {'SQL': 40, 'Python': 50, 'Java': 40}
);

-- Record 2: DAC
INSERT INTO course (cid, cname, subject, skills, marks)
VALUES (
    2, 
    'DAC', 
    ['Java', 'C++', 'Web Dev'], 
    {'Coding', 'Python', 'Problem Solving'}, 
    {'Java': 40, 'C++': 35, 'Web Dev': 50}
);

-- Record 3: DITISS
INSERT INTO course (cid, cname, subject, skills, marks)
VALUES (
    3, 
    'DITISS', 
    ['Networking', 'Linux', 'Security'], 
    {'System Admin', 'Ethical Hacking', 'Networking'}, 
    {'Networking': 45, 'Linux': 40, 'Security': 50}
);

-- Record 4: DESD
INSERT INTO course (cid, cname, subject, skills, marks)
VALUES (
    4, 
    'DESD', 
    ['Microcontrollers', 'Embedded C', 'RTOS'], 
    {'Circuit Design', 'C Programming', 'Hardware Bugging'}, 
    {'Microcontrollers': 50, 'Embedded C': 45, 'RTOS': 40}
);

-- Record 5: AI
INSERT INTO course (cid, cname, subject, skills, marks)
VALUES (
    5, 
    'AI', 
    ['Maths', 'Machine Learning', 'Deep Learning'], 
    {'Statistics', 'Python', 'TensorFlow'}, 
    {'Maths': 50, 'Machine Learning': 60, 'Deep Learning': 55}
);

11. Add new subject Apptitude in course DBDA
=>
update  course
set subject = subject + ['Apptitude'] 
where cid = 1;

12. modify marks for aptitude subject to 90
=>
update course
set marks['Apptitude'] = 90 
where cid = 1;

13. Remove skill python from DAC course
=>
update course
set skill = skill - {'Python'}
where cid = 2 and cname = 'DAC';

14. Modify minimum passing marks to 45 for java
=>
update course 
set marks['Java'] = 45 
where cid = 2; 

Ecommerce System

1. Create a keyspace ecommerce with replication factor 1.
=>
create keyspace ecommerce with replication = {'class':'SimpleStrategy','replication_factor':'1'};


2. Use the keyspace ecommerce.

Create a table product with the following columns:
Column Name Data Type
product_id int (Primary Key)
name text
category text
price double
Column Name Data Type
stock int
created_at timestamp

=>

create table product (
product_id int,
name text,
category text,
price double,
stock int,
created_at timestamp,
Primary Key (product_id)
);	

Insert Data
Insert the following records:
product_id name category price stock
101 Laptop Electronics 65000 10
102 Mobile Electronics 20000 20
103 Headphones Electronics 2000 50
104 O]ice Chair Furniture 8000 15
105 Study Table Furniture 12000 5

=>

INSERT INTO product (product_id, name, category, price, stock, created_at)
VALUES (101, 'Laptop', 'Electronics', 65000, 10, toTimestamp(now()));

INSERT INTO product (product_id, name, category, price, stock, created_at)
VALUES (102, 'Mobile', 'Electronics', 20000, 20, toTimestamp(now()));

INSERT INTO product (product_id, name, category, price, stock, created_at)
VALUES (103, 'Headphones', 'Electronics', 2000, 50, toTimestamp(now()));

INSERT INTO product (product_id, name, category, price, stock, created_at)
VALUES (104, 'Office Chair', 'Furniture', 8000, 15, toTimestamp(now()));

INSERT INTO product (product_id, name, category, price, stock, created_at)
VALUES (105, 'Study Table', 'Furniture', 12000, 5, toTimestamp(now()));
Questions

1. Display all products.
=>
select * from product;

2. Display only product name and price.
=>
select name, price from product ;

3. Display the product whose product_id = 103.
=>
select * from product where product_id=103;

4. Display all products belonging to category Electronics.
=>
select * from product where category = 'Electronics' allow filtering;

5. Update the price of product_id = 102 to 18000.
=>
update product 
set price = 18000
where product_id = 102;

6. Update the stock of product_id = 105 to 8.
=>
update product
set stock = 8
where product_id = 105;

7. Change the category of product_id = 104 from Furniture to OMice Furniture.
=>
update product
set category = 'OMice Furniture'
where product_id = 104;

8. Delete the entire row where product_id = 103.
=>
delete from product 
where product_id = 103;




9. Delete only the column value stock from the row where product_id = 101.
=>

delete stock from product
where product_id=101;

10. Delete the row where product name is Headphones
=>
delete from product 
where product_id = 103; 

Create Customer Table
Create a table customer.
Column Name Type Collection Type
customer_id int Primary Key
name text Normal column
phones set<text> SET
orders list<int> LIST
address map<text,text> MAP

=>
create table customer(
	customer_id int Primary Key,
name text,
phones set<text>,
orders list<int>,
address map<text,text>
);

Insert Data
Insert the following data:

• customer_id: 1

§ name: Priya
§ phones: {9876543210, 9123456780}
§ orders: [101,102,103]
§ address: {city:Pune, state:Maharashtra}

• customer_id: 2

§ name: Amit
§ phones: {9998887776}
§ orders: [104,105]
§ address: {city:Mumbai, state:Maharashtra}

• customer_id: 3

§ name: Neha
§ phones: {8887776665,7776665554}
§ orders: [101]
§ address: {city:Delhi, state:Delhi}

=>


insert into customer(customer_id, name, phones, orders, address ) 
values(1,'Priya',{'9876543210', '9123456780'},[101,102,103],{ 'city' : 'Pune', 'state' : 'Maharashtra'});

insert into customer(customer_id, name, phones, orders, address) 
values(2,'Amit',{'9998887776'},[104,105],{ 'city' : 'Mumbai', 'state': 'Maharashtra'});

insert into customer(customer_id, name, phones, orders, address) 
values(3,'Neha',{'8887776665','7776665554'},[101],{ 'city': 'Delhi', 'state' : 'Delhi'});


11. Display all customers.
=>
select * from customer;

12. Display only phone numbers of customer_id = 1.
=>
select phones from customer where customer_id = 1;

13. Display orders list of customer_id = 2.
=>
select orders from customer where customer_id = 2;

14. Display city from address map for customer_id = 1.
=>
select address['city'] from customer where customer_id = 1;

15. Add a new phone number 9999999999 to the phones SET for customer_id = 1.
=>
update customer
set phones = phones + {'9999999999'} 
where customer_id = 1;

16. Add a new order 106 to the orders LIST for customer_id = 2.
=>
update customer
set orders = orders + [106]
where customer_id = 2;

17. Update the city in address MAP for customer_id = 3 to Bangalore.
=>
update customer
set address['city'] = 'Bangalore' 
where customer_id = 3;

18. Remove phone number 9123456780 from phones SET of customer_id = 1.
=>
update customer
set phones = phones - { '9123456780'}
where customer_id = 1;

19. Remove order 103 from orders LIST of customer_id = 1.
=>
update customer
set orders = orders - [103]
where customer_id = 1;

20. Remove state key from address MAP of customer_id = 2.
=>
update customer
set address = address - {'state'}
where customer_id = 2;

Create a User Defined Type called payment_info
Field Type
method text
status text

=>
CREATE TYPE payment_info (
    method text,
    status text
);




Create table orders
Column Type
order_id int (Primary Key)
product_name text
amount double
payment_details frozen<payment_info>
=>
CREATE TABLE orders (
    order_id int,
    product_name text,
    amount double,
    payment_details frozen<payment_info>,
    PRIMARY KEY (order_id)
);

Insert Data
Insert the following records:

order_id product_name amount payment
1 Laptop 65000

method:Credit Card,
status:Paid

2 Mobile 20000 method:UPI, status:Paid
3 Chair 8000

method:Cash,
status:Pending

=>
insert into orders (order_id, product_name, amount, payment_details)
values(
    1, 
    'Laptop', 
    65000.0, 
    {method: 'Credit Card', status: 'Paid'}
);






insert into orders (order_id, product_name, amount, payment_details)
values(
    2, 
    'Mobile', 
    20000.0, 
    {method: 'UPI', status: 'Paid'}
);

insert into orders (order_id, product_name, amount, payment_details)
VALUES (
    3, 
    'Chair', 
    8000.0, 
    {method: 'Cash', status: 'Pending'}
);


21. Display all orders.
=>
select * from orders;

22. Display payment method of order_id = 1.
=>
select payment_details.method from orders where order_id = 1;

23. Update payment status of order_id = 3 to Paid.
=>
update orders
set payment_details = {method: 'Cash', status: 'Paid'}
where order_id = 3;

24. Delete the order where order_id = 2.
=>
delete from orders 
where order_id=3;

# Mongo Assignment

Assignment 11


1. Write a MongoDB query to display all the documents in the collection restaurants
=>
db.restaurent.find()

2. Write a MongoDB query to display the fields restaurant_id, name, borough and cuisine for
all the documents in the collection restaurant.
=>
db.restaurent.find ({} , { "restuarant_id" :1, "name":1, "borough":1 ,"cuisine":1 } ) 

3. Write a MongoDB query to display the fields restaurant_id, name, borough and cuisine,
but exclude the field _id for all the documents in the collection restaurant.
=>
db.restaurent.find( {} , { "restuarant_id" :1, "name":1, "borough":1 ,"cuisine":1, "_id" :0 } )

4. Write a MongoDB query to display the fields restaurant_id, name, borough and zip code,
but exclude the field _id for all the documents in the collection restaurant.
=>
db.restaurent.find( {} , { "restaurant_id" :1, "name":1, "borough":1 ,"address.zipcode":1, "_id" :0 })

5. Write a MongoDB query to display all the restaurant which is in the borough Bronx
=>
db.restaurent.find({"borough": "Bronx"})

6. Write a MongoDB query to display the first 5 restaurant which is in the borough Bronx.
=>
db.restaurent.find({"borough":"Bronx"}).limit(5)

7.Write a MongoDB query to display the next 5 restaurants after skipping first 5 which are in
the borough Bronx.
=>
db.restaurent.find({"borough":"Bronx"}).skip(5).limit(5)

8. Write a MongoDB query to find the restaurants who achieved a score more than 90.
=>
db.restaurent.find({"grades.score" : {$gt: 90}})

9. Write a MongoDB query to find the restaurants that achieved a score, more than 80 but
less than 100.
=>
db.restaurent.find({ "grades.score" : {$gt: 80, $lt: 100} })

10. Write a MongoDB query to find the restaurants which locate in latitude value less than -
95.754168.
=>
db.restaurent.find( { "address.coord.0": {$lt: -95.754168}})

11. Write a MongoDB query to find the restaurants that do not prepare any cuisine of
'American' and their grade score more than 70 and latitude less than -65.754168.
=> 
db.restaurent.find({cuisine: {$ne: "American"}, "grades.score" : {$gt: 70}, "address.coord.1": {$lt: -65.754168}})


12. Write a MongoDB query to find the restaurants which do not prepare any cuisine of
'American' and achieved a score more than 70 and located in the longitude less than -
65.754168.
=>
db.restaurent.find({ "cuisine": {$ne: "American"}  , "grades.score": {$gt: 70} , "address.coord.0": {$lt: 65.754168} })

13. Write a MongoDB query to find the restaurants which do not prepare any cuisine of
'American ' and achieved a grade point 'A' not belongs to the borough Brooklyn. The
document must be displayed according to the cuisine in descending order.
=> 
db.restaurent.find({"cuisine": {$ne: "American"}, "grades.grade": {$eq: "A"} , "borough": {$ne: "Brooklyn"}}).sort({"cuisine": -1} )

14. Write a MongoDB query to find the restaurant Id, name, borough and cuisine for those
restaurants which contain 'Wil' as first three letters for its name.
=> 
db.restaurent.find({ "name": {$regex: '^Wil'} } , {"restaurant_id": 1 , "name": 1, "borough": 1, "cuisine": 1 } )

15. Write a MongoDB query to find the restaurant Id, name, borough and cuisine for those
restaurants which contain 'ces' as last three letters for its name.
=>
db.restaurent.find( {"name": {$regex: 'ces$'} }, {"restaurant_id": 1 , "name": 1, "borough": 1, "cuisine": 1 } )

16. Write a MongoDB query to find the restaurant Id, name, borough and cuisine for those
restaurants which contain 'Reg' as three letters somewhere in its name.
=> 
db.restaurent.find({"name": {$regex: 'Reg'} }, {"restaurent_id": 1, "name": 1, "borough": 1, "cuisine": 1 } )


17. Write a MongoDB query to find the restaurants which belong to the borough Bronx and
prepared either American or Chinese dish.
=> 
db.restaurent.find({ "borough": {$eq: "Bronx"} , "cuisine": {$in: ["Americam","Chinese"]}
 })

18. Write a MongoDB query to find the restaurant Id, name, borough and cuisine for those
restaurants which belong to the borough Staten Island or Queens or Bronxor Brooklyn.
=>
db.restaurent.find({"borough": {$in: ["Staten Island", "Queens", "Bronx", "Brooklyn"]}}, {"restaurent_id": 1, "name": 1, "borough": 1, "cuisine": 1} )

19. Write a MongoDB query to find the restaurant Id, name, borough and cuisine for those
restaurants which are not belonging to the borough Staten Island or Queens or Bronx or
Brooklyn.
=> 
db.restaurent.find({"borough": {$nin: ["Staten Island" , "Queens", "Bronx" , "Brooklyn"] }} , {"restaurant_id": 1, "name": 1, "borough": 1, "cuisine": 1})

20. Write a MongoDB query to find the restaurant Id, name, borough and cuisine for those
restaurants which achieved a score which is not more than 10.
=>
db.restaurent.find( {"grades.score": {$lte: 10}} , {"restaurant_id": 1, "name": 1, "borough": 1, "cuisine": 1} )

21. Write a MongoDB query to find the restaurant Id, name, borough and cuisine for those
restaurants which prepared dish except 'American' and 'Chinees' or restaurant's name begins
with letter 'Wil'.
=> 
db.restaurent.find({$or: [{"cuisine": {$nin: ["American" , "Chinese"]}} , {"name": {$regex: '^Wil'}}]} ,  {"restaurant_id": 1, "name": 1, "borough": 1, "cuisine": 1})

22. Write a MongoDB query to find the restaurant Id, name, and grades for those restaurants
which achieved a grade of "A" and scored 11 on an ISODate "2014-08-11T00:00:00Z"
among many of survey dates
=> 

db.restaurent.find()
db.restaurant.find(
  {
    grades: {
      $elemMatch: {
        grade: "A",
        score: 11,
        date: ISODate("2014-08-11T00:00:00Z")
      }
    }
  },
  { restaurant_id: 1, name: 1, grades: 1, _id: 0 }
)


23. Write a MongoDB query to find the restaurant Id, name and grades for those restaurants
where the 2nd element of grades array contains a grade of "A" and score 9 on an ISODate
"2014-08-11T00:00:00Z".
=> 
db.restaurant.find(
  {
    "grades.1.grade": "A",
    "grades.1.score": 9,
    "grades.1.date": ISODate("2014-08-11T00:00:00Z")
  },
  { restaurant_id: 1, name: 1, grades: 1, _id: 0 }
)


24. Write a MongoDB query to find the restaurant Id, name, address and geographical
location for those restaurants where 2nd element of coord array contains a value which is
more than 42 and upto 52
=>  
db.restaurant.find(  {"address.coord.1": { $gt: 42 , $lte: 52}} ,  { restaurant_id: 1, name: 1, address: 1, "address.coord": 1,  _id: 0 } )

25. Write a MongoDB query to arrange the name of the restaurants in ascending order along
with all the columns.
=> 
db.restaurant.find().sort({name : 1})

26. Write a MongoDB query to arrange the name of the restaurants in descending along with
all the columns.
=>
db.restaurant.find().sort({name: -1})
27. Write a MongoDB query to arranged the name of the cuisine in ascending order and for
that same cuisine borough should be in descending order.
=>
db.restaurant.find().sort({cuisine: 1 , borough: -1})

28. Write a MongoDB query to know whether all the addresses contains the street or not.
=>
db.restaurant.countDocuments({"address.street": {$exists: false}})

29. Write a MongoDB query which will select all documents in the restaurants collection
where the coord field value is Double.
=>
db.restaurant.find({"address.coord": {$type: "double"}})

30. Write a MongoDB query which will select the restaurant Id, name and grades for those
restaurants which returns 0 as a remainder after dividing the score by 7.
=>
db.restaurant.find( {"grades.score": {$mod: [7, 0]}} , {restaurant_id: 1, name: 1, grades: 1, _id: 0})

31. Write a MongoDB query to find the restaurant name, borough, longitude and attitude and
cuisine for those restaurants which contains 'mon' as three letters somewhere in its name.
=>
db.restaurant.find({name: /mon/i  } , {name: 1, borough: 1, "address.coord": 1, cuisine: 1, _id : 0})

32. Write a MongoDB query to find the restaurant name, borough, longitude and latitude and
cuisine for those restaurants which contain 'Mad' as first three letters of its name.
=>
db.restaurant.find({name: /^Mad/  } , {name: 1, borough: 1, "address.coord": 1, cuisine: 1, _id : 0})

# Mongo Update assignment 

Create a Employee Collection add 5 documents:
Example:
{empno:111,
ename:"Deepali Vaidya",
sal:40000.00,
dept:{deptno:12,dname:,"Hr",dloc:"Mumbai"},
desg:"Analyst",
mgr:{name:"Satish",num:222},
project:[{name:"project-1",Hrs:4},
{name:"project-2",Hrs:4}],
skillset:['perl','python','java'],
hobbies:['reading','biking']}

db.employee.insertMany([
  {
    empno: 111,
    ename: "Deepali Vaidya",
    sal: 40000.00,
    dept: { deptno: 12, dname: "Hr", dloc: "Mumbai" },
    desg: "Analyst",
    mgr: { name: "Satish", num: 222 },
    project: [
      { name: "project-1", Hrs: 4 },
      { name: "project-2", Hrs: 4 }
    ],
    skillset: ["perl", "python", "java"],
    hobbies: ["reading", "biking"]
  },
  {
    empno: 222,
    ename: "Rajesh",
    sal: 55000.00,
    dept: { deptno: 14, dname: "Purchase", dloc: "Pune" },
    desg: "CLERK",
    mgr: { name: "Rajan", num: 333 },
    project: [
      { name: "project-1", Hrs: 3 },
      { name: "project-3", Hrs: 4 }
    ],
    skillset: ["python", "c++"],
    hobbies: ["chess", "swimming"]
  },
  {
    empno: 333,
    ename: "Pankaj",
    sal: 25000.00,
    dept: { deptno: 15, dname: "Sales", dloc: "Mumbai" },
    desg: "CLERK",
    mgr: { name: "Revati", num: 444 },
    project: [
      { name: "project-2", Hrs: 5 }
    ],
    skillset: ["java"],
    hobbies: ["singing", "gaming"]
  },
  {
    empno: 444,
    ename: "Deepak",
    sal: 33000.00,
    dept: { deptno: 12, dname: "Hr", dloc: "Mumbai" },
    desg: "Analyst",
    mgr: { name: "Satish", num: 222 },
    project: [
      { name: "project-4", Hrs: 3 }
    ],
    skillset: ["perl", "sql"],
    hobbies: ["reading", "biking"]
  },
  {
    empno: 555,
    ename: "Rutuja",
    sal: 8000.00,
    dept: { deptno: 16, dname: "IT", dloc: "Pune" },
    desg: "Developer",
    mgr: { name: "Suresh", num: 555 },
    project: [
      { name: "project-1", Hrs: 2 },
      { name: "project-2", Hrs: 4 }
    ],
    skillset: ["python"],
    hobbies: ["reading", "travelling"]
  }
]);





1. All Employee’s with the desg as ‘CLERK’ are now called as (AO) Administrative Officers.
Update the Employee collection for this.
=>
db.employee.updateMany(
{desg: "CLERK"},
{$set: {desg: "Administrative Officers" } }
); 

2. Change the number of hours for project-1 to 5 for all employees with designation analyst.
=>
db.employee.updateMany( 
{desg: "Analyst" , "project.name": "project-1"} , 
{$set: {"project.$.Hrs": 5}}
);

3. Add 2 projects project-3 and project-4 for employee whose name starts with ”Deep” with 2 hrs
=>
db.employee.updateMany(
{ename: /^Deep/ } ,
{
 $push: { 
project : { 
$each: [
{name: "project-3", Hrs: 2 } ,
{name: "project-4", Hrs: 2}
]
}
}
          }
);


4. Add bonus rs 2000 for all employees with salary > 50000
=>
db.employee.update(
	{sal: {$gt: 50000} } ,
	{$set: {bonus: 2000} }
);

5. Add bonus rs 1500 if salary <50000 and > 30000
=>
db.employee.updateMany(
	{sal : {$gt: 30000 , $lt: 50000}} ,
	{$set: {bonus: 1500}} );
6. increment bounus by 1000 for all employees if salary <=30000
=>
db.employee.updateMany(
{ sal: {$lte: 30000} },
{ $inc : {bonus: 1000}}
);

7. Change manager name to Tushar for all employees whose manager is currently “satish”
And manager number to 3333
=>
db.employee.updateMany(
{ "mgr.name": /^satish$/i } ,
{ $set: { "mgr.name": "Tushar" , "mgr.num": 3333}}
);

8. Increase salary of all employees from “purchase department” by 15000
=>
db.employee.updateMany(
{"dept.dname": /^purchase$/i },
{$inc: {sal: 15000}}
);

9. Decrease number of hrs by 2 for all employees who are working on project-2
=>
db.employee.updateMany(
	{ "project.name" : "project-2" } , 
{$inc : {"project.$.Hrs" : -2}}
);

10. Delete project-2 from all employee document if they are working on the project for 4
hrs.
=>
db.employee.deleteMany(
	{"project.name": "project-2" } , 
	{ $pull : { project: { name: "projedct-2", Hrs: 4 } }}
);

11. Change the salary of employees to 10000 only if their salary is < 10000
=>
db.employee.updateMany(
	{ sal : {$lt: 10000} }, 
{ $set: {"sal": 10000}}
);

12. Increase bonus of all employees by 500 if the bonus is <2000 or their salary is <
20000 or if employee belong to sales department
=>
db.employee.updateMany(
{ $or: [
{ bonus: {$lt: 2000} }, 
{"sal": {$lt: 20000}} ,
{ "dept.dname": /^sales$/i }
]
}, 
{ $inc: {"bonus": 500}}
);

13. Add 2 new project at position 2 for all employees with designation analyst or salary is
equal to either 30000 or 33000 or 35000
=>
db.employee.updateMany(
{$or: [{desg: "Analyst" } , {"sal": {$in: [30000, 33000, 35000 ] }} ] },
{$push: {"project": {
$each: [
 {name: "project-new1" , Hrs: 3},
  {name: "project-new1" , Hrs: 3}
],
$position: 1
	}
}
	}
);

14. Delete last project of all employees with department name is “HR” and if the location
is Mumbai
=>
db.employee.deleteMany(
{ "dept.dname": "HR" , "dept.dloc": 'Mumbai'} ,
{ $pop: { project: 1 }}
);

15. Change designation of all employees to senior programmer if they are working on
name:”Project-1” for 4 hrs
=>
db.employee.updateMany(
	{  "project": { $elemMatch: { name: /^Project-1$/i , Hrs : 4} } } , 
{ $set: {"desg": "senior programmer" } } );

16. Add list of hobbies in all employees document whose manager is Rajan or Revati
=>
db.employee.updateMany( 
{"mgr": {$in: ["Rajan" , "Revati"] } } ,
{$set: { hobbies: [] }}
);

17. Add list of skillset in all employee documents who are working on project-4 for 3 hrs
or on project-3 for 4 hrs
=>
db.employee.updateMany( 
		{$or: [
			{project: {$elemMatch: {"name" : "project-4", Hrs: 3 }} }, 
			{project: {$elemMatch: {"name" : "project-3", Hrs: 4 }}} 
			  ] 
			},
			{ $addToSet: { skillset: { $each: ["Docker", "Kubernetes"]}}}
		);

18. Add a new hobby as blogging at 3 position in hobbies array for all employess whose
name starts with R or p and ends with j or s
=>
db.employee.updateMany( 
{ename: /^[Rp].*[js]$/i } , 
{$push: { "hobbies": { $each: [ "blogging" ] , $position: 2 } } } 
);

19. Increase salary by 10000 for all employees who are working on project-2 or project-3
or project-1
=>
db.employee.updateMany( 
			{"project.name": {$in: ["project-4","project-3","project-1"] } },  
			{ $inc: { "sal": 10000}}
		);





Decrease bonus by 1000 rs And increase salary by 1000rs for all employees whose
department location is Mumbai
=>
db.employee.updateMany( 
			{"dept.dloc": /^mumbai$/i },  
			{ $inc: { "bonus": -1000, "sal": 1000}}
		);


20. Remove all employees working on project-1
=>
db.employee.replaceOne(
{ "project.name": "project-1" } );


21. Replace document of employee with name “Deepak to some new document
=>
db.employee.replaceOne(
{ "ename": "Deepak" },
{
empno: 444,
ename: "Deepak Sharma",
sal: 45000.00,
dept: { deptno: 12, dname: "Hr", dloc: "Mumbai" },
Desg: "Senior Analyst",
mgr: { name: "Tushar", num: 3333 },
project: [{ name: "project-5", Hrs: 6 }],
skillset: ["python", "aws"],
hobbies: ["photography"]
}
);

22. Change skill python to python 3.8 for all employees if python is there in the skillset
=>
db.employee.updateMany(
{ skillset: "python" } , 
{ $set: { "skillset.$": "python 3.8" } } );






23. Add 2 skills MongoDb and Perl at the end of skillset array for all employees who are
working at Pune location
=>
db.employee.updateMany(
{"dept.dloc": "Pune" } , 
{ $push: { skillset: { $each: ["MongoDb", "Perl" ] } } } );

24. Delete first hobby from hobby array for all employees who are working on project-1
or project-2
=>
db.employee.updateMany(
{ "project.name": { $in: [ "project-1", "project-2" ] } } , 
{$pop: { hobbies, -1 } } );


25. Delete last hobby from hobbies array for all employees who are working on project
which is at 2 nd position in projects array for 4 hrs
=>
db.employee.updateMany(
{ "project.1.Hrs": 4 } , 
{ $pop: { hobbies: 1 } } );

26. Add 2 new projects at the end of array for all employees whose skillset contains Perl
or python
=>
db.employee.updateMany(
{ skillset: {$in: ["Perl","Python"] } } , 
{ $push: { project: { $each: [ { name: "python-X", Hrs: 3} , { name: "project-Y", Hrs: 4 } ] } } } );

27. Change hrs to 6 for project-1 for all employees if they working on the project-1 for <
6 hrs. otherwise keep the existing value.
=>
db.employee.updateMany(
{ project: { $elemMatch: { name: "project-1" , Hrs: {$lt: 6 } } } } , 
{ $set: { "project.$.Hrs": 6 } }  );


# Mongo BOOK Assignment 

Create a Employee Collection add 5 documents:
Example:
{empno:111,
ename:"Deepali Vaidya",
sal:40000.00,
dept:{deptno:12,dname:,"Hr",dloc:"Mumbai"},
desg:"Analyst",
mgr:{name:"Satish",num:222},
project:[{name:"project-1",Hrs:4},
{name:"project-2",Hrs:4}],
skillset:['perl','python','java'],
hobbies:['reading','biking']}

db.employee.insertMany([
  {
    empno: 111,
    ename: "Deepali Vaidya",
    sal: 40000.00,
    dept: { deptno: 12, dname: "Hr", dloc: "Mumbai" },
    desg: "Analyst",
    mgr: { name: "Satish", num: 222 },
    project: [
      { name: "project-1", Hrs: 4 },
      { name: "project-2", Hrs: 4 }
    ],
    skillset: ["perl", "python", "java"],
    hobbies: ["reading", "biking"]
  },
  {
    empno: 222,
    ename: "Rajesh",
    sal: 55000.00,
    dept: { deptno: 14, dname: "Purchase", dloc: "Pune" },
    desg: "CLERK",
    mgr: { name: "Rajan", num: 333 },
    project: [
      { name: "project-1", Hrs: 3 },
      { name: "project-3", Hrs: 4 }
    ],
    skillset: ["python", "c++"],
    hobbies: ["chess", "swimming"]
  },
  {
    empno: 333,
    ename: "Pankaj",
    sal: 25000.00,
    dept: { deptno: 15, dname: "Sales", dloc: "Mumbai" },
    desg: "CLERK",
    mgr: { name: "Revati", num: 444 },
    project: [
      { name: "project-2", Hrs: 5 }
    ],
    skillset: ["java"],
    hobbies: ["singing", "gaming"]
  },
  {
    empno: 444,
    ename: "Deepak",
    sal: 33000.00,
    dept: { deptno: 12, dname: "Hr", dloc: "Mumbai" },
    desg: "Analyst",
    mgr: { name: "Satish", num: 222 },
    project: [
      { name: "project-4", Hrs: 3 }
    ],
    skillset: ["perl", "sql"],
    hobbies: ["reading", "biking"]
  },
  {
    empno: 555,
    ename: "Rutuja",
    sal: 8000.00,
    dept: { deptno: 16, dname: "IT", dloc: "Pune" },
    desg: "Developer",
    mgr: { name: "Suresh", num: 555 },
    project: [
      { name: "project-1", Hrs: 2 },
      { name: "project-2", Hrs: 4 }
    ],
    skillset: ["python"],
    hobbies: ["reading", "travelling"]
  }
]);





1. All Employee’s with the desg as ‘CLERK’ are now called as (AO) Administrative Officers.
Update the Employee collection for this.
=>
db.employee.updateMany(
{desg: "CLERK"},
{$set: {desg: "Administrative Officers" } }
); 

2. Change the number of hours for project-1 to 5 for all employees with designation analyst.
=>
db.employee.updateMany( 
{desg: "Analyst" , "project.name": "project-1"} , 
{$set: {"project.$.Hrs": 5}}
);

3. Add 2 projects project-3 and project-4 for employee whose name starts with ”Deep” with 2 hrs
=>
db.employee.updateMany(
{ename: /^Deep/ } ,
{
 $push: { 
project : { 
$each: [
{name: "project-3", Hrs: 2 } ,
{name: "project-4", Hrs: 2}
]
}
}
          }
);


4. Add bonus rs 2000 for all employees with salary > 50000
=>
db.employee.update(
	{sal: {$gt: 50000} } ,
	{$set: {bonus: 2000} }
);

5. Add bonus rs 1500 if salary <50000 and > 30000
=>
db.employee.updateMany(
	{sal : {$gt: 30000 , $lt: 50000}} ,
	{$set: {bonus: 1500}} );
6. increment bounus by 1000 for all employees if salary <=30000
=>
db.employee.updateMany(
{ sal: {$lte: 30000} },
{ $inc : {bonus: 1000}}
);

7. Change manager name to Tushar for all employees whose manager is currently “satish”
And manager number to 3333
=>
db.employee.updateMany(
{ "mgr.name": /^satish$/i } ,
{ $set: { "mgr.name": "Tushar" , "mgr.num": 3333}}
);

8. Increase salary of all employees from “purchase department” by 15000
=>
db.employee.updateMany(
{"dept.dname": /^purchase$/i },
{$inc: {sal: 15000}}
);

9. Decrease number of hrs by 2 for all employees who are working on project-2
=>
db.employee.updateMany(
	{ "project.name" : "project-2" } , 
{$inc : {"project.$.Hrs" : -2}}
);

10. Delete project-2 from all employee document if they are working on the project for 4
hrs.
=>
db.employee.deleteMany(
	{"project.name": "project-2" } , 
	{ $pull : { project: { name: "projedct-2", Hrs: 4 } }}
);

11. Change the salary of employees to 10000 only if their salary is < 10000
=>
db.employee.updateMany(
	{ sal : {$lt: 10000} }, 
{ $set: {"sal": 10000}}
);

12. Increase bonus of all employees by 500 if the bonus is <2000 or their salary is <
20000 or if employee belong to sales department
=>
db.employee.updateMany(
{ $or: [
{ bonus: {$lt: 2000} }, 
{"sal": {$lt: 20000}} ,
{ "dept.dname": /^sales$/i }
]
}, 
{ $inc: {"bonus": 500}}
);

13. Add 2 new project at position 2 for all employees with designation analyst or salary is
equal to either 30000 or 33000 or 35000
=>
db.employee.updateMany(
{$or: [{desg: "Analyst" } , {"sal": {$in: [30000, 33000, 35000 ] }} ] },
{$push: {"project": {
$each: [
 {name: "project-new1" , Hrs: 3},
  {name: "project-new1" , Hrs: 3}
],
$position: 1
	}
}
	}
);

14. Delete last project of all employees with department name is “HR” and if the location
is Mumbai
=>
db.employee.deleteMany(
{ "dept.dname": "HR" , "dept.dloc": 'Mumbai'} ,
{ $pop: { project: 1 }}
);

15. Change designation of all employees to senior programmer if they are working on
name:”Project-1” for 4 hrs
=>
db.employee.updateMany(
	{  "project": { $elemMatch: { name: /^Project-1$/i , Hrs : 4} } } , 
{ $set: {"desg": "senior programmer" } } );

16. Add list of hobbies in all employees document whose manager is Rajan or Revati
=>
db.employee.updateMany( 
{"mgr": {$in: ["Rajan" , "Revati"] } } ,
{$set: { hobbies: [] }}
);

17. Add list of skillset in all employee documents who are working on project-4 for 3 hrs
or on project-3 for 4 hrs
=>
db.employee.updateMany( 
		{$or: [
			{project: {$elemMatch: {"name" : "project-4", Hrs: 3 }} }, 
			{project: {$elemMatch: {"name" : "project-3", Hrs: 4 }}} 
			  ] 
			},
			{ $addToSet: { skillset: { $each: ["Docker", "Kubernetes"]}}}
		);

18. Add a new hobby as blogging at 3 position in hobbies array for all employess whose
name starts with R or p and ends with j or s
=>
db.employee.updateMany( 
{ename: /^[Rp].*[js]$/i } , 
{$push: { "hobbies": { $each: [ "blogging" ] , $position: 2 } } } 
);

19. Increase salary by 10000 for all employees who are working on project-2 or project-3
or project-1
=>
db.employee.updateMany( 
			{"project.name": {$in: ["project-4","project-3","project-1"] } },  
			{ $inc: { "sal": 10000}}
		);





Decrease bonus by 1000 rs And increase salary by 1000rs for all employees whose
department location is Mumbai
=>
db.employee.updateMany( 
			{"dept.dloc": /^mumbai$/i },  
			{ $inc: { "bonus": -1000, "sal": 1000}}
		);


20. Remove all employees working on project-1
=>
db.employee.replaceOne(
{ "project.name": "project-1" } );


21. Replace document of employee with name “Deepak to some new document
=>
db.employee.replaceOne(
{ "ename": "Deepak" },
{
empno: 444,
ename: "Deepak Sharma",
sal: 45000.00,
dept: { deptno: 12, dname: "Hr", dloc: "Mumbai" },
Desg: "Senior Analyst",
mgr: { name: "Tushar", num: 3333 },
project: [{ name: "project-5", Hrs: 6 }],
skillset: ["python", "aws"],
hobbies: ["photography"]
}
);

22. Change skill python to python 3.8 for all employees if python is there in the skillset
=>
db.employee.updateMany(
{ skillset: "python" } , 
{ $set: { "skillset.$": "python 3.8" } } );






23. Add 2 skills MongoDb and Perl at the end of skillset array for all employees who are
working at Pune location
=>
db.employee.updateMany(
{"dept.dloc": "Pune" } , 
{ $push: { skillset: { $each: ["MongoDb", "Perl" ] } } } );

24. Delete first hobby from hobby array for all employees who are working on project-1
or project-2
=>
db.employee.updateMany(
{ "project.name": { $in: [ "project-1", "project-2" ] } } , 
{$pop: { hobbies, -1 } } );


25. Delete last hobby from hobbies array for all employees who are working on project
which is at 2 nd position in projects array for 4 hrs
=>
db.employee.updateMany(
{ "project.1.Hrs": 4 } , 
{ $pop: { hobbies: 1 } } );

26. Add 2 new projects at the end of array for all employees whose skillset contains Perl
or python
=>
db.employee.updateMany(
{ skillset: {$in: ["Perl","Python"] } } , 
{ $push: { project: { $each: [ { name: "python-X", Hrs: 3} , { name: "project-Y", Hrs: 4 } ] } } } );

27. Change hrs to 6 for project-1 for all employees if they working on the project-1 for <
6 hrs. otherwise keep the existing value.
=>
db.employee.updateMany(
{ project: { $elemMatch: { name: "project-1" , Hrs: {$lt: 6 } } } } , 
{ $set: { "project.$.Hrs": 6 } }  );
