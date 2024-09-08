## Data Landscape

Data Landscape : https://mattturck.com/wp-content/uploads/2020/09/2020-Data-and-AI-Landscape-Matt-Turck-at-FirstMark-v1.pdf
Matt Turck: https://mattturck.com/data2020/

The general idea behind the modern stack is the same as with older technologies:  building a data pipeline where you first extract data from a bunch of different sources, store it in a centralized data warehouse, and then analyze and visualize it. But the big shift has been the enormous scalability and elasticity of cloud data warehouses (Amazon Redshift, Snowflake, Google BigQuery, Microsoft Synapse, in particular).  They have become the cornerstone of the modern, cloud-first data stack and pipeline.

![Modern Data Stack](/chap4/modern-data-stack-2048x624.webp)

In the modern data pipeline, you can extract large amounts of data from multiple data sources, dump it all in the data warehouse without worrying about scale or format, and *then* transform the data directly inside the data warehouse – in other words, extract, load and transform (“ELT”). A new generation of tools has emerged to enable this evolution from ETL to ELT.  For example, DBT is an increasingly popular command line tool that enables data analysts and engineers to transform data in their warehouse more effectively.



### Choosing Technologies Across the Data Engineering Lifecycle

Data Architecture is the high-level design, roadmap, and blueprint of data systems that satisfy the strategic aims for the business. Architecture is the what, why, and when. Tools are used to make the architecture a reality; tools are the how.

We often see teams going “off the rails” and choosing technologies before mapping out an architecture. The reasons vary: shiny object syndrome, resume-driven development, and a lack of expertise in architecture. We strongly advise against choosing technology before getting your architecture right. Architecture first, technology second.


The following are some considerations for choosing data technologies across the data engineering lifecycle:

- Team size and capabilities: There is a continuum of simple to complex technologies, and a team’s size roughly determines the amount of bandwidth your team can dedicate to complex solutions. We sometimes see small data teams read blog posts about a new cutting-edge tech‐ nology at a giant tech company and then try to emulate these same extremely com‐ plex technologies and practices. We call this cargo-cult engineering, and it’s generally a big mistake that consumes a lot of valuable time and money, often with little to nothing to show in return. 


- `Speed to market`: In technology, speed to market wins. This means choosing the right technologies that help you deliver features and data faster while maintaining high-quality standards and security. It also means working in a tight feedback loop of launching, learning, iterating, and making improvements.

Slow decisions and output are the kiss of death to data teams. We’ve seen more than a few data teams dissolve for moving too slow and failing to deliver the value they were hired to produce.


- `Interoperability`: Interoperability describes how various technologies or systems connect, exchange information, and interact. Always be aware of how simple it will be to connect your various technologies across the data engineering lifecycle. We suggest designing for modularity and giving yourself the ability to easily swap out technologies as new practices and alternatives become available.


- `Cost optimization and business value`: Your organization expects a positive ROI from your data projects, so you must understand the basic costs you can control. Technology is a major cost driver, so your technology choices and management strategies will significantly impact your budget.


- `Today versus the future: immutable versus transitory technologies`: In a domain like data engineering, it’s all too easy to focus on a rapidly evolving future while ignoring the concrete needs of the present. The intention to build a better future is noble but often leads to overarchitecting and overengineering. Tooling chosen for the future may be stale and out-of-date when this future arrives; the future frequently looks little like what we envisioned years before.

You should choose the best technology for the moment and near future, but in a way that supports future unknowns and evolution. 

Given the rapid pace of tooling and best-practice changes, we suggest evaluating tools every two years. Whenever possible, find the immutable technologies along the data engineering lifecycle, and use those as your base. Build transitory tools around the immutables.


- `Location (cloud, on prem, hybrid cloud, multicloud)`: Companies now have numerous options when deciding where to run their technol‐ ogy stacks. A slow shift toward the cloud culminates in a veritable stampede of companies spinning up workloads on AWS, Azure, and Google Cloud Platform (GCP). 

    - On Premises
    - Cloud
    - Hybrid Cloud
    - Multicloud


- `Build versus buy`: Build versus buy is an age-old debate in technology. The argument for building is that you have end-to-end control over the solution and are not at the mercy of a vendor or open source community. The argument supporting buying comes down to resource constraints and expertise; do you have the expertise to build a better solution than something already available? Either decision comes down to TCO, TOCO, and whether the solution provides a competitive advantage to your organization.

Build versus buy comes back to knowing your competitive advantage and where it makes sense to invest resources toward customization. In general, we favor OSS and COSS by default, which frees you to focus on improving those areas where these options are insufficient. Focus on a few areas where building something will add significant value or reduce friction substantially.


- `Monolith versus modular`: Monoliths versus modular systems is another longtime debate in the software archi‐ tecture space. Monolithic systems are self-contained, often performing multiple func‐ tions under a single system. The monolith camp favors the simplicity of having everything in one place. The modular camp leans toward decoupled, best-of-breed technologies performing tasks at which they are uniquely great. 

Here are some things to consider when evaluating monoliths versus modular options:
    - Interoperability
    - Avoiding the “bear trap”
    - Flexibility


- `Serverless versus servers`: A big trend for cloud providers is serverless, allowing developers and data engineers to run applications without managing servers behind the scenes. Serverless provides a quick time to value for the right use cases. 

    - Serverless: serverless has been around for quite some time, with the promise of executing small chunks of code on an as-needed basis without having to manage a server, serverless exploded in popularity. The main reasons for its popularity are cost and convenience. Instead of paying the cost of a server, why not just pay when your code is evoked?

    - Containers: Containers are often referred to as lightweight virtual machines. Serverless environments typically run on containers behind the scenes. Indeed, Kubernetes is a kind of server‐ less environment because it allows developers and ops teams to deploy microservices without worrying about the details of the machines where they are deployed.


- `Optimization, performance, and the benchmark wars`

- `The undercurrents of the data engineering lifecycle`


### Conclusion

Choosing the right technologies is no easy task, especially when new technologies and patterns emerge daily. Today is possibly the most confusing time in history for evaluating and selecting technologies. Choosing technologies is a balance of use case, cost, build versus buy, and modularization. Always approach technology the same way as architecture: assess trade-offs and aim for reversible decisions.