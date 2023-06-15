---
title: "An Introduction to Spring Boot: Simplifying Java Application Development"
author: Eric Rebadona
date: 2022-06-12 11:33:00 +0800
categories: [Blogging, Demo, Spring Boot]
tags: [springboot,spring]
pin: true
math: true
mermaid: true
image:
  path: /github/spring-boot-logo_kxhexe.png
  lqip: /github/spring-boot-logo_kxhexe.png
  alt: Spring Boot Logo
---


Spring Boot has emerged as a game-changer in the world of Java application development. Built on the foundation of the popular Spring Framework, Spring Boot offers a streamlined approach to building robust and scalable applications. In this blog post, we will explore what Spring and Spring Boot is, delve into its history, discuss its components, and examine why Spring Boot has become the preferred choice for developers worldwide. 

## What is Spring? 

Before diving into Spring Boot, it is crucial to grasp its underlying framework, Spring. Spring is a powerful framework that leverages the use of annotations to simplify and streamline the development process, allowing for more efficient and maintainable code.

### Understanding the Core Java Framework
Using regular JPA or JDBC often involves writing low-level code for tasks like connection management, query execution, and transaction handling. While JPA simplifies object-relational mapping, it still requires configuration and entity management. On the other hand, Spring provides dedicated modules like Spring Data JPA that abstract away the complexities, resulting in cleaner and more concise code. Using plain old JPA requires you to handle all the boilerplate code when creating a class that handles the CRUD operations.

```java 
//JPA Code example

import javax.persistence.EntityManager;
import javax.persistence.EntityManagerFactory;
import javax.persistence.Persistence;
import javax.persistence.Query;
import java.util.List;

public class UserDB {
    public static void main(String[] args) {
        EntityManagerFactory emf = Persistence.createEntityManagerFactory("myPersistenceUnit");
        EntityManager em = emf.createEntityManager();

        try {
            // Executing a JPQL query
            String jpql = "SELECT u FROM User u";
            Query query = em.createQuery(jpql);
            List<User> users = query.getResultList();

            // Processing the query result
            for (User user : users) {
                System.out.println(user.getName());
                // ... process the retrieved data
            }
        } finally {
            // Closing resources
            if (em != null) em.close();
            if (emf != null) emf.close();
        }
    }
}


```

With the implementation of the Spring framework, code for data access is reduced. Spring eliminates the bulk setup needed with JPA. So we don't need to handle all those pesky JPA persistence components!

```java
//Spring Data JPA code example

import org.springframework.data.jpa.repository.JpaRepository;

@Repository
public interface UserRepository extends JpaRepository<User, Long> {
    // No need to provide implementation for basic CRUD operations
    // Spring Data JPA generates the implementation at runtime
}

```

### Spring Beans, Dependencies, and Properties
At the heart of the Spring framework lies the concept of Spring Beans. Spring beans are similar to the Java beans we know of, but with the added power of the Spring IoC container, enabling easy configuration, dependency injection, and promoting loose coupling for flexible and reusable components within the application. 

Dependencies in Spring are like the external libraries we have imported into projects in the past or modules that your application relies on, defined in the project's configuration file.  Managed by build tools like Maven or Gradle, they include the Spring framework and other integrations like database drivers or logging frameworks, enabling your application to leverage their functionality. Any libraries missing from the project can be easily imported with the addition of dependencies.

Properties are used for configuring various aspects of the application. Defined in files like application.properties or application.yml, they specify settings such as database connections, logging levels, caching configurations, and more. Properties offer flexibility for customizing behavior without code modifications, accessed using Spring's property injection mechanism. They can be overridden or tailored for different environments to adapt to specific deployment scenarios.

### Spring Configuration Files

Similar to JPA, Spring provides classes for data access, model representation, controller logic, and service layers. These classes work together to achieve the Model-View-Controller (MVC) architecture, promoting separation of concerns and modularity. Spring includes additional files that we have not worked with previously. These files are used for configuration and include:

- `application.properties`: This file serves the purpose of configuring various properties specific to the Spring application. It includes details like database connection information, logging configuration, and other application-specific settings.

```properties
# Database Configuration
spring.datasource.url=jdbc:mysql://localhost:3306/mydatabase
spring.datasource.username=db_username
spring.datasource.password=db_password

# Logging Configuration
logging.level.root=INFO

# Application-specific Settings
app.jwtSecret=my_secret_key
app.fileUploadPath=/path/to/uploaded/files
```


