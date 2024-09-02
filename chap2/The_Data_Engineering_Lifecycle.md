## The Data Engineering Lifecycle

- `Generation`
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



- `Storage`
    - Storage runs across the entire data engineering lifecycle, often occurring in multiple places in a data pipeline, with storage systems crossing over with source systems, ingestion, transformation, and serving. I

    ### Evaluating storage systems: Key engineering considerations
        - Is this storage solution compatible with the architecture’s required write and read speeds?
        - Will storage create a bottleneck for downstream processes?
        - Do you understand how this storage technology works? Are you utilizing the storage system optimally or committing unnatural acts? For instance, are you applying a high rate of random access updates in an object storage system? (This is an antipattern with significant performance overhead.)
        - Will this storage system handle anticipated future scale? You should consider all capacity limits on the storage system: total available storage, read operation rate, write volume, etc.
        - Will downstream users and processes be able to retrieve data in the required service-level agreement (SLA)?
        - Are you capturing metadata about schema evolution, data flows, data lineage, and so forth? Metadata has a significant impact on the utility of data. Meta‐ data represents an investment in the future, dramatically enhancing discoverabil‐ ity and institutional knowledge to streamline future projects and architecture changes.
        - Is this a pure storage solution (object storage), or does it support complex query patterns (i.e., a cloud data warehouse)?
        - Is the storage system schema-agnostic (object storage)? Flexible schema (Cassandra)? Enforced schema (a cloud data warehouse)?
        - How are you tracking master data, golden records data quality, and data lineage for data governance?
        - How are you handling regulatory compliance and data sovereignty? For example, can you store your data in certain geographical locations but not others?
    
    ### Selecting a storage system
    What type of storage solution should you use? This depends on your use cases, data volumes, frequency of ingestion, format, and size of the data being ingested— essentially, the key considerations listed in the preceding bulleted questions. There is no one-size-fits-all universal storage recommendation. Every storage technology has its trade-offs. Countless varieties of storage technologies exist, and it’s easy to be overwhelmed when deciding the best option for your data architecture.


- `Ingestion`
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
    - In the push model of data ingestion, a source system writes data out to a target, whether a database, object store, or filesystem. In the pull model, data is retrieved from the source system. The line between the push and pull paradigms can be quite blurry; data is often pushed and pulled as it works its way through the various stages of a data pipeline. 
    
    - Consider, for example, the extract, transform, load (ETL) process, commonly used in batch-oriented ingestion workflows. ETL’s extract (E) part clarifies that we’re dealing with a pull ingestion model. With streaming ingestion, data bypasses a backend database and is pushed directly to an endpoint, typically with data buffered by an event-streaming platform.


- `Transformation`
    - The next stage of the data engineering lifecycle is transformation, meaning data needs to be changed from its original form into something useful for downstream use cases. Without proper transformations, data will sit inert, and not be in a useful form for reports, analysis, or ML. Typically, the transformation stage is where data begins to create value for downstream user consumption.

    ### Key considerations for the transformation phase
        - What’s the cost and return on investment (ROI) of the transformation? What is the associated business value?
        - Is the transformation as simple and self-isolated as possible?
        - What business rules do the transformations support?
    
    - Transformation is often entangled in other phases of the lifecycle. Typically, data is transformed in source systems or in flight during ingestion. 
    - Transformations are ubiquitous in various parts of the lifecycle. Data preparation, data wrangling, and cleaning—these transformative tasks add value for end consumers of data. 
    - Business logic is a major driver of data transformation, often in data modeling. Data translates business logic into reusable elements.



- `Serving Data`
    - Data serving is perhaps the most exciting part of the data engineering lifecycle. This is where the magic happens. This is where ML engineers can apply the most advanced techniques.

    ### Analytics
    Analytics is the core of most data endeavors. Once your data is stored and transformed, you’re ready to generate reports or dashboards and do ad hoc analysis on the data.

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


- `Security`

