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
2. Mobile native app to register POC
3. Searching POCs by name, language and tags
4. Multi-tentant system
5. Generate reports
6. Generate videos
7. System must be secure
8. Users will have their credentials


```
Recomended Reading: http://diego-pacheco.blogspot.com/2021/10/breaking-problems-down.html

### 2. 🎯 Goals

List in form of bullets what goals do have. Here it's great to have 5-10 lines.
Example:
```
1. Security is non-negociable! Security at-rest, transite, threat analysis and review for by at least 3 different people.
2. Mobile-native: We want a native app using Swift and Kotlin 
3. Cloud-Native: All backend must be 100% cloud native, using AWS Cloud services
4. Microservices: Do not use AWS Lambdas. App services must use the microservice architecture
5. Cache: AWS Elastic Cache to cache user data information
6. Tests: Employ unit tests, integration tests, regression tests, chaos tests
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

🗂️ 4.1 Overall architecture: Show the big picture, relationship between macro components.

<img src="images/overall_diagram_simple_v2.png">

<img src="images/overall_diagram.png">

🗂️ 4.2 Deployment: Show the infra in a big picture. 

<img src="images/deployment_diagram.png">

🗂️ 4.3 Use Cases: Make 1 macro use case diagram that list the main capability that needs to be covered. 

<img src="images/use_case_diagram_v2.png">

Recommended Reading: http://diego-pacheco.blogspot.com/2020/10/uml-hidden-gems.html

### 🧭 5. Trade-offs

List the tradeoffs analysis, comparing pros and cons for each major decision.
Before you need list all your major decisions, them run tradeoffs on than.
example:
Major Decisions: 
```
1. Two mobile code base - Mobile application should be developed in Swift to handle IOS and Kotlin for Android versions.
2. Backend application should be built using microservices separated by schema/domains instead of serveless approach.
3. Cache - Use AWS Elastic cache instead of Redis
```
Tradeoffs:
```
1. AWS Cognito vs Keycloak
2. AWS ECS vs AWS EKS
3. AWS KEYSPACES vs AWS RDS POSTGRES
4. Redis (Self-Hosted) vs AWS Elastic Cache
5. LiveKit vs WebRTC
```


## AWS Cognito vs Keycloak

AWS Cognito
```
PROS (+) 
  * Setup and Maintenance: Easy to set up with managed service, minimal infra overhead.
  * Cost: Pricing based on MAUs (monthly active users), no server costs.
  * Integration: Tight integration with AWS (IAM, API Gateway, Lambda).
CONS (+)
  * Scalability: Bound to AWS regions and infra.
  * Customization: Branding and flow changes limited to AWS features.
  * Vendor Lock-in: Difficult to migrate away from AWS infra.
```

Keycloak
```
PROS (+) 
  * Setup and Maintenance: Full control over installation, configuration, and setup.
  * Scalability: Can scale with Kubernetes/infra as needed.
  * Cost: Open-source, no license fees.
  * Customization: Fully customizable login, flows, and theming.
  * Integration: Works well across cloud and on-prem systems (OIDC, SAML, LDAP).
CONS (+)
  * Limitations: Full burden of upgrades and migration lies on you.
  * Security: Security depends on in-house team vigilance.
```

## AWS ECS vs AWS EKS

AWS EKS on Fargate
```
PROS (+) 
  * Zero node management: AWS abstracts away VMs.
  * Strong workload isolation: Each Pod runs in a micro-VM-like sandbox. Great for multi-tenancy.
  * Predictable billing: Pay per vCPU and GiB-hour. No overprovisioning nodes.
  * Scaling: Near-instant for moderate workloads (subject to Fargate launch quotas).

CONS (+)
  * Feature limitations: No DaemonSets, no privileged/host networking, no GPUs, limited ephemeral disk (~20 GiB).
  * Observability overhead: Agents must be sidecars (instead of cluster-wide DaemonSets).
  * Cost at scale: Can be more expensive than EC2 if you have high sustained workloads.
  * Startup throttles: Large Pod bursts can hit account Fargate launch quotas.
```

