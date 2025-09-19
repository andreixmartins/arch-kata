# 🧬 ARCHITECTURE KATA


## 🏛️ Structure

### 1. 🎯 Problem Statement and Context

What is the problem? What is the context of the problem?
Example:
```
MR Bill, wants a system to keep track of his favorite pocs, so you need to build a mobile app where mr Bill can register all his pocs, and also  
he needs to be able to search pocs, by name, by language,m by tags.
This system should be multi-tenant because mr bill will sell such system to.
Bunch of people in brazil, such system must have also ability to generate repots and generate a video with the all pocs
the users did in 1 year. Such system must be secure and have proper login and be able to support Realtime dojos using mr bill platform you will build for him.

1. Track favorites POCs
2. Mobile app to register POC
3. Searching POCs by name, language and tags
4. Multi-tentant system
5. Generate reports
6. Generate POC videos
7. System must be secure
8. Users will have their credentials


```
Recomended Reading: http://diego-pacheco.blogspot.com/2021/10/breaking-problems-down.html

### 2. 🎯 Goals

List in form of bullets what goals do have. Here it's great to have 5-10 lines.
Example:
```
1. Solution needs to be fast! Performance for all operations bellow ~1 ms.
2. Security is non-negociable! Security at-rest, transite, threat analysis and review for by at least 3 different people.
3. Mobile-native: We want a native app using Swift and Kotlin 
4. Cloud-Native: All backend must be 100% cloud native, using AWS Cloud services
5. Microservices: Do not use AWS Lambdas
6. Cache: AWS Elastic Cache to cache user data information
7. Tests: Employ unit tests, integration tests, regression tests, chaos tests
8. Microservices: App services must use the microservice architecture
9. Amazon OpenSearch: Realtime application monitoring and security Analytics
10. Amazon RDS: Postgres for relational data 
```
Recommended Learning: http://diego-pacheco.blogspot.com/2020/05/education-vs-learning.html

### 3. 🎯 Non-Goals

List in form of bullets what non-goals do have. Here it's great to have 5-10 lines.
Example:
```
1. Be perfect: There will be mistakes, we dont want have automatic-rollback for everything.
2. DynamoDB: Dynamo is expensive, we want be away from the DB.
3. AWS Lambdas: We do not want to use it.
4. Monoliths: We must not use monolith architecture
5. Single Relational Database: We must use relational database distributed in different AZ
6. Other Cloud Services: We must only use AWS Services
7. AWS Cloudwatch: We must not use Cloudwatch
```
Recommended Reading: http://diego-pacheco.blogspot.com/2021/01/requirements-are-dangerous.html

### 📐 3. Principles

List in form of bullets what design principles you want to be followed, it's great to have 5-10 lines.
Example:
```
1. Low Coupling: We need to watch for coupling all times.
2. Flexibility: Users should be able to customize behavior without leaking the internals of the system. Leverage interfaces.
3. Observability: we should expose all key metrics on main features. Sucess and errors counters need to be exposed.
4. Testability: Chaos engineering is a must and property testing. Testing should be done by engineers all times.
5. Cache efficiency: Should leverage SSD caches and all forms of caches as much as possible.
6. Testability: We must apply unit-test, regression-tests, chaos tests
7. Scalability: Handle amounts of work, data, or users without performance degradation
8. Availability: Ensure a high level of uptime 99.9%
```
Recommended Reading: http://diego-pacheco.blogspot.com/2018/01/stability-principles.html

### 🏗️ 4. Overall Diagrams

Here there will be a bunch of diagrams, to understand the solution.
```
🗂️ 4.1 Overall architecture: Show the big picture, relationship between macro components.
🗂️ 4.2 Deployment: Show the infra in a big picture. 
🗂️ 4.3 Use Cases: Make 1 macro use case diagram that list the main capability that needs to be covered. 
```
Recommended Reading: http://diego-pacheco.blogspot.com/2020/10/uml-hidden-gems.html

