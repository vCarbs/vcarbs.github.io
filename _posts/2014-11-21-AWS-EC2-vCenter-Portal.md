---
title: Amazon Management Portal for vCenter – VM to EC2 Instance
date: 2014-11-21
tags: [AWS]     # TAG names should always be lowercase
---

Unless you’ve been under a rock for the past few years, you more than likely have heard of “cloud computing” and the major players that eat up the marketshare in that space. The company I work for has been utilizing Amazon Web Services for the past year, and the journey to get there all started with a project that needed to utilize object based storage. I’ll share that journey with you all shortly, but for now I would like to cover a recently developed plugin for us Virtualization Admins.

From my experience and the discussions I have had with other IT Professionals in the past, being a Virtualization Admin does not inherently mean that you will be managing the company’s public cloud infrastructure. However, you still need to be able to manage, control and audit Virtual Servers whether they are hosted on-prem, or off-prem with a provider like Amazon with EC2. And what about the guys who are responsible for provisioning, maintaining and managing the Virtual Servers in the on-prem infrastructure? Should they have access to the AWS management portal? With the recently announced Amazon Management Portal for vCenter, there is finally a solution for the Virtualization Admin without having to implement a complete Cloud Management Platform such as vCloud Automation Center.

Deploying the AWS Management Portal for vCenter is a fairly straightforward process. Before you get started, I highly recommend reading the setup guide which can be found [here.](http://docs.aws.amazon.com/amp/latest/userguide/setting-up.html) In this post I want to cover the steps it takes to import an existing VM into an AWS EC2 instance.

– Login to your vCenter Server and shutdown the VM you would like to import.

– Right click on the Host you want import, and select the “Migrate to EC2” option.

– A new window will pop up, and here we will define any settings for this VM.

– Once you have input all of the settings, click on the “Migrate to EC2” button.

– Another window will pop up and display the Import Task ID as well as the new Instance ID.

– Under the vCenter Tasks pane you will see an Export OVF Template task start.

– Once the task is complete, navigate over to your AWS console and validate that the VM has been imported as an instance, as seen below.

![Desktop View](/assets/posts/aws_ec2_migration/1.png){: .normal}

If you are currently a customer of Amazon Web Services, then I would highly recommend deploying this management portal in your environment. Sure, importing a VM from vSphere to AWS is pretty cool, but the bottom line is that the major benefits can be seen only when you are able to manage both of your vSphere and AWS environments from a central location. The AWS Management Portal plugin for vCenter accomplishes this.