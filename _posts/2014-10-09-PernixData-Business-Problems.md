---
title: PernixData – Solving Business Problems
date: 2014-10-09
tags: [PernixData]     # TAG names should always be lowercase
---
With the release of PernixData FVP 2.0 last week, I figured it would be a good time to take a look back and share my experience with this impressive technology, and take you all on a journey with me regarding how it solved a major business problem at the company I work for.

When I first joined this company, there were roughly 500 employees, and the Virtual Server Infrastructure had about 250 VM’s. In the past two years, the company has grown exponentially from both a workforce and infrastructure standpoint. As of this writing, there are currently over 1800 employees, and 750 VM’s running in our Virtual Server Infrastructure, not including the roughly 75 EC2 instances we have deployed. One of these VM’s has been running since the printing press was invented, and is an MS SQL Reporting Server. This bad boy has grown from 4vCPU and 64GB of RAM to 16vCPU and 192GB of RAM, resource contention be damned. The consumed storage of this VM has also gone from a rather pedestrian 500GB to a now-staggering 6TB.

About a year ago, we started to receive one-off reports from Business Analysts that the query and job times were starting to grow. Utilizing the monitoring tools we had, we started to see some alerts reporting the under-provisioning of CPU and Memory, with some Disk Latency spikes. Allocating additional resources to this VM bought us some breathing room for a few months, however, as we continued to expand, the issue would inevitably return. Coincidentally, around the same time these performance issues started to occur, I began testing PernixData FVP 1.0 in our Lab Environment to see what the technology was, and how we could potentially reap its benefits in the future. The performance metrics we saw just by simply adding an SSD to the Host and installing a VIB was eye opening.

Now that I have painted you a pretty picture around my initial experience with PernixData and the performance issues surrounding the massive (and quickly expanding) Reporting Server, let’s take a walk down memory lane and see how this particular story ends.

Five months ago, the performance of this Reporting Server became increasingly unbearable during routine end-of-month job cycles. This server is the sole Reporting Server for the entire company; Business units rely on this data daily to make critical business decisions- downtime, slowness, or unexpected delays in data retrieval are simply unacceptable. Nothing will bring an executive down to the dungeon that houses IT Engineering faster than an issue with this Reporting Server. Earlier in the year, we had planned to break out some of the databases and implement a new architecture utilizing SQL Server 2012 Always on Availability Group functionality. We also wanted to take advantage of the re-architecture to load-balance the databases between the multiple arrays that are utilized in the Virtual Server Infrastructure. However, due to the virulent growth the company experiences, and a lack of resources from the core DBA team, we had to come up with an alternative solution to stop the bleeding… PernixData, I’m looking at you!

Having previously seen what PernixData could do with minimal design changes, I created a quick, yet decisively persuasive Powerpoint presentation for my Management Team to review. The content of that presentation outlined who PernixData is, what it is that they do, and how the technology can help prevent Business Processes from being impacted. Within three days, we had our demo SSD’s and licenses from PernixData. Pro-Tip: The team at PernixData is World Class; from management all the way down to the support staff!

I won’t go over the details of the installation, as it’s been extremely well documented by both PernixData and other bloggers. I have included a few links at the bottom of this post in case you feel compelled to check them out. Long story short, we ran FVP in our Staging Environment for roughly a week, testing failover scenarios and validating the performance gains. Once the infrastructure and DBA teams felt comfortable with the results of our testing, we set August 1st as the official implementation day for the Production Environment. The implementation was extremely easy and went through without an issue, and all that was left to do now was sit back, relax, and wait for the results.

After monitoring the performance for a few days, we finally had the data we had been looking for from both the Infrastructure and Business Analyst Teams, and the results were impressive- From an infrastructure perspective, the average latency of this VM dropped from 25-75ms down to 2-8ms, and the Business Analysts reported a consistent 40% reduction in job run times! We looked like rock stars. Partied like them too.

PernixData stats from today:

![Desktop View](/assets/posts/pernixdata/1.png){: .normal}

Although we still have a long way to go in regards to re-architecting the Reporting Server environment, PernixData solved a critical business issue for us with minimal changes to the existing environment. With the indisputable success of this project, I have since had the opportunity to share my experiences with a few of my friends from the Los Angeles VMUG. One of the members who works for a school district had encountered similar performance issues, and even brought in an expensive all flash array vendor to help mitigate the issue. I encouraged him to give FVP a try, and I am confident that I will hear a story with the same successful ending that I have shared. I’ll post a follow up after our next event, and let you all know how it went!

Blog Posts Covering PernixData FVP Installation

[PernixData FVP in my lab](http://www.thevirtualway.it/en/?p=827) – Francesco Bonetti

[PernixData in the Lab](http://theithollow.com/2013/09/pernix-data-in-the-lab/) – By Eric Shanks