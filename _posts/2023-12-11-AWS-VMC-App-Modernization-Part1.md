---
title: VMC on AWS Application Modernization - Part 1
date: 2023-12-11
tags: [VMC, VMC on AWS]     # TAG names should always be lowercase
---
*Note: This is part 3 of a 3 part series.* 
[Part 1](/posts/AWS-VMC-App-Modernization-Part1)   \||   [Part 2](/posts/AWS-VMC-App-Modernization-Part2)   \||   [Part 3](/posts/AWS-VMC-App-Modernization-Part3)

Application modernization is a multi-faceted approach to achieving faster software delivery cycles, lower TCO, and gain operational efficiencies. This is achieved through an automation first approach, as well as offloading the heavy lifting by leveraging managed services when possible. Ways to modernize include going to the cloud, using automation, and taking advantage of cloud-enabled possibilities not previously achievable on-premises (such as real-time analytics or containerization).
This chapter covers three common use cases as well as design and other considerations for modernizing applications that run on the VMware Cloud on AWS service. 

## 1.1	Modernization Considerations

Presumably by now, you have decided that modernizing is the next phase for your VMware and AWS journey and are ready to take the leap into the next phase. In order to accomplish this goal, you must lean on people, processes, and technology to make the modernization a reality.
Why Modernize
Regardless of the industry, organizations are looking to become more agile so they can innovate and respond to change faster. This enables organizations to meet their customers where they are by developing features and bug fixes at a faster rate, which leads to higher customer satisfaction. Modern applications are built with a combination of modular architecture patterns, serverless operational modules and agile developer processes. This allows for organizations to innovate faster while reducing risk, time to market, and total cost of ownership.  

In traditional on-premises deployments, customers are responsible for the deployment and operational components of not only the infrastructure, but the containers, databases, and message brokers that support the applications. By migrating your workloads to VMware Cloud on AWS, you can start to offload the undifferentiated heavy lifting of managing these components to managed AWS services such as Amazon Relational Database Service (RDS). With this new agility and speed, your team can now focus on value adding projects instead of worrying about backups, patching, and software upgrades.
Where to Start
You need buy-in from your organization’s business decision makers and executives as well as the IT leaders who will direct the project. Involve your DevOps team, who are known for being nimble, agile, and forward- looking. You may also want to involve a cloud migration partner, such as an AWS Partner Network (APN) Partner company, to help you plan and execute your cloud modernization.
It is important to pick a target project on which to begin. Now that these workloads are on AWS, consider how you can begin to modernize—such as embracing backup and restore for data storage, performing deeper data and analytics, or adopting containers. Finding a team internally that has buy in is extremely helpful as well as they will have the motivation to see the project succeed.

A critical path to modernization begins with education, and leveraging the best practices that AWS has learned over the years will help to build the foundational knowledge. The AWS Cloud Adoption Framework (AWS CAF) helps organizations understand how cloud adoption transforms the way they work. AWS CAF leverages our experiences assisting companies around the world with their Cloud Adoption Journey. Assessing migration readiness across key business and technical areas, referred to as Perspectives, helps determine the most effective approach to an enterprise cloud migration effort. First, let’s outline what we mean by perspective. AWS CAF is organized into six areas of focus, which span your entire organization. We describe these areas of focus as Perspectives: Business, People, Governance, Platform, Security, and Operations. For further reading, see the AWS CAF Whitepaper.