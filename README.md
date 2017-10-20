# Swagger to MSF4J Code Generator
WSO2 Microservices Framework for Java (MSF4J) is a lightweight high performance framework for developing & running microservices. WSO2 MSF4J is one of the highest performing lightweight Java microservices frameworks. This tool will generate micro service skeleton from swagger definition. So you can use this project to convert your swagger definitions to micro service quickly.

# Server Stub generation
You can create simple maven project with maven archtype generator. Then create following structure within your project.
```
.
├── pom.xml
└── src
    └── main
        └── resources
            └── api.yaml
```
Now you need to include the API definition inside maven project an ideal place to have it is under “src/main/resources/api.yaml”.

Add the following plugin to the project.
The generated code will be at “src/gen/java” so we need to add this to the build which can be done by maven build helper plugin as below.
```
<plugin>
  <groupId>org.wso2.maven.plugins</groupId>
  <artifactId>swagger2msf4j-maven-plugin</artifactId>
  <version>1.0-SNAPSHOT</version>
  <configuration>
      <inputSpec>${project.basedir}/src/main/resources/api.yaml</inputSpec>
      <autoEnableOptions>true || false</autoEnableOptions>
  </configuration>
</plugin>
```

Then use “mvn swagger2msf4j:generate” command to generate the server stub. For that you need to add following plugin to your maven project.
```
<plugin>
  <groupId>org.codehaus.mojo</groupId>
  <artifactId>build-helper-maven-plugin</artifactId>
  <version>1.9.1</version>
  <executions>
      <execution>
          <id>add-source</id>
          <phase>generate-sources</phase>
          <goals>
              <goal>add-source</goal>
          </goals>
          <configuration>
              <sources>
                  <source>src/gen/java</source>
              </sources>
          </configuration>
      </execution>
  </executions>
</plugin>
```

Once code generated you can see all generated classes within you project as follows.

```
.
├── pom.xml
└── src
    ├── gen
    │   └── java
    │       └── org
    │           └── wso2
    │               └── carbon
    │                   └── apimgt
    │                       └── rest
    │                           └── api
    │                               └── publisher
    │                                   ├── ApiException.java
    │                                   ├── ApiResponseMessage.java
    │                                   ├── ApisApi.java
    │                                   ├── dto
    │                                   │   ├── APIBusinessInformationDTO.java
    │                                   ├── EnvironmentsApi.java
    │                                   ├── EnvironmentsApiService.java
    │                                   ├── factories
    │                                   │   ├── ApisApiServiceFactory.java
    │                                   ├── ProductsApi.java
    └── main
        ├── java
        │   └── org
        │       └── wso2
        │           └── carbon
        │               └── apimgt
        │                   └── rest
        │                       └── api
        │                           └── publisher
        │                               └── impl
        │                                   ├── ApisApiServiceImpl.java
        │                                   ├── ApplicationsApiServiceImpl.java
        │                                   ├── EnvironmentsApiServiceImpl.java
        │                                   ├── ProductsApiServiceImpl.java
        │                                   ├── SubscriptionsApiServiceImpl.java
        │                                   └── TiersApiServiceImpl.java
        └── resources
            └── api.yaml
            
```

Now you can add relavant dependencies and implement your logic for microservice. Then you need to add driver class for your service. Please sample driver class below

```
package org.wso2.carbon.apimgt.rest.api.store;
import org.wso2.msf4j.MicroservicesRunner;

public class Application {
    public static void main(String[] args) {
        new MicroservicesRunner()
                .deploy(new ApisApi())
                .start();
    }
}
```

Add following plugin by pointing to driver class. Then it will be added to menifiest file.
```
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-jar-plugin</artifactId>
        <configuration>
          <archive>
            <manifest>
              <mainClass>org.wso2.carbon.apimgt.rest.api.publisher.Application</mainClass>
            </manifest>
          </archive>
        </configuration>
      </plugin>
```

Then add following property as well.
```
    <properties>
        <microservice.mainClass>org.wso2.carbon.apimgt.rest.api.publisher.Application</microservice.mainClass>
    </properties>
```
Then build project and start it as follows.
```
sanjeewa@sanjeewa-ThinkPad-X1-Carbon-3rd:~/work/tech/micro-service$ java -jar target/HelloWorld-1.0.0-SNAPSHOT.jar 
2016-10-18 16:57:58 INFO  MicroservicesRegistry:76 - Added microservice: org.wso2.webinar.samples.msf4j.HelloWorld@1ed6993a
2016-10-18 16:57:58 INFO  NettyListener:56 - Starting Netty Http Transport Listener
2016-10-18 16:57:58 INFO  NettyListener:80 - Netty Listener starting on port 8080
2016-10-18 16:57:58 INFO  MicroservicesRunner:122 - Microservices server started in 222ms

```

