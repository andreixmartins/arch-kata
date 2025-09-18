### What is Amazon RDS?
- Amazon RDS stands for Relational Database Service, it's the overaching service for managing relational databases in the cloud

## What is Amazon Aurora?
- It's a database engine that can be used with RDS service, alongside other engines like MySQL and PostgreSQL.


### Architecture
- RDS uses a more traditional arch with compute and storage integrated on the same instance

- Aurora has a decoupled, cloud-native architecture that separates compute and storage, allowing for independent scaling of storage up to 128TB.

### Performance and Scalability
- Aurora offers significant better performance and scalability, with higher throughput and faster failover times (typically less then 30s) compared to standards RDS instances

- RDS is a standard, fully managed service but lacks specialized performance features of Aurora demanding workloads.

### Costs
- Aurora has a higher minimum price point and is more expansive for some workloads, but provides better performance and availability for critical applications

- RDS is a great choice for general purpose applications that don't require extreme performance and scalability of Aurora, and it's easier to use for simple projects.


