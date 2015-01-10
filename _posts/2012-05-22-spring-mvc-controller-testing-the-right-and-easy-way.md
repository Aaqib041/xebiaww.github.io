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

The other day I was playing with Spring MVC to create some demo app. For writing test cases for the Spring MVC Controllers, I used the normal way described in most of the blogs I searched. But few days later, I came to know about [spring-test-mvc][1] module by springsource guys. I must say its simply awesome. All the boiler place code is gone now and writing test cases is more fun.

For the lazy readers and code addicts here is the link to [GitHub Repo][2].  Now lets get back, below is the Controller class I will be testing in this blog : 

``` 
 package in.xebia.mvc.controller;

//all the imports 

@Controller public class MainController {
    
    
    protected static Logger logger = Logger.getLogger(&quot;MainController&quot;);
    
    @Autowired
    private UserRepository userRepository;
    
    @RequestMapping(value = &quot;/&quot;, method = RequestMethod.GET)
    public String home() {
        return &quot;login&quot;;
    }
    
    @RequestMapping(value = &quot;/login&quot;, method = RequestMethod.POST)
    public String login(@Valid User user, BindingResult result, Model model) {
        if(result.hasErrors()){
            model.addAttribute(&quot;STATUS&quot;, &quot;INVALID LOGIN&quot;);
            return &quot;redirect:/login&quot;;
        }
        if(userRepository.findByUserNameAndPassword(user.getUserName(), user.getPassword())!=null){
            model.addAttribute(&quot;STATUS&quot;, &quot;SUCCESS&quot;);
            return &quot;home&quot;;
        }
        model.addAttribute(&quot;STATUS&quot;, &quot;DOES NOT EXIST&quot;);
        return &quot;redirect:/login&quot;;
    }
    
    @RequestMapping(value = &quot;/users/all&quot;, method = RequestMethod.GET)
    public @ResponseBody List&lt;User&gt; getAllUsers() {
        return userRepository.findAll();
    }
    
    @RequestMapping(value = &quot;/users/{userName}&quot;, method = RequestMethod.GET)
    public @ResponseBody User getUser(@PathVariable String userName) {
        return userRepository.findByUserName(userName);
    }
    
    @RequestMapping(value = &quot;/users&quot;, method = RequestMethod.GET)
    public @ResponseBody User getUserByRequestParameter(@RequestParam String userName) {
        return userRepository.findByUserName(userName);
    }
    

} 
 ```

I have tried to test most of the use cases like, GET, POST with parameters, GET with parameters, **@ResponseBody**, **@RequestParam**, **@Valid** , **@PathVariable**, Model Attributes, View Names, etc. 

