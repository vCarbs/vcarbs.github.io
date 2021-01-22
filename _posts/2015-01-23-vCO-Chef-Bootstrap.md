---
title: Bootstrapping the Chef Windows Client Through vCenter Orchestrator
date: 2015-01-23
tags: [vCenter Orchestrator, Chef, Configuration Management]     # TAG names should always be lowercase
---
Over the past few weeks, our team has been talking internally about Configuration Management and why we need to get it implemented. This is an important step in the evolution of our IT Infrastructure Management and is something that every serious shop should consider. We have a ton of deployment scripts written in PowerShell, however we had no way to track Configuration Drift and have succumbed to the Snowflake Syndrome. We had initially started looking at vCenter Configuration Manager, but ultimately decided against it due to the lack of documentation and community support. We knew that the engineers who oversee the linux environment are using Chef, and after a two hour conversation, we took a serious look at that option. At this point, we’ve written a few Cookbooks that specifically address MSSQL and IIS, as these are the two types of environments we needed to get into Configuration Management first. With that being said, the need to automate the bootstrap process of the Chef client also presented itself, and what better way to accomplish that then through vCO!

<br>
First, we need to setup our PowerShell Host with the Chef Development Kit. I am going to provide a very high level overview of setting up a Chef workstation, in this case it’s our PowerShell Host used by vCenter Orchestrator. If you have not done so already, follow the documentation to get a PowerShell Host setup in vCO as well as setup a Chef workstation node. [ChefDK Documentation](http://docs.chef.io/devkit/install_dk.html) & [vCO PowerShell Host](http://blogs.vmware.com/orchestrator/2011/12/vco-powershell-plug-in.html)

. Download and install the Chef Development Kit on your PowerShell Host

. Once we have the Chef Development Kit installed, we need to install the knife-windows gem.

. In PowerShell or a Command Prompt navigate to the following directory: C:\opscode\chefdk\embedded\bin

. Once we are in the directory, validate that you can run the following command:

`.\gem`

![Desktop View](/assets/posts/vco_chef/1.png){: .normal}

. We now need to run the following command to install the required knife-windows gem:

`.\gem install knife-windows`

. We should now see the package being installed like the below image:

![Desktop View](/assets/posts/vco_chef/2.png){: .normal}

. Now that we have installed the knife-windows gem, it is time to download our starter kit and create a directory to store our local repo.

. With that complete, open up PowerShell and navigate to the Chef Repo directory. I am using C:\chef-repo for this example.

. Let’s validate that we can bootstrap a Windows node by running the following command:

`knife bootstrap windows winrm localhost`

. If everything is working we should be prompted with the below message:

![Desktop View](/assets/posts/vco_chef/3.png){: .normal}

. Finally, it’s time to open up our PowerShell ISE and write the script that will allow us to bootstrap the node. Enter the below snippet of code into your ISE console:


```powershell
# Parameters gathered from vCenter Orchestrator
 
param(
 
[string]$vmName,
 
[string]$User,
 
[string]$Password
 
)
 
  
# Create a credential store object
 
$SecurePassword = Convertto-SecureString -String $Password -AsPlainText -force
 
$mycred = New-Object System.Management.Automation.PSCredential $User, $SecurePassword
 
  
 
# Set winrm timeout value. Depending on the location of you Chef server it may take a while to download the required packages.
 
Invoke-Command -ComputerName $vmName -ScriptBlock {set-Item wsman:\localhost\Config\MaxTimeoutms -value '1800000' -force} -Credential $mycred
 
 
# Bootstrap the node with Chef client. We need to launch the knife command from the Chef repo folder.
 
Set-Location -Path "c:\chef-repo"
 
& knife bootstrap windows winrm $vmName -x $User -P $Password
```

. Save the file to your PowerShell Host as “chef_bootstrap.ps1”. We will be calling this script from vCO later on.

. Open up vCO and create a new workflow named “Chef Bootstrap”.

. Drag the the “Scriptable task” object into the workflow.

. Add the “Invoke an external script” object into the workflow and click on setup:

![Desktop View](/assets/posts/vco_chef/4.png){: .normal}

. Input the following values:

. Host: Select the PowerShell host for our environment.

. externalScript: This is the location of the PowerShell script “chef_bootstrap.ps1”. My value is C:\scripts\chef_bootstrap.ps1

. arguments: Leave this blank.

. Click on “promote” to add these values into the workflow.

![Desktop View](/assets/posts/vco_chef/5.png){: .normal}

. Under the General tab, add the following Attributes:

. User. We will leave the type as a string and enter in our username.

. Password. We need to change the type to SecureString and enter in the password for our username.

![Desktop View](/assets/posts/vco_chef/6.png){: .normal}

. Now under the Inputs tab add a new parameter called “vmName”. Leave the type as a string.

![Desktop View](/assets/posts/vco_chef/7.png){: .normal}

. Under the Schema tab we need to edit the “Scriptable task” and add the following settings.

. Bind vmName, User and Password as inputs.

![Desktop View](/assets/posts/vco_chef/8.png){: .normal}

. Bind arguments as an output.

![Desktop View](/assets/posts/vco_chef/9.png){: .normal}

. Validate the bindings by looking at the Visual Binding tab.

. Under the Scripting tab, add in the following lines of code. This will pass through the required parameters to the PowerShell script.


```powershell
arguments =     " -vmName " + vmName +
 
                " -User " + User +
 
                " -Password '" + Password + "'" ;
```

. Close out of this window. It is now time to debug our workflow.

. Click on the debug button and enter in the hostname of a Windows server you want to bootstrap with the Chef client:

![Desktop View](/assets/posts/vco_chef/10.png){: .normal}

. If we navigate to our Chef Server, we should see the newly bootstrapped node.

Although this works, there are errors in the vCO workflow even though the bootstrap process completed successfully. I spent a good amount of time trying to figure this one out and ended up having to rely on an alternate validation solution which I will document in a later blog post. This process has worked well for us as we also provision our servers through a vCO workflow. Ultimately, it is up to you and your team to decide the process and tools that will help you scale, audit, and automate your infrastructure for future needs.