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
