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


Amazon Keyspaces
PROS (+)
  * Multi-Tenant Isolation - Native partition-based isolation ensures complete tenant data separation without complex application logic.
  * Auto-Scaling - Seamlessly handles variable workloads across tenants without manual intervention or capacity planning.
  * Zero Maintenance - Fully managed service eliminates patching, backups, and cluster management overhead.
  * High Availability - Built-in multi-AZ replication with automatic failover provides 99.99% availability SLA.
  * Cost Efficiency - Pay-per-request pricing model scales cost with actual usage, ideal for multi-tenant SaaS applications.
CONS (-)
  * Query Limitations - No support for complex joins or aggregations limits analytical capabilities for reporting features.
  * Eventual Consistency - Default eventual consistency may cause temporary data inconsistencies in real-time scenarios.
  * Vendor Lock-in - AWS-specific service creates dependency on AWS ecosystem and CQL dialect.
  * Cold Start Latency - Initial queries after idle periods may experience higher latency due to distributed nature.
  * Limited Indexing - Secondary indexes have performance implications and query restrictions compared to relational databases.

AWS RDS Aurora
PROS (+)
  * ACID Compliance - Full transactional consistency ensures data integrity for critical business operations and financial transactions.
  * Compatibility - Complete PostgreSQL compatibility enables complex queries, joins, and existing application migration.
  * Performance - Sub-millisecond read latencies with read replicas support high-throughput analytical workloads.
  * Advanced Features - Supports stored procedures, triggers, and advanced SQL features for complex business logic.
  * Monitoring - Performance Insights provides detailed query-level monitoring and optimization recommendations.
CONS (-)
  * Operational Complexity - Requires instance sizing, connection pooling, and performance tuning expertise.
  * Multi-Tenant Challenges - Requires application-level tenant isolation and careful schema design to prevent data leakage.
  * Scaling Limitations - Vertical scaling has limits; horizontal scaling requires read replicas and application changes.
  * Cost Predictability - Fixed instance costs regardless of usage patterns may be expensive for variable workloads.
  * Maintenance Windows - Requires planned downtime for patches and major version upgrades.

AWS RDS for PostgreSQL
PROS (+)
  * Mature Ecosystem - Extensive PostgreSQL ecosystem with proven libraries, tools, and community support.
  * Rich Data Types - Native support for JSON, arrays, and custom types enables flexible data modeling.
  * Full SQL Support - Complete SQL standard compliance with advanced features like window functions and CTEs.
  * Backup & Recovery - Automated backups with point-in-time recovery provide robust data protection.
  * Security Features - Row-level security and advanced authentication options support complex multi-tenant security models.
CONS (-)
  * Manual Scaling - Requires manual intervention for scaling up/down and lacks automatic capacity adjustment.
  * Single Point of Failure - Standard RDS has potential downtime during failover events in Multi-AZ deployments.
  * Resource Contention - Shared resources between tenants can lead to noisy neighbor problems affecting performance.
  * Storage Limitations - EBS storage has IOPS limits that may constrain high-throughput applications.
  * Maintenance Overhead - Requires regular maintenance, parameter tuning, and performance monitoring.

S3 (Amazon Simple Storage Service)
PROS (+)
  * Scalability - Highly scalable, can handle vast amounts of data.
  * Low maintainability - Managed service by AWS, reducing the need for in-house maintenance.
  * Integration - Seamlessly integrates with other AWS services.
  * Global Availability - Data can be accessed from anywhere in the world with low latency.
  * Cost-Effective - Pay-as-you-go pricing model, which can be economical for many use cases.

CONS (-)
  * Cost - Can become expensive with high data retrieval and transfer rates.
  * Vendor Lock-in - Ties you to the AWS ecosystem, making migration to other platforms challenging.
  * Limited Customization - Less flexibility in terms of configuration and customization compared to self-managed solutions.
  * Latency - May have higher latency for certain applications compared to local storage solutions.
  * Data Transfer Costs - Charges for data transfer out of S3 can add up, especially for high-traffic applications.

