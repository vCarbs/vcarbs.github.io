---
title: HP Agentless Monitoring Service Bug
date: 2014-11-06
---

Early last week, I ran into some issues where recently patched hosts were unable to perform any vMotion operations, or provision new data stores. After some investigation, we validated that all of the guest VM’s were still online and functional, however, we could not enable any type of management service, including SSH. Once we logged into the iLO portal, we started to see the host get spammed with the following error message: “can’t fork”.

![Desktop View](/assets/posts/hp_ams/1.png){: .normal}

After some in-depth and resource intensive research (Googling) we quickly found out that this is a known bug caused by two specific versions of the HP-AMS service that is included with the custom HP ESXi ISO. (I have included the versions as well as a link to the KB below).

Knowing now what the issue was, we waited until we had a maintenance window that night and performed the recommendations provided in the VMware KB, which is essentially an uninstall of the problematic VIB and a reboot of the host. To give you a high level overview, the HP-AMS service enables Agentless Management to monitor certain components through iLO, including CPUs, Memory, Temperature Sensors, Fans, Power Supplies, etc. After a quick round table with the team going over the benefits and use case of re-installing an older version, we decided that it was not worth the effort and to leave it uninstalled. It will be up to the system owner to determine if they truly need this functionality.

Check out the KB Article [here.](http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2085618)