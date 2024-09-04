## Designing Good Data Architecture

Good data architecture provides seamless capabilities across every step of the data lifecycle and undercurrent. Successful data engineering is built upon rock-solid data architecture.

Data architecture is the design of systems to support the evolving data needs of an enterprise, achieved by flexible and reversible decisions reached through a careful evaluation of trade-offs.

- `Operational architecture` encompasses the functional requirements of what needs to happen related to people, processes, and technology. For example, 
    - what business processes does the data serve? 
    - How does the organization manage data quality? 
    - What is the latency requirement from when the data is produced to when it becomes available to query?

- `Technical architecture` outlines how data is ingested, stored, transformed, and served along the data engineering lifecycle. For instance, how will you move `10TB` of data every hour from a source database to your data lake? In short, operational architecture describes what needs to be done, and technical architecture details how it will happen.

### “Good” Data Architecture

“Architecture represents the significant design decisions that shape a system, where significant is measured by cost of change.” Data architects aim to make significant decisions that will lead to good architecture at a basic level. 

Good data architecture serves business requirements with a common, widely reusable set of building blocks while maintaining flexibility and making appropriate trade-offs. Bad architecture is authoritarian and tries to cram a bunch of one-size-fits-all decisions into a big ball of mud.

Agility is the foundation for good data architecture; it acknowledges that the world is fluid. Good data architecture is flexible and easily maintainable. It evolves in response to changes within the business and new technologies and practices that may unlock even more value in the future. Businesses and their use cases for data are always evolving. The world is dynamic, and the pace of change in the data space is accelerating. Last year’s data architecture that served you well might not be sufficient for today, let alone next year.

Bad data architecture is tightly coupled, rigid, overly centralized, or uses the wrong tools for the job, hampering development and change management. Ideally, by designing architecture with reversibility in mind, changes will be less costly.

Good data architecture is a living, breathing thing. It’s never finished. In fact, per our definition, change and evolution are central to the meaning and purpose of data architecture.


### Principles of Good Data Architecture

Principles of data engineering architecture:

1. `Choose common components wisely`.

Common components can be anything that has broad applicability within an orga‐ nization. Common components include object storage, version-control systems, observability, monitoring and orchestration systems, and processing engines. Com‐ mon components should be accessible to everyone with an appropriate use case, and teams are encouraged to rely on common components already in use rather than reinventing the wheel. Common components must support robust permissions and security to enable sharing of assets among teams while preventing unauthorized access.


2. `Plan for failure`.

Modern hardware is highly robust and durable. Even so, any hardware component will fail, given enough time. To build highly robust data systems, you must consider failures in your designs.

key terms for evaluating failure scenarios
- Availability
- Reliability
- Recovery time objective
- Recovery point objective

Engineers need to consider acceptable reliability, availability, RTO, and RPO in designing for failure. These will guide their architecture decisions as they assess possible failure scenarios.


3. `Architect for scalability`.

Scalability in data systems encompasses two main capabilities. First, scalable systems can scale up to handle significant quantities of data. Second, scalable systems can scale down. Once the load spike ebbs, we should automatically remove capacity to cut costs. 


4. `Architecture is leadership`.

Data architects are responsible for technology decisions and architecture descrip‐ tions and disseminating these choices through effective leadership and training. Data architects should be highly technically competent but delegate most individual contributor work to others. Strong leadership skills combined with high technical competence are rare and extremely valuable. The best data architects take this duality seriously.

An ideal data architect manifests similar characteristics. They possess the technical skills of a data engineer but no longer practice data engineering day to day; they mentor current data engineers, make careful technology choices in consultation with their organization, and disseminate expertise through training and leadership. They train engineers in best practices and bring the company’s engineering resources together to pursue common goals in both technology and business.



5. `Always be architecting`.

Data architects don’t serve in their role simply to maintain the existing state; instead, they constantly design new and exciting things in response to changes in business and technology. 

Modern architecture should not be command-and-control or waterfall but collaborative and agile. The data architect maintains a target architecture and sequencing plans that change over time. The target architecture becomes a moving target, adjusted in response to business and technology changes internally and world‐ wide. 



6. `Build loosely coupled systems`. 

    "When the architecture of the system is designed to enable teams to test, deploy, and change systems without dependencies on other teams, teams require little communica‐ tion to get work done. In other words, both the architecture and the teams are loosely coupled."

