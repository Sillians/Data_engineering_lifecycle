## The Data Engineering Lifecycle

- Generation
    - Source Systems: A source system is the origin of the data used in the data engineering lifecycle.

    ### Evaluating source systems: Key engineering considerations
        - What are the essential characteristics of the data source? Is it an application? A swarm of IoT devices?
        - How is data persisted in the source system? Is data persisted long term, or is it temporary and quickly deleted?
        - At what rate is data generated? How many events per second? How many giga‐ bytes per hour?
        - What level of consistency can data engineers expect from the output data? If you’re running data-quality checks against the output data, how often do data inconsistencies occur—nulls where they aren’t expected, lousy formatting, etc.?
        - How often do errors occur?
        - Will the data contain duplicates?
        - Will some data values arrive late, possibly much later than other messages pro‐ duced simultaneously?
        - What is the schema of the ingested data? Will data engineers need to join across several tables or even several systems to get a complete picture of the data?
        - If schema changes (say, a new column is added), how is this dealt with and communicated to downstream stakeholders?
        - How frequently should data be pulled from the source system?
        - For stateful systems (e.g., a database tracking customer account information), is data provided as periodic snapshots or update events from change data capture (CDC)? What’s the logic for how changes are performed, and how are these tracked in the source database?
        - Who/what is the data provider that will transmit the data for downstream consumption?
        - Will reading from a data source impact its performance?
        - Does the source system have upstream data dependencies? What are the charac‐ teristics of these upstream systems?
        - Are data-quality checks in place to check for late or missing data?


    - `Schemaless` doesn’t mean the absence of schema. Rather, it means that the application defines the schema as data is written, whether to a message queue, a flat file, a blob, or a document database such as MongoDB. 
    - A more traditional model built on relational database storage uses a fixed schema enforced in the database, to which application writes must conform.



- Storage
    - Storage runs across the entire data engineering lifecycle, often occurring in multiple places in a data pipeline, with storage systems crossing over with source systems, ingestion, transformation, and serving. I

    ### Evaluating storage systems: Key engineering considerations
        - Is this storage solution compatible with the architecture’s required write and read speeds?
        - Will storage create a bottleneck for downstream processes?
        - Do you understand how this storage technology works? Are you utilizing the storage system optimally or committing unnatural acts? For instance, are you applying a high rate of random access updates in an object storage system? (This is an antipattern with significant performance overhead.)
        - Will this storage system handle anticipated future scale? You should consider all capacity limits on the storage system: total available storage, read operation rate, write volume, etc.
        - Will downstream users and processes be able to retrieve data in the required service-level agreement (SLA)?
        - Are you capturing metadata about schema evolution, data flows, data lineage, and so forth? Metadata has a significant impact on the utility of data. Meta‐ data represents an investment in the future, dramatically enhancing discoverabil‐ ity and institutional knowledge to streamline future projects and architecture changes.
        - Is this a pure storage solution (object storage), or does it support complex query patterns (i.e., a cloud data warehouse)?
        - Is the storage system schema-agnostic (object storage)? Flexible schema (Cassan‐ dra)? Enforced schema (a cloud data warehouse)?
        - How are you tracking master data, golden records data quality, and data lineage for data governance?
        - How are you handling regulatory compliance and data sovereignty? For example, can you store your data in certain geographical locations but not others?
    
    ### Selecting a storage system
    What type of storage solution should you use? This depends on your use cases, data volumes, frequency of ingestion, format, and size of the data being ingested— essentially, the key considerations listed in the preceding bulleted questions. There is no one-size-fits-all universal storage recommendation. Every storage technology has its trade-offs. Countless varieties of storage technologies exist, and it’s easy to be overwhelmed when deciding the best option for your data architecture.


