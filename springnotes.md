Building a RESTful Web Service with Spring

1. Overview of the Web Service
A RESTful web service is created to handle HTTP GET requests at http://localhost:8080/greeting.
Response Example:
json
{"id":1,"content":"Hello, World!"}
Customizing the Greeting:
Add a name query parameter to override the default greeting.
Example: http://localhost:8080/greeting?name=User produces:
json
{"id":1,"content":"Hello, User!"}

2. Prerequisites
Time: About 15 minutes.
Tools/Requirements:
Text editor or IDE (e.g., IntelliJ IDEA, VSCode, or Spring Tool Suite).
Java 17+.
Gradle 7.5+ or Maven 3.5+.

3. Starting the Project
Use Spring Initializr to set up the project:
Navigate to https://start.spring.io.
Select Gradle or Maven, choose Java, and add Spring Web as a dependency.
Click Generate to download a pre-configured ZIP file.
Alternatively:
Clone the starter code:
bash
git clone https://github.com/spring-guides/gs-rest-service.git
cd gs-rest-service/initial

4. Creating the Greeting Resource
Resource Representation Class:
Represents the Greeting resource with fields for id and content.
Example (Greeting.java):
java
public record Greeting(long id, String content) {}
Uses the Jackson JSON library to automatically convert Java objects to JSON.

5. Implementing the Controller
GreetingController.java:
Handles HTTP GET requests to /greeting.
Example:
java
@RestController
public class GreetingController {
    private static final String template = "Hello, %s!";
    private final AtomicLong counter = new AtomicLong();

    @GetMapping("/greeting")
    public Greeting greeting(@RequestParam(value = "name", defaultValue = "World") String name) {
        return new Greeting(counter.incrementAndGet(), String.format(template, name));
    }
}
Key Annotations:
@RestController: Combines @Controller and @ResponseBody to directly return domain objects as JSON.
@GetMapping: Maps GET requests to a specific method.
@RequestParam: Extracts query parameters with an optional default value.
Behavior:

The counter increments with each call, providing unique id values.
The name parameter defaults to "World" if not provided.

6. Running the Application
Spring Initializr generates the main application class:
java
@SpringBootApplication
public class RestServiceApplication {
    public static void main(String[] args) {
        SpringApplication.run(RestServiceApplication.class, args);
    }
}
@SpringBootApplication simplifies setup with:
@Configuration: Defines beans.
@EnableAutoConfiguration: Configures beans automatically based on the classpath.
@ComponentScan: Detects components in the base package.
Run the application:

Gradle:
bash
./gradlew bootRun
Maven:
bash
./mvnw spring-boot:run
Build and run a JAR file:
bash
./gradlew build
java -jar build/libs/gs-rest-service-0.1.0.jar

7. Testing the Service
Default Endpoint:
Visit http://localhost:8080/greeting:
json
{"id":1,"content":"Hello, World!"}
Custom Greeting:
Visit http://localhost:8080/greeting?name=User:
json
Copy code
{"id":2,"content":"Hello, User!"}

8. Summary
Created a simple RESTful web service with Spring.
Key features include:
JSON responses.
Query parameter customization.
Incremental IDs for greetings.
Application is entirely Java-based with minimal configuration, thanks to Spring Boot.
