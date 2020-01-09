### @SpringBootApplicaiton = @Configuration + @ComponentScan + @EnableAutoConfgiuration 

@Configuration: which is used in Java-based configuration on Spring framework, 
@ComponentScan: to enable component scanning of components you write e.g. @Controller classes, 
@EnableAutoConfgiuration: which is used to enable auto-configuration in Spring Boot application. 


* The Spring Boot auto-configuration feature tries to automatically configure your Spring application based upon the JAR dependency you have added in the classpath.

For example, if HSQLDB is present on your classpath and you have not configured any database manually, Spring will auto-configure an in-memory database for you.

By default, this auto-configuration feature is not enabled and you need to opt-in for it by adding the @EnableAutoConfiguration or @SpringBootApplicaiton annotations to one of your @Configuration classes, generally the Main class which is used to run your application.

* You should annotate the Main class or Bootstrap class with the @SpringBootApplication, this will allow you to run as a JAR with embedded web server Tomcat. If you want you can change that to Jetty or Undertow.

* The @SpringBootApplication is a combination of three annotations @Configuration (used for Java-based configuration), @ComponentScan (used for component scanning), and @EnableAutoConfiguration (used to enable auto-configuration in Spring Boot).

* The @EnableAutoConfiguration annotations enable auto-configuration features of Spring Boot which configures modules based on the presence of certain classes on the classpath. For example, if Thymeleaf JAR is present in classpath and Spring MVC is enabled e.g. using spring-boot-web-starter package then it can automatically configure template resolver and view resolver for you.

* The @EnableAutoConfiguration annotation is based on @Conditional annotation of Spring 4.0 which enables conditional configuration.

* In case of auto-configuration, manually declared beans can override beans automatically created by auto-configuration feature. This is achieved by using @ConditionalOnMissingBean of Spring 4.0

* If you are using @EnableAutoConfiguration classes then you can selectively exclude certain classes from auto-configuration by using exclude as shown below:

      ``` @EnableAutoConfiguration(exclude=DataSourceAutoConfiguration.class) ```

* The @SpringBootApplication annotation also provides aliases to customize the attributes of @EnableAutoConfiguration and @ComponentScan annotations.
