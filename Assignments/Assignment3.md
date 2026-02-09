# 📘 MongoDB Regex Full Practice (With Insert + Query + Output)

Collection: db.xyz

------------------------------------------------------------

# 🔹 1️⃣ Insert Student Data

db.xyz.insertMany([
  { name: "Aman", rollNo: "CS2024" },
  { name: "Anuj", rollNo: "CS2024" },
  { name: "Raj", rollNo: "AL2023" }
])

Query:
db.xyz.find({ name: /^A/i })

Output:
{ name: "Aman", rollNo: "CS2024" }
{ name: "Anuj", rollNo: "CS2024" }

------------------------------------------------------------

# 🔹 2️⃣ Insert Course Data

db.xyz.insertMany([
  { name: "Umesh", courseName: "Big Data Analytics" },
  { name: "Veerendra", courseName: "Database Management" },
  { name: "Vinod", courseName: "Web Development" }
])

Query:
db.xyz.find({ courseName: /data/i })

Output:
{ name: "Umesh", courseName: "Big Data Analytics" }
{ name: "Veerendra", courseName: "Database Management" }

------------------------------------------------------------

# 🔹 3️⃣ Insert Email Data

db.xyz.insertMany([
  { name: "Krishna", email: "krishna01@gmail.com" },
  { name: "Muskan", email: "muskan02@gmail.com" },
  { name: "Rajendra", email: "rajendra@yahoo.com" }
])

Query:
db.xyz.find({ email: /@gmail\.com$/i })

Output:
{ name: "Krishna", email: "krishna01@gmail.com" }
{ name: "Muskan", email: "muskan02@gmail.com" }

------------------------------------------------------------

# 🔹 4️⃣ Insert City Data

db.xyz.insertMany([
  { name: "Priya", city: "Coimbt" },
  { name: "Meena", city: "Chandl" },
  { name: "Rakesh", city: "Chennai" }
])

Query:
db.xyz.find({ city: /^C.{5}$/ })

Output:
{ name: "Priya", city: "Coimbt" }
{ name: "Meena", city: "Chandl" }

------------------------------------------------------------

# 🔹 5️⃣ Insert Mobile Data

db.xyz.insertMany([
  { name: "Raj", mobile: "9876543210" },
  { name: "Simran", mobile: "9123456789" },
  { name: "Aman", mobile: "8123456789" }
])

Query:
db.xyz.find({ mobile: /^9[0-9]{9}$/ })

Output:
{ name: "Raj", mobile: "9876543210" }
{ name: "Simran", mobile: "9123456789" }

------------------------------------------------------------

# 🔹 6️⃣ Insert Experience (Embedded Array)

db.xyz.insertMany([
  {
    name: "Krishna",
    experience: [
      { company: "HDFC Bank" },
      { company: "TCS" }
    ]
  },
  {
    name: "Mohit",
    experience: [
      { company: "Axis Bank" }
    ]
  }
])

Query:
db.xyz.find({ "experience.company": /bank/i })

Output:
{ name: "Krishna", company: "HDFC Bank" }
{ name: "Mohit", company: "Axis Bank" }

------------------------------------------------------------

# 🔹 7️⃣ Insert Password Data

db.xyz.insertMany([
  { username: "rahul", password: "rahul123" },
  { username: "amit", password: "amit2024" },
  { username: "sara", password: "saraPass" }
])

Query:
db.xyz.find({ password: /[0-9]/ })

Output:
{ username: "rahul", password: "rahul123" }
{ username: "amit", password: "amit2024" }

------------------------------------------------------------

# 🔹 8️⃣ Insert Product Data

db.xyz.insertMany([
  { productName: "Smartphone", category: "Electronics" },
  { productName: "Speaker", category: "Home Electronics" },
  { productName: "Shirt", category: "Clothing" }
])

Query 1:
db.xyz.find({ productName: /^S/ })

Output:
{ productName: "Smartphone" }
{ productName: "Speaker" }
{ productName: "Shirt" }

Query 2:
db.xyz.find({ category: /electronics/i })

Output:
{ productName: "Smartphone", category: "Electronics" }
{ productName: "Speaker", category: "Home Electronics" }

------------------------------------------------------------

# 🔹 9️⃣ Insert Reviews Data

db.xyz.insertOne({
  reviews: [
    { review: "This product is very good" },
    { review: "Bad quality item" },
    { review: "Excellent product" }
  ]
})

Query:
db.xyz.find({ "reviews.review": /bad/i })

Output:
{ review: "Bad quality item" }

------------------------------------------------------------

# 🔟 Roll Number Query

Query:
db.xyz.find({ rollNo: /^CS2024/ })

Output:
{ name: "Aman", rollNo: "CS2024" }
{ name: "Anuj", rollNo: "CS2024" }

------------------------------------------------------------

# 📚 Regex Symbols

^      → Starts with  
$      → Ends with  
.      → Any character  
{n}    → Exact length  
[0-9]  → Digit  
/i     → Case insensitive  
$not   → Not matching  

------------------------------------------------------------
