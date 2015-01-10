---
layout: post
header-img: img/default-blog-pic.jpg
author: jitens
description: 
post_id: 13622
created: 2012/05/04 11:46:05
created_gmt: 2012/05/04 06:46:05
comment_status: open
---

# Viewing the complete  source code and stealing passwords from  Websites   with PHP-CGI based  setups

<p><b>Viewing the complete  source code and stealing passwords from  Websites   with PHP-CGI based  setups</b></p>
<p>Websites Running on PHP-CGI based setup are completely vulnerable to   Remote Code execution.While parsing query string Parameters from a php file, The PHP-CGI based setups are ending up executing command line switches on their servers.  An user can simply run a command line switch on PHP-CGI server using the flaw in query execution</p>
<p>If a website is running on a PHP-CGI based setup. Just simply pass a command line argument  along with the URL and see the fun.  ‘Show source’ Switch like “–s” passed via query string from a php page would result in displaying the entire code of the page to the attacker.This Vulnerability has also resulted in a warning being issued by Computer Emergency Response Team in US. The developers despite of working overtime are yet to find any fix for this mess till now.</p>
<p>The above vulnerability is a treat to all those who want to have some extra fun on any website built in PHP and running in CGI based setup.Here are few Quickies if want to have some fun too
<ul>
    <li>Google for hosting providers providing servers for PHP-CGI bases setups</li>
    <li>Run a Reverse IP lookup in BING search engine. (It’s the only lame search engine running reverse lookups) Use it J</li>
    <li>Now once the result are listed,  look for the ones in php-CGI based setups</li>
    <li>Choose a target. Navigate the site for some login pages or any search pages.</li>
    <li>Look at the Page source and find out the name of php scripts doing authentication or  making database  connections.</li>
    <li>Run the source code display  Switch –s passed via query string from a php page</li>
    <li>For example You could see the source of  the demo.php  on localhost via <a href="http://%20localhost/demo.php?-s">http:// localhost/demo.php?-s</a>.</li>
    <li>This will display the complete source code in plain text.</li>
    <li>Once you see the scripts in the plain text. Take passwords, Upload shells, Do server rooting, deface.</li>
    <li>Do what ever you like till they fix this but be nice : )</li>
</ul>
Cheers !</p>