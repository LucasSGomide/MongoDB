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
* [**Modifying Documents**](#modifying-documents)
  * [UPDATE ONE](#update-one)
  * [UPDATE MANY](#update-many)
* [**Array Operators**](#array-operators)
  * [$ADD TO SET](#add-to-set)
  * [$ARRAY FILTERS](#array-filters)
  * [$POP](#pop)
  * [$PULL](#pull)
  * [$PUSH](#push)
  * [$ALL](#all)
  * [$ELEMMATCH](#elemmatch)
  * [$SIZE](#size)
* [**Array Modifiers**](#array-modifiers)
  * [EACH](#each)
  * [SLICE](#slice)
  * [ARRAY SORT](#array-sort)
  * [POSITION](#position)
* [**Evaluation Operators**](#evaluation-operators)
  * [$WHERE](#where)
  * [$EXPR](#expr)
  * [$REGEX](#regex)
  * [$TEXT](#text)
  * [$MOD](#mod)
* [**Aggregation**](#aggregation)

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
db.scores.find(
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

### $WHERE

Use to pass either a string containing a JavaScript expression or a full JavaScript function.

Starting in MongoDB 3.6, the **$expr** operator allows the use of aggregation expressions.

If you must create custom expressions, $function is preferred over **$where**.

### $EXPR

Allows the use of [aggregation](#aggregation) expressions. It compares fields from **the same** document.

``` javascript
db.monthlyBudget.find(
  {
expr: { $gt: [ "$spent", "$budget" ] }
  }
);
```

### $REGEX

Provides regular expression capabilities for pattern matching strings.

``` javascript
db.products.find({ sku: { $regex: /789$/ } });
```

``` javascript
db.products.find({ sku: { $regex: /^ABC/i } });
```

### $TEXT

Performs a text search on the content of the fields indexed with a text index.


```javascript
{
  text:
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

### $MOD

Select documents where the value of a field divided by a divisor has the specified remainder.

```javascript
db.inventory.find({ qty: { $mod: [4, 0] } });
```

## Aggregation

Aggregation expressions use **field path** to access fields in the input documents. To specify a field path, prefix the field name with a dollar sign $.
