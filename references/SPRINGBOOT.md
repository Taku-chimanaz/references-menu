# SPRINGBOOT REFERENCE

![SPRINGBOOT logo](./../images/springboot.webp)

I have created this reference document to help me reference or recall concepts I have learnt in SpringBoot.Please note that this list only reflect on concepts that I have learnt and does not exhaust all the conceptes.It can also serve as a starting point for anyone who wants to learn about Springboot but do not know where to begin.Hope this helps.

## Fundamentals

### What is Spring Framework

It is an open source framework for building enterprise Java Application.Its aim is to simplify the complex enterprise Java application development.It is also lightweight.

#### Core Features of Spring Framework

- IOC (Inversion of Control) - It is the principle of how objects are managed and created in your application.Instead of you providing the dependencies you need the framework(Spring container) does that for you.The Spring does this by implementing Dependency Injection.
- Aspect Oriented Programming - It allows you to separate cross-cutting concerns from your business logic.This prevents scattering of functionality across the application.
- Data Access Framework
- MVC Framework

#### What is a SPRING BEAN

Spring Bean is an object that manages by the Spring Framework in a Java application.

#### @Configuration Example

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
