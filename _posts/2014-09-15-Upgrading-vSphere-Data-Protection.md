---
title: Upgrading vSphere Data Protection 5.5.6 to 5.8
date: 2014-09-15
#categories: [TOP_CATEGORIE, SUB_CATEGORIE]
tags: [vSphere Data Protection]     # TAG names should always be lowercase
---
For those of you that don’t know, VMware recently released vSphere 5.5 Update 2 which includes updates to ESXi, vCenter Server, SRM, and more which I have listed at the end of this post. I have already updated my vCenter Server and ESXi Hosts in the lab, so the objective of this post will be documenting the steps of upgrading the vSphere Data Protection Appliance.

1. Upload the vSphereDataProtection-5.8 ISO to a datastore that the VDP appliance has access to.

1

2. Mount the vSphereDataProtection-5.8 ISO to the VDP appliance.

2

3. Login to the vSphere Data Protection Configuration Utility web portal at https://ip_address:5840/vdp-configure.

3

4. Navigate to the Upgrade Tab and the latest upgrade will be available for review.

4

5. Before proceeding with the upgrade, take a snapshot of the system otherwise you will receive a message like this.

5

6. Now that we have a snapshot in place, we can go ahead and upgrade the appliance. Depending upon the
performance capabilities of your environment, this may take a few hours so go ahead and grab a cup of coffee!

6

7. Once the update completes, the Virtual Appliance will be in a Powered Off state. At this point we need to power it on through the vSphere Client.

8. Once you login to the appliance you see will see a different UI than you are used to. Although minor in the grand scheme of things, I really like the look and feel of the new vSphere Data Protection Configuration Utility.

7

After performing the upgrade, I highly recommend testing Backup and Restore capabilities before deleting the pre-installation snapshot.

Recently Upgraded Products
VMware ESXi 5.5.0 Update 2
VMware vCenter Server 5.5 Update 2
VMware vSphere Data Protection 5.8
VMware vSphere Replication 5.8
VMware vCenter Operations Manager Standard 5.8.3
VMware vCenter Orchestrator Appliance 5.5.2
VMware vCloud Automation Center 6.1
VMware vCloud Director 5.5.2
VMware vCloud Networking and Security 5.5.3