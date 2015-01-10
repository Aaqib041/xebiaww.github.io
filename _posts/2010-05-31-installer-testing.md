---
layout: post
header-img: img/default-blog-pic.jpg
author: rnamta
description: 
post_id: 3729
created: 2010/05/31 14:45:23
created_gmt: 2010/05/31 09:45:23
comment_status: open
---

# Installer Testing

Ever seen a Formula 1 car lined up in the front row on race day failing to take off the grid and the hard work put in by the whole team coming to a naught due to some minor glitch in the start-up system. That's probably the same feeling when a software meant to make possible the impossible fails to install at the first place.

Testing of the installer assumes significant importance since this is the first experience of your customer with your product. In my opinion installer testing is not only all about [Installation testing ][1]but it also falls into the realm of [Usability][2] or User experience testing. A good installer must be like a good browser which quickly gets out of your way and lets you start with the real thing.

Recently I have been involved in testing the installer for one of the products we are creating for our customers. This blog is specific to experiences with installer testing. Additionally this blog is not meant to be a set of instructions or checklist for testing a installer. Here I have tried to explain a generic set of guidelines which should help you to come up with a test strategy which is most suitable for testing your installer.

### Clarify the scope

Testing a installer; how trivial and simple it may seem, requires systematic analysis of the application, its users and the technology involved to come up with a good test approach. Even before you start testing your installer you should have information related to its distribution and support.

**Delivery of the software installer: **How the installer itself is being pushed to the end users machine(over network, web, using Hdd or CD etc). The distribution of the installer is important as you need to see the size and the time it takes to download or install via Internet. Additionally poor network connection or security settings within a company premises may actually prevent downloading or corrupt the installer even before its delivered!

**Operating systems and platform support: **It should be clear stated at the very beginning of the development process that what all Operating systems and there versions are supported. In all likelihood you will have separate installers for separate platforms. Having a clear idea of the platform support would enable a tester to plan the test environment and test strategy effectively. 

### Test environment

It would be big mistake to use the same machine for testing an installer on which the it was developed or for that matter any other development machine. Software installers play around with things like path variables, registry settings, services and at times are dependent upon the underlying run time environment as well and these are invariably set correctly on a development machine. Its a good idea to start with a clean slate and have a test environment in place before the installer gets to you.Virtual machines (VM) are a good option to be used for installer testing as different configurations can be hosted on the same server machine. Additionally its very easy on a VM to rollback to the clean state for repeated runs. 

### Have a good test strategy in place

Here are some pointers to ensure that you have a good strategy to test the installer:

**Focus on the basics : **You should be aware of basics of operating systems and there different flavours. User management, path variables, registry and file writes and services installation is handled differently by different OS. Within a OS we can have multiple versions that should be tested for support. There can be issues like a installer working fine in Windows Vista might fail to kick off on Windows7 due to some minor upgrade. It would also make sense to verify the installer on 32 and 64 bit machines to check to any undesirable behaviour.

Additionally check that the installer works fine with and without Administrator privileges. Some services might require elevation to Admin while installing or start-up and this might cause a glitch in the installation process when installing with a guest login without Admin privileges.

Verify the installation using multiple users on the same machine or by changing the default locations of install directory and other directories being created. Its not rare to find install directories and path settings hard coded in an installer. Testing using long paths and paths with spaces will help eliminate any defects related to path variables not handled correctly.

The installer also might involve installing and starting an application container like tomcat and its always good to check if the default port is configurable of not. In case default port is not configurable and is already occupied while installation is in progress, the installer must warn the user and exit gracefully.

Basic checks like minimum free disk memory required for the set-up to complete successfully should be verified by trying a installation on machine with insufficient resources.

Installer UI should be tested exhaustively on different platforms and resolutions. Any web pages being launched after the installation should be tested for cross browser support and if more than one page is launched preferably it should be displayed in a separate browser window or tab.

All the program menu and desktop shortcuts created should be verified and by default the installer must create the shortcuts for only the current user (unless and untill its clearly indicated that the shortcuts should created for all users).

Further the UI for installer contains a lot of information like terms and conditions, licensing and distribution information, legal notices, version information along with some propriety stuff and actual customter site and marketing links.These should be checked thoroughly as this relates directly to the business of your customer and it has to be spot on.

Always remember to reboot the system post install and uninstall to check for the system integrity.

**Focus on your application: **Knowing the application would help you to identify scenarios where application specific configuration settings, file writes, directory structures and web page launches are done among other things.Each application has its unique set of pre-requisites which should be satisfied for it to install and run successfully. Hence its imperative that you know your application and all its components inside out to test the installer exhaustively. Their might be some components of the application or some configuration settings which are done outside the installer and it becomes all the more important to check the interaction of these variables with the installer since they are not under the control of the installer.

**Focus on the user : **You should have the clear understanding of the actual audience of your one-click would-be-shiny installer. Although it might be hard to identify all the types of users for some products, nonetheless you should invest effort in identifying the key stakeholders who would be running this thing on their machines. Identifying the user would probably help you to understand why the installer is offering multiple install options like a express install for business users with minimum technical know how and a custom install for more technical users. Based on this knowledge you can identify usability and experience issues with different install options and can suggest improvements before the actual users get their hands on it. Additionally getting to know your users would have an added benefit as you will probably have an idea of the software and hardware configuration the users would be having on their machines.

   [1]: http://en.wikipedia.org/wiki/Installation_testing
   [2]: http://en.wikipedia.org/wiki/Usability_testing