ECS on Fargate
```
PROS (+) 
  * Zero infrastructure management: AWS handles provisioning, scaling, patching.
  * Isolation: Strong security boundary per task (ideal for multi-tenancy).
  * Pay-as-you-go: Billing is per vCPU and GiB-hour, no wasted capacity.
  * Auto-scaling: Tasks scale without cluster capacity planning.

CONS (+)
  * Feature limits: No privileged containers, host networking, GPUs.
  * Cost: More expensive for sustained or heavy workloads than EC2.
  * Quotas: Fargate launch throttles and ephemeral storage (~20 GiB default).
  * Observability: No DaemonSets — logging/monitoring agents must run as sidecars (extra cost overhead).
```

## AWS KEYSPACES vs AWS RDS POSTGRES

AWS KEYSPACES
```
PROS (+) 
  * Scalability: Horizontal scaling, Elastic on-demand throughput.
  * Avaiability: Built-in AWS multi-Region
  * Performance: Millisecond latencies with predictable performance when partitions are modeled well.
  * Replication: Built-in multi-Region, active-active (async replication).
  * Operation: Fully managed, serverless-like. No clusters/patching. Pay-per-request.
  
CONS (+)
  * Consistency: Tunable consistency, but no global ACID transactions.
  * Analytics: Not suitable for BI/reporting without ETL into another system.
  * Cost: Per-request pricing can get expensive if query patterns are inefficient.
```

AWS RDS PostgreSQL
```
PROS (+) 
  * Scalability: Scales vertically and horizontally via Aurora read replicas.
  * Perfomance: Strong OLTP performance with indexes and joins.
  * Latency: Low-latency reads in multiple Regions
  
CONS (+)
  * Limitation: Must use RDS Proxy or pooling for apps with millions of users.
  * Operation: Still need to think about version upgrades, storage, scaling thresholds, and failover planning.
  * Multi-Region writes: Global database only supports fast cross-Region reads; writes go to one Region.
```


## AWS MSK (KAFKA) vs AWS SQS

AWS MSK (KAFKA)
```
PROS (+) 
  * Debugging: Event streaming with replay/rewind
  * Scalability: High throughput
  * Consistency: Strict ordering per partition
  * Integration: Rich ecosystem (Kafka Streams, Flink, Connect)
CONS (+)
  * Overhead: Operational complexity
  * Cost: Costly at low/variable volume
```

AWS SQS
```
PROS (+) 
  * Simplicity: Fully managed, zero ops
  * Cost: Pay-per-request pricing
  * Reliability: DLQs and retries built-in
  * Security: IAM integration + per-queue isolation
  * Automation: Tight AWS integration (Lambda, Step Functions)
CONS (+)
  * Duplication: At-least-once delivery only
```


## Redis (Self-Hosted) vs AWS Elastic Cache

Redis
```
PROS (+) 
  * Performance: Redis offers multiple methods for caching data, which can significantly reduce data access latency and increase throughput.
CONS (+)
  * Cost: Redis clustering solutions needs to be done in-house and requires a significant amount of effort.
```

AWS Elastic Cache
```
PROS (+) 
  * Setup and Maintenance: ElastiCache is a fully managed AWS service for Redis, not necessary to deal with ec2 instances and configs to install Redis.
CONS (+)
  * Limitations: ElastiCache runs only within the Amazon Web Services ecosystem, you may be concerned about vendor lock-in
```

## LiveKit vs WebRTC (self hosted)

LiveKit
```
PROS (+) 
  * Scalability: Designed with cloud-native scaling in mind; Kubernetes-ready with multi-node support and autoscaling.
  * Setup and Maintenance: Easy to deploy with prebuilt Docker images and Helm charts. Managed cloud option available (LiveKit Cloud).
  * Availability: LiveKit Cloud offers high availability out of the box with managed infrastructure and SLAs.
CONS (+)
  * Complexity: Abstracted logic limits low-level control. Custom media routing or deep protocol tweaks are not straightforward.
  * Cost: LiveKit Cloud can become expensive at scale compared to self-hosted SFUs like MediaSoup.
```

WebRTC
```
PROS (+) 
  * Scalability: Can scale with custom SFU (Selective Forwarding Unit) setup.
  * Optimization: Can deeply optimize SFU settings.
CONS (+)
  * Setup and Maintenance: Must build it from scratch. Must self-host, integrate, and configure.
  * Security: You are Responsible for correct, secure and authentication setups.
```

PS: Be careful to not confuse problem with explanation. 
<BR/>Recommended reading: http://diego-pacheco.blogspot.com/2023/07/tradeoffs.html

### 🌏 6. For each key major component

