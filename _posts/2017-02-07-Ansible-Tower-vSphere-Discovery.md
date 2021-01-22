---
title: Ansible Tower & vSphere Dynamic Discovery
date: 2017-02-07
tags: [Ansible, Configuration Management, VMware]     # TAG names should always be lowercase
---
It’s a new year, which means it’s time to learn new things!

For the past 6 months, I’ve wanted to dive deeper into Ansible as a way to continuously force myself to write code in python. Naturally, I decided I would deploy Ansible Tower to monitor and manage my lab environment for changes and any configuration drift – there are two of us in the lab now, and we can’t afford to have any finger pointing =). As I was planning the inventory setup for the environment, I was a bit disappointed that there was no out of the box way to separate the Windows and Linux VM’s when using vCenter as an inventory source.

After digging through the Ansible docs, they sent me over to this gem of a GitHub repo – [Link.](https://github.com/ansible/ansible/tree/devel/contrib/inventory) I immediately found the vmware inventory python script and accompanying ini configuration file. There was only one problem.. It seems as if most of these custom inventory scripts are built for use without Ansible Tower. After spending a few hours reading through some docs and of course trial and error, I finally got it figured out.

Note: I assume that you have a fully functional Ansible Tower environment deployed.

First off, we need to setup the vmware_inventory.ini file.

- Copy the vmware_inventory.ini file to the “/etc/ansible/“ directory on the Ansible server.

![Desktop View](/assets/posts/ansible_vsphere_discovery/1.png){: .normal}

- Open up the  vmware_inventory.ini file with vi or nano.

- Replace the values for server, username and password.

- Save the file once you are done editing.

Now that our inventory file is setup, we need to create a custom inventory script in Tower. To do this, we need to navigate to Settings > Inventory Scripts. Click on the +ADD button and give it a name.

- Copy the contents of the vmware_inventory.py file into the custom script text box.

![Desktop View](/assets/posts/ansible_vsphere_discovery/2.png){: .normal}

- Do a search for the following text:  `‘ini_path’: os.path.join(os.path.dirname(__file__), ‘%s.ini’ % scriptbasename)`

- We need to replace `‘ini_path’: os.path.join(os.path.dirname(__file__), ‘%s.ini’ % scriptbasename)` with this: `‘ini_path’: ‘/etc/ansible/vmware_inventory.ini’`

- Old INI_PATH Setting: `’ini_path’: os.path.join(os.path.dirname(__file__), ‘%s.ini’ % scriptbasename),`

- New INI_PATH Setting: `‘ini_path’: ‘/etc/ansible/vmware_inventory.ini’,`

- Go ahead and click Save for the custom inventory script.

Next, we need to setup our new inventory. To do this click on Inventories and select +ADD.

- Give the inventory a name and click Save.

- Next, Click on +ADD GROUP.

- Give the Group a name and then select Custom Script from the Source dropdown menu.

- Click on the magnifying glass under Custom Inventory Script and select the script we just created.

- Click Save and then scroll to the bottom of the page.

- Run the initial sync for our newly created group.

![Desktop View](/assets/posts/ansible_vsphere_discovery/3.png){: .normal}

If the run was successful, we should see the VM’s added to our newly created inventory! Now for my specific use case, I needed to create multiple inventory files that had the appropriate host filters set. One to limit the scope to Windows VM’s and the other to limit the scope Linux VM’s. I’ve uploaded the modified vmware_inventory.ini and vmware_inventory.py files to my GitHub page for easy review. And with that, it’s time to start writing some playbooks!

- [Modified Ansible Files](https://github.com/vCarbs/Ansible)