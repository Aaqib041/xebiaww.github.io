---
layout: post
header-img: img/default-blog-pic.jpg
author: gbansal
description: 
post_id: 17557
created: 2013/10/31 23:23:47
created_gmt: 2013/10/31 18:23:47
comment_status: open
---

# Ruby Express!!

<p>In this blog I am going to talk about "Ruby" language that I feel is a express language. Like the way we have 'Express Trains' that are faster than some other trains on the line in question, we have this express language that is faster to learn, faster to code and faster to execute. You must be wondering that this is nothing new and Ruby has been there since a decade then what is so special. For being special I think something does not have to be "New" always. When we have something old to do something in new way to make our lives easier, it becomes special. </p>
<p>We, testers, (in fact everyone) always keep looking for tools that help us to do our jobs better. It would be good if we set the context first. We are considering here a) basic scripting code to automate manual tasks and b ) automated way of testing our applications, as examples to discuss about the challenges that we face, how we usually address this and what should be the ideal way.<!--more--></p>
<p>There is a good list of pain points that come in our way when we think about automating few manual tasks using a language. Let's have a look on these challenges: </p>
<ol>
    <li>Why should we tell which type of data we are going to assign a variable. We want a language that can cooperate with this style of coding and can adapt to the data we provide to that.</li>
    <li>We want to write less code and achieve more but not at the cost of readability / maintainability. Why should we forgo conciseness and follow long style of writing the code? For testing related stuff anything that works on high level of abstraction would be a good choice. We all know how DSL helps us to achieve our goals without requiring us to write boilerplate code.</li>
    <li>Why should we wait for code to compile first? Time is money. It would be great to run the code as we edit that.</li>
</ol>

<p>So here comes that nothing new, nothing hidden gem called Ruby to say that it can help us testers to resolve these issues. With Ruby you can focus on solving the problem, instead of resolving compiler and language issues. That is how it can help you become a better automation tester by giving you the time to spend your resources creating solutions for your testing requirements. </p>
<p>Using Ruby you write solutions that express your problem domain directly and express them elegantly. So you can code faster &amp; it also means your solution stays readable and maintainable. Let me share with you what we wanted to automate in our current project and how easily we could do that with ruby. In our current project, we do following activities on regular basis:</p>
<ol>
<li>Deploy latest code(war files) on tomcat</li>
<li>Clean the DB / Delete existing data</li>
<li>Refresh another set of tables in Oracle database that help us in searching our records and other data cleaning operations.</li>
<li>Checking whether server is up</li>
<li>Onboarding of fresh data</li>
</ol>

<p>In this blog I will cover 1, 2 &amp; 4 steps. So let's start from scratch by installing ruby and other required gems:
<ol>
<li>Download &amp; Install RubyInstaller for Windows</li>
<li>Download &amp; Install DevKit. We need to associate DevKit with our ruby installation so please execute following steps:</li>
<ul>
    <li>Extract DevKit to any directory</li>
    <li>Go to cd DEVKIT_INSTALL_DIR</li>
    <li>ruby dk.rb init</li>
    <li>ruby dk.rb install</li>
</ul></p>
<li>Go to Command Prompt, do ruby -v. It should display version of Installed Ruby.
<li>Install gems for connecting to a remote machine using ssh protocol and connecting to PostgresQL database:
<ul>
<li>gem install net-ssh</li>
<li>gem install postgres-pr</li>
</ul>
</ol>
Once we have installed Ruby, DevKit and other gems, it is time to dive into ruby code.

<code>
# Connecting to a remote machine using ssh
require 'net/ssh'
$url = "test1.xebiatesting.com"
$ssh_user = "ds_user"
$ssh_password = "ds_pwd"

Net::SSH.start($url, $ssh_user, :password => $ssh_password) do |ssh|
  puts "Connected to #{$url}"

# In case you want to execute some command on this ssh connection, you can use 'exec!' method
ssh.exec!("ps -ef|grep tomcat")
end
</code>

See how easy it is to connect to a remote machine and do all the operations that you want. Apart from this we also look for process id of running tomcat. After killing that process, we deploy new war files on '/webapps' directory by using wget command. And finally we just restart the tomcat. This is how we can achieve automatic deployment of new build with 'express' ruby. Easy...isn't it?

Now let's see how we can connect to PostgesQL db and clean the tables.

<code>
require 'postgres-pr/connection'

$db = "PostgresQL"
$conn_string = "tcp://devdb1.xebiatesting.com"
$db_port = "8765"
$db_username = "postgres123"
$db_password = "postgres123"
$db_name = "BBDB1"

conn = PostgresPR::Connection.new("#{$db_name}", "#{$db_username}", "#{$db_password}", "#{$conn_string}" + ':' + "#{$db_port}")
puts conn.query("delete from table1")
puts conn.query("delete from table2")
puts conn.query("delete from table3")
puts conn.close
</code>

It feels so powerful to have a handle of database locally that is on some remote machine. We can interact with this handle to send commands and get the results of our queries from our ruby script. Great relief as now we do not have to open an application to connect to our db and then firing sql commands. We can easily do this from our script. To add to this awesomeness, we can send http(s) request to our server to check whether it is up or not. In case it is still not up and our request gets timeout then we can do this in loop to wait for server to come up. Let's see how we can achieve this by simple commands:

<code>
require 'net/http'

$web_url = "http://xebiatest:8080/ui/spring_security_login"

# Method to send http request to server.
# It evaluates the http_response and returns the response body if HTTPNotFound otherwise fires the http request again in case of HTTPNotFound or Timeout error.

def get_Web_document(url)

uri =  URI.parse(url)
ssl_mode = false
ssl_mode = true if url =~ /^https/

response = Net::HTTP.start(uri.host, uri.port) do |http|  
  http.get uri.request_uri
  end

  case response
    when Net::HTTPSuccess
      return response.body
    when Net::HTTPNotFound
      fireRequestAgain
    else
      return nil
  end

  rescue Timeout::Error => e 
  puts "Request got timeout, seems server is not up yet. Will try after 60 seconds"
  fireRequestAgain
end

# Method to ensure that if http request gets timeout then after waiting for 60 secs, it will fire another http request until 5 such timeouts.
def fireRequestAgain
  if $count > 5
        return nil
      else 
        sleep(60)
        return get_Web_document($web_url)  
      end
end

# Checks whether server sends successful response of http request
get_Web_document($web_url)
</code>

After all these examples it would be fair to say that with Ruby there are very few syntax errors, no type violations, and far fewer bugs than usual. There is less scope to get wrong. No mandatory semicolons to type mechanically at the end of each line. No unnecessary words just to keep the compiler happy. No error-prone scripting code. I am enjoying playing with this language and I am sure my peer testers will also do the same. I will post few more blogs as I do deep diving into advanced ruby scripting. Till then <em>Happy Automation & Happy Diwali!!</em>