

# Problem 1 - Design
## Problem Statement:
Design a web application (Front-End layer, Back-End layer, Data layer), which scales up and down based on website traffic. The final infrastructure should consider various components that are available in a major public cloud provider (AWS, Azure, GCP).
Please state the design characteristics of various components like Load balancing, Security controls, Network topology, Logging, Metrics, Alerts, Backup policies, Failover mechanism, etc.
If anything is unclear, you may set reasonable assumptions and state at the start of the design.


## Evaluation Criteria:
The state of the design characteristics are considered on the following aspects:
- Operational
- Security
- Reliability
- Performance Efficiency
- Cost Optimization
- Sustainability
Architecture diagram and Documentation

# Resolve Problem

With the above requirement I will have the following:
- I will design a system based on existing components of AWS Cloud.
- The system will include many AZs located on a Region and multiple Region.
- The system is highly available and reliable.
- Configured security at a basic level.
- Good performance and serviceable globally.
- Sustainability is resilient if a disaster strikes.


# Design Infrastructure
![image](https://github.com/ThanhLam2396/Problem01/assets/39935839/0645d1df-b3be-49c6-9e7e-bc0ed276b941)



## Main components
Our system includes the following main components:
- Network Infrastructure
- Web Applications
- CDN
- Monitoring System
- Logs Centralization System

## Detailed description of the main components.
### Network Infrastructure:
Network infrastructure is designed with different network layers:
- Public network: Components located in the public network are accessible to the outside and can be accessed from the outside.
- Private network: With private allows to go out to the internet. But do not allow access from outside. 

The system is designed with multiple AZs in the same Region to ensure availability if one of the AZs fails. These AZs are placed in the same VPC, so the subnets within it can access each other if the route table, NACL is configured.

To increase availability, reliability and Security. Infrastructure is designed with multiple Regions. Networks of two regions can connect to each other. Through the Transit Gateway configuration.


### Web Applications:
The web application is designed with 3 layers.
- FrontEnd: Will be accessed directly from the outside through the Application Loadbalancer, to increase loadbalance traffic and increase availability.
- BackEnd:  Will only allow FrontEnd access. And it also has access to the Database Layer.
- Data Layer: allows access only from the Backend and blocks all other access. It will also be configured to open some port needed.
- The web application is scalable and flexible through Auto Scaling Group based on metrics such as number of requests, Memory, Cpu ....

Web Applications is deployed across different AZs and across different Regions. To ensure features are available. Therefore, ensure data consistency between different Az and Regions.
- Elastic Cache For Redis: Configure Elastic Cache For Redis with ElasticCache for Redis Global Datastore. To ensure data in redis is consistent across regions
- Aurora DB: Enable Asynchronous Replication Configuration for Aurora DB to ensure data is consistent across regions
- Amazon EFS: Enable sync data for Amazon EFS across regions using DataSync Service

Web application will be cross Multiple region through CloudFront to load balance, increase availability and global.



### CDN
CDN system using CloudFront with data from S3. To increase load capacity and availability across multiple regions, S3 is configured with Cross Multiple Replication and is interconnected via the S3 Multiple-Region Access Point.

To increase security and prevent DOS, DDOS and other types of attacks, CloudFront will be configured with Enable Shield and configured with WAF rules.

### Monitoring System
The monitoring system is built based on CloudWatch, SNS, Lamdard...
- CloudWatch: CloudWatch will be responsible for getting metrics from components in the system such as: EC2, Application LoadBalancer, Database, SQS, Redis, CloudFront ... Then it will display it on the Dashboard through CloudWatch Dashboard.
- SNS: After Cloudwatch has collected metrics and relies on metrics to trigger values ​​to be alarmed, SNS will be triggered to call Lamdar and run functions to send alerts to SRE, DevOps, etc. via mail, slack, telegram or webhook...

### Logs Centralization System

Centralized syslog built on top of Opensearch, Lamdar, Firehouse, and S3 (we can use other components in AWS)
- Firehouse and Lamda: will support getting logs from CloudFront, filtering, converting and storing it to S3. Also push logs to Opensearch.
- S3 will be the store of logs pushed from Firehouse or any other component that supports storing logs to S3.
- Opensearch will be used to querylogs and visualize it on the Dashboard. It can also support triggering an alert if a necessary log appears. Often used to alert audit logs, to prevent security problems at the earliest.

## SUMMARY:
The system is designed with components including: Load balancing, Security controls, Network topology, Logging, Metrics, Alerts, Backup policies, Failover mechanism, Recovery Disaster ....
And meet the requirements:
- Security
- Reliability
- Performance Efficiency
- Cost Optimization
- Sustainability
- Operational