What is a majore component? A service, a lambda, a important ui, a generalized approach for all uis, a generazid approach for computing a workload, etc...


## 6.1 - Class Diagram 

<img src="images/class_diagram_v2.png">


## 6.2 - Contract Documentation

- We must use OAuth 2.0 to access/refresh tokens
- We must use REST API
- We must use AWS Gateway

> **USER**


### <span style='color:#3BC143 ;font-weight: bold;'>LOGIN</span>
method: <span style='color:#FFBE33;font-weight: bold;'>POST</span>
path: <span style='color:#FFBE33;font-weight: bold;'>v1/login</span>
- User authentication endpoint usually used to identify the current user session and fetch user data. Response must return the userId and user token. 
  1. username and password are required fields
  - request
  ```json
  {
    username : "string",
    password : "string"    
  }
  ```
  - response
  ```json
  {
    userId : "string",
    token : "string"
  }
  ```

### <span style='color:#3BC143 ;font-weight: bold;'>LOGOUT</span>
method: <span style='color:#FFBE33;font-weight: bold;'>POST</span>
path: <span style='color:#FFBE33;font-weight: bold;'>v1/logout</span>
- Invalidate user session
  1. username is required
  - request
  ```json
  {
    username : "string"
  }
  ```
  - response
  ```json
  {
    success : boolean
  }
  ```  

### <span style='color:#3BC143 ;font-weight: bold;'>CREATE_USER_ACCOUNT</span>
method: <span style='color:#FFBE33;font-weight: bold;'>POST</span>
path: <span style='color:#FFBE33;font-weight: bold;'>v1/account</span>
- Create user account allowing the user to log into the app. 
  1. first name is required
  2. firstName is required
  3. lastName is required
  4. email is required
  5. username is required
  6. password is required
  7. Account status account_status should be updated to C (require confirmation). update account update_date column with the current date 
  8. After account creation user must receive an email to confirm this account
  9. After account confirmation user account_status should be updated to A (active). update account update_date column with current date
  10. After these steps user can log into app
  - request
  ```json
  {
    firstName : "string",
    lastName : "string",    
    email : "string"        
    username : "string"        
    password : "string"                
  }
  ```            
  - response
  ```json
  {
    id: long
    account_status: "string"      
  }  
  ```  

### <span style='color:#3BC143 ;font-weight: bold;'>USER_PROFILE</span>
method: <span style='color:#FFBE33;font-weight: bold;'>POST</span>
path: <span style='color:#FFBE33;font-weight: bold;'>v1/account/profile</span>
- Create user profile allowing the user create/update user settings, privacy and notifications
  1. user_id is required
  - request
  ```json  
  {
    user_id: "string"
  }    
  ```  
  - response
  ```json
  {
    firstName : "string",
    lastName : "string",    
    email : "string",        
    picture : "string",   
  }    
  ```    
  
### <span style='color:#3BC143 ;font-weight: bold;'>FIND_USER</span>
method: <span style='color:#FFBE33;font-weight: bold;'>GET</span>
path: <span style='color:#FFBE33;font-weight: bold;'>v1/user</span>
- Fetch user by username or user_id, 
  1. To fetch user information , you must add to the payload the user_id or username
  - request
  ```json
  {
    user_id  : "string",      
    username : "string"      
  }    
  ```   
  - response
  ```json
  {
    firstName : "string",
    lastName : "string",    
    email : "string",        
    picture : "string", 
    status : "string"     
  }
  ``` 


### <span style='color:#3BC143 ;font-weight: bold;'>FIND_USER_NOTIFICATIONS</span>
method: <span style='color:#FFBE33;font-weight: bold;'>GET</span>
path: <span style='color:#FFBE33;font-weight: bold;'>v1/user/notification</span>
- Get last user notifications by user_id sorted by created date
  1. This service should limit 50, you must pass this limit in the payload request
  2. You must pass an offset to the payload to paginate the notification list
  3. user_id is required, limit default value is 50, offset default value is zero
  - request
  ```json
  {
    user_id : "string"  
    limit : integer 
    offset: integer
  }
  ```
  - response
  ```json
  {
    notifications : []
  }
  ```

