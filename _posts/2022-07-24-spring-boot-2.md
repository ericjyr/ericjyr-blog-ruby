---
title: "Mastering Spring Boot: From Installation to Building a To-Do List Application"
author: Eric Rebadona
date: 2022-07-24 11:00:00 +0800
categories: [Blogging, Demo, Spring Boot]
tags: [springboot,spring]
pin: true
math: true
mermaid: true
image:
  path: /github/spring_wtzqse.png
  lqip: /github/spring_wtzqse.png
  alt: Spring Boot Logo with Nice Lady
---


In the first blog, we explored the rich history and fundamental concepts of Spring and Spring Boot. Now, let's take a step further into the world of Spring Boot development by diving into the practical aspects. In this blog, we will cover what software we will use for the tutorial, understanding the Spring Boot Initializer, and finally, creating a fully functional to-do list application using Spring Boot.

To-Do List Project Source Code: <https://drive.google.com/file/d/1yLRBh5rtd_FKEOu31-Q5J-0HlbrRCnOV/view?usp=sharing> 


# The To-Do List Project

This project will reflect on the labs in the Web Application course we completed in the previous semester. Like the labs from the previous semester, this project will use an MVC architecture. 

![](/github/src_pr9ief.jpg)


With this application we will be able to:

- View existing tasks within a database.
    ![](/github/proj1_vuu15t.jpg)

- Create new tasks.
    ![](/github/proj2_nul3fi.jpg)

- Edit existing tasks.
    ![](/github/proj3_n6i8yr.jpg)


- Delete unneeded tasks. 

## Software Requirements 
Spring Boot is a versatile framework that can seamlessly work with different Integrated Development Environments (IDEs) and databases. Developers have the freedom to choose popular IDEs such as IntelliJ IDEA, Eclipse, Visual Studio Code, or even use basic text editors for coding. As for databases, Spring Boot provides native support for various options, including MySQL, PostgreSQL, H2 Database, MongoDB, and Redis. This flexibility empowers developers to select their preferred development environment and the most suitable database technology based on their project requirements without sacrificing the smooth and unified development experience that Spring Boot offers.

For this demonstration we will be using IntelliJ IDEA 2022.2.3 which comes with Maven built in. We will also be using MySQL 5.7.32, the database system and version that we used in our ITSC-315 Security for Software Developers course. 


### Setting up the Database
Before we jump into Spring Boot, let us create a database for our project. We can keep all the current settings we have and use our existing MySQL connections from our prior classes. What we are doing here is creating a fresh database for our project. Follow the steps below to complete this:

1. In the Windows Search box find the MySQL Workbench application and open. 

    ![](/github/mysqldb01_ruzwtq.jpg){: width="auto" .w-100 .left}

2. Start/Open the existing instance. 

    ![](/github/mysqldb02_adhm6y.jpg){: width="auto" .w-100 .left}

3. On the top icon menu press the add database button. 

    ![](/github/mysqldb1_mdedyx.jpg){: width="auto" .w-100 .left}

4. For the name enter todoDB and press apply. 

    ![](/github/mysqldb2_umhs7i.jpg){: width="auto" .w-100 .left}

Now that we created our database, let's move onto the Spring Boot Initializr! 





## Understanding Spring Boot Initializr

The Spring Boot Initializr is a powerful web-based tool that simplifies project setup by generating the initial configuration for your Spring Boot applications. In this section, we'll explore the various options available in the Initializr and how to use it effectively to kickstart your projects. The Spring Boot Initializr web tool can be accessed through your browser using the following link <https://start.spring.io/>

![img-description 1](/github/initializr1_iuagdu.jpg)

For our Project we will check off Maven for our project management tool and we will be using Spring Boot 3.0.9. 

![img-description 2](/github/initializr2_chhch3.jpg)


Next, we will complete the Project Metadata. In Maven, the "group" identifies the project's organization, and the "artifact" is the unique identifier for the project or module. The "name" gives a human-readable title, and the "description" provides an overview of the project's purpose, while the "package name" is used to organize the Java source files, like "com.example.demo" in the Spring Boot demo project. For the Java our project we will be working with Java 8. 

