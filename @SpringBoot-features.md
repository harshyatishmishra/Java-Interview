Spring Boot is a microservice-based framework and making a production-ready application in it takes very less time.
Prerequisite for Spring Boot is the basic knowledge Spring framework.

Spring Boot is built on the top of the conventional spring framework. So, it provides all the features of spring and is yet easier to use than spring.

### It allows to avoid heavy configuration of XML which is present in spring:
Unlike the Spring MVC Project, in spring boot everything is auto-configured. We just need to use proper configuration for utilizing a particular functionality.
For example: If we want to use hibernate(ORM) then we can just add @Table annotation above model/entity class(discussed later) and add @Column annotation to map it to table and columns in the database

### It provides easy maintenance and creation of REST end points:
Creating a REST API is very easy in Spring Boot. Just the annotation @RestController and @RequestMapping(/endPoint) over the controller class does the work.
### It includes embedded Tomcat-server:
Unlike Spring MVC project where we have to manually add and install the tomcat server, Spring Boot comes with an embedded Tomcat server, so that the applications can be hosted on it.

### Deployment is very easy, war and jar file can be easily deployed in the tomcat server:
war or jar files can be directly deployed on the Tomcat Server and Spring Boot provides the facility to convert our project into war or jar files. Also, the instance of Tomcat can be run on the cloud as well.

### Microservice Based Architecture:
Microservice, as the name suggests is the name given to a module/service which focuses on a single type of feature, exposing an API(application peripheral interface).
Let us consider an example of a hospital management system.

In case of monolithic systems, there will be a single code containing all the features which are very tough to maintain on a huge scale.
But in the microservice-based system, each feature can be divided into smaller subsystems like service to handle patient registration, service to handle database management, service to handle billing etc.

Microservice based system can be easily migrated as only some services need to be altered which also makes debugging and deployment easy. Also, each service can be integrated and can be made in different technologies suited to them.
