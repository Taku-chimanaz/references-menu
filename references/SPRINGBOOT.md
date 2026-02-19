# SPRINGBOOT REFERENCE

![SPRINGBOOT logo](./../images/springboot.webp)

I have created this reference document to help me reference or recall concepts I have learnt in SpringBoot.Please note that this list only reflect on concepts that I have learnt and does not exhaust all the conceptes.It can also serve as a starting point for anyone who wants to learn about Springboot but do not know where to begin.Hope this helps.

## Fundamentals

### What is Spring Framework

It is an open source framework for building enterprise Java Application.Its aim is to simplify the complex enterprise Java application development.It is also lightweight.

### Core Features of Spring Framework

- IOC (Inversion of Control) - It is the principle of how objects are managed and created in your application.Instead of you providing the dependencies you need the framework(Spring container) does that for you.The Spring does this by implementing Dependency Injection.
- Aspect Oriented Programming - It allows you to separate cross-cutting concerns from your business logic.This prevents scattering of functionality across the application.
- Data Access Framework
- MVC Framework

### What is a SPRING BEAN

Spring Bean is an object that manages by the Spring Framework in a Java application.

### @Configuration Example

This annotation marks a class as a configuration class.The Class should be public and non-final\
In the class you can also use **_@Bean_** annotation to declare a bean object

```java
@Configuration
public class AppConfig {

    @Bean
    public PaymentService paymentService(AccountRepository accountRepository){
        return new PaymentService(accountRepository);
    }

    @Bean
    public AccountRepository accountRepository() {
        return new AccountRepository(dataSource());
    }

    @Bean
    public DataSource dataSource(){
        return (...)
    }
}
```

### @Component Example

This annotation is used to make a class as a Spring Component.\
There are other sub types that allows further refinement of the @Component annotation and these are: @Repository,@Service and @Controller.\
@Component as a general component annotation indicating that the class should be initialized.configured and managed by the core container

```java
@Component
public class PaymentServiceImpl {

    private final AccountRepository accountRepository;

    // Autowired is unnecessary if there is only one constructor
    @Autowired
    public PaymentServiceImpl(AccountRepositor accountRepository){
        this.accountRepository = accountRepository
    }
}
```

### Bean Naming

```java
@Configuration
public class AppConfig {

    @Bean
    public PaymentService paymentService(AccountRepository accountRepository){
        return new PaymentService(accountRepository);
    }

    @Bean
    public AccountRepository accountRepository() {
        return new AccountRepository(dataSource());
    }

    @Bean("ds")
    public DataSource dataSource(){
        return (...)
    }
}
```

- For the first 2 beans the name of the beans is the same as the the name of the method since we did not provide one.
- For the last bean the name is "ds".This can be used to retrieve this bean.

### Dependency Injection

There are different types as follows:

#### Constructor Injection

```java
@Service
public class DefaultPaymentService {

    private final AccountRepository accountRepository;

    public DefaultPaymentService(AccountRepository accountRepository){
        this.accountRepository = accountRepository
    }
}
```

#### @Qualifier

It is used to specify which bean to inject
In this case we have 2 beans with the same type so we give Spring will not know which one to inject.\
In this case we have provided "primary" qualifier so that the first bean takes priority and the second bean takes the secondary priority.

```java
@Configuration
public class AppConfig {

    @Bean
    @Qualifier("primary")
    public PaymentService paymentService(AccountRepository accountRepository){
        return new PaymentService(accountRepository);
    }

    @Bean
     @Qualifier("secondary")
    public AccountRepository accountRepository() {
        return new AccountRepository(dataSource());
    }
}
 // another file
@Service
public class DefaultPaymentService {

    private final AccountRepository accountRepository;

    public DefaultPaymentService(Qualifier("primary") AccountRepository accountRepository){
        this.accountRepository = accountRepository
    }
}

```

**_The @Primary annotation can also be used to mark a bean as primary_**

