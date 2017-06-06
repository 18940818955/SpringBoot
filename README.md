# 28. Security

如果Spring Security已经加入到classpath中，那么应用的所有http端点都会被'basic认证\(用户名密码方式\)'管理。如果想仅仅添加方法级的安全管理，可以在Security的Configuration类上加上`@EnableGlobalMethodSecurity` 注解。更多信息请点击 [Spring Security Reference](http://docs.spring.io/spring-security/site/docs/4.2.2.RELEASE/reference/htmlsingle#jc-method).

默认的`AuthenticationManager` 已经有了一个用户\(用户名是‘user’ ，密码是一个随即字符串，在应用启动时，密码会以INFO级别的LOG形式打印再控制台中\)

```
Using default security password: 78fa095d-3f4c-48b1-ad50-e24c31d5cf35
```

| ![](http://docs.spring.io/spring-boot/docs/1.5.3.RELEASE/reference/htmlsingle/images/note.png.pagespeed.ce.9zQ_1wVwzR.png "\[Note\]") |
| :--- |
| 如果恁需要调整日志级别，请记住默认密码会在INFO级别时打印。参数：`org.springframework.boot.autoconfigure.security` |

你可以配置 `security.user.password` 参数，这样就可以自定义密码。它和其它有用的属性也都可以通过[`SecurityProperties`](https://github.com/spring-projects/spring-boot/tree/v1.5.3.RELEASE/spring-boot-autoconfigure/src/main/java/org/springframework/boot/autoconfigure/security/SecurityProperties.java)\(前缀 "security"\) 来实现外部化。

The default security configuration is implemented in`SecurityAutoConfiguration`and in the classes imported from there \(`SpringBootWebSecurityConfiguration`for web security and`AuthenticationManagerConfiguration`for authentication configuration which is also relevant in non-web applications\). To switch off the default web application security configuration completely you can add a bean with`@EnableWebSecurity`\(this does not disable the authentication manager configuration or Actuator’s security\). To customize it you normally use external properties and beans of type`WebSecurityConfigurerAdapter`\(e.g. to add form-based login\). To also switch off the authentication manager configuration you can add a bean of type`AuthenticationManager`, or else configure the global`AuthenticationManager`by autowiring an`AuthenticationManagerBuilder`into a method in one of your`@Configuration`classes. There are several secure applications in the[Spring Boot samples](https://github.com/spring-projects/spring-boot/tree/v1.5.3.RELEASE/spring-boot-samples/)to get you started with common use cases.

The basic features you get out of the box in a web application are:

* An
  `AuthenticationManager`
  bean with in-memory store and a single user \(see
  `SecurityProperties.User`
  for the properties of the user\).
* Ignored \(insecure\) paths for common static resource locations \(
  `/css/**`
  ,
  `/js/**`
  ,
  `/images/**`
  ,
  `/webjars/**`
  and
  `**/favicon.ico`
  \).
* HTTP Basic security for all other endpoints.
* Security events published to Spring’s
  `ApplicationEventPublisher`
  \(successful and unsuccessful authentication and access denied\).
* Common low-level features \(HSTS, XSS, CSRF, caching\) provided by Spring Security are on by default.

All of the above can be switched on and off or modified using external properties \(`security.*`\). To override the access rules without changing any other auto-configured features add a`@Bean`of type`WebSecurityConfigurerAdapter`with`@Order(SecurityProperties.ACCESS_OVERRIDE_ORDER)`and configure it to meet your needs.

| ![](http://docs.spring.io/spring-boot/docs/1.5.3.RELEASE/reference/htmlsingle/images/note.png.pagespeed.ce.9zQ_1wVwzR.png "\[Note\]") |
| :--- |
| By default, a`WebSecurityConfigurerAdapter`will match any path. If you don’t want to completely override Spring Boot’s auto-configured access rules, your adapter must explicitly configure the paths that you do want to override. |



