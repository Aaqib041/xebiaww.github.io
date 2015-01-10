---
layout: post
header-img: img/default-blog-pic.jpg
author: rrawat
description: 
post_id: 5439
created: 2010/10/21 09:01:36
created_gmt: 2010/10/21 04:01:36
comment_status: open
---

# Building a Hadoop Cluster using VirtualBox

Getting a few spare machines to start learning a NoSQL database or distributed software in early stages looks like an overkill($$). This blog post addresses creating a Ubuntu based Virtual machine for running [Hadoop][1] and automation of virtual machine (VM) creation from a given disk images using [VirtualBox][2].

While starting with NoSQL group at Xebia we decided to initially use virtual machines to host HBase, Hadoop etc, so that it is relatively easy for a new person to get started. Actual cluster deployment helps to learn about a _real world_ deployment, and we could use this process setup an office wide cluster to run an actual map-reduce job on the office laptops. Creating a new Virtual Machine takes some time and is repetitive task which can be automated.

If you are already familiar with VirtualBox then you could jump to [Automation][3] directly.

**Creating a Guest OS Disk**

The exercise started out with choosing a small OS (of course *nix based) with just a command line interface. We wanted the OS drive to be as compact as possible. We shortlisted following 3 OS distros for testing.

  1. TinyCore
  2. DamnSmallLinux
  3. Ubuntu-Mini
  4. Ubuntu-Server

**Why Ubuntu-Server**

TinyCore actually is a tiny installation, only problem faced was comfort level with the softwares available in its repositories (which is not as vast as the apt repositories).

Damn Small Linux needed a bit more analysis in terms of installing applications (ssh, java), so it is on hold now.

Ubuntu-Mini installation fetched everything for apt repositories and basically became a ubuntu-server.

Finally Ubuntu-server was tried trusting that the OS settings are optimized for server operation. The choice of OS is open and basically an expert could build his/her own tiny linux distribution.

