# Starting the Java Spring Boot Backend

NOTE: This is to begin the initial project as a modular monolith. If the application is to grow into multiple services/sub-applications, this is fine as a starting point for solo development, but alternative varying setup flows can be expected down the line.

### Prerequisites

- [Java Development Kit 21](https://www.oracle.com/java/technologies/downloads/#java21)
  - ensure that the JAVA_HOME environment variable is set and added to path; this process varias by operating system
  - can check current java version using the `java --version` command
    - at time of writing, output for this included `java 21.0.10 2026-01-20 LTS`

### Development Environment
- [Eclipse IDE for Enterprise Java and Web Developers](https://www.eclipse.org/downloads/packages/release/2026-03/r/eclipse-ide-enterprise-java-and-web-developers)
  - Also - [Project Lombok](https://projectlombok.org/setup/eclipse) for data annotations without IDE issues

## My Setup Steps

### Spring Initializr

- go to [the Spring Initializr website](https://start.spring.io/)
- Select the following:
  - Project: `Maven`
  - Language: `Java`
  - Spring Boot: `4.0.6`
  - Project Metadata:
    - Group: `com.myapp`
    - Artifact: `myapp-backend` - I just used the name of my repo, `nullnet-backend`
    - Package name: `com.myapp.api`
    - Packaging: `jar`
    - Configuration: `Properties`
    - Java: `21`
- On the right side, press the "ADD DEPENDENCIES" button (or press `ctrl+B`), and add:
  - `Spring Boot DevTools`
  - `Lombok`
  - `Spring Web`
  - `Spring Security`
    - will require coding additional setup after exporting the initial project
  - `Spring Data JPA`
  - `H2 Database`
    - for development only; easily removed after AWS DEV database & connection credentials are set up
  - `PostgreSQL Driver`
  - `SpringDoc OpenAPI`
  - `Testcontainers`
- On the bottom, press the "GENERATE" button (or press `ctrl+⏎`)
- Inside the folder within the generated .zip file, take the files and move them into the folder containing the application (for me, the `myapp-backend` repo)

### Development Environment

- Open Eclipse IDE, and add the project to the workspace:
  - `File` -> `Open Projects from File System...`
  - In the window that opens, press the `Directory...` button next to the `Import source:` dropdown near the top
  - Select the folder containing the Spring Initializr project
  - Press the `Finish` button
- Add the Spring Boot Dashboard to Eclipse:
  - `Help` -> `Eclipse Marketplace...`
  - Search "Spring Tool Suite" to install the latest RELEASE version of Spring Tools (for me, this was version `5.1.1.RELEASE`)
  - Restart Eclipse
  - `Window` -> `Show View` -> `Other...`
  - Search "Spring" or "Boot" to select `Boot Dashboard`
  - Press the `Open` button
- In the Boot Dashboard sub-window that now appears in Eclipse, hit the dropdown arrow next to "local" and ensure that your equivalent of `myapp-backend` is present
  - If it's not present:
    - Right-click the project's folder in the Project Explorer sub-window
    - `Run As` -> `Spring Boot App`
    - Select the option that does NOT start with "Test"
    - Select the `myapp-backend` option that now appears in the Boot Dashboard
    - Hit the red-square Stop button at the top bar of the Boot Dashboard

### Configuration

- Set properties in `src/main/resources/application.properties`:
  - `server.port=8081`
    - Changes the server port; ensure this isn't shared with any other applications running on the system, INCLUDING other Spring applications
      - Watch out for `9000` (SonarQube default), `5432` (Postgres local default), `3000` (Next.js default)
      - Range `8080` to `8099` should be enough; if not, `9X01` to `9X99`
  - H2 local file-persistent database properties
    - AGAIN, THIS H2 SETUP IS FOR DEVELOPMENT ONLY
    - `spring.datasource.url=jdbc:h2:jdbc:h2:file:./data/localdb;MODE=PostgreSQL`
    - `spring.h2.console.enabled=true`
    - `spring.h2.console.path=/h2-console`
      - Provides a Web browser H2 console at `localhost:[your port number]/h2-console`
  - Further database settings:
    - `spring.jpa.database-platform=org.hibernate.dialect.PostgreSQLDialect`
    - `spring.jpa.show-sql=true`
      - Shows what SQL queries are being made in a local dev environment; should only be present in dev properties
    - `spring.jpa.hibernate.ddl-auto=create-drop`
      - The `create-drop` setting is only for development with H2, where data structure might change between builds
      - When data structures are more solidified, consider switching to `update` to preserve local-db state between runs
      - Only `none` or `validate` should be used when a remote database is being used, and DDL should be handled elsewhere (like in a database client such as DBeaver or SSMS)
    - Database credentials through environment variables
      - `spring.datasource.username=${DB_USERNAME}`
      - `spring.datasource.password=${DB_PASSWORD}`
      - To set these environment variables for the project:
        - Right-click the project's folder in the Project Explorer sub-window
        - `Run As` -> `Run Configurations...`
        - Under the "Spring Boot App" section (may need to scroll down & press its dropdown arrow), select your `myapp-backend` equivalent
        - Go to the "Environment" tab
        - `Add...`
        - THESE CREDENTIALS ARE FOR H2; REMEMBER TO CHANGE THEM WHEN SWITCHING TO A LIVE DATABASE:
          - Name: `DB_USERNAME`, Value: `sa`
          - Name: `DB_PASSWORD`, Value: ``
            - If the variable doesn't appear after entering no value, you may add a dummy value to initialize the variable, then press `Edit...` to empty its value while keeping the variable
        - Press `Apply` and close the Run Configurations window
  - Basic logging config
    - `logging.file.name=application.log`
    - `logging.level.org.springframework.web=INFO`
    - `logging.level.org.hibernate=ERROR`
    - `logging.level.com.myapp.api=DEBUG` - mind that this targets the `com.myapp.api` package, and your equivalent for this package may vary
  - Maximum file-upload limit raise (only for an application that may take file uploads larger than 1 MB)
    - `spring.servlet.multipart.max-file-size=10MB`
    - `spring.servlet.multipart.max-request-size=10MB`
- Add test-scope-only properties in a new file `src/main/resources/application-test.properties`:
  - `spring.datasource.url=jdbc:h2:file:./data/localtestdb;MODE=PostgreSQL`
  - `spring.jpa.properties.hibernate.default_schema=public`
  - `spring.datasource.username=sa`
  - `spring.datasource.password=`
  - `spring.jpa.database-platform=org.hibernate.dialect.PostgreSQLDialect`
  - `spring.jpa.show-sql=true`
  - `spring.jpa.hibernate.ddl-auto=create-drop`
- Place these additional items in the `pom.xml` file:
  - Between `<properties>` and `</properties>` tags:
    - `<sonar.projectKey>Myapp-Backend</sonar.projectKey>`
    - `<sonar.host.url>http://localhost:9000</sonar.host.url>`
    - `<sonar.token>${env.SONAR_TOKEN}</sonar.token>`
      - This requires an environment variable, which can be set through:
        - Right-click the project's folder in the Project Explorer sub-window
        - `Run As` -> `Run Configurations...`
        - Right-click the "Maven Build" section, and press `New Configuration`
          - Enter a name in the "Name" box at the top (like `Sonar-Myapp`)
          - Press the first `Workspace...` button under the "Base directory" box
          - Select your project
          - Paste this into the "Goals" text box: `clean verify org.sonarsource.scanner.maven:sonar-maven-plugin:sonar`
          - Go to the "Environment" tab
          - `Add...`
          - Name: `SONAR_TOKEN`, Value: ``
            - If the variable doesn't appear after entering no value, you may add a dummy value to initialize the variable, then press `Edit...` to empty its value while keeping the variable
            - You can come back and fill in this value when SonarQube is set up
          - Press `Apply` and close the Run Configurations window
  - Add these extra dependencies between the `<dependencies></dependencies>` tags (that should already make up most of the file):
```
<!-- Internal cache enabler -->
<!-- Source: https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-cache -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-cache</artifactId>
</dependency>

<!-- Internal cache provider -->
<!-- Source: https://mvnrepository.com/artifact/com.github.ben-manes.caffeine/caffeine -->
<dependency>
    <groupId>com.github.ben-manes.caffeine</groupId>
    <artifactId>caffeine</artifactId>
    <scope>compile</scope>
</dependency>
```
  - Add these plugins between the `<plugin></plugin>` tags:
<details>
  <summary>
    Click for plugins to add...
  </summary>

```
<!-- Alleviates a warning about adding Mockito as an agent in my build -->
<!-- From Mockito documentation: https://javadoc.io/doc/org.mockito/mockito-core/latest/org.mockito/org/mockito/Mockito.html#0.3 -->
<plugin>
  <groupId>org.apache.maven.plugins</groupId>
  <artifactId>maven-dependency-plugin</artifactId>
  <executions>
      <execution>
          <goals>
              <goal>properties</goal>
          </goals>
      </execution>
  </executions>
</plugin>

<!-- 
  Above & below plugins are part of the same setup.
  
  The argline path was selected based on this warning for not doing this setup:
  
  WARNING: A Java agent has been loaded dynamically (C:\Users\user\.m2\repository\net\bytebuddy\byte-buddy-agent\1.17.8\byte-buddy-agent-1.17.8.jar)
-->
<plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-surefire-plugin</artifactId>
        <configuration>
          <!-- using "${argline}" to APPEND TO the arguments instead of replacing them -->
          <!-- needed for Jacoco to work, as it injects " -javaagent:jacocoagent.jar " -->
            <argLine>
                ${argLine} -javaagent:${settings.localRepository}/net/bytebuddy/byte-buddy-agent/1.17.8/byte-buddy-agent-1.17.8.jar
            </argLine>
        </configuration>
    </plugin>
    
    <!-- JaCoCo test coverage report tool -->
    <!-- Source: https://www.baeldung.com/jacoco -->
<plugin>
  <groupId>org.jacoco</groupId>
  <artifactId>jacoco-maven-plugin</artifactId>
  <!-- version retrieved April 12, 2026, from https://www.eclemma.org/jacoco/ -->
  <version>0.8.14</version>
  <executions>
    <execution>
      <goals>
          <goal>prepare-agent</goal>
      </goals>
    </execution>
    <execution>
      <id>report</id>
      <phase>test</phase>
      <goals>
        <goal>report</goal>
      </goals>
    </execution>
  </executions>
</plugin>

<!-- Sonar for code linting -->
<!-- Source: https://mvnrepository.com/artifact/org.sonarsource.scanner.maven/sonar-maven-plugin -->
<!-- Running as plugin; this doesn't belong in the runtime, it only analyzes the build -->
<plugin>
  <groupId>org.sonarsource.scanner.maven</groupId>
  <artifactId>sonar-maven-plugin</artifactId>
  <version>5.6.0.6792</version>
</plugin>
```
</details>

  - Add this after the `</build>` closing tag but before the `</project>` closing tag:
```
<!-- Additional Jacoco setup -->
<!-- https://www.jacoco.org/jacoco/trunk/doc/maven.html -->
<reporting>
  <plugins>
    <plugin>
      <groupId>org.jacoco</groupId>
      <artifactId>jacoco-maven-plugin</artifactId>
      <reportSets>
        <reportSet>
          <reports>
            <report>report</report>
          </reports>
        </reportSet>
      </reportSets>
    </plugin>
  </plugins>
</reporting>
```

### Security Reset
To enable API endpoints to be reached, Spring Security must be configured to authorize HTTP requests

- For the sake of early setup, this configuration class (in a `.config` package under `com.myapp.api`) can be added to just enable any request to each any endpoint:
```
package com.myapp.api.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.method.configuration.EnableMethodSecurity;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.web.SecurityFilterChain;

@Configuration
@EnableMethodSecurity
public class SecurityConfig {
	
	// https://docs.spring.io/spring-security/reference/api/java/org/springframework/security/config/annotation/web/builders/HttpSecurity.html
	@Bean
 	public SecurityFilterChain securityFilterChain(HttpSecurity http)  {
		http
			// cross-site request forgery security disabled - will likely want to remove or modify this line before deployment
			.csrf(csrf -> csrf.disable())
			
			.authorizeHttpRequests(authorize -> 
				authorize
					.anyRequest().permitAll()
			)
		;
		
		return http.build();
	}
}

```
- This effectively DISABLES Spring Security for user auth
  - If/when user auth is needed for endpoint security, come back to this file, and make the necessary changes

## Run Check

Ensure the application runs.

- In the Boot Dashboard sub-window in Eclipse (set up above)
  - select your equivalent to `myapp-backend`
  - hit the leftmost button at the top of the Boot Dashboard (tooltip "Start or restart...")
  - wait for console output to stop
  - ensure there's a green up arrow next to `myapp-backend` in the Boot Dashboard
- Using a Web browser, visit one of the local dev endpoints auto-generated by the dependency setup above, to ensure that the application can be reached:
  - http://localhost:8081/swagger-ui/index.html - Swagger UI
  - http://localhost:8081/v3/api-docs - OpenAPI JSON

---

<details>
  <summary>
    Click here to see my initial application.properties
  </summary>

```

spring.application.name=myapp-backend
server.port=8081

# H2 LOCAL FILE-PERSISTENT DATABASE - ONLY FOR LOCAL DEVELOPMENT & TESTING SCOPE
spring.datasource.url=jdbc:h2:file:./data/localdb;MODE=PostgreSQL

# PROVIDES A WEB BROWSER H2 CONSOLE AT /h2-console
spring.h2.console.enabled=true
spring.h2.console.path=/h2-console

# DEPLOYMENT-USABLE DATABASE SETTINGS
spring.jpa.properties.hibernate.default_schema=public
#spring.datasource.driver-class-name=org.postgresql.Driver # will need "jdbc:postgresql:" url instead of "jdbc:h2:" url to use postgres driver
spring.datasource.username=${DB_USERNAME}
spring.datasource.password=${DB_PASSWORD}

spring.jpa.database-platform=org.hibernate.dialect.PostgreSQLDialect
spring.jpa.show-sql=true

# only "none" or "validate" should be used in a deployment project, and DDL should be handled elsewhere
spring.jpa.hibernate.ddl-auto=create-drop

# logging config
logging.file.name=application.log
logging.level.org.springframework.web=INFO
logging.level.org.hibernate=ERROR
logging.level.com.myapp.api=DEBUG

# increase maximum upload size from the default 1MB (implemented for thumbnail API)
spring.servlet.multipart.max-file-size=10MB
spring.servlet.multipart.max-request-size=10MB
```
</details>

<details>
  <summary>
    Click here to see my initial application-test.properties
  </summary>

```

# H2 LOCAL FILE-PERSISTENT DATABASE - ONLY FOR LOCAL DEVELOPMENT & TESTING SCOPE
spring.datasource.url=jdbc:h2:file:./data/localtestdb;MODE=PostgreSQL

# DEPLOYMENT-USABLE DATABASE SETTINGS
spring.jpa.properties.hibernate.default_schema=public
#spring.datasource.driver-class-name=org.postgresql.Driver # would need "jdbc:postgresql:" url instead of "jdbc:h2:" url
spring.datasource.username=sa
spring.datasource.password=

spring.jpa.database-platform=org.hibernate.dialect.PostgreSQLDialect
spring.jpa.show-sql=true

# FOR TESTING, this cleans up after itself
spring.jpa.hibernate.ddl-auto=create-drop

```
</details>

<details>
  <summary>
    Click here to see my initial pom.xml
  </summary>

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>4.0.6</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>
	<groupId>com.myapp</groupId>
	<artifactId>myapp-backend</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<name/>
	<description/>
	<url/>
	<licenses>
		<license/>
	</licenses>
	<developers>
		<developer/>
	</developers>
	<scm>
		<connection/>
		<developerConnection/>
		<tag/>
		<url/>
	</scm>
	<properties>
		<java.version>21</java.version>
		
		<!--
			For use while running SonarQube server on my local system at 
			localhost:9000
			
			Scans are initiated through my own custom Maven build 
			configuration in Eclipse. In deployment, I would get the Sonar 
			token from an environment config that reads its value from a 
			secret and makes the value available for use in the application.
		-->
		<sonar.projectKey>Myapp-Backend</sonar.projectKey>
		<sonar.host.url>http://localhost:9000</sonar.host.url>
		<sonar.token>${env.SONAR_TOKEN}</sonar.token>
		<sonar.test.exclusions>
			**/testclassfailures/**
		</sonar.test.exclusions>
	</properties>
	
	<dependencies>
		<!-- Spring Boot Starter - Basic Spring Starter project backbone -->
		<!-- From Spring Initializr -->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter</artifactId>
		</dependency>
		
		<!-- For application hot reloading during local development -->
		<!-- From Spring Initializr -->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-devtools</artifactId>
			<scope>runtime</scope>
			<optional>true</optional>
		</dependency>
		
		<!-- Testing dependency bundle - JUnit, Mockito -->
		<!-- From Spring Initializr -->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>
		
		<!-- Spring Boot Security -->
		<!-- Source: https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-security -->
		<dependency>
		    <groupId>org.springframework.boot</groupId>
		    <artifactId>spring-boot-starter-security</artifactId>
		    <scope>compile</scope>
		</dependency>
		
		<!-- Rest API Controller dependencies -->
		<!-- Source: https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-web -->
		<dependency>
		    <groupId>org.springframework.boot</groupId>
		    <artifactId>spring-boot-starter-web</artifactId>
		</dependency>
		<!-- Additional MVC from Spring Initializr -->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-webmvc</artifactId>
		</dependency>
		
		<!-- Springdoc OpenAPI documentation tool that also runs Swagger UI -->
		<!-- Source: https://mvnrepository.com/artifact/org.springdoc/springdoc-openapi-starter-webmvc-ui -->
		<dependency>
		    <groupId>org.springdoc</groupId>
		    <artifactId>springdoc-openapi-starter-webmvc-ui</artifactId>
		    <version>3.0.2</version>
		    <scope>compile</scope>
		</dependency>
		
		<!-- JPA entity support for interfacing with databases -->
		<!-- Source: https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-data-jpa -->
		<dependency>
		    <groupId>org.springframework.boot</groupId>
		    <artifactId>spring-boot-starter-data-jpa</artifactId>
		</dependency>
		
		<!-- PostgreSQL dialect -->
		<!-- Source: https://mvnrepository.com/artifact/org.postgresql/postgresql -->
		<dependency>
		    <groupId>org.postgresql</groupId>
		    <artifactId>postgresql</artifactId>
			<scope>runtime</scope>
		</dependency>
		
		<!-- For local H2 database -->
		<!-- Source: https://mvnrepository.com/artifact/com.h2database/h2 -->
		<dependency>
			<groupId>com.h2database</groupId>
			<artifactId>h2</artifactId>
			<!-- RECOMMENDED to use "test" scope later on, if not switching entirely to testcontainers postgres -->
			<scope>runtime</scope>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-h2console</artifactId>
		</dependency>
		
		<!-- For quicker boilerplate -->
		<!-- Source: https://mvnrepository.com/artifact/org.projectlombok/lombok -->
		<dependency>
		    <groupId>org.projectlombok</groupId>
		    <artifactId>lombok</artifactId>
		    <optional>true</optional>
		    <scope>compile</scope>
		</dependency>
		
		<!-- Required for @WebMvcTest in Spring Boot 4.X -->
		<!-- Source: https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-webmvc-test -->
		<dependency>
		    <groupId>org.springframework.boot</groupId>
		    <artifactId>spring-boot-starter-webmvc-test</artifactId>
		    <scope>test</scope>
		</dependency>
		
		<!-- Required for @DataJpaTest in Spring Boot 4.X -->
		<!-- Source: https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-data-jpa-test -->
		<dependency>
		    <groupId>org.springframework.boot</groupId>
		    <artifactId>spring-boot-starter-data-jpa-test</artifactId>
		    <scope>test</scope>
		</dependency>
		
		<!-- Required for @WithMockUser in Spring Boot 4.X, for testing routes with auth states -->
		<!-- From Spring Initializr -->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-security-test</artifactId>
			<scope>test</scope>
		</dependency>
		
		<!-- For Docker-composable local test-scope instances of infra & service dependencies -->
		<!-- For more advanced testing once setup is off the ground -->
		<!-- From Spring Initializr -->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-testcontainers</artifactId>
			<scope>test</scope>
		</dependency>
		<dependency>
			<groupId>org.testcontainers</groupId>
			<artifactId>testcontainers-junit-jupiter</artifactId>
			<scope>test</scope>
		</dependency>
		<dependency>
			<groupId>org.testcontainers</groupId>
			<artifactId>testcontainers-postgresql</artifactId>
			<scope>test</scope>
		</dependency>
		
		<!-- Internal cache enabler -->
		<!-- Source: https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-cache -->
		<dependency>
		    <groupId>org.springframework.boot</groupId>
		    <artifactId>spring-boot-starter-cache</artifactId>
		</dependency>
		
		<!-- Internal cache provider -->
		<!-- Source: https://mvnrepository.com/artifact/com.github.ben-manes.caffeine/caffeine -->
		<dependency>
		    <groupId>com.github.ben-manes.caffeine</groupId>
		    <artifactId>caffeine</artifactId>
		    <scope>compile</scope>
		</dependency>
	</dependencies>

	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
				<configuration>
					<excludes>
						<exclude>
							<groupId>org.projectlombok</groupId>
							<artifactId>lombok</artifactId>
						</exclude>
					</excludes>
				</configuration>
			</plugin>
			<plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-compiler-plugin</artifactId>
				<executions>
					<execution>
						<id>default-compile</id>
						<phase>compile</phase>
						<goals>
							<goal>compile</goal>
						</goals>
						<configuration>
							<annotationProcessorPaths>
								<path>
									<groupId>org.projectlombok</groupId>
									<artifactId>lombok</artifactId>
								</path>
							</annotationProcessorPaths>
						</configuration>
					</execution>
					<execution>
						<id>default-testCompile</id>
						<phase>test-compile</phase>
						<goals>
							<goal>testCompile</goal>
						</goals>
						<configuration>
							<annotationProcessorPaths>
								<path>
									<groupId>org.projectlombok</groupId>
									<artifactId>lombok</artifactId>
								</path>
							</annotationProcessorPaths>
						</configuration>
					</execution>
				</executions>
			</plugin>
			
			<!-- Alleviates a warning about adding Mockito as an agent in my build -->
			<!-- From Mockito documentation: https://javadoc.io/doc/org.mockito/mockito-core/latest/org.mockito/org/mockito/Mockito.html#0.3 -->
			<plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-dependency-plugin</artifactId>
				<executions>
				    <execution>
				        <goals>
				            <goal>properties</goal>
				        </goals>
				    </execution>
				</executions>
			</plugin>
			
			<!-- 
				Above & below plugins are part of the same setup.
				
				The argline path was selected based on this warning for not doing this setup:
				
				WARNING: A Java agent has been loaded dynamically (C:\Users\user\.m2\repository\net\bytebuddy\byte-buddy-agent\1.17.8\byte-buddy-agent-1.17.8.jar)
			-->
			<plugin>
	            <groupId>org.apache.maven.plugins</groupId>
	            <artifactId>maven-surefire-plugin</artifactId>
	            <configuration>
	            	<!-- using "${argline}" to APPEND TO the arguments instead of replacing them -->
	            	<!-- needed for Jacoco to work, as it injects " -javaagent:jacocoagent.jar " -->
	                <argLine>
	                    ${argLine} -javaagent:${settings.localRepository}/net/bytebuddy/byte-buddy-agent/1.17.8/byte-buddy-agent-1.17.8.jar
	                </argLine>
	            </configuration>
	        </plugin>
	        
	        <!-- JaCoCo test coverage report tool -->
	        <!-- Source: https://www.baeldung.com/jacoco -->
			<plugin>
				<groupId>org.jacoco</groupId>
				<artifactId>jacoco-maven-plugin</artifactId>
				<!-- version retrieved April 12, 2026, from https://www.eclemma.org/jacoco/ -->
				<version>0.8.14</version>
				<executions>
					<execution>
						<goals>
					    	<goal>prepare-agent</goal>
						</goals>
					</execution>
					<execution>
						<id>report</id>
						<phase>test</phase>
						<goals>
							<goal>report</goal>
						</goals>
					</execution>
				</executions>
			</plugin>
			
			<!-- Sonar for code linting -->
			<!-- Source: https://mvnrepository.com/artifact/org.sonarsource.scanner.maven/sonar-maven-plugin -->
			<!-- Running as plugin; this doesn't belong in the runtime, it only analyzes the build -->
			<plugin>
				<groupId>org.sonarsource.scanner.maven</groupId>
				<artifactId>sonar-maven-plugin</artifactId>
				<version>5.6.0.6792</version>
		    </plugin>
		</plugins>
	</build>
	
	<!-- Additional Jacoco setup -->
	<!-- https://www.jacoco.org/jacoco/trunk/doc/maven.html -->
	<reporting>
		<plugins>
			<plugin>
				<groupId>org.jacoco</groupId>
				<artifactId>jacoco-maven-plugin</artifactId>
				<reportSets>
					<reportSet>
						<reports>
							<report>report</report>
						</reports>
					</reportSet>
				</reportSets>
			</plugin>
		</plugins>
	</reporting>
</project>

```
</details>
