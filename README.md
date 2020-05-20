# hibernate-example
Simple example of using Swagger and such realization of Spring Data JPA as Hibernate.

## Stack: Spring Boot, Maven, Spring Data JPA, PostgreSQL, Swagger, Lombok

## Step 1
Add dependencies in pom.xml.

```.xml
<dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
            <version>2.0.0.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-jpa</artifactId>
            <version>2.0.0.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.postgresql</groupId>
            <artifactId>postgresql</artifactId>
            <version>42.2.12</version>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>1.18.12</version>
        </dependency>
        <dependency>
            <groupId>io.springfox</groupId>
            <artifactId>springfox-swagger2</artifactId>
            <version>2.9.2</version>
        </dependency>
        <dependency>
            <groupId>io.springfox</groupId>
            <artifactId>springfox-swagger-ui</artifactId>
            <version>2.9.2</version>
        </dependency>
    </dependencies>
```

## Step 2
Change application.properties to application.yml and add settings.

```.yml
spring:
  datasource:
    driver-class-name: org.postgresql.Driver
    url: jdbc:postgresql://localhost:5432/autopark
    username: root
    password: root
  jpa:
    hibernate:
      ddl-auto: update
    show-sql: false
    database: postgresql
    database-platform: org.hibernate.dialect.PostgreSQLDialect
    open-in-view: false
    generate-ddl: false
    properties:
      hibernate:
        jdbc:
          lob:
            non_contextual_creation: true
server:
  port: 8090
```
## Step 3
Create configuration class for swagger.

```.java
@Configuration
@EnableSwagger2
public class SwaggerConfig {

    @Bean
    public Docket api() {
        return new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(apiInfo())
                .select()
                .apis(RequestHandlerSelectors.basePackage("com.github.camelya58.controllers"))
                .paths(PathSelectors.any())
                .build();
    }

    /* Describe APIs */
    private ApiInfo apiInfo() {
        return new ApiInfoBuilder()
                .title("Autopark API v.1.0")
                .description("Example of spring boot and hibernate REST application")
                .build();
    }

}
```

## Step 4
Create own repository.

```.java
@Repository
public interface CarRepository extends JpaRepository<Car, Long> {
}
```
## Step 5
Create models and add neccessary annotations.

```.java 
@Data
@Entity
@Table(name = "cars")
public class Car {

    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    private long id;

    private String brand;
    private Color color;

}
```