### For software architecture, a loosely coupled system has the following properties:
- Systems are broken into many small components.
- These systems interface with other services through abstraction layers, such as a messaging bus or an API. These abstraction layers hide and protect internal details of the service, such as a database backend or internal classes and method calls.
- As a consequence of property 2, internal changes to a system component don’t require changes in other parts. Details of code updates are hidden behind stable APIs. Each piece can evolve and improve separately.
- As a consequence of property 3, there is no waterfall, global release cycle for the whole system. Instead, each component is updated separately as changes and improvements are made.


### Let’s translate these technical characteristics into organizational characteristics:
- Many small teams engineer a large, complex system. Each team is tasked with engineering, maintaining, and improving some system components.
- These teams publish the abstract details of their components to other teams via API definitions, message schemas, etc. Teams need not concern themselves with other teams’ components; they simply use the published API or message specifications to call these components. They iterate their part to improve their performance and capabilities over time. They might also publish new capabilities as they are added or request new stuff from other teams. Again, the latter happens without teams needing to worry about the internal technical details of the requested features. Teams work together through loosely coupled communication.
- As a consequence of characteristic 2, each team can rapidly evolve and improve its component independently of the work of other teams.
- Specifically, characteristic 3 implies that teams can release updates to their com‐ ponents with minimal downtime. Teams release continuously during regular working hours to make code changes and test them.

Loose coupling of both technology and human systems will allow your data engineer‐ ing teams to more efficiently collaborate with one another and with other parts of the company. 


7. `Make reversible decisions`.

The data landscape is changing rapidly. Today’s hot technology or stack is tomorrow’s afterthought. Popular opinion shifts quickly. You should aim for reversible decisions, as these tend to simplify your architecture and keep it agile.

    “One of an architect’s most important tasks is to remove architecture by finding ways to eliminate irreversibility in software designs.”

Bezos refers to reversible decisions as “two-way doors.” As he says, “If you walk through and don’t like what you see on the other side, you can’t get back to before. We can call these Type 1 decisions. But most decisions aren’t like that-they are changeable, reversible—they’re two-way doors.” Aim for two-way doors whenever possible.

Given the pace of change—and the decoupling/modularization of technologies across your data architecture—always strive to pick the best-of-breed solutions that work for today. Also, be prepared to upgrade or adopt better practices as the landscape evolves.


8. `Prioritize security`.

Every data engineer must assume responsibility for the security of the systems they build and maintain. 


9. `Embrace FinOps`.

FinOps is an evolving cloud financial management discipline and cultural practice that enables organizations to get maximum business value by helping engineering, finance, technology, and business teams to collaborate on data-driven spending decisions.

Cloud tooling necessitates a set of processes for managing spending and resources. In the past, data engineers thought in terms of performance engineering—maximizing the performance for data processes on a fixed set of resources and buying adequate resources for future needs.


## Major Architecture Concepts

If you follow the current trends in data, it seems like new types of data tools and architectures are arriving on the scene every week. Amidst this flurry of activity, we must not lose sight of the main goal of all of these architectures: to take data and transform it into something useful for downstream consumption.

### Distributed Systems, Scalability, and Designing for Failure 

As data engineers, we’re interested in four closely related characteristics of data systems 
- Scalability : Allows us to increase the capacity of a system to improve performance and handle the demand. 

- Elasticity: The ability of a scalable system to scale dynamically; a highly elastic system can automatically scale up and down based on the current workload. Scaling up is critical as demand increases, while scaling down saves money in a cloud environ‐ ment.

- Availability: The percentage of time an IT service or component is in an operable state.

- Reliability: The system’s probability of meeting defined standards in performing its intended function during a specified interval.

Distributed systems are widespread in the various data technologies you’ll use across your architecture. Almost every cloud data warehouse object storage system you use has some notion of distribution under the hood. Management details of the distributed system are typically abstracted away, allowing you to focus on high-level architecture instead of low-level plumbing. 


### Tight Versus Loose Coupling: Tiers, Monoliths, and Microservices

When designing a data architecture, you choose how much interdependence you want to include within your various domains, services, and resources. On one end of the spectrum, you can choose to have extremely centralized dependencies and workflows. Every part of a domain and service is vitally dependent upon every other domain and service. This pattern is known as tightly coupled.