![img-description 3](/github/initializr33_gdprhs.jpg)

In the previous blog we were introduced to dependencies which are external libraries or modules that our project relies on to implement specific functionality. Spring Boot, being an opinionated framework, makes it incredibly easy to add dependencies using the Spring Boot Initializr.

To build a robust to-do list application, we will utilize the following Spring Boot dependencies:

1. `Spring Web`: This dependency enables the development of web applications with ease, providing HTTP request handling and serving web content.
2. `H2 Database`: H2 is a lightweight, in-memory database that allows us to persist and manage our to-do list data efficiently. We will be working with this rather than incorporating MySQL or MariaDB for this demonstration. 
3. `Spring Data JPA`: With this dependency, we can seamlessly integrate JPA (Java Persistence API) to interact with our database using Spring Data's powerful and simplified data access mechanisms.
4. `Spring Boot Web Tools`: Spring Boot Web Tools provide additional development support for web applications. They include features like live reloading, hot swapping, and enhanced IDE integration, making the development process more efficient and productive.

![img-description 4](/github/dependency2_rkvvat.jpg)

By adding these dependencies via the Spring Boot Initializr, we'll save significant development time and have a solid foundation for our to-do list application. After configuring the details of the project, Tap GENERATE on the bottom of the web page to download a zip file of the project and unzip the file to your IDEs workspace. 

Let's get started with creating our Spring Boot to-do list application and unlock the full potential of this powerful framework!

## Creating a To-Do List Application Using Spring Boot
Now comes the exciting part - building our own to-do list application with the magic of Spring Boot. We'll break this process down into several steps to ensure a clear understanding of each stage.

### Importing our Project Skeleton
Upon opening up your Intellij IDE, import the project we created from the Initializr. To import our newly generated project:
bun
1. Under the File menu create a new project from an existing source. 

    ![img-description 4](/github/importing1_zffb8t.jpg){: width="auto" .w-100 .left}

2. We want to make sure we import our project as a Maven project

    ![img-description 4](/github/importing2_guakkf.jpg){: width="auto" .w-100 .left}




### What's in our Project Skeleton?  

Below is the directories and files that the Spring Boot Initializr generated, this is an example of how Spring Boot can really kickstart your project. 

![tree 1](/github/tree1_kjeaqr.jpg)

We did mention some of these files in a prior blog, but it's always helpful to refresh our memory and keep these fundamental components in mind as we progress further. As a friendly reminder, let's revisit and reinforce our understanding of these essential elements! Below are some of the key files and their corresponding definitions:

- `pom.xml`: The "pom.xml" file is an XML configuration file used by Apache Maven. It defines the project's metadata, such as the project name, version, description, and the project's dependencies on external libraries (JARs). It also contains information about the build process, plugins, and other project-related configurations.
- `HELP.md`: "HELP.md" is a valuable resource providing essential links to documentation and insightful guides that offer guidance and support in navigating the intricacies of Spring Boot.
- `TodoApplication.java`: The "TodoApplication.java" file is the main Java class that serves as the entry point for our Spring Boot application. It contains the main method, responsible for bootstrapping the Spring Boot application context and initiating the application's execution.
- `application.properties`: The "application.properties" file is a configuration file for Spring Boot that allows developers to specify various application settings. This includes properties related to database connections, logging configurations, server ports, and other customizable parameters that define the behavior of the Spring Boot application. It plays a crucial role in fine-tuning the application's functionality according to specific requirements. This file is by default empty. 
- `templates folder`: Here we will keep our HTML files that we will use for this example. 

To build our project, we will configure the application.properties file and manually create packages for our Controller, Model, Repository, and Service classes. 




### Configuring application.properties file

Now we will specify the connections to our database. Let's delve into the essential configurations within the "application.properties" file:

