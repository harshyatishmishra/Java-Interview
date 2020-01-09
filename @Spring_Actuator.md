Monitoring our app, gathering metrics, understanding traffic or the state of our database becomes trivial with this dependency.

Actuator brings production-ready features to our application.

Actuator is mainly used to expose operational information about the running application – health, metrics, info, dump, env, etc. It uses HTTP endpoints or JMX beans to enable us to interact with it.

`<artifactId>spring-boot-starter-actuator</artifactId>`

#### Actuator brings its own security model. It takes advantage of Spring Security constructs, but needs to be configured independently from the rest of the application.

Also, most endpoints are sensitive – meaning they're not fully public, or in other words, most information will be omitted – while a handful is not e.g. /info.

Here are some of the most common endpoints Boot provides out of the box:

`/auditevents` – lists security audit-related events such as user login/logout. Also, we can filter by principal or type among others fields
`/beans` – returns all available beans in our BeanFactory. Unlike /auditevents, it doesn't support filtering
`/conditions` – formerly known as /autoconfig, builds a report of conditions around auto-configuration
`/configprops` – allows us to fetch all @ConfigurationProperties beans
`/env` – returns the current environment properties. Additionally, we can retrieve single properties
`/flyway` – provides details about our Flyway database migrations
`/health` – summarises the health status of our application
`/heapdump `– builds and returns a heap dump from the JVM used by our application
`/info `– returns general information. It might be custom data, build information or details about the latest commit
`/liquibase` – behaves like /flyway but for Liquibase
`/logfile` – returns ordinary application logs
`/loggers` – enables us to query and modify the logging level of our application
`/metrics` – details metrics of our application. This might include generic metrics as well as custom ones
`/prometheus` – returns metrics like the previous one, but formatted to work with a Prometheus server
`/scheduledtasks` – provides details about every scheduled task within our application
`/sessions` – lists HTTP sessions given we are using Spring Session
`/shutdown` – performs a graceful shutdown of the application
`/threaddump` – dumps the thread information of the underlying JVM


#### Configuring Existing Endpoints
Three properties are available:

id – by which this endpoint will be accessed over HTTP
enabled – if true then it can be accessed otherwise not
sensitive – if true then need the authorization to show crucial information over HTTP

`
endpoints.beans.id=springbeans
endpoints.beans.sensitive=false
endpoints.beans.enabled=true
`
#### Creating A New Endpoint
Besides using the existing endpoints provided by Spring Boot, we could also create an entirely new one.

`
@Component
public class CustomEndpoint implements Endpoint<List<String>> {
     
    @Override
    public String getId() {
        return "customEndpoint";
    }
 
    @Override
    public boolean isEnabled() {
        return true;
    }
 
    @Override
    public boolean isSensitive() {
        return true;
    }
 
    @Override
    public List<String> invoke() {
        // Custom logic to build the output
        List<String> messages = new ArrayList<String>();
        messages.add("This is message 1");
        messages.add("This is message 2");
        return messages;
    }
}
`
expose all endpoints :
`management.endpoints.web.exposure.include=*`

To explicitly enable a specific endpoint (for example /shutdown), we use:
`management.endpoint.shutdown.enabled=true`

To expose all enabled endpoints except one (for example /loggers), we use:
`management.endpoints.web.exposure.include=*
management.endpoints.web.exposure.exclude=loggers`