### <span style='color:#3BC143 ;font-weight: bold;'>FIND_USER_MESSAGES</span>
method: <span style='color:#FFBE33;font-weight: bold;'>GET</span>
path: <span style='color:#FFBE33;font-weight: bold;'>v1/user/messages</span>
- Fetch user messages by user_id and room_id and sort it by created date
  1. user_id is required
  2. room_id is required
  3. It will fetch all active user messages
  - request
  ```json
  {
    user_id : "string"
    room_id :integer
  }
  ```
  - response
  ```json
  {
    messages : []        
  }
  ```


### <span style='color:#3BC143 ;font-weight: bold;'>REMOVE_USER_MESSAGES</span>
method: <span style='color:#FFBE33;font-weight: bold;'>POST</span>
path: <span style='color:#FFBE33;font-weight: bold;'>v1/user/messages</span>
- This endpoint will provide a soft delete of user messages updating the message_status to R (Removed)
  1. user_id is required
  2. messages_id - list of messages_id
  - request
  ```json
  {
    user_id : long
    messages_id : []              
  }
  ```

  - response
  ```json
  {
    success boolean          
  }
  ```


### <span style='color:#3BC143 ;font-weight: bold;'>UPDATE_STATUS</span>
method: <span style='color:#FFBE33;font-weight: bold;'>POST</span>
path: <span style='color:#FFBE33;font-weight: bold;'>v1/user/status</span>
- Update user status to ONLINE or OFFLINE
  - request
  ```json
  {
    status : int
  }
  ```
  - response
  ```json
  {
    success : boolean   
  }
  ```


### <span style='color:#3BC143 ;font-weight: bold;'>UPDATE_PICTURE</span>
method: <span style='color:#FFBE33;font-weight: bold;'>POST</span>
path: <span style='color:#FFBE33;font-weight: bold;'>v1/user/picture</span>
- Upload the user profile picture 
  1. user_id is required
  2. image is required, this image must be encoded in base64
  - request
  ```json
  {
    user_id: "string",
    image: "string"    
  }
  ```

  - response
  ```json
  {
    success: "boolean"  
  }
  ```

**BILLING**

### <span style='color:#3BC143 ;font-weight: bold;'>PAYMENT</span>
method: <span style='color:#FFBE33;font-weight: bold;'>POST</span>
path: <span style='color:#FFBE33;font-weight: bold;'>v1/payment</span>
- Perform payment of subscription purchase using payment gateway
  1. tenant_id is required
  2. total is required 
  3. payment_gateway is required
  - request
  ```json
  {
    tenant_id: "string",
    total: "float",
    payment_gateway: "string"  
  }
  ```

  - response
  ```json
  {
    success boolean  
  }
  ```

> **DOJO**

### <span style='color:#3BC143 ;font-weight: bold;'>CREATE_DOJO_ROOM</span>
method: <span style='color:#FFBE33;font-weight: bold;'>POST</span>
path: <span style='color:#FFBE33;font-weight: bold;'>v1/dojo/create</span>
- Create a Dojo Room for a tenant
  1. tenant_id is required
  2. schedule date for a session
  3. team members
  4. subject 
  - request
  ```json
  {
    tenant_id: "long",
    dojo_date: "date"        
    members: [],
    subject: "string"    
  }
  ```

  - response
  ```json
  {
    dojo_id long  
  }
  ```
### <span style='color:#3BC143 ;font-weight: bold;'>START_DOJO</span>
method: <span style='color:#FFBE33;font-weight: bold;'>POST</span>
path: <span style='color:#FFBE33;font-weight: bold;'>v1/dojo/start</span>
- Start a Recording of a Dojo session
  1. tenant_id is required
  2. dojo_id is required
  
  - request
  ```json
  {
    tenant_id: "long",
    dojo_id: "long"        
  }
  ```

  - response
  ```json
  {
    success boolean
  }
  ```
### <span style='color:#3BC143 ;font-weight: bold;'>STOP_DOJO</span>
method: <span style='color:#FFBE33;font-weight: bold;'>POST</span>
path: <span style='color:#FFBE33;font-weight: bold;'>v1/dojo/stop</span>
- Stop a  record of a Dojo Room session
  1. tenant_id is required
  2. dojo_id is required
  
  - request
  ```json
  {
    tenant_id:  "string",
    dojo_id: "long"        
  }
  ```

  - response
  ```json
  {
    success boolean
  }
  ```

> **REPORT**

