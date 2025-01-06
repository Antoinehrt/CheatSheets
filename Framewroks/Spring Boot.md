**Spring Boot** is an open-source Java framework used for programming standalone, production-grade Spring-based applications with a bundle of libraries that make project startup and management easier.

#Java #Backend #API #RestAPI

- [[Spring Boot#Setup a project|Setup a project]] 
	- [[Spring Boot#1. DockerFile For Spring Boot|1. DockerFile]]
	- [[Spring Boot#2. Environment File|2. Environment File]]
	- [[Spring Boot#3. Docker-compose with the database|3. Docker Compose Configuration]]
	- [[Spring Boot#4. Setup Application Properties|4. Application Properties]]
- [[Spring Boot#Creation of a basic CRUD|Basic CRUD]]
	- [[Spring Boot#1. Entity|1. Entity]]
	- [[Spring Boot#2. Repository|2. Repository]]
	- [[Spring Boot#3. Controller|3. Controller]]
- [[Spring Boot#Problems|Problems]]

# Setup a project

To setup the project you can either follow your IDE suggestion or go to [start.spring.io](https://start.spring.io/) and follow their guide.

This cheat sheet will explain how to containerise a Spring Boot project with PostgreSQL and Maven using [[Docker]] 


## Spring Boot DockerFile

When the project is setup, we will already begin by the containerisation because in order to run properly, Spring Boot need to be connected to a running database.

### 1. DockerFile For Spring Boot

Unlike many common [[Docker#DockerFile|DockerFile]] setups that only copy the compiled JAR, this approach copies the entire project into the container. This allows the container to build the JAR itself, making it more suitable for active development workflows.  

```DockerFile
FROM openjdk:23-jdk-slim  
  
# Install Maven  
RUN apt-get update && apt-get install -y maven && rm -rf /var/lib/apt/lists/*  
  
# Verify versions  
RUN java -version && mvn -version  
  
# Copy the project  
COPY . .  
  
# Compile the project  
RUN ./mvnw clean package -DskipTests  
  
# Run the project  
CMD ["java", "-jar", "target/<project_name>-0.0.1-SNAPSHOT.jar"]
```

### 2. Environment File

Before the next step, to be more secure I will create a .env file at the project root.

```env
DATABASE_NAME=<YourDbName>
DATABASE_URL=<YourDbURL>
DATABASE_USERNAME=<YourUser>
DATABASE_PASSWORD=<YourPassword>
```
**Don't forget to remove it from the [[GIT#.gitignore|.gitignore]]**

### 3. Docker-compose with the database

Here is the docker compose I used. I'll use the environment variable created before.

In a local setup, the JDBC URL typically looks like this:  
`jdbc:postgresql://localhost:5432/${DATABASE_NAME}`

In Docker Compose, `localhost` refers to the container itself, not the shared network. To connect to the database service, use the service name defined in `docker-compose.yml` (e.g., `db`):  
`jdbc:postgresql://db:5432/${DATABASE_NAME}`

This allows containers to communicate within the Docker network.

```yml
services:  
  db:  
    image: 'postgres:latest'  
    container_name: '<container_name>'  
    restart: unless-stopped  
    env_file:  
      - .env  
    environment:  
      - 'POSTGRES_DB=${DATABASE_NAME}'  
      - 'POSTGRES_PASSWORD=${DATABASE_PASSWORD}'  
      - 'POSTGRES_USER=${DATABASE_USERNAME}'  
    ports:  
      - '5432:5432'  
    volumes:  
      - postgres-data:/var/lib/postgresql/data  
  
  app:  
    container_name: '<container_name>'  
    build:  
      context: .  
      dockerfile: Dockerfile  
    environment:  
      - SPRING_DATASOURCE_URL=jdbc:postgresql://db:5432/${DATABASE_NAME}  
      - SPRING_DATASOURCE_USERNAME=${DATABASE_USERNAME}  
      - SPRING_DATASOURCE_PASSWORD=${DATABASE_PASSWORD}  
      - SPRING_JPA_HIBERNATE_DDL_AUTO=update  
      - SPRING_JPA_SHOW_SQL=true  
    ports:  
      - '8080:8080'  
    depends_on:  
      - db  
    volumes:  
      - .:/app  
    stdin_open: true  
    tty: true  
  
volumes:  
  postgres-data: { }
```

The `postgres-data` volume ensures that your database data persists even if the container is recreated. Without it, all data would be lost when the container is stopped or removed



### 4. Setup Application Properties

Using environment variables defined in Docker Compose avoids exposing sensitive information in application-level files and ensures better security during deployment.
They are also easier to access then a `.env` file.

```properties
spring.application.name=<project_name>
spring.datasource.url=${SPRING_DATABASE_URL}  
spring.datasource.username=${SPRING_DATASOURCE_USERNAME}  
spring.datasource.password=${SPRING_DATABASE_PASSWORD}  
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.PostgreSQLDialect
# With this line the project will create the db schematic
spring.jpa.hibernate.ddl-auto=update 
```

# Creation of a basic CRUD

### 1. Entity

The **Entity** represents a table in your database. Each field in the class corresponds to a column in the table.
You need to:

- Replace `<ClassName>` with your entity name (e.g., `Player`, `Team`).
- Replace `<table_name>` with the desired database table name.
- Define fields and their corresponding database columns (e.g., `<first_column>`).

```java
@Entity  
@Table(name = "<TABLE_NAME>")  
public class <ENTITY_NAME> {  
    @Id  
    @GeneratedValue(strategy = GenerationType.UUID)  
    private UUID id;  
  
    @Column(name = "<COLUMN_NAME>", nullable = false)  
    private String <COLUMN_NAME>;

	// Add other fields and columns as needed
  
    public <ENTITY_NAME>() {  
    }
	
	// Add getters and setters

}
```


### 2. Repository

The **Repository** handles database operations (like save, update, delete) for the `Entity`.  
You need to:

- Replace `<Class_name_repository>` with your repository name (e.g., `PlayerRepository`).
- Replace `<entity_name>` with your entity class name (e.g., `Player`).

`UUID` is the type of the entity id

```java
  
public interface <REPOSITORY_NAME>> extends JpaRepository<<ENTITY_NAME>, UUID> {  
}

```


### 3. Controller

The **Controller** provides RESTful APIs to interact with your application.

- Replace `<class_name_controller>` with the controller name (e.g., `PlayerController`).
- Replace `<repository_class>` with the repository name.
- Replace `<Entity>` with the entity class name (e.g., `Player`).
- Replace `<name_api>` with the desired API endpoint (e.g., `players`).

```java
@RestController  
@RequestMapping("/api/<API_ENDPOINT>")  
public class <CONTROLLER_NAME> {  
  
    @Autowired  
    private <REPOSITORY_CLASS> repository;  
  
    @GetMapping  
    public List<Entity> getAll() {  
        return repository.findAll();  
    }  
  
    @GetMapping("/{id}")  
    public ResponseEntity<Entity> getById(@PathVariable UUID id) {  
        Entity entity = repository.findById(id).orElse(null);  
        if (entity == null) {  
            return new ResponseEntity<>(HttpStatus.NOT_FOUND);  
        }  
        return new ResponseEntity<>(entity, HttpStatus.OK);  
    }  
  
    @PostMapping  
    public ResponseEntity<Entity> add(@RequestBody Entity Entity) {  
        Entity savedEntity = repository.save(entity);  
        return new ResponseEntity<>(savedEntity, HttpStatus.CREATED);  
    }  
  
    @PutMapping("/{id}")  
    public ResponseEntity<Entity> updatePlayer(@PathVariable UUID id, @RequestBody Entity entity) {  
        Entity existingEntity = repository.findById(id).orElse(null);  
        if (existingEntity == null) {  
            return new ResponseEntity<>(HttpStatus.NOT_FOUND);  
        }  
        existingEntity.setFirstname(entity.getFirstname());  
        existingEntity.setSurname(entity.getSurname());  
        Entity updatedEntity = repository.save(existingEntity);  
        return new ResponseEntity<>(updatedEntity, HttpStatus.OK);  
    }  
  
    @DeleteMapping("/{id}")  
    public ResponseEntity<Void> deleteEntity(@PathVariable UUID id) {  
        Player existingEntity = repository.findById(id).orElse(null);  
        if (existingEntity == null) {  
            return new ResponseEntity<>(HttpStatus.NOT_FOUND);  
        }  
        repository.delete(existingEntity);  
        return new ResponseEntity<>(HttpStatus.NO_CONTENT);  
    }  
}
```
