---
title: vCenter Orchestrator Windows Upgrade
date: 2015-02-27
tags: [vCenter Orchestrator]     # TAG names should always be lowercase
---

At the company I work for, we just recently upgraded our vCenter Server from 5.5U1 to 5.5U2. To keep in sync with the versioning we also decided to upgrade our vCenter Orchestrator instance. The process is very simple, however I wanted to document it so that I can hand it off to another engineer when it’s time to upgrade vCO in a different site. That being said, let’s jump into the upgrade process for vCO running on Windows Server.

1.) Login to the vCO configuration portal at https://yourvco:8283

2.) Under the general tab navigate to “Export Configuration”. Enter a password if you would like and click export. The config file will be saved to C:\Program Files\VMware\Orchestrator\

![Desktop View](/assets/posts/vco_upgrade/1.png){: .normal}

3.) Next we need to back up the MSSQL database that stores all of the vCO information.

![Desktop View](/assets/posts/vco_upgrade/2.png){: .normal}

4.) Once we have the database backed, up we are going to copy the following folders to our desktop. These folders contain all of the plugins and their configurations.

A.) install_directory\VMware\Orchestrator\app-server\plugins

![Desktop View](/assets/posts/vco_upgrade/3.png){: .normal}

B.) install_directory\VMware\Orchestrator\app-server\conf\plugins

![Desktop View](/assets/posts/vco_upgrade/4.png){: .normal}

5.) Extract the vCenter Orchestrator executable from the vCenter installation media and copy it to the vCO Server.

6.) Take a snapshot of your vCO Server.

7.) Launch the vCO installer.

8.) We are now prompted with a warning that an existing installation has been found, and are presented with recommended backup steps before continuing. If you are satisfied with your level of backups, click on continue with update.

![Desktop View](/assets/posts/vco_upgrade/5.png){: .normal}

9.) We now need to select the components we want to update:

![Desktop View](/assets/posts/vco_upgrade/6.png){: .normal}

10.) Select the installation path and click install!

Once the installation is complete, make sure to run some workflows to validate that your plugins, configurations, etc are functioning properly. If all looks good, we can go ahead and remove the snapshot we took earlier in Step 6.