```bash
    # DataSource Configuration for MySQL Database
    spring.datasource.url=jdbc:mysql://localhost:3306/todoDB
    spring.datasource.username=root
    spring.datasource.password=password
    spring.datasource.driverClassName=com.mysql.cj.jdbc.Driver

    # JPA and Hibernate Configuration
    spring.jpa.database-platform=org.hibernate.dialect.MySQLDialect
    spring.jpa.hibernate.ddl-auto=update
    spring.jpa.show-sql=true

    # Thymeleaf Configuration
    spring.thymeleaf.cache=false
```
{: file='todo\src\main\resources\application.properties'}

1. `spring.datasource.url`: With this property we define the URL for connecting to the MySQL database server on the localhost, using port 3306, and we will be accessing the database we created "todoDB".

2. `spring.datasource.username`: With this property, we set the username required for authentication when connecting to the MySQL database. For mine I just used the default root user. 

3. `spring.datasource.password`: Here, we specify the password associated with the provided username for successful authentication during MySQL database connection.

4. `spring.datasource.driverClassName`: This property designates the fully qualified class name of the MySQL JDBC driver, which Spring Boot will use to establish database connections.

5. `spring.jpa.database-platform`: By configuring this property, we set the database dialect used by Hibernate. In this case, we choose the MySQL-specific SQL dialect for Hibernate's queries and schema generation.

6. `spring.jpa.hibernate.ddl-auto`: With this property set to "update," Hibernate automatically updates the schema based on the entity model, enabling seamless schema management.

7. `spring.jpa.show-sql`: When this property is enabled, Hibernate logs the SQL queries executed, proving valuable during debugging and understanding the application's database interactions.

8. `spring.thymeleaf.cache`: By setting this property to "false," we disable Thymeleaf caching, ensuring that templates are reloaded on every request. This behavior is particularly beneficial for development environments.

By configuring these properties thoughtfully, we lay a solid foundation for our Spring Boot application, empowering it to efficiently handle database connections, manage schema updates, optimize template rendering, and provide valuable insights into SQL query execution for a productive and smooth development experience.




### Testing the Database Connection 

Now that we made configurations to connect our database let's run the main method, TodoApplication.java. Before we move further into the project, we want to make sure we have a working connection. Run the main method and it should start without any issues. 

![testdb1](/github/testdb1_cixy3e.jpg)
![testdb2](/github/testdb2_tbjp1x.jpg)

If everything is successful, a Tomcat server will start. Stop the server for now and we we will now create our Java classes. 





### The Model Class

The first class we will create is the problem domain or the model class. We will just be making a single class for this project. Right click the root package folder in java and under "New" menu press Java Class. 

![models1](/github/model1_zrdpct.jpg)

Create a new class with `models` for the package name and `TodoItem` class name. 

![models2](/github/model2_ctc9cb.jpg)

The TodoItem class will represent a model for a TodoItem in the application. This class will implement the Serializable interface, which allows objects of this class to be converted into a byte stream for storage or transmission. We will also be working with the Instant class which is just a timestamp class in the java.time package. 

For our TodoItem, we will created private fields for the following attributes:

- `id (Long)`: This field represents the unique identifier for each TodoItem. It is of type Long to store large numeric values.

- `description (String)`: This field represents the description or content of the TodoItem. It is of type String to store textual information.

- `isComplete (Boolean)`: This field indicates whether the TodoItem is complete or not. It is of type Boolean, and it can have a value of true (completed) or false (not completed).

- `createdAt (Instant)`: This field stores the timestamp when the TodoItem was created. It is of type Instant from the java.time package, which represents a point in time.

- `updatedAt (Instant)`: This field stores the timestamp when the TodoItem was last updated. It is also of type Instant.

By creating these private fields, we encapsulate the data and define the attributes that a TodoItem can have. The access to these fields is restricted to methods within the class itself, and they can be accessed using getter and setter methods or through other class methods.

```java
package com.ericjyr.tut.todo.models;

import java.io.Serializable;
import java.time.Instant;

public class TodoItem implements Serializable {


    private Long id;
    private String description;
    private Boolean isComplete;
    private Instant createdAt;
    private Instant updatedAt;


}
```
{: file='models\TodoItem.java'}

