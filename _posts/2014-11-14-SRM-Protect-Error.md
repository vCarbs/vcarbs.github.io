---
title: SRM – Protect VM Error
date: 2014-11-14
tags: [Site Recovery Manager]     # TAG names should always be lowercase
---

Over the last few months, I’ve spent a considerable amount of time deploying and configuring services in our Disaster Recovery datacenter. The majority of services had been configured, and SRM was one of the the last remaining items that needed to be deployed within our environment. Last week, I ran into some issues while configuring an SRM Protection Group. All steps were completing successfully, except certain VM’s would fail during the “Protect VM” task. After reading KB article [2078854](http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2078854), I validated and confirmed that all of the inventory mappings were configured correctly. Yet, the issue persisted.

![Desktop View](/assets/posts/srm_protect_error/1.png){: .normal}

After looking through the settings of some of the newly provisioned virtual machines, I noticed that each of them had their CD/DVD drive mapped to a Windows ISO located on our template datastore. We had recently made some changes to our Gold Images, and the ISO was accidentally left mounted once we converted the virtual machine back to a template. Easy fix, right? I quickly validated the issue by changing the CD/DVD device type back to “Client Device” and launched the “Protect VM’s” action from the SRM client. Once that was complete, the “Protect VM” task went through without issues, the VM object was successfully placed at the Disaster Recovery site, and order was restored, staving off widespread hysteria.

![Desktop View](/assets/posts/srm_protect_error/2.png){: .normal}

All jokes aside, the main take away from this is to make sure you have a detailed process in place that is rock-solid. Although this was a relatively small issue, it took a lot more time to solve than I would like to admit, and the result was an extended timeframe to deploy and test SRM in our environment all because of an oversight during template changes. Hopefully, this saves someone some of that time.