---
layout: post
header-img: img/default-blog-pic.jpg
---

# Override Alfresco Custom Authentication System

Alfresco is one of the most popular and widely used Document management system in the world. In this blog I am explaining how we can override the complete alfresco authentication system. Alfresco is the Document Management System and in these type of system document security is one of the key concern of all whose who are working with these type of systems. In this blog I am not bothering about document Authorization my main concern is about Alfresco authentication system. Alfresco authentication system is based on modules in which each of the module is the Subsystem for authentication we can use one or more subsystem at one time for authentication within alfresco leave other at same time by defining it within the chain of authentication. When user try to Login within the Alfresco. Authentication system check for all those subsystem which are configured in the authentication chain of alfresco if user try to Login with credential present in one authentication subsystem but not in another user will be able to Login but if user is not valid in all the subsystem configured in authentication chain then he/she is not able to Login within the Alfresco system. In Alfresco we are having many kind of authentication subsystem employed for user authentication.

TYPE
DESCRIPTION

ALFRESCO NTLM
This is the default alfresco way of authentication comes with alfresco sometime called Native Alfresco Authentication.

EXTERNAL
Authentication based on some external authentication mechanism.

LDAP-AD
This authentication system provide user authentication from LDAP active directory based on LDAP protocol it also allow user export from LDAP active directory.

KERBEROS

Kerberos provide very strong encryption mechanism compared to other. Java Authentication and Authorization Service (JAAS) is used within the Kerberos subsystem to support Kerberos authentication of user name and password.

LDAP
This authentication system provide user authentication from open LDAP based on LDAP protocol it also allow user export from open LDAP.

PASSTHRU

Thisauthentication system isused to replace the User database in Alfdresco System with a Windows server controller, domain controller, or list of servers to authenticate users.

We can use more than one authentication system at one time to allow user within alfresco system. There is authentication chain within alfresco where we can define which all authentication subsystem we want to employ for authentication in alfresco. Like :-  `authentication.chain= ldap1:ldap,alfrescoNtlm1:alfrescoNtlm;` Here we have used two authentication system one is Open LDAP and another is Alfresco NTLM. Here we start how to override the complete alfresco authentication and write our own code for authenticating user. STEP 1 : Create Folder MyCustomAuthentication in project directory Alfresco_Installation/tomcat/webapps/alfresco/WEB-INF/classes/alfresco/subsystems/Authentication. STEP 2 : Create file mycustom-authentication.properties within MyCustomAuthentication folder with following content. `external.authentication.defaultAdministratorUserNames=admin` STEP 3: Create file mycustom-authentication-context.xml within MyCustomAuthentication folder with following content.

<?xmlversion=_'1.0'_encoding=_'UTF-8'_?>

<!DOCTYPEbeansPUBLIC'-//SPRING//DTD BEAN//EN''http://www.springframework.org/dtd/spring-beans.dtd'>

<beans>

<beanid=_"authenticationComponent"_class=_"org.alfresco.repo.security.authentication.__MyCustomAuthentication__.__My__CustomAuthenticationImpl"_

parent=_"authenticationComponentBase"_>

<propertyname=_"nodeService"_>

<refbean=_"nodeService"_/>

</property>

<propertyname=_"personService"_>

## Comments

**[lelak](#9511 "2014-09-01 06:07:47"):** Hello Sourabh, congrats for the great article. My name is Tiago director at LELAK hipermidia. We have a Alfresco Project that is requiring some expertise that we don't have currently on our team. We need to setup Alfresco Community 5 to Authenticate through Google Apps accounts. We are considering these options (in order of preferred solution): 1) Authenticate on Alfresco with google Apps accounts directly using some SSO support like: https://github.com/gdepourtales/share-oauth-sso 2) Authenticate through openLDAP and synchronize openLDAP with Google APPS (GADS) 3) Authenticate with passthrough and active directory, using the GADS (google directory sync) to synchronize accounts with the Active Directory. Are you interested on working on some Alfresco tasks with us? Please let us know, and give your cost for this task. Please leave this comment private. Thank you! Tiago | LELAK

