---
title: VMC on AWS Application Modernization - Part 2
date: 2023-12-15
tags: [VMC, VMC on AWS]     # TAG names should always be lowercase
---
*Note: This is part 2 of a 3 part series.* 
[Part 1](/posts/AWS-VMC-App-Modernization-Part1)   \||   [Part 2](/posts/AWS-VMC-App-Modernization-Part2)   \||   [Part 3](/posts/AWS-VMC-App-Modernization-Part3)

## 1.2	Application Modernization Use Cases

While there may be milestones to meet along the way, it is important to recognize that ultimately your modernization journey is an iterative process, and you can work in smaller phases. You migrate, then you keep modernizing and keep maturing. It is also important to note that every customer and company has their own internal policies and processes, modernization, too, varies from customer to customer. To help you get started, we are covering three use cases. These application modernization use cases are Load Balancing, Databases, and Containers.

**Load Balancing**

Load Balancers are a critical component in highly available architectures. By running your VM workloads on VMware Cloud on AWS, you gain the ability to offload common tasks associated with managing these load balancers. This includes operational things like handling scaling, registering your services and performing software upgrades.

Amazon Elastic Load Balancing (ELB) automatically distributes incoming application traffic across multiple targets, such as Amazon EC2 instances, containers, IP addresses, Lambda functions, and virtual appliances. It can handle the varying load of your application traffic in a single Availability Zone or across multiple Availability Zones. Elastic Load Balancing offers four types of load balancers that all feature the high availability, automatic scaling, and robust security necessary to make your applications fault tolerant.

Application Load Balancer: Application Load Balancer is best suited for load balancing of HTTP and HTTPS traffic and provides advanced request routing targeted at the delivery of modern application architectures, including microservices and containers. 

Network Load Balancer: Network Load Balancer is best suited for load balancing of Transmission Control Protocol (TCP), User Datagram Protocol (UDP), and Transport Layer Security (TLS) traffic where extreme performance is required.

Gateway Load Balancer: Gateway Load Balancer makes it easy to deploy, scale, and run third-party virtual networking appliances. Providing load balancing and auto scaling for fleets of third-party appliances, Gateway Load Balancer is transparent to the source and destination of traffic.

Classic Load Balancer: Classic Load Balancer provides basic load balancing and is intended for applications that were built within the EC2-Classic network.

ELB supports the load balancing capabilities critical for you to migrate to AWS. ELB is well positioned to load balance both traditional as well as cloud native applications with auto scaling capabilities that eliminate the guess work in capacity planning. ELB is easy to configure and use, which makes your migration experience simple. The managed experience of ELB means that you can focus on the most critical part of a successful migration - migrating applications - instead of configuring load balancers.

![Desktop View](/assets/posts/vmc_design_p2/p1.png){: .normal}

**Figure 1 – Load Balance Web and App Servers hosted in VMC**
The below steps describe the architecture above:
**External User Access**
1.	The Public Hosted Zone in Amazon Route 53 resolves requests to the Application Load Balancer (ALB)
2.	Amazon Route 53 directs the public internet traffic to the Public ALB
3.	The Public ALB directs the traffic to the Web Server VM’s in VMware Cloud on AWS
4.	The Web Server VM’s direct the traffic to the Private ALB to access the App Server VM’s
5.	The Private ALB directs the traffic to the App Server VM’s in VMware Cloud on AWS
6.	The App Server VM’s communicate with Amazon RDS for database access

**Internal User Access**
7.	Internal users initiate requests to Amazon Route 53 Resolver Inbound Endpoints through a VPN connection or AWS Direct Connect
8.	The Inbound Endpoints forward requests to Route 53 which resolves requests to the Private ALB
9.	Route 53 directs the internal traffic to the Private ALB
10.	The Private ALB directs the traffic to the Web Server VM’s in VMware Cloud on AWS
11.	The Web Server VM’s direct the traffic to the Private ALB to access the App Server VM’s
12.	The Private ALB directs the traffic to the App Server VM’s in VMware Cloud on AWS
13.	The App Server VM’s communicate with Amazon RDS for database access


**Databases**

