---
title: "1hr37mins30secs de SpringBoot"
date: 2023-12-26T14:13:34+08:00
author: "celine"
tags: [""]
categories: ["StudyNote"]
---

## SpringBoot 2023

##### SpringBoot initializr

Dependencies:

- SpringBoot DevTools
- Spring Web 
- Spring Data JPA and Hibernate
- PostgreSQL Driver

##### Starting the server

```
src
  - main
    - java
      - com.example.demo.DemoApplication
    - resources
      - static
      - templates
      - application.properties
  - test
    - java
    - resources
```

##### Create a simple API

```java
@RestController //To Make the class to serve restful endpoint
public class DemoController {}
  @GetMapping //To set Restful endpoint on method
  public List<String> hello() {
    return List.of("Hello", "World");
  }
}
```

##### Student Class

```java
public class Student {
    private Long id;
    private String name;
    private String email;
    private LocalDate dob;
    private Integer age;
}
// No arg constructor
// All args constructor
// constuctor w/ everything but without ID
// Getter & Setter
// toString
```

```java
@GetMapping
public List<Student> hello() {
  return List.of(
    new Student(
                1L, 
                "Mariam", 
                "mj@gmail.com", 
                LocalDate.of(2000, Month.FEBRUARY, 16),
                24
               )
  );
}
```

##### API Layer

```java
@RestController
@RequestMapping(path="api/v1/student")
public class StudentController {
  
  private final StudentService studentService;
  @Autowired
  public StudentController(StudentService studentService) {
    this.studentService = studentService;
  }
  
  @GetMapping
  public List<Student> getStudents() {
    
  }
}
```

##### Business Layer

```java
//@Component //To tell spring that this is a bean
@Service //More for semantics, exactly the same as component
public class StudentService {
  public List<Student> getStudents() {
    return List.of(
      new Student(
          1L,
          "chu",
          "cu@gmail",
           LocalDate.of(1998, Month.FEBRUARY, 5),
           26
      )
    );
  }
}
```

##### Properties File

```properties
# Application.properties
spring.datasource.url=jdbc:postgresql://localhost:5432/student
spring.datasource.username=
spring.datasource.password=
# create-drop: ensure we have a clean state every time we run the app
spring.jpa.hibernate.ddl-auto=create-drop
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.PostgresSQLDialect
spring.jpa.properties.hibernate.format_sql=true
```

#### Connecting to Database

打開 shell，輸入 `psql`

`\l` 可以查看現有的資料庫清單

建立新的資料庫

```sql
CREATE DATABASE student;
```

`\du` 可以查看現有角色 role

```sql
	GRANT ALL PRIVILEGES ON DATABASE "student" TO {role_name};
```

`\c student` 連線進去此資料庫 

`\d` 確認此資料庫有無relations (如果有app正在存取應該會顯示出來)

- Run project -> Hikari-Pool started

#### JPA and @Entity

```java
@Entity //For Hibernate
@Table //For table in the datable
public class Student {
  // some fields
  @Id
  @SequenceGenerator(
    name="student_sequence", 
    sequenceName = "student_sequence",
    allocationSize=1)
  @GeneratedValue(
    strategy = GenerationType.SEQUENCE,
    generator = "student_sequence"
  )
  private Long id;
  private String name;
}
```

####  JPA repository

Implement data access layer

StudentRepository --- 
使用 xxx+Repository 命名，當程式要存取資料庫，尤其是使用JPA存取DB的時候

繼承JpaRepository，Generic#1 的 T 放 the type of object that we want this repository to work with；Generic#2 the ID for the type that we want.

`@Repository` --- responsible for data access

```java
@Repository
public interface StudentRepository extends JpaRepository<Student, Long> {
  
}
```

在student service 使用student repository

```java
@Service
public class StudentService {
  private final StudentRepository studentRepository;
  @Autowired
  public StudentService(StudentRepository studentRepository) {
    this.studentRepository = studentRepository;
  }
  
  public List<Student> getStudents() {
    return studentRepository.findAll();
  }
}
```

