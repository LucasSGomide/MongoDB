# MongoDB Cheat Sheet
* [**Filter Operators**](#filter-operators)
  * [COUNT](#count)
  * [DELETE](#delete)
  * [EXISTS](#exists)
  * [FIND](#find)
  * [INSERT](#insert)
  * [LIMIT](#limit)
  * [SORT](#sort)
* [**Comparison Operators**](#comparison-operators)
  * [LESS THAN](#less-than)
  * [LESS THAN OR EQUAL TO](#less-than-or-equal-to)
  * [GREATER THAN](#greater-than)
  * [GREATER THAN OR EQUAL TO](#greater-than-or-equal-to)
  * [EQUAL TO](#equal-to)
  * [NOT EQUAL TO](#not-equal-to)
  * [IN](#in)
  * [NOT IN](#not-in)
* [**Logical Operators**](#logical-operators)
  * [NOT](#not)
  * [OR](#or)
  * [NOR](#nor)
  * [AND](#and)
  * [AND OR](#and-or)
* [**Array Modifiers**](#array-modifiers)
  * [EACH](#each)
  * [SLICE](#slice)
  * [ARRAY SORT](#array-sort)
  * [POSITION](#position)
* [**Array Operators**](#array-operators)
  * [$ADD TO SET](#add-to-set)
  * [$FILTERS](#filters)
  * [$POP](#pop)
  * [$PULL](#pull)
  * [$PUSH](#push)


---

## Filter Operators

### COUNT
```
db.collection.count()
```

This method counts and returns the number of results that match a query.

### DELETE
```
db.collection.deleteOne({ status: "D" });
```

Delete **the first** document that match a specified filter.

```
db.collection.deleteMany({ status : "A" });
```

Delete **all** documents that match a specified filter.


### EXISTS

Check if a field exists in the document.
  - Use true to find a field that exists.
  - Use false to find documents in which a field does not exist.

```
db.collection.find({ qty: { $exists: true } })
```

The query returns the documents that contain the field `qty`, including documents where the field value is `null`.

```
db.collection.find({ qty: { $exists: false } })
``` 

The query returns only the documents that do not contain the field `qty`.

### FIND

```
db.collection.find({});
``` 

Selects all documents in a collection.

```
db.collection.find({}, { name: 1 });
``` 

The second parameter (optional) specifies the fields to return in the document.

```
db.collection.find({ releaseYear: "2021" }, { title: 1, author: 1, _id: 0 });
``` 

Returns all documents that match the query, and display only the `title` and `author`.

### INSERT

```
db.createCollection( "minhaColecao4", { collation: { locale: "fr" } } );
```

Create a new collection.

```
db.collection.insertOne({ chave: "Valor", chave2: "Valor2" });
``` 

Insert only one document in the collection. If the collection doesn't exist, mongodb automatically creates it.

```
db.collection.insertMany([ { chave: "Valor", chave2: "Valor2" }, { chave: "Valor", chave2: "Valor2" } ]);
```

Insert more than one document in a collection.

### LIMIT

```
db.collection.find("<query>").limit("<número>");
```

Specify the number of documents to be returned.

```
db.collection.find().skip(2); 
```

Specify the number of documents to be skipped. It control from where mongodb will begin to return results.

```
db.collection.find({}).limit(10).skip(5);
``` 

Return ten results, skipping the first five documents, from the collection.

### SORT

Sort items, numerically or alphabetically.
  - Ascending order (Ex: sort: { key: 1 })
  - Descending order (Ex: sort: { key: -1 })

```
db.collection.find({}).sort({ "price": 1 })
``` 

Sort the field `price` in ascending order for all documents in a collection. 

---

## Comparison Operators

### LESS THAN
```
db.collection.find({ qty: { $lt: 20 } });
``` 
Returns all documents in which the field `qty` is less than 20.

### LESS THAN OR EQUAL TO

```
db.collection.find({ qty: { $lte: 20 } });
``` 

Returns all documents in which the field `qty` is less than or equal 20.

### GREATER THAN

```
db.collection.find({ qty: { $gt: 20 } });
``` 

Returns all documents in which the field `qty` is greater than 20.

### GREATER THAN OR EQUAL TO

```
db.collection.find({ qty: { $gte: 20 } });
``` 

Returns all documents in which the field `qty` is greater than or equal 20.


### EQUAL TO

```
db.collection.find({ qty: { $eq: 20 } });
db.collection.find({ qty: 20 });
``` 

Returns all documents in which the field `qty` is equal to 20.


### NOT EQUAL TO

```
db.collection.find({ qty: { $ne: 20 } });
```

Returns all documents in which the field `qty` is not equal to 20.


### IN

```
db.collection.find({ qty: { $in: [ 5, 15 ] } });
```

Returns all documents in which the field `qty` is equal to 5 **or** 15.


### NOT IN

```
db.collection.find({ qty: { $in: [ 5, 15 ] } });
```

Returns all documents in which the field `qty` is not equal to 5 **neither** 15. It also returns documents in which the field `qty` does not exist.


---

## Logical Operators 

### NOT

```
db.collection.find({ price: { $not: { $gt: 1.99 } } });
``` 

### OR

```
db.collection.find({ $or: [{ qty: { $lt: 20 } }, { price: 10 }] });
``` 

Executes a logical operator **OR** on an array of two or more query expressions, and returns the documents that satisfy *at least* one of them.

### NOR

```
db.collection.find({ $nor: [{ price: 1.99 }, { sale: true }] });
``` 

Executes a logical operator **NOR** on an array of two or more query expressions, and returns the documents that fail *at least* one of them.

### AND

```
db.collection.find({
  $and: [
    { price: { $ne: 1.99 } },
    { price: { $exists: true } }
  ]
});
``` 

Executes a logical operator **AND** on an array of two or more expressions, and returns the documents that satisfy *all* of them.

### AND OR

It is possible to combine the operators to create more complex query. The expression below returns all documents in the collection that have the field `price` between 0.99 - 1.99 **AND** the field `sale` true or the field `qty`less than 20.

```
db.collection.find({
  $and: [
    {
      $or: [
        { price: { $gt: 0.99, $lt: 1.99 } }
      ]
    },
    {
      $or: [
        { sale : true },
        { qty : { $lt : 20 } }
      ]
    }
  ]
});
``` 

---

## MODIFYING DOCUMENTS

### UPDATE ONE

Updates **a single document** based on the filter (the first one to match it). This method takes the following parameters:
1. filter: the selection criteria for the update
2. $set: The modifications that will be applied
3. $upsert: When `true`, creates a new document if no documents match the filter. This parameter is *optional*. 

```
db.collection.updateOne(
  { item: "paper" },
  { $set: { "size.uom": "cm", status: "P" } },
  { $upsert: true }
)
```


### UPDATE MANY

Updates **all** the documents that match the filter criteria. This method takes the same parameters as `updateOne`.

```
db.collection.updateMany(
  { item: "paper" },
  { $set: { "size.uom": "cm", status: "P" } },
  { $upsert: true }
)
```

---

## Array Operators

### ADD TO SET

The `$addToSet` operator adds a value to an array **only** if it is not present. If the array already contains the value, the `$addToSet` does nothing. It ensures that there are no duplicate items being added to the array.

```
db.collection.updateOne(
  { _id: 1 },
  { $addToSet: { tags: "accessories" } }
);
``` 

When multiple values are added. the method can be used with the operator `$each`. In the example below, the items "camera" and "electronic" will be added to the array in the field `tags`. It won't add "accessories" because this item already exists in the array.

```
db.collection.updateOne(
  { _id: 2 },
  {
    addToSet: {
      tags: {
        each: ["camera", "electronics", "accessories"]
      }
    }
  }
);
```

### POP

Removes the first item of an array in the field `items`.

```
db.collection.updateOne({ _id: 1 }, { $pop: { items: -1 } });
```

Removes the last item of an array in the field `items`.

```
db.collection.update({ _id: 1 }, { $pop: { items: 1 } });
```

### PULL

```
db.collection.updateMany(
  {},
  {
pull: {
      "items": {
        "name": { $in: ["pens", "envelopes"] }
      },
    }
  }
);
```

```
db.collection.updateOne(
  { _id: 1 },
  {
pull: {
      votes: { $gte: 6 }
    }
  }
);
```

```
db.collection.updateMany(
  {},
  {
pull: {
      results: { score: 8 , item: "B" }
    }
  }
);
```

### PUSH

```
db.collection.updateOne(
  {_id: 1 },
  { $push:
    { 
      items: {
        "name": "notepad",
        "price":  35.29,
        "quantity": 2
      },
    } 
  },
  { upsert: true }
);
```

```
db.collection.updateOne(
  {},
  { $push: {
      items:
      { $each: [ 
         {
          "name": "pens",
          "price": 56.12,
          "quantity": 5 
        },
        {
          "name": "envelopes",
          "price": 19.95,
          "quantity": 8
        }
      ] }
    }
  },
  { upsert: false }
);
```

```
db.collection.updateOne(
  { _id: 1},
  { $push: {
      items: { 
each: [
          {
            "name" : "notepad",
            "price" : 35.29,
            "quantity" : 2
          },
          {
            "name": "envelopes",
            "price": 19.95,
            "quantity": 8
          },
          {
            "name": "pens",
            "price": 56.12,
            "quantity": 5
          }
      ],
sort: { quantity: -1 },
slice: 2
    }
    }
  },
  { upsert: true }
);
```

---

## Array Modifiers

### EACH

Modifier: **$each**
Add multiple values into an Array

### SLICE

Modifier: **$slice**
Slice the array in a specific index (Ex: slice: 2)
  - **Must** be used with **$each**

### ARRAY SORT

Modifier: **$sort**
Sort items into the array.
  - Ascending order (Ex: sort: { key: 1 })
  - Descending order (Ex: sort: { key: -1 })
  - **Must** be used with **$each**

### POSITION

Modifier: **$position**
Insert item into a specific index
  - Without it the new item will be inserted at the last position in the array
  - **Must** be used with **$each**

---