### <span style='color:#3BC143 ;font-weight: bold;'>ANUAL_REPORT</span>
method: <span style='color:#FFBE33;font-weight: bold;'>POST</span>
path: <span style='color:#FFBE33;font-weight: bold;'>v1/report/anual</span>
- Generate an anual report for entire year from specific tenant 
  1. tenant_id is required
    
  - request
  ```json
  {
    tenant_id long
  }
  ```

  - response
  ```json
  {
    report_id: long
  }
  ```
### <span style='color:#3BC143 ;font-weight: bold;'>REPORT_BILLING</span>
method: <span style='color:#FFBE33;font-weight: bold;'>POST</span>
path: <span style='color:#FFBE33;font-weight: bold;'>v1/report/billing</span>
- Generate a Billing report for a specific tenant
  1. tenant_id is required
    
  - request
  ```json
  {
    tenant_id: "string"
  }
  ```

  - response
  ```json
  {
    report_id: long
  }
  ```

#### 6.3 Persistence Model

### **User**

| NAME      | TYPE        | SIZE | NOT NULL | DEFAULT           | DESCRIPTION                     |
|-----------|-------------|------|----------|-------------------|---------------------------------|
| id        | uuid        |      | NO       |                   | uuid primary key                |                 
| tenantid  | uuid        |      | NO       |                   | uuid user table                 |                 
| username  | varchar     | 15   | NO       |                   | username must have an index     |
| firstname | varchar     | 30   | NO       |                   |                                 |
| lastname  | varchar     | 30   | NO       |                   |                                 |
| email     | varchar     | 50   | NO       |                   |                                 |
| age       | tinyint     | 3    | NO       |                   |                                 |
| created   | timestamp   |      | NO       | current_timestamp |                                 |
| updated   | timestamp   |      | NO       | current_timestamp |                                 |


### **Room**

- PRIMARY KEY (userid, created)

| NAME      | TYPE        | SIZE | NOT NULL | DEFAULT           | DESCRIPTION                     |
|-----------|-------------|------|----------|-------------------|---------------------------------|
| id        | uuid        |      | NO       |                   | uuid primary key                |   
| userid    | uuid        |      | NO       |                   | uuid user table                 |
| tenantid  | uuid        |      | NO       |                   | uuid user table                 |                 
| date      | timestamp   |      | NO       |                   | Dojo scheduled date             |                 
| name      | varchar     | 50   | NO       |                   | name must have an index         |
| status    | varchar     | 1    | NO       | A                 | A-Active, I-Inactive            |
| created   | timestamp   |      | NO       | current_timestamp |                                 |
| updated   | timestamp   |      | NO       | current_timestamp |                                 |


### **Billing**

- PRIMARY KEY (tenantid, paymentgateway)

| NAME           | TYPE        | SIZE | NOT NULL | DEFAULT           | DESCRIPTION                     |
|----------------|-------------|------|----------|-------------------|---------------------------------|
| id             | uuid        |      | NO       |                   | uuid primary key                |   
| tenantid       | uuid        |      | NO       |                   | uuid user table                 |
| plan           | varchar     | 15   | NO       |                   | M - monthly - Y - yearly        |
| price          | float       | 10   | NO       | A                 | Total amount                    |
| expirationdate | timestamp   |      | NO       |                   | Expiration date plan            |
| autorenew      | integer     | 1    | NO       |                   | 0 - NO - 1 - YES                |
| paymentgateway | varchar     | 10   | NO       |                   | Payment gateway name            |
| created        | timestamp   |      | NO       | current_timestamp |                                 |
| updated        | timestamp   |      | NO       | current_timestamp |                                 |


### **PocUser** 

- PRIMARY KEY (userid, tenantid, created)

| NAME           | TYPE        | SIZE | NOT NULL | DEFAULT           | DESCRIPTION                     |
|----------------|-------------|------|----------|-------------------|---------------------------------|
| id             | uuid        |      | NO       |                   | uuid                            |   
| userid         | uuid        |      | NO       |                   | uuid user table                 |
| tenantid       | uuid        |      | NO       |                   | uuid user table                 |
| name           | varchar     |      | NO       |                   | POC Name                        |
| language       | uuid        |      | NO       |                   | POC languages e.g java, js, GO  |
| favorite       | integer     | 1    | NO       | N                 | N - NO - Y - YES                |
| tags           | varchar     | 255  | NO       |                   | POC Tags                        |
| repourl        | varchar     | 255  | NO       |                   | POC Repo URL                    |
| created        | timestamp   |      | NO       | current_timestamp |                                 |
| updated        | timestamp   |      | NO       | current_timestamp |                                 |


