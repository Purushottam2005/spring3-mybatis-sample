# spring 3.2新機能と強化ポイント
- New Features and Enhancements in Spring Framework 3.2  

本セクションはwhat's new をカバーしています。移行手順部分もあわせて見てください。

This section covers what's new in Spring Framework 3.2. See also Appendix C, Migrating to Spring Framework 3.2

# Servlet3ベースの非同期リクエスト処理
- Support for Servlet 3 based asynchronous request processing

spring mvcで servlet3ベースの非同期処理をサポートしました。@RequestMappingなどがあります。  
The Spring MVC programming model now provides explicit Servlet 3 async support. @RequestMapping methods can return one of:  
  
* spring mvcはjava.util.concurrent.Callable
*java.util.concurrent.Callable to complete processing in a separate thread managed by a task executor within Spring MVC.
*org.springframework.web.context.request.async.DeferredResult to complete processing at a later time from a thread not known to Spring MVC — for example, in response to some external event (JMS, AMQP, etc.)
*org.springframework.web.context.request.async.AsyncTask to wrap a Callable and customize the timeout value or the task executor to use.
See Section 17.3.4, “Asynchronous Request Processing”.

4.2 Spring MVC Test framework
First-class support for testing Spring MVC applications with a fluent API and without a Servlet container. Server-side tests involve use of the DispatcherServlet while client-side REST tests rely on the RestTemplate. See Section 11.3.6, “Spring MVC Test Framework”.

4.3 Content negotiation improvements
A ContentNeogtiationStrategy is now available for resolving the requested media types from an incoming request. The available implementations are based on the file extension, query parameter, the 'Accept' header, or a fixed content type. Equivalent options were previously available only in the ContentNegotiatingViewResolver but are now available throughout.

ContentNegotiationManager is the central class to use when configuring content negotiation options. For more details see Section 17.15.4, “Configuring Content Negotiation”.

The introduction of ContentNegotiationManger also enables selective suffix pattern matching for incoming requests. For more details, see the Javadoc of RequestMappingHandlerMapping.setUseRegisteredSuffixPatternMatch.

4.4 @ControllerAdvice annotation
Classes annotated with @ControllerAdvice can contain @ExceptionHandler, @InitBinder, and @ModelAttribute methods and those will apply to @RequestMapping methods across controller hierarchies as opposed to the controller hierarchy within which they are declared. @ControllerAdvice is a component annotation allowing implementation classes to be auto-detected through classpath scanning.

4.5 Matrix variables
A new @MatrixVariable annotation adds support for extracting matrix variables from the request URI. For more details see the section called “Matrix Variables”.

4.6 Abstract base class for code-based Servlet 3+ container initialization
An abstract base class implementation of the WebApplicationInitializer interface is provided to simplify code-based registration of a DispatcherServlet and filters mapped to it. The new class is named AbstractDispatcherServletInitializer and its sub-class AbstractAnnotationConfigDispatcherServletInitializer can be used with Java-based Spring configuration. For more details see Section 17.14, “Code-based Servlet container initialization”.

4.7 ResponseEntityExceptionHandler class
A convenient base class with an @ExceptionHandler method that handles standard Spring MVC exceptions and returns a ResponseEntity that allowing customizing and writing the response with HTTP message converters. This servers as an alternative to the DefaultHandlerExceptionResolver, which does the same but returns a ModelAndView instead.

See the revised Section 17.11, “Handling exceptions” including information on customizing the default Servlet container error page.

4.8 Support for generic types in the RestTemplate and in @RequestBody arguments
The RestTemplate can now read an HTTP response to a generic type (e.g. List<Account>). There are three new exchange() methods that accept ParameterizedTypeReference, a new class that enables capturing and passing generic type info.

In support of this feature, the HttpMessageConverter is extended by GenericHttpMessageConverter adding a method for reading content given a specified parameterized type. The new interface is implemented by the MappingJacksonHttpMessageConverter and also by a new Jaxb2CollectionHttpMessageConverter that can read read a generic Collection where the generic type is a JAXB type annotated with @XmlRootElement or @XmlType.

