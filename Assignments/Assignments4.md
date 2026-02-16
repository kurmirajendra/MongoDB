# 📘 MongoDB Array & Query Practice

This README contains all MongoDB commands used for:

- Insert operations
- Array queries
- $in, $all, $elemMatch
- $size
- Update operations
- Complex filtering

------------------------------------------------------------

# 1️⃣ Create Course Collection

## Insert Documents

db.Course.insertMany([
{
  courseName: "FrontEnd Mastery",
  modules: ["HTML","CSS","Java Script"],
  instructors: ["Sir","MaM"]
},


{
  courseName: "Web Development",
  modules: ["CSS","HTML","Java Script"],
  instructors: ["Mam","Sir"]
},

{
  courseName: "Full Stack Development",
  modules: ["Java Script","HTML","CSS"],
  instructors: ["Sir"]
},

{
  courseName: "Full Stack Development",
  modules: ["Java","CSS","HTML","NodeJS"],
  instructors: ["SIR","MAM"]
},

{
  courseName: "BackEnd Development",
  modules: ["MongoDB","NodeJS"],
  instructors: ["Sir"]
}

])

------------------------------------------------------------

# 2️⃣ Find Courses Whose Modules EXACTLY Match

db.Course.find({
  modules: ["HTML","CSS","Java Script"]
})

------------------------------------------------------------

# 3️⃣ Find Courses Containing "Java Script"

db.Course.find({
  modules: { $in: ["Java Script"] }
})

------------------------------------------------------------

# 4️⃣ Create Developers Collection

db.developers.insertMany([
{
  name: "Krishna",
  skills: ["MongoDB","React","NodeJs","Spring Boot"],
  certifications: ["Orcale"]
},

{
  name: "Veer",
  skills: ["Spring Boot","Python","MongoDB","React","NodeJs","Orcale"],
  certifications: ["Orcale","Azure"]
},


{
  name: "Rajendra",
  skills: ["Orcale","MongoDB","React","NodeJs","Sprint Boot"],
  certifications: ["AWS"]
},


{
  name: "Vinod",
  skills: ["Spring Boot","Orcale","Java"],
  certifications: ["Orcale"]
},


{
  name: "Muskan",
  skills: ["MongoDB","NodeJs","Spring Boot"],
  certifications: ["Azure"]
},


{
  name: "Vaishnavi",
  skills: ["MongoDB","NodeJs","HTML"],
  certifications: ["AWS"]
}


])

------------------------------------------------------------

# 5️⃣ Developers Who Have MongoDB AND NodeJs

db.developers.find({
  skills: { $all: ["MongoDB","NodeJs"] }
})

------------------------------------------------------------

# 6️⃣ Developers With React, NodeJs, MongoDB

db.developers.find({
  skills: { $all: ["React","NodeJs","MongoDB"] }
})

------------------------------------------------------------

# 7️⃣ Create Students Collection

db.createCollection("students")

db.students.insertMany([
{
  name: "Rahul",

  subjects: [
    {name:"Math",marks:83,attempts:1},
    {name:"Science",marks:78,attempts:2},
    {name:"English",marks:65,attempts:1}
  ]

},


{
  name: "Vinod",

  subjects: [
    {name:"Math",marks:82,attempts:2},
    {name:"Science",marks:75,attempts:1},
    {name:"Hindi",marks:89,attempts:1}
  ]

},


{
  name: "Vaishnavi",

  subjects: [
    {name:"Math",marks:78,attempts:1}
  ]

},
{
  name: "Abhay",

  subjects: [
    {name:"Math",marks:85,attempts:1},
    {name:"Science",marks:73,attempts:2},
    {name:"English",marks:68,attempts:1}
  ]

},
{
  name: "Umesh",

  subjects: [
    {name:"Math",marks:75,attempts:2},
    {name:"Science",marks:65,attempts:1},
    {name:"Hindi",marks:89,attempts:1}
  ]

},
{
  name: "Dev",

  subjects: [
    {name:"Math",marks:80,attempts:1},
    {name:"Science",marks:85,attempts:2},
    {name:"English",marks:86,attempts:1}
  ]

}
])

------------------------------------------------------------

# 8️⃣ Students Who Scored More Than 80 in Math (Correct Way)

db.students.find({
  subjects: {
    $elemMatch: {
      name: "Math",
      marks: { $gt: 80 }
    }
  }
})

------------------------------------------------------------

# 9️⃣ Students Where marks ≥ 70 AND attempts = 1 (Same Object)

db.students.find({
  subjects: {
    $elemMatch: {
      marks: { $gte: 70 },
      attempts: 1
    }
  }
})

------------------------------------------------------------

# 🔟 Create Teams Collection

db.createCollection("teams")

db.teams.insertMany([
{
  teamName: "GodLike",

  members: ["Krishna","Rahul","Pankaj","Mohit Bhaiya","Anuraj","Rohan","Abhay"]

},

{
  teamName: "Glorix",

  members: ["Veer","Umesh","Gargi","Astha Didi","Muskan S","Muskan","Veerendra"]

},

{
  teamName: "Dominators",

  members: ["Me","Vaishnavi","Ashwin","Aadrash","Shivam Bhaiya","Yasika"]
},

{
  teamName: "RO-KO",

  members: ["Veer","Krishna","Rajendra"]

},

{
  teamName: "Matrix",

  members: ["Dev","Parth","Sakshi"]

}

])

------------------------------------------------------------

# 1️⃣1️⃣ Teams With Exactly 3 Members

db.teams.find({
  members: { $size: 3 }
})

------------------------------------------------------------

# 1️⃣2️⃣ Teams With Exactly 5 Members

db.teams.find({
  members: { $size: 5 }
})

------------------------------------------------------------

# 1️⃣3️⃣ Insert Team "Cheeta"

db.teams.insertOne({

  teamName: "Cheeta",

  members: ["Rahul","Amit","Sneha","Karan"]
  
})

------------------------------------------------------------

# 1️⃣4️⃣ Teams That Contain "Rahul" AND Have Exactly 4 Members

db.teams.find({
  $and: [
    { members: "Rahul" },
    { members: { $size: 4 } }
  ]
})

------------------------------------------------------------

# 🔑 MongoDB Operators Used

$in         → Match any value in array  
$all        → Must contain all specified values  
$elemMatch  → Multiple conditions on same array object  
$size       → Exact array length  
$set        → Update specific fields  

------------------------------------------------------------

# ✅ Conclusion

This file demonstrates:

- Array matching
- Exact array comparison
- Array of objects filtering
- Multi-condition filtering
- Size constraints
- Proper MongoDB syntax

------------------------------------------------------------
