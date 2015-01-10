---
layout: post
header-img: img/default-blog-pic.jpg
author: gkohli
description: 
post_id: 8816
created: 2011/05/21 14:08:35
created_gmt: 2011/05/21 09:08:35
comment_status: open
---

# Accessing Crowd with Python and enabling SSO

<p style="text-align: left;">What we are trying to have here is a python client which can talk to Crowd and be part of the Crowd SSO (Single sign-on)  along with other applications written in Java/.NET[<a href="#1">1</a>]. This is possible because of the SOAP &amp; REST API's which the Crowd exposes. In our organization we are still using Crowd 2.0.6, so I would be explaining only about how to use  SOAP API's and not  the REST API's.</p>

<p style="text-align: left;">Crowd is a application from Atlassian world which provides you a framework for single sign-on and centralized authentication. And underneath it can connect to a lot of User Directories like LDAP, Microsoft Active Directory, SUN and Fedora Directories and many more. So for all your enterprise applications, you can use Crowd to manage all the User details, and it greatly simplifies the thing as now there is just one location where all the information is stored.</p>

<p style="text-align: left;"><!--more--></p>

<p style="text-align: left;">The Crowd's SSO capabilities enables you to log into one website and move between your other web applications without having to log in repeatedly. This provides a lot better User interaction and since all the applications are internal to one organization, it also doesn't make sense for each user to sign into each one of them again and again, even though he is using a same browser.</p>

<p style="text-align: left;">Now most of the SSO implementations are based on the concept of storing some HTTP cookies, since you need some way to acknowledge who is coming to your website and same is with Crowd Server. Usually everyone stores some authentication token in cookies which each application can use and have it validated by the main SSO server. Now the only problem with HTTP cookies is that only the application of same domain can have access to it. So now there could be two possibilities, either our python client is in the same domain as Crowd, in which case we can use the Crowd domain information or we are in some other domain and we will have to explicitly set our own cookie.</p>

<p style="text-align: left;">Let's get into some code now.</p>

<p style="text-align: left;">The code is written in python, but it is pretty simple SOAP usage, so you could  write the same thing in any language, you only need some basic SOAP understanding .</p>

<p style="text-align: left;">We start with installing the crowd server first. Once we have the crowd server, we can access the SOAP wsdl at http://localhost:8095/crowd/services/SecurityServer?wsdl [<a href="#2">2</a>]. The WSDL can help you understand all the functions which you can call on the Crowd Server. I have currently used just 4 functions: authenticateApplication, authenticatePrincipal, isValidPrincipalToken and getCookieInfo. For integration with Crowd application server, we first need to create a application from within Crowd server and specify the name, password and the ip address from where we would be using the application. This is just the snippets and full  code is uploaded on github[<a href="#3">3</a>]</p>

<p style="text-align: left;">We use Python Suds module for talking to the Crowd SOAP endpoint. We create a soap client using "Client(self.wsdlurl,doctor=doctor)"  and we  pass in the url for the Soap Wsdl. The second parameter which you see is the Suds way to clean the wsdl xml. The Crowd wsdl has some namespace problems for which we create a ImportDoctor which fixes the missing namespace for few of the elements in WSDL.</p>

<p>[sourcecode lang="python"]
class CrowdPy:
    def <strong>init</strong>(self):
        cfg = ConfigParser()
        cfg.read('crowd.properties')
        self.appname = cfg.get('default','application.name')
                #.... Read all the properties from the Configuration File</p>
<pre><code>    #Use Pythin Suds module to create a SOAP client
def createClient(self):
            # The Crowd Wsdl has some problems in namespace, so we need to patch thoes
    patches = { &amp;quot;urn:SecurityServer&amp;quot;: [&amp;quot;http://authentication.integration.crowd.atlassian.com&amp;quot;}
    doctor = dr.ImportDoctor()

    # Patch all the imports into the proper targetNamespaces
    for targetNamespace in patches:
        for nsimport in patches[targetNamespace]:
        imp = dr.Import(nsimport)
        imp.filter.add(targetNamespace)
        doctor.add(imp)
            # Creating the Suds Client
    self.client = Client(self.wsdlurl,doctor=doctor)
    return self.client
</code></pre>
<p>[/sourcecode]</p>
<p>Using the client we have to authenticate the application using the application name and the password specified when a new application was added in Crowd. The Crowd internally uses this information along with the ip address from where the request is coming to validate the application and then returns a application authentication token which we would use further on to authenticate Principal (User)</p>
<p>[sourcecode lang="python"]
    def authenticateApplication(self):
        auth_context = self.client.factory.create('ns1:ApplicationAuthenticationContext')
        auth_context.name = 'application name specified in crowd'
        auth_context.credential.credential = 'application password specified in crowd'
        self.token = self.client.service.authenticateApplication(auth_context)
        return self.token</p>
<p>[/sourcecode]</p>
<p>Now we call the authenticatePrincipal function to authenticate the User using the application authentication token and the user credentials. This would return a user authentication token which we can save in the cookies. This is perfectly save, as this is valid only if used along with the application authentication token and also from the same machine ip address.</p>
<p>[sourcecode lang="python"]
    def authenticatePrincipal(self,username,passwd):
        user_context = self.client.factory.create('ns1:UserAuthenticationContext')
        user_context.application = 'application name specified in crowd'
        user_context.name = username
        user_context.credential.credential = passwd
        self.user_token = self.client.service.authenticatePrincipal(self.token,user_context)
        return self.user_token
[/sourcecode]</p>
<p>So at this moment we have the user authenticated by Crowd, but where is the SSO ? For having the SSO feature, we have to do two things before even asking for username/password from user, first whenever we need the user authentication we need to check if the user was already authenticated by some other application. For doing that we have to check for cookies with key "cookie.token_key" and the value of the this key is the user authentication token saved by some other application. I would be telling how to validate this token in a minute. In case we don't have such a cookie in browser, we then ask username/password and call the function authenticatePrincipal specified above and then save the user token back into a new cookie.</p>
<p>And here is the code to validate the cookie in case it was already there in the browser. We have to use the user authentication token from the cookie and the application token and along with that we pass the Validataion Factors, which should have the correct IP address where the proxy application is running. In my case both the proxy and the Crowd server were on the same machine, so I have used "127.0.0.1"</p>
<p>[sourcecode lang="python"]
       def isValidPrincipalToken(self,user_token):
        vf1 = self.client.factory.create('ns1:ValidationFactor')
        vf1.name = 'remote_host'
        vf1.value =  '127.0.0.1'
        vf = [vf1]
        isValid = self.client.service.isValidPrincipalToken(self.token,user_token,vf)
        return isValid
[/sourcecode]</p>
<p>So in this way either the application uses the cookie saved by some other application in the browser and have it validated by the Crowd server, or ask the user for username/password and have it authenticated from Crowd and creates a new cookie in the browser for all the other applications within the same SSO group to use. You just have to make sure the domain name in the cookie is same for all the applications.</p>
<ol>
<li><a title="Java Integration Libraries" name="1" href="http://confluence.atlassian.com/display/CROWDDEV/Java+Integration+Libraries">Java Integration Libraries</a></li>
</ol>