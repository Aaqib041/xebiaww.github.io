---
layout: post
header-img: img/default-blog-pic.jpg
author: vhazratiblog
description: 
post_id: 264
created: 2007/07/16 12:12:49
created_gmt: 2007/07/16 10:12:49
comment_status: open
---

# Playing Another User Role Using ACEGI

Recently, on one of our projects we had a requirement to allow the ROLE_ADMIN to login as another user without knowing or changing the password of that user. For example 'Jack' has the ROLE_ADMIN and 'Suzy' has the ROLE_USER. Now 'Jack' wants to login as 'Suzy' without knowing her password and carry out some tasks on her behalf acting as her when 'Suzy' is unavailable and some work needs to be done, of course you should provide a mechanism to audit and log whenever 'Jack' wants to play a different role.

This is fairly easy to implement using Acegi

The SwitchUserProcessingFilter in Acegi helps to achieve this functionality. The steps below will show how to configure and use it

**1\. **In the filter chain for acegi, include the filter SwitchUserProcessingFilter as

[xml]  CONVERT_URL_TO_LOWERCASE_BEFORE_COMPARISON PATTERN_TYPE_APACHE_ANT /images/**=#NONE# /scripts/**=#NONE# /styles/**=#NONE# / **=httpSessionContextIntegrationFilter,authenticationProcessingFilter,securityContextHolderAwareRequestFilter,rememberMeProcessingFilter, anonymousProcessingFilter,exceptionTranslationFilter,filterInvocationInterceptor,switchUserProcessingFilter  [/xml]

Make sure that this filter is placed AFTER filterInvocationInterceptor and FilterSecurityInterceptor else any role would be able to do "Role play" rather than ROLE_ADMIN role. Remember it is filter chaining.

**2\. **Define the bean definition for SwitchUserProcessingFilter as

[xml]  /j_acegi_switch_user /j_acegi_exit_user /mainMenu.html [/xml]

here you would just need to change the targetUrl depending on where you need to go after the SWITCH.

userDao is the Dao which you use for your daoAuthenticationProvider [xml]  [/xml]

**3.** In your filterInvokationFilter be sure to restrict the access just for ROLE_ADMIN, you definitely don't want all your users logging in as someone else.

[xml]  PATTERN_TYPE_APACHE_ANT ...... ...... /j_acegi_switch_user*=ROLE_ADMIN  [/xml]

**4.** Now whenever the ROLE_ADMIN wants to role play as another user The ROLE_ADMIN logs in with his username and password. Then,there should be a link which has the following

**http://localhost:8080/j_acegi_switch_user?j_username=vikas**

where 'vikas' is the username of the user that the ROLE_ADMIN wants to play as. Since only the ROLE_ADMIN should be allowed to see this link you can use the Acegi taglibs to make the link available only to ROLE_ADMIN

[xml]  <> [/xml]

When successful switch happens, the user's SecurityContextHolder will be updated to reflect the specified user and will also contain an additional SwitchUserGrantedAuthority which contains the original user. To 'exit' from a user context, the user will then need to access a URL (see exitUserUrl) that will switch back to the original user as identified by the SWITCH_USER_GRANTED_AUTHORITY

To exit from the Role play there should be a link to say

**http://localhost:8080/j_acegi_exit_user**

This would bring the ROLE_ADMIN to his own account.

Nested switching and exit is also possible. What this means is that if Jack and Suzy both have ROLE_ADMIN, then Jack can switch to become Suzy who can further switch to become John. Now once the user exits from John realm he comes to Suzy and then on another exit back to Jack.

**Conclusion: **Switching users is can be easily configured using Acegi. However remember that with 'Great Power comes Great Responsibility' so use this with care.