4. Splunk vs Loki vs CloudWatch
Splunk
PROS (+)
  * Provides advanced analytics and full-text search capabilities through SPL (Search Processing Language), which makes it ideal for complex queries and security/compliance use cases.
  * Flexible deployment options (on-premises, hybrid, or cloud), giving organizations control over their infrastructure strategy.
  * Mature ecosystem with dashboards, alerting, and SIEM(Security Information and Event Management) features that cover enterprise needs out of the box.

CONS (−)
  * Licensing and data ingestion/storage costs scale steeply, making Splunk very expensive at high log volumes.
  * Requires significant operational overhead for scaling and managing indexes, even in Splunk Cloud environments.
  * Vendor lock-in due to proprietary ecosystem and pricing model.

Grafana Loki
PROS (+)
  * Benefit: Open-source and cost-efficient because only metadata (labels + timestamp) is indexed, reducing storage and compute costs.
  * Benefit: Integrates seamlessly with Grafana, Prometheus, and Kubernetes, making it a natural fit for cloud-native observability stacks.
  * Benefit: Scales horizontally and is well-suited for high-volume logs with structured labels.

CONS (−)
  * Weaker full-text search compared to Splunk, since only metadata is indexed.
  * Requires self-hosting or managed services, which introduces infrastructure and operational overhead.
  * Lacks some enterprise-grade features (security/compliance, SIEM(Security Information and Event Management)) without additional tooling.

Amazon CloudWatch
PROS (+)
  * Fully managed service with minimal operational overhead, tightly integrated with AWS ecosystem (EC2, Lambda, RDS, etc.).
  * Pay-as-you-go pricing model with a free tier, making it easy to adopt and scale within AWS.
  * Provides unified metrics, logs, alarms, and dashboards in one platform, reducing integration complexity.

CONS (−)
  * Costs can escalate quickly with high log ingestion, retention, or custom metrics usage.
  * Limited flexibility and portability—mainly useful for AWS workloads, not multi-cloud or on-premises.
  * Log search and query capabilities (CloudWatch Logs Insights) are less powerful and slower compared to Splunk for complex analytics.


  * Problem: Managed Service Constraints. You trade control for convenience. You have limited access to the underlying OS and cannot fine-tune every low-level database or system parameter. You are also dependent on AWS's timeline for support of new database versions and features.


5. Network
AWS EKS
PROS (+)
  *  Managed Kubernetes reduces operational burden by handling control plane provisioning, scaling, patching, and upgrades automatically. Integrates natively with AWS networking, security, and monitoring services.

CONS (-)
  * Problem: Extra cost for control plane and managed services; networking setup (CNI, service mesh) can be complex, especially for multi-AZ or hybrid deployments.

AWS API GATEWAY
PROS (+)
  * Provides a fully managed API layer with built-in authentication, throttling, caching, and monitoring, automatically handling high request volumes without infrastructure scaling.

CONS (-)
  * Adds network latency (~10–50ms per request), which may impact ultra-low-latency requirements; pricing can grow with traffic; limited low-level network control.

MULTI TENANT ARCHITECTURE
PROS (+)
  * Enables logical isolation of tenants, reducing operational cost by sharing infrastructure while maintaining security. Centralized network policies and monitoring simplify management.

CONS (-)
  * Problem: Implementing strict isolation and traffic policies is complex; misconfigurations can lead to data leakage; scaling to many tenants may require careful IP and cluster planning.

```
PS: Be careful to not confuse problem with explanation.

```
## Dojo Session

**LiveKit**

**PROS (+)**

