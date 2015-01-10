---
layout: post
header-img: img/default-blog-pic.jpg
author: anirudh.xebia
description: 
post_id: 17680
created: 2013/11/29 11:06:25
created_gmt: 2013/11/29 06:06:25
comment_status: open
---

# Vagrant - a swiss Army knife for every developer

<p>Whenever we move to a new project, or we want to explore a new stack of technologies; we face the problem of environments. Being a developer we generally have a tendency to install everything on our local environment; which many times proves to be a disaster.</p>
<p>Today, modern web applications involve a lot of moving parts, numerous underlying technologies and a lot of complexity.
To talk about languages we have JS,python,Ruby,Java,Scala,Closure etc.
For databases there is MySQL, PostgreSQL, Redis, Cassandra, MongoDB etc. And there are multiple web servers, backend servers, application servers and services each with their own use cases (eg. Apache,Unicorn, Nginx, RabbitMQ, Solr etc.)
Installing and uninstalling all these technologies; their combinations and different versions manually on one local system could be a very tedious job.
Another problem with this approach is that every developer would do it in his or her own way and hence the famous "Worked on my system" excuse would appear now and then as the systems where QA are testing might have some difference in some configuration.</p>
<p>So, a better approach would be to install and configure everything on a virtual machine in an automated way, using the concept of Infrastructure as Code, wherein we create blue prints of our infrastructure which could be used to create identical environments in an automated way.</p>
<p>So, how do we create virtual machines, how do we configure them, what about SSH, network configuration, hostname etc.</p>
<p>This is where <strong>Vagrant</strong> comes into the picture.
Vagrant is a tool for building an environment sandboxed in a virtual machine. Vagrant encourages use of shell scripts or configuration management softwares (like chef, puppet etc.) for creation of environments.
Vagrant allows you to work with the same OS that is running in production or QA, irrespective of the OS your physical machine is running. This is possible because Vagrant puts your development environment into a virtual machine. So you can work on different projects; different environments and different teams hassle free. Moreover on boarding a new team member to the project becomes very easy and quick.</p>
<p>Vagrant is a layer on top of some virtualisation solution; which, in one command does the following :</p>
<p>1.) Creates a virtual machine based on the os of your choice.
2.) Modifies physical properties like RAM,CPUs of the vm.
3.) Manage and establish network interfaces to access the vm.
4.) Set up shared folders between host and vm.
5.) Boots the vm, sets the hostname and other properties.
6.) Can club with provisioning softwares like chef and puppet.</p>
<p>So, basically vagrant handles the entire life cycle of the machine for you.</p>
<p>Additionally it can also,</p>
<ul>
<li>SSH into machine</li>
<li>shut down machine, destroy machine and completely delete all feta</li>
<li>suspend/resume</li>
<li>package the machine state so that you can distribute it to other developers (Infrastructure as Code)</li>
</ul>
<p><strong>Installation</strong></p>
<p>Vagrant works on Linux, Mac OSX and yes on Windows as well. We can download the installers based on the choice of OS from <a href="http://downloads.vagrantup.com">here</a>. The complete documentation can be found <a href="http://docs.vagrantup.com/v2/">here</a>.
Or for a more detailed installation on ubuntu; refer my blog <a href="http://anirudhbhatnagar.com/2013/09/11/create-an-ubuntu-vm-using-vagrant-and-virtual-box/">here</a>.</p>
<p><strong>Cloud</strong></p>
<p><strong></strong>Another approach is to ditch desktop virtualisation altogether and do development directly in the cloud, such as on EC2. The main benefit of this approach is that it allows the developers to work from incredibly low-powered desktop machines in the environment that closely resembles production environments. The main disadvantage here is that this approach requires and Internet connection and have a much higher financial cost associated with it. So , for experimentation and dev environments cloud can be very expensive solution.</p>
<p>If you are using Vagrant you would be using some automated script or chef/puppet for provisioning and hence replicating this environment to other dev machine or to a QA or prod environment set-up on cloud, would be much easier.</p>
<p><strong>Using Vagrant without Virtual Box</strong></p>
<p>Previously (before 1.1) vagrant could only be used with VirtualBox, but after 1.1 vagrant can be used with VMware,EC2 or any other software providing virtualisation. We can also use vagrant with containers.
There is lxc support for vagrant - https://github.com/fgrehm/vagrant-lxc
We can also use <a href="http://www.docker.io">docker</a> with vagrant.</p>
<p>So, Vagrant can be clubbed with any underlying virtualisation technology and can provide a seamless interface for it.</p>
<p>It can do everything you need to create and manage a virtual machine and helps enforce good practices by use of automation.</p>
<p>In the coming blogs we will discuss about how we can use vagrant with chef to create our dev environment. A basic blog on same can be referred <a href="http://anirudhbhatnagar.com/2013/09/25/chef-solo-with-vagrant/">here</a>.</p>
<p>To know more about Xebia's offerings on continuous delivery, <a href="http://www.xebia.in/continuous-delivery.html">Click here</a></p>