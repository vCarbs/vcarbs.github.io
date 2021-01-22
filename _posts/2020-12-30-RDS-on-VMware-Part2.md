---
title: Amazon RDS on VMware – Part 2 – vSphere Cluster Onboarding
date: 2020-12-30
tags: [AWS, RDS on VMware, VMware]     # TAG names should always be lowercase
---
*Note: This is part 2 of a 3 part series.* 
[Part 1](/posts/RDS-on-VMware-Part1)   \||   [Part 2](/posts/RDS-on-VMware-Part2)   \||   [Part 3](/posts/RDS-on-VMware-Part3)

<br>
Now that the prerequisites have been completed, we are ready to proceed with the deployment of RDS on VMware. Part 1 of the deployment can be viewed here.

The first thing we are going to do is create a new resource pool within vCenter. This is where all of the RDS on VMware components will be deployed. Since this is a lab environment, I left everything as default.

![Desktop View](/assets/posts/rds_on_vmware_p2/1.png){: .normal}

Now from the AWS console, we need to create a new Custom Availability Zone within the RDS console and download the installer.

![Desktop View](/assets/posts/rds_on_vmware_p2/2.png){: .normal}

These are the settings I used for my custom AZ.

![Desktop View](/assets/posts/rds_on_vmware_p2/3.png){: .normal}

Next, we are going to deploy the downloaded virtual appliance into our vSphere environment. Make sure that you have DHCP enabled otherwise the virtual appliance will not get an IP address.

![Desktop View](/assets/posts/rds_on_vmware_p2/4.png){: .normal}

Once the virtual appliance is deployed and powered on, grab one of the IP addresses and open it up in a web browser. Example: https://10.1.200.110/ui

Now we need to enter in our AWS Access Key ID and AWS Secret Access Key from the CSV file we downloaded during the user creation process.

![Desktop View](/assets/posts/rds_on_vmware_p2/5.png){: .normal}

Select the region in which you created your custom RDS AZ, then select your custom AZ.

![Desktop View](/assets/posts/rds_on_vmware_p2/6.png){: .normal}

These are the network settings for my environment.

![Desktop View](/assets/posts/rds_on_vmware_p2/7.png){: .normal}

On the next page, enter in your vCenter credentials and then select your placement details. Next, hit validate (This can take a few minutes depending on the available resources in your environment).

![Desktop View](/assets/posts/rds_on_vmware_p2/8.png){: .normal}

If the validation checks complete successfully, we can move on to the summary page and finally to the deployment. At this point, the installer will start to deploy 8 different virtual machines into the environment. **This process can take a few hours to complete.**

![Desktop View](/assets/posts/rds_on_vmware_p2/9.png){: .normal}

**Note: A couple of things I want to point out.** The virtual appliance we initially deployed (RDS on VMware Installer) will automatically shut itself down once it has completed all of its tasks. **Make sure you do not power it back on.** Secondly, you can quickly check the VPN status between the RDS Edge Router and AWS by opening up the RDS Edge Router console. **It took about 10 minutes to establish connectivity in my environment so be patient.**

![Desktop View](/assets/posts/rds_on_vmware_p2/10.png){: .normal}

Now that we have our newly deployed 8 virtual machines, we can head back to the RDS console and verify that our custom AZ is now in the Active state. 

![Desktop View](/assets/posts/rds_on_vmware_p2/11.png){: .normal}

At this point we are now ready to start deploying databases from RDS into our vSphere environment. **One thing to note, if you are planning to create a DB instance with a licensed engine, such as SQL Server, you must import your operating system media and database engine media before you can create an Amazon RDS DB instance.** If you are deploying MySQL or PostgreSQL, you can go ahead and start provisioning!

In [Part 3](/posts/RDS-on-VMware-Part3) I will cover provisioning a new RDS instance and connecting to it from a virtual machine.