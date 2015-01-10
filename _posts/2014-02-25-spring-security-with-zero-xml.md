---
layout: post
header-img: img/default-blog-pic.jpg
author: pgupta
description: 
post_id: 17993
created: 2014/02/25 16:24:52
created_gmt: 2014/02/25 11:24:52
comment_status: open
---

# Spring Security with zero XML

<p>Let me first convince you that its great to use java configuration as compared to schema based configuration, just to elaborate we will discuss the advantages and disadvantages of schema based configuration: 
Some of the advantages are listed below: 
<ul><li> Schema based configuration is centralized, it’s not scattered among all different components so you can have a nice overview of beans and their wirings in a single place.</li>
<li> If you need to split your files spring let you do that. It then reassembles them at runtime through internal tags or external context files aggregation</li>
<li>XML is completely orthogonal to the Java file: there’s no coupling between the two so that the class can be used in more than one context with different configurations</li></ul></p>
<p>The only problem with XML is that you have to wait until runtime to discover typos in a bean or some other issues. On the other side, using Spring IDE plugin (or the integrated Spring Tools Suite) definitely can help you there.</p>
<p>An interesting alternative to both XML and direct annotations on bean classes is JavaConfig, a former separate project embedded into Spring itself since v3.0. It merges XML decoupling advantage with Java compile-time checks. JavaConfig can be seen as the XML file equivalent, only written in Java.</p>
<p>You might have explored basic <strong>Spring Configuration</strong> with no xml, but today we will look how to configure <strong>Spring Security</strong> with zero xml.
I am writing this blog assuming you have an idea for Spring basic configuration with no xml.
We need to make the following entry in POM.xml file:</p>
<p>[sourcecode language="xml"]
&lt;dependency&gt;
    &lt;groupId&gt;org.springframework.security&lt;/groupId&gt;
    &lt;artifactId&gt;spring-security-web&lt;/artifactId&gt;
    &lt;version&gt;3.2.0.RELEASE&lt;/version&gt;
&lt;/dependency&gt;
&lt;dependency&gt;
    &lt;groupId&gt;org.springframework.security&lt;/groupId&gt;
    &lt;artifactId&gt;spring-security-config&lt;/artifactId&gt;
    &lt;version&gt;3.2.0.RELEASE&lt;/version&gt;
&lt;/dependency&gt;
&lt;dependency&gt;
    &lt;groupId&gt;org.springframework.security&lt;/groupId&gt;
    &lt;artifactId&gt;spring-security-taglibs&lt;/artifactId&gt;
    &lt;version&gt;3.2.0.RELEASE&lt;/version&gt;
&lt;/dependency&gt;
[/sourcecode]
<strong>spring-security-taglibs</strong> is used to use taglib in jsp files for gathering security related information.</p>
<p>Next step would be Creating your Spring Security configuration, for this you need to create a class for configuring this:
[sourcecode language="java"]
@Configuration
@EnableWebMvcSecurity
@ComponentScan(&quot;com.priyank.sample&quot;)
public class MultiHttpSecurityConfig extends WebSecurityConfigurerAdapter {</p>
<pre><code>@Autowired
private DataSource dataSource;

@Autowired
private LocalUserDetailService userDetailService;

@Autowired
private PasswordEncoder passwordEncoder;

@Override
protected void configure(AuthenticationManagerBuilder auth)
        throws Exception {
    auth.userDetailsService(userDetailService)
            .passwordEncoder(passwordEncoder).and()
            .inMemoryAuthentication().withUser(&amp;quot;admin&amp;quot;).password(&amp;quot;admin&amp;quot;)
            .roles(&amp;quot;ADMIN&amp;quot;, &amp;quot;USER&amp;quot;);
}

@Override
protected void configure(HttpSecurity http) throws Exception {
    http.authorizeRequests()
            .antMatchers(&amp;quot;/resources/**&amp;quot;, &amp;quot;/signup&amp;quot;, &amp;quot;/logout&amp;quot;, &amp;quot;/login&amp;quot;)
            .permitAll().antMatchers(&amp;quot;/admin/**&amp;quot;).hasRole(&amp;quot;ADMIN&amp;quot;)
            .anyRequest().authenticated().and().formLogin()
            .loginPage(&amp;quot;/login&amp;quot;).permitAll().and().logout()
            .logoutSuccessUrl(&amp;quot;/login?logout=success&amp;quot;).logoutUrl(&amp;quot;/logout&amp;quot;);
}

@Bean
public BCryptPasswordEncoder getPasswordEncoder() {
    return new BCryptPasswordEncoder();
}

@Bean
public AuthenticationManager getAuthenticationManager() throws Exception {
    return super.authenticationManager();
}
</code></pre>
<p>}</p>
<p>[/sourcecode]</p>
<ul>
<li>The annotation <strong>@EnableWebMvcSecurity</strong> is added this to an @Configuration class to have the Spring Security configuration integrate with Spring MVC.</li>
<li>We are using authentication by two means, first as database authentication and secondly in-memory authentication.</li>
<li>The above configuration is done in <strong>configure</strong> method with <strong>AuthenticationManagerBuilder</strong> as parameter, we register our userDetailService and in-memory authentication with <strong>AuthenticationManagerBuilder</strong>.</li>
<li>I have used <strong>BCryptPasswordEncoder</strong> just to show its functionality that how to integrate it with our authentication system.</li>
<li>The <strong>configure</strong> method with <strong>HttpSecurity</strong> as a parameter is used to configure the authorization of request, same as <strong></strong> tag used in security-context xml file.</li></ul>

<p>Next step would be registering Spring Security with the war, to do this we need to create a Class that will extend <strong>AbstractSecurityWebApplicationInitializer</strong> and register our configuration file in the constructor by passing it to the super class constructor as :
[sourcecode language="java"]
public class SecurityWebApplicationInitializer extends
        AbstractSecurityWebApplicationInitializer {
    public SecurityWebApplicationInitializer() {
        super(MultiHttpSecurityConfig.class);
    }
}
[/sourcecode]</p>
<p>This class will serve two purpose 
<ul><li>Automatically register the springSecurityFilterChain Filter for every URL in your application</li>
<li>Add a ContextLoaderListener that loads the SecurityConfig.</li></ul></p>
<p>We are almost done with the configuration part, the only thing left is to configure <strong>UserDetailsService</strong>, we can have a look on the implementation below:
[sourcecode language="java"]
@Component
public class LocalUserDetailService implements UserDetailsService {</p>
<pre><code>@Autowired
private EmployeeService employeeService;

@Override
public CustomUserBean loadUserByUsername(String userName)
        throws UsernameNotFoundException {
    Employee employee = employeeService.getByEmail(userName);
    if (employee == null) {
        throw new UsernameNotFoundException(&amp;quot;User with user name :&amp;quot;
                + userName + &amp;quot; not found&amp;quot;);
    }
    Collection&amp;lt;SimpleGrantedAuthority&amp;gt; authorities = new ArrayList&amp;lt;SimpleGrantedAuthority&amp;gt;();
    for (UserRoles roles : employee.getUserRoleses()) {
        authorities.add(new SimpleGrantedAuthority(roles.getRoleName()));
    }
    CustomUserBean user = new CustomUserBean(userName,
            employee.getPassword(), true, true, true, true, authorities,
            employee);
    return user;
}
</code></pre>
<p>}
[/sourcecode]
The above class override the <strong>loadUserByUsername</strong> method and returns the user object after manipulation from the database persisted user.</p>
<p>You can download the source code <a href="http://xebee.xebia.in/wp-content/uploads/2014/02/JavaConfiguration1.zip">JavaConfiguration.zip</a> here.</p>