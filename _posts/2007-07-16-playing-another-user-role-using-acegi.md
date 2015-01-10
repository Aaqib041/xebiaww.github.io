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

<p>Recently, on one of our projects we had a requirement to allow the ROLE_ADMIN to login as another user without knowing or changing the password of that user. For example 'Jack' has the ROLE_ADMIN and 'Suzy' has the ROLE_USER. Now 'Jack' wants to login as 'Suzy' without knowing her password and carry out some tasks on her behalf acting as her when 'Suzy' is unavailable and some work needs to be done, of course you should provide a mechanism to audit and log whenever 'Jack' wants to play a different role.</p>
<p>This is fairly easy to implement using Acegi</p>
<p>The SwitchUserProcessingFilter in Acegi helps to achieve this functionality. The steps below will show how to configure and use it</p>
<!--more-->

<p><strong>1. </strong>In the filter chain for acegi, include the filter SwitchUserProcessingFilter as</p>
<p>[xml]
<!-- ======================== FILTER CHAIN ======================= -->
    <bean id="filterChainProxy" class="org.acegisecurity.util.FilterChainProxy">
        <property name="filterInvocationDefinitionSource">
            <value>
                CONVERT_URL_TO_LOWERCASE_BEFORE_COMPARISON
                PATTERN_TYPE_APACHE_ANT
                /images/<strong>=#NONE#
                /scripts/</strong>=#NONE#
                /styles/<strong>=#NONE#
                /       </strong>=httpSessionContextIntegrationFilter,authenticationProcessingFilter,securityContextHolderAwareRequestFilter,rememberMeProcessingFilter, anonymousProcessingFilter,exceptionTranslationFilter,filterInvocationInterceptor,switchUserProcessingFilter
            </value>
        </property>
    </bean>
[/xml]</p>
<p>Make sure that this filter is placed AFTER filterInvocationInterceptor and FilterSecurityInterceptor else any role would be able to do "Role play" rather than ROLE_ADMIN role. Remember it is filter chaining.</p>
<p><strong>2. </strong>Define the bean definition for SwitchUserProcessingFilter as</p>
<p>[xml]
    <bean id="switchUserProcessingFilter" class="org.acegisecurity.ui.switchuser.SwitchUserProcessingFilter">
        <property name="userDetailsService" ref="userDao" />
        <property name="switchUserUrl"><value>/j_acegi_switch_user</value></property>
        <property name="exitUserUrl"><value>/j_acegi_exit_user</value></property>
        <property name="targetUrl"><value>/mainMenu.html</value></property>
    </bean>
[/xml]</p>
<p>here you would just need to change the targetUrl depending on where you need to go after the SWITCH.</p>
<p>userDao is the Dao which you use for your daoAuthenticationProvider
[xml]
    <bean id="daoAuthenticationProvider" class="org.acegisecurity.providers.dao.DaoAuthenticationProvider">
         <property name="userDetailsService" ref="userDao"/>
         <property name="userCache" ref="userCache"/>
         <property name="passwordEncoder" ref="passwordEncoder"/>
    </bean>
[/xml]</p>
<p><strong>3.</strong> In your filterInvokationFilter be sure to restrict the access just for ROLE_ADMIN, you definitely don't want all your users logging in as someone else.</p>
<p>[xml]
    <bean id="filterInvocationInterceptor" class="org.acegisecurity.intercept.web.FilterSecurityInterceptor">
        <property name="authenticationManager" ref="authenticationManager"/>
        <property name="accessDecisionManager" ref="accessDecisionManager"/>
        <property name="objectDefinitionSource">
            <value>
                PATTERN_TYPE_APACHE_ANT
                ......
                ......
               /j_acegi_switch_user*=ROLE_ADMIN
            </value>
        </property>
    </bean>
[/xml]</p>
<p><strong>4.</strong> Now whenever the ROLE_ADMIN wants to role play as another user
The ROLE_ADMIN logs in with his username and password. Then,there should be a link which has the following</p>
<p><strong>http://localhost:8080/j_acegi_switch_user?j_username=vikas</strong></p>
<p>where 'vikas' is the username of the user that the ROLE_ADMIN wants to play as. Since only the ROLE_ADMIN should be allowed to see this link you can use the Acegi taglibs to make the link available only to ROLE_ADMIN</p>
<p>[xml]
<authz:authorize ifAllGranted="ROLE_ADMIN">
    &lt;<switch User Link Here>&gt;
</authz:authorize>
[/xml]</p>
<p>When successful switch happens, the user's SecurityContextHolder will be updated to reflect the specified user and will also contain an additional SwitchUserGrantedAuthority which contains the original user. To 'exit' from a user context, the user will then need to access a URL (see exitUserUrl) that will switch back to the original user as identified by the SWITCH_USER_GRANTED_AUTHORITY</p>
<p>To exit from the Role play there should be a link to say</p>
<p><strong>http://localhost:8080/j_acegi_exit_user</strong></p>
<p>This would bring the ROLE_ADMIN to his own account.</p>
<p>Nested switching and exit is also possible. What this means is that if Jack and Suzy both have ROLE_ADMIN, then Jack can switch to become Suzy who can further switch to become John. Now once the user exits from John realm he comes to Suzy and then on another exit back to Jack.</p>
<p><strong>Conclusion: </strong>Switching users is can be easily configured using Acegi. However remember that with 'Great Power comes Great Responsibility' so use this with care.</p>