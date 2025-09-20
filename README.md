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


1. AWS Aurora PostgreSQL vs. AWS RDS for PostgreSQL
PROS (+)
  * Benefit: Superior Performance Architecture. Aurora's cloud-native, decoupled storage layer is specifically engineered for high throughput and low-latency I/O, making the consistent ~1ms performance target achievable. While RDS relies on network-attached EBS storage, which introduces inherent latency. Even the fastest io2 EBS volumes typically have latencies in the low milliseconds, making it extremely difficult to consistently stay under the 1ms threshold, especially for write operations and uncached reads under load.
CONS (-)
  * Problem: Higher Financial Cost. Aurora has a more complex and expensive pricing model based on instance size, storage consumption, and I/O operations, leading to a higher monthly bill compared to standard RDS.

2. AWS Aurora PostgreSQL vs. Self-Managed PostgreSQL on EC2
PROS (+)
  * Benefit: Reduced Operational Complexity. As a fully managed service, Aurora automates provisioning, patching, backups, and, most critically, high-availability failover, freeing developers from database administration tasks.
CONS (-)
  * Problem: Less Granular Control. Choosing Aurora means accepting AWS's managed path, which involves less direct control over underlying OS and database engine tuning compared to a self-managed instance on EC2.

3. AWS Aurora PostgreSQL vs. Apache Cassandra
PROS (+)
  * Benefit: Fully Managed by AWS. AWS handles all underlying infrastructure, including provisioning, patching, backups, failure detection, and failover. The management interface is integrated into the AWS console, CLI, and APIs, providing a seamless experience with other AWS services. This drastically reduces the operational burden on your team.
CONS (-)
  * Problem: Managed Service Constraints. You trade control for convenience. You have limited access to the underlying OS and cannot fine-tune every low-level database or system parameter. You are also dependent on AWS's timeline for support of new database versions and features.


5. Network
AWS EKS
PROS (+)
  * Benefit: Managed Kubernetes reduces operational burden by handling control plane provisioning, scaling, patching, and upgrades automatically. Integrates natively with AWS networking, security, and monitoring services.

CONS (-)
  * Problem: Extra cost for control plane and managed services; networking setup (CNI, service mesh) can be complex, especially for multi-AZ or hybrid deployments.

AWS API GATEWAY
PROS (+)
  * Benefit: Provides a fully managed API layer with built-in authentication, throttling, caching, and monitoring, automatically handling high request volumes without infrastructure scaling.

CONS (-)
  * Problem: Adds network latency (~10–50ms per request), which may impact ultra-low-latency requirements; pricing can grow with traffic; limited low-level network control.

MULTI TENANT ARCHITECTURE
PROS (+)
  * Benefit: Enables logical isolation of tenants, reducing operational cost by sharing infrastructure while maintaining security. Centralized network policies and monitoring simplify management.

CONS (-)
  * Problem: Implementing strict isolation and traffic policies is complex; misconfigurations can lead to data leakage; scaling to many tenants may require careful IP and cluster planning.

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


## Report Service
### 6.1 - Report Service Class Diagram
here go the image

### 6.2 - Report Service Contract Documentation
- ReportService
  * createReport(params: Map) → Input: params (filters, type, user) → Output: Report
  * getReportById(id: UUID) → Input: report ID → Output: Report
  * listReports() → Input: none → Output: List of Reports

- ReportGenerator
  * generate(data: Any, format: Format) → Input: raw data, desired format → Output: Report

- ReportFormatter
  * formatAsPDF(report: Report) → Input: Report → Output: PDF File
  * formatAsCSV(report: Report) → Input: Report → Output: CSV File
  * formatAsHTML(report: Report) → Input: Report → Output: HTML File

- ReportRepository
  * save(report: Report) → Input: Report → Output: void
  * findById(id: UUID) → Input: report ID → Output: Report
  * findAll() → Input: none → Output: List of Reports

- User
  * requestReport(params: Map) → Input: parameters → Output: Report

### 6.3 - Report Service Persistence Model
here go the image

Main Queries
Insert:
  * INSERT INTO reports (id, title, content, created_at) VALUES (?, ?, ?, ?)

Find by ID:
  * SELECT * FROM reports WHERE id = ?

List All:
  * SELECT * FROM reports ORDER BY created_at DESC

### 6.4 - Report Service Algorithms / Data Structures
Algorithms
  * Formatting:
    * PDF generation: simple template-based rendering (e.g., iText or jsPDF).
    * CSV: string serialization with delimiter.
    * HTML: lightweight template rendering.

  * Search & Retrieval:
    * basic indexed lookups by id and created_at.

Data Structures
  * Map: for dynamic params when creating reports.
  * List: to store multiple reports in memory when listing.
  * UUID: unique identifier for Report and User.
  * DateTime: for timestamps.
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

- Language: Java 23
- Framework:	Spring Boot 3
- Container: Docker
- Orchestration:	Kubernetes
- API: REST
- Auth: OAuth2 + JWT
- Databases: PostgreSQL + Redis
- Messaging: Kafka
- CI/CD:	GitHub Actions
- Monitoring: Prometheus + Grafana
- Logging: Loki
- Tracing: Xray


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