**Create a Virtual Machine:** I created the first virtual machine manually. You could follow the instructions on (http://www.online-tech-tips.com/cool-websites/free-virtual-machine-software/) to install ubuntu (Virtual box now has a Ubuntu option in OS Type dropdown). You will have to mount the Ubuntu ISO server image in the CD drive.

Keep the RAM size to 512M and disk space size to ~1.5 - 2 GB.

**Setting up the machine.**

Once Ubuntu is installed, adding a ssh server is as easy as apt-get install openssh-server openssh-client. You can setup a passwordless ssh login using instruction at the link (http://www.debian-administration.org/articles/152)

Java need to be installed on the machine as well. Most of the NoSQL servers recommend sun-jre for now. You will have to download the installer from Sun/Oracle site. You could refer to following steps.

[sourcecode] ssh-add ~/xebia/id_dsa scp jre-6u21-linux-i586.bin xebia@192.168.56.101: ssh xebia@192.168.56.101 'chmod u+x jre*;./jre-6u21-linux-i586.bin;ln -s jre-6u21-linux-i586 jre' [/sourcecode]

**Network Setup**

VirtualBox provides 4 types of networking (detailed in its help). For now, we are going to use Host Networking as we want to have a network between the VMs and host.

This completes the basic setup of the Guest OS.

**Hadoop on a New drive.**

It is desirable to have a basic OS image and an additional hardware disk image. This gives an option to use the same OS image with some other softwares.

Following steps show how to mount and configure a new drive in the OS.

[sourcecode] // Manually create a 1 GB drive using VirtualBox Virtual Media Manager and attach it to the VM. // new disk should be /dev/sdb fdisk /dev/sdb mkfs -t ext4 /dev/sdb1 tune2fs -m 1 /dev/sdb1

// create an entry in fstab to mount this drive automaticallly (/media/hadoop)

[/sourcecode]

I have setup access to the VM from my Host Ubuntu machine so that i do not have to live with the flickering display of the VM. Following script automates installation of hadoop on the just mounted drive.

[sourcecode] scp hadoop_ xebia@$<VM IP>:/media/hadoop/ ssh xebia@$<VM IP> 'cd /media/hadoop;gunzip hadoop.tar.gz;tar xvf hadoop.tar' ssh xebia@$<VM IP> 'cd /media/hadoop;ln -s hadoop-_ hadoop' [/sourcecode]

Hadoop config is modified to refer to java installation and modify core-site.xml, hdfs-site.xml, mapred-site.xml. Later "slaves" file in conf need to be modified to refer to other VMs.

**Automation** The above process is lengthy and i did not want to do this for every VM. VirtalBox provides a command line interface to perform all activities that are done from UI. And now we had two disk images (Guest OS and Hadoop install) which can be sort of reused for every VM. See the following annotated script for details.

[sourcecode]

# !/bin/bash

# Path to already created disk images

cd ~/.VirtualBox/HardDisks OS_IMAGE=ubuntu-mini2.vdi HADOOP_DISK=hadoop-disk.vdi

# ARGS is new VM name

vmname="$1" VB=VBoxManage

if [ -z "$vmname" ]; then echo "Provide a VM Name"; exit -1; fi

# Does as it says .. clones the disks.

function clonedisks() { if [ -f "${vmname}.vdi" ]; then echo "Disk image exists... ${vmname}.vdi"; exit -1; fi echo "Cloning disks...${vmname}"; $VB clonehd $OS_IMAGE ${vmname}.vdi --remember $VB clonehd $HADOOP_DISK ${vmname}-hadoop.vdi --remember }

# Created a Virtual Machine

function createVM() { $VB createvm --name ${vmname} --ostype "Ubuntu" \--register }

function configureVM() {

# Machine settings

$VB modifyvm ${vmname} --memory 512 $VB modifyvm ${vmname} --vram 12 $VB modifyvm ${vmname} --cpus 1 $VB modifyvm ${vmname} --boot1 disk

# Configures the network

$VB modifyvm ${vmname} --nic1 hostonly $VB modifyvm ${vmname} --nictype1 82540EM $VB modifyvm ${vmname} --hostonlyadapter1 vboxnet0

# Attached the two hard drives

$VB storagectl ${vmname} --name "${vmname}-sata" \--add sata --controller IntelAhci --sataportcount 2 $VB storageattach ${vmname} --storagectl "${vmname}-sata" \--port 1 --device 0 --type hdd --medium ${vmname}.vdi $VB storageattach ${vmname} --storagectl "${vmname}-sata" \--port 2 --device 0 --type hdd --medium ${vmname}-hadoop.vdi

# fix the VMs hostname

$VB guestcontrol execute hello /bin/ls --username xebia --password xebia123 --verbose --wait-for stdout

}

function start() { VBoxHeadless --startvm ${vmname} --vrdp=off }

clonedisks createVM configureVM

# start

[/sourcecode]

With the new VM setup do not forget to add its IP/hostname to 'slaves' file.

**Open Issues:** Since we are using the OS disk image every host has the same name which creates a problem. I am fixing this manually at present by editing /etc/hosts and /etc/hostname. This can be fixed by installing Guest Additions in the OS image and automating the process(in progress).

Armed with the 3 machine hadoop cluster i was able to run the sample map reduce task. This cluster would not run the job faster but at least it gives an ability to do a distributed install on your local machine and ability to "scale out".

Do let me know your feedback/questions/suggestions.

   [1]: http://hadoop.apache.org/ (Apache Hadoop)
   [2]: http://www.virtualbox.org/ (VirtualBox)
   [3]: post.php?post=5439&action=edit&message=10#automation

## Comments

**[srivatsa](#5374 "2011-03-31 22:17:55"):** Can u help me out wit the entire automation of hadoop in shell script.

**[Roman](#5804 "2011-08-02 16:43:33"):** Hi, I want to create 50 nodes cluster on Virtual box VMs. I found your paper pretty well. Did you solved a problem of populating config file on name and data nodes?

**[Roman](#5805 "2011-08-02 16:45:18"):** You can see on SCM Express by Cloudera. It does the whole automation but requires 64 bit Linux