#### Saving Students

建立一個學生配置檔，這樣啟動專案的時候就會將以下資料插入資料庫的table

```java
@Configuration
public class StudentConfig {
  @Bean
  CommandLineRunner commandLineRunner(StudentRepository repository) {
    return args -> {
      Student mariam = new Student(
                 "Mariam",
                 "mj@gmail.com",
                 LocalDate.of(2000, FEBRUARY, 16)
      );
      Student himmel = new Student(
                 "Himmel",
                 "hj@gmail.com",
                 LocalDate.of(1998, FEBRUARY, 5)
      );
      repository.saveAll(
        List.of(mariam, himmel)
      );
    };
  }
}
```

#### @Transient

存入資料庫table時，與其塞入年齡值，選擇直接讓系統藉由生日(dob)計算出年齡

使用 `javax.persistence.*`，如此以來之後如果把Hibernate換成其它provider，還是能正常運作

標註`@Transient`的類別欄位：There is no need for you to be a column in our database, meaning that age will be calculated first.

把 age 從 Student constructor 移除之後，在 getAge 方法中
修改回傳值 `Period.between(this.dob, LocalDate.now()).getYears();`

```java
import javax.persistence.*;
import java.time.LocalDate;

@Entity 
@Table 
public class Student {
  @Id
  @SequenceGenerator(
    name="student_sequence", 
    sequenceName = "student_sequence",
    allocationSize=1)
  @GeneratedValue(
    strategy = GenerationType.SEQUENCE,
    generator = "student_sequence"
  )
  private Long id;
  private String name;
  private String email;
  private LocalDate dob;
  @Transient //This field
  private Integer age;
  
  public Student() {
  }
  public Student(Strng name, String email, LocalDate dob) {
    this.name = name;
    this.email = email;
    this.dob = dob;
  }
  // some getter and setter
  public Integer getAge() {
    return Period.between(this.dob, LocalDate.now()).getYears();
  }
}
```

#### PostMapping

Post is used when you want to add new resources to your system. 
在這個例子就是新增一筆 Student。

1. 在 StudentController.java 新增一個 registerNewStudent 方法 

```java
@PostMapping
public void registerNewStudent(@RequestBody Student student) {
  studentService.addNewStudent(student);
}
```

2. 在 StudentService.java 新增 addNewStudent 方法

```java
public void addNewStudent(Student student) {
  System.out.print(student); //sout
}
```

3. 右鍵上面的 `@PostMapping`，點擊 Open in HTTP client

   ```
    ---Actions for URL---
    Go to declaration or usages
    Open in HTTP client ✅ click this ✅
    Show all endpoints of module
   ```

4. generated-requests.http

   ```
   ###
   POST http://localhost:8080/api/vi/student
   CONTENT-TYPE: application/json
   
   {
     "firstName": "Alicia",
     "lastName": "Hadid",
     "email": "ha@gmail.com",
     "password": "passw0rd"
   }
   ```

5. 寫好 payload 之後，點擊 POST 左邊的綠色play按鈕，Run this request，就可以在 console 查看

6. 步驟3~6也可以改用postman完成

#### Writing Business Logic

在StudentRepository寫自定義的商業邏輯

```java
@Repository
public interface StudentRepository extends JpaRepository<Student, Long> {
  //@Query("SELECT s FROM Student s WHERE s.email = ?1")
  Optional<Student> findStudentByEmail(String email);
}
```

在 StudentService.addNewStudent 方法加入判斷是否有重複 email 的商業邏輯

```java
public void addNewStudent(Student student) {
  Optional<Student> studentOptional = studentRepository.findStudentByEmail(student.getEmail());
  if (studentOptional.isPresent()) {
    throw new IllegalStateException("email taken"); 
    //could be improvised
  }
  studentRepository.save(student);
}
```