Data engineers must understand both data and access security, exercising the principle of least privilege. The principle of least privilege means giving a user or system access to only the essential data and resources to perform an intended function. A common antipattern we see with data engineers with little security experience is to give admin access to all users. This is a catastrophe waiting to happen! The first line of defense for data security is to create a culture of security that permeates the organization. All individ‐ uals who have access to data must understand their responsibility in protecting the company’s sensitive data and its customers.

Data should be protected from unwanted visibility, both in flight and at rest, by using encryption, tokenization, data masking, obfuscation, and simple, robust access controls. Data engineers must be competent security administrators, as security falls in their domain. A data engineer should understand security best practices for the cloud and on prem. Knowledge of user and identity access management (IAM) roles, policies, groups, network security, password policies, and encryption are good places to start.


- `Data management`

Data management is the development, execution, and supervision of plans, policies, programs, and practices that deliver, control, protect, and enhance the value of data and information assets throughout their lifecycle

    - `Data governance` 

    Data governance is, first and foremost, a data management function to ensure the quality, integrity, security, and usability of the data collected by an organization. Data governance is a foundation for data-driven business practices and a mission-critical part of the data engineering lifecycle. When data governance is practiced well, people, processes, and technologies align to treat data as a key business driver; if data issues occur, they are promptly handled.

        - `Discoverability`

        In a data-driven company, data must be available and discoverable. End users should have quick and reliable access to the data they need to do their jobs. They should know where the data comes from, how it relates to other data, and what the data means.
            - `Metadata`. Metadata is “data about data,” and it underpins every section of the data engineering lifecycle. Metadata is exactly the data needed to make data discoverable and governable.
            `Four main categories of metadata`:
                - `Business metadata`: Business metadata provides a data engineer with the right context and definitions to properly use data.

                - `Technical metadata`: Technical metadata describes the data created and used by systems across the data engineering lifecycle. It includes the data model and schema, data lineage, field mappings, and pipeline workflows. A data engineer uses technical metadata to create, connect, and monitor various systems across the data engineering lifecycle.

                - `Operational metadata`: Operational metadata describes the operational results of various systems and includes statistics about processes, job IDs, application runtime logs, data used in a process, and error logs. A data engineer uses operational metadata to determine whether a process succeeded or failed and the data involved in the process.

                - `Reference metadata`: Reference metadata is data used to classify other data. This is also referred to as lookup data. Standard examples of reference data are internal codes, geographic codes, units of measurement, and internal calendar standards.

        - `Data Quality`: A data engineer ensures data quality across the entire data engineering lifecycle. This involves performing data-quality tests, and ensuring data conformance to schema expectations, data completeness, and precision.

        Data quality is defined by three main characteristics:
            - Accuracy: Is the collected data factually correct? Are there duplicate values? Are the numeric values accurate?
            - Completeness: Are the records complete? Do all required fields contain valid values?
            - Timeliness: Are records available in a timely fashion?

        - `Data Accountability`: Data accountability means assigning an individual to govern a portion of data. The responsible person then coordinates the governance activities of other stakeholders. Managing data quality is tough if no one is accountable for the data in question.

    - `Data Modeling and design`:

    To derive business insights from data, through business analytics and data science, the data must be in a usable form. The process for converting data into a usable form is known as data modeling and design. Data engineers need to understand modeling best practices as well as develop the flexibility to apply the appropriate level and type of modeling to the data source and use case.


    - `Data lineage`

    Data lineage describes the recording of an audit trail of data through its lifecycle, tracking both the systems that process the data and the upstream data it depends on. Data lineage helps with error tracking, accountability, and debugging of data and the systems that process it. It has the obvious benefit of giving an audit trail for the data lifecycle and helps with compliance.


    - `Storage and Operations`


    - `Data integration and interoperability`

    Data integration and interoperability is the process of integrating data across tools and processes. As we move away from a single-stack approach to analytics and toward a heterogeneous cloud environment in which various tools process data on demand, integration and interoperability occupy an ever-widening swath of the data engineer’s job.


    - `Data lifecycle management`

    The advent of data lakes encouraged organizations to ignore data archival and destruction. Why discard data when you can simply add more storage ad infinitum? Two changes have encouraged engineers to pay more attention to what happens at the end of the data engineering lifecycle. Tools such as Hive ACID and Delta Lake allow easy management of deletion transactions at scale. New generations of metadata management, data lineage, and cataloging tools will also streamline the end of the data engineering lifecycle.

    Data engineers need to ensure that datasets mask personally identifiable information (PII) and other sensitive information; bias can be identified and tracked in datasets as they are trans‐ formed.


    - `Data Integrity`

    Data integrity is a concept and process that ensures the accuracy, completeness, consistency, and validity of an organization's data. By following the process, organizations not only ensure the integrity of the data but guarantee they have accurate and correct data in their database.


    - `Data systems for advanced analytics and ML`

    - `Ethics and privacy`

    The last several years of data breaches, misinformation, and mishandling of data make one thing clear: data impacts people.


