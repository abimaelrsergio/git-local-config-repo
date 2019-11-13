#git-local-config-repo


##Debugging problems with Spring Cloud Config Server

First of all check if you have any typos in the URL. Does it match exactly what is given below?

(1) Does the URL http://localhost:8888/limits-service/default work? If the URL does not work, check if you have the same name for limits-service in (a) spring.application.name in bootstrap.properties (b) in the URL (c) in the name of the property file

(2) Check if the name in @ConfigurationProperties("limits-service") matches the prefix of property values in application.properties. limits-service.minimum=9 limits-service.maximum=999

(3) Check if you have @EnableConfigServer enabled on SpringCloudConfigServerApplication class

(4) Check if you have the right repository url in /spring-cloud-config-server/src/main/resources/application.properties - spring.cloud.config.server.git.uri=file:///in28Minutes/git/spring-micro-services/03.microservices/git-localconfig-repo

(5) Do not have any spaces in your git repository path.

(6) If you are on windows, make sure that you are using one of these formats for your git repository

 file:\\C:/WORKSPACE/GIT/git-localconfig-repo
 file:///C:/microservices/git-localconfig-repo
file:///C:/Users/Gautham/Documents/workspace-sts-3.9.4.RELEASE/git-localconfig-repo
file:\\C:/Users/Gautham/Documents/workspace-sts-3.9.4.RELEASE/git-localconfig-repo
(7) Make sure that you have the right code - Compare against the code below.

(8) Make sure that you have committed all the code to GIT Local Repo

If everything is fine

(1) Stop all the servers

(2) Launch Config Server First

(3) Launch Limits Service

(4) Wait for 2 minutes

If you still have a problem, post a question including all the details:

(1) Response for http://localhost:8080/limits

(2) Response for http://localhost:8888/limits-service/default

(3) Response for http://localhost:8888/limits-service/dev

(4) Start up logs for limits-service and spring cloud config server with debug mode enabled

(5) All code for files included below.

/limits-service/src/main/java/com/in28minutes/microservices/limitsservice/Configuration.java New

package com.in28minutes.microservices.limitsservice;

import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.stereotype.Component;

@Component
@ConfigurationProperties("limits-service")
public class Configuration {
  
  private int minimum;
  private int maximum;
/limits-service/src/main/java/com/in28minutes/microservices/limitsservice/LimitsConfigurationController.java New

@RestController
public class LimitsConfigurationController {

  @Autowired
  private Configuration configuration;

  @GetMapping("/limits")
  public LimitConfiguration retrieveLimitsFromConfigurations() {
    return new LimitConfiguration(configuration.getMaximum(), 
        configuration.getMinimum());
  }

}
/limits-service/src/main/java/com/in28minutes/microservices/limitsservice/bean/LimitConfiguration.java New

package com.in28minutes.microservices.limitsservice.bean;

public class LimitConfiguration {
  private int maximum;
  private int minimum;
/limits-service/src/main/resources/application.properties Modified New Lines

spring.application.name=limits-service
limits-service.minimum=9
limits-service.maximum=999
/git-localconfig-repo/limits-service-dev.properties New

limits-service.minimum=1
limits-service.maximum=111
/git-localconfig-repo/limits-service.properties New

limits-service.minimum=8
limits-service.maximum=888
/spring-cloud-config-server/src/main/java/com/in28minutes/microservices/springcloudconfigserver/SpringCloudConfigServerApplication.java Modified

@EnableConfigServer
@SpringBootApplication
public class SpringCloudConfigServerApplication {
/spring-cloud-config-server/src/main/resources/application.properties New

spring.application.name=spring-cloud-config-server
server.port=8888
spring.cloud.config.server.git.uri=file:///in28Minutes/git/spring-micro-services/03.microservices/git-localconfig-repo
/limits-service/src/main/resources/application.properties Deleted

/limits-service/src/main/resources/bootstrap.properties New

spring.application.name=limits-service
spring.cloud.config.uri=http://localhost:8888

## Faça download do Zipking:

# https://zipkin.io/pages/quickstart
  
   a) curl -sSL https://zipkin.io/quickstart.sh | bash -s
   b) Copie o jar para o seu repositório para ter a versao usada.
   c) java -jar zipingk.jar
   d) entre no navegador: http://localhost:9411/zipkin/

