# MongoDB Study Notes
> Practical session covering embedded documents, arrays, aggregation, and lookups.

---

## 1. Collection: `Students`

### Insert Multiple Students
```js
db.Students.insertMany([
  {
    studentId: 101,
    name: 'Rahul Sharma',
    course: 'BSC',
    address: { street: "MP Nagar", city: "Bhopal", state: "MP", pincode: 462001 },
    emergencyContacts: [
      { name: 'Dev',    mobile_no: '9087653421' },
      { name: 'Sakshi', mobile_no: '8765421309' }
    ]
  },
  {
    studentId: 102,
    name: 'Abhay',
    course: 'B Tech',
    address: { street: "Arera Colony", city: "Bhopal", state: "MP", pincode: 462016 },
    emergencyContacts: [
      { name: 'Krishna',  mobile_no: '8979042134' },
      { name: 'Veerendra', mobile: '8079190246' }
    ]
  },
  {
    studentId: 103,
    name: 'Vaishnavi',
    course: 'B Tech',
    address: { street: "Vijay Nagar", city: "Indore", state: "MP", pincode: 452010 },
    emergencyContacts: [
      { name: 'Muskan', mobile_no: '9086574321' }
    ]
  },
  {
    studentId: 104,
    name: 'Vinod',
    course: 'MCA',
    address: { street: "Civil Lines", city: "Gwalior", state: "MP", pincode: 474001 },
    emergencyContacts: [
      { name: 'Shivam', mobile_no: '9873246150' },
      { name: 'Woh',    mobile_no: '9087654321' }
    ]
  },
  {
    studentId: 5,
    name: 'Karan Patel',
    course: 'B.Sc',
    address: { street: "Shahpura", city: "Bhopal", state: "MP", pincode: 462039 },
    emergencyContacts: [
      { name: 'Sunita Patel', phone: '7777777777' }
    ]
  }
])
```

---

### Query — Find Students by Nested Field (city)
```js
db.Students.find({ 'address.city': 'Bhopal' })
```
Returns students with `studentId` 101, 102, and 5 (Karan Patel).

> **Key Concept:** Use dot notation (`'address.city'`) to query nested/embedded document fields.

---

### Update — Modify a Nested Field with `$set`
```js
db.Students.updateOne(
  { studentId: 101 },
  { $set: { 'address.pincode': 462002 } }
)
```

---

### Update — Add to an Array with `$push`
```js
db.Students.updateOne(
  { studentId: 103 },
  { $push: { emergencyContacts: { name: 'Rajendra', mobile_no: '9650721409' } } }
)
```
Adds a new contact object into the `emergencyContacts` array.

---

### Update — Remove from an Array with `$pull`
```js
db.Students.updateOne(
  { studentId: 103 },
  { $pull: { emergencyContacts: { name: 'Rajendra', mobile_no: '9650721409' } } }
)
```
Removes the matching element from the array.

---

## 2. Collection: `Orders`

### Insert Orders
```js
db.Orders.insertMany([
  {
    orderId: 101,
    customerName: 'Veer',
    orderDate: new Date("2026-02-20"),
    items: [
      { productName: 'Mouse',    price: 450,  quantity: 3 },
      { productName: 'KeyBoard', price: 1100, quantity: 2 }
    ],
    shippingAddress: { street: 'Civil Lines', city: 'Sagar', pincode: 46001 }
  },
  {
    orderId: 102,
    customerName: 'Krishna',
    orderDate: new Date("2026-02-20"),
    items: [
      { productName: 'Laptop',    price: 49000, quantity: 1 },
      { productName: 'headPhone', price: 2100,  quantity: 2 }
    ],
    shippingAddress: { street: 'Madical side', city: 'Sagar', pincode: 46101 }
  },
  {
    orderId: 103,
    customerName: 'Veerendra',
    orderDate: new Date("2026-02-20"),
    items: [
      { productName: 'Mouse',     price: 1450, quantity: 2 },
      { productName: 'Pen Drive', price: 1300, quantity: 3 }
    ],
    shippingAddress: { street: 'Makronia', city: 'Sagar', pincode: 46003 }
  },
  {
    orderId: 104,
    customerName: 'Veer',
    orderDate: new Date("2026-02-20"),
    items: [
      { productName: 'Ear Buds', price: 2700,  quantity: 2 },
      { productName: 'Mobile',   price: 18500, quantity: 1 }
    ],
    shippingAddress: { street: 'Paradise Hotel', city: 'Sagar', pincode: 463001 }
  },
  {
    orderId: 105,
    customerName: 'Vinod',
    orderDate: new Date("2026-02-20"),
    items: [
      { productName: 'SSD',      price: 2200, quantity: 2 },
      { productName: 'KeyBoard', price: 750,  quantity: 1 }
    ],
    shippingAddress: { street: 'Gopal Ganj', city: 'Sagar', pincode: 46005 }
  }
])
```

---

### Query — Find by Array Item Field
```js
db.Orders.find({ 'items.productName': 'Mouse' })
```
Returns orders 101 and 103.

> **Key Concept:** MongoDB automatically searches through array elements when you use dot notation on an array field.

---

### Update — Increment a Specific Array Element with Positional Operator `$`
```js
db.Orders.updateOne(
  { 'items.productName': 'Laptop' },
  { $inc: { 'items.$.quantity': 1 } }
)
```
The `$` positional operator targets the **first matching element** in the array.

> ⚠️ Using `'items.quantity'` (without `$`) fails because MongoDB cannot determine which array element to update.

---

