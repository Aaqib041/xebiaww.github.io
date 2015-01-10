---
layout: post
header-img: img/default-blog-pic.jpg
author: rjaiswal
description: 
post_id: 7989
created: 2011/03/15 16:19:57
created_gmt: 2011/03/15 11:19:57
comment_status: open
---

# Java and Ruby integration via SOAP

<p>In my last blog we talked about Ruby and Java integration via JRuby. Since JRuby runs on JVM, bytecode can be shared but in case you do not want a JVM level integration then SOAP is usually your next level of support.</p>
<p>Imagine a scenario where you want to call a SOAP server written in Java from Ruby, this is quite common as most enterprises have their middleware written in Java and new application can be written quickly in Ruby/Rails. Here is how you can go about integrating the two -</p>
<!--more-->

<p><span style="color: #0000ff;"><strong>The Java Server</strong></span></p>
<p>I am not going to cover this in detail here but for new deployments Apache CXF is quite popular. You can integrate it with Spring as given <a href="https://cwiki.apache.org/CXF20DOC/writing-a-service-with-spring.html">here</a>. The Maven dependencies are as follows -</p>
<p>[code language="xml"]
    &lt;properties&gt;
        &lt;cxf.version&gt;2.3.3&lt;/cxf.version&gt;
        &lt;spring.version&gt;3.0.4.RELEASE&lt;/spring.version&gt;
    &lt;/properties&gt;</p>
<pre><code>&amp;lt;dependencies&amp;gt;
    &amp;lt;!-- CXF dependencies --&amp;gt;
    &amp;lt;dependency&amp;gt;
        &amp;lt;groupId&amp;gt;org.apache.cxf&amp;lt;/groupId&amp;gt;
        &amp;lt;artifactId&amp;gt;cxf-rt-frontend-jaxws&amp;lt;/artifactId&amp;gt;
        &amp;lt;version&amp;gt;${cxf.version}&amp;lt;/version&amp;gt;
    &amp;lt;/dependency&amp;gt;
    &amp;lt;dependency&amp;gt;
        &amp;lt;groupId&amp;gt;org.apache.cxf&amp;lt;/groupId&amp;gt;
        &amp;lt;artifactId&amp;gt;cxf-rt-transports-http&amp;lt;/artifactId&amp;gt;
        &amp;lt;version&amp;gt;${cxf.version}&amp;lt;/version&amp;gt;
    &amp;lt;/dependency&amp;gt;
    &amp;lt;!-- End CXF dependencies --&amp;gt;

    &amp;lt;!-- Spring dependencies --&amp;gt;
    &amp;lt;!-- add other dependencies --&amp;gt;