- `DataOps`

DataOps maps the best practices of Agile methodology, DevOps, and statistical process control (SPC) to data. Whereas DevOps aims to improve the release and quality of software products, DataOps does the same thing for data products.

Data products differ from software products because of the way data is used. A software product provides specific functionality and technical features for end users. By contrast, a data product is built around sound business logic and metrics, whose users make decisions or build models that perform automated actions. A data engineer must understand both the technical aspects of building software products and the business logic, quality, and metrics that will create excellent data products.

DataOps is a collection of technical practices, workflows, cultural norms, and architectural patterns that enable:
    - Rapid innovation and experimentation delivering new insights to customers with increasing velocity
    - Extremely high data quality and very low error rates
    - Collaboration across complex arrays of people, technology, and environments
    - Clear measurement, monitoring, and transparency of results


    ### Three Pillars of DataOps
        - `Automation`

            Automation enables reliability and consistency in the DataOps process and allows data engineers to quickly deploy new product features and improvements to existing workflows. DataOps automation has a similar framework and workflow to DevOps, consisting of change management (environment, code, and data version control), continuous integration/continuous deployment (CI/CD), and configuration as code.

            DataOps practices monitor and maintain the reliability of technology and systems (data pipelines, orchestration, etc.), with the added dimension of checking for data quality, data/model drift, metadata integrity, and more.

            As the organization’s data maturity grows, data engineers will typically adopt an orchestration framework, perhaps Airflow or Dagster. 

        - `Observabilty and monitoring`

        If you’re not observing and monitoring your data and the systems that produce the data, you’re inevitably going to experience your own data horror story. Observability, monitoring, logging, alerting, and tracing are all critical to getting ahead of any problems along the data engineering lifecycle. We recommend you incorporate SPC to understand whether events being monitored are out of line and which incidents are worth responding to.

        - `Incident reporting`

        A high-functioning data team using DataOps will be able to ship new data products quickly. But mistakes will inevitably happen. A system may have downtime, a new data model may break downstream reports, an ML model may become stale and provide bad predictions—countless problems can interrupt the data engineering lifecycle. Incident response is about using the automation and observability capabilities mentioned previously to rapidly identify root causes of an incident and resolve it as reliably and quickly as possible.

        Incident response is as much about retroactively responding to incidents as proactively addressing them before they happen.



- `Data architecture`

A data architecture reflects the current and future state of data systems that support an organization’s long-term data needs and strategy. 

A data engineer should first understand the needs of the business and gather require‐ ments for new use cases. Next, a data engineer needs to translate those requirements to design new ways to capture and serve data, balanced for cost and operational simplicity. This means knowing the trade-offs with design patterns, technologies, and tools in source systems, ingestion, storage, transformation, and serving data.

    - Analyze trade-offs
    - Design for Agility
    - Add value to the business



- `Orchestration`

Orchestration is the process of coordinating many jobs to run as quickly and efficiently as possible on a scheduled cadence. Orchestration systems also build job history capabilities, visualization, and alerting. Advanced orchestration engines can backfill new DAGs or individual tasks as they are added to a DAG. They also support dependencies over a time range.