- **Compatibility**: Provides built-in signaling, SFU logic, room management, track control, adaptive simulcast, user roles — all abstracted and ready to use with official SDKs for Web, iOS, Android, Flutter, Unity, and more.
- **Code Reusability**: Offers high-level APIs and pre-built infrastructure, which promotes reuse across different platforms and products.
- **Scalability**: Designed with cloud-native scaling in mind; Kubernetes-ready with multi-node support and autoscaling.
- **Setup and Maintenance**: Easy to deploy with prebuilt Docker images and Helm charts. Managed cloud option available (LiveKit Cloud).
- **Latency**: Low-latency architecture optimized for real-time use cases such as video conferencing, gaming, and virtual collaboration.
- **Monitoring / Observability**: Offers built-in Prometheus metrics, structured logs, and integration with Grafana dashboards.
- **Performance**: Includes optimizations like bandwidth adaptation, packet loss recovery, and adaptive streaming (simulcast, SVC).
- **Availability**: LiveKit Cloud offers high availability out of the box with managed infrastructure and SLAs.

**CONS (-)**

- **Complexity**: Abstracted logic limits low-level control. Custom media routing or deep protocol tweaks are not straightforward.
- **Cost**: LiveKit Cloud can become expensive at scale compared to self-hosted SFUs like MediaSoup.
- **Disk Usage Overhead**: Recording or archiving requires external services (like LiveKit Recorder) and adds complexity/cost.
- **Limitations**: Geared mainly toward standard RTC workflows. Less flexible for exotic use cases or novel media topologies.

**MediaSoup**

**PROS (+)**

- **Complexity / Flexibility**: Offers full control over RTP, SCTP, DTLS, and media pipeline. Ideal for custom or unconventional WebRTC architectures.
- **Cost**: Fully open-source and self-hosted; no vendor lock-in or usage-based fees.
- **Latency**: Minimal internal processing makes it highly performant with ultra-low-latency potential.
- **Performance**: Efficient, lightweight, and highly tunable. Fine-grained control over bandwidth, layers, and routing.
- **Disk Usage Overhead**: No forced dependency on recorders or file storage — you implement what you need.
- **Code Reusability**: Node.js and C++ APIs allow tight integration into custom server architectures.
- **Observability**: Raw metrics and debugging access to RTP internals allow fine performance analysis.

**CONS (-)**

- **Setup and Maintenance**: Requires significant effort to build signaling, room logic, and orchestration from scratch. No built-in user or track management.
- **Scalability**: No built-in horizontal scaling or cluster awareness — scaling needs manual implementation (e.g., via load balancers or custom SFU routers).
- **Monitoring**: Lacks integrated dashboards or metrics. Requires custom Prometheus exporters or external tooling.
- **Compatibility**: Fewer official SDKs and integrations; community-led efforts vary in quality and completeness.
- **Availability**: You must build your own HA/Failover setup — especially challenging in multi-region environments.
- **Limitations**: Steep learning curve; requires strong understanding of WebRTC internals and RTP networking.

### Final Recommendation: Use LiveKit

Why?

```
- You get battle-tested signaling, media routing, SDKs, scaling, and observability out-of-the-box.

- Perfect fit for low-latency 1:1 and small group live coding sessions.

- Lower ops burden for a small or mid-size engineering team.

- You can ship a working MVP in weeks, not months.
```


AWS Cognito
PROS (+)
  * Benefit: Easy SMS MFA Setup - Phone verification is provided out of the box with pre-built web pages and verification logic, reducing development time.
  * Benefit: Great Mobile App Support - Native Android and iOS SDKs are available, enabling seamless integration with mobile applications.
  * Benefit: Fully Managed Service - AWS handles all infrastructure, scaling, and maintenance, reducing operational overhead.
  * Benefit: AWS Ecosystem Integration - Seamless integration with other AWS services like Lambda, API Gateway, and S3 for unified security.
CONS (-)
  * Problem: Multi-Tenant Limitations - User pools are not multi-tenant by design, requiring a dedicated user pool per tenant which complicates architecture.
  * Problem: Scalability Constraints - Limited to 1000 user pools per AWS account, which severely restricts multi-tenant application scaling.
  * Problem: Limited Customization - Restricted ability to customize authentication flows and user interface compared to self-hosted solutions.
  * Problem: Vendor Lock-in - Tight integration with AWS ecosystem makes migration to other platforms or hybrid cloud approaches difficult.

