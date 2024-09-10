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

- Key-value stores: A key-value database is a nonrelational database that retrieves records using a key that uniquely identifies each record. 


- Document stores: A document store is a specialized key-value store. In this context, a document is a nested object; we can usually think of each document as a JSON object for practical purposes. Documents are stored in collec‐ tions and retrieved by key.


- Wide-column: A wide-column database is optimized for storing massive amounts of data with high transaction rates and extremely low latency. These databases can scale to extremely high write rates and vast amounts of data. Specifically, wide- column databases can support petabytes of data, millions of requests per second, and sub-10ms latency.

Data engineers must be aware of the operational characteristics of the wide-column databases they work with to set up a suitable configuration, design the schema, and choose an appropriate row key to optimize performance and avoid common operational issues.


- Graph databases: Graph databases explicitly store data with a mathematical graph structure (as a set of nodes and edges). `Neo4j` has proven extremely popular, while `Amazon`, `Oracle`, and other vendors offer their graph database products. Roughly speaking, graph databases are a good fit when you want to analyze the connectivity between elements.

Graph databases are designed for precisely this type of query. Their data structures allow for queries based on the connectivity between elements; graph databases are indicated when we care about understanding complex traversals between elements.

Depending on the underlying graph database engine, graph databases utilize specialized query languages such as SPARQL, Resource Description Framework (RDF), Graph Query Language (GQL), and Cypher.


- Search: A search database is a nonrelational database used to search your data’s complex and straightforward semantic and structural characteristics. Two prominent use cases exist for a search database: text search and log analysis. 

Text search involves searching a body of text for keywords or phrases, matching on exact, fuzzy, or semantically similar matches.

Log analysis is typically used for anomaly detection, real-time monitoring, security analytics, and operational analyt‐ ics. Queries can be optimized and sped up with the use of indexes.

Search databases are popular for fast search and retrieval and can be found in various applications; an ecommerce site may power its product search using a search database. As a data engineer, you might be expected to bring data from a search database (such as Elasticsearch, Apache Solr or Lucene, or Algolia) into downstream KPI reports or something similar.


- Time series: A time series is a series of values organized by time. Any events that are recorded over time—either regularly or sporadically—are time-series data. A time-series database is optimized for retrieving and statistical processing of time-series data.

The schema for a time series typically contains a timestamp and a small set of fields. Because the data is time-dependent, the data is ordered by the timestamp.


### APIs

APIs are now a standard and pervasive way of exchanging data in the cloud, for SaaS platforms, and between internal company systems.


#### REST

REST stands for `representational state transfer`. REST is built around HTTP verbs, such as GET and PUT. One of the principal ideas of REST is that interactions are stateless.

At virtually any large company, data engineers will need to deal with the problem of writing and maintaining custom code to pull data from APIs, which requires understanding the structure of the data as provided, developing appropriate data-extraction code, and determining a suitable data synchronization strategy.


#### GraphQL

GraphQL was created at Facebook as a query language for application data and an alternative to generic REST APIs. Whereas REST APIs generally restrict your queries to a specific data model, GraphQL opens up the possibility of retrieving multiple data models in a single request. This allows for more flexible and expressive queries than with REST. GraphQL is built around JSON and returns data in a shape resembling the JSON query.

#### Webhooks

Webhooks are a simple event-based data-transmission pattern. The data source can be an application backend, a web page, or a mobile app. When specified events happen in the source system, this triggers a call to an HTTP endpoint hosted by the data consumer. 

#### RPC and gRPC

A remote procedure call (RPC) is commonly used in distributed computing. It allows you to run a procedure on a remote system. gRPC emphasizes the efficient bidirectional exchange of data over HTTP/2. Efficiency refers to aspects such as CPU utilization, power consumption, battery life, and band‐ width. Like GraphQL, gRPC imposes much more specific technical standards than REST, thus allowing the use of common client libraries and allowing engineers to develop a skill set that will apply to any gRPC interaction code.


### Data Sharing

The core concept of cloud data sharing is that a multitenant system supports security policies for sharing data among tenants. Concretely, any public cloud object storage system with a fine-grained permission system can be a platform for data sharing.

Data sharing can also streamline data pipelines within an organization. Data sharing allows units of an organization to manage their data and selectively share it with other units while still allowing individual units to manage their compute and query costs separately, facilitating data decentralization.

### Third-Party Data Sources

Direct third-party data access is commonly done via APIs, through data sharing on a cloud platform, or through data download. APIs often provide deep integration capabilities, allowing customers to pull and push data.

### Message Queues and Event-Streaming Platforms

Event-driven architectures are pervasive in software applications and are poised to grow their popularity even further. First, message queues and event-streaming platforms—critical layers in event-driven architectures—are easier to set up and manage in a cloud environment. 

Second, the rise of data apps—applications that directly integrate real-time analytics—are growing from strength to strength. Event-driven architectures are ideal in this setting because events can both trigger work in the application and feed near real-time analytics.

- #### Message queues

A message queue is a mechanism to asynchronously send data (usually as small individual messages, in the kilobytes) between discrete systems using a publish and subscribe model. Data is published to a message queue and is delivered to one or more subscribers. The subscriber acknowledges receipt of the message, removing it from the queue.

![Message Queue](/chap5/images.png)

