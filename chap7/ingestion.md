## Ingestion

You’ve learned about the various source systems you’ll likely encounter as a data engineer and about ways to store data. Let’s now turn our attention to the patterns and choices that apply to ingesting data from various source systems. The key engineering considerations for the ingestion phase, the major patterns for batch and streaming ingestion, technologies you’ll encounter, whom you’ll work with as you develop your data ingestion pipeline, and how the undercurrents feature in the ingestion phase.


  ![Ingestion aspect of the Data engineering lifecycle](/chap5/1_1t2zah1TG5_FCu0nTwkKtg.png)


### What is Ingestion?

Data ingestion is the process of moving data from one place to another. Data inges‐ tion implies data movement from source systems into storage in the data engineering lifecycle, with ingestion as an intermediate step.

  ![Data Source and Storage](/chap7/Untitled-2024-09-14-1236.png)


### Data Pipelines Defined

A data pipeline is the combination of architecture, systems, and processes that move data through the stages of the data engineering lifecycle.

Data pipelines begin in source systems, but ingestion is the stage where data engi‐ neers begin actively designing data pipeline activities. In the data engineering space, a good deal of ceremony occurs around data movement and processing patterns, with established patterns such as ETL, newer patterns such as ELT, and new names for long-established practices (reverse ETL) and data sharing.


### Key Engineering Considerations for the Ingestion Phase

When preparing to architect or build an ingestion system, here are some primary considerations and questions to ask yourself related to data ingestion:

- What’s the use case for the data I’m ingesting?
- Can I reuse this data and avoid ingesting multiple versions of the same dataset?
- Where is the data going? What’s the destination?
- How often should the data be updated from the source?
- What is the expected data volume?
- What format is the data in? Can downstream storage and transformation accept this format?
- Is the source data in good shape for immediate downstream use? That is, is the data of good quality? What post-processing is required to serve it? What are data-quality risks 
- Does the data require in-flight processing for downstream ingestion if the data is from a streaming source?


These questions undercut batch and streaming ingestion and apply to the underlying architecture you’ll create, build, and maintain. Regardless of how often the data is ingested, you’ll want to consider these factors when designing your ingestion architecture:

- `Bounded versus unbounded`: Unbounded data is data as it exists in reality, as events happen, either sporadically or continuously, ongoing and flowing.

Bounded data is a convenient way of bucketing data across some sort of boundary, such as time. Keep in mind the true unboundedness of your data; streaming ingestion systems are simply a tool for preserving the unbounded nature of data so that subsequent steps in the lifecycle can also process it continuously.


- `Frequency`: One of the critical decisions that data engineers must make in designing data-ingestion processes is the data-ingestion frequency. Ingestion processes can be batch, micro-batch, or real-time.

![Ingestion frequencies](/chap7/ingestion_frequencies.png)

We note that “real-time” ingestion patterns are becoming increasingly common. We put “real-time” in quotation marks because no ingestion system is genuinely real-time. Any database, queue or pipeline has inherent latency in delivering data to a target system. It is more accurate to speak of near real-time, but we often use real-time for brevity. 


- `Synchronous versus asynchronous`: With synchronous ingestion, the source, ingestion, and destination have complex dependencies and are tightly coupled. Just like the image below, each stage of the data engineering lifecycle has processes A, B, and C directly dependent upon one another. If process A fails, processes B and C cannot start; if process B fails, process C doesn’t start. This type of synchronous workflow is common in older ETL systems, where data extracted from a source system must then be transformed before being loaded into a data warehouse. Processes downstream of ingestion can’t start until all data in the batch has been ingested. If the ingestion or transformation process fails, the entire process must be rerun.

![Synchronous Ingestion](/chap7/synchronous_ingestion.png)

With asynchronous ingestion, dependencies can now operate at the level of individ‐ ual events, much as they would in a software backend built from microservices. Individual events become available in storage as soon as they are inges‐ ted individually. Take the example of a web application on AWS that emits events into Amazon Kinesis Data Streams (here acting as a buffer). The stream is read by Apache Beam, which parses and enriches events, and then forwards them to a second Kinesis stream; Kinesis Data Firehose rolls up events and writes objects to Amazon S3.

![Asynchronous Ingestion](/chap7/asynchronous_application.png)


- `Serialization and deserialization`: Moving data from source to destination involves serialization and deserialization. As a reminder, serialization means encoding the data from a source and preparing data structures for transmission and intermediate storage stages. When ingesting data, ensure that your destination can deserialize the data it receives.