Below are the Test Cases I wrote earlier(before I knew about spring-test-mvc): ``` 
 package in.xebia.mvc.controllers.test;

@SuppressWarnings("unused") public class MainControllerTest extends AbstractTest {
    
    
    private static final String SUCCESS = &quot;SUCCESS&quot;;
    
    private static final String INVALID_LOGIN = &quot;INVALID LOGIN&quot;;
    
    private static final String DOES_NOT_EXIST = &quot;DOES NOT EXIST&quot;;
    
    private static final String STATUS = &quot;STATUS&quot;;
    
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
        request.setMethod(&quot;GET&quot;);
        request.setRequestURI(&quot;/&quot;);
        Object handler = handlerMapping.getHandler(request).getHandler();
        ModelAndView modelAndView = handlerAdapter.handle(request, response,
                handler);
        assertViewName(modelAndView, &quot;login&quot;);
    }
    
    @Test
    public void testInvalidLogin() throws Exception {
        request.setMethod(&quot;POST&quot;);
        request.setRequestURI(&quot;/login&quot;);
        request.setParameter(&quot;userName&quot;, &quot;vijayrawat&quot;);
        Object handler = handlerMapping.getHandler(request).getHandler();
        final ModelAndView modelAndView = handlerAdapter.handle(request, response, handler);
    
        assertViewName(modelAndView, &quot;redirect:/login&quot;);
        final BindingResult errors = assertAndReturnModelAttributeOfType(modelAndView, 
                &quot;org.springframework.validation.BindingResult.user&quot;, 
                BindingResult.class);
        System.out.println(errors.getAllErrors());
        assertEquals(1, errors.getErrorCount());
        assertModelAttributeValue(modelAndView, STATUS, INVALID_LOGIN);
    }
    
    @Test
    public void testSuccessfulLogin() throws Exception {
        request.setMethod(&quot;POST&quot;);
        request.setRequestURI(&quot;/login&quot;);
        request.setParameter(&quot;userName&quot;, &quot;vijay&quot;);
        request.setParameter(&quot;password&quot;, &quot;1a1dc91c907325c69271ddf0c944bc72&quot;);
        Object handler = handlerMapping.getHandler(request).getHandler();
        final ModelAndView modelAndView = handlerAdapter.handle(request, response, handler);
        assertViewName(modelAndView, &quot;home&quot;);
        assertModelAttributeValue(modelAndView, STATUS, SUCCESS);
    }
    
    @Test
    public void testDoesNotExistLogin() throws Exception {
        request.setMethod(&quot;POST&quot;);
        request.setRequestURI(&quot;/login&quot;);
        request.setParameter(&quot;userName&quot;, &quot;vijay&quot;);
        request.setParameter(&quot;password&quot;, &quot;some random pass&quot;);
        Object handler = handlerMapping.getHandler(request).getHandler();
        final ModelAndView modelAndView = handlerAdapter.handle(request, response, handler);
        assertViewName(modelAndView, &quot;redirect:/login&quot;);
        assertModelAttributeValue(modelAndView, STATUS, DOES_NOT_EXIST);
    }
    
    
    @Test
    public void testGetUsers() throws Exception {
        request.setMethod(&quot;GET&quot;);
        request.setRequestURI(&quot;/users/vijay&quot;);
        Object handler = handlerMapping.getHandler(request).getHandler();
        HttpMessageConverter&lt;?&gt;[] messageConverters = {new MappingJacksonHttpMessageConverter()};
        handlerAdapter.setMessageConverters(Arrays.asList(messageConverters));
        final ModelAndView modelAndView = handlerAdapter.handle(request, response, handler);
        assertEquals(mapper.writeValueAsString(userRepository.findByUserName(&quot;vijay&quot;)), response.getContentAsString());
    }
    
    @Test
    public void testGetAllUsers() throws Exception {
        request.setMethod(&quot;GET&quot;);
        request.setRequestURI(&quot;/users/all&quot;);
        Object handler = handlerMapping.getHandler(request).getHandler();
        HttpMessageConverter&lt;?&gt;[] messageConverters = {new MappingJacksonHttpMessageConverter()};
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
        request.setMethod(&quot;GET&quot;);
        request.setRequestURI(&quot;/users&quot;);
        request.setParameter(&quot;userName&quot;, &quot;vijay&quot;);
        Object handler = handlerMapping.getHandler(request).getHandler();
        HttpMessageConverter&lt;?&gt;[] messageConverters = {new MappingJacksonHttpMessageConverter()};
        handlerAdapter.setMessageConverters(Arrays.asList(messageConverters));
        final ModelAndView modelAndView = handlerAdapter.handle(request, response, handler);
        System.out.println(response.getContentAsString());
        assertEquals(mapper.writeValueAsString(userRepository.findByUserName(&quot;vijay&quot;)), response.getContentAsString());
    }
    

} 
 ```

Lets compare the above code with the new test cases written below(after learning about spring-test-mvc): ``` 
 package in.xebia.mvc.controllers.test; //important imports @SuppressWarnings("unused") @RunWith(SpringJUnit4ClassRunner.class) 

   [1]: https://github.com/SpringSource/spring-test-mvc
   [2]: https://github.com/vijayrawatsan/spring-mvc-test

## Comments

**[Luciano](#8914 "2012-06-01 00:54:58"):** Very good examples m8, they pretty much cover all the main use cases. Compliments

**[Panka Sharma](#8943 "2012-06-04 08:15:27"):** Thanks for writing ..Very nice article. - javapins.com

**[Jaya](#9445 "2013-09-20 21:52:13"):** Very nice blog. Easy to understand and good to know this type of stuff.