Message queues allow applications and systems to be decoupled from each other and are widely used in microservices architectures. The message queue buffers messages to handle transient load spikes and makes messages durable through a distributed architecture with replication.

Message queues are a critical ingredient for decoupled microservices and event- driven architectures. Some things to keep in mind with message queues are frequency of delivery, message ordering, and scalability.

- Message ordering and delivery: The order in which messages are created, sent, and received can significantly impact downstream subscribers. Message queues often apply a fuzzy notion of order and first in, first out (FIFO). Strict FIFO means that if message A is ingested before message B, message A will always be delivered before message B. In practice, messages might be published and received out of order, especially in highly distributed message systems.

In general, don’t assume that your messages will be delivered in order unless your message queue technology guarantees it. You typically need to design for out-of- order message delivery.

- Delivery frequency: Messages can be sent exactly once or at least once. If a message is sent exactly once, then after the subscriber acknowledges the message, the message disappears and won’t be delivered again. 

Ideally, systems should be idempotent. In an idempotent system, the outcome of processing a message once is identical to the outcome of processing it multiple times.

- Scalability: The most popular message queues utilized in event-driven applications are horizontally scalable, running across multiple servers. This allows these queues to scale up and down dynamically, buffer messages when systems fall behind, and durably store messages for resilience against failure. 

- Event-streaming platforms: In some ways, an event-streaming platform is a continuation of a message queue in that messages are passed from producers to consumers. The big difference between messages and streams is that a message queue is primarily used to route messages with certain delivery guarantees. 

An event is “something that happened, typically a change in the state of something.” An event has the following features: a key, a value, and a timestamp. Multiple key-value timestamps might be contained in a single event. 

For example, an event for an e-commerce order might look like this:

```JSON
{
"Key":"Order # 12345",
"Value":"SKU 123, purchase price of $100", "Timestamp":"2023-01-02 06:01:00"
}
```

    Characteristics of an event-streaming platform that you should be aware of as a data engineer.

    - `Topics`: In an event-streaming platform, a producer streams events to a topic, a collection of related events. A topic might contain fraud alerts, customer orders, or temperature readings from IoT devices. 

    - `Stream partitions`: Stream partitions are subdivisions of a stream into multiple streams. A good analogy is a multilane freeway. Having multiple lanes allows for parallelism and higher throughput. Messages are distributed across partitions by partition key. Messages with the same partition key will always end up in the same partition.

    A key concern with stream partitioning is ensuring that your partition key does not generate hotspotting—a disproportionate number of messages delivered to one partition.

    - `Fault tolerance and resilience`: Event-streaming platforms are typically distributed sys‐ tems, with streams stored on various nodes. If a node goes down, another node replaces it, and the stream is still accessible. 

    This fault tolerance and resilience make streaming platforms a good choice when you need a system that can reliably produce, store, and ingest event data.


You, as the data engineer are often at the mercy of the stakeholder’s ability to follow correct software engineering, database management, and development practices. Ideally, the stakeholders are doing DevOps and working in an agile manner. 


### Undercurrents and Their Impact on Source Systems

- Security
    - Do you trust the source system? Always be sure to trust but verify that the source system is legitimate. You don’t want to be on the receiving end of data from a malicious actor.
    - Is the source system architected so data is secure and encrypted, both with data at rest and while data is transmitted?
    - Keep passwords, tokens, and credentials to the source system securely locked away.

- Data Management
Data management of source systems is challenging for data engineers. In most cases, you will have only peripheral control—if any control at all—over source systems and the data they produce. To the extent possible, you should understand the way data is managed in source systems since this will directly influence how you ingest, store, and transform the data.

areas to consider
    - Data governance
    - Data Quality
    - Schema
    - Master data management
    - Privacy and ethics
    - Regulatory

- DataOps
Operational excellence—DevOps, DataOps, MLOps, XOps—should extend up and down the entire stack and support the data engineering and lifecycle. While this is ideal, it’s often not fully realized.

A few DataOps considerations are as follows:
    - Automation
    - Observability
    - incident response

- Data Architecture
Similar to data management, unless you’re involved in the design and maintenance of the source system architecture, you’ll have little impact on the upstream source system architecture. You should also understand how the upstream architecture is designed and its strengths and weaknesses. 

Here are some things to consider regarding source system architectures:
    - Reliability
    - Durability
    - Availability
    - People


- Orchestration
When orchestrating within your data engineering workflow, you’ll primarily be con‐ cerned with making sure your orchestration can access the source system, which requires the correct network access, authentication, and authorization.

Here are some things to think about concerning orchestration for source systems:
    - Cadence and frequency
    - Common frameworks
    - 


- Software Engineering
As the data landscape shifts to tools that simplify and automate access to source systems, you’ll likely need to write code. Here are a few considerations when writing code to access a source system:

    - Networking
    - Authentication and authorization
    - Access patterns
    - Orchestration
    - Parallelization
    - Deployment


Be aware that the integration between data engineers and source system teams is growing. One example is reverse ETL, which has long lived in the shadows but has recently risen into prominence. We also discussed that the event-streaming platform could serve a role in event-driven architectures and analytics; a source system can also be a data engineering system. Build shared systems where it makes sense to do so.
Look for opportunities to build user-facing data products. Talk to application teams about analytics they would like to present to their users or places where ML could improve the user experience. Make application teams stakeholders in data engineer‐ ing, and find ways to share your successes.




