### Update — Remove an Item from an Order's Array
```js
db.Orders.updateOne(
  { orderId: 101 },
  { $pull: { items: { productName: "Mouse" } } }
)
```

---

### Aggregation — Total Quantity Per Order

```js
db.Orders.aggregate([
  { $unwind: "$items" },
  {
    $group: {
      _id: "$orderId",
      totalItems: { $sum: "$items.quantity" }
    }
  }
])
```

**Result:**

| orderId | totalItems |
|---------|------------|
| 103     | 5          |
| 101     | 5          |
| 102     | 4          |
| 105     | 3          |
| 104     | 3          |

> **Key Concepts:**
> - `$unwind` — Deconstructs an array field so each element becomes a separate document.
> - `$group` — Groups documents by a field and applies accumulators like `$sum`, `$avg`, `$count`.
> - Always prefix field references with `$` inside expressions (e.g., `"$items.quantity"`).

---

## 3. Collection: `blogs`

### Insert Blogs with Nested Comments
```js
db.blogs.insertMany([
  {
    title: "MongoDB Basics",
    author: "Raj",
    comments: [
      { username: "Rahul", message: "Very helpful!",     date: new Date("2026-02-01") },
      { username: "Priya", message: "Nice explanation",  date: new Date("2026-02-02") },
      { username: "Ankit", message: "Good blog",         date: new Date("2026-02-03") },
      { username: "Neha",  message: "Thanks!",           date: new Date("2026-02-04") }
    ]
  },
  {
    title: "Spring Boot Guide",
    author: "Amit",
    comments: [
      { username: "Karan", message: "Very informative", date: new Date("2026-02-10") },
      { username: "Rahul", message: "Helped me a lot",  date: new Date("2026-02-11") }
    ]
  }
])
```

---

### Query — Find Blogs with More Than 3 Comments using `$expr`
```js
db.blogs.find({
  $expr: { $gt: [ { $size: "$comments" }, 3 ] }
})
```
Returns "MongoDB Basics" (4 comments).

> **Key Concept:** `$expr` allows using aggregation expressions inside `find()`. `$size` returns the length of an array.

---

### Update — Modify a Specific Array Element with Positional `$`
```js
db.blogs.updateOne(
  { 'comments.username': 'Neha' },
  { $set: { "comments.$.message": 'Thank you very much' } }
)
```

---

### Update — Remove Array Elements by Condition with `$pull`
```js
db.blogs.updateMany(
  {},
  {
    $pull: {
      comments: { date: { $lt: new Date("2026-02-01") } }
    }
  }
)
```
Removes all comments with a date before Feb 1, 2026 across all blog documents.

---

## 4. Collections: `books` & `borrowRecords` — Joins with `$lookup`

### Setup
```js
db.books.insertMany([
  { _id: 1, title: "Java Programming",  author: "James Gosling", category: "Programming" },
  { _id: 2, title: "MongoDB Guide",     author: "John Smith",    category: "Database" },
  { _id: 3, title: "Data Structures",   author: "Mark Allen",    category: "Computer Science" },
  { _id: 4, title: "Operating Systems", author: "Galvin",        category: "Computer Science" }
])

db.borrowRecords.insertMany([
  { bookId: 1, studentName: "Rahul",    borrowDate: new Date("2026-01-10") },
  { bookId: 2, studentName: "Priya",    borrowDate: new Date("2026-01-15") },
  { bookId: 1, studentName: "Ankit",    borrowDate: new Date("2026-02-01") }
])
```

---

### Aggregation — Join collections with `$lookup`
```js
db.books.aggregate([
  {
    $lookup: {
      from:         'borrowRecords',  // collection to join
      localField:   '_id',           // field in books
      foreignField: 'bookId',        // field in borrowRecords
      as:           'borrowDetails'  // output array field name
    }
  }
])
```
> **Key Concept:** `$lookup` is MongoDB's equivalent of SQL `LEFT JOIN`. Books with no borrow records will have an empty `borrowDetails: []` array.

>

---

### Aggregation — Find Books That Have Never Been Borrowed
```js
db.books.aggregate([
  {
    $lookup: {
      from:         'borrowRecords',
      localField:   '_id',
      foreignField: 'bookId',
      as:           'borrowDetails'
    }
  },
  {
    $match: { borrowDetails: { $size: 0 } }
  }
])
```
Returns "Data Structures" and "Operating Systems".

---

### Aggregation — Filter Before Joining with `$match` + `$lookup`
```js
db.books.aggregate([
  { $match: { _id: 1 } },
  {
    $lookup: {
      from:         'borrowRecords',
      localField:   '_id',
      foreignField: 'bookId',
      as:           'borrowDetails'
    }
  }
])
```
Returns only "Java Programming" along with its borrow records.

> **Key Concept:** Placing `$match` before `$lookup` improves performance by reducing the number of documents that go through the join.

---

## Quick Reference — Key Operators

| Operator | Purpose |
|----------|---------|
| `$set` | Update/add a field value |
| `$inc` | Increment a numeric field |
| `$push` | Add an element to an array |
| `$pull` | Remove matching element(s) from an array |
| `$` (positional) | Targets the first matched array element in updates |
| `$size` | Matches/returns the size of an array |
| `$expr` | Use aggregation expressions inside `find()` |
| `$unwind` | Deconstructs array field into separate documents |
| `$group` | Groups documents and applies accumulators |
| `$lookup` | Joins documents from another collection (like SQL LEFT JOIN) |
| `$match` | Filters documents (like SQL WHERE) |
| `$sum` | Accumulator — sums numeric values in `$group` |