At this writing, several nascent open source projects aim to mimic the best elements of Airflow’s core design while improving on it in key areas. Some of the most interesting examples are Prefect and Dagster, which aim to improve the portability and testability of DAGs to allow engineers to move from local development to production more easily. Argo is an orchestration engine built around Kubernetes primitives; Metaflow is an open source project out of Netflix that aims to improve data science orchestration.

We must point out that orchestration is strictly a batch concept. The streaming alternative to orchestrated task DAGs is the streaming DAG. Streaming DAGs remain challenging to build and maintain, but next-generation streaming platforms such as Pulsar aim to dramatically reduce the engineering and operational burden.

    - Coordinate workflows
    - Schedule jobs
    - Manage tasks



- `Software engineering`

Cloud data warehouses support powerful transfor‐ mations using SQL semantics; tools like Spark have become more user-friendly, transitioning away from low-level coding details and toward easy-to-use dataframes. Despite this abstraction, software engineering is still critical to data engineering.

    - A few common areas of software engineering that apply to the data engineering lifecycle.

        - `Core data processing code`
        Though it has become more abstract and easier to manage, core data processing code still needs to be written, and it appears throughout the data engineering lifecycle. Whether in ingestion, transformation, or data serving, data engineers need to be highly proficient and productive in frameworks and languages such as Spark, SQL, or Beam. 

        It’s also imperative that a data engineer understand proper code-testing methodologies, such as unit, regression, integration, end-to-end, and smoke.

        - `Development of open source frameworks`
        Many data engineers are heavily involved in developing open source frameworks. They adopt these frameworks to solve specific problems in the data engineering lifecycle, and then continue developing the framework code to improve the tools for their use cases and contribute back to the community.

        A new batch of open source competitors (including Prefect, Dagster, and Metaflow) has sprung up to fix perceived limitations of Airflow, providing better metadata handling, portability, and dependency management. Where the future of orchestration goes is anyone’s guess.

        - `Streaming`
        Streaming data processing is inherently more complicated than batch, and the tools and paradigms are arguably less mature. As streaming data becomes more pervasive in every stage of the data engineering lifecycle, data engineers face interesting soft‐ ware engineering problems.

        Windowing allows real-time systems to calculate valuable metrics such as trailing statistics. Engineers have many frameworks to choose from, including various function platforms (OpenFaaS, AWS Lambda, Google Cloud Func‐ tions) for handling individual events or dedicated stream processors (Spark, Beam, Flink, or Pulsar) for analyzing streams to support reporting and real-time actions.

        - `Infrastructure as code`

        Infrastructure as code (IaC) applies software engineering practices to the configura‐ tion and management of infrastructure. The infrastructure management burden of the big data era has decreased as companies have migrated to managed big data systems—such as Databricks and Amazon Elastic MapReduce (EMR)—and cloud data warehouses. When data engineers have to manage their infrastructure in a cloud environment, they increasingly do this through IaC frameworks rather than manually spinning up instances and installing software. Several general-purpose and cloud- platform-specific frameworks allow automated infrastructure deployment based on a set of specifications. Many of these frameworks can manage cloud services as well as infrastructure. There is also a notion of IaC with containers and Kubernetes, using tools like Helm.

        - `Pipelines as code`

        Pipelines as code is the core concept of present-day orchestration systems, which touch every stage of the data engineering lifecycle. Data engineers use code (typically Python) to declare data tasks and dependencies among them. The orchestration engine interprets these instructions to run steps using available resources.

        - `General-purpose problem solving`

        In practice, regardless of which high-level tools they adopt, data engineers will run into corner cases throughout the data engineering lifecycle that require them to solve problems outside the boundaries of their chosen tools and to write custom code. They should be proficient in software engineering to understand APIs, pull and transform data, handle exceptions, and so forth.

    - Programming and coding skills
    - Software design patterns
    - Testing and debugging