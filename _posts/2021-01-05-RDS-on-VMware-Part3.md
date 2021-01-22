---
title: Amazon RDS on VMware – Part 3 – DB Provisioning
date: 2021-01-05
tags: [AWS, RDS on VMware, VMware]     # TAG names should always be lowercase
---
*Note: This is part 3 of a 3 part series.* 
[Part 1](/posts/RDS-on-VMware-Part1)   \||   [Part 2](/posts/RDS-on-VMware-Part2)   \||   [Part 3](/posts/RDS-on-VMware-Part3)

<br>
Welcome to Part 3 of deploying RDS on VMware. In this post we are going to deploy a new MySQL RDS instance into our vSphere environment and connect it to an Ubuntu VM that will run WordPress.

Navigating to our AWS RDS console, go ahead and click on Create database. A new page will pop up where we will input our database settings. 

- Select On-Premises under the database location options and then select RDS on VMware. Our custom Availability Zone will be selected by default. 

- Next I am going to select MySQL as the database engine.

![Desktop View](/assets/posts/rds_on_vmware_p3/1.png){: .normal}

Now we need to input the settings for our database including the database instance ID, username and password.

![Desktop View](/assets/posts/rds_on_vmware_p3/2.png){: .normal}

We will now select the instance size for our database as well as the allocated storage capacity. I have chosen to go with 1vCPU and 2GB of RAM and 20GB of storage.

![Desktop View](/assets/posts/rds_on_vmware_p3/3.png){: .normal}

Since this a lab environment, under the Additional configuration section I have disabled automatic backups. I have also specified an initial database to be created names WordPress. Go ahead and click Create database to start the provisioning process.

![Desktop View](/assets/posts/rds_on_vmware_p3/4.png){: .normal}

Now that our database instance is provisioned, we need to grab the endpoint address so that we can configure WordPress. 

![Desktop View](/assets/posts/rds_on_vmware_p3/5.png){: .normal}

I already have WordPress installed on my Ubuntu VM so all we need to do is create a php config file with the database settings. I have highlighted the RDS endpoint in red.

![Desktop View](/assets/posts/rds_on_vmware_p3/6.png){: .normal}

Now that the database settings have been configured for WordPress, we should get the WordPress Welcome page when we navigate to our site. Success!

![Desktop View](/assets/posts/rds_on_vmware_p3/7.png){: .normal}

And with that, we now have a WordPress website that is utilizing a MySQL backend that has been provisioned through AWS RDS into our on-premises vSphere environment. At this point we can do things like take snapshots, install available update patches and monitor our RDS instance. This was a really fun series to write and hopefully it helps those out there that are looking to deploy RDS on VMware in their environment. As always if you have any questions please don’t hesitate to reach out!

![Desktop View](/assets/posts/rds_on_vmware_p3/8.png){: .normal}