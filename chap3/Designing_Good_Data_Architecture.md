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

        - `Multi-tier`:


            `Additional Considerations`:
            - N-tier architectures are not restricted to three tiers. For more complex applications, it is common to have more tiers.
            - Tiers are the boundary of scalability, reliability, and security. Consider having separate tiers for services with different requirements in those areas.
            - Use virtual machine scale sets for autoscaling.
            - Look for places in the architecture where you can use a managed service without significant refactoring. In particular, look at caching, messaging, storage, and databases.
            - For higher security, place a network DMZ in front of the application

![n-tier architecture](/chap3/q9UGw.png)


    - `Monoliths`:



    - `Microservices`: 





