```java
@Configuration
public class AppConfig {

    @Bean
    @Primary
    public PaymentService paymentService(AccountRepository accountRepository){
        return new PaymentService(accountRepository);
    }

    @Bean
    public AccountRepository accountRepository() {
        return new AccountRepository(dataSource());
    }
}
 // another file
@Service
public class DefaultPaymentService {

    private final AccountRepository accountRepository;

    public DefaultPaymentService(Qualifier("primary") AccountRepository accountRepository){
        this.accountRepository = accountRepository
    }
}

```

#### Field Injection

Field injection allows a dependency to be injected in the field with using a constructor or method.\
@Autowired indicated that you want to do field injection.
**Please note: This is not a good practice.Spring advocates for the constructor injection**

```java
@Service
public class DefaultPaymentService {

    @Autowired
    private final AccountRepository accountRepository;

    public DefaultPaymentService(Qualifier("primary") AccountRepository accountRepository){
        this.accountRepository = accountRepository
    }
}
```

#### Setter Injection

@Autowired allow Spring to look for a matching bean in the application context and inject it.\
**Please note: This is not a good practice.Spring advocates for the constructor injection**

```java
@Service
public class DefaultPaymentService {

    @Autowired
    public setAccount(AccountRepository accountRepository){
        this.accountRepository = accountRepository
    }
}
```

#### Method Injection

@Autowired allow Spring to look for a matching bean in the application context and inject it.\
**Please note: This is not a good practice.Spring advocates for the constructor injection**

```java
@Service
public class DefaultPaymentService {

    @Autowired
    public configure(AccountRepository accountRepository){
        this.accountRepository = accountRepository
    }
}
```

### Bean Scope

This refers to the life cycle of a bean and its availability in the context of the Spring application.
Spring provided multi contexts and the default context is Singleton

1. **Singleton** - only one instance of a bean is created and all request for that bean are provided that bean.
1. **Prototype** - In this a new instance is created everytime there is a request for that bean.This is useful if we need unique beans for a thread or request.
1. **Request** - Only available in http request.A new bean instance is created for each http request
1. **Session** - Only available in http session.A new bean instance is created for each http session.
1. **Application** - Bean is scoped at application level.
1. **WebSocket** - Bean is scoped at WEBSOCKET level.

```java
@Configuration
public class AppConfig {

    @Bean
    @Scope("prototype")
    public PaymentService paymentService(AccountRepository accountRepository){
        return new PaymentService(accountRepository);
    }

    @Bean
    @SessionScope // use this to avoid typos
    public AccountRepository accountRepository() {
        return new AccountRepository(dataSource());
    }
}
```

### Special Beans

- Environment Beans
- Injectable

```java
// injecting the environment bean

private Environment environment;

@Autowired
public void setEnviornment(Environment environment){
    this.environment = environment;
}

public String getJavaVersion(){
    return environment.getProperty("java.version");
}
```

### Ways of fetching a bean

```java
public static void main(String[] args){
    var ctx = SpringApplication.run(ExampleApplication.class, args);

    MyFirstClass myFirstClass = ctx.getBean(MyFirstClass.class)
    MyFirstClass myFirstClass = ctx.getBean("myBeanName", MyFirstClass.class)
}
```

### Reading From Custom Properties

```java
@PropertySource("classpath:custom.properties")
@PropertySources({
    @PropertySource("classpath:custom.properties"),
    @PropertySource("classpath:custom2.properties")
})

@Value("${my.prop}")
private String myProp;
```

### PROFILES

- To change the active profile you can change it in project properties.
- You also need to create a .properties file for that profile e.g for a dev profile - application-dev.properties.

```properties
spring.profile.active=dev
spring.profile.active=test,dev
```

The last one here is the one that will be active

To make a bean available for a certain profile/environment we say

```java
@Bean
@Profile("dev")
public FirstBean myFirstBean(){
    return new FirstBean();
}
```

## Spring Rest

### HTTP Methods

GET - fetch a resource from the server
POST - used to create a new resource
PUT - used to update an existing resource or create a new one if it does not exist
DELETE - used to delete a resource
PATCH - this is used for partial updates to the resource as compared to PUT which is used for full updates.
OPTIONS - used to find the methods allows for a specific url
HEAD - used to return head of a resource

### Status Codes