On the other end of the spectrum, you have decentralized domains and services that do not have strict dependence on each other, in a pattern known as loose coupling. In a loosely coupled scenario, it’s easy for decentralized teams to build systems whose data may not be usable by their peers. 

Designing “good” data architecture relies on trade-offs between the tight and loose coupling of domains and services.

- `Architecture tiers`: As you develop your architecture, it helps to be aware of architecture tiers. Your architecture has layers—data, application, business logic, presentation, and so forth —and you need to know how to decouple these layers. Because tight coupling of modalities presents obvious vulnerabilities, keep in mind how you structure the layers of your architecture to achieve maximum reliability and flexibility.

    - `Single tier`: In a single-tier architecture, your database and application are tightly coupled, residing on a single server. While single-tier architectures are good for prototyping and development, they are not advised for production environments because of the obvious failure risks.

    - `Multi-tier`: The challenges of a tightly coupled single-tier architecture are solved by decoupling the data and application. A multitier (also known as n-tier) architecture is composed of separate layers: data, application, business logic, presentation, etc. These layers are bottom-up and hierarchical, meaning the lower layer isn’t necessarily dependent on the upper layers; the upper layers depend on the lower layers.

    A common multitier architecture is a three-tier architecture, a widely used client-server design. A three-tier architecture consists of data, application logic, and presentation tiers. Each tier is isolated from the other, allowing for separation of concerns. With a three-tier architecture, you’re free to use whatever technologies you prefer within each tier without the need to be monolithically focused.

        `Additional Considerations`:
        - N-tier architectures are not restricted to three tiers. For more complex applications, it is common to have more tiers.
        - Tiers are the boundary of scalability, reliability, and security. Consider having separate tiers for services with different requirements in those areas.
        - Use virtual machine scale sets for autoscaling.
        - Look for places in the architecture where you can use a managed service without significant refactoring. In particular, look at caching, messaging, storage, and databases.
        - For higher security, place a network DMZ in front of the application


![n-tier architecture](/chap3/q9UGw.png)


- `Monoliths`: The general notion of a monolith includes as much as possible under one roof; in its most extreme version, a monolith consists of a single codebase running on a single machine that provides both the application logic and user interface. 

Coupling within monoliths can be viewed in two ways: technical coupling and domain coupling. Technical coupling refers to architectural tiers, while domain cou‐ pling refers to the way domains are coupled together. A monolith has varying degrees of coupling among technologies and domains. You could have an application with various layers decoupled in a multitier architecture but still share multiple domains.



- `Microservices`: Microservices architecture comprises separate, decentralized, and loosely coupled services. Each service has a specific function and is decoupled from other services operating within its domain. If one service temporarily goes down, it won’t affect the ability of other services to continue functioning.


### Event-Driven Architecture

An event-driven workflow encompasses the ability to create, update, and asynchronously move events across various parts of the data engineering lifecycle. This workflow boils down to three main areas: `event production`, `routing`, and `consumption`. An event must be produced and routed to something that consumes it without tightly coupled dependencies among the `producer`, `event router`, and `consumer`.

### Brownfield Versus Greenfield Projects

- `Brownfield projects`: Brownfield projects often involve refactoring and reorganizing an existing architecture and are constrained by the choices of the present and past. A popular alternative to a direct rewrite is the strangler pattern: new systems slowly and incrementally replace a legacy architecture’s components.20 Eventually, the legacy architecture is completely replaced. The attraction to the strangler pattern is its targeted and surgical approach of deprecating one piece of a system at a time. This allows for flexible and reversible decisions while assessing the impact of the deprecation on dependent systems.

- `Greenfield projects`: a greenfield project allows you to pioneer a fresh start, unconstrained by the history or legacy of a prior architecture. Greenfield projects tend to be easier than brownfield projects, and many data architects and engineers find them more fun! You have the opportunity to try the newest and coolest tools and architectural patterns. 


### Examples and Types of Data Architecture