- `Throughput and scalability`: Data throughput and system scalability become critical as your data volumes grow and requirements change. Design your systems to scale and shrink to flexibly match the desired data throughput.


- `Reliability and durability`: Reliability and durability are vital in the ingestion stages of data pipelines. Reliability entails high uptime and proper failover for ingestion systems. Durability entails making sure that data isn’t lost or corrupted. 


- `Payload`: A payload is the dataset you’re ingesting and has characteristics such as kind, shape, size, schema and data types, and metadata. 

    - Kind: The kind of data you handle directly impacts how it’s dealt with downstream in the data engineering lifecycle. Kind consists of type and format. Data has a type—tabular, image, video, text, etc. 

    - Shape: Every payload has a shape that describes its dimensions. Data shape is critical across the data engineering lifecycle. 
    Here are some examples of the shapes of various kinds of data: 
        - Tabular: The number of rows and columns in the dataset, commonly expressed as M rows and N columns.
        - Semistructured JSON: The key-value pairs and nesting depth occur with subelements.
        - Unstructured text: Number of words, characters, or bytes in the text body.
        - Images: The width, height, and RGB color depth (e.g., 8 bits per pixel).
        - Uncompressed audio: Number of channels (e.g., two for stereo), sample depth (e.g., 16 bits per sample), sample rate (e.g., 48 kHz), and length (e.g., 10,003 seconds).

    - Size: The size of the data describes the number of bytes of a payload. A payload may range in size from single bytes to terabytes and larger. 

    - Schema and data types: Many data payloads have a schema, such as tabular and semistructured data. A schema describes the fields and types of data within those fields. Other data, such as unstructured text, images, and audio, will not have an explicit schema or data types. However, they might come with technical file descriptions on shape, data and file format, encoding, size, etc.


- `Push versus pull versus poll patterns`: A push strategy involves a source system sending data to a target, while a pull strategy entails a target reading data directly from a source. 

Another pattern related to pulling is `polling` for data. Polling involves periodically checking a data source for any changes. When changes are detected, the destination pulls the data as it would in a regular pull situation.


### Batch Ingestion Considerations

Batch ingestion, which involves processing data in bulk, is often a convenient way to ingest data. This means that data is ingested by taking a subset of data from a source system, based either on a time interval or the size of accumulated data.

- Time-interval batch ingestion is widespread in traditional business ETL for data warehousing. This pattern is often used to process data once a day, overnight during off-hours, to provide daily reporting, but other frequencies can also be used.

- Size-based batch ingestion is quite common when data is moved from a streaming-based system into object storage; ultimately, you must cut the data into discrete blocks for future processing in a data lake. Some size-based ingestion systems can break data into objects based on various criteria, such as the size in bytes of the total number of events.


### Some commonly used batch ingestion patterns, include the following:

1. Snapshot or differential extraction: 

Data engineers must choose whether to capture full snapshots of a source system or differential (sometimes called incremental) updates. With full snapshots, engineers grab the entire current state of the source system on each update read. With the differential update pattern, engineers can pull only the updates and changes since the last read from the source system.

While differential updates are ideal for minimizing network traffic and target storage usage, full snapshot reads remain extremely com‐ mon because of their simplicity.


2. File-based export and ingestion:

Data is serialized into files in an exchangeable format, and these files are provided to an ingestion system. We consider file-based export to be a push-based ingestion pattern. 

File-based ingestion has several potential advantages over a direct database connec‐ tion approach. It is often undesirable to allow direct access to backend systems for security reasons. With file-based ingestion, export processes are run on the data-source side, giving source system engineers complete control over what data gets exported and how the data is preprocessed. 



3. ETL versus ELT:

The following are brief definitions of the extract and load parts of ETL and ELT:

- Extract: This means getting data from a source system. While extract seems to imply pulling data, it can also be push based. Extraction may also require reading metadata and schema changes.

- Load: Once data is extracted, it can either be transformed (ETL) before loading it into a storage destination or simply loaded into storage for future transformation. When loading data, you should be mindful of the type of system you’re loading, the schema of the data, and the performance impact of loading.


4. Inserts, updates, and batch size:

