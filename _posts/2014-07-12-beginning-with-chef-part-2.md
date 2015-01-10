---
layout: post
header-img: img/default-blog-pic.jpg
---

# Beginning with Chef - Part 2

Lets recap what all we have done in the last blog :

1.) Setup workstation and chef-repo.  
2.) Registered on chef to use hosted chef as the chef-server.  
3.) Bootstrapped a node to be managed by the chef-server.  
4.) Downloaded the "apache" cookbook in our chef-repo.  
5.) Uploaded the "apache" cookbook to the chef-server.  
6.) Added the recipe[apache] in the run-list of the node.  
7.) Ran the chef-client on the client to apply the cookbook.

Now lets continue, and try to understand some more concepts around chef and see them in action.  


**Node Object**  
The beauty of chef is that it gives an object oriented approach to the entire configuration management.  
The Node Object as the name suggests is an object of the class Node (http://rubydoc.info/gems/chef/Chef/Node).  
The node object consists of the run-list and node attributes, which is a JSON file that is stored on the Chef server. The chef-client gets a copy of the node object from the Chef server and maintains the state of a node.

**Attributes:** :  
An attribute is a specific detail about a node, such as an IP address, a host name, a list of loaded kernel modules, etc.

**Data-bags:**  
Data bags are JSON files used to store the data essential across all nodes and not relative to particular cookbooks.  
They can be accessed inside the cookbooks, attribute files using search. example: user profiles, groups, users, etc. Used by roles and environments, a persistence available across all the nodes.

Now, lets explore the node object and see the attributes and databags. We will also see how we can modify and set them.  
First lets see what all nodes are registered with chef-server:

[sourcecode]<br /> Anirudhs-MacBook-Pro:chef-repo anirudh$ knife node list<br /> aws-linux-node<br /> aws-node-ubuntu<br /> awsnode<br /> [/sourcecode]

Now lets see the details of the node awsnode.

[sourcecode]<br /> Anirudhs-MacBook-Pro:chef-repo anirudh$ knife node show awsnode<br /> Node Name: awsnode<br /> Environment: _default<br /> FQDN: ip-172-31-36-73.us-west-2.compute.internal<br /> IP: 172.31.36.73<br /> Run List: recipe[apache]<br /> Roles:<br /> Recipes: apache, apache::default<br /> Platform: redhat 7.0<br /> Tags:<br /> [/sourcecode]

**Finding specific attributes** : You can find the fqdn of the aws node.  
[sourcecode]<br /> Anirudhs-MacBook-Pro:chef-repo anirudh$ knife node show awsnode -a fqdn<br /> awsnode:<br /> fqdn: ip-172-31-36-73.us-west-2.compute.internal<br /> [/sourcecode]

**Search** : Search is one of the best features of chef, 'Search'. Chef Server uses [Solr](http://lucene.apache.org/solr/) for searching the node objects. So we can provide Solr style queries to search the Json node object attributes and data-bags.

Lets see how we can search all the nodes and see their fqdn (fully qualiifed domain name)  
[sourcecode]<br /> Anirudhs-MacBook-Pro:chef-repo anirudh$ knife search node &quot;*:*&quot; -a fqdn<br /> 2 items found</p> <p>node1:<br /> fqdn: centos63.example.com</p> <p>awsnode:<br /> fqdn: ip-172-31-36-73.us-west-2.compute.internal</p> <p>[/sourcecode]

**Changing the defaults using attributes**  
Lets try to change some defaults in our apache cookbook using the attributes.

In the /chef-repo/cookbooks/apache/attributes folder we can find the file default.rb (create if not)  
Add the following :  
[sourcecode]<br /> default[&quot;apache&quot;][&quot;indexfile&quot;]=&quot;index1.html&quot;<br /> [/sourcecode]  
Now go to the folder cookbooks/apache/files/default and make a file index1.html  
[sourcecode]<br /> &lt;html&gt;<br /> &lt;body&gt;<br /> &lt;h1&gt; Dude!! This is index1.html, it has been changed by chef!&lt;/h1&gt;<br /> &lt;/body&gt;<br /> &lt;/html&gt;<br /> [/sourcecode] 

The last thing we need to do get this working is change the recipe and tell it to pick the default index file from the node attribute 'indexfile' which we have just set.  
So, open the file 'cookbooks/apache/recipes/default.rb' and append this:  
[sourcecode]<br /> cookbook_file &quot;/var/www/index.html&quot; do<br /> source node[&quot;apache&quot;][&quot;indexfile&quot;]<br /> mode &quot;0644&quot;<br /> end<br /> [/sourcecode]  
Now upload the cookbook to the chef server using the command :  
[sourcecode]<br /> Anirudhs-MacBook-Pro:chef-repo anirudh$ knife cookbook upload apache<br /> [/sourcecode]  
And then go to the node, and run the chef-client  
[sourcecode]<br /> opscode@awsnode:~$ sudo chef-client<br /> [/sourcecode]

Now, hit the external IP of the node in the browser, and we can see the change.  
So, we just now used the attribute to change the default index page of the apache server.

An important thing to note here is the precedence of setting attributes.  
defaults in recipe take a precedence over the attributes, and Role takes precedence over the recipes.  
The order of precedence is as follows:  
[sourcecode]<br /> Ohai &gt; Role &gt; Environment &gt; Recipe &gt; Attribute<br /> [/sourcecode]

**Roles:**  
A Role tell us what a particular node is acting as, the type of the node, is it a "web server", a "database" etc. The use of this feature is that we can associate the run_list with it.  
So, instead of providing recipies as run_list to the node, We will associate the run_lists with a role and then apply this role to a node.

**Creating a role:**  
[sourcecode]<br /> knife create role webserver<br /> [/sourcecode]  
Check if role is created:  
[sourcecode]<br /> Anirudhs-MacBook-Pro:chef-repo anirudh$ knife role show webserver<br /> chef_type: role<br /> default_attributes:<br /> apache:<br /> sites:<br /> admin:<br /> port: 8000<br /> description: Web Server<br /> env_run_lists:<br /> json_class: Chef::Role<br /> name: webserver<br /> override_attributes:<br /> run_list: recipe[apache]<br /> [/sourcecode]  
This role we just created has added apache recipe in the run_list.

**Assign this role to the node "awsnode"**  
[sourcecode]<br /> Anirudhs-MacBook-Pro:chef-repo anirudh$ knife node run_list add awsnode 'role[webserver]'<br /> awsnode:<br /> run_list:<br /> recipe[apache]<br /> role[webserver]<br /> [/sourcecode]

**Upload this role to the chef-server:**  
[sourcecode]<br /> Anirudhs-MacBook-Pro:chef-repo anirudh$ knife role from file webserver.rb<br /> [/sourcecode