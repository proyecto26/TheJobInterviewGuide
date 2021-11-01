# Design Exercises

- Define Actors/Stakeholders:
  - Customers: anyone interested in purchasing their products.
  - Merchants/Providers
  - Third party systems (sources of truth).
- Define limitations and trade-offs:
  - Design decisions (weaknesses & strengths).
  - Number of write/read Transactions per day/month.
- Create a high level system design:
  - Constraints and Assumptions.
  - Limitations & Unknowns.
  - Out of scope.
  - Define Non-functional and functional requirements.
  - Define Risks and Strategies (Bottlenecks, Restrictions, delimit edges, Troubleshooting scenarios)
  - Define Deployment Strategy: Treat configuration as code (Ansible, Terraform, Jenkins, Docker and Kubernetes) and Deploy services independently.
  - Scaling to enforce high cohesion and loose coupling (Redundancy / Consistency).
  - Availability patterns: **Fail-over** (load balancer, managing traffic and DNS using public IPs or CNAME records), **Database Replication** (Master-slave, master-master), Federation, Sharding, etc
  - Consider the **type of data**: Put transactional data into SQL, put JSON documents into a document database, put telemetry data into a time series data base, put application logs in Elasticsearch, and put blobs in Azure Blob Storage.
  - Performance: use asynchronous messaging with a pub/sub architecture (Kibana) and backend services using RPC-style messaging protocol.
  - Monitoring: use distributed tracing (NewRelic, Splunk) to identify the cause of the failure (Traces should include a correlation ID).

## Amazon Dashboard

- Define Objective/Requeriments
  * Create a Search textbox for products
- Define Limitations
  * Ranking system
  * Number of products (database)
  * Number of categories (database)
  * Number of write Transactions per month
  * Number or read Transactions per month
- Out of Scope
  * Suggestions
  * Infinite scrolling
- Actions/Actors
  * Users (JWT roles)

## Facebook Fake Accounts

- Design a system that allow people to report fake accounts on Facebook

## Remember
- Are there any details I need?
- Are there any restrictions?
- What are all the major parts?
- Then, refine as much as possible.

## Credits:
- [The System Design Primer](https://github.com/donnemartin/system-design-primer)
- [Ten design principles for Azure applications](https://docs.microsoft.com/en-us/azure/architecture/guide/design-principles)
- [Evaluating Software Architectures for Real-Time Systems](https://www.researchgate.net/publication/220300661_Evaluating_Software_Architectures_for_Real-Time_Systems)
- [Architect's Guide to Frontend Frameworks](https://youtu.be/HI2vFGxiwkM)
- [BrowserDiet](https://browserdiet.com) - How to lose weight in the browser
- [System design: Como empezar el diseño de un sistema](https://youtu.be/kZNr1RfHhow)
