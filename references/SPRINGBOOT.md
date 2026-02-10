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

@Value("${my.prop }")
private String myProp;
```

### PROFILES
