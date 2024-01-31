---
title: "coffee and software"
date: 2023-06-16T19:06:21+08:00
author: ""
tags: [""]
categories: [""]
---

Youtube @coffeesoftware

Spring 6 & Spring Boot 3.X



---

##### Humor

Learn gradle, lose weight

morally superior in every way

be the change you wanna see in the world...

it's better than Disneyland

more than two times as good as java 8

it's oracle, sometimes it's never simple...(robot tone)

Friends do not let friends write Dockerfile.

Make `.jar`, not `.war`!

for legitimately slacking off "MY CODE'S COMPILING"

Use Graphql,  don't use Swagger!

You get it photoshopped at the end of the day!

---



1. TestContainers - dependencies - (good for building database automatically)

2. `@ServiceConnection`

3. `spring-boot-devtools`

   + Changing the scope of Gradle dependencies
   + 

4. Recompile - vs build 

5. `@ControllerAdvice`

   ```
   @ControllerAdvice
   class
       @ExceptionHandler
       ProblemDetail handle (ise, rq) {
           // sth
       }
   ```

   

6. Jakarta ee 9 === Jakarta ee8

7. spring-boot-starter-actuator

   + show details of the endpoint health

   + URL : `localhost:8080/actuator`

   + URL : `localhost:8080/actuator/metrics`

     ```
     ### old: book 2.x
     spring cloud sleuth (tracing)
     spring cloud
     spring boot
     spring framework
     micrometer.io (metrics: mean, median, average)
     // Micrometer.io : similar to log4j
     // @Timed -- track how long this method takes
     // ObservationRegistry 
     ```

8. Terminal

   ```terminal
   ./gradlew 
   ./mvnw spring-boot:build-image
   ```

   

9. GraalVM

   + Ahead-of-time compiler 
   + But configuration files needed (usually in json files)
   + `Goldeneye 007` Elevator music
   + `ps -o rss <PID>` : retrieve the resident set size

10. `@GetExchange`  

    ```java
    @GetExchange  
    Flux<Customer> customers
    ```

11. `var wc = builder.baseURL("xxx").build();`

12. resources > graphql

    + 3 operations:
      1. Query (`qwee-ree`)
      2. Subscription
      3. Mutation (all changes inc. delete, update, etc)
    + 

13. `@SchemaMapping`

    ```
    @SchemaMapping(typeName="Query")  //alias @QueryMapping
    Flux<Customer> customers() {
    		return http...
    }
    ```

    

14. Pull the apis in a whole

15. 自訂 typeName (e.g. Customer)

    Customer setting at `schema.graphql` file

    ```
    @SchemaMapping(typeName = "Customer") 
    Profile profiles (Customer cust) {
        // sth
    }
    ```

    

16. Reactive programming --- `Mono<Profile>`

17. `@BatchMapping`

    ```
    
    ```

18. `@Argument`

    ```
    @QueryMapping
    Flux<Customer> customersByName(@Argument String name) {
        return this.http.customersByName(name);
    }
    ```

19. `AbstractRoutingDatasource` --- 

    + To have a map for datasources
    + Refer to [Baeldung](https://www.baeldung.com/spring-abstract-routing-data-source)

    

    > https://tanzu.vmware.com/content/white-papers/spring-boot-3
    >
    > https://www.youtube.com/watch?v=TOfYlLjXufw
    >
    > https://techdozo.dev/spring-for-graphql-schemamapping-and-querymapping/
    >
    > 

    







