Batch-oriented systems often perform poorly when users attempt to perform many small-batch operations rather than a smaller number of large operations. For example, while it is common to insert one row at a time in a transactional database, this is a bad pattern for many columnar databases, as it forces the creation of many small, suboptimal files and forces the system to run a high number of create object operations. Running many small in-place update operations is an even bigger problem because it causes the database to scan each existing column file to run the update.

Understand the appropriate update patterns for the database or data store you’re working with. Also, understand that certain technologies are purpose-built for high insert rates. 



5. Data migration:

Migrating data to a new database or environment is not usually trivial, and data needs to be moved in bulk. Sometimes this means moving data sizes that are hundreds of terabytes or much larger, often involving the migration of specific tables and moving entire databases and systems. 

Data migrations probably aren’t a regular occurrence as a data engineer, but you should be familiar with them. As is often the case for data ingestion, schema manage‐ ment is a crucial consideration. Suppose you’re migrating data from one database system to a different one (say, SQL Server to Snowflake). No matter how closely the two databases resemble each other, subtle differences almost always exist in the way they handle schema. Fortunately, it is generally easy to test ingestion of a sample of data and find schema issues before undertaking a complete table migration.

Be aware that many tools are available to automate various types of data migrations. Especially for large and complex migrations, we suggest looking at these options before doing this manually or writing your own migration solution.



### Message and Stream Ingestion Considerations

Issues you should consider when ingesting events;

- Schema Evolution: Schema evolution is common when handling event data; fields may be added or removed, or value types might change (say, a string to an integer). Schema evolution can have unintended impacts on your data pipelines and destinations. For example, an IoT device gets a firmware update that adds a new field to the event it transmits, or a third-party API introduces changes to its event payload or countless other scenarios. All of these potentially impact your downstream capabilities.

- Late-Arriving Data: Though you probably prefer all event data to arrive on time, event data might arrive late. A group of events might occur around the same time frame (similar event times), but some might arrive later than others (late ingestion times) because of various circumstances.

- Ordering and Multiple Delivery: Streaming platforms are generally built out of distributed systems, which can cause some complications. Specifically, messages may be delivered out of order and more than once (at-least-once delivery).

- Replay: Replay allows readers to request a range of messages from the history, allowing you to rewind your event history to a particular point in time. Replay is a key capability in many streaming ingestion platforms and is particularly useful when you need to reingest and reprocess data for a specific time range. For example, RabbitMQ typically deletes messages after all subscribers consume them. Kafka, Kinesis, and Pub/Sub all support event retention and replay.

- Time to Live: How long will you preserve your event record? A key parameter is maximum message retention time, also known as the time to live (TTL). TTL is usually a configuration you’ll set for how long you want events to live before they are acknowledged and ingested. Any unacknowledged event that’s not ingested after its TTL expires auto‐ matically disappears. This is helpful to reduce backpressure and unnecessary event volume in your event-ingestion pipeline.

- Message Size: Message size is an easily overlooked issue: you must ensure that the streaming frame‐ work in question can handle the maximum expected message size. Amazon Kinesis supports a maximum message size of 1 MB. Kafka defaults to this maximum size but can be configured for a maximum of 20 MB or more.

- Error Handling and Dead-Letter Queues: Sometimes events aren’t successfully ingested. Perhaps an event is sent to a nonexis‐ tent topic or message queue, the message size may be too large, or the event has expired past its TTL. Events that cannot be ingested need to be rerouted and stored in a separate location called a dead-letter queue.

![Dead-Letter Queue](/chap7/dead-letter-queue.png)

A dead-letter queue segregates problematic events from events that can be accepted by the consumer. If events are not rerouted to a dead-letter queue, these erroneous events risk blocking other messages from being ingested. Data engineers can use a dead-letter queue to diagnose why event ingestions errors occur and solve data pipeline problems, and might be able to reprocess some messages in the queue after fixing the underlying cause of errors.


- Consumer Pull and Push: A consumer subscribing to a topic can get events in two ways: push and pull. Kafka and Kinesis support only pull subscriptions. Subscribers read messages from a topic and confirm when they have been processed. In addition to pull subscriptions, Pub/Sub and RabbitMQ support push subscriptions, allowing these services to write messages to a listener.

Pull subscriptions are the default choice for most data engineering applications, but you may want to consider push capabilities for specialized applications.


- Location: It is often desirable to integrate streaming across several locations for enhanced redundancy and to consume data close to where it is generated. As a general rule, the closer your ingestion is to where data originates, the better your bandwidth and latency.



### Ways to Ingest Data













































