Note: Cassandra database does not have a column size property. We must limit the number of characters on the application side.


- AWS POSTGRESQL REPORT TABLE

```sql
CREATE TABLE reports (
    id SERIAL PRIMARY KEY, -- SERIAL to auto-increment as a primary key
    user_id STRING NOT NULL, -- UUID User id comming from Cassandra tables
    created_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP, -- Date to inform when the report was generated
    report_type VARCHAR(50) NOT NULL -- Column to inform the report type - BILLING, DOJOS, USERS 
);  
```

#### 6.4 - Algorithms/Data Structures : Specific algos that need to be used, along size with spesific data structures.

- Circuit breaker Pattern
- Retry with backoff
- Queue for message notifications
- REDIS SET data structure for POCs Caching. 


Samples of other components: Batch jobs, Events, 3rd Party Integrations, Streaming, ML Models, ChatBots, etc... 

Recommended Reading: http://diego-pacheco.blogspot.com/2018/05/internal-system-design-forgotten.html

### 🖹 7. Migrations

IF Migrations are required describe the migrations strategy with proper diagrams, text and tradeoffs.

- N/A

### 🖹 8. Testing strategy

Explain the techniques, principles, types of tests and will be performaned, and spesific details how to mock data, stress test it, spesific chaos goals and assumptions.

- Manual testing -  Involves manual inspection and testing of the software by a human tester.
- Automated testing - software tools to automate the testing process
- Unit testing - Tests individual units or components of the software to ensure they are functioning as intended.
- Integration testing - Tests the integration of different components of the software to ensure they work together as a system.
- Regression testing – Tests the software after changes or modifications have been made to ensure the changes have not introduced new defects.
- Performance testing - Tests the software to determine its performance characteristics such as speed, scalability, and stability.
- Security testing – Tests the software to identify vulnerabilities and ensure it meets security requirements.
- Usability testing – Tests the software to evaluate its user-friendliness and ease of use.

### 🖹 9. Observability strategy

Explain the techniques, principles,types of observability that will be used, key metrics, what would be logged and how to design proper dashboards and alerts.

- Dojo system will generated service metrics , the metrics will be consumed by Prometheus,Grafana and Loki stack 
- Grafana will provide customized dashboards to identify errors , alerts and also providing ways to troubleshoot 
- Metrics dashboards should contain errors,alert identifying any service errors

### 🖹 10. Data Store Designs

For each different kind of data store i.e (Postgres, Memcached, Elasticache, S3, Neo4J etc...) describe the schemas, what would be stored there and why, main queries, expectations on performance. Diagrams are welcome but you really need some dictionaries.

- AWS S3 for videos, images, text files, reports
- AWS RDS Postgres for structured data
- AWS Elastic cache for caching data
- AWS Keyspaces (Apache Cassandra)

### **AWS Keyspaces (Cassandra) Queries**

- Find user
```sql
SELECT id, username, firstname, lastname, email, age, created, updated FROM app.user where id = ?;  
```

- Find poc by name
```sql
SELECT id, name, language, description, favorite, tags FROM app.pocuser where userid = ? and name = ?;   
```

- Find poc by language
```sql
SELECT id, name, language, description, favorite, tags FROM app.pocuser where userid = ? and language = ?;   
```

- Find poc by tag
```sql
SELECT id, name, language, description, favorite, tags FROM app.pocuser where userid = ? and tag = ?;   
```

- Find room by user, userid and created date
```sql
select currentDate(), dateof(now()), id, userid, name, created from app.room where userid = ? AND created >= ? - 1d;
```

### 🖹 11. Technology Stack

Describe your stack, what databases would be used, what servers, what kind of components, mobile/ui approach, general architecture components, frameworks and libs to be used or not be used and why.

- Android UI: Kotlin
- IOS UI: Swift
- Language: Java 23
- Framework:	Spring Boot 3
- Container: Docker
- Orchestration:	EKS Kubernetes
- API: REST
- Auth: OAuth2 + JWT
- Databases: AWS Keyspaces (Cassandra) PostgreSQL + Redis
- Messaging: Kafka (AWS MSK)
- CI/CD: GitHub Actions + ArgoCD
- Monitoring: Prometheus + Grafana
- Logging: Loki
- Tracing: Xray
- Realtime video - NodeJS + WebRTC

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
