# Swagger to MSF4J Generator
WSO2 Microservices Framework for Java (MSF4J) is a lightweight high performance framework for developing & running microservices. WSO2 MSF4J is one of the highest performing lightweight Java microservices frameworks. This tool will generate micro service skeleton from swagger definition. So you can use this project to convert your swagger definitions to micro service quickly.

# Server Stub generation
First you need to include the API definition inside maven project an ideal place to have it is under “src/main/resources/api.yaml”.

Add the following plugin to the project.
The generated code will be at “src/gen/java” so we need to add this to the build which can be done by maven build helper plugin as below.
```
<plugin>
  <groupId>org.wso2.maven.plugins</groupId>
  <artifactId>swagger2msf4j-maven-plugin</artifactId>
  <version>1.0-SNAPSHOT</version>
  <configuration>
      <inputSpec>${project.basedir}/src/main/resources/store-api.yaml</inputSpec>
  </configuration>
</plugin>
```

Then use “mvn swagger2msf4j:generate” command to generate the server stub.
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



