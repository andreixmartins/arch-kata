# Storage, S3 vs Ceph

## What is an Object Storage?

- Object storage is a relative low performance storage and relative low cost design for the needs of the internet workload (web applications, websites, hosting).
The goal is pick a large volume of data write that down and put it on a safe, secure and long therm requirement in a space where we need to maintain the data for a long time.
Any king of file can be an object, for every object we have an id, we also have some data (excel, video, document), also need metadata (if is everything you need to know about the object, who create, what it is used for, what the size, etc.),
the last thing it is attributes(what users have permission to overwrite or downloaded for example).

## Trade-offs Comparison

### 1. S3 (Amazon Simple Storage Service)
**Pros:**
- **Scalability**: Highly scalable, can handle vast amounts of data.
- **Low maintainability**: Managed service by AWS, reducing the need for in-house maintenance.
- **Integration**: Seamlessly integrates with other AWS services.
- **Global Availability**: Data can be accessed from anywhere in the world with low latency.
- **Security**: Offers robust security features including encryption and access control.
- **Cost-Effective**: Pay-as-you-go pricing model, which can be economical for many use cases.

**Cons:**
- **Cost**: Can become expensive with high data retrieval and transfer rates.
- **Vendor Lock-in**: Ties you to the AWS ecosystem, making migration to other platforms challenging.
- **Limited Customization**: Less flexibility in terms of configuration and customization compared to self-managed solutions.
- **Latency**: May have higher latency for certain applications compared to local storage solutions.
- **Data Transfer Costs**: Charges for data transfer out of S3 can add up, especially for high-traffic applications.

### 2. Ceph
**Pros:**
- **Open Source**: Free to use and modify, with a large community for support.
- **Flexibility**: Highly customizable to fit specific needs and configurations.
- **Scalability**: Can scale out by adding more nodes to the cluster.
- **Cost Control**: No licensing fees, allowing for potentially lower costs if managed effectively.
- **Data Redundancy**: Built-in data replication and erasure coding for fault tolerance.
- **Multi-Protocol Support**: Supports block, file, and object storage in a single platform.

**Cons:**
- **Complexity**: Requires significant expertise to set up, configure, and maintain.
- **Maintenance**: Ongoing maintenance and monitoring are necessary, which can be resource-intensive.
- **Initial Setup Cost**: While the software is free, the hardware and operational costs can be significant.
- **Support**: Lacks the dedicated support that comes with commercial solutions, relying instead on community support or paid third-party services.
- **Learning Curve**: Steeper learning curve for administrators unfamiliar with distributed storage systems.

## Recommendation
- **S3** it is a better choice, we need a hassle-free, scalable solution with minimal maintenance, and we are already using AWS services. It is ideal for us that prioritize ease of use and integration over customization.