#### Testing post request

試著發送email與現有資料一樣的payload， 結果發現 status為 500，但 message 是空的 ""

在 application.properties 加入錯誤訊息的顯示

```properties
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.PostgreSQLDialect
spring.jpa.properties.hibernate.format_sql=true

# 顯示 error message 的配置
server.error.include-message=always
```

#### Delete student

在 StudentController 加入 deleteStudent 方法

```java
@DeleteMapping(path = "{studentId}")
public void deleteStudent(
  @PathVariable("studentId") Long studentId) {
  studentService.deleteStudent(studentId);
}
```

在 StudentService 加入 deleteStudent 方法

```java
public void deleteStudent(Long studentId) {
  boolean exists = studentRepository.existsById(studentId);
  if (!exists) {
    throw new IllegalStateException(
      "student with id " + studentId + " does not exists");
  }
  studentRepository.deleteById(studentId);
}
```

測試這個方法，刪除 id 為 1 的學生

```http
###
DELETE http://localhost:8080/api/v1/student/1
```

#### Exercise

PutMapping - to update both name and email。要掛上`@Transaction` 標註。

StudentController:

```java
@PutMapping(path="{studentId}")
pubic void updateStudent(@PathVariable("studentId") Long studentId,
                        @RequestParam(required=false) String name,
                        @RequestParam(required=false) String email) {
  studentService.updateStudent(studentId, name, email);
}
```

StudentService:

When we use `@Transactional` annotation, the entity goes into managed state (參考 Spring Data JPA course).

```java
@Transactional
public void updateStudent(Long studentId, String name, String email) {
  Student student = studentRepository.findById(studentId)
    .orElseThrow(() -> new IllegalStateException(
            "student with id " + studentId + " does not exist."));
  //變更名字
  if (name != null && name.length() > 0 &&
      !Objects.equals(student.getName(), name)) {
    student.setName(name);
  }
  // change email
  if (email != null && email.length() > 0 &&
     !Objects.equals(student.getEmail(), email)) {
    Optional<Student> studentOptional = studentRepository.findStudentByEmail(email);
    // check that new email hasn't been taken
    if (studentOptional.isPresent()) {
      throw new IllegalStateException("email taken");
    }
    student.setEmail(email);
  }
}
```

測試更新一筆資料 request payload

```http
###
PUT http://localhost:8080/api/v1/student/1?name=Maria
Content-Type: application/json
```

```http
###
PUT http://localhost:8080/api/v1/student/1?name=Maria&email=already@gmail.com
```

## 10 SpringBoot mistakes

1. **Leaking internals**

   洩漏 user 不需要知道的訊息，例如 Customer entity 的 password 欄位

   方法... 使用 `@JsonIgnore`

   ```java
   @Override
   @JsonIgnore
   public String getPassword() { return this.password; }
   ```

2. **Not using Record** ... 好東西幹嘛不用

   取代 `class CustomerResponse`

   ```java
   record CustomerResponse (String name) {
     
   }
   // Before appearance of Record
   class CustomerResponse {
     String name;
     //...
   }
   ```
   
3. Not using dependency injection correctly

   不要再用 new ... initialize 物件，改用依賴注入	

   Beans, they are singleton, which means they are reused all over the shot.

   需要有建構子較好，不用加 物件的 setter	

   ```java
   public class Test {
     
     private final CustomerRepository customerRepository;
     
     //@Autowired ❌
     public Test(CustomerRepository customerRepository) {
       this.customerRepository = customerRepository;	 
     }
     // some codes
     
   //  Setter injection ❌
   //  public void setCustomerRepository(CustomerRepository customerRepository) {
   //    this.customerRepository = customerRepository;
   //  }
   }
   ```

4. Separation of misconcern

   Controller 不應該直接呼叫 Repository 的方法存許資料庫

   Controller 的職責是處理 httpRequest、Service 處理商業邏輯、Repository與資料來源互動

