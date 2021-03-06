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

Virtualization refers to technologies designed to provide a layer of abstraction between computer hardware systems and the software running on them. By providing a logical view of computing resources, rather than a physical view, virtualization solutions make it possible to do a couple of very useful things.

Before starting with container based deployment let me define some of the key concepts & terminologies used in this context.

In server virtualization the physical server is called the **host**. The virtual servers are called **guests**.

Hypervisor is a program which manages multiple operating systems on a single computer system.

It manages system's processor, memory, and other resources to allocate what each operating system requires. They vary from one processor architectures to another. On a broader picture there are two types of **hypervisors**.

Type 1 hypervisors: Also known as bare metal hypervisors. These hypervisors directly run on the host’s hardware and manages guest operating systems. Example: XenServer.

Type 2 hypervisors: Also know as hosted hypervisors. These hypervisors requires a host operating system for guest operating systems management. Example: Virtualbox, VMware WorkStation .

![Virtualization-Hypervisor-Types][1]

In three ways we can create virtual servers

  1. Full Virtualization

  2. Para-Virtualization

  3. OS-level Virtualization.

**Full virtualization** uses bare metal type hypervisors. Due to this each virtual server is completely independent and unaware of the other virtual servers running on the physical machine. We can have different OS on each guest.

In **para-virtualization** a software hypervisor is installed on a physical server and a guest OS is installed into the environment. The guest servers are aware of one another. Since guest OS needs to “know” that it is virtualized to take advantage of the functions, Operating systems require extensions to make API calls to the hypervisor.

**OS-level virtualization** approach doesn't use a hypervisor at all. Virtualization capability is part of the host OS, which performs all the functions of a fully virtualized hypervisor. The biggest limitation of this approach is that all the guest servers must run the same OS. Each virtual server remains independent from all the others, but you can't mix and match operating systems among them.

Because OS level virtual partitions can be much smaller than virtual machine partitions, many more can run in the same hardware configuration. OS level virtualization is also known as containers virtualization. Linux Containers (lxc) is an example of OS-level virtualization.

**Container based deployment** is a deployment technique in which the whole application and its dependencies are packaged as light weight portable containers. It differs from the traditional VM’s in the sense that it doesn’t contain any entire Guest operating system. Docker is an example of this technique.

**Docker** is an open-source engine that automates the deployment of any application as a lightweight, portable, self-sufficient container that will run virtually anywhere.

Docker leverages lxc & aufs to provide containers based deployment and is available for all major linux distributions.

![docker_vm][2]

It provides

**Portable deployment** across machines by defining a format for bundling an application and all its dependencies into a single object which can be transferred to any docker-enabled machine, and executed there with the guarantee that the execution environment exposed to the application will be the same.

**Automatic build** to automatically assemble a container from their source code, with full control over application dependencies, build tools, packaging etc.

**Versioning capabilities** for tracking successive versions of a container, inspecting the diff between versions, committing new versions, rolling back etc.

**Component re-use** so that any container can be used as a "base image" to create more specialized components.

Due to the above mentioned capabilities it is of great use for the devOps community.

Red Hat along with Rackspace, Puppet Labs, and Linode, all of which already make use of Docker for existing solutions, has become partner of Docker inc., the company behind the Docker project.

To know more about Xebia's offerings on continuous delivery, [Click here][3]

   [1]: http://xebee.xebia.in/wp-content/uploads/2013/11/Virtualization-Hypervisor-Types-300x187.png
   [2]: http://xebee.xebia.in/wp-content/uploads/2013/11/docker_vm-300x167.jpg
   [3]: http://www.xebia.in/continuous-delivery.html