Now that we have our foundation for the class down, we can add the annotations that will provide metadata that Spring Boot will use. As mentioned in the previous blog, Spring Boot uses annotations for many reasons including dependency injection and ensuring that the application prioritizes Convention over Configuration. As we add annotations, Intellij will automatically import statements for each annotation. We will also create our own instance of the toString method to display our Todo items in our logs. 

Let's go through each annotation and the method:

- `@Id`: This annotation is from the javax.persistence package and is used to indicate that the id field is the primary key of the entity (i.e., the unique identifier for each TodoItem).

- `@GeneratedValue`: This annotation specifies the strategy for generating values for the primary key. In this case, GenerationType.AUTO means that the database will automatically generate the primary key value.

- `@NotBlank`: This annotation is from the javax.validation.constraints package and is used to indicate that the description field must not be blank (i.e., it should have some content). This is a validation constraint, and if the description is blank when saving a TodoItem, a validation error will be thrown.

- `@Entity`: This annotation indicates that the TodoItem class is an entity and should be mapped to a database table. It is part of the Java Persistence API (JPA) and is used for Object-Relational Mapping (ORM).

- `@Table`: This annotation specifies the name of the database table to which the TodoItem entity is mapped. In this case, we will specify the table name as "todo_items".

- `@Getter` and `@Setter`: These annotations generate the getter and setter methods for the private fields automatically. These annotations help to reduce boilerplate code, making the class more concise. We won't need to make any code for getters and setters!

The code below is the enhanced version of the TodoItem class, with added annotations for JPA mapping and validation, as well as an overridden toString() method for better representation of the object as a string.

```java
package com.ericjyr.tut.todo.models;

import jakarta.persistence.*;
import jakarta.validation.constraints.NotBlank;
import lombok.Getter;
import lombok.Setter;

import java.io.Serializable;
import java.time.Instant;

@Getter
@Setter
@Entity
@Table(name = "todo_items")
public class TodoItem implements Serializable {

    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    private Long id;
    @NotBlank(message = "Description is required")
    private String description;
    private Boolean isComplete;
    private Instant createdAt;
    private Instant updatedAt;

    @Override
    public String toString() {
        return String.format("TodoItem{id=%d, description='%s', isComplete='%s', createdAt='%s', updatedAt='%s'}",
                id, description, isComplete, createdAt, updatedAt);
    }

}
```
{: file='models\TodoItem.java'}





### The Repository class
Now that we created the TodoItem class, we need a way to define the interface between our application and the database. To do this, we are going to make a Repository class for TodoItem. This class is similar to the data access classes from our Web Application course minus all the boilerplate code. Let's make a new package named `repositories` and an interface named `TodoItemRepository`. 

![repo1](/github/repo1_t0raoh.jpg)

For our repository interface we will make it extend JpaRepository with our model class `TodoRepository` and it's primary key's type `Long` for it's parameters.

```java
package com.ericjyr.tut.todo.repositories;

import com.ericjyr.tut.todo.models.TodoItem;
import org.springframework.data.jpa.repository.JpaRepository;

public interface TodoItemRepository extends JpaRepository<TodoItem, Long> { }
```
{: file='repositories\TodoItemRepository.java'}

Next, we'll add the `@Repository` annotation to mark the interface as a repository bean, this is to allow Spring Boot to identify that the interface is a Spring Data repository which will allow for features such as CRUD methods. 

```java
package com.ericjyr.tut.todo.repositories;

import com.ericjyr.tut.todo.models.TodoItem;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

@Repository
public interface TodoItemRepository extends JpaRepository<TodoItem, Long> { }
```
{: file='repositories\TodoItemRepository.java'}





### The Service Class

With the repository class completed, we need to create a service class that will communicate with the repository class. The service class acts as an intermediary between the controller and repository classes, so when we want to add or get a TodoItem from the database we will be working through this class. Just like the service classes we worked with in the Web Application course, we will apply all the business logic here.

Let's create another package `services` and a class `TodoItemService`.

![service1](/github/controller1_qzwprz.jpg)