- `pom.xml`: The Project Object Model (POM) file is an XML file that provides essential information about the project and its dependencies. It defines the necessary dependencies required by the Spring application, such as the Spring Framework, Spring Boot, and other relevant libraries or modules.

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.example</groupId>
    <artifactId>spring-app</artifactId>
    <version>1.0.0</version>

    <properties>
        <spring.version>5.3.8</spring.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <!-- Dependencies would go here -->
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
</project>
```


- `Spring configuration files`: These files can be either XML or Java-based configuration files. They play a vital role in defining beans, their dependencies, and other relevant settings for the Spring application. Examples of such files include `applicationContext.xml` or `MyAppConfig.java`, where you define the Beans and establish their relationships.

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!-- Define beans and their relationships here -->
    <bean id="userService" class="com.example.UserService">
        <!-- Dependencies and properties for Service bean -->
        <property name="userRepository" ref="userRepository"/>
        <!-- Other properties as needed -->
    </bean>

    <bean id="userRepository" class="com.example.UserRepository">
        <!-- Additional dependencies and properties for userRepository bean -->
    </bean>

</beans>
```

### Leveraging Annotations and Conventions
One of the key differentiators of Spring is its extensive use of annotations. With Spring, developers can annotate classes and methods to define database operations, eliminating the need for manual configuration files. For instance, by simply annotating a method with `@Transactional`, Spring takes care of transaction management behind the scenes, reducing boilerplate code and enhancing productivity. Below is just a small fraction of the annotations available within the Spring Framework:

- `@Component`: Indicates that a class is a component and allows it to be detected and registered as a Spring bean.

- `@Controller`: Marks a class as a controller in the MVC pattern, used for handling web requests.

- `@RestController`: Combines @Controller and @ResponseBody annotations to create RESTful web services.

- `@Service`: Indicates that a class is a service component that provides business logic and is typically used in the service layer.

- `@Repository`: Marks a class as a repository component, used for database operations. It enables exception translation and other repository-specific features.

- `@Async`: Marks a method as asynchronous, allowing it to be executed in a separate thread.




### The Benefits of Spring
Let's explore the key benefits of Spring by examining the functionalities it replaces in Java code:

-	Manual object instantiation: Spring replaces manual object creation with dependency injection and its IoC container. Adding the `@Component` annotation above, Spring will handle instantiation and inject the instance of the dependency. 

```java
@Component
public class MyService {
    // Dependency injection via constructor
    private final MyDependency myDependency;

    public MyService(MyDependency myDependency) {
        this.myDependency = myDependency;
    }
}

```
-	Connection and transaction management: Spring simplifies managing database connections and transactions, particularly through its ORM tool, such as Spring Data JPA. Annotating with `@Transactional` allows Spring to manage transactional behaviour within the class

```java
@Service
@Transactional
public class MyService {
    private final MyRepository myRepository;

    public MyService(MyRepository myRepository) {
        this.myRepository = myRepository;
    }

    public void saveData(MyData data) {
        myRepository.save(data);
    }
}
```

-	Cross-cutting concerns: Spring simplifies the implementation of logging, security, and transaction management with reusable components.

-	Configuration management: Spring offers a comprehensive solution for externalizing and modifying configuration details without code changes. The `@Configuration` annotation helps define a class and that provides bean definitions and other configuration details. and the `@Value` annotation allows you to inject values from external configuration files. 

```java 
@Configuration
public class MyConfig {
    @Value("${myapp.property}")
    private String myProperty;

    // Bean definitions and other configuration methods
}
```

-	Web development complexities: Spring provides a high-level abstraction for web development, removing the need for low-level details like servlet management.

-	Threading and concurrency: Spring provides robust support for concurrent programming, including task scheduling, thread pooling, and asynchronous execution.	

However, setting up a Spring application traditionally required configuring numerous components, which could be a time-consuming and tedious process. Developers have to manage XML configurations and define various beans-to-wire dependencies, often leading to an abundance of its own boilerplate code and increased complexity. This is where Spring Boot comes in!

## What is Spring Boot?

While Spring itself can enhance development speed, Spring Boot takes it a step further by offering additional features and conventions that accelerate application development.

### History and Evolution of Spring Boot
The history of Spring Boot is marked by its remarkable evolution and innovative contributions to the world of Java frameworks. Introduced in 2013 by the Spring team at Pivotal Software. Spring Boot emerged as an open-source project with a mission to simplify the Spring development process. 

The motivation behind Spring Boot was to eliminate boilerplate configurations required for setting up Spring and address the complex XML configurations traditionally encountered in Spring applications. By providing a simpler and more opinionated approach to Spring development, Spring Boot quickly gained traction among developers worldwide. The continuous advancements and community-driven nature of Spring Boot have contributed to its position as a leading framework in the Java ecosystem, enabling developers to build robust and scalable applications with ease. 

### Simplifying Spring Java Application Development
Spring Boot is an extension of the Spring framework that simplifies the process of creating and deploying Spring applications. It embraces the "convention over configuration" principle, allowing developers to focus on writing business logic rather than dealing with infrastructure setup. Spring Boot's streamlined development experience, convention-over-configuration approach, and support for microservices architecture make it a framework that inherently reflects the principles of Agile development.

