# Java Stack Template

## Detected Technologies
- **Framework:** [Spring Boot/Quarkus/Micronaut/etc.]
- **Package Manager:** [Maven/Gradle]
- **Test Runner:** [JUnit/TestNG/etc.]
- **Linter:** [Checkstyle/SpotBugs/etc.]
- **Build Tool:** [Maven/Gradle]

## Common Stack Patterns

### Spring Boot
- Controllers: `@RestController`, `@Controller`
- Services: `@Service`
- Repositories: `@Repository`, JPA
- Configuration: `application.yml`, `application.properties`
- Profiles: `@Profile`, `spring.profiles.active`
- Dependency injection: `@Autowired`, constructor injection

### Maven
- POM: `pom.xml`
- Plugins: `maven-compiler-plugin`, `spring-boot-maven-plugin`
- Dependencies: `groupId`, `artifactId`, `version`

### Gradle
- Build: `build.gradle` or `build.gradle.kts`
- Dependencies: implementation, testImplementation
- Tasks: bootRun, bootJar, bootBuildImage

### Common File Locations
```
src/
├── main/
│   ├── java/
│   │   └── com/example/
│   │       ├── Application.java
│   │       ├── controller/
│   │       ├── service/
│   │       ├── repository/
│   │       ├── model/
│   │       └── config/
│   └── resources/
│       ├── application.yml
│       ├── application-*.yml
│       └── static/
└── test/
    └── java/
```

## Project Type Detection Signals
- `pom.xml` (Maven)
- `build.gradle` or `build.gradle.kts` (Gradle)
- `src/main/java/` directory
- `application.yml` or `application.properties`
- Spring Boot dependency in build file

## Agent Customizations

### Java-Specific
- Stream API patterns
- Optional usage
- Record types (Java 16+)
- Sealed classes (Java 17+)
- Pattern matching

### Spring Boot
- Bean lifecycle
- Transaction management
- Security configuration
- Actuator endpoints
- Profile-specific config

### Testing
- JUnit 5 annotations
- MockMvc for controller tests
- Testcontainers for integration tests
- Spring Boot Test slices

## Common Issues
- Dependency/version conflicts (classpath hell)
- Heap sizing / missing container-aware `-XX` flags
- Thread pool or connection pool exhaustion
- Swallowed checked exceptions
- JPA N+1 and lazy-loading surprises
- Mutable static state breaking tests

## Testing Patterns
- `mvn test` / `gradle test`
- JUnit 5 + Mockito for unit tests
- Spring Boot `@SpringBootTest` / MockMvc
- Testcontainers for DB/integration tests
- ArchUnit for dependency rules
