---
layout: post
header-img: img/default-blog-pic.jpg
author: ramsharma
description: 
post_id: 9938
created: 2011/10/16 13:52:19
created_gmt: 2011/10/16 08:52:19
comment_status: open
---

# Multitasking aka Multi-Processing in PHP

<p>PHP is now quite old in industry and has been used widely in web based applications. PHP has also evolved much more as enterprise web application language. But PHP still lacks a lot many features, which are very commonly and heavily used in Java, .Net or any other enterprise language. One of those lacking features in PHP is 'Multi-tasking' or support for concurrent processing.</p>
<p>To implement multi-tasking in PHP, people have found a couple of ways like by using pcntl_fork(), exec() and curl. You can Google out those solutions easily.</p>
<p>Today, I am going to explain you one more way to  implement 'Multi-tasking' using Gearman library and Gearman server. To implement Gearman solution for PHP, you first have to install Gearman server to your machine. You can find more details about Gearman and its installation here: <a title="Gearman" href="http://gearman.org" target="_blank">http://gearman.org/</a></p>
<p>Gearman server comes in 3 flavours C, Java and Perl.</p>
<!--more-->

<p>I am using Gearman C implementation 'gearmand' as Gearman server in this blog. You can download &amp; install gearmand from here <a title="download gearman" href="http://gearman.org/index.php?id=getting_started" target="_blank">http://gearman.org/index.php?id=getting_started</a>. After installing it you will have to start the server by running this command:</p>
<p>[sourcecode language="php"]
$ /usr/local/sbin gearmand -d</p>
<h1>or</h1>
<p>$ gearmand -d
[/sourcecode]</p>
<p>If you are using windows or Mac, I would suggest you to use Java version of Gearman server. I have not tried that but I think Java version can easily be installed and run on Windows/Mac.</p>
<p>Now when you have Gearman server in place and running you can install any of the PHP client to work with Gearman server. There are two solutions available officially to use Gearman for PHP:
<ol>
    <li> 1. PHP extension (currently only supported on Linux but you can try to compile the library on Windows by using Cygwin)</li>
    <li> 2. Net_Gearman - pure PHP implementation, a pear module</li>
