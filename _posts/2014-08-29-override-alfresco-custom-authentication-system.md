---
layout: post
header-img: img/default-blog-pic.jpg
author: SOURABH AGGARWAL
description: 
post_id: 18861
created: 2014/08/29 11:51:48
created_gmt: 2014/08/29 06:51:48
comment_status: open
---

# Override Alfresco Custom Authentication System

<p><span style="font-family: arial, helvetica, sans-serif;">Alfresco is one of the most popular and widely used Document management system in the world.</span></p>
<p><span style="font-family: arial, helvetica, sans-serif;">In this blog I am explaining how we can override the complete alfresco authentication system.</span></p>
<p><span style="font-family: arial, helvetica, sans-serif;">Alfresco is the Document Management System and in these type of system document security is one of the key concern of all whose who are working with these type of systems.</span></p>
<p><span style="font-family: arial, helvetica, sans-serif;">In this blog I am not bothering about document Authorization my main concern is about Alfresco authentication system.</span></p>
<p>Alfresco authentication system is based on modules in which each of the module is the Subsystem for authentication we can use one or more subsystem at one time for authentication within alfresco leave other at same time by defining it within the chain of authentication.</p>
<p>When user try to Login within the Alfresco. Authentication system check for all those subsystem which are configured in the authentication chain of alfresco if user try to Login with credential present in one authentication subsystem but not in another user will be able to Login but if user is not valid in all the subsystem configured in authentication chain then he/she is not able to Login within the Alfresco system.</p>
<p><span style="font-family: arial, helvetica, sans-serif;">In Alfresco we are having many kind of authentication subsystem employed for user authentication.</span>
<table style="width: 100%;" cellspacing="0" cellpadding="4"><colgroup> <col width="128*" /> <col width="128*" /> </colgroup>
<tbody>
<tr valign="top">
<td width="50%"><span style="font-family: arial, helvetica, sans-serif;">TYPE</span></td>
<td width="50%"><span style="font-family: arial, helvetica, sans-serif;">DESCRIPTION</span></td>
</tr>
<tr valign="top">
<td width="50%"><span style="font-family: arial, helvetica, sans-serif;">ALFRESCO NTLM</span></td>
<td width="50%"><span style="font-family: arial, helvetica, sans-serif;">This is the default alfresco way of authentication comes with alfresco sometime called Native Alfresco Authentication.</span></td>
</tr>
<tr valign="top">
<td width="50%"><span style="font-family: arial, helvetica, sans-serif;">EXTERNAL</span></td>
<td width="50%"><span style="font-family: arial, helvetica, sans-serif;">Authentication based on some external authentication mechanism.</span></td>
</tr>
<tr valign="top">
<td width="50%"><span style="font-family: arial, helvetica, sans-serif;">LDAP-AD</span></td>
<td width="50%"><span style="font-family: arial, helvetica, sans-serif;">This authentication system provide user authentication from LDAP active directory based on LDAP protocol it also allow user export from LDAP active directory.</span></td>
</tr>
<tr valign="top">
<td width="50%"><span style="font-family: arial, helvetica, sans-serif;">KERBEROS</span></td>
<td width="50%">
<p align="left"><span style="font-family: arial, helvetica, sans-serif;"><span style="color: #000000;">Kerberos provide very strong encryption mechanism compared to other. </span><span style="color: #000000;">Java Authentication and Authorization Service (JAAS) is used within the Kerberos subsystem to support Kerberos authentication of </span><span style="color: #000000;">user name </span><span style="color: #000000;">and password.</span></span></p>
</td>
</tr>
<tr valign="top">
<td width="50%"><span style="font-family: arial, helvetica, sans-serif;">LDAP</span></td>
<td width="50%"><span style="font-family: arial, helvetica, sans-serif;">This authentication system provide user authentication from open LDAP based on LDAP protocol it also allow user export from open LDAP.</span></td>
</tr>
<tr valign="top">
<td width="50%"><span style="font-family: arial, helvetica, sans-serif;">PASSTHRU</span></td>
<td width="50%">
<p align="left"><span style="font-family: arial, helvetica, sans-serif;"><span style="color: #000000;">This</span><span style="color: #000000;">authentication </span><span style="color: #000000;">system </span><span style="color: #000000;">is</span><span style="color: #000000;">used to replace the </span><span style="color: #000000;">Us</span><span style="color: #000000;">er database </span><span style="color: #000000;">in Alfdresco System</span><span style="color: #000000;"> with a Windows server </span><span style="color: #000000;">controller, </span><span style="color: #000000;">domain controller, or list of servers to authenticate users.</span></span></p>
</td>
</tr>
</tbody>
</table>
<span style="font-family: arial, helvetica, sans-serif;">We can use more than one authentication system at one time to allow user within alfresco system.</span></p>
<p><span style="font-family: arial, helvetica, sans-serif;">There is authentication chain within alfresco where we can define which all <span style="font-size: 14px;">authentication</span> subsystem we want to employ for authentication in alfresco.</span></p>
<p><span style="font-family: arial, helvetica, sans-serif;">Like :- </span></p>
<p><code>authentication.chain= ldap1:ldap,alfrescoNtlm1:alfrescoNtlm;</code></p>
<p><span style="color: #000000; font-size: 14px; font-family: arial, helvetica, sans-serif;">Here we have used two authentication system one is Open LDAP and another is Alfresco NTLM.</span></p>
<p><span style="color: #000000; font-size: 14px; font-family: arial, helvetica, sans-serif;">Here we start how to override the complete alfresco authentication and write our own code for authenticating user.</span></p>
<p><span style="font-family: arial, helvetica, sans-serif;">STEP 1 : Create Folder MyCustomAuthentication in project directory <span style="color: #008000;">Alfresco_Installation/tomcat/webapps/alfresco/WEB-INF/classes/alfresco/subsystems/Authentication.</span></span></p>
<p><span style="font-family: arial, helvetica, sans-serif;">STEP 2 : Create file <span style="color: #008000;">mycustom-authentication.properties</span> within <span style="color: #008000;">MyCustomAuthentication</span> folder with following content.</span></p>
<p><code>external.authentication.defaultAdministratorUserNames=admin</code></p>
<p><span style="font-family: arial, helvetica, sans-serif;">STEP 3: Create file mycustom-authentication-context.xml within MyCustomAuthentication folder with following content.</span>
<p style="padding-left: 30px;"><span style="color: #008000; font-family: arial, helvetica, sans-serif;">&lt;?xmlversion=<i>'1.0'</i>encoding=<i>'UTF-8'</i>?&gt;</span></p>
<p style="padding-left: 30px;" align="left"><span style="color: #008000; font-family: arial, helvetica, sans-serif;">&lt;!DOCTYPEbeansPUBLIC'-//SPRING//DTD BEAN//EN''http://www.springframework.org/dtd/spring-beans.dtd'&gt;</span></p>
<p style="padding-left: 30px;" align="left"><span style="color: #008000; font-family: arial, helvetica, sans-serif;">&lt;beans&gt;</span></p>
<p style="padding-left: 30px;" align="left"><span style="color: #008000; font-family: arial, helvetica, sans-serif;">&lt;beanid=<i>"authenticationComponent"</i>class=<i>"org.alfresco.repo.security.authentication.</i><i>MyCustomAuthentication</i><i>.</i><i>My</i><i>CustomAuthenticationImpl"</i></span></p>
<p style="padding-left: 30px;" align="left"><span style="color: #008000; font-family: arial, helvetica, sans-serif;">parent=<i>"authenticationComponentBase"</i>&gt;</span></p>
<p style="padding-left: 30px;" align="left"><span style="color: #008000; font-family: arial, helvetica, sans-serif;">&lt;propertyname=<i>"nodeService"</i>&gt;</span></p>
<p style="padding-left: 30px;" align="left"><span style="color: #008000; font-family: arial, helvetica, sans-serif;">&lt;refbean=<i>"nodeService"</i>/&gt;</span></p>
<p style="padding-left: 30px;" align="left"><span style="color: #008000; font-family: arial, helvetica, sans-serif;">&lt;/property&gt;</span></p>
<p style="padding-left: 30px;" align="left"><span style="color: #008000; font-family: arial, helvetica, sans-serif;">&lt;propertyname=<i>"personService"</i>&gt;</span></p></p>

## Comments

**[lelak](#9511 "2014-09-01 06:07:47"):** Hello Sourabh, congrats for the great article. My name is Tiago director at LELAK hipermidia. We have a Alfresco Project that is requiring some expertise that we don't have currently on our team. We need to setup Alfresco Community 5 to Authenticate through Google Apps accounts. We are considering these options (in order of preferred solution): 1) Authenticate on Alfresco with google Apps accounts directly using some SSO support like: https://github.com/gdepourtales/share-oauth-sso 2) Authenticate through openLDAP and synchronize openLDAP with Google APPS (GADS) 3) Authenticate with passthrough and active directory, using the GADS (google directory sync) to synchronize accounts with the Active Directory. Are you interested on working on some Alfresco tasks with us? Please let us know, and give your cost for this task. Please leave this comment private. Thank you! Tiago | LELAK

