---
title: Nimble Storage Array Upgrade
date: 2014-09-15
tags: [Nimble Storage]     # TAG names should always be lowercase
---
It’s one of the things we all get to look forward to as a customer: Storage Array Upgrades. When planning our data center migration last year, the company I work for decided to purchase arrays from Nimble Storage. Overall the arrays have met our requirements of both our Virtual Server and Virtual Desktop infrastructures.

Now that the table has been set, it’s time to upgrade the array from version 1.4.11 to 2.1.4. The upgrade of the Nimble Array is extremely easy, however since we need to take advantage of replicating over the 10GbE interfaces, we will need to setup some static route entries after the upgrade is finished. During the upgrade process, the standby controller will be upgraded first. Following a reboot, the controller will failover from Node A to the recently upgraded Node B, and the same process will occur on Node A. Tip: If your array has less than 25% available capacity, hold off on the upgrade. There is currently a bug in the software that can potentially cause performance degradation.

1.) Log in to the Nimble Array.

![Desktop View](/assets/posts/nimble_upgrade/1.png){: .normal}

2.) Navigate to the Administration drop-down, and select the Software option.

![Desktop View](/assets/posts/nimble_upgrade/2.png){: .normal}

3.) At this point, we need to download the desired software version. In my case, I have already pre-downloaded the version we have tested to save time during the upgrade process.

![Desktop View](/assets/posts/nimble_upgrade/3.png){: .normal}

4.) Once the new software version has been downloaded, we are ready to launch the missile! The entire upgrade process will take around 45 minutes.

![Desktop View](/assets/posts/nimble_upgrade/4.png){: .normal}

5.) After the first controller comes back online and the failover occurs, we will get an error message stating that the array has timed out. Refresh the page and we will be presented with the new login page, where we now need to specify a username due to the fact that this release includes Role Based Access Control. Riveting, I know!

![Desktop View](/assets/posts/nimble_upgrade/5.png){: .normal}

![Desktop View](/assets/posts/nimble_upgrade/6.png){: .normal}

6.) Navigate back to the Software page and we will see that the software upgrade is being performed on the Secondary Controller. Once that upgrade is complete, we will be left with a successfully upgraded Nimble Array.

![Desktop View](/assets/posts/nimble_upgrade/7.png){: .normal}

7.) Next up is adding in the Static Routes to allow us to replicate data over the 10GbE interfaces. Heading over to the Network Configuration page under the Administration tab, we need to edit the Active Settings.

![Desktop View](/assets/posts/nimble_upgrade/8.png){: .normal}

8.) Hit the Edit button in the top right corner, and start adding the required routes.

![Desktop View](/assets/posts/nimble_upgrade/9.png){: .normal}

9.) Once all of the static routes have been added, we need to update the configuration by selecting the Update button in the top right corner.

![Desktop View](/assets/posts/nimble_upgrade/10.png){: .normal}

At this stage we have an upgraded Nimble Array running the latest software version that has been tested in our Staging environment. We also have the ability to replicate data over the 10GbE interfaces vs the 1GbE Management Interface. This will allow us to take advantage of the 10GbE circuit between our two data centers. I suggest monitoring the array for 20 or so minutes to validate that all operations are fully functional.