Firstly, we will add the `@Service` annotation so Spring Boot can identify it as a service class. 

```java 
package com.ericjyr.tut.todo.services;

import org.springframework.stereotype.Service;

@Service
public class TodoItemService {

}
```
{: file='services\TodoItemService.java'}



Next, we need to pull in our TodoItemRepository class using the `@Autowired` annotation, this is dependency injection. Using the @Autowired annotation, the TodoItemRepository is injected into the TodoItemService. This means that Spring will automatically provide an instance of TodoItemRepository when creating an instance of TodoItemService.


```java 
package com.ericjyr.tut.todo.services;

import com.ericjyr.tut.todo.repositories.TodoItemRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

@Service
public class TodoItemService {

    @Autowired
    private TodoItemRepository todoItemRepository;

}
```
{: file='services\TodoItemService.java'}


Finally, lets add all the service methods. These methods will be used to perform DDL and DQL methods in the database. 

```java 
package com.ericjyr.tut.todo.services;

import com.ericjyr.tut.todo.models.TodoItem;
import com.ericjyr.tut.todo.repositories.TodoItemRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.time.Instant;
import java.util.Optional;

@Service
public class TodoItemService {

    @Autowired
    private TodoItemRepository todoItemRepository;

    public Optional<TodoItem> getById(Long id) {
        return todoItemRepository.findById(id);
    }

    public Iterable<TodoItem> getAll() {
        return todoItemRepository.findAll();
    }

    public TodoItem save(TodoItem todoItem) {
        if (todoItem.getId() == null) {
            todoItem.setCreatedAt(Instant.now());
        }
        todoItem.setUpdatedAt(Instant.now());
        return todoItemRepository.save(todoItem);
    }

    public void delete(TodoItem todoItem) {
        todoItemRepository.delete(todoItem);
    }

}
```
{: file='services\TodoItemService.java'}


### The Controller Class

For our last Java class we will be creating two different controller classes. Like servlets, these Spring controller classes are responsible for the HTTP requests and responses. But, Spring Boot Controllers provide a higher-level abstraction, annotation-based configuration, and dependency injection compared to traditional Servlets, simplifying web application development.

To advance our application development, we will create two Controller classes: `HomeController` and `TodoFormController`. These controllers play crucial roles in managing HTTP requests and coordinating with the TodoItemService for data processing. 

![controller1](/github/controller1_fq6lmg.jpg)

Let's understand the functions of our controller classes:

1. **HomeController:**
   - Handles requests for the homepage ("/") and presents a list of `TodoItems`.
   - Communicates with the `TodoItemService` to retrieve the necessary data for display.
   - Renders the "index" view, showing all `TodoItems`, providing users an overview of their tasks.

2. **TodoFormController:**
   - Manages requests related to CRUD (Create, Read, Update, Delete) operations on `TodoItems`.
   - Handles the creation of new `TodoItems` through form submissions.
   - Processes updates to existing `TodoItems` and ensures they are saved correctly.
   - Facilitates the deletion of `TodoItems` from the database.
   - Displays forms for creating, updating, and deleting `TodoItems`, enabling smooth user interaction.
   - Communicates with the `TodoItemService` to process and persist user-entered data.

By splitting these functionalities into separate Controller classes, we maintain a well-organized and structured application architecture, following the principles of the Model-View-Controller (MVC) design pattern. This approach enhances code readability, simplifies maintenance, and fosters a clear separation of concerns within our application.

Rather than doGet and doPost, in Spring MVC, we use the `@GetMapping` and `@PostMapping` annotations to map HTTP GET and POST requests, respectively, to specific methods in a controller class. These annotations provide a more convenient and expressive way to handle HTTP requests and seamlessly integrate with the Spring framework, simplifying the development process for handling different types of HTTP requests. Additionally, they promote better code organization and readability, making it easier to distinguish between methods handling different types of requests within the controller.

The Java code for both controller classes can be found below. 