1xx - Informational\
2xx - Success\
3xx - Redirection\
4xx - Client Error\
5xx - Server Error\

### 2xx - Success

200 - OK (successful http response)\
201 - Created (successful and new resource was created)\
204 - No Content (successful request and no content sent back to the client)\

### 3xx - Redirection

304 - NOT Modified

### 4xx - Client Error

400 - Bad Request (Client provided bad data)\
401 - Unauthorized (Request requires auth,if auth has been done it mean user lack authority for that request)\
403 - Forbidden (Client does not necessary authorization for the specific request).Unlike the 401 Unauthorized re-Authorization here does not make a difference.The server uses this code when it does not want to review why a request was denied or when no other status is applicable.

### 5xx - Server Error

500 - Internal Server Error (Given when an unexpected error happens and there is no specific message to send)\
503 - Service unavailable (Specifies that the server is not available at the moment)

### Rest Implementation

```java

@RestController
public FirstController {

    @GetMapping("/hello-world")
    @ResponseStatus(HttpStatus.OK)
    public String sayHelloWorld(){
        return "Hello World"
    }

    @PostMapping("/post-from-world")
    public String post(@RequestBody String message){
        return "Message: " + message
    }

}
```

To mark a java class as a controller we use the @RestController annotation\
The Http Methods for a specific method are marked by the mapping annotations e.g @GetMapping\
If you need to specify the return status code you can that by using the @ResponseStatus anotation
The @RequestBody annotation specifies that the passed parameter is a request body

## Converting JSON object to Java Object

```java
public Order {

    private String productName;
    private int productQauntity;
    private double productPrice;
}
```

```java

@RestController
public FirstController {

    @GetMapping("/hello-world")
    @ResponseStatus(HttpStatus.OK)
    public String sayHelloWorld(){
        return "Hello World"
    }

    @PostMapping("/post-from-world")
    public String post(@RequestBody Order order){
        return "Message: " + order.toString()
    }

}
```

If converting your json(RequestBody) to a java Class object you cannot just create a class without accessors as we have done above it will give values default values.\
You need to do the following so that you Order object is correctly populated

```java
public Order {

    private String productName;
    private int productQauntity;
    private double productPrice;

    public String getProductName() {
        return productName;
    }

    public int getProductQuantity() {
        return productQuantity;
    }

    public int getProductPrice() {
        return productPrice;
    }

      public String setProductName(String productName) {
        this.productName = productName;
    }

    public int getProductQuantity(int productQuantity) {
        this.productQuantity = productQuantity;
    }

    public int getProductPrice(double productPrice) {
        this.productPrice = productPrice;
    }

    @Override
    public String toString(){
        // return .....
    }
}
```

Here we have provided accessor (getters and setters) which are mutators which will set your object and then return the provided values when toString method is invocked

## JSON mapping in a class

Let say we have a json from a client with property name p-price instead of productPrice\
We can map this to our order class without having to stand the field name using @JsonPropery annotation.

```java
public Order {


    private String productName;
    private int productQauntity;
    @JsonPropery("p-price")
    private double productPrice;
}
```

## Java Records

```java

public record OrderRecord(
    String customerName,
    String productName,
    int quantity
){
}
```

Use records if you need simple carrier of data
Records fields are final (not mutable)

## Path Variable

```java
// http://localhost/hello/john-doe
@GetMapping("/hello/{user-name}")
public String sayHello(
    @PathVariable("user-name") String userName
){
    return "My value is: " + username
}
```

## Request Parameters

```java

// http://localhost:8080/hello?param_name=paramvalue&param_name_2=value_2
@GetMapping("/hello")
public String sayHelloParams(
    @RequestParams("user-name") String userName,
    @RequestParams("user-lastName") String userLastName
){
    return "My value is: " + userName + " " + userLastName;
}
```

## Data JPA

### Adding depedencies in pom.xml

Under dependencies in the pom.xml file add the following dependency

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>

<dependency>
    <groupId>org.postgresql</groupId>
    <artifactId>postgresql</artifactId>
