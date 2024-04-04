---
cover: >-
  https://t3.ftcdn.net/jpg/02/50/95/20/360_F_250952030_oZAZskeVPjUTWlPSCKaz2gRQqEB5adCE.jpg
coverY: 0
---

# Data Modeling

## What is data modeling? <a href="#what-is-data-modeling" id="what-is-data-modeling"></a>

Data modeling is the process of creating a visual representation of the structure and relationship between different types of data. It involves analyzing and organizing the data and then creating one or more diagrams that show how different pieces of data relate to each other.

By organizing and categorizing unstructured information, data modeling allows you to make sense of the data, much like building a Lego castle from a giant collection of Lego bricks.&#x20;

The first step is to group the bricks by their features and properties, such as minifigures, building blocks, and decorations. These groups can then be divided into sub-groups based on additional features like color or size.

By categorizing the bricks in this way, we gain a better understanding of what we have to work with and how we can best use them to achieve our goal (building a cool castle 🏰).&#x20;

Additionally, we can identify relationships between different groups of bricks, such as how a minifigure is made up of various smaller parts like a head, torso, arms, and legs (see figure 1).

This process is similar to data modeling, where we identify entities and their attributes, and define the relationships between them. This helps us to understand the data we have and how to use it effectively to achieve our goals.

<figure><img src="https://docs.appfarm.io/~gitbook/image?url=https:%2F%2F29237295-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-MiLU-xcHu0eLZiTxcmZ%252Fuploads%252FUE2woP0sjqHPR51Bqf2A%252Fdata-model-minifigure.png%3Falt=media%26token=3843ef10-61f6-4f29-a04c-1439e5767289&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=4bc885f147d7f7974d6b1c3309dcc30f259410097b1d880bf4b819da1f32de49" alt=""><figcaption><p>Figure 1: Data model of a Lego minifigure’s composition.</p></figcaption></figure>

## Why does it matter? <a href="#why-does-it-matter" id="why-does-it-matter"></a>

A strong data model is essential for the success of any project and enables you to adapt to changes in ever-evolving application requirements. In addition, a well-designed data model is crucial for optimal performance. An inefficient model can hinder your ability to utilize your data at its full potential.

Data modeling is a critical process that enables you to make sense of large amounts of information. Without a good data model, it can be challenging to interpret what the data means or how to use it effectively.

To illustrate, imagine a complex Lego build with thousands of pieces. Without instructions it would be nearly impossible to assemble it as it was designed. Similarly, a data model serves as a guide for the data, clarifying how different pieces of information fit together and relate to each other.

#### A robust data model allows us to:

1. **Organize data logically:** By grouping similar pieces of information together and labeling them appropriately, we can easily locate and utilize the data we need.
2. **Analyze data effectively:** A well-designed data model simplifies the process of querying data to uncover insights and patterns that might be difficult to detect otherwise.
3. **Make informed decisions:** With a clear understanding of the data, we can make better decisions based on the insights we gain.

In short, data modeling is crucial because it transforms raw data into actionable and valuable information.

## Types of data models <a href="#types-of-data-models" id="types-of-data-models"></a>

There are different types of data models, such as conceptual, logical, and physical models, which can be used at different stages of the data modeling process.

### **Conceptual** 🎨 <a href="#conceptual" id="conceptual"></a>

In the first stage of data modeling, the conceptual data model is developed to identify the core processes that the application needs to support. This is often done in collaboration with business experts and stakeholders. The conceptual data model is an abstract representation of the relevant entities and their relationships, and it's used to communicate with people at any technical level.

### **Logical** 🧠 <a href="#logical" id="logical"></a>

The logical data model complements the conceptual data model and describes how the data will be structured. Based on the input and feedback from business experts and stakeholders, the data modeler extends the conceptual model with entity attributes and necessary relationships not defined in the conceptual model.

### **Physical** 🗄️ <a href="#physical" id="physical"></a>

The physical data model represents how the database stores the data model in a database management system. It specifies data types for the attributes as well as primary and foreign keys, constraints, and indexes, and is modeled by a developer.

