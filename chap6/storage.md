## Storage

Data gets stored many times as it moves through the lifecycle. To paraphrase an old saying, it’s storage all the way down. Whether data is needed seconds, minutes, days, months, or years later, it must persist in storage until systems are ready to consume it for further processing and transmission. 


![Storage aspect of the Data engineering lifecycle](/chap5/1_1t2zah1TG5_FCu0nTwkKtg.png)


The raw ingredients that compose storage systems, including hard drives, solid state drives, and system memory. It’s essential to understand the basic characteristics of physical storage technologies to assess the trade-offs inherent in any storage architecture. The serialization and compression, key software elements of practical storage, and a deeper technical discussion of serialization and compression.

![Raw Ingredients of Storage systems](/chap6/1_Vp-VFmT43kjwe50aumA9-g.webp)


### Raw Ingredients of Data Storage

Storage is so common that it’s easy to take it for granted. We’re often surprised by the number of software and data engineers who use storage every day but have little idea how it works behind the scenes or the trade-offs inherent in various storage media.

In most data architectures, data frequently passes through magnetic storage, SSDs, and memory as it works its way through the various processing phases of a data pipeline. Data storage and query systems generally follow complex recipes involving distributed systems, numerous services, and multiple hardware storage layers. These systems require the right raw ingredients to function correctly.

some of the raw ingredients of data storage: `disk drives`, `memory`, `networking` and `CPU`, `serialization`, `compression`, and `caching`.

- `Magnetic Disk Drive`: Magnetic disk drives have been around for ages. They still form the backbone of bulk data storage systems because they are significantly cheaper than SSDs per gigabyte of stored data.

- `Solid-State Drive`: Solid-state drives (SSDs) store data as charges in flash memory cells. SSDs eliminate the mechanical components of magnetic drives; the data is read by purely electronic means. SSDs can look up random data in less than 0.1 ms (100 microseconds).  Because of these exceptional performance characteristics, SSDs have revolutionized transactional databases and are the accepted standard for commercial deployments of OLTP systems. SSDs allow relational databases such as PostgreSQL, MySQL, and SQL Server to handle thousands of transactions per second.

- `Random Access Memory`: RAM is used in various storage and processing systems and can be used for caching, data processing, or indexes. Several databases treat RAM as a primary storage layer, allowing ultra-fast read and write performance. In these applications, data engineers must always keep in mind the volatility of RAM.

- `Networking and CPU`: CPUs handle the details of servicing requests, aggregating reads, and distributing writes. Storage becomes a web application with an API, backend service components, and load balancing.  Data engineers need to understand how networking will affect the systems they build and use. Engineers constantly balance the durability and availability achieved by spreading out data geographically versus the performance and cost benefits of keeping storage in a small geographic area and close to data consumers or writers.

- `Serialization`: Serialization is another raw storage ingredient and a critical element of database design. The decisions around serialization will inform how well queries perform across a network, CPU overhead, query latency, and more. Designing a data lake, for example, involves choosing a base storage system (e.g., Amazon S3) and standards for serialization that balance interoperability with performance considerations.

Serialization is the process of flattening and packing data into a standard format that a reader will be able to decode. Serialization formats provide a standard of data exchange. We might encode data in a row-based manner as an XML, JSON, or CSV file and pass it to another user who can then decode it using a standard library. A serialization algorithm has logic for handling types, imposes rules on data structure, and allows exchange between programming languages and CPUs. The serialization algorithm also has rules for handling exceptions. 

Row-oriented relational databases organize data as rows on disk to support speedy lookups and in-place updates. Columnar databases organize data into column files to optimize for highly efficient compression and support fast scans of large data volumes. Each serialization choice comes with a set of trade-offs, and data engineers tune these choices to optimize performance to requirements.

- `Compression`: Compression is another critical component of storage engineering. On a basic level, compression makes data smaller, but compression algorithms interact with other details of storage systems in complex ways.

- `Caching`: The core idea of caching is to store frequently or recently accessed data in a fast access layer. The faster the cache, the higher the cost and the less storage space available. Less frequently accessed data is stored in cheaper, slower storage. Caches are critical for data serving, processing, and transformation.


### Data Storage Systems

Storage systems exist at a level of abstraction above raw ingredients. For example, magnetic disks are a raw storage ingredient, while major cloud object storage platforms and HDFS are storage systems that utilize magnetic disks. Still higher levels of storage abstraction exist, such as data lakes and lakehouses. 


`Single Machine Versus Distributed Storage`
As data storage and access patterns become more complex and outgrow the useful‐ ness of a single server, distributing data to more than one server becomes necessary. Data can be stored on multiple servers, known as distributed storage.

A distributed storage system is infrastructure that can split data across multiple physical servers, and often across more than one data center. It typically takes the form of a cluster of storage units, with a mechanism for data synchronization and coordination between cluster nodes.

