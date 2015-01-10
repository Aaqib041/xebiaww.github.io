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

Let me first convince you that its great to use java configuration as compared to schema based configuration, just to elaborate we will discuss the advantages and disadvantages of schema based configuration: Some of the advantages are listed below: 

  * Schema based configuration is centralized, it’s not scattered among all different components so you can have a nice overview of beans and their wirings in a single place.
  * If you need to split your files spring let you do that. It then reassembles them at runtime through internal tags or external context files aggregation
  * XML is completely orthogonal to the Java file: there’s no coupling between the two so that the class can be used in more than one context with different configurations

The only problem with XML is that you have to wait until runtime to discover typos in a bean or some other issues. On the other side, using Spring IDE plugin (or the integrated Spring Tools Suite) definitely can help you there.

An interesting alternative to both XML and direct annotations on bean classes is JavaConfig, a former separate project embedded into Spring itself since v3.0. It merges XML decoupling advantage with Java compile-time checks. JavaConfig can be seen as the XML file equivalent, only written in Java.

You might have explored basic **Spring Configuration** with no xml, but today we will look how to configure **Spring Security** with zero xml. I am writing this blog assuming you have an idea for Spring basic configuration with no xml. We need to make the following entry in POM.xml file:

[sourcecode language="xml"] <dependency> <groupId>org.springframework.security</groupId> <artifactId>spring-security-web</artifactId> <version>3.2.0.RELEASE</version> </dependency> <dependency> <groupId>org.springframework.security</groupId> <artifactId>spring-security-config</artifactId> <version>3.2.0.RELEASE</version> </dependency> <dependency> <groupId>org.springframework.security</groupId> <artifactId>spring-security-taglibs</artifactId> <version>3.2.0.RELEASE</version> </dependency> [/sourcecode] **spring-security-taglibs** is used to use taglib in jsp files for gathering security related information.

Next step would be Creating your Spring Security configuration, for this you need to create a class for configuring this: [sourcecode language="java"] @Configuration @EnableWebMvcSecurity @ComponentScan("com.priyank.sample") public class MultiHttpSecurityConfig extends WebSecurityConfigurerAdapter {
    
    
    @Autowired
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
                .inMemoryAuthentication().withUser(&quot;admin&quot;).password(&quot;admin&quot;)
                .roles(&quot;ADMIN&quot;, &quot;USER&quot;);
    }
    
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http.authorizeRequests()
                .antMatchers(&quot;/resources/**&quot;, &quot;/signup&quot;, &quot;/logout&quot;, &quot;/login&quot;)
                .permitAll().antMatchers(&quot;/admin/**&quot;).hasRole(&quot;ADMIN&quot;)
                .anyRequest().authenticated().and().formLogin()
                .loginPage(&quot;/login&quot;).permitAll().and().logout()
                .logoutSuccessUrl(&quot;/login?logout=success&quot;).logoutUrl(&quot;/logout&quot;);
    }
    
    @Bean
    public BCryptPasswordEncoder getPasswordEncoder() {
        return new BCryptPasswordEncoder();
    }
    
    @Bean
    public AuthenticationManager getAuthenticationManager() throws Exception {
        return super.authenticationManager();
    }
    

}

[/sourcecode]

  * The annotation **@EnableWebMvcSecurity** is added this to an @Configuration class to have the Spring Security configuration integrate with Spring MVC.
  * We are using authentication by two means, first as database authentication and secondly in-memory authentication.
  * The above configuration is done in **configure** method with **AuthenticationManagerBuilder** as parameter, we register our userDetailService and in-memory authentication with **AuthenticationManagerBuilder**.
  * I have used **BCryptPasswordEncoder** just to show its functionality that how to integrate it with our authentication system.
  * The **configure** method with **HttpSecurity** as a parameter is used to configure the authorization of request, same as **** tag used in security-context xml file.

Next step would be registering Spring Security with the war, to do this we need to create a Class that will extend **AbstractSecurityWebApplicationInitializer** and register our configuration file in the constructor by passing it to the super class constructor as : [sourcecode language="java"] public class SecurityWebApplicationInitializer extends AbstractSecurityWebApplicationInitializer { public SecurityWebApplicationInitializer() { super(MultiHttpSecurityConfig.class); } } [/sourcecode]

This class will serve two purpose 

  * Automatically register the springSecurityFilterChain Filter for every URL in your application
  * Add a ContextLoaderListener that loads the SecurityConfig.

We are almost done with the configuration part, the only thing left is to configure **UserDetailsService**, we can have a look on the implementation below: [sourcecode language="java"] @Component public class LocalUserDetailService implements UserDetailsService {
    
    
    @Autowired
    private EmployeeService employeeService;
    
    @Override
    public CustomUserBean loadUserByUsername(String userName)
            throws UsernameNotFoundException {
        Employee employee = employeeService.getByEmail(userName);
        if (employee == null) {
            throw new UsernameNotFoundException(&quot;User with user name :&quot;
                    + userName + &quot; not found&quot;);
        }
        Collection&lt;SimpleGrantedAuthority&gt; authorities = new ArrayList&lt;SimpleGrantedAuthority&gt;();
        for (UserRoles roles : employee.getUserRoleses()) {
            authorities.add(new SimpleGrantedAuthority(roles.getRoleName()));
        }
        CustomUserBean user = new CustomUserBean(userName,
                employee.getPassword(), true, true, true, true, authorities,
                employee);
        return user;
    }
    

} [/sourcecode] The above class override the **loadUserByUsername** method and returns the user object after manipulation from the database persisted user.

You can download the source code [JavaConfiguration.zip][1] here.

   [1]: http://xebee.xebia.in/wp-content/uploads/2014/02/JavaConfiguration1.zip