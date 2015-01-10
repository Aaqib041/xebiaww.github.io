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

<p>Getting a few spare machines to start learning a NoSQL database or distributed software in early stages looks like an overkill($$). This blog post addresses creating a Ubuntu based Virtual machine for running <a title="Apache Hadoop" href="http://hadoop.apache.org/">Hadoop</a> and automation of virtual machine (VM) creation from a given disk images using <a title="VirtualBox" href="http://www.virtualbox.org/">VirtualBox</a>.</p>
<p>While starting with NoSQL group at Xebia we decided to initially use virtual machines to host HBase, Hadoop etc, so that it is relatively easy for a new person to get started. Actual cluster deployment helps to learn about a <em>real world</em> deployment, and we could use this process setup an office wide cluster to run an actual map-reduce job on the office laptops. Creating a new Virtual Machine takes some time and is repetitive task which can be automated.</p>
<p>If you are already familiar with VirtualBox then you could jump to <a href="post.php?post=5439&amp;action=edit&amp;message=10#automation">Automation</a> directly.</p>
<!--more-->

<p><strong>Creating a Guest OS Disk</strong></p>
<p>The exercise started out with choosing a small OS (of course *nix based) with just a command line interface. We wanted the OS drive to be as compact as possible. We shortlisted following 3 OS distros for testing.</p>
<ol>
<li>TinyCore</li>
<li>DamnSmallLinux</li>
<li>Ubuntu-Mini</li>
<li>Ubuntu-Server</li>
</ol>
<p><strong>Why Ubuntu-Server</strong></p>
<p>TinyCore actually is a tiny installation, only problem faced was comfort level with the softwares available in its repositories (which is not as vast as the apt repositories).</p>
<p>Damn Small Linux needed a bit more analysis in terms of installing applications (ssh, java),
so it is on hold now.</p>
<p>Ubuntu-Mini installation fetched everything for apt repositories and basically became a ubuntu-server.</p>
<p>Finally Ubuntu-server was tried trusting that the OS settings are optimized for server operation.  The choice of OS is open and basically an expert could build his/her own tiny linux distribution.</p>
<p><strong>Create a Virtual Machine:</strong>
I created the first virtual machine manually. You could follow the instructions on (http://www.online-tech-tips.com/cool-websites/free-virtual-machine-software/) to install ubuntu (Virtual box now has a Ubuntu option in OS Type dropdown). You will have to mount the Ubuntu ISO server image in the CD drive.</p>
<p>Keep the RAM size to 512M and disk space size to ~1.5 - 2 GB.</p>
<p><strong>Setting up the machine.</strong></p>
<p>Once Ubuntu is installed, adding a ssh server is as easy as
apt-get install openssh-server openssh-client.
You can setup a passwordless ssh login using instruction at the link (http://www.debian-administration.org/articles/152)</p>
<p>Java need to be installed on the machine as well. Most of the NoSQL servers recommend sun-jre for now. You will have to download the installer from Sun/Oracle site. You could refer to following steps.</p>
<p>[sourcecode]
    ssh-add ~/xebia/id_dsa
    scp jre-6u21-linux-i586.bin xebia@192.168.56.101:
    ssh xebia@192.168.56.101 'chmod u+x jre*;./jre-6u21-linux-i586.bin;ln -s jre-6u21-linux-i586 jre'
[/sourcecode]</p>
<p><strong>Network Setup</strong></p>
<p>VirtualBox provides 4 types of networking (detailed in its help). For now, we are going to use Host Networking as we want to have a network between the VMs and host.</p>
<p>This completes the basic setup of the Guest OS.</p>
<p><strong>Hadoop on a New drive.</strong></p>
<p>It is desirable to have a basic OS image and an additional hardware disk image. This gives an option to use the same OS image with some other softwares.</p>
<p>Following steps show how to mount and configure a new drive in the OS.</p>
<p>[sourcecode]
// Manually create a 1 GB drive using VirtualBox Virtual Media Manager and attach it to the VM.
// new disk should be /dev/sdb
fdisk /dev/sdb
mkfs -t ext4 /dev/sdb1
tune2fs -m 1 /dev/sdb1</p>
<p>// create an entry in fstab to mount this drive automaticallly (/media/hadoop)</p>
<p>[/sourcecode]</p>
<p>I have setup access to the VM from my Host Ubuntu machine so that i do not have to live with the flickering display of the VM. Following script automates installation of hadoop on the just mounted drive.</p>
<p>[sourcecode]
    scp hadoop<em> xebia@$&lt;VM IP&gt;:/media/hadoop/
    ssh xebia@$&lt;VM IP&gt; 'cd /media/hadoop;gunzip hadoop.tar.gz;tar xvf hadoop.tar'
    ssh xebia@$&lt;VM IP&gt; 'cd /media/hadoop;ln -s hadoop-</em> hadoop'
[/sourcecode]</p>
<p>Hadoop config is modified to refer to java installation and modify core-site.xml, hdfs-site.xml, mapred-site.xml.
Later "slaves" file in conf need to be modified to refer to other VMs.</p>
<p><strong>Automation</strong>
The above process is lengthy and i did not want to do this for every VM. VirtalBox provides a command line interface to perform all activities that are done from UI. And now we had two disk images (Guest OS and Hadoop install) which can be sort of reused for every VM. See the following annotated script for details.</p>
<p>[sourcecode]</p>
<h1>!/bin/bash</h1>
<h1>Path to already created disk images</h1>
<p>cd ~/.VirtualBox/HardDisks
OS_IMAGE=ubuntu-mini2.vdi
HADOOP_DISK=hadoop-disk.vdi</p>
<h1>ARGS is new VM name</h1>
<p>vmname=&quot;$1&quot;
VB=VBoxManage</p>
<p>if [ -z &quot;$vmname&quot; ]; then
        echo &quot;Provide a VM Name&quot;; exit -1;
fi</p>
<h1>Does as it says .. clones the disks.</h1>
<p>function clonedisks() {
  if [ -f &quot;${vmname}.vdi&quot; ]; then
    echo &quot;Disk image exists... ${vmname}.vdi&quot;;
    exit -1;
  fi
  echo &quot;Cloning disks...${vmname}&quot;;
  $VB clonehd $OS_IMAGE ${vmname}.vdi --remember
  $VB clonehd $HADOOP_DISK ${vmname}-hadoop.vdi --remember
}</p>
<h1>Created a Virtual Machine</h1>
<p>function createVM() {
  $VB createvm --name ${vmname} --ostype &quot;Ubuntu&quot; --register
}</p>
<p>function configureVM() {</p>
<h1>Machine settings</h1>
<p>$VB modifyvm ${vmname} --memory 512
  $VB modifyvm ${vmname} --vram 12
  $VB modifyvm ${vmname} --cpus 1
  $VB modifyvm ${vmname} --boot1 disk</p>
<h1>Configures the network</h1>
<p>$VB modifyvm ${vmname} --nic1 hostonly
  $VB modifyvm ${vmname} --nictype1 82540EM
  $VB modifyvm ${vmname} --hostonlyadapter1 vboxnet0</p>
<h1>Attached the two hard drives</h1>
<p>$VB storagectl ${vmname} --name &quot;${vmname}-sata&quot; --add sata --controller IntelAhci --sataportcount 2
  $VB storageattach ${vmname} --storagectl &quot;${vmname}-sata&quot; --port 1 --device 0 --type hdd --medium ${vmname}.vdi
  $VB storageattach ${vmname} --storagectl &quot;${vmname}-sata&quot; --port 2 --device 0 --type hdd --medium ${vmname}-hadoop.vdi</p>
<h1>fix the VMs hostname</h1>
<p>$VB guestcontrol execute hello /bin/ls --username xebia --password xebia123 --verbose --wait-for stdout</p>
<p>}</p>
<p>function start() {
    VBoxHeadless --startvm ${vmname} --vrdp=off
}</p>
<p>clonedisks
createVM
configureVM</p>
<h1>start</h1>
<p>[/sourcecode]</p>
<p>With the new VM setup do not forget to add its IP/hostname to 'slaves' file.</p>
<p><strong>Open Issues:</strong>
Since we are using the OS disk image every host has the same name which creates a problem. I am fixing this manually at present by editing /etc/hosts and /etc/hostname.
This can be fixed by installing Guest Additions in the OS image and automating the process(in progress).</p>
<p>Armed with the 3 machine hadoop cluster i was able to run the sample map reduce task. This cluster would not run the job faster but at least it gives an ability to do a distributed install on your local machine and ability to "scale out".</p>
<p>Do let me know your feedback/questions/suggestions.</p>

## Comments

**[srivatsa](#5374 "2011-03-31 22:17:55"):** Can u help me out wit the entire automation of hadoop in shell script.

**[Roman](#5804 "2011-08-02 16:43:33"):** Hi, I want to create 50 nodes cluster on Virtual box VMs. I found your paper pretty well. Did you solved a problem of populating config file on name and data nodes?

**[Roman](#5805 "2011-08-02 16:45:18"):** You can see on SCM Express by Cloudera. It does the whole automation but requires 64 bit Linux