- `Data Warehouse`:

 A data warehouse is a central data hub used for reporting and analysis. Data in a data warehouse is typically highly formatted and structured for analytics use cases. It’s among the oldest and most well-established data architectures.

    - organizational data warehouse architecture: The organizational data warehouse architecture organizes data associated with certain business team structures and processes.
        - Separates online analytical processing (OLAP) from production databases (online trans‐ action processing)
        - Centralizes and organizes data.


    - technical data warehouse architecture: The technical data warehouse architec‐ ture reflects the technical nature of the data warehouse, such as MPP. 

    - The cloud data warehouse: Cloud data warehouses represent a significant evolution of the on-premises data warehouse architecture and have thus led to significant changes to the organiza‐ tional architecture. 

    - Data marts: A data mart is a more refined subset of a warehouse designed to serve analytics and reporting, focused on a single suborganization, department, or line of business; every department has its own data mart, specific to its needs.

    Data marts exist for two reasons. First, a data mart makes data more easily accessible to analysts and report developers. Second, data marts provide an additional stage of transformation beyond that provided by the initial ETL or ELT pipelines.

![Data marts](/chap3/q9UGw.png)


- `Data Lake`: 

Instead of imposing tight structural limitations on data, why not simply dump all of your data—structured and unstructured—into a central location? The data lake promised to be a democratizing force, liberating the business to drink from a fountain of limitless data. Data lake allows an immense amount of data of any size and type to be stored.


### History of Data Architectures

`Background on Data Warehouses`

Data warehouses have a long history in decision support and business intelligence applications, though were not suited or were expensive for handling unstructured data, semi-structured data, and data with high variety, velocity, and volume.


`Emergence of Data Lakes`

Data lakes then emerged to handle raw data in a variety of formats on cheap storage for data science and machine learning, though lacked critical features from the world of data warehouses: they do not support transactions, they do not enforce data quality, and their lack of consistency/isolation makes it almost impossible to mix appends and reads, and batch and streaming jobs.

`Common Two-Tier Data Architecture`

The two-tier architecture requires regular maintenance and often results in data staleness, a significant concern of data analysts and data scientists.


### Convergence, Next-Generation Data Lakes, and the Data Platform

Databricks introduced the notion of a data lakehouse. The lakehouse incorporates the controls, data management, and data structures found in a data warehouse while still housing data in object storage and supporting a variety of query and transformation engines. Some examples of data lakehouses include `Amazon Redshift Spectrum` or `Delta Lake`.

A data lakehouse is a new, open data management architecture that combines the flexibility, cost-efficiency, and scale of data lakes with the data management and ACID transactions of data warehouses, enabling business intelligence (BI) and machine learning (ML) on all data.

![Data Lakehouse](/chap3/data-lakehouse-new.png)


There are a few key technology advancements that have enabled the data lakehouse:

- metadata layers for data lakes
- new query engine designs providing high-performance SQL execution on data lakes
- optimized access for data science and machine learning tools.

The data lakehouse supports atomicity, consistency, isolation, and durability (ACID) transactions, a big departure from the original data lake, where you simply pour in data and never update or delete it. The term data lakehouse suggests a convergence between data lakes and data warehouses.

Instead of choosing between a data lake or data warehouse architecture, future data engineers will have the option to choose a converged data platform based on a variety of factors, including vendor, ecosystem, and relative openness.


#### Modern Data Stack

The main objective of the modern data stack is to use cloud-based, plug-and-play, easy-to-use, off-the-shelf components to create a modular and cost-effective data architecture. 


#### The Dataflow Model and Unified Batch and Streaming

The core idea in the Dataflow model is to view all data as events, as the aggregation is performed over various types of windows. Ongoing real-time event streams are unbounded data. Data batches are simply bounded event streams, and the boundaries provide a natural window. Engineers can choose from various windows for real-time aggregation, such as sliding or tumbling. Real-time and batch processing happens in the same system using nearly identical code.


#### Data Mesh

The data mesh attempts to invert the challenges of centralized data architecture, taking the concepts of domain-driven design (commonly used in software architectures) and applying them to data architecture. A data mesh architecture effectively unites the disparate data sources and links them together through centrally managed data sharing and governance guidelines. 


key components of the data mesh:

• Domain-oriented decentralized data ownership and architecture
• Data as a product
• Self-serve data infrastructure as a platform
• Federated computational governance


#### Other Data Architecture Examples

Data architectures have countless other variations, such as data fabric, data hub, scaled architecture, metadata-first architecture, event-driven architecture, live data stack, and many more. And new architectures will continue to emerge as practices consolidate and mature, and tooling simplifies and improves. We’ve focused on a handful of the most critical data architecture patterns that are extremely well established, evolving rapidly, or both.