- Ingestion
    - After you understand the data source, the characteristics of the source system you’re using, and how data is stored, you need to gather the data. The next stage of the data engineering lifecycle is data ingestion from source systems.

    ### Key engineering considerations for the ingestion phase
        - What are the use cases for the data I’m ingesting? Can I reuse this data rather than create multiple versions of the same dataset?
        - Are the systems generating and ingesting this data reliably, and is the data available when I need it?
        - What is the data destination after ingestion?
        - How frequently will I need to access the data?
        - In what volume will the data typically arrive?
        - What format is the data in? Can my downstream storage and transformation systems handle this format?
        - Is the source data in good shape for immediate downstream use? If so, for how long, and what may cause it to be unusable?
        - If the data is from a streaming source, does it need to be transformed before reaching its destination? Would an in-flight transformation be appropriate, where the data is transformed within the stream itself?

    ### Batch versus streaming
    - Batch ingestion is simply a specialized and convenient way of processing this stream in large chunks—for example, handling a full day’s worth of data in a single batch. Batch data is ingested either on a predetermined time interval or as data reaches a preset size threshold. Batch ingestion is a one-way door: once data is broken into batches, the latency for downstream consumers is inherently constrained. Because of limitations of legacy systems, batch was for a long time the default way to ingest data.

    - Streaming ingestion allows us to provide data to downstream systems—whether other applications, databases, or analytics systems—in a continuous, real-time fashion. Real-time (or near real-time) means that the data is available to a down‐ stream system a short time after it is produced (e.g., less than one second later).

    The choice largely depends on the use case and expectations for data timeliness.
    
    #### Key considerations for batch versus stream ingestion
    Should you go streaming-first? Despite the attractiveness of a streaming-first approach, there are many trade-offs to understand and think about.

    - If I ingest the data in real time, can downstream storage systems handle the rate of data flow?
    - Do I need millisecond real-time data ingestion? Or would a micro-batch approach work, accumulating and ingesting data, say, every minute?
    - What are my use cases for streaming ingestion? What specific benefits do I realize by implementing streaming? If I get data in real time, what actions can I take on that data that would be an improvement upon batch?
    - Will my streaming-first approach cost more in terms of time, money, mainte‐ nance, downtime, and opportunity cost than simply doing batch?
    - Are my streaming pipeline and system reliable and redundant if infrastructure fails?
    - What tools are most appropriate for the use case? Should I use a managed service (Amazon Kinesis, Google Cloud Pub/Sub, Google Cloud Dataflow) or stand up my own instances of Kafka, Flink, Spark, Pulsar, etc.? If I do the latter, who will manage it? What are the costs and trade-offs?
    - If I’m deploying an ML model, what benefits do I have with online predictions and possibly continuous training?
    - Am I getting data from a live production instance? If so, what’s the impact of my ingestion process on this source system?

    ### Push versus pull
    In the push model of data ingestion, a source system writes data out to a target, whether a database, object store, or filesystem. In the pull model, data is retrieved from the source system. The line between the push and pull paradigms can be quite blurry; data is often pushed and pulled as it works its way through the various stages of a data pipeline. 
    
    Consider, for example, the extract, transform, load (ETL) process, commonly used in batch-oriented ingestion workflows. ETL’s extract (E) part clarifies that we’re dealing with a pull ingestion model. With streaming ingestion, data bypasses a backend database and is pushed directly to an endpoint, typically with data buffered by an event-streaming platform.


- Transformation 
    - The next stage of the data engineering lifecycle is transformation, meaning data needs to be changed from its original form into something useful for downstream use cases. Without proper transformations, data will sit inert, and not be in a useful form for reports, analysis, or ML. Typically, the transformation stage is where data begins to create value for downstream user consumption.

    ### Key considerations for the transformation phase
        - What’s the cost and return on investment (ROI) of the transformation? What is the associated business value?
        - Is the transformation as simple and self-isolated as possible?
        - What business rules do the transformations support?
    
    - Transformation is often entangled in other phases of the lifecycle. Typically, data is transformed in source systems or in flight during ingestion. 
    - Transformations are ubiquitous in various parts of the lifecycle. Data preparation, data wrangling, and cleaning—these transformative tasks add value for end consumers of data. 
    - Business logic is a major driver of data transformation, often in data modeling. Data translates business logic into reusable elements.



- Serving Data
    - Data serving is perhaps the most exciting part of the data engineering lifecycle. This is where the magic happens. This is where ML engineers can apply the most advanced techniques.

    ### Analytics
    Analytics is the core of most data endeavors. Once your data is stored and trans‐ formed, you’re ready to generate reports or dashboards and do ad hoc analysis on the data.

    #### Business intelligence
    BI requires using business logic to process raw data. A BI system maintains a repository of business logic and definitions. This business logic is used to query the data warehouse so that reports and dashboards align with business definitions.


    #### Operational analytics
    Operational analytics focuses on the fine-grained details of operations, promoting actions that a user of the reports can act upon immediately. Operational analytics could be a live view of inventory or real-time dashboarding of website or application health. In this case, data is consumed in real time, either directly from a source system or from a streaming data pipeline. 


    #### Embedded analytics
    With embedded analytics, the request rate for reports, and the corresponding bur‐ den on analytics systems, goes up dramatically; access control is significantly more complicated and critical. Businesses may be serving separate analytics and data to thousands or more customers. Each customer must see their data and only their data.

    ### Machine Learning
    The feature store is a recently developed tool that combines data engineering and ML engineering. Feature stores are designed to reduce the operational burden for ML engineers by maintaining feature history and versions, supporting feature sharing among teams, and providing basic operational and orchestration capabilities, such as backfilling. In practice, data engineers are part of the core support team for feature stores to support ML engineering.

    While ML is exciting, our experience is that companies often prematurely dive into it. Before investing a ton of resources into ML, take the time to build a solid data foundation. This means setting up the best systems and architecture across the data engineering and ML lifecycle. It’s generally best to develop competence in analytics before moving to ML. Many companies have dashed their ML dreams because they undertook initiatives without appropriate foundations.


