---
layout: post
header-img: img/default-blog-pic.jpg
author: vgupta
description: 
post_id: 8882
created: 2011/05/12 17:29:56
created_gmt: 2011/05/12 12:29:56
comment_status: open
---

# Ruby version management using RVM

I had just started exploring Ruby and Rails and I came across a nice tool, RVM (Ruby Version Manager), which allows you to install and manage multiple versions of Ruby on the same machine. Although RVM works on Linux based systems only, but windows users can accomplish the same feat with [Pik][1]. RVM not only helps in managing different versions of Ruby on the same machine, but it also facilitates working with different combinations of Ruby and Rails. In this blog, I will share my experience of installing RVM and using it to manage different Ruby and Rails versions.  **Installing RVM**

To install RVM, use the following command 
    
    
    bash < <(curl -s https://rvm.beginrescueend.com/install/rvm)

The above command will download and install RVM. RVM will get installed in the **.rvm** folder inside the user's home directory. After installing RVM, please follow the instructions at[ https://rvm.beginrescueend.com/rvm/install/][2] to complete and test the installation. While referring the instructions at this page, please keep in mind the difference between `.bash_profile `and `.bashrc` as it will come to use while executing point no. 2 of the installation steps. The main difference between `.bashrc` and `.bash_profile` is that while `.bash_profile` is executed for login shells, `.bashrc` is executed for interactive non-login shells. So, use the appropriate file while following the installation steps.

You can install RVM for all users also. For that, please refer to the RVM documentation.

In order to make RVM work properly, please ensure that operating system has all the dependencies mentioned required by RVM. To get the list of dependencies is very easy. Just type `rvm notes` and you will be displayed a list of dependencies which needs to be installed.

**Installing Ruby with RVM**

Once you have RVM installed, installing ruby with it fairly straightforward. Typing the following two commands would install Ruby versions 1.8.7 and 1.9.2 respectively.

`rvm install 1.8.7`

`rvm install 1.9.2`

Now, you have installed two version of Ruby and want to make one of them (say 1.9.2) as default, then use the following command

`rvm --default use 1.9.2`

`ruby -v` will tell which Ruby is the default one now.

**Managing Gems with RVM**

Ruby programs are typically distributed via gems, which are self-contained packages of Ruby code. Since gems with different version numbers sometimes conflict, it is often convenient to create separate gemsets, which are self-contained bundles of gems. In particular, Rails is distributed as a gem, and there are conflicts between Rails 2 and Rails 3, so if you want to run multiple versions of Rails on the same system you need to create a separate gemset for each: ` rvm --create 1.8.7@rails2 rvm --create use 1.9.2@rails3 `

Here the first command creates the gemset rails2 associated with Ruby 1.8.7, while the second command creates the gemset rails3 associated with Ruby 1.9.2 and uses it (via the **use **command) at the same time. RVM supports a large variety of commands for manipulating gemsets; see the documentation at [http://rvm.beginrescueend.com/gemsets/][3].

If you want to default to Ruby 1.9.2 and rails3 gemset, then we need to use the following command:

`rvm --default use 1.9.2@rails3`

This simultaneously sets the default Ruby to 1.9.2 and the default gemset to rails3. Please note that there is a default gemset associated with each version of Ruby installed via RVM.

By the way, if you ever get stuck with RVM, help is available at your doorstep via the following commands:

`rvm --help rvm gemset --help`

RVM makes installing and managing Ruby and gems very easy, but, the capabilities of RVM is not limited to these. It is a fairly extensive tool and has many more features which can be explored by visiting [https://rvm.beginrescueend.com][4].

   [1]: Pikhttps://github.com/vertiginous/pik
   [2]: https://rvm.beginrescueend.com/rvm/install/ (https://rvm.beginrescueend.com/rvm/install/)
   [3]: http://rvm.beginrescueend.com/gemsets/http://rvm.beginrescueend.com/gemsets/ (http://rvm.beginrescueend.com/gemsets/)
   [4]: https://rvm.beginrescueend.comhttps://rvm.beginrescueend.com (https://rvm.beginrescueend.com)