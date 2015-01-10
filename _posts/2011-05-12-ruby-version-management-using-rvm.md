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

<p>I had just started exploring Ruby and Rails and I came across a nice tool, RVM (Ruby Version Manager), which allows you to install and manage multiple versions of Ruby on the same machine. Although RVM works on Linux based systems only, but windows users can accomplish the same feat with <a href="Pikhttps://github.com/vertiginous/pik">Pik</a>. RVM not only helps in managing different versions of Ruby on the same machine, but it also facilitates working with different combinations of Ruby and Rails. In this blog, I will share my experience of installing RVM and using it to manage different Ruby and Rails versions.
<!--more-->
<strong>Installing RVM</strong></p>
<p>To install RVM, use the following command
<pre>bash &lt; &lt;(curl -s https://rvm.beginrescueend.com/install/rvm)</pre>
The above command will download and install RVM. RVM will get installed in the <strong>.rvm</strong> folder inside the user's home directory. After installing RVM, please follow the instructions at<a title="https://rvm.beginrescueend.com/rvm/install/" href="https://rvm.beginrescueend.com/rvm/install/"> https://rvm.beginrescueend.com/rvm/install/</a> to complete and test the installation. While referring the instructions at this page, please keep in mind the difference between <code>.bash_profile </code>and <code>.bashrc</code> as it will come to use while executing point no. 2 of the installation steps. The main difference between <code>.bashrc</code> and <code>.bash_profile</code> is that while <code>.bash_profile</code> is executed for login shells, <code>.bashrc</code> is executed for interactive non-login shells. So, use the appropriate file while following the installation steps.</p>
<p>You can install RVM for all users also. For that, please refer to the RVM documentation.</p>
<p>In order to make RVM work properly, please ensure that operating system has all the dependencies mentioned required by RVM. To get the list of dependencies is very easy. Just type <code>rvm notes</code> and you will be displayed a list of dependencies which needs to be installed.</p>
<p><strong>Installing Ruby with RVM</strong></p>
<p>Once you have RVM installed, installing ruby with it fairly straightforward. Typing the following two commands would install Ruby versions 1.8.7 and 1.9.2 respectively.</p>
<p><code>rvm install 1.8.7</code></p>
<p><code>rvm install 1.9.2</code></p>
<p>Now, you have installed two version of Ruby and want to make one of them (say 1.9.2) as default, then use the following command</p>
<p><code>rvm --default use 1.9.2</code></p>
<p><code>ruby -v</code> will tell which Ruby is the default one now.</p>
<p><strong>Managing Gems with RVM</strong></p>
<p>Ruby programs are typically distributed via gems, which are self-contained packages of Ruby code. Since gems with different version numbers sometimes conflict, it is often convenient to create separate gemsets, which are self-contained bundles of gems. In particular, Rails is distributed as a gem, and there are conflicts between Rails 2 and Rails 3, so if you want to run multiple versions of Rails on the same system you need to create a separate gemset for each:
<code>
rvm --create 1.8.7@rails2
rvm --create use 1.9.2@rails3
</code></p>
<p>Here the first command creates the gemset rails2 associated with Ruby 1.8.7, while the second command creates the gemset rails3 associated with Ruby 1.9.2 and uses it (via the <strong>use </strong>command) at the same time. RVM supports a large variety of commands for manipulating gemsets; see the documentation at <a title="http://rvm.beginrescueend.com/gemsets/" href="http://rvm.beginrescueend.com/gemsets/http://rvm.beginrescueend.com/gemsets/">http://rvm.beginrescueend.com/gemsets/</a>.</p>
<p>If you want to default to Ruby 1.9.2 and rails3 gemset, then we need to use the following command:</p>
<p><code>rvm --default use 1.9.2@rails3</code></p>
<p>This simultaneously sets the default Ruby to 1.9.2 and the default gemset to rails3. Please note that there is a default gemset associated with each version of Ruby installed via RVM.</p>
<p>By the way, if you ever get stuck with RVM, help is available at your doorstep via the following commands:</p>
<p><code>rvm --help
rvm gemset --help</code></p>
<p>RVM makes installing and managing Ruby and gems very easy, but, the capabilities of RVM is not limited to these. It is a fairly extensive tool and has many more features which can be explored by visiting <a title="https://rvm.beginrescueend.com" href="https://rvm.beginrescueend.comhttps://rvm.beginrescueend.com">https://rvm.beginrescueend.com</a>.</p>