> Convention over configuration is a software design paradigm that emphasizes the use of sensible defaults and predefined conventions to minimize the need for explicit configuration. In the context of Spring Boot, this means that developers can get started quickly with minimal setup and configuration. 
{: .prompt-info }

By following a set of well-established conventions, Spring Boot eliminates the need for extensive XML configurations that were traditionally associated with Spring applications. Instead, it provides a powerful set of default configurations and sensible assumptions based on the dependencies and libraries included in the application's classpath.

## Who uses Spring Boot?
### Industry Adoption
Many notable companies and organizations, including Netflix, Alibaba, Intuit, Accenture, and Ticketmaster, have adopted Spring Boot as their framework of choice for building Java applications. The power of Spring Boot has attracted businesses operating in various industries, ranging from e-commerce to finance and entertainment. Spring Boot's robustness, scalability, and ease of development have made it a go-to solution for building enterprise-grade applications at scale. With widespread adoption and the backing of a strong community, Spring Boot has established itself as a trusted framework for delivering high-quality and efficient applications.

## Why Spring Boot?
### The Advantages
Spring Boot surpasses traditional Spring by offering streamlined development experience, eliminating excessive configuration through sensible defaults and auto-configuration. It includes an embedded server for easy deployment, seamless integration with the Spring ecosystem, and promotes best practices. With its emphasis on productivity and scalability, Spring Boot has become the preferred framework for building modern Java applications.

Another advantage of Spring Boot is its seamless integration with other Spring projects. Whether it's incorporating Spring Security for authentication and authorization or Spring Integration for messaging and event-driven architectures, Spring Boot provides easy integration and enhances the capabilities of the overall Spring ecosystem.

## How Spring Boot Works
Now that we have explored the advantages of Spring Boot, let's delve into its inner workings. Spring Boot operates in a distinct manner compared to the conventional Java code that we have been working with throughout our program. 

### Streamlining Application Development with Key Components
At the core of Spring Boot are its key components that work together to provide efficient and easy application development. These components are managed and imported through the project's build configuration file. Let's explore these components in a more coherent manner:

-	Starters: Spring Boot starters are pre-configured dependencies that simplify application setup. They include necessary dependencies and configurations for common tasks like web development, database integration, security, and more. By using starters, developers can bypass manual configuration and concentrate on writing application-specific code.

```xml 
<!-- Add the Spring Boot Web Starter -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>

<!-- Add the Spring Boot Data JPA Starter -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
```

-	Spring Boot Actuator: A production-ready suite of features for monitoring and managing applications. It exposes endpoints for runtime insights, health checks, metrics, logging, and more. 

-	Spring Boot DevTools: Enhances developer productivity with automatic application restarts and live reload functionality. It speeds up the development cycle by eliminating the need to manually stop and start the application after code changes. It also improves error reporting for easier issue diagnosis and resolution.

-	Spring Boot Data: Simplifies database interactions by providing a consistent and intuitive API. It leverages Spring Data to offer unified access to various database technologies, including relational and NoSQL databases. It reduces boilerplate code, generates queries based on method names, and provides convenient features like pagination, caching, and transaction management.

While the components above may seem like a lot, Spring Boot offers a tool that can create all the inital setup, this tool is called the "Spring Initializr". By leveraging these components, Spring Boot allows developers to rapidly build robust and scalable applications with minimal configuration and maximum productivity. It empowers developers to focus on writing business logic, while the framework takes care of common infrastructure concerns, resulting in accelerated development cycles and reduced time to market.

## Conclusion
Spring Boot has simplified Java application development by providing sensible defaults, reducing configuration complexity, and offering a comprehensive toolset. Its convention over configuration approach, coupled with seamless integration with other Spring projects, enhances developer productivity and accelerates development cycles. With widespread adoption and a strong community, Spring Boot has become a trusted framework for building robust and scalable applications in various industries. Its key components, including starters, Actuator, DevTools, and Spring Boot Data, work together to streamline application development and minimize boilerplate code. Overall, Spring Boot empowers developers to focus on writing business logic and delivers efficient solutions for building high-quality applications.

In the next blog, we will jump right into the setup of Spring Boot, learn how to use the Spring Boot provided Spring Initializr, and take a look at a simple project envolving a To-Do list Spring application similar to one we worked with last semester. 







## Sources
Spring Boot: <https://spring.io/projects/spring-boot#learn>

A Comparison Between Spring and Spring Boot: <https://www.baeldung.com/spring-vs-spring-boot>

Spring Framework - Overview: <https://www.tutorialspoint.com/spring/spring_overview.htm>

Spring Tutorial: <https://www.javatpoint.com/spring-tutorial>





