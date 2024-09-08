## Data Generation in Source Systems


Source systems generate the data for the rest of the data engineering lifecycle.

![Data engineering lifecycle](/chap5/1_1t2zah1TG5_FCu0nTwkKtg.png)

### Sources of Data: How Is Data Created?

- Analog data creation occurs in the real world, such as vocal speech, sign language, writing on paper, or playing an instrument.

- Digital data is either created by converting analog data to digital form or is the native product of a digital system. Data is everywhere in the world around us. We capture data from IoT devices, credit card terminals, telescope sensors, stock trades, and more.


### Source Systems: Main Ideas

- `Files and Unstructured Data`

A file is a sequence of bytes, typically stored on a disk. Applications often write data to files. Files may store local parameters, events, logs, images, and audio. The main types of source file formats you’ll run into as a data engineer—files that originate either manually or as an output from a source system process—are Excel, CSV, TXT, JSON, and XML. These files have their quirks and can be structured (Excel, CSV), semistructured (JSON, XML, CSV), or unstructured (TXT, CSV).

- `APIs`

Application programming interfaces (APIs) are a standard way of exchanging data between systems.  

- `Application Databases (OLTP Systems)`

An application database stores the state of an application. Typically, an application database is an online transaction processing (OLTP) system— a database that reads and writes individual data records at a high rate. 


### ACID transactions

Support for atomic transactions is one of a critical set of database characteristics known together as ACID, this stands for atomicity, consistency, isolation, and durability. 

- Consistency means that any database read will return the last written version of the retrieved item. 

- Isolation entails that if two updates are in flight concurrently for the same thing, the end database state will be consistent with the sequential execution of these updates in the order they were submitted. 

- Durability indicates that committed data will never be lost, even in the event of power loss.

- Atomic transactions: An atomic transaction is a set of several changes that are committed as a unit.


### Online Analytical Processing System

In contrast to an OLTP system, an online analytical processing (OLAP) system is built to run large analytics queries and is typically inefficient at handling lookups of individual records. Note that we’re using the term OLAP to refer to any database system that supports high-scale interactive analytics queries; we are not limiting ourselves to systems that support OLAP cubes (multidimensional arrays of data). 

The online part of OLAP implies that the system constantly listens for incoming queries, making OLAP sys‐ tems suitable for interactive analytics.


### Change Data Capture

Change data capture (CDC) is a method for extracting each change event (insert, update, delete) that occurs in a database. CDC is frequently leveraged to replicate between databases in near real time or create an event stream for downstream processing.


### Logs

A log captures information about events that occur in systems. For example, a log may capture traffic and usage patterns on a web server. Your desktop computer’s operating system (Windows, macOS, Linux) logs events as the system boots and when applications start or crash, for example.

- `Database Logs`

Database logs are essential enough that they deserve more detailed coverage. Write- ahead logs—typically, binary files stored in a specific database-native format—play a crucial role in database guarantees and recoverability.


### CRUD

CRUD, which stands for create, read, update, and delete, is a transactional pattern commonly used in programming and represents the four basic operations of persis‐ tent storage. CRUD is the most common pattern for storing application state in a database. A basic tenet of CRUD is that data must be created before being used. After the data has been created, the data can be read and updated. Finally, the data may need to be destroyed. CRUD guarantees these four operations will occur on data, regardless of its storage.


### Messages and Streams

Related to event-driven architecture, two terms that you’ll often see used interchange‐ ably are message queue and streaming platform, but a subtle but essential difference exists between the two. A message is raw data communicated across two or more systems. Messages are discrete and singular signals in an event-driven system.

A stream is an append-only log of event records. Streams are ingested and stored in event-streaming platforms. 



### Source System Practical Details

These details are critical knowledge for working data engineers. We suggest that you study this information as baseline knowledge but read extensively to stay abreast of ongoing developments.

- `Databases`

We’ll look at common source system database technologies that you’ll encounter as a data engineer and high-level considerations for working with these systems. There are as many types of databases as there are use cases for data.


    ### Major considerations for understanding database technologies
    - Database management system
    - Lookups
    - Query optimizer
    - Scaling and distribution
    - Modeling patterns
    - CRUD
    - Consistency
    

    ### Relational databases

    A relational database management system (RDBMS) is one of the most common application backends. Data is stored in a table of relations (rows), and each relation contains multiple fields (columns). RDBMS stores and retrieves data at a row level. Tables are typically indexed by a primary key, a unique field for each row in the table. Tables can also have various foreign keys—fields with values connected with the values of primary keys in other tables, facilitating joins, and allowing for complex schemas that spread data across multiple tables.

    Normalization is a strategy for ensuring that data in records is not duplicated in multiple places, thus avoiding the need to update states in multiple locations at once and preventing inconsistencies. 

    RDBMS systems are typically ACID compliant. Combining a normalized schema, ACID compliance, and support for high transaction rates makes relational database systems ideal for storing rapidly changing application states. The challenge for data engineers is to determine how to capture state information over time.


    ### Nonrelational databases: NoSQL

    NoSQL, which stands for not only SQL, refers to a whole class of databases that abandon the relational paradigm. On the one hand, dropping relational constraints can improve performance, scala‐ bility, and schema flexibility. But as always in architecture, trade-offs exist. NoSQL databases also typically abandon various RDBMS characteristics, such as strong con‐ sistency, joins, or a fixed schema. 

    There are far too many NoSQL databases to cover exhaustively, we consider the following database types: 
    `key-value`, 
    `document`, 
    `wide-column`, 
    `graph`, 
    `search`, and 
    `time series`. 

    These databases are all wildly popular and enjoy widespread adoption. A data engineer should understand these types of databases, including usage considerations, the structure of the data they store, and how to leverage each in the data engineering lifecycle.

    - Key-value stores.
    - Document stores
    - Wide-column.
    - Graph databases.
    - Search.
    - Time series.
    





































