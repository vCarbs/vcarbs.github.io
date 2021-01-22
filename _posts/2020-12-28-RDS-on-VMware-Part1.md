---
title: Amazon RDS on VMware – Part 1 – Prerequisites
date: 2020-12-28
tags: [AWS, RDS on VMware, VMware]     # TAG names should always be lowercase
---
*Note: This is part 1 of a 3 part series.* 
[Part 1](/posts/RDS-on-VMware-Part1)   \||   [Part 2](/posts/RDS-on-VMware-Part2)   \||   [Part 3](/posts/RDS-on-VMware-Part3)

<br>
In this post I am going to cover the initial prerequisites for deploying Amazon RDS on VMware. I plan on writing additional posts that cover a new database deployment as well as migrating my Veeam database from a standalone Microsoft SQL Server to an Amazon RDS on VMware instance. Here is a link to the Amazon RDS on VMware User Guide: [https://docs.aws.amazon.com/AmazonRDS/latest/RDSonVMwareUserGuide/rds-on-vmware.html](https://docs.aws.amazon.com/AmazonRDS/latest/RDSonVMwareUserGuide/rds-on-vmware.html)

For this deployment, I have created a new nested vSphere environment based on William Lam’s content library which you can learn about [here.](https://www.virtuallyghetto.com/nested-virtualization/nested-esxi-virtual-appliance)

- ESXi Version: 6.5U3
- vCenter Version: 6.7 Update 2 (I ran into issues when using different minor versions and ended up deploying Build: 13007421)

<br>
Now the first thing we need to do is to prepare the vSphere environment so that we can onboard our cluster into Amazon RDS. I will be utilizing a single network (10.1.200.0) for the Internet Network, Application Network and Management Network. For the Cluster Control Network, I created a new Port Group with a VLAN of 300 (Note: This network should not be routable).

**Cluster Control Network Port Group:**

![Desktop View](/assets/posts/rds_on_vmware_p1/1.png){: .normal}

Now that our Cluster Control Network Port Group is created, we need to create vmkernel adapters with vSphere Replication and vSphere Replication NFC traffic enabled on each of our ESXi hosts. For the IP address, make sure to use automatic assignment.

![Desktop View](/assets/posts/rds_on_vmware_p1/2.png){: .normal}

With the Cluster Control Network configuration completed, we are now going to create a new user and assign them the below policy within the AWS IAM console. Note: Make sure to download the AWS Access Key ID and Secret Access Key csv file when creating the user. We will need this later on.

![Desktop View](/assets/posts/rds_on_vmware_p1/3.png){: .normal}

Policy JSON document:
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "RDSonVMware",
            "Effect": "Allow",
            "Action": [
                "rds:DescribeCustomAvailabilityZones",
                "rds:RegisterCustomAvailabilityZone"
            ],
            "Resource": "*"
        }
    ]
}
```                

Before we can move on to the onboarding of our vSphere cluster, we need to create a new zone that will map **rdsonvmware.rds.amazonaws.com** to the IP address of our RDS Edge Router IP. I know we have not yet deployed the RDS Edge Router but I will be using **10.1.200.50** for this.

Heading over to my domain controller, I am going to create a new Conditional Forwarder with the information I specified above.

![Desktop View](/assets/posts/rds_on_vmware_p1/4.png){: .normal}

Please join me in [Part 2](/posts/RDS-on-VMware-Part2) as we go through the vSphere Cluster onboarding process!