### 🧭 5. Trade-offs

List the tradeoffs analysis, comparing pros and cons for each major decision.
Before you need list all your major decisions, them run tradeoffs on than.
example:
Major Decisions: 
```
1. One mobile code base - should be (...)
2. Reusable capability and low latency backends should be (...)
3. Cache efficiency therefore should do (...)
4. Prioritize extreme performance (~1ms latency) for all database operations.
5. Choose a fully managed service to ensure high availability and minimize operational overhead.
```
Tradeoffs:
```
1. React Native vs (Flutter and Native)
2. Serverless vs Microservices
3. Redis vs Enbeded Caches
```
Each tradeoff line need to be:
```
PROS (+) 
  * Benefit: Explanation that justify why the benefit is true.
CONS (+)
  * Problem: Explanation that justify why the problem is true.

1. Amazon Keyspaces vs. AWS RDS Aurora (PostgreSQL-Compatible)
PROS (+)
- Complexity: Keyspaces is fully serverless, no instance sizing or cluster design. Aurora requires careful sizing of compute and storage.
- Scalability: Keyspaces auto-scales throughput and storage; Aurora requires manual scaling or Aurora Serverless v2 (with limitations).
- Availability: Keyspaces handles multi-AZ replication automatically, while Aurora requires managing failover events.
- Cost: Pay-per-request pricing in Keyspaces is cost-efficient for spiky workloads; Aurora incurs continuous instance charges.
- Setup & Maintenance: Keyspaces has zero maintenance overhead; Aurora requires DB parameter tuning and I/O optimization.
CONS (–)
- Performance: Aurora excels for real-time transactional workloads with strong consistency; Keyspaces is optimized for partition-based, high-volume workloads like time-series or IoT.
- Limitations: Keyspaces does not support joins, triggers, or advanced SQL features; Aurora supports full relational SQL.
- Latency: Aurora can provide sub-millisecond local reads; Keyspaces has higher network-based latencies.
- Disk Usage Overhead: Aurora’s storage layer is efficient; Keyspaces stores data redundantly across zones, potentially increasing storage costs.
- Observability / Monitoring: Aurora integrates with Performance Insights; Keyspaces relies on CloudWatch metrics with less granularity.

2. Amazon Keyspaces vs. AWS RDS for PostgreSQL
PROS (+)
- Complexity: Keyspaces eliminates all infrastructure decisions; RDS requires provisioning instances, storage, and replication.
- Scalability: Keyspaces scales seamlessly with workload; RDS scaling is tied to instance classes and storage resizing.
- Availability: Keyspaces automatically replicates across multiple AZs; RDS uses Multi-AZ failover with 60–120s downtime.
- Setup & Maintenance: Keyspaces has no patching or backups; RDS still requires operational planning.
- Cost: Pay-per-use pricing benefits variable workloads; RDS pricing is predictable for steady workloads.
CONS (–)
- Performance: RDS is optimized for real-time transactional workloads with strong ACID compliance; Keyspaces provides tunable consistency but not full ACID.
- Limitations: Keyspaces does not support relational queries, foreign keys, or complex joins; RDS supports full SQL and PL/pgSQL.
- Latency: Keyspaces latencies are typically higher than RDS for single-row transactional lookups.
- Disk Usage Overhead: RDS lets you control EBS volume type; Keyspaces abstracts storage, potentially increasing costs at scale.
- Observability / Monitoring: RDS integrates with CloudWatch, Enhanced Monitoring, and Performance Insights; Keyspaces only provides CloudWatch metrics with limited query profiling.

3. Amazon Keyspaces vs. Apache Cassandra (Self-Managed on AWS)
PROS (+)
- Complexity: Keyspaces removes all cluster design, node scaling, and replication management; Cassandra requires expertise to configure ring topology.
- Scalability: Keyspaces scales automatically; Cassandra scaling requires manual node provisioning and balancing.
- Availability: Keyspaces ensures multi-AZ fault tolerance by default; Cassandra requires manual replication factor setup.
- Performance: Keyspaces delivers predictable performance without compaction overhead; Cassandra can suffer from write amplification and compaction stalls.
- Cost: Keyspaces avoids EC2/EBS and admin costs; Cassandra adds infra plus significant operational costs.
- Setup & Maintenance: Keyspaces is zero-maintenance; Cassandra requires ops for backups, patches, upgrades, and monitoring.
- Latency: Keyspaces provides consistent regional latency; Cassandra may give lower latency only with carefully tuned local clusters.
- Observability / Monitoring: Keyspaces integrates with CloudWatch automatically; Cassandra requires deploying and managing monitoring tools.
CONS (–)
- Limitations: Keyspaces has fewer tunables; Cassandra allows deep control over consistency, partitioning, compaction, and caching strategies.
- Disk Usage Overhead: Cassandra allows storage-level tuning; Keyspaces abstracts storage, potentially increasing costs for large datasets.
- Vendor Lock-in: Keyspaces ties you to AWS; Cassandra runs on any cloud or on-prem.

```
PS: Be careful to not confuse problem with explanation. 
<BR/>Recommended reading: http://diego-pacheco.blogspot.com/2023/07/tradeoffs.html