5. Lack of ErrorHandling

   以下程式碼為例，找不到就會拋出錯誤，message="foo"，但statusCode=500

   比較適切的回應是 404 error 才對

   ```java
   @GetMapping("{id}")
   private Customer getCustomer(@PathVariable Integer id) {
     return customerRepository.findById(id)
       .orElseThrow(() -> new RuntimeException("foo"));
   }
   
   ```

   最佳方式是寫自定義的錯誤處理機制

   src.main.java.com.example.exception.ApiError

   ```java
   package com.example.exception;
   
   import java.time.LocalDateTime;
   
   public record ApiError(String path,
                         String message,
                         int statusCode,
                         LocalDateTime localDateTime) {
   }
   ```

   DefaultExceptionHandler: 負責處理所有不同類型的例外

   `@ControllerAdvice`

   `@ExcpetionHandler`

   ```java
   @ControllerAdvice
   public class DefaultExceptionHandler {
     
     @ExceptionHandler(ResourceNotFoundException.class)
     public ResponseEntity<ApiError> handleException(ResourceNotFoundException e, HttpServletRequest request) {
       ApiError apiError = new ApiError(
         request.getRequestURI(),
         e.getMessage(),
         HttpStatus.NOT_FOUND.value(),
         LocalDateTime.now()
       );
       return new ResponseEntity<>(apiError, HttpStatus.NOT_FOUND);
     }
     
     @ExceptionHandler(InsufficientAuthenticationException.class)
     public ResponseEntity<ApiError> handleException(InsufficientAuthenticationException e, HttpServletRequest request) {
       ApiError apiError = new ApiError(
         request.getRequestURI(),
         e.getMessage(),
         HttpStatus.FORBIDDEN.value(),  // there
         LocalDateTime.now()
       );
       return new ResponseEntity<>(apiError, HttpStatus.FORBIDDEN);
     }
     
     @ExceptionHandler(BadCredentialException.class)
     public ResponseEntity<ApiError> handleException(BadCredentialException e, HttpServletRequest request) {
       ApiError apiError = new ApiError(
         request.getRequestURI(),
         e.getMessage(),
         HttpStatus.UNAUTHORIZED.value(),  // there
         LocalDateTime.now()
       );
       return new ResponseEntity<>(apiError, HttpStatus.UNAUTHORIZED);
     }
     
    // @ExceptionHandler(Exception.class)
     @ExceptionHandler({Exception.class, RuntimeException.class})
     public ResponseEntity<ApiError> handleException(Exception e, HttpServletRequest request) {
       ApiError apiError = new ApiError(
         request.getRequestURI(),
         e.getMessage(),
         HttpStatus.INTERNAL_SERVER_ERROR.value(),  // there
         LocalDateTime.now()
       );
       return new ResponseEntity<>(apiError, HttpStatus.INTERNAL_SERVER_ERROR);
     }
   }
   ```

   這樣的寫法就不會導致拋出容易使人混淆的 statusCode 了

   見上述程式碼最後一個方法
   `@ExceptionHandler({Exception.class, RuntimeException.class})`

6. Testing (NOOB Level)

   `@DataJpaTest`

   `@TestContainers`

   整合測試使用的 `@SpringBootTest` 

7. Not Using Annotation Validation

    使用 Validation 標註可以減少很多 if 判斷

   ```java
   @GetMapping("{id}")
   private void getCustomer(@Validated CustomerRequest customerRequest) {
     // some codes
   }
   
   record CustomerRequest (@NonNull String name) {}
   ```

   使用 `@Validated` + `@NonNull` 再加上自定義的 ExceptionHandler 捕獲錯誤，轉換成user看的懂的payload

8. Don't log enough

   - Know how to log (when to info, debug, warn)

   - Don't log frequently, because it could also impact the response round trip

9. Using xml to build Spring applications

10. Immutability

















