</ol>
I am using PHP extension on my Ubuntu machine to run examples to explain Multi-tasking. Once you installed extension on your machine (see here for help: http://docs.php.net/manual/en/book.gearman.php), do the following steps:</p>
<p>Create worker php file worker.php or gearman-worker.php. A gearman worker is thread running as worker to perform a specific task requested by clients. Like in this example we are going to use string reverse method as worker task. Here is the code for worker:</p>
<p>[sourcecode language="php"]
&lt;?php
// following example code is taken from : http://www.php.net/manual/en/gearman.examples-reverse.php
echo &quot;Starting\n&quot;;</p>
<h1>Create our worker object.</h1>
<p>$gmworker= new GearmanWorker();</p>
<h1>Add default server (localhost).</h1>
<p>$gmclient-&gt;addServer();</p>
<h1>Register function &quot;reverse&quot; with the server. Change the worker function to</h1>
<h1>&quot;reverse_fn_fast&quot; for a faster worker with no output.</h1>
<p>$gmworker-&gt;addFunction(&quot;reverse&quot;, &quot;reverse_fn&quot;);</p>
<p>print &quot;Waiting for job...\n&quot;;
while($gmworker-&gt;work())
{
  if ($gmworker-&gt;returnCode() != GEARMAN_SUCCESS)
  {
    echo &quot;return_code: &quot; . $gmworker-&gt;returnCode() . &quot;\n&quot;;
    break;
  }
}</p>
<p>function reverse_fn($job)
{
  echo &quot;Received job: &quot; . $job-&gt;handle() . &quot;\n&quot;;</p>
<p>$workload = $job-&gt;workload();
  $workload_size = $job-&gt;workloadSize();</p>
<p>echo &quot;Workload: $workload ($workload_size)\n&quot;;</p>
<p># This status loop is not needed, just showing how it works
  for ($x= 0; $x &lt; $workload_size; $x++)   {     echo &quot;Sending status: &quot; . ($x + 1) . &quot;/$workload_size complete\n&quot;;     $job-&gt;sendStatus($x, $workload_size);
    sleep(1);
  }</p>
<p>$result= strrev($workload);
  echo &quot;Result: $result\n&quot;;</p>
<p># Return what we want to send back to the client.
  return $result;
}
[/sourcecode]</p>
<p>The above code is creating a GearmanWorker and attaching it to the default Gearman server (local gearmand server process, you can pass server ip and port if your gearman server running on different machine)</p>
<p>To see if the above worker is running properly and producing expected results, you have to run the worker by using the following command:</p>
<p>[sourcecode language="bash"]
$ php gearman-worker.php
[/sourcecode]</p>
<p>Now create a client to create Job for the worker. Here is the code for the client.php or gearman-client.php:</p>
<p>[sourcecode language="php"]
&lt;?php
// following example code is taken from : http://www.php.net/manual/en/gearman.examples-reverse.php</p>
<h1>Create our client object.</h1>
<p>$gmclient= new GearmanClient();</p>
<h1>Add default server (localhost).</h1>
<p>$gmclient-&gt;addServer();</p>
<p>echo &quot;Sending job\n&quot;;</p>
<h1>Send reverse job</h1>
<p>do
{
  $result = $gmclient-&gt;do(&quot;reverse&quot;, &quot;Hello!&quot;);</p>
<p># Check for various return packets and errors.
  switch($gmclient-&gt;returnCode())
  {
    case GEARMAN_WORK_DATA:
      echo &quot;Data: $result\n&quot;;
      break;
    case GEARMAN_WORK_STATUS:
      list($numerator, $denominator)= $gmclient-&gt;doStatus();
      echo &quot;Status: $numerator/$denominator complete\n&quot;;
      break;
    case GEARMAN_WORK_FAIL:
      echo &quot;Failed\n&quot;;
      exit;
    case GEARMAN_SUCCESS:
      break;
    default:
      echo &quot;RET: &quot; . $gmclient-&gt;returnCode() . &quot;\n&quot;;
      exit;
  }
}
while($gmclient-&gt;returnCode() != GEARMAN_SUCCESS);</p>
<p>?&gt;
[/sourcecode]</p>
<p>Now when you run the above code by:</p>
<p>[sourcecode language="bash"]
$ php gearman-client.php
[/sourcecode]</p>
<p>It will generate a JOB for gearman worker(s) available on server (in our case, we created only one worker but you can run multiple workers for the same JOB).
The above code is not running the reverse job in background or you can say not running in multi-threaded style. Like we have used:</p>
<p>[sourcecode language="php"]
  $result = $gmclient-&gt;do(&quot;reverse&quot;, &quot;Hello!&quot;);
[/sourcecode]</p>
<p>Which will hold the code execution of the client until it gets the result or response from worker. To make it multi-threaded you have to modify your code a bit. For example see below:</p>
<p>[sourcecode language="php"]
&lt;?php
//http://docs.php.net/manual/en/gearman.examples-reverse-bg.php</p>
<h1>create our client object</h1>
<p>$gmclient= new GearmanClient();</p>
<h1>add the default server (localhost)</h1>
<p>$gmclient-&gt;addServer();</p>
<h1>run reverse client in the background</h1>
<p>$job_handle = $gmclient-&gt;doBackground(&quot;reverse&quot;, &quot;this is a test&quot;);</p>
<p>if ($gmclient-&gt;returnCode() != GEARMAN_SUCCESS)
{
  echo &quot;bad return code\n&quot;;
  exit;
}</p>
<p>echo &quot;done!\n&quot;;</p>
<p>?&gt;
[/sourcecode]</p>
<p>Now just by making the call to worker as:</p>
<p>[sourcecode language="php"]
$job_handle = $gmclient-&gt;doBackground(&quot;reverse&quot;, &quot;this is a test&quot;);
[/sourcecode]</p>
<p>The code execution will not halt. doBackground() method is the method which is being used for creating a separate thread to perform a specific JOB. The above program will do:</p>
<p>[sourcecode language="php"]
echo &quot;done!\n&quot;;
[/sourcecode]</p>
<p>and finish the execution as soon as the Job is sent to worker (as it has nothing more to do, you can make some more logic and then check the client status and response)</p>
<p><strong>Conclusion</strong>: Multi-taskingin PHP is not supported in PHP natively, but you have several ways to achieve the same. Gearman server and client libraries is one of the ways you can choose to implement multi-processing in PHP very easily. You can user Gearman library for processing heavy application like: image processing, resizing or do bulk data manipulation, sending emails in bulk etc.</p>

## Comments

**[János Szurovecz](#6027 "2011-10-17 01:35:06"):** First of all, it's a good article about Gearman. But basically it is not multithreading, it's multi-processing. Of cource in some cases it is better because you can use more than one machine, but processes cannot share resources for each other, threads can. The other thing is most of available PHP libraries doesn't handle long-running processes. For example it's not enough to open a DB connection, you have to check later its availability.

**[Ram Sharma](#6029 "2011-10-17 20:41:55"):** Thanks Janos for your feedback :) . I will definitely check the DB problems.

