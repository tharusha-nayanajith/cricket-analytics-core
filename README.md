Product Service
A lightweight Spring Boot microservice for managing products via a RESTful API. It uses an in-memory H2 database for persistence and exposes interactive API documentation through Swagger UI.

Tech Stack

Java (11 or 17 recommended)
Spring Boot — Web, Data JPA
H2 — In-memory database
Lombok — Boilerplate reduction
springdoc-openapi — Swagger/OpenAPI docs
Maven — Build tool


Project Structure
com.sliit.product_service
├── controller        # REST endpoints (ProductController)
├── service           # Business logic (interfaces + implementations)
├── repository        # Spring Data JPA (ProductRepository)
└── entity            # JPA entity classes (Product)

API Endpoints
All routes are prefixed with /api via the configured context path.
MethodEndpointDescriptionPOST/api/productsCreate a new productGET/api/productsGet all productsGET/api/products/{id}Get a product by IDDELETE/api/products/{id}Delete a product by ID
Request body for POST:
json{ "name": "Widget", "price": 9.99 }

Getting Started
Prerequisites

Java 11 or 17
Maven (or use the included ./mvnw wrapper)

Build
bash./mvnw clean package
Run
bash# Using the Spring Boot Maven plugin
./mvnw spring-boot:run

# Or run the packaged JAR
java -jar target/*.jar

Configuration
Key settings in src/main/resources/application.properties:
propertiesserver.servlet.context-path=/api

spring.datasource.url=jdbc:h2:mem:testdb
spring.datasource.driverClassName=org.h2.Driver
spring.datasource.username=sa
spring.datasource.password=

spring.jpa.hibernate.ddl-auto=update
spring.h2.console.enabled=true

H2 Console
Access the in-memory database while the app is running:

URL: http://localhost:8080/api/h2-console
JDBC URL: jdbc:h2:mem:testdb
Username: sa
Password: (leave blank)


Data is not persisted between restarts.


Swagger UI
Interactive API documentation is available at:
http://localhost:8080/api/swagger-ui.html

Quick curl Examples
bash# Create a product
curl -X POST http://localhost:8080/api/products \
  -H "Content-Type: application/json" \
  -d '{"name":"Widget","price":9.99}'

# Get all products
curl http://localhost:8080/api/products

# Get a product by ID
curl http://localhost:8080/api/products/1

# Delete a product
curl -X DELETE http://localhost:8080/api/products/1

Troubleshooting
Lombok / javac error (NoSuchFieldError: JCImport)
This is a Lombok–JDK compatibility issue. To fix it:

Make sure JAVA_HOME points to JDK 11 or 17.
Update Lombok to the latest stable version in pom.xml.
If using IntelliJ, install/update the Lombok plugin and enable annotation processing.
Run a clean build: ./mvnw -U clean package

H2 Console not reachable
Confirm spring.h2.console.enabled=true is set. Also note that the context path /api applies to the console — the correct URL is /api/h2-console, not /h2-console.
Swagger UI not found
Ensure springdoc-openapi-ui is present in pom.xml and that you're accessing it via the full context path: /api/swagger-ui.html.