4.9 Jackson JSON 2 and related improvements
The Jackson JSON 2 library is now supported. Due to packaging changes in the Jackson library, there are separate classes in Spring MVC as well. Those are MappingJackson2HttpMessageConverter and MappingJackson2JsonView. Other related configuration improvements include support for pretty printing as well as a JacksonObjectMapperFactoryBean for convenient customization of an ObjectMapper in XML configuration.

4.10 Tiles 3
Tiles 3 is now supported in addition to Tiles 2.x. Configuring it should be very similar to the Tiles 2 configuration, i.e. the combination of TilesConfigurer, TilesViewResolver and TilesView except using the tiles3 instead of the tiles2 package.

Also note that besides the version number change, the tiles dependencies have also changed. You will need to have a subset or all of tiles-request-api, tiles-api, tiles-core, tiles-servlet, tiles-jsp, tiles-el.

4.11 @RequestBody improvements
An @RequestBody or an @RequestPart argument can now be followed by an Errors argument making it possible to handle validation errors (as a result of an @Valid annotation) locally within the @RequestMapping method. @RequestBody now also supports a required flag.

4.12 HTTP PATCH method
The HTTP request method PATCH may now be used in @RequestMapping methods as well as in the RestTemplate in conjunction with Apache HttpComponents HttpClient version 4.2 or later. The JDK HttpURLConnection does not support the PATCH method.

4.13 Excluded patterns in mapped interceptors
Mapped interceptors now support URL patterns to be excluded. The MVC namespace and the MVC JavaConfig both expose these options.

4.14 Using meta-annotations for injection points and for bean definition methods
As of 3.2, Spring allows for @Autowired and @Value to be used as meta-annotations, e.g. to build custom injection annotations in combination with specific qualifiers. Analogously, you may build custom @Bean definition annotations for @Configuration classes, e.g. in combination with specific qualifiers, @Lazy, @Primary, etc.

4.15 Initial support for JCache 0.5
Spring provides a CacheManager adapter for JCache, building against the JCache 0.5 preview release. Full JCache support is coming next year, along with Java EE 7 final.

4.16 Support for @DateTimeFormat without Joda Time
The @DateTimeFormat annotation can now be used without needing a dependency on the Joda Time library. If Joda Time is not present the JDK SimpleDateFormat will be used to parse and print date patterns. When Joda Time is present it will continue to be used in preference to SimpleDateFormat.

4.17 Global date & time formatting
It is now possible to define global formats that will be used when parsing and printing date and time types. See Section 7.7, “Configuring a global date & time format” for details.

4.18 New Testing Features
In addition to the aforementioned inclusion of the Spring MVC Test Framework in the spring-test module, the Spring TestContext Framework has been revised with support for integration testing web applications as well as configuring application contexts with context initializers. For further details, consult the following.

Configuring and loading a WebApplicationContext in integration tests
Testing request and session scoped beans
Improvements to Servlet API mocks
Configuring test application contexts with ApplicationContextInitializers
4.19 Concurrency refinements across the framework
Spring Framework 3.2 includes fine-tuning of concurrent data structures in many parts of the framework, minimizing locks and generally improving the arrangements for highly concurrent creation of scoped/prototype beans.

4.20 New Gradle-based build and move to GitHub
Building and contributing to the framework has never been simpler with our move to a Gradle-based build system and source control at GitHub. See the building from source section of the README and the contributor guidelines for complete details.

4.21 Refined Java SE 7 / OpenJDK 7 support
Last but not least, Spring Framework 3.2 comes with refined Java 7 support within the framework as well as through upgraded third-party dependencies: specifically, CGLIB 3.0, ASM 4.0 (both of which come as inlined dependencies with Spring now) and AspectJ 1.7 support (next to the existing AspectJ 1.6 support).