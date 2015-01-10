---
layout: post
header-img: img/default-blog-pic.jpg
author: ankit0rastogi
description: 
post_id: 17698
created: 2013/11/30 18:34:12
created_gmt: 2013/11/30 13:34:12
comment_status: open
---

# Introduction to virtualization and container based deployment

<p>Virtualization refers to technologies designed to provide a layer of abstraction between computer hardware systems and the software running on them. By providing a logical view of computing resources, rather than a physical view, virtualization solutions make it possible to do a couple of very useful things.</p>
<p>Before starting with container based deployment let me define some of the key concepts &amp; terminologies used in this context.</p>
<p>In server virtualization the physical server is called the <b>host</b>. The virtual servers are called <b>guests</b>.</p>
<p>Hypervisor is a program which manages multiple operating systems on a single computer system.</p>
<p>It manages system's processor, memory, and other resources to allocate what each operating system requires. They vary from one processor architectures to another. On a broader picture there are two types of <b>hypervisors</b>.</p>
<p>Type 1 hypervisors: Also known as bare metal hypervisors. These hypervisors directly run on the host’s hardware and manages guest operating systems. Example: XenServer.</p>
<p>Type 2 hypervisors: Also know as hosted hypervisors. These hypervisors requires a host operating system for guest operating systems management. Example: Virtualbox, VMware WorkStation .</p>
<p><a href="http://xebee.xebia.in/wp-content/uploads/2013/11/Virtualization-Hypervisor-Types.png"><img class="alignnone size-medium wp-image-17707" alt="Virtualization-Hypervisor-Types" src="http://xebee.xebia.in/wp-content/uploads/2013/11/Virtualization-Hypervisor-Types-300x187.png" width="365" height="227" /></a></p>
<p>In three ways we can create virtual servers</p>
<ol>
<li>
<p>Full Virtualization</p>
</li>
<li>
<p>Para-Virtualization</p>
</li>
<li>
<p>OS-level Virtualization.</p>
</li>
</ol>
<p><b>Full virtualization</b> uses bare metal type hypervisors. Due to this each virtual server is completely independent and unaware of the other virtual servers running on the physical machine. We can have different OS on each guest.</p>
<p>In <b>para-virtualization</b> a software hypervisor is installed on a physical server and a guest OS is installed into the environment. The guest servers are aware of one another. Since guest OS needs to “know” that it is virtualized to take advantage of the functions, Operating systems require extensions to make API calls to the hypervisor.</p>
<p><b>OS-level virtualization</b> approach doesn't use a hypervisor at all. Virtualization capability is part of the host OS, which performs all the functions of a fully virtualized hypervisor. The biggest limitation of this approach is that all the guest servers must run the same OS. Each virtual server remains independent from all the others, but you can't mix and match operating systems among them.</p>
<p>Because OS level virtual partitions can be much smaller than virtual machine partitions, many more can run in the same hardware configuration. OS level virtualization is also known as containers virtualization. Linux Containers (lxc) is an example of OS-level virtualization.</p>
<p><b>Container based deployment</b> is a deployment technique in which the whole application and its dependencies are packaged as light weight portable containers. It differs from the traditional VM’s in the sense that it doesn’t contain any entire Guest operating system. Docker is an example of this technique.</p>
<p><b>Docker</b> is an open-source engine that automates the deployment of any application as a lightweight, portable, self-sufficient container that will run virtually anywhere.</p>
<p>Docker leverages lxc &amp; aufs to provide containers based deployment and is available for all major linux distributions.</p>
<p><a href="http://xebee.xebia.in/wp-content/uploads/2013/11/docker_vm.jpg"><img class="alignnone size-medium wp-image-17710" alt="docker_vm" src="http://xebee.xebia.in/wp-content/uploads/2013/11/docker_vm-300x167.jpg" width="399" height="222" /></a></p>
<p>It provides</p>
<p><b>Portable deployment</b> across machines by defining a format for bundling an application and all its dependencies into a single object which can be transferred to any docker-enabled machine, and executed there with the guarantee that the execution environment exposed to the application will be the same.</p>
<p><b>Automatic build</b> to automatically assemble a container from their source code, with full control over application dependencies, build tools, packaging etc.</p>
<p><b>Versioning capabilities</b> for tracking successive versions of a container, inspecting the diff between versions, committing new versions, rolling back etc.</p>
<p><b>Component re-use</b> so that any container can be used as a "base image" to create more specialized components.</p>
<p>Due to the above mentioned capabilities it is of great use for the devOps community.</p>
<p>Red Hat along with Rackspace, Puppet Labs, and Linode, all of which already make use of Docker for existing solutions, has become partner of Docker inc., the company behind the Docker project.</p>
<p>To know more about Xebia's offerings on continuous delivery, <a href="http://www.xebia.in/continuous-delivery.html">Click here</a></p>