```java
package com.ericjyr.tut.todo.controllers;

import org.springframework.stereotype.Controller;
import com.ericjyr.tut.todo.services.TodoItemService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.servlet.ModelAndView;

@Controller
public class HomeController {

    @Autowired
    private TodoItemService todoItemService;

    @GetMapping("/")
    public ModelAndView index() {
        ModelAndView modelAndView = new ModelAndView("index");
        modelAndView.addObject("todoItems", todoItemService.getAll());
        return modelAndView;
    }

}
```
{: file='controllers\HomeController.java'}



```java
package com.ericjyr.tut.todo.controllers;

import org.springframework.beans.factory.annotation.Autowired;
import com.ericjyr.tut.todo.services.TodoItemService;
import org.springframework.web.bind.annotation.*;
import org.springframework.stereotype.Controller;
import com.ericjyr.tut.todo.models.TodoItem;
import jakarta.validation.Valid;
import org.springframework.ui.Model;
import org.springframework.validation.BindingResult;

@Controller
public class TodoFormController {

    @Autowired
    private TodoItemService todoItemService;

    @GetMapping("/create-todo")
    public String showCreateForm(TodoItem todoItem) {
        return "new-todo-item";
    }

    @PostMapping("/todo")
    public String createTodoItem(@Valid TodoItem todoItem, BindingResult result, Model model) {

        TodoItem item = new TodoItem();
        item.setDescription(todoItem.getDescription());
        item.setIsComplete(todoItem.getIsComplete());

        todoItemService.save(todoItem);
        return "redirect:/";
    }

    @GetMapping("/delete/{id}")
    public String deleteTodoItem(@PathVariable("id") Long id, Model model) {
        TodoItem todoItem = todoItemService
                .getById(id)
                .orElseThrow(() -> new IllegalArgumentException("TodoItem id: " + id + " not found"));

        todoItemService.delete(todoItem);
        return "redirect:/";
    }

    @GetMapping("/edit/{id}")
    public String showUpdateForm(@PathVariable("id") Long id, Model model) {
        TodoItem todoItem = todoItemService
                .getById(id)
                .orElseThrow(() -> new IllegalArgumentException("TodoItem id: " + id + " not found"));

        model.addAttribute("todo", todoItem);
        return "edit-todo-item";
    }

    @PostMapping("/todo/{id}")
    public String updateTodoItem(@PathVariable("id") Long id, @Valid TodoItem todoItem, BindingResult result, Model model) {

        TodoItem item = todoItemService
                .getById(id)
                .orElseThrow(() -> new IllegalArgumentException("TodoItem id: " + id + " not found"));

        item.setIsComplete(todoItem.getIsComplete());
        item.setDescription(todoItem.getDescription());

        todoItemService.save(item);

        return "redirect:/";
    }
}

```
{: file='controllers\TodoFormController.java'}





### Implementing the User Interface (UI):
A visually appealing and user-friendly UI is crucial for any application's success. We'll use Thymeleaf, a powerful template engine, to design and implement the front-end of our to-do list application. This is not the focus of this blog, the three HTML files that will be needed for this tutorial can be found in the source code provided at the beginning of the blog. After retrieving the HTML code it will be placed in the templates folder found in `\src\main\resources`. 

![](/github/temp1_pffkdh.jpg)

After we add the html files to the resources folder, let's run the main method once again and take a look at our end project! When we run the driver class, we will start a Tomcat server and we can access our project through the browser of our local machine. We can confirm this in the IDEs console. Below, we see that Tomcat started on port 8080.

![](/github/tom1_ny0up0.jpg)

To access the project open your browser enter localhost:8080 for your URL. 

![](/github/host1_g9cc8z.jpg)


## Conclusion
Congratulations! You've successfully completed the journey from creating a project skeleton using Spring Boot Initializr to building a functional to-do list application using Spring Boot. In this blog, we covered the fundamentals of the Spring Boot Initializer, the essentials of designing a data model, the importance of a user interface, implementing business logic, and adding security to our application. Armed with this knowledge, you are now ready to take on more ambitious Spring Boot projects and explore the endless possibilities this powerful framework has to offer. Happy coding!






