## Document Databases

A document database (also known as a document-oriented database or a document store) is a database that stores information in documents.

Document databases offer a variety of advantages, including:

- An intuitive data model that is fast and easy for developers to work with
- A flexible schema that allows for the data model to evolve as application needs change
- The ability to horizontally scale out.

Document databases are considered to be non-relational (or NoSQL) databases. Instead of storing data in fixed rows and columns, document databases use flexible documents. Document databases are the most popular alternative to tabular, relational databases.

### What are documents?

A document is a record in a document database. A document typically stores information about one object and any of its related metadata. Documents store data in field-value pairs. The values can be a variety of types and structures, including strings, numbers, dates, arrays, or objects. Documents can be stored in formats like JSON, BSON, and XML.

Below is a JSON document that stores information about a user named Tom.

```JSON
{
     "_id": 1,
     "first_name": "Tom",
     "email": "tom@example.com",
     "cell": "765-555-5555",
     "likes": [
        "fashion",
        "spas",
        "shopping"
     ],
     "businesses": [
        {
           "name": "Entertainment 1080",
           "partner": "Jean",
           "status": "Bankrupt",
           "date_founded": {
              "$date": "2012-05-19T04:00:00Z"
           }
        },
        {
           "name": "Swag for Tweens",
           "date_founded": {
              "$date": "2012-11-01T04:00:00Z"
           }
        }
     ]
  }
```

### Collections

A collection is a group of documents. Collections typically store documents that have similar contents. Not all documents in a collection are required to have the same fields, because document databases have flexible schemas. Note that some document databases provide schema validation, so the schema can optionally be locked down when needed.

### CRUD operations

Document databases typically have an API or query language that allows developers to execute the CRUD (create, read, update, and delete) operations.

- `Create:` Documents can be created in the database. Each document has a unique identifier.
- `Read:` Documents can be read from the database. The API or query language allows developers to query for documents using their unique identifiers or field values. Indexes can be added to the database in order to increase read performance.
- `Update:` Existing documents can be updated — either in whole or in part.
- `Delete:` Documents can be deleted from the database.


### What are the key features of document databases?

Document databases have the following key features:

- `Document model:` Data is stored in documents (unlike other databases that store data in structures like tables or graphs). Documents map to objects in most popular programming languages, which allows developers to rapidly develop their applications.

- `Flexible schema:` Document databases have flexible schemas, meaning that not all documents in a collection need to have the same fields. Note that some document databases support schema validation, so the schema can be optionally locked down.

- `Distributed and resilient:` Document databases are distributed, which allows for horizontal scaling (typically cheaper than vertical scaling) and data distribution. Document databases provide resiliency through replication.

- `Querying through an API or query language:` Document databases have an API or query language that allows developers to execute the CRUD operations on the database. Developers have the ability to query for documents based on unique identifiers or field values.


### What makes document databases different from relational databases?

- The intuitiveness of the data model
- The ubiquity of JSON documents
- The flexibility of the schema


### How much easier are documents to work with than tables?

Developers commonly find working with data in documents to be easier and more intuitive than working with data in tables. Documents map to data structures in most popular programming languages. Developers don't have to worry about manually splitting related data across multiple tables when storing it or joining it back together when retrieving it.


### What are the relationships between document databases and other databases?

The document model is a superset of other data models, including key-value pairs, relational, objects, graph, and geospatial.

- Key-value pairs can be modeled with fields and values in a document. Any field in a document can be indexed, providing developers with additional flexibility in querying the data.

- Relational data can be modeled differently (and some would argue more intuitively) by keeping related data together in a single document using embedded documents and arrays. 

- Documents map to objects in most popular programming languages.

- Graph nodes and/or edges can be modeled as documents. Edges can also be modeled through database references. Graph queries can be run using operations like `$graphLookup`.

- Geospatial data can be modeled as arrays in documents.


### What are the strengths and weaknesses of document databases?

Document databases have many strengths:

- The document model is ubiquitous, intuitive, and enables rapid software development.

- The flexible schema allows for the data model to change as an application's requirements change.

- Document databases have rich APIs and query languages that allow developers to easily interact with their data.

- Document databases are distributed (allowing for horizontal scaling as well as global data distribution) and resilient.


### What are the use cases for document databases?

Document databases are general-purpose databases that serve a variety of use cases for both transactional and analytical applications:

- Single view or data hub
- Customer data management and personalization
- Internet of Things (IoT) and time-series data
- Product catalogs and content management
- Payment processing
- Mobile apps
- Mainframe offload
- Operational analytics
- Real-time analytics


Document databases utilize the intuitive, flexible document data model to store data. Document databases are general-purpose databases that can be used for a variety of use cases across industries.









































