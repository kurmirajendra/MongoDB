# MongoDB Employees Operations

## Commands and Queries

```js
db.createCollection("Employees");

db.Employees.insertOne({
       empId:101,
      name:"Krishna Kurmi",
      department:"HR",
      designation:"HR Executive",
      salary:40000,
      experience:3,
      city:"Indore"
});

db.Employees.insertOne({
    empId:102,
      name:"Abhay",
      department:"IT",
      designation:"Software Engineer",
      salary:50000,
      experience:4,
      city:"Pune"
});

db.Employees.insertOne({
   empId:103,
      name:"Muskan",
      department:"IT",
      designation:"Senior Developer",
      salary:55000,
      experience:3,
      city:"Bangalore"
});

db.Employees.insertOne({
   empId:104,
      name:"Veer",
      department:"IT",
      designation:"Senior Developer",
      salary:75000,
      experience:5,
      city:"Mumbai"
});

db.Employees.insertOne({
    empId:105,
      name:"Vaishnavi",
      department:"HR",
      designation:"Manager",
      salary:70000,
      experience:7,
      city:"Mumbai"
});

db.Employees.insertOne({
  empId:106,
      name:"Rajendra",
      department:"IT",
      desgination:"Junior Developer",
      salary:38000,
      experience:1,
      city:"Bangalore"
});

db.Employees.find();

db.Employees.find({department : "HR"});

db.Employees.find({salary : {$gt:50000}});

db.Employees.find({experience : {$lt:5}});

db.Employees.find({
  $and : [
    {salary : {$gt:40000} },
    {salary : {$lt:70000}}
  ]
});

db.Employees.find({
  $and : [
    { department : "IT"},
    { experience : {$gt:2}}
  ]
});

db.Employees.find({
  $or : [
    {department : "Finance"},
    {salary : {$gt:80000}}
  ]
});

db.Employees.find({
  $and : [
    {experience : {$gt:3}},
    {salary : {$lt:60000}}
  ]
});

db.Employees.find({
  $and : [
    {experience : {$gt:3}},
    {salary : {$lt:6000}}
  ]
});

db.Employees.find({
  $and : [
    {experience : {$gt:3}},
    {salary : {$gt:60000}}
  ]
});

db.Employees.find(
  {
    $and:[
      {
        city:"Mumbai"
      },
      {
        designation:"Manager"
      }
    ]
  }
);