### 🌏 6. For each key major component

What is a majore component? A service, a lambda, a important ui, a generalized approach for all uis, a generazid approach for computing a workload, etc...
```
6.1 - Class Diagram              : classic uml diagram with attributes and methods
6.2 - Contract Documentation     : Operations, Inputs and Outputs
6.3 - Persistence Model          : Diagrams, Table structure, partiotioning, main queries.
6.4 - Algorithms/Data Structures : Spesific algos that need to be used, along size with spesific data structures.
```

Exemplos of other components: Batch jobs, Events, 3rd Party Integrations, Streaming, ML Models, ChatBots, etc... 

Recommended Reading: http://diego-pacheco.blogspot.com/2018/05/internal-system-design-forgotten.html

### 🖹 7. Migrations

IF Migrations are required describe the migrations strategy with proper diagrams, text and tradeoffs.

### 🖹 8. Testing strategy

Explain the techniques, principles, types of tests and will be performaned, and spesific details how to mock data, stress test it, spesific chaos goals and assumptions.

### 🖹 9. Observability strategy

Explain the techniques, principles,types of observability that will be used, key metrics, what would be logged and how to design proper dashboards and alerts.

### 🖹 10. Data Store Designs

For each different kind of data store i.e (Postgres, Memcached, Elasticache, S3, Neo4J etc...) describe the schemas, what would be stored there and why, main queries, expectations on performance. Diagrams are welcome but you really need some dictionaries.

### 🖹 11. Technology Stack

Describe your stack, what databases would be used, what servers, what kind of components, mobile/ui approach, general architecture components, frameworks and libs to be used or not be used and why.

### 🖹 12. References

* Architecture Anti-Patterns: https://architecture-antipatterns.tech/
* EIP https://www.enterpriseintegrationpatterns.com/
* SOA Patterns https://patterns.arcitura.com/soa-patterns
* API Patterns https://microservice-api-patterns.org/
* Anti-Patterns https://sourcemaking.com/antipatterns/software-development-antipatterns
* Refactoring Patterns https://sourcemaking.com/refactoring/refactorings
* Database Refactoring Patterns https://databaserefactoring.com/
* Data Modelling Redis https://redis.com/blog/nosql-data-modeling/
* Cloud Patterns https://docs.aws.amazon.com/prescriptive-guidance/latest/cloud-design-patterns/introduction.html
* 12 Factors App https://12factor.net/
* Relational DB Patterns https://www.geeksforgeeks.org/design-patterns-for-relational-databases/
* Rendering Patterns https://www.patterns.dev/vanilla/rendering-patterns/
* REST API Design https://blog.stoplight.io/api-design-patterns-for-rest-web-services
