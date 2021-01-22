---
title: Configuring the NetApp Simulator with Chef
date: 2017-02-15
tags: [Chef, Configuration Management, NetApp]     # TAG names should always be lowercase
---
Late last year, I decided that I would try to automate the deployment of the NetApp Virtual Simulator. The organization I work for needed a method to easily and consistently deploy the simulator for customer demos and workshop sessions. Fortunately for me, Chef had already produced a cookbook that did exactly this, although I did run into a few issues getting it setup, which is why this blog post exists.

Below are the steps needed to automate the simulator using Chef. I will not cover the base deployment of the NetApp Simulator in this post, nor will it cover getting chef solo deployed. If you need help with either of these, please check out these great resources. [NetApp Setup](https://vmstorageguy.wordpress.com/2015/08/18/netapp-ontap-simulator-8-3-how-to/) // [Chef Quick Start](https://docs.chef.io/quick_start.html)

1.) Grab the Chef cookbook from their GitHub repo: [NetApp cookbook](https://github.com/chef-partners/netapp-cookbook)

2.) Place the cookbook in the cookbook directory inside of the chef-repo directory.

![Desktop View](/assets/posts/netapp_simulator_chef/1.png){: .normal}

3.) Download the NetApp ruby api files and place it inside of the “libraries” directory inside of the cookbook. These can be downloaded from the NetApp Support portal located here: [NetApp Ruby Files](http://mysupport.netapp.com/NOW/cgi-bin/software?product=NetApp+Manageability+SDK&platform=All+Platforms) 

![Desktop View](/assets/posts/netapp_simulator_chef/2.png){: .normal}

4.) Next, we need to edit the *NaServer.rb* file and specify the path of NaElement. Replace the line: `require NaElement` with the following: `require File.dirname(__FILE__) + “/NaElement"`

![Desktop View](/assets/posts/netapp_simulator_chef/3.png){: .normal}

Now that we have the necessary files to interact with the NetApp API, we need start modifying our cookbook.

1.) Open the default.rb file located in the “attributes” directory. Edit the file to match the settings you supplied during the NetApp setup.

2.) In the same default.rb file, we need to add an additional line of code or else running the cookbook will error out. New line of code to add:
`default[:netapp][:api][:timeout] = 999`

![Desktop View](/assets/posts/netapp_simulator_chef/4.png){: .normal}

3.) Next, we need to edit the default.rb file located in the “recipes” directory.

Now you will notice that this file is empty. We have two options here. We can add all the functionality we want into the default.rb file or simply include the functionality by calling the other ruby files, such as aggregate.rb. For simplicity, I decided to add all the configuration information into the default.rb file. Below is a sample of what my configuration looks like:

```ruby
# Cookbook Name:: netapp
# Recipe:: default
netapp_aggregate 'aggr1' do
  disk_count 20
 
  action :create
end
 
netapp_svm 'nfs-svm' do
  security 'unix'
  aggregate 'aggr1'
  volume 'nfs_root'
  nsswitch ['file']
 
  action :create
end
 
netapp_feature 'nfs' do
  codes ['MBXNQRRRYVHXCFABG']
 
  action :enable
end
 
netapp_role 'vcarbs-role' do
  svm 'nfs-svm'
  command_directory 'volume'
 
  action :create
end
 
netapp_user 'vcarbs-user' do
  vserver 'nfs-svm'
  role 'vcarbs-role'
  application 'ontapi'
  authentication 'password'
  password 'password1'
 
  action :create
end
 
netapp_lif 'nfs-lif01' do
  address '172.25.0.100'
  home_node 'lab-01'
  home_port 'e0a'
  role 'data'
  svm 'nfs-svm'
  netmask '255.255.255.0'
 
  action :create
end
 
netapp_nfs 'nfs-svm' do
  pathname '/vol/nfs_root'
 
  action :enable
end
 
netapp_nfs 'nfs-svm' do
  pathname '/vol/nfs_root'
  persistent true
  read_write_all_hosts true
 
  action :add_rule
end
 
netapp_volume 'vmware' do
  svm 'nfs-svm'
  aggregate 'aggr1'
  size '10g'
  percentage_snapshot 0
  junction_path '/vmware'
  is_junction_active true
  export_policy 'default'
 
  action :create
end
```

With our cookbook complete, it’s time to setup chef solo to automate the deployment tasks.

1.) Create a new file called “node.json” within the “chef-repo” directory.

2.) Add the following code and save the file.

```json
{
"run_list": [ "recipe[netapp]" ]
}
```

3.) In the same directory, create a file named “solo.rb”. Add the following lines of code but make sure to replace each line with the path to your chef-repo directory.

```ruby
file_cache_path "/vCarbs/vCarbs Blog Posts/NetApp Automation/chef-repo"
cookbook_path "/vCarbs/vCarbs Blog Posts/NetApp Automation/chef-repo/cookbooks"
json_attribs "/vCarbs/vCarbs Blog Posts/NetApp Automation/chef-repo/node.json"
```

And with that it’s time to run the cookbook.

1.) Open a terminal and navigate to the chef-repo directory

2.) Enter the following command to run the cookbook: `chef-solo -c solo.rb`

If the run is successful, we should receive an output like the below image. And with that, we have automated the deployment of the NetApp Virtual Simulator!

![Desktop View](/assets/posts/netapp_simulator_chef/5.png){: .normal}