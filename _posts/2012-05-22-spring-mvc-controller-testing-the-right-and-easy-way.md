---
layout: post
header-img: img/default-blog-pic.jpg
author: vrawat
description: 
post_id: 13985
created: 2012/05/22 12:43:26
created_gmt: 2012/05/22 07:43:26
comment_status: open
---

# Spring MVC Controller Testing : The right and easy way...

<p>The other day I was playing with Spring MVC to create some demo app. For writing test cases for the Spring MVC Controllers, I used the  normal way described in most of the blogs I searched. But few days later, I came to know about <a href="https://github.com/SpringSource/spring-test-mvc" target="_blank">spring-test-mvc</a> module by springsource guys. I must say its simply awesome. All the boiler place code is gone now and writing test cases is more fun.</p>
<p>For the lazy readers and code addicts here is the link to <a href="https://github.com/vijayrawatsan/spring-mvc-test" target="_blank">GitHub Repo</a>. 
<!--more-->
Now lets get back, below is the Controller class I will be testing in this blog : </p>
<p>[code language="java"]
package in.xebia.mvc.controller;</p>
<p>//all the imports </p>
<p>@Controller
public class MainController {</p>
<pre><code>protected static Logger logger = Logger.getLogger(&amp;quot;MainController&amp;quot;);

@Autowired
private UserRepository userRepository;

@RequestMapping(value = &amp;quot;/&amp;quot;, method = RequestMethod.GET)
public String home() {
    return &amp;quot;login&amp;quot;;
}

@RequestMapping(value = &amp;quot;/login&amp;quot;, method = RequestMethod.POST)
public String login(@Valid User user, BindingResult result, Model model) {
    if(result.hasErrors()){
        model.addAttribute(&amp;quot;STATUS&amp;quot;, &amp;quot;INVALID LOGIN&amp;quot;);
        return &amp;quot;redirect:/login&amp;quot;;
    }
    if(userRepository.findByUserNameAndPassword(user.getUserName(), user.getPassword())!=null){
        model.addAttribute(&amp;quot;STATUS&amp;quot;, &amp;quot;SUCCESS&amp;quot;);
        return &amp;quot;home&amp;quot;;
    }
    model.addAttribute(&amp;quot;STATUS&amp;quot;, &amp;quot;DOES NOT EXIST&amp;quot;);
    return &amp;quot;redirect:/login&amp;quot;;
}

@RequestMapping(value = &amp;quot;/users/all&amp;quot;, method = RequestMethod.GET)
public @ResponseBody List&amp;lt;User&amp;gt; getAllUsers() {
    return userRepository.findAll();
}

@RequestMapping(value = &amp;quot;/users/{userName}&amp;quot;, method = RequestMethod.GET)
public @ResponseBody User getUser(@PathVariable String userName) {
    return userRepository.findByUserName(userName);
}

@RequestMapping(value = &amp;quot;/users&amp;quot;, method = RequestMethod.GET)
public @ResponseBody User getUserByRequestParameter(@RequestParam String userName) {
    return userRepository.findByUserName(userName);
}
</code></pre>
<p>}
[/code]</p>
<p>I have tried to test most of the use cases like, GET, POST with parameters, GET with parameters, <strong>@ResponseBody</strong>, <strong>@RequestParam</strong>, <strong>@Valid</strong> , <strong>@PathVariable</strong>, Model Attributes, View Names, etc. </p>
<p>Below are the Test Cases I wrote earlier(before I knew about spring-test-mvc):
[code language="java"]
package in.xebia.mvc.controllers.test;</p>
<p>@SuppressWarnings(&quot;unused&quot;)
public class MainControllerTest extends AbstractTest {</p>
<pre><code>private static final String SUCCESS = &amp;quot;SUCCESS&amp;quot;;

private static final String INVALID_LOGIN = &amp;quot;INVALID LOGIN&amp;quot;;

private static final String DOES_NOT_EXIST = &amp;quot;DOES NOT EXIST&amp;quot;;

private static final String STATUS = &amp;quot;STATUS&amp;quot;;

/**
 * json mapper
 */
ObjectMapper mapper = new ObjectMapper();

@Autowired
private UserRepository userRepository;

@Autowired
private RequestMappingHandlerAdapter handlerAdapter;

@Autowired
private RequestMappingHandlerMapping handlerMapping;

private MockHttpServletRequest request;
private MockHttpServletResponse response;

@Before
public void setUp() {
    request = new MockHttpServletRequest();
    response = new MockHttpServletResponse();
}

@Test
public void testHome() throws Exception {
    request.setMethod(&amp;quot;GET&amp;quot;);
    request.setRequestURI(&amp;quot;/&amp;quot;);
    Object handler = handlerMapping.getHandler(request).getHandler();
    ModelAndView modelAndView = handlerAdapter.handle(request, response,
            handler);
    assertViewName(modelAndView, &amp;quot;login&amp;quot;);
}

@Test
public void testInvalidLogin() throws Exception {
    request.setMethod(&amp;quot;POST&amp;quot;);
    request.setRequestURI(&amp;quot;/login&amp;quot;);
    request.setParameter(&amp;quot;userName&amp;quot;, &amp;quot;vijayrawat&amp;quot;);
    Object handler = handlerMapping.getHandler(request).getHandler();
    final ModelAndView modelAndView = handlerAdapter.handle(request, response, handler);

    assertViewName(modelAndView, &amp;quot;redirect:/login&amp;quot;);
    final BindingResult errors = assertAndReturnModelAttributeOfType(modelAndView, 
            &amp;quot;org.springframework.validation.BindingResult.user&amp;quot;, 
            BindingResult.class);
    System.out.println(errors.getAllErrors());
    assertEquals(1, errors.getErrorCount());
    assertModelAttributeValue(modelAndView, STATUS, INVALID_LOGIN);
}

@Test
public void testSuccessfulLogin() throws Exception {
    request.setMethod(&amp;quot;POST&amp;quot;);
    request.setRequestURI(&amp;quot;/login&amp;quot;);
    request.setParameter(&amp;quot;userName&amp;quot;, &amp;quot;vijay&amp;quot;);
    request.setParameter(&amp;quot;password&amp;quot;, &amp;quot;1a1dc91c907325c69271ddf0c944bc72&amp;quot;);
    Object handler = handlerMapping.getHandler(request).getHandler();
    final ModelAndView modelAndView = handlerAdapter.handle(request, response, handler);
    assertViewName(modelAndView, &amp;quot;home&amp;quot;);
    assertModelAttributeValue(modelAndView, STATUS, SUCCESS);
}

@Test
public void testDoesNotExistLogin() throws Exception {
    request.setMethod(&amp;quot;POST&amp;quot;);
    request.setRequestURI(&amp;quot;/login&amp;quot;);
    request.setParameter(&amp;quot;userName&amp;quot;, &amp;quot;vijay&amp;quot;);
    request.setParameter(&amp;quot;password&amp;quot;, &amp;quot;some random pass&amp;quot;);
    Object handler = handlerMapping.getHandler(request).getHandler();
    final ModelAndView modelAndView = handlerAdapter.handle(request, response, handler);
    assertViewName(modelAndView, &amp;quot;redirect:/login&amp;quot;);
    assertModelAttributeValue(modelAndView, STATUS, DOES_NOT_EXIST);
}


@Test
public void testGetUsers() throws Exception {
    request.setMethod(&amp;quot;GET&amp;quot;);
    request.setRequestURI(&amp;quot;/users/vijay&amp;quot;);
    Object handler = handlerMapping.getHandler(request).getHandler();
    HttpMessageConverter&amp;lt;?&amp;gt;[] messageConverters = {new MappingJacksonHttpMessageConverter()};
    handlerAdapter.setMessageConverters(Arrays.asList(messageConverters));
    final ModelAndView modelAndView = handlerAdapter.handle(request, response, handler);
    assertEquals(mapper.writeValueAsString(userRepository.findByUserName(&amp;quot;vijay&amp;quot;)), response.getContentAsString());
}

@Test
public void testGetAllUsers() throws Exception {
    request.setMethod(&amp;quot;GET&amp;quot;);
    request.setRequestURI(&amp;quot;/users/all&amp;quot;);
    Object handler = handlerMapping.getHandler(request).getHandler();
    HttpMessageConverter&amp;lt;?&amp;gt;[] messageConverters = {new MappingJacksonHttpMessageConverter()};
    handlerAdapter.setMessageConverters(Arrays.asList(messageConverters));
    final ModelAndView modelAndView = handlerAdapter.handle(request, response, handler);
    assertEquals(mapper.writeValueAsString(userRepository.findAll()), response.getContentAsString());
}

/**
 * Test @RequestParam
 * @throws Exception
 */
@Test
public void testgetUsers() throws Exception {
    request.setMethod(&amp;quot;GET&amp;quot;);
    request.setRequestURI(&amp;quot;/users&amp;quot;);
    request.setParameter(&amp;quot;userName&amp;quot;, &amp;quot;vijay&amp;quot;);
    Object handler = handlerMapping.getHandler(request).getHandler();
    HttpMessageConverter&amp;lt;?&amp;gt;[] messageConverters = {new MappingJacksonHttpMessageConverter()};
    handlerAdapter.setMessageConverters(Arrays.asList(messageConverters));
    final ModelAndView modelAndView = handlerAdapter.handle(request, response, handler);
    System.out.println(response.getContentAsString());
    assertEquals(mapper.writeValueAsString(userRepository.findByUserName(&amp;quot;vijay&amp;quot;)), response.getContentAsString());
}
</code></pre>
<p>}
[/code]</p>
<p>Lets compare the above code with the new test cases written below(after learning about spring-test-mvc):
[code language="java"]
package in.xebia.mvc.controllers.test;
//important imports
@SuppressWarnings(&quot;unused&quot;)
@RunWith(SpringJUnit4ClassRunner.class)  </p>

## Comments

**[Luciano](#8914 "2012-06-01 00:54:58"):** Very good examples m8, they pretty much cover all the main use cases. Compliments

**[Panka Sharma](#8943 "2012-06-04 08:15:27"):** Thanks for writing ..Very nice article. - javapins.com

**[Jaya](#9445 "2013-09-20 21:52:13"):** Very nice blog. Easy to understand and good to know this type of stuff.