Distributed storage coordinates the activities of multiple servers to store, retrieve, and process data faster and at a larger scale, all while providing redundancy in case a server becomes unavailable. Distributed storage is common in architectures where you want built-in redundancy and scalability for large amounts of data.


`Eventual Versus Strong Consistency`
Eventual consistency is a common trade-off in large-scale, distributed systems. If you want to scale horizontally (across multiple nodes) to process data in high volumes, then eventually, consistency is often the price you’ll pay. Eventual consistency allows you to retrieve data quickly without verifying that you have the latest version across all nodes.

The opposite of eventual consistency is strong consistency. With strong consistency, the distributed database ensures that writes to any node are first distributed with a consensus and that any reads against the database return consistent values. You’ll use strong consistency when you can tolerate higher query latency and require correct data every time you read from the database.

A data engineer might need to negotiate consistency requirements with other technical and business stakeholders. Note that this is both a technology and organizational problem; ensure that you have gathered requirements from your stakeholders and choose your technologies appropriately.


### File Storage

A file is a data entity with specific read, write, and reference characteristics used by software and operating systems. File storage systems organize files into a directory tree. 


### Block Storage

Fundamentally, block storage is the type of raw storage provided by SSDs and mag‐ netic disks. In the cloud, virtualized block storage is the standard for VMs. These block storage abstractions allow fine control of storage size, scalability, and data durability beyond that offered by raw disks.

    - Block storage applications
    - RAID: RAID stands for redundant array of independent disks.
    - Storage area network
    - Cloud virtualized block storage
    - Local instance volumes


### Object Storage

Object storage contains objects of all shapes and sizes. The term object storage is somewhat confusing because object has several meanings in computer science. In this context, we’re talking about a specialized file-like construct. It could be any type of file—TXT, CSV, JSON, images, videos, or audio.

Object stores have grown in importance and popularity with the rise of big data and the cloud. Amazon S3, Azure Blob Storage, and Google Cloud Storage (GCS) are widely used object stores. In addition, many cloud data warehouses (and a growing number of databases) utilize object storage as their storage layer, and cloud data lakes generally sit on object stores.


### Cache and Memory-Based Storage Systems

RAM-based storage systems are generally focused on caching applications, presenting data for quick access and high bandwidth. Data should generally be written to a more durable medium for retention purposes.


- Memcached
Memcached is a general-purpose distributed memory-caching system. It is often used to speed up dynamic database-driven websites by caching data and objects in RAM to reduce the number of times an external data source must be read. 

Memcached uses simple data structures, supporting either string or integer types. Memcached can deliver results with very low latency while also taking the load off backend systems.


### Streaming Storage

Streaming data has different storage requirements than nonstreaming data. In the case of message queues, stored data is temporal and expected to disappear after a certain duration. However, distributed, scalable streaming frameworks like Apache Kafka now allow extremely long-duration streaming data retention. Kafka supports indefinite data retention by pushing old, infrequently accessed messages down to object storage. Kafka competitors (including Amazon Kinesis, Apache Pulsar, and Google Cloud Pub/Sub) also support long data retention.


### Indexes, Partitioning, and Clustering

Indexes provide a map of the table for particular fields and allow extremely fast lookup of individual records. Without indexes, a database would need to scan an entire table to find the records satisfying a WHERE condition.

In most RDBMSs, indexes are used for primary table keys (allowing unique identifi‐ cation of rows) and foreign keys (allowing joins with other tables). Indexes can also be applied to other columns to serve the needs of specific applications. Using indexes, an RDBMS can look up and update thousands of rows per second.


### From indexes to partitions and clustering

While columnar databases allow for fast scan speeds, it’s still helpful to reduce the amount of data scanned as much as possible. In addition to scanning only data in columns relevant to a query, we can partition a table into multiple subtables by splitting it on a field. It is quite common in analytics and data science use cases to scan over a time range, so date and time-based partitioning is extremely common. Columnar databases generally support a variety of other partition schemes as well.

Clusters allow finer-grained organization of data within partitions. A clustering scheme applied within a columnar database sorts data by one or a few fields, colocating similar values. This improves performance for filtering, sorting, and joining these values.



## Data Engineering Storage Abstractions

The storage abstraction you require as a data engineer boils down to a few key considerations:

- Purpose and use case: You must first identify the purpose of storing the data. What is it used for?
- Update patterns: Is the abstraction optimized for bulk updates, streaming inserts, or upserts?
- Cost: What are the direct and indirect financial costs? The time to value? The opportunity costs?
- Separate storage and compute: The trend is toward separating storage and compute, but most systems hybridize separation and colocation.

### The Data Warehouse