## N/B
Data processing pipeline:
- `Upstream` would be the data sources, data ingestion, and ETL (Extract, Transform, Load) processes that prepare the raw data.
- `Downstream` would be the data analysis, visualization, and reporting components that consume the transformed data.

Similarly, in a software development lifecycle:
- `Upstream` would be requirements gathering, design, and coding of individual modules.
- `Downstream` would be integration, testing, deployment, and support of the full application.


## Major Undercurrents Across the Data Engineering Lifecycle
Data engineering now encompasses far more than tools and technology. The field is now moving up the value chain, incorporating traditional enterprise practices such as data management and cost optimization and newer practices like DataOps.


- Security
Data engineers must understand both data and access security, exercising the principle of least privilege. The principle of least privilege means giving a user or system access to only the essential data and resources to perform an intended function. A common antipattern we see with data engineers with little security experience is to give admin access to all users. This is a catastrophe waiting to happen! The first line of defense for data security is to create a culture of security that permeates the organization. All individ‐ uals who have access to data must understand their responsibility in protecting the company’s sensitive data and its customers.

Data should be protected from unwanted visibility, both in flight and at rest, by using encryption, tokenization, data masking, obfuscation, and simple, robust access controls. Data engineers must be competent security administrators, as security falls in their domain. A data engineer should understand security best practices for the cloud and on prem. Knowledge of user and identity access management (IAM) roles, policies, groups, network security, password policies, and encryption are good places to start.

Access Control to;
    - Data
    - Systems



- Data management
Data management is the development, execution, and supervision of plans, policies, programs, and practices that deliver, control, protect, and enhance the value of data and information assets throughout their lifecycle
    - `Data governance` 
    Data governance is, first and foremost, a data management function to ensure the quality, integrity, security, and usability of the data collected by an organization. Data governance is a foundation for data-driven business practices and a mission- critical part of the data engineering lifecycle. When data governance is practiced well, people, processes, and technologies align to treat data as a key business driver; if data issues occur, they are promptly handled.

        - `Discoverability`
        In a data-driven company, data must be available and discoverable. End users should have quick and reliable access to the data they need to do their jobs. They should know where the data comes from, how it relates to other data, and what the data means.
            - `Metadata`. Metadata is “data about data,” and it underpins every section of the data engineering lifecycle. Metadata is exactly the data needed to make data discoverable and governable.
            `Four main categories of metadata`:
                - Business metadata: Business metadata provides a data engineer with the right context and definitions to properly use data.

                - Technical metadata: Technical metadata describes the data created and used by systems across the data engineering lifecycle. It includes the data model and schema, data lineage, field mappings, and pipeline workflows. A data engineer uses technical metadata to create, connect, and monitor various systems across the data engineering lifecycle.

                - Operational metadata: Operational metadata describes the operational results of various systems and includes statistics about processes, job IDs, application runtime logs, data used in a process, and error logs. A data engineer uses operational metadata to determine whether a process succeeded or failed and the data involved in the process.

                - Reference metadata: Reference metadata is data used to classify other data. This is also referred to as lookup data. Standard examples of reference data are internal codes, geographic codes, units of measurement, and internal calendar standards.

        - Data Quality and Definitions: A data engineer ensures data quality across the entire data engineering lifecycle. This involves performing data-quality tests, and ensuring data conformance to schema expectations, data completeness, and precision.

        Data quality is defined by three main characteristics:
            - Accuracy: Is the collected data factually correct? Are there duplicate values? Are the numeric values accurate?
            - Completeness: Are the records complete? Do all required fields contain valid values?
            - Timeliness: Are records available in a timely fashion?

        - Data Accountability: Data accountability means assigning an individual to govern a portion of data. The responsible person then coordinates the governance activities of other stakeholders. Managing data quality is tough if no one is accountable for the data in question.

    - `Data Modeling and design`




    - Data lineage
    - Storage and Operations
    - Data integration and interoperability
    - Data lifecycle management
    - Data Integrity
    - Data systems for advanced analytics and ML
    - Ethics and privacy




- DataOps
    - Data goverance
    - Observabilty and monitoring
    - Incident reporting



- Data architecture
    - Analyze trade-offs
    - Design for Agility
    - Add value to the business



- Orchestration
    - Coordinate workflows
    - Schedule jobs
    - Manage tasks



- Software engineering
    - Programming and coding skills
    - Software design patterns
    - Testing and debugging