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



























































