Appfarm Create uses a hybrid of the conceptual and logical data model in the [Global Data Model](https://docs.appfarm.io/reference/data-model). The physical data model is handled automatically by the platform.

## Relational databases vs Document databases <a href="#relational-databases-vs.-document-databases" id="relational-databases-vs.-document-databases"></a>

There are two common approaches to storing data in a database, relational databases and document databases.

A relational database and a document database differ in their ability to store _structured_ and _unstructured_ data, respectively.&#x20;

In a relational database, data is organized in tables with pre-defined columns and rows that enforce relationships between entities.&#x20;

On the other hand, a document database stores unstructured data as JSON-like documents within collections, making it self-describing and free from a pre-defined structure. Therefore, there is no need for a defined table reference, as the data itself provides context and can vary in structure.

### Relational table structure <a href="#relational-table-structure" id="relational-table-structure"></a>

A relational database design for a data model for books and their authors could look like the tables below.

**Author**

| ID   | First Name | Last Name |
| ---- | ---------- | --------- |
| 1001 | J.R.R.     | Tolkien   |
| 1002 | Stephen    | King      |
| 1003 | Mark       | Twain     |
| 1004 | Virginia   | Woolf     |

**Book**

| ID   | Title                              | Genre     | Author |
| ---- | ---------------------------------- | --------- | ------ |
| 2001 | The Hobbit                         | Fantasy   | 1001   |
| 2002 | The Adventures of Huckleberry Finn | Fiction   | 1003   |
| 2003 | To the Lighthouse                  | Modernism | 1004   |
| 2004 | The Lord of the Rings              | Fantasy   | 1001   |

### Document structure <a href="#document-structure" id="document-structure"></a>

In contrast, document databases store data as documents, where each document represents a specific entity and contains all the information related to that entity in a nested data structure.&#x20;

This type of database is best suited for unstructured or semi-structured data, where the data does not have a clear and defined structure.

```json
[
  {
    "id": "1001",
    "firstName": "J.R.R.",
    "lastName": "Tolkien",
    "books": [
      { "id": "2001", "title": "The Hobbit", "genre": "Fantasy" },
      { "id": "2004", "title": "The Lord of the Rings", "genre": "Fantasy" }
    ]
  },
  {
    "id": "1002",
    "firstName": "Stephen",
    "lastName": "King",
    "books": []
  },
  {
    "id": "1003",
    "firstName": "Mark",
    "lastName": "Twain",
    "books": [
      {
        "id": "2002",
        "title": "The Adventures of Huckleberry Finn",
        "genre": "Fiction"
      }
    ]
  },
  {
    "id": "1004",
    "firstName": "Virginia",
    "lastName": "Woolf",
    "books": [
      {
        "id": "2003",
        "title": "To the Lighthouse",
        "genre": "Modernism"
      }
    ]
  }
]
```

Appfarm Create uses a document database, but adds a layer on top through the [Global data model](https://docs.appfarm.io/reference/data-model) to provide relational capabilities (referential integrity, foreign keys), delivering the best of both worlds.

## Key concepts in relational data modeling <a href="#key-concepts-in-relational-data-modeling" id="key-concepts-in-relational-data-modeling"></a>

Relational data modeling is a crucial process that involves a number of essential concepts for creating an efficient database design.

### Entity <a href="#entity" id="entity"></a>

> _An entity is a “thing” or object that we want to keep track of in our database. It can be a person, a place, a thing, or an event._

In Appfarm Create, an entity is described with an [object class](https://docs.appfarm.io/reference/data-model/object-classes).

### Attribute <a href="#attribute" id="attribute"></a>

> _An attribute is a characteristic of an entity. It describes some aspect of the entity, such as its name, age, or address._

In Appfarm Create, an attribute is an [object class property](https://docs.appfarm.io/reference/data-model/object-classes#object-class-properties).

### Data type <a href="#data-type" id="data-type"></a>

> _A data type defines the type of data that can be stored in a property. Common data types include text, number, date, and boolean._

In Appfarm Create, the [data type](https://docs.appfarm.io/reference/data-model/object-classes#data-types) is a part of the definition of an [object class property](https://docs.appfarm.io/reference/data-model/object-classes#object-class-properties). Simple data types such as boolean, string, integer, and datetime are supported, as well as [enums](https://docs.appfarm.io/reference/data-model/enumerated-types) and references to other object classes.

### Relationships <a href="#relationships" id="relationships"></a>

> _A relationship describes how entities are related to each other. For example, a customer can have many orders, and each order belongs to only one customer. Closely connected to this concept is cardinality, which describes the number of instances of one entity that can be related to another entity. This is usually expressed with the terms one or many._

#### **One-to-one**

A one-to-one relationship is probably the least common relationship. It defines that there can only be one record in table A associated with a specific record in table B. This can be interpreted as a bijective function from a mathematics perspective.

A real-world example for this scenario is the connection between a human and its brain. A human can only have one brain and a brain can only be connected to one human, and this relationship is constant. There’s (at least for now) no possibility for someone to have their brain replaced or duplicated.

Another, more relevant, example is the connection between a user and their user settings. One user has their set of settings, and a set of settings belongs to exactly one user, as seen in figure 2.

In data modeling this relationship is not that commonly used, and is often modeled as an ordinary one-to-many relationship instead. This provides greater flexibility should the requirements for either brains or user settings change in the future.

<figure><img src="https://docs.appfarm.io/~gitbook/image?url=https:%2F%2F29237295-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-MiLU-xcHu0eLZiTxcmZ%252Fuploads%252F5SV2gFaNiSb5MZgX9C2z%252Fdata-model-one-to-one.png%3Falt=media%26token=3dccd203-5ba2-4457-83a7-e70e513b262a&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=84223dd75c13a834b61bcfd51731cb8bff2c1a9a41e25626a96b671b3d91ace2" alt=""><figcaption><p>Figure 2: One-to-one relationship as a data model.</p></figcaption></figure>

#### **One-to-many**

A one-to-many relationship is very common. It defines that a record in table A can only be associated with a specific record in table B, while a record in table B can have multiple records in table A associated with it.

As an example, when a customer places an order in an online store, they might want to purchase different items at the same time.&#x20;

To avoid requiring one order for each item, you can create an order object holding the details of that particular order (including the customer, the state of the order, the order date, etc.), while each item is represented as a single order line with a reference to the order object.

A one-to-many relationship is illustrated in figure 3 with a data model consisting of `Order` and `Order Line`. An order line can be connected to only one order through its property `Order`**.** An order can have any number of order lines referencing it.

<figure><img src="https://docs.appfarm.io/~gitbook/image?url=https:%2F%2F29237295-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-MiLU-xcHu0eLZiTxcmZ%252Fuploads%252FdY50g9xXAb23J5JqlTiV%252Fdata-model-one-to-many.png%3Falt=media%26token=0d8efd34-1425-466d-a476-181c507b5cbf&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=fa8176cb405eef3112cdd85e9874f5cb4edf1322e88f8fa7241b21ae23481c35" alt=""><figcaption><p>Figure 3: One-to-many relationship as a data model</p></figcaption></figure>

#### **Many-to-many**

The last type of relationship is the many-to-many-relationship. It defines that multiple records in table A can be connected to multiple records in table B.

To illustrate this visually, imagine drawing lines between books and authors. Each book may have lines connecting it to multiple authors, and each author may have lines connecting them to multiple books.

This is illustrated in figure 4 with a data model consisting of `Book`, `Author` and `Written By`. A `Book` can be written by multiple authors and an `Author` can write multiple books.&#x20;

This is modeled as `Written By` which combines these two object classes and represents the relationship between books and authors.&#x20;

This object class can also be extended to contain more metadata about the relationship (e.g. number of pages that a particular author has written of a particular book).

<figure><img src="https://docs.appfarm.io/~gitbook/image?url=https:%2F%2F29237295-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-MiLU-xcHu0eLZiTxcmZ%252Fuploads%252FxcVfRHPp9lKb7dFUjomT%252Fdata-model-many-to-many.png%3Falt=media%26token=8229699f-88bc-44d0-9d10-067671f5366e&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=8cd3df81dc21127c9f0167dcbfe8f1236e29daa0e6928914054677e8be35f2c8" alt=""><figcaption><p>Figure 4: Many-to-many relationship as a data model</p></figcaption></figure>

You may find [this guide](https://docs.appfarm.io/how-to/data-modeling/many-to-many-relationships) useful if you want practical examples of how to use many-to-many relationships in Appfarm Create.

### Primary key <a href="#primary-key" id="primary-key"></a>

> _A primary key is a unique identifier for an entity in a table. It is used to ensure that each record in the table can be uniquely identified._

In Appfarm Create, the primary key is stored in the [object class property](https://docs.appfarm.io/reference/data-model/object-classes#built-in-properties) **ID**.

### Foreign key <a href="#foreign-key" id="foreign-key"></a>

> _A foreign key is a field in a table that refers to the primary key of another table. It is used to establish a relationship between the two tables._

In Appfarm Create, the foreign key is represented as an [object class property](https://docs.appfarm.io/reference/data-model/object-classes#object-class-properties) referencing another object class.&#x20;

In the diagram view of the [data model designer](https://docs.appfarm.io/reference/data-model#data-model-designer) it is visualized with an arrow pointing from the property to the referenced object class (`Book` and `Author` in the `Written By` object class are foreign keys in figure 4).&#x20;

The object that has the property of a referenced object class will store the ID of the referenced object as seen in the `Written By` table below, which references both the `Book` object class and the `Author` object class.

**Book**

| ID \[primary key]        | Title             | Number of Pages |
| ------------------------ | ----------------- | --------------- |
| 6376344c01ed7ce7fb4f1967 | Data Modeling 101 | 1337            |

**Author**

| ID \[primary key]        | First Name | Last Name |
| ------------------------ | ---------- | --------- |
| 6376347f4a2bd917bc5128f2 | App        | Farmer    |

**Written By**

| ID \[primary key]        | Book \[foreign key]      | Author \[foreign key]    |
| ------------------------ | ------------------------ | ------------------------ |
| 637f9382462b2a1d0e6b823a | 6376344c01ed7ce7fb4f1967 | 6376347f4a2bd917bc5128f2 |

**Normalization**

> _Normalization is the process of organizing data in a database to minimize redundancy and ensure data integrity. It involves breaking down tables into smaller, more specific tables and establishing relationships between them._

Please read [this article](https://docs.appfarm.io/appcademy/background/databases/database-normalization) if you want to learn more about the concepts of database normalization.

By understanding these key concepts, you can create a well-structured and organized relational database that accurately represents the data you want to store, allowing you to build elegant and efficient apps.
