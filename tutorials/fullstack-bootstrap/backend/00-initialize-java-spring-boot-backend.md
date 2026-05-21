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
  - Basic logging config
    - `logging.file.name=application.log`
    - `logging.level.org.springframework.web=INFO`
    - `logging.level.org.hibernate=ERROR`
    - `logging.level.com.myapp.api=DEBUG` - mind that this targets the `com.myapp.api` package, and your equivalent for this package may vary
  - Maximum file-upload limit raise (only for an application that may take file uploads larger than 1 MB)
    - `spring.servlet.multipart.max-file-size=10MB`
    - `spring.servlet.multipart.max-request-size=10MB`
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

## Run Check

Ensure the application runs.