Once workloads are running in VMware Cloud on AWS, customers can begin to leverage additional services to help modernize databases and offload the heavy lifting of database management. You can now begin to ingest data from your VMware Cloud on AWS environment into your native AWS accounts. One example of this is using AWS Lake Formation to build data lake architectures which can be used to run real-time, interactive analytics using services such as Amazon Kinesis, Amazon Elasticsearch Service, and Amazon Athena. In this example, we will focus on integrating VMware Cloud on AWS with Amazon Relational Database Service (RDS).

Amazon RDS makes it easy to set up, operate, and scale a relational database in the cloud. It provides cost-efficient and resizable capacity while automating time-consuming administration tasks such as hardware provisioning, database setup, patching and backups. It frees you to focus on your applications so you can give them the fast performance, high availability, security and compatibility they need.
You can scale your database's compute and storage resources with only a few mouse clicks or an API call, often with no downtime. Many Amazon RDS engine types allow you to launch one or more Read Replicas to offload read traffic from your primary database instance. When you provision a Multi-AZ database Instance, Amazon RDS synchronously replicates the data to a standby instance in a different Availability Zone (AZ). Amazon RDS has many other features that enhance reliability for critical production databases, including automated backups, database snapshots, and automatic host replacement.

![Desktop View](/assets/posts/vmc_design_p2/p2.png){: .normal}

**Figure 2 – Amazon RDS Integration with VMware Cloud on AWS**
Amazon RDS is available on several database instance types - optimized for memory, performance or I/O - and provides you with six familiar database engines to choose from, including Amazon Aurora, PostgreSQL, MySQL, MariaDB, Oracle Database, and Microsoft SQL Server. You can use the AWS Database Migration Service to easily migrate or replicate your existing databases to Amazon RDS. 

**Containers**

Containers provide a standard way to package your application's code, configurations, and dependencies into a single object. Containers share an operating system installed on the server and run as resource-isolated processes, ensuring quick, reliable, and consistent deployments, regardless of environment. Containers are a powerful way for developers to package and deploy their applications. They are lightweight and provide a consistent, portable software environment for applications to easily run and scale anywhere. Building and deploying microservices, running batch jobs, for machine learning applications, and moving existing applications into the cloud are just some of the popular use cases for containers.
AWS offers services that give you a secure place to store and manage your container images, orchestration that manages when and where your containers run, and flexible compute engines to power your containers. AWS can help manage your containers and their deployments for you, so you don't have to worry about the underlying infrastructure. If you are running Kubernetes on-premises you can leverage Amazon Elastic Container Service for Kubernetes (Amazon EKS); this solution makes it easy to deploy, manage, and scale containerized applications using Kubernetes on AWS. If you are new to containers, don’t have experience with container orchestrators, or simply want to stick with using AWS API’s, use the Amazon Elastic Container Service (Amazon ECS). Amazon ECS featuring AWS Fargate will enable you to deploy containers without provisioning servers, ultimately reducing management overhead. 
 
![Desktop View](/assets/posts/vmc_design_p2/p3.png){: .normal}

**Figure 3 – Modernize Applications with Microservices Architecture using Amazon Elastic Kubernetes Service (Amazon EKS)**
The below steps describe the architecture above:
1.	The Elastic Network Interface is automatically attached to the EC2 bare metal (ESXi) hosts in VMware Cloud on AWS during the software defined data center (SDDC) provisioning.
2.	Provision fully managed Amazon EKS clusters for different environments (dev/test/production).
3.	Use tools such as AWS App2Container (A2C) to accelerate refactoring/rearchitecting applications into containerized microservices, and use Amazon EKS to manage and automate the testing and deployment of container workloads.
4.	Transform and containerize legacy systems to modern applications with minimum disruptions, while keep the existing database tier running on VMware Cloud on AWS to avoid the complexity and delay for database migrations.
5.	Network Load Balancer integrates with Kubernetes Ingress controller, providing a secure and consistent approach for exposing applications.
6.	Amazon Route 53 resolves incoming requests to the Network Load Balancer in the primary AWS Region.
7.	Dev team commits code to an AWS CodeCommit repository, which triggers AWS CodePipeline to start processing the code changes through the pipeline.
8.	AWS CodeBuild packages the code changes and dependencies and builds a Docker image.
9.	The new Docker image is pushed to Amazon Elastic Container Registry (Amazon ECR).
10.	AWS CodeBuild uses Kubectl command line tool to invoke Kubernetes API and update the image tag for the microservice deployment.
11.	Kubernetes performs a rolling update of the pods in the application deployment as per the new docker image specified in Amazon ECR.