</dependency>
```

After adding this dependency do not forget to reload your depedencies\
You can do this by right clicking in the pom.xml file and navigating to maven and then select reload\
In the above snippet we have added the dependency for Data JPA and also added the driver for postgresql

### Database connection configuration

You need to replace the application.properties with application.yml\

```yml
spring:
  datasource:
    url: [paste your database url here]
    username: [database_username]
    password: [database_password]
    driver-class-name: org.postgresql.Driver
```

### Hibernate configuration

```yml
spring:
  datasource:
    url: [paste your database url here]
    username: [database_username]
    password: [database_password]
    driver-class-name: org.postgresql.Driver

  jpa:
    hibernate:
      ddl-auto: create
    show-sql: true
    properties:
      hibernate:
        format-sql: true
    database: postgresql
    database-platform: org.hibernate.dialect.PostgreSQLDialect
```

ddl-auto - Tell Spring how to handle the schema create,update,deletion or validation.\
show-sql - Toggles between showing and hiding the sql queries when we execute a piece of code that interacts with database.\
format-sql - Formats the sql shown to you\
database: Specifies the database that you are using\
database-platform - Specifies the dialect(language) that is doing to be used

### Entity Class

```java
@Entity
@Table(name= "Students") // changing the table name from Student to Students
class Student{

    @Id
    @GeneratedValue // tell JPA to generate the primary keys for us
    private Integer id;

    @Column(
        name = "c_fname",
        lenght = 250
    ) // can change column name
    private String firstname;
    private String lastname;

    @Column(
        unique=true
    ) // making the email unique
    private String email;
    private int age;


    public Student(String firstname, String lastname, String email, int age){
        this.firstname = firstname;
        this.lastname = lastname;
        this.email = email;
        this.age = age
    }



    //getters and setter
    // generate them
}

```

Entity - means a class that is meant to be persistant in a relational database using JPA.\
Entity class should have a primary key - use @Id\
You also need to add any empty constructor.\
@Entity, @Table, @Id comes from jakarta.persistence

## Persisting Data in Database

This makes use of repositories\
You must make an interface that extends the JPA repositoy interface\

```java

public interface StudentRepository extends JpaRepository<Student, Integer>{

}
```

Next Step is to inject this into the file you need to use it e.g a controller

```java

@RestController
public MyController{

    private StudentRepository repository;

    public MyController(StudentRepository repository){
        this.repository = repository;
    }

    @GetMapping("/students")
    public List<Student> getAllStudents(){
        return repository.findAll();
    }

    // post

    @GetMapping("/students")
    public Student post(
        @RequestBody Student student
    ){
        return repository.save(student);
    }

    // fetch by id
     @GetMapping("/students/{student-id}")
    public Student post(
        @PathVariable("student-id") Integer id
    ){
        return repository.findById(id)
                .orElse(new Student())
    }

    @DeleteMapping("/students/{student-id}")
    public void delete(
        @PathVariable("student-id") Integer id
    ){
        repository.deleteById(id)
    }
}

```

## Mapping and Relationships

First you need to create the entities that you need to link
After that you need to add annotation e.g @OneToOne, @ManyToOne, @OneToMany, etc.
After that you then need to join the tables using the @JoinColumn Annotaion

### One to One Mapping

```Java

public Student {

    @OneToOne(
        mappedBy = "student"
        cascade = CascadeType.All
    )
    private StudentProfile studentProfile
}

public StudentProfile {
    @OneToOne
    @JoinColumn(
        name = "student_id"
    )
    private Student student
}

```

### One to Many Mapping

```Java

public Student {

    @ManyToOne(
        mappedBy = "student"
        cascade = CascadeType.All
    )
    @JoinColumn(
        name = "school_id"
    )
    private School school
}

public School {
    @OneToMany(
        mapped="school"
    )
    private List<Student> students
}

```

## Adding Data for School

To add data here you just need to create the controller and then the repository\
Make use of @JsonManagedReference and @JsonBackReference

@JsonManagedReference - This tell JPA or Hibernate that only the parent entity can serialize the child and child cannot serialize the parent.\
@JsonBackReference - Is used so that it cannot serialize the parent\
Both these are used on top of respective variable names
