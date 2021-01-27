# MongoDB Cheat Sheet

- [MongoDB Cheat Sheet](#mongodb-cheat-sheet)
  - [Filter Operators](#filter-operators)
    - [COUNT](#count)
    - [DELETE](#delete)
    - [EXISTS](#exists)
    - [FIND](#find)
    - [INSERT](#insert)
    - [LIMIT](#limit)
    - [SORT](#sort)
  - [Comparison Operators](#comparison-operators)
    - [LESS THAN](#less-than)
    - [LESS THAN OR EQUAL TO](#less-than-or-equal-to)
    - [GREATER THAN](#greater-than)
    - [GREATER THAN OR EQUAL TO](#greater-than-or-equal-to)
    - [EQUAL TO](#equal-to)
    - [NOT EQUAL TO](#not-equal-to)
    - [IN](#in)
    - [NOT IN](#not-in)
  - [Logical Operators](#logical-operators)
    - [NOT](#not)
    - [OR](#or)
    - [NOR](#nor)
    - [AND](#and)
    - [AND OR](#and-or)
  - [MODIFYING DOCUMENTS](#modifying-documents)
    - [UPDATE ONE](#update-one)
    - [UPDATE MANY](#update-many)
    - [MUL](#mul)
    - [INC](#inc)
    - [MAX](#max)
    - [MIN](#min)
    - [RENAME](#rename)
    - [SET](#set)
    - [UNSET](#unset)
    - [CURRENT DATE](#current-date)
  - [Array Operators](#array-operators)
    - [ADD TO SET](#add-to-set)
    - [ARRAY FILTERS](#array-filters)
    - [POP](#pop)
    - [PULL](#pull)
    - [PUSH](#push)
  - [Array Modifiers](#array-modifiers)
    - [EACH](#each)
    - [SLICE](#slice)
    - [ARRAY SORT](#array-sort)
    - [POSITION](#position)
    - [$ALL](#all)
    - [$ELEMMATCH](#elemmatch)
    - [$SIZE](#size)
  - [Evaluation Operators](#evaluation-operators)
    - [WHERE](#where)
    - [EXPR](#expr)
    - [REGEX](#regex)
    - [TEXT](#text)
    - [MOD](#mod)
  - [Aggregation](#aggregation)
  - [Aggregation Pipeline](#aggregation-pipeline)
  - [Aggregation Operators](#aggregation-operators)
    - [MATCH](#match)
    - [LIMIT](#limit-1)
    - [LOOKUP](#lookup)
      - [Equality Match](#equality-match)
    - [GROUP](#group)
      - [List of most used accumulators:](#list-of-most-used-accumulators)
    - [UNWIND](#unwind)
    - [PROJECT](#project)
  - [Arithmetic Expressions](#arithmetic-expressions)
    - [ABS](#abs)
    - [ADD FIELDS](#add-fields)
    - [ADD](#add)
    - [SUBTRACT](#subtract)
    - [CEIL](#ceil)
    - [FLOOR](#floor)
    - [DIVIDE](#divide)
    - [MULTIPLY](#multiply)

---

## Filter Operators

### COUNT

```javascript
db.collection.count();
```

This method counts and returns the number of results that match a query.

### DELETE

``` javascript
db.collection.deleteOne({ status: "D" });
```

Delete **the first** document that match a specified filter.

``` javascript
db.collection.deleteMany({ status : "A" });
```

Delete **all** documents that match a specified filter.

``` javascript
db.collection.deleteMany( {} );
```

### EXISTS

Check if a field exists in the document.
  - Use true to find a field that exists.
  - Use false to find documents in which a field does not exist.

``` javascript
db.collection.find({ qty: { $exists: true } });
```
The query returns the documents that contain the field `qty`, including documents where the field value is `null`.

``` javascript
db.collection.find({ qty: { $exists: false } });
```
The query returns only the documents that do not contain the field `qty`.

### FIND

```javascript
db.collection.find({});
```

Selects all documents in a collection.

```javascript
db.collection.find({}, { name: 1 });
```

The second parameter (optional) specifies the fields to return in the document.

```javascript
db.collection.find({ releaseYear: "2021" }, { title: 1, author: 1, _id: 0 });
```

Returns all documents that match the query, and display only the `title` and `author`.

### INSERT

``` javascript
db.createCollection( "minhaColecao4", { collation: { locale: "fr" } } );
```

Create a new collection.

```javascript
db.collection.insertOne({ chave: "Valor", chave2: "Valor2" });
```

Insert only one document in the collection. If the collection doesn't exist, mongodb automatically creates it.

```javascript
db.collection.insertMany([ { chave: "Valor", chave2: "Valor2" }, { chave: "Valor", chave2: "Valor2" } ]);
```

Insert more than one document in a collection.

### LIMIT

``` javascript
db.collection.find("<query>").limit("<número>");
```
Specify the number of documents to be returned.

```javascript
db.collection.find().skip(2); 
```

Specify the number of documents to be skipped. It controls where mongodb will begin to return results.

```javascript
db.collection.find({}).limit(10).skip(5);
```

Return ten results, skipping the first five documents, from the collection.

### SORT

Sort items, numerically or alphabetically.
  - Ascending order (Ex: sort: { key: 1 })
  - Descending order (Ex: sort: { key: -1 })

```javascript
db.collection.find({}).sort({ "price": 1 });
```

Sort the field `price` in ascending order for all documents in a collection. 

---

## Comparison Operators

### LESS THAN
``` javascript
db.collection.find({ qty: { $lt: 20 } });
```
Returns all documents in which the field `qty` is less than 20.

### LESS THAN OR EQUAL TO

``` javascript
db.collection.find({ qty: { $lte: 20 } });
```

Returns all documents in which the field `qty` is less than or equal 20.

### GREATER THAN

``` javascript
db.collection.find({ qty: { $gt: 20 } });
```

Returns all documents in which the field `qty` is greater than 20.

### GREATER THAN OR EQUAL TO

``` javascript
db.collection.find({ qty: { $gte: 20 } });
```

Returns all documents in which the field `qty` is greater than or equal 20.


### EQUAL TO

``` javascript
db.collection.find({ qty: { $eq: 20 } });
db.collection.find({ qty: 20 });
```

Returns all documents in which the field `qty` is equal to 20.


### NOT EQUAL TO

``` javascript
db.collection.find({ qty: { $ne: 20 } });
```

Returns all documents in which the field `qty` is not equal to 20.


### IN

``` javascript
db.collection.find({ qty: { $in: [ 5, 15 ] } });
```

Returns all documents in which the field `qty` is equal to 5 **or** 15.


### NOT IN

``` javascript
db.collection.find({ qty: { $in: [ 5, 15 ] } });
```

Returns all documents in which the field `qty` is not equal to 5 **neither** 15. It also returns documents in which the field `qty` does not exist.


---

## Logical Operators 

### NOT

``` javascript
db.collection.find({ price: { $not: { $gt: 1.99 } } });
```

### OR

``` javascript
db.collection.find({ $or: [{ qty: { $lt: 20 } }, { price: 10 }] });
```

Executes a logical operator **OR** on an array of two or more query expressions, and returns the documents that satisfy *at least* one of them.

### NOR

```javascript
db.collection.find({ $nor: [{ price: 1.99 }, { sale: true }] });
```

Executes a logical operator **NOR** on an array of two or more query expressions, and returns the documents that fail *at least* one of them.

### AND

``` javascript
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

```javascript
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

```javascript
db.collection.updateOne(
  { item: "paper" },
  { $set: { "size.uom": "cm", status: "P" } },
  { $upsert: true }
)
```


### UPDATE MANY

Updates **all** the documents that match the filter criteria. This method takes the same parameters as `updateOne`.

```javascript
db.collection.updateMany(
  { item: "paper" },
  { $set: { "size.uom": "cm", status: "P" } },
  { $upsert: true }
)
```

### MUL

Multiply the value of a field by a number, updating the document. If the field does not exist, $mul creates it and sets the value to zero.

```javascript
db.collection.updateOne(
   { _id: 1 },
   { $mul: { price: NumberDecimal("1.25"), qty: 2 } }
)
```

### INC
Increments (positive number) or decrements (negative number) a field by a specified value. If the field does not exist, $inc creates the field and sets the field to the specified value.

```javascript
db.collection.updateOne(
  { sku: "abc123" },
  { $inc: { quantity: -2, orders: 10 } }
);
```

### MAX
The $max sets the upper limit of a field. It compares the specified value with the field's value. If the current value is lower than the specified value, the document will be updated. Otherwise the field's value won't be changed.

```javascript
db.collection.update({ _id: 1 }, { $max: { highScore: 950 } });
```
In the example above, if the current value is lower than 950 the $max will update the `highScore` field to 950. If it is already higher than 950, nothing will happen.

### MIN
The $min sets the lower limit of a field. It compares the specified value with the field's value. If the current value is greater than the specified value, the document will be updated. Otherwise the field's value won't be changed.

```javascript
db.collection.update({ _id: 1 }, { $min: { lowScore: 150 } });
```
In the example above, if the current value is higher than 150 the $min will update the `lowScore` field to 150. If it is already lower than 150, nothing will happen.

### RENAME
Update the name of a field.

```javascript
db.collection.updateMany([
  {},
  { $rename: {
      "name": "productName"
    }
  }
]
);
```
In the example above, $rename will update the `name` field to `productName` in all documents.

### SET
Add one or more fields.

```javascript
db.collections.update(
  { _id: 100 },
  { $set: {
      quantity: 500,
      details: { model: "14Q3", make: "xyz" },
      tags: [ "coats", "outerwear", "clothing" ]
    }
  }
);
```
In the example above, $set adds the fields `quantity`, `details` and `tags` in the documents with `_id: 100`.

### UNSET
Remove one or more fields.

```javascript
db.collection.updateMany(
  { productName: "Banana" },
  { $unset: { quantity: "" } }
);
```
In the example above, $unset deletes the field `quantity` in the documents with `productName: "Banana"`.

### CURRENT DATE
Set the value of a field to the current date, either as a Date or a timestamp. 

```javascript
db.customers.updateOne(
   { _id: 1 },
   {
     $currentDate: {
        lastModified: true,
        "cancellation.date": { $type: "timestamp" }
     },
     $set: {
        "cancellation.reason": "user request",
        status: "D"
     }
   }
)
```
The example above updates the lastModified field to the current date, the "cancellation.date" field to the current timestamp as well as updating the status field to "D" and the "cancellation.reason" to "user request".

---

## Array Operators

### ADD TO SET

The `$addToSet` operator adds a value to an array **only** if it is not present. If the array already contains the value, the `$addToSet` does nothing. It ensures that there are no duplicate items being added to the array.

```javascript
db.collection.updateOne(
  { _id: 1 },
  { $addToSet: { tags: "accessories" } }
);
```
When multiple values are added. the method can be used with the operator `$each`. In the example below, the items "camera" and "electronic" will be added to the array in the field `tags`. It won't add "accessories" because this item already exists in the array.

```javascript
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

### ARRAY FILTERS

``` javascript
db.collection.updateMany(
  {},
  { $set : {
    "ingredients.$[elemento].unit": "xícara",
    "ingredients.$[elemento].name": "Farinha Integral"
    }
  }, 
  { arrayFilters: [ { "elemento.name": "Farinha" } ]}
);
```

### POP

```javascript
db.collection.updateOne({ _id: 1 }, { $pop: { items: -1 } });
```
Removes the first item of an array in the field `items`.

```javascript
db.collection.update({ _id: 1 }, { $pop: { items: 1 } });
```
Removes the last item of an array in the field `items`.

### PULL

Removes items of an array if the item match the query condition.

```javascript
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

``` javascript
db.collection.updateOne(
  { _id: 1 },
  {
pull: {
      votes: { $gte: 6 }
    }
  }
);
```

``` javascript
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

``` javascript
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
  { $upsert: true }
);
```

``` javascript
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
);
```

``` javascript
db.collection.updateOne(
  { _id: 1},
  { $push: {
      items: { 
$each: [
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
$sort: { quantity: -1 },
$slice: 2
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

### $ALL

Selects the documents where the value of a field is an array that contains all the specified elements.

``` javascript
db.inventory.find(
  { tags: { $all: ["red", "blank"] } }
);
```

### $ELEMMATCH

Select documents that contain an array field with at least one element that matches all the specified filters.

``` javascript
db.collection.find(
  { results: { $elemMatch: { $gte: 80, $lt: 85 } } }
);
```

### $SIZE

Select documents that match any array with the number of elements specified by the argument.

``` javascript
db.products.find(
  { tags: { $size: 2 } }
);
```

---

## Evaluation Operators

### WHERE

Use to pass either a string containing a JavaScript expression or a full JavaScript function.

Starting in MongoDB 3.6, the **$expr** operator allows the use of aggregation expressions.

If you must create custom expressions, $function is preferred over **$where**.

### EXPR

Allows the use of [aggregation](#aggregation) expressions. It compares fields from **the same** document.

``` javascript
db.monthlyBudget.find(
  {
expr: { $gt: [ "$spent", "$budget" ] }
  }
);
```

### REGEX

Provides regular expression capabilities for pattern matching strings.

``` javascript
db.products.find({ sku: { $regex: /789$/ } });
```

``` javascript
db.products.find({ sku: { $regex: /^ABC/i } });
```

### TEXT

Performs a text search on the content of the fields indexed with a text index.


```javascript
{
  $text:
  {
    search: <string>,
    language: <string>,
    caseSensitive: <boolean>,
    diacriticSensitive: <boolean>
  }
}
```

`$search`: string of terms that MongoDB parses and uses to query the text index;

`$language`: determines the list of stop words for the search and the rules for the tokenizer;

`$caseSensitive`: boolean flag to enable or disable case sensitive search. Defaults to **false**;

`$diacriticSensitive`: boolean flag to enable or disable diacritic sensitive. Defaults to **false**

### MOD

Select documents where the value of a field divided by a divisor has the specified remainder.

```javascript
db.inventory.find({ qty: { $mod: [4, 0] } });
```

## Aggregation

Aggregation expressions use **field path** to access fields in the input documents. To specify a field path, prefix the field name with a dollar sign $.

## Aggregation Pipeline

The aggregation pipeline is a framework for MongoDB. Documents enter a multi-stage pipeline that transforms the documents into an aggregated result. Documents pass through the stages in sequence. Each stage transforms the documents as they pass from one stage to another. 

## Aggregation Operators

### MATCH

````javascript
db.collection.aggregate([
    { $match: { <query> } },
]);
````

Filters the documents so only those that match the specified condition(s) may enter the next stage. It resembles the filters from **find()** method.

```javascript
db.collection.aggregate([
    { $match: { name: "Alex" } },
]);
```

```javascript
db.collection.find({ name: "Alex" });
```

Select all documents that match the query.

This operator may be used at any stage of the pipeline. When used as the first stage, it should improve considerably the performace of your query. In the other hand, when used at any other point, it will act similar to a **HAVING** from the SQL.

---

### LIMIT

```` javascript
db.collection.aggregate([
    { $limit: <positive integer> },
]);
````

Passes only the first n documents to the next stage in the pipeline, in which n is the specified positive integer.

````javascript
db.collection.aggregate([
	{ $match: { price: 10 } },
    { $limit: 5 },
]);
````

Returns the first five results that matches the specified condition from the previous stage.

---

### LOOKUP

This operator allows the use of documents from another collection (table), similar to a **JOIN** from the SQL. The documents from the joined collection will be avaible through a new array.

There are two types of `$lookup`:

#### Equality Match

````javascript
db.collection.aggregate([
	{
        $lookup:
        {
          from: <collection to join>,
       	  localField: <field from the input documents>,
       	  foreignField: <field from the documents of the "from" collection>,
       	  as: <output array field>
        },
    },
]);
````

````javascript
db.collection.aggregate([
	{
        $lookup:
        {
          from: <inventory>,
       	  localField: <item>,
       	  foreignField: <sku>,
       	  as: <inventory_docs>
        },
    },
]);
````

----

### GROUP

This operator groups documents by the specified `_id` expression. It's worth mentioning that the specified `_id` is actually a "parameter" of `$group` and is not to be mistaken with `_id` from the collections. The operator `_id` is required. If you specify a value of `null` the $group stage calculates accumulated values for the input document as a whole.

````javascript
db.collection.aggregate([
	{
		$group:
		{
			_id: <expression>,
			<field1>: { <accumulator1> : <expression1> },
			...
			<fieldN>: { <accumulatorN> : <expressionN> },
		},
	},
]);
````

The `<field>` may be named as you see fit, however, the `<accumulator>` must be an accumulator operator.

````javascript
db.collection.aggregate([
  {
    $group : {
      _id : "$itemId",
      count: { $sum: 1},
    }
  }
]);
````

The output would be something like this:

````bash
{ _id : "Chair", count : 3 }
{ _id : "Table", count : 2 }
{ _id : "Lamp", count : 2 }
````


#### List of most used accumulators:

- $addToSet
- $avg
- $first
- $last
- $max
- $sum

---

### UNWIND

````javascript
db.collection.aggregate([
    { $unwind: <field path> },
]);
````

This operator desconstructs an array field into documents. To be more precisely, a document for each element of the array.

Let's take the following document as an example:

````javascript
{
    _id: 7,
    name: "GMusicApp",
    subscription: ["free", "premium", "family", "university"]
}
````

If we apply the `$unwind` operator:

````javascript
db.collection.aggregate([
    { $unwind: "$subscription" },
]);
````

We would get following output:

````javascript
{ _id: 7, name: "GMusicApp", subscription: "free" },
{ _id: 7, name: "GMusicApp", subscription: "premium" },
{ _id: 7, name: "GMusicApp", subscription: "family" },
{ _id: 7, name: "GMusicApp", subscription: "university" },
````

---

### PROJECT

````javascript
db.collection.aggregate([
    {
        project: {
            <specification(s)>
        },
    },
]);
````

Simple speaking,  this operator is similar to the 2nd parameter of `.find()`. It allows you to manipulate the output, passing along documents with only the specified fields. Those fields can be either from the input documents or newly computed ones.

````javascript
db.collection.aggregate([
    {
        $project: {
            _id: 0, // or false
            productName: "$name",
            quantity: 1, // or true
            profit: {
                $subtract: ["$sale_price", "$cost_price"]
            },
        },
    },
]);
````

## Arithmetic Expressions

### ABS

Expression: **$abs**
This arithmetic expression returns the absolute value of a number.

```javascript
db.collection.aggregate([
  {
    project: {
      delta: {
        abs: { $subtract: ["$start", "$end"] }
      }
    }
  }
]);
```

### ADD FIELDS

Expression: **$addFields**
This arithmetic expression let you add new fields to the documents but only into the query output.

```javascript
db.collection.aggregate([
  {
    $addFields: {
      totalHomework: { $sum: "$homework" } ,
      totalQuiz: { $sum: "$quiz" }
    }
  },
  {
    $addFields: {
      totalScore: {
        $add: [ "$totalHomework", "$totalQuiz", "$extraCredit" ]
      }
    }
  }
]);
```

### ADD

Expression: **$add**
This arithmetic expression let you sum field values.

```javascript
db.collection.aggregate([
  { $project: { item: 1, total: { $add: ["$price", "$fee"] } } }
]);
```

### SUBTRACT

Expression: **$subtract**
This arithmetic expression let you subtract field values.

```javascript
db.collection.aggregate([
  {
    $project: {
      item: 1,
      total: {
        $subtract: [
          { $add: ["$price", "$fee"] },
          "$discount"
        ]
      }
    }
  }
]);
```

### CEIL

Expression: **$ceil**
This arithmetic expression returns the smallest integer that is greater than or equal to the field value.

```javascript
db.collection.aggregate([
  { $project: { value: 1, ceilingValue: { $ceil: "$value" } } }
]);
```

### FLOOR

Expression: **$floor**
This arithmetic expression returns the smallest integer that is lower than or equal to the field value.

```javascript
db.collection.aggregate([
  { $project: { value: 1, floorValue: { $floor: "$value" } } }
]);
```

### DIVIDE

Expression: **$divide**
This arithmetic expression returns the result of a division between fields.

```javascript
db.collection.aggregate([
  {
    project: {
      name: 1,
      workdays: {
        divide: ["$hours", 8]
      }
    }
  }
]);
```

### MULTIPLY

Expression: **$multiply**
This arithmetic expression returns the result of a multiplication between fields.

```javascript
db.collection.aggregate([
  {
    project: {
      date: 1,
      item: 1,
      total: {
        multiply: ["$price", "$quantity"]
      }
    }
  }
]);
```