Data warehouses are a standard OLAP data architecture. The term data warehouse refers to technology platforms (e.g., Google BigQuery and Teradata), an architecture for data centralization, and an organizational pattern within a company. 

A data warehouse is an enterprise system used for the analysis and reporting of structured and semi-structured data from multiple sources, such as point-of-sale transactions, marketing automation, customer relationship management, and more. 


### The Data Lake

The data lake was originally conceived as a massive store where data was retained in raw, unprocessed form. A data lake is a centralized repository designed to store, process, and secure large amounts of structured, semistructured, and unstructured data. It can store data in its native format and process any variety of it, ignoring size limits.

### The Data Lakehouse

The data lakehouse is an architecture that combines aspects of the data warehouse and the data lake. As it is generally conceived, the lakehouse stores data in object storage just like a lake. 

The lakehouse stores data in object storage just like a lake. However, the lakehouse adds to this arrangement features designed to streamline data management and create an engineering experience similar to a data warehouse. This means robust table and schema support and features for managing incremental updates and deletes. Lakehouses typically also support table history and rollback; this is accomplished by retaining old versions of files and metadata.

A lakehouse system is a metadata and file-management layer deployed with data management and transformation tools. Databricks has heavily promoted the lakehouse concept with `Delta Lake`, an open source storage management system.

The key advantage of the data lakehouse over proprietary tools is interoperability. It’s much easier to exchange data between tools when stored in an open file format. Reserializing data from a proprietary database format incurs overhead in processing, time, and cost. In a data lakehouse architecture, various tools can connect to the metadata layer and read data directly from object storage.

It is important to emphasize that much of the data in a data lakehouse may not have a table structure imposed. We can impose data warehouse features where we need them in a lakehouse, leaving other data in a raw or even unstructured format.


### Data Platforms

A data platform is a central repository and processing house for all of an organization's data. A data platform handles the collection, cleansing, transformation, and application of data to generate business insights.


### Stream-to-Batch Storage Architecture

The stream-to-batch storage architecture has many similarities to the Lambda archi‐ tecture, though some might quibble over the technical details. Essentially, data flowing through a topic in the streaming storage system is written out to multiple consumers.


## Big Ideas and Trends in Storage

- `Data Catalog`

A data catalog is a centralized metadata store for all data across an organization. Strictly speaking, a data catalog is not a top-level data storage abstraction, but it integrates with various systems and abstractions.

Data catalogs are often used to provide a central place where people can view their data, queries, and data storage. As a data engineer, you’ll likely be responsible for setting up and maintaining the various data integrations of data pipeline and storage systems that will integrate with the data catalog and the integrity of the data catalog itself.

- `Data Sharing`

Data sharing allows organizations and individuals to share specific data and carefully defined permissions with specific entities. Data sharing allows data scientists to share data from a sandbox with their collaborators within an organization. Across organizations, data sharing facilitates collaboration between partner businesses. For example, an ad tech company can share advertising data with its customers.


- `Schema`

What is the expected form of the data? What is the file format? Is it structured, semistructured, or unstructured? What data types are expected? How does the data fit into a larger hierarchy? Is it connected to other data through shared keys or other relationships?

Note that schema need not be relational. Rather, data becomes more useful when we have as much information about its structure and organization. For images stored in a data lake, this schema information might explain the image format, resolution, and the way the images fit into a larger hierarchy.


Schema on write is essentially the traditional data warehouse pattern: a table has an integrated schema; any writes to the table must conform. To support schema on write, a data lake must integrate a schema metastore.

With schema on read, the schema is dynamically created when data is written, and a reader must determine the schema when reading the data. Ideally, schema on read is implemented using file formats that implement built-in schema information, such as Parquet or JSON. CSV files are notorious for schema inconsistency and are not recommended in this setting.


### Data Storage Lifecycle and Data Retention

Storing data isn’t as simple as just saving it to object storage or disk and forgetting about it. You need to think about the data storage lifecycle and data retention. When you think about access frequency and use cases, ask, “How important is the data to downstream users, and how often do they need to access it?” This is the data storage lifecycle. Another question you should ask is, “How long should I keep this data?” Do you need to retain data indefinitely, or are you fine discarding it past a certain time frame? 


- Hot data. Hot data has instant or frequent access requirements. The underlying storage for hot data is suited for fast access and reads, such as SSD or memory. Because of the type of hardware involved with hot data, storing hot data is often the most expensive form of storage. 

- Warm data is accessed semi-regularly, say, once per month. No hard and fast rules indicate how often warm data is accessed, but it’s less than hot data and more than cold data. The major cloud providers offer object storage tiers that accommodate warm data. For example, S3 offers an Infrequently Accessed Tier, and Google Cloud has a similar storage tier called Nearline. 

- Cold data. On the other extreme, cold data is infrequently accessed data. 


































































