Keycloak
PROS (+)
  * Benefit: Multi-Tenant Architecture - Easily configurable for multi-tenant scenarios with realm-based isolation and flexible tenant management.
  * Benefit: Open Source Flexibility - Open-source solution provides full control over customization, extensions, and deployment strategies.
  * Benefit: Cost Efficiency - Free software with only infrastructure costs, providing significant cost savings for large-scale deployments.
  * Benefit: High Customization - Highly customizable authentication flows, themes, and extensions to meet specific business requirements.
  * Benefit: Platform Agnostic - Can be deployed on any infrastructure or cloud provider, avoiding vendor lock-in.
CONS (-)
  * Problem: Operational Overhead - Self-hosted solution requires managing infrastructure, updates, security patches, and high availability setup.
  * Problem: Limited Mobile SDKs - No native mobile SDKs available, requiring custom integration work for mobile applications.
  * Problem: Complexity - More complex to set up and configure compared to managed services, requiring specialized expertise.
  * Problem: Maintenance Burden - Requires ongoing maintenance, monitoring, and troubleshooting that managed services handle automatically.
  
=======

## Application Load Balancer (ALB)

### Use Cases
- HTTP/HTTPS traffic
- Web applications, REST APIs
- Microservices or containerized applications (ECS, EKS)
- Host-based or path-based routing
- Authentication via Cognito or OIDC
- WebSocket and HTTP/2 support

### ✅ Pros
- Operates at Layer 7 (Application Layer)
- Intelligent request routing (host/path headers)
- Integrated with AWS WAF 
- Supports advanced routing (redirects, fixed responses)
- Native support for HTTP/2 and WebSockets

### ❌ Cons
- Only supports HTTP and HTTPS protocols
- Slightly higher latency than NLB
- No static IP support


## Network Load Balancer (NLB)

### Use Cases
- TCP, UDP, and TLS traffic
- Real-time applications (VoIP, gaming, financial apps)
- Applications requiring very low latency and high throughput
- Need for static IP or Elastic IP addresses
- Preserving the client IP address

### ✅ Pros
- Operates at Layer 4 (Transport Layer)
- Extremely high performance and low latency
- Handles millions of requests per second
- Static IP and Elastic IP support
- TLS termination and pass-through support

### ❌ Cons
- No application-layer routing (no path/host-based routing)
- Limited visibility into HTTP-layer traffic
- No direct integration with AWS WAF or Cognito

## Load Balancing Algorithms Used in AWS

### Application Load Balancer (ALB)

- **Round Robin (Default)**  
  Requests are distributed sequentially across targets in a target group.  
  Good for evenly sized workloads.

- **Least Outstanding Requests**  
  Requests are routed to the target with the fewest active (in-flight) requests.  
  Better for variable latency workloads.

### Network Load Balancer (NLB)

- **Flow Hash Algorithm (Default)**  
  Uses a 5-tuple hash (source IP, source port, destination IP, destination port, protocol) to consistently route traffic to the same target.  
  Preserves session affinity (*sticky sessions*) by default.

- **Source IP Affinity (Optional)**  
  Allows you to enable stickiness based on the source IP address.  
  Useful when consistent backend assignment is needed as session-based apps.


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

## Dojo Session

### Class Diagram

```plaintext
+---------------------------------+
|         RoomService             |
+---------------------------------+
| - rooms: Map<string, Room>      |
+---------------------------------+
| + createRoom(name): Room        |
| + getRoom(name): Room           |
| + closeRoom(name): void         |
+---------------------------------+

               |
               ▼

+---------------------------------+
|             Room                |
+---------------------------------+
| - name: string                  |
| - participants: Set<Participant>|
| - tracks: Map<string, Track>    |
+---------------------------------+
| + addParticipant(p): void       |
| + removeParticipant(p): void    |
| + broadcastMessage(msg): void   |
+---------------------------------+

              |
              ▼

+--------------------------------+
|         Participant            |
+--------------------------------+
| - id: string                   |
| - identity: string             |
| - role: string                 |
| - tracks: Set<Track>           |
+--------------------------------+
| + publishTrack(track): void    |
| + unpublishTrack(track): void  |
| + sendSignal(msg): void        |
+--------------------------------+

             |
             ▼

+--------------------------------+
|             Track              |
+--------------------------------+
| - id: string                   |
| - kind: 'audio' | 'video'      |
| - codec: string                |
+--------------------------------+
| + start(): void                |
| + stop(): void                 |
+--------------------------------+
```