&amp;lt;/dependencies&amp;gt;
</code></pre>
<p>[/code]</p>
<p>The Java code looks like -</p>
<p>[code language="java"]
@WebService
public interface EmployeeService {</p>
<pre><code>Employee findEmployeeByID(@WebParam(name = &amp;quot;employeeID&amp;quot;)long empID);
boolean storeEmployee(@WebParam(name = &amp;quot;employee&amp;quot;)Employee emp);
</code></pre>
<p>}
[/code]</p>
<p>And the Implementation looks like -</p>
<p>[code language="java"]
@Component(&quot;employeeService&quot;)
@WebService(endpointInterface = &quot;in.mysoapserver.services.EmployeeService&quot;)
public class EmployeeServiceImpl implements EmployeeService {</p>
<pre><code>@Override
public Employee findEmployeeByID(long empID) {
    Employee employee = new Employee();
    employee.setEmpID(101);
    employee.setName(&amp;quot;Rocky&amp;quot;);
    employee.setAge(32);
    return employee;
}

@Override
public boolean storeEmployee(Employee emp) {
    //Store Employee
    return true;
}
</code></pre>
<p>}
[/code]</p>
<p>After deploying the project in Tomcat you can get the WSDL from a link like - <strong>http://localhost:8080/mysoapserver/soap?wsdl</strong></p>
<p>Once you have the WSDL you should use SoapUI for some testing to check if the SOAP requests are working.</p>
<p><span style="color: #0000ff;"><strong>The Ruby Client</strong></span>
Now we are ready to create the SOAP client in Ruby. Please note the instructions below are same for Ruby or JRuby.</p>
<p>First install soap4r gem -
<strong>gem install soap4r</strong></p>
<p>Download (and unpack) the soap4r binary .tar.gz also from <a href="http://dev.ctor.org/soap4r">here</a></p>
<p>Now, just to organize things create a folder say "ruby_soap_client" and copy the WSDL of the SOAP services we created there.</p>
<p>Got to the folder where the WSDL is copied and run -
<strong>~/01rocky/02apps/soap4r-1.5.8/bin/wsdl2ruby.rb --type client --wsdl mysoapserver.wsdl</strong></p>
<ul>
<li>Change the path to wsdl2ruby.rb as per your setup.</li>
</ul>
<p>This will create a set of Ruby scripts and also create an example client. Since we will make our own client you can safely delete the <em>Client.rb and </em>Driver.rb scripts.</p>
<p>Now let us create our own Ruby client -</p>
<p>[code]
require 'soap/wsdlDriver'
require 'EmployeeServiceImplService.rb'</p>
<p>wsdl = &quot;mysoapserver.wsdl&quot;</p>
<p>employee_service = SOAP::WSDLDriverFactory.new(wsdl).create_rpc_driver</p>
<p>response = employee_service.findEmployeeByID(FindEmployeeByID.new(10))
puts response.return.name
puts response.return.age</p>
<p>response = employee_service.storeEmployee(StoreEmployee.new(Employee.new(30,102,&quot;Tushar&quot;)))
puts response.return
[/code]</p>
<p>Lets examine each line in detail.
Line 1 - Just loading the required libraries for soap4r
Line 2 - The 3rd file that wsdl2ruby.rb script generated. We'll look at it in a minute.
Line 4 - Just a variable for the wsdl file name. Change it according to your setup.
Line 6 - We create a SOAP RPC driver using the wsdl we saved earlier.
Line 8/12 - Next we make the SOAP call. But wait a minute, what is being passed as a parameter here?</p>
<p>The answer to this can be found if we look at the SOAP request we made from SoapUI -</p>
<p>[code language="xml"]
&lt;soapenv:Envelope xmlns:soapenv=&quot;http://schemas.xmlsoap.org/soap/envelope/&quot; xmlns:ser=&quot;http://services.mysoapserver.in/&quot;&gt;
   &lt;soapenv:Header/&gt;
   &lt;soapenv:Body&gt;
      &lt;ser:findEmployeeByID&gt;
         &lt;employeeID&gt;10&lt;/employeeID&gt;
      &lt;/ser:findEmployeeByID&gt;
   &lt;/soapenv:Body&gt;
&lt;/soapenv:Envelope&gt;
[/code]</p>
<p>And for storeEmployee call -</p>
<p>[code language="xml"]
&lt;soapenv:Envelope xmlns:soapenv=&quot;http://schemas.xmlsoap.org/soap/envelope/&quot; xmlns:ser=&quot;http://services.mysoapserver.in/&quot;&gt;
   &lt;soapenv:Header/&gt;
   &lt;soapenv:Body&gt;
      &lt;ser:storeEmployee&gt;
         &lt;employee&gt;
            &lt;age&gt;30&lt;/age&gt;
            &lt;empID&gt;102&lt;/empID&gt;
            &lt;name&gt;Tushar&lt;/name&gt;
         &lt;/employee&gt;
      &lt;/ser:storeEmployee&gt;
   &lt;/soapenv:Body&gt;
&lt;/soapenv:Envelope&gt;
[/code]</p>
<p>If we ignore the XML namespaces, our request is wrapped in a "findEmployeeByID" tag (for the findEmployeeByID call). Therefore, when we gave the wsdl2ruby.rb scpirt our WSDL reference, it created the equivalent Ruby classes for all wrappers, request and response types and stored it in a file which we referenced at Line 2.</p>
<p>So looking at the sample requests in SoapUI we can create wrapper and request objects and similarly parse the response received. Its as simple as that. In case you are stuck, you can look at all the classes created by the wsdl2ruby script by having a look at the generated file (EmployeeServiceImplService.rb in our case).</p>