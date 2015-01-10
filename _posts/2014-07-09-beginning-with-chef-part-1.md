---
layout: post
header-img: img/default-blog-pic.jpg
author: anirudh.xebia
description: 
post_id: 18570
created: 2014/07/09 00:25:01
created_gmt: 2014/07/08 19:25:01
comment_status: open
---

# Beginning with chef - Part 1

This is first of the series of the blogs, designed for a developer to start using chef. Chef is a configuration management tool which follows the concept of infrastructure as code and could be used for the automation of creating infrastructure on cloud as well as on-premise data centres.

The three main components of chef are :

** 1.) Work station:** This is the developer's machine will be used to author cookbooks and recipes and upload them to the chef-server using the command line utility called knife. ** 2.) Chef-Server:** This is the main server on which all the cookbooks, roles, policies are uploaded. ** 3.) Node:** This is the instance which would be provisioned by applying the cookbooks uploaded on the chef-server. Apart from these, there are few basic build blocks of chef like cookbooks,recipes,roles,environemts,run-lists,policies etc. The complete documentation and architecture of chef can be found [here][1].

Assuming the theory is somewhat clear; lets get started with the practial and will try to understand more while doing it.

**1.) set up the workstation.** \- install Chef in your workstation, To do that follow here http://www.getchef.com/chef/install/

**2.) Use hosted chef as chef-server** -Register on chef on the chef's site at http://www.getchef.com \- You can use hosted Chef, it gives you the option to manage upto 5 nodes for free. -Create your user and an organisation. In order to authenticate your workstation with the chef-server we would need these 3 things:

[sourcecode] -[validator].PEM -knife.rb -[username].PEM [/sourcecode]

So, you need to download these 3 items in your workstation. (You can try reset keys option or download the starter kit.) **3.) Set up chef-repo in the workstation** \- Open your workstation, go to the folder which you want to be your base folder for writing cookbooks. \- Download the chef-repo from opscode git repo or use the starter kit provided on the chef site. \- Put these 3 files in your .chef folder inside the chef-repo folder in your workstation (Create .chef, if not already present)

Now your workstation is set, authenticated with chef-server and your chef-repo is configured. So lets begin configuring a node on which the cookbooks would be applied.

**4.) Setting up the node** The node could be an EC2 instance or could be provided by any other cloud provider or a vm. The first step is to bootstrap it. **[knife][2]** : Knife is a command-line tool that provides an interface between a local chef-repo and the Chef server **\- Bootstrap any instance**

[sourcecode] knife bootstrap [ip-address] --sudo -x [user-name] -P [password] -N "[node name]" [/sourcecode]

Or for an AWS instance

[sourcecode] knife bootstrap [AWS external IP] --sudo -x ec2-user -i [AWS key] -N "awsnode" [/sourcecode]

These are things that happen during the bootstraping :

[sourcecode] 1.) Installs chef client and OHAI on the node 2.) Establishes authentication for ssh keys. 3.) Send the 3 keys to chef-client [/sourcecode]

Once the node is bootstrapped, Its now time to author some cookbooks to apply on the node.

**5.) Download a cookbook** \- A cookbook is the fundamental unit of configuration and policy distribution and contains everything needed to install and configure the required resource (e.g MySQL). We will download an already existing cookbook of apache webserver, using the following knife command. (Remember all the knife commands should be executed from the base chef-repo directory.)

[sourcecode] knife cookbook site download apache [/sourcecode]

This will download the tar.gz zipped folder in your chef-repo, We will need to unzip and copy it to the cookbooks folder. (After unzipping it remove the zipped file) (use tar -xvf [file], then mv command)

[sourcecode] mv apache ../chef-repo/cookbooks [/sourcecode]

Inside the apache folder we can find the "recipes" folder and inside that there is a file called as "default.rb". A **recipe** is the most fundamental configuration element which is part of the cookbook. This "default.rb" ruby file contains the default recipe required to configure the apache server. Lets have a look at an excerpt from it.

[sourcecode] .... package "httpd" do action :install end .... [/sourcecode]

So this cookbook is defining the default action on application of this recipe to be "install", this will install the apache webserver on the node. More details about these we will cover in the next blog, for now lets just upload this coookbook.

**6.) Upload a cookbook to the chef-server**

[sourcecode] knife cookbook upload apache [/sourcecode]

Now, the cookbook is uploaded on to the chef-server. Once chef-server has the cookbook we can apply it to any of the nodes which are configured with the chef-server. First lets find what all nodes we have:

** \- To see all my nodes**

[sourcecode] knife node list [/sourcecode]

**7.) Apply the run-list to the node.** **Run-List** : Run-list is like a playList for a node. It tells the node which recipes are needed to be run and in which order. In order to apply the cookbook to a given node , we need to add it to the run-list of the node

[sourcecode] knife node run_list add node-name "recipe[apache]" [/sourcecode]

Now we have successfully uploaded a cookbook and added it to the run-list of a node with alias "node-name". Next time when chef-client will run on the node, it will fetch the details of its run-list from the chef-server and download any cookbook required from the chef-server and run it. For now, lets ssh into the node and run the chef-client manualy to see the results.

**8.) run chef-client on the node.**

[sourcecode] sudo chef-client [/sourcecode]

If the chef-client run is successful, we can hit the IP address of the instance to see the default page of apache up and running. If you are using AWS, don't forget to open the port 80.

This was just a basic introduction to chef, in the next blog we will see the killer feature of chef, which is search and go into the details of node object, roles, environments.

   [1]: http://docs.opscode.com/
   [2]: http://docs.opscode.com/knife.html