---

### Contract Documentation

### **RoomService**

- **createRoom(name: string) → Room**
  Creates a new room instance.

- **getRoom(name: string) → Room**
  Retrieves the room by name.

- **closeRoom(name: string) → void**
  Closes the room and disconnects participants.

---

### **Room**

- **addParticipant(p: Participant) → void**
  Adds a participant to the room.

- **removeParticipant(p: Participant) → void**
  Removes a participant from the room.

- **broadcastMessage(msg: SignalMessage) → void**
  Sends a signal to all participants.

---

### **Participant**

- **publishTrack(track: Track) → void**
  Publishes a media track to the room.

- **unpublishTrack(track: Track) → void**
  Stops publishing a track.

- **sendSignal(msg: SignalMessage) → void**
  Sends a signaling message to server or other participants.

---

### Persistence Model

### **Diagrams / Table Structures**

```plaintext
Table: rooms
- id (PK)
- name
- created_at
- is_active

Table: participants
- id (PK)
- room_id (FK -> rooms.id)
- identity
- role
- joined_at
- is_connected

Table: tracks
- id (PK)
- participant_id (FK -> participants.id)
- kind
- codec
- created_at
```

### **Partitioning Strategy**

- **Rooms**: Could be partitioned by `region` or `tenant_id` if multi-tenant.
- **Participants**: Partition by `room_id` for faster lookup.
- **Tracks**: Co-located with participants for better locality.

### **Main Queries**

1. Fetch all participants in a room:

   ```sql
   SELECT * FROM participants WHERE room_id = :room_id;
   ```

2. Get active rooms:

   ```sql
   SELECT * FROM rooms WHERE is_active = true;
   ```

3. Get tracks by participant:

   ```sql
   SELECT * FROM tracks WHERE participant_id = :participant_id;
   ```

---

### Algorithms / Data Structures

### **Data Structures**

- `Map<String, Room>` for room lookup
- `Set<Participant>` in a Room for O(1) joins/leaves
- `Map<String, Track>` in Participant for quick access
- `MessageQueue` (Pub/Sub) for signaling layer (if custom signaling is used)

### **Algorithms**

- **Auto-room cleanup**:
  When the last participant leaves, schedule a timeout job to close the room (debounced).

- **Adaptive bitrate selection**:
  Based on packet loss / RTT metrics — adjust simulcast layer dynamically

- **Track Routing Algorithm** (in SFU layer):

  - Inputs: subscriber bandwidth, CPU load
  - Output: optimal video quality layer
  - Could use a simple scoring model per subscriber:

    ```python
    score = available_bandwidth / number_of_tracks
    ```

- **Participant Sorting**:
  Sort active speakers by loudness score or priority for layout/rendering:

  ```python
  sorted(participants, key=lambda p: p.audio_level, reverse=True)
  ```
  
  ### Contract Documentation

- RoomService

  - createRoom(name: string) → Room
    Creates a new room instance.

  - getRoom(name: string) → Room
    Retrieves the room by name.

  - closeRoom(name: string) → void
    Closes the room and disconnects participants.

- Room

  - addParticipant(p: Participant) → void
    Adds a participant to the room.

  - removeParticipant(p: Participant) → void
    Removes a participant from the room.

  - broadcastMessage(msg: SignalMessage) → void
    Sends a signal to all participants.

- Participant

  - publishTrack(track: Track) → void
    Publishes a media track to the room.

  - unpublishTrack(track: Track) → void
    Stops publishing a track.

  - sendSignal(msg: SignalMessage) → void
    Sends a signaling message to server or other participants.

---

Exemplos of other components: Batch jobs, Events, 3rd Party Integrations, Streaming, ML Models, ChatBots, etc... 
=======
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
