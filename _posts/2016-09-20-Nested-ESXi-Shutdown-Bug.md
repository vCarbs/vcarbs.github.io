---
title: Nested ESXi Shutdown Bug
date: 2016-09-20
tags: [VMware, Home Lab, Nested]     # TAG names should always be lowercase
---
I recently decided to update my home lab with some new gear. The old 3 node cluster worked well however, I was running out of capacity and frankly they took up too much space in my office. I plan to write a post on the new setup but for now, let’s take a look at this issue with Nested ESXi!

As part of this new lab build out, I wanted to run a majority of the workloads on nested ESXi hosts. I’ve done this before and honestly it works well and is better than dropping the cash for additional hardware! If you haven’t worked with nested hosts before, I highly recommend heading over to William Lam’s blog and checking out his posts on the subject – [http://www.virtuallyghetto.com/nested-virtualization](http://www.virtuallyghetto.com/nested-virtualization)

I built out my 3 nested hosts, connected them to my NAS and all looked good. I then started working on a new CentOS template for the lab. After installing all of the required software and preparing it for use as a template, I went to shutdown and convert it. This is the point where I started to run into issues. As soon as I sent the “Initiate guest shutdown” command, I noticed that the nested host running this VM dropped from vCenter.. I couldn’t ping it however, I was able to login to the console. 

![Desktop View](/assets/posts/nested_shutdown_bug/1.png){: .normal}

Once logged into the console I proceeded to restart the management network and boom, the nested host instantly re-connected to vCenter. Thinking this was odd I spent the next 1-2 hours testing and tweaking settings. I won’t bore you with the details of all of that testing.. More importantly I was able to find a solution.

Option 1: Create a separate standard vSwitch for virtual machine traffic

Option 2: Create a distributed vSwitch for virtual machine traffic

For whatever reason, nested ESXi did not seem to want to run the management vmkernel and virtual machine traffic from the same vSwitch. As soon as I made these changes I was able to shutdown the VM without the nested hosts dropping connectivity. Hopefully this post helps in the event you also run into a problem with nested hosts!