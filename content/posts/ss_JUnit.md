---
title: "使用 JUnit & Mockito 測試 Java 程式"
date: 2023-12-26T14:14:47+08:00
author: "celine"
tags: [""]
categories: ["StudyNote"]
---

## 使用 JUnit & Mockito 測試 Java 程式

Mockito --- 本來以為這跟薄荷飲料mochito同音，實際要唸成 /mɑkiːtoʊ/ 或者 /mˈɒkiːtoʊ/ ，音譯就是『莫kito』或者『碼kito』，總之 k 要發音，因為他是模擬方法 ("mock all methods")

### Unit Testing簡介

#### Unit Test 是什麼

單元測試是用來測試你的程式碼一部分區塊 的一個小型方法

```java
public String isEmailValid(String email) {
    //Code that needs to be tested
}
```

如何寫單元測試以確保一個類別中的某方法正常運作呢？

這是一段除法程式：

```java
public class Calculator {
  public int integerDivision(int dividend, int divisor) {
    return dividend / divisor;
  }
}
```

我們可以寫以下的單元測試，以驗證上方程式裡，開發者沒有敲錯鍵，把除號打成乘號.

```java
@Test
void testIntegerDivision_whenValidValuesProvided_shouldReturnExpectedResult() {
  //Arrange
  Calculator calculatore = new Calculator();
  //Act
  int result = calculator.integerDivision(4,2);
  //Assert
  assertEquals(2, result, "4/2 should have returned 2");
}
```

觀察上面的測試程式，發現  (1) 方法名稱特別長  (2) 上面掛 `@Test`

程式的第三段『assertEquals』驗證方法回傳結果是否正確，傳入第一個參數是期望得到的結果，第二個參數是程式實際回傳的結果，第三個參數(非必填) 則是測試失敗時，會印在console的錯誤提示訊息。

單元測試跑的很快，因為方法內會用到的依賴都可以被替換掉 --- fake, mocked, spied version

假設方法實際上會呼叫 http 客戶端 並發出http請求，相較於被測試的方法，單元測試不會發出實際的 http 請求，因為這個單元測試不是在測http client side 有沒有正常工作，單元測試只是在測方法內部的 Java 程式碼。

#### 為何要寫單元測試

1. 確保程式碼可以正常工作

2. 確保程式碼可以很好的處理有效和無效的輸入參數，不至於拋錯；

   避免修改完程式後，忘記測試某一個特性

3. 確保這程式方法目前以及將來都能運作良好，
   免得未來接手的人更改此方法時不小心弄壞現有功能

4. 確保你做的修改不會影響到其它程式碼，確保沒有回歸 (Regression)，所有功能仍然按照預期工作

5. 幫助後續重構、優化效能

##### F.I.R.S.T. 原則

- Fast - 單元測試不通過網路通信，也不執行數據庫操作，
- Independent - 一個單元測試不應該依賴於另一個單元測試的測試結果，也不受到依賴項影響
- Repeatable - 單元測試應該是可重複的，在不同環境/作業系統下，有相同結果
- Self-validating - 自我驗證 - 測試完成後，結果應該一目了然，不用手動檢查
- Thorough & Timely - 徹底與即時 - 正向/反向/極端案例都要覆蓋

#### Test code in isolation

假設現在有個 class_A 內有兩個 method (m_A & m_B)，理想情況裡 m_A 只有單一職責，且此方法與任何外部類別隔離，這樣要寫單元測試很容易。但實務上的 m_A 很可能會實例化另外一個 class_B，並調用此類別的其中幾個方法，這樣單元測試failed的話，有可能是 在程式執行到 class_B 裡的某個方法時出錯了，但被錯誤推論成 class_A  的方法 m_A 有問題。

- 如何將 class_A 的 m_A 與其它依賴項目隔離開來？使用Dependency Injection
  - 假設 Object A 其中一個方法實例化了 Object B 和 Object C，在單元測試的情況就是將 Object B 和 Object C 注入 Object A。

```
       [Object A]                
       ↙        ↘
[Object B]   [Object C]

[Mock Obj_B]    [Mock Obj_C]
         ↘        ↙
         [Object A]   
```

- 如上圖（下半部），這些依賴關係(Mock or Stub)停止注入，改用 predefined behavior

#### Testing Pyramid

```
          3 (金字塔頂)。End-to-End Testing / UI Testing
             ↳ Testing software functionality from beginning to end
     2 (金字塔中層)。 Integration Tests
        ↳ Application code is tested without mocking DB or HTTP connections
1 (金字塔底部)。Unit Tests              
  ↳ Testing isolated small pieces of code with Fake or Mock dependencies
```



單元測試是這三種測試中最快的，它不用連接資料庫，也不用透過網路發送http請求

End-to-end / 自動化UI測試是速度最慢的

### JUnit 5

JUnit 5 = JUnit Platform + JUnit Jupiter + JUnit Vintage

- JUnit platform: 在JVM上啟動測試框架的基礎
- JUnit Jupiter: 結合『新的programming模型』以及『用來以JUnit5編寫測試的擴展模型』
- JUnit Vintage: 在平台上運行 JUnit3 和 JUnit4 測試的測試引擎

#### Build Tools

支援 JUnit 的 IDEs：IntelliJ IDEA, Eclipse, NetBeans, VSCode

支援 JUnit 的 Build Tools：Maven, Gradle, Ant

Maven:

1. 在pom.xml加入 JUnit Jupiter 的 dependency

2. 在 src\test\java 目錄建一個 DemoTest.java，隨意寫個測試

   ```java
   @Test
   void testDemo() {
     System.out.println("Demo Test");
   }
   ```

3. 打開terminal，輸入 `ls` 確認跟 `pom.xml` 在同一層目錄

4. 輸入 `mvn package` 編譯並打包程式碼 → console 顯示 Tests run:0

5. 輸入 `mvn test` → 即使 build 成功，還是沒有執行測試（需要Plugin!)

6. 打開 `pom.xml`，在 `<dependencies>...</dependencies>`區塊之後，加入`<build>`

7. 在maven.repository.com網頁搜尋 Maven Surefire plugin，Maven頁籤下，複製\<dependency>裡面內容

8. 貼上到 \<build>\<plugins>\<plugin>裡面

9. reload maven project: 在 pom.xml右鍵→選Maven→reload project

10. 再執行指令`mvn package` 就可以看到不僅build project 還跑完了測試

11. 如果不想在 build 的時候執行測試，則執行指令 `mvn package -Dmaven.test.skip=true`

    ```xml
    <project>
      <dependencies>
        <dependency><!--JUnit Jupiter-->
          <groupId>org.junit.jupiter</groupId>
          <artifactId>junit-jupiter</artifactId>
          <version>5.8.2</version>
          <scope>test</scope>
        </dependency> 
      </dependencies>
      
      <build>
        <plugins>
          <plugin> <!--Maven Surefire plugin-->
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-surefire-plugin</artifactId>
            <version>3.0.0-M6</version>
          </plugin>
        </plugins>
      </build>
    </project>
    ```


#### Creating a new project_Gradle

```groovy
dependencies {
  testImplementation 'org.junit.jupiter:junit-jupiter:5.8.2'
  //testImplementation 'org.junit.jupiter:junit-jupiter-api:5.8.2'
  //testImplementation 'org.junit.jupiter:junit-jupiter-params:5.8.2'
  //testRuntimeOnly 'org.junit.jupiter:junit-jupiter-engine:5.8.2'
}

test {
  useJUnitPlatform()
  testLogging.showStandardStreams=true
}
```

- 搜尋 JUnit Jupiter Params，選Gradle(short)的頁籤

- 其實有另外一個 mvnRepository 已經集成了以上三個依賴：JUnit Jupiter (Aggregator)
- 在 test block 加入 testLogging.showStandardStreams
- 接著reload: 要再 load gradle changes (點擊 Gradle logo icon)

#### Check if JUnit works

1. src/test/java 目錄下建立一個 testDemo 方法

   ```java
   @Test
   void testDemo() {
     System.out.println("Demo test");
   }
   ```

2. 打開terminal，`ls`確認當前與`gradlew`在同一階層目錄下

3. 輸入指令 `./gradlew clean test` 清理專案，移除上一次的 build，並執行名為 test 的 task

4. 打開 console 驗證結果

#### Create a new project_Java

如何不靠 Maven 或 Gradle，自己建單元測試架構

1. 先在 專案名字/src 目錄下建立一個類別，並寫一些要被測試的方法 (UserService.java)

2. Test folder: 右鍵專案名字 → New Directory → 命名為 test

3. 接著右鍵這個新建立的資料夾，Mark Directory as "Test Sources Root"

4. 在要被測試的方法點右鍵 → Generate Test...

5. Create Test / Testing library 選 JUnit5

   / Class name 通常是命名成（要被測試的類別)+Test (例如. UserServiceTest.java)

   / Destination package 會與要被測試類別的package目錄一樣 (例如 com.appsdeveloper)

   /Generate 有兩個可選擇功能，後續會提 `setUp/Before`, `tearDown/@After`

   打勾要被測試的方法

6. 存檔後就可以看到他出現在 test/${ DestinationPackage} 路徑底下了

7. `fail("error message");` assertion

Create a new Java project_Eclipse IDE

1. 像平常一樣 create a java project，在 src 目錄下 new Java class

   ​    / Package 輸入套件 (e.g. `com.apachedeveloper.blog`)

2. 建立新類別完成，寫一個要被測試的方法

3. 接著右鍵專案名字 → new Source Folder  → 命名為 test

4. 右鍵剛剛建好的新類別 → new JUnit Test Case

5. 在 New JUnit test case 視窗內三個選項中，勾選 "New JUnit Jupiter test"

   [   ]New JUnit 3 test    [   ]New JUnit 4 test     [V]New JUnit Jupiter test

   / Source folder 原本是 `**/src`，改成 `**/test`

   / Class under test 應該就是Step4時右鍵類別的路徑

6. 點擊Finish後會出現彈窗問你 JUnit 5 目前不在 build path 上，是否要加入？

   維持`Perform the following action-Add JUnit5 library to the build path` 即可

7. 測試類別建立好之後，要運行單元測試則要右鍵這個測試類  → Run As <u>JUnit Test</u>

   系統可能會提醒你還沒存檔

8. 接著就會出現 JUnit 的頁籤，可供查看測試結果

#### Write Unit Test

```java
class CalculatorTest {
  
  @Test
  void integerDivision() {
    Calculator calculator = new Calculator();
    int result = calculator.integerDivision(4, 2);
    assertEquals(2, result, "should be 2");
  }
}
```

assertEquals, fail, assertTrue, assertFalse, assertNull, assertNotNull, assertThrows, assertDoesNotThrow ... 等這些方法是來自jupiter.api.Assertions 裡面的靜態方法

#### Lazy Assert Messages

```java
int expectedResult = 32;
int actualResult = calculator.integerSubtraction(33, 1);
assertEquals(expectedResult, actualResult, actualResult + " is wrong. Correct answer should be "+ expectedResult);
```

- 第三個param，也就是如果不符合expectedResult會用到的錯誤訊息，這個參數非必填
- 可以用字串串接，但並不是每次斷言都會用到這個message，因此比較建議改寫成 lambda

```java
assertEquals(expectedResult, actualResult, () -> actualResult + " is wrong. Correct answer should be "+ expectedResult);
```

- 這樣只有測試fail的時候，這段lambda才會執行

#### Naming Unit Tests

測試方法的命名最好可以一目瞭然，通常是這樣命名的：

`test{System under test}_{condition or state change}_{Expected Result}`

例如：testIntegerDivision_WhenFourIsDividedByTwo_ShouldReturnTwo() {...}

testIntegerDivision_WhenDividendIsDividedByZero_ShouldThrowArithmeticException() {...}

#### @DisplayName annotation

在測試類別上方掛`@DisplayName("Name to be displayed on test result")`

在方法名稱上面也可以掛這個標註，可以避免方法名稱長度太長，以至於報表不易讀取。

#### Tripple A - arrange, act, assert

Arrange: 準備變數並初始化需要用的的物件，又名為 Given

Act: 調用要被測試的方法，又名為 When

Assert: 用來驗證被測試的方法之回傳值，又名為 Then

```java
@DisplayName("Test 4/2 = 2")
@Test
void testIntegerDivision_Whe4IsDividedBy2_ShouldReturn2() {
  // Arrange
  Calculator calculator = new Calculator();
  int dividend = 4;
  int divisor = 2;
  int expectedResult = 2;
  // Act
  int actualResult = calculator.integerDivision(dividend, divisor);
  // Assert
  assertEquals(expectedResult, actualResult, "4/2 did not produce 2");
  log.info("4/2 沒有得出2")
}
```

#### Lifecycle Methods

當我們在一個測試類別建立三個測試方法並且掛上`@Test`時，JUnit 預設會替這個測試類別建立個數與測試方法一致的 Class Instance。這些方法以隨機的順序執行。

但 Lifecycle 測試方法掛的是不同的標註

- 其中一個是`@BeforeAll`，通常作為setup()方法，像是準備測試需要用的資源，例如建立資料庫
- 另外一個是`@AfterAll`，通常作為cleanup()方法， 在所有測試方法都結束後才執行，例如刪除測試一開始建立的資料庫
- 還有一個是 `@BeforeEach`，會在每一個單元測試開始之前執行，以減少重複的程式碼
- 最後一個是`@AfterEach`，在每一個單元測試結束後執行，例如關閉資料庫連線

BeforeAll 以及 AfterAll 都是只會執行一次 (one-time only) 的靜態 (static) 方法

```java
@BeforeAll
static void setup() {
  System.out.println("Executing @BeforeAll method.");
}
@AfterAll
static void clean() {
  System.out.println("Executing @AfterAll method.");
}
@BeforeEach
void beforeEachTestMethod() {
  // 每一個單元測試都要實例化的物件可以移到這裡
  Calculator calculator = new Calculator();
}
@AfterEach
void afterEachTestMethod() {
  // 可以放每個單元測試結束前的商業邏輯，例如關閉資料庫連線
  System.out.println("Executing @AfterEach method.");
}
```

接著就可以 Run CalculatorTest 查看這個類別所有有效單元測試的結果

停用某一單元測試可以選擇下兩種方式其中一種

1. 註解掉`@Test` 或者

2. 不動 `@Test`，但是加上一個標註`@Disabled()`

   ```java
   @Disabled("TODO: Still need to work on")
   @DisplayName("Division by zero")
   @Test 
   void testIntegerDivision_WhenDividedBy0_ShouldThrowArithmeticException() {
     fail("Not implemented yet.");
   }
   ```

`@Disabled`

- 測試結果還是看的到這個測試項目，但會顯示 `ignore 1 of 3 tests` 以及Disabled標註的參數("TODO: ...")

#### Assert an Exception

要怎麼寫一個反向測試，並且預期他會拋出某個特定錯誤呢？

```java
assertThrows(ArithmeticException.class, () -> {
  calculator.integerDivision(divident, divisor);
}, "Division by 0 should have thrown an Arithmetic exception");
```

#### ParameterizedTest 以及 MethodSource

移除原本的`@Test`，改標註`@ParameterizedTest`，這樣一來，此測試方法就可以接收傳入參數

再加上另一個標註`@MethodSource()`可以指定傳入參數來自於哪個方法

從以下範例來看，`integerSubtractionInputParameters`就是MethodSource方法名稱，有幾個要點：

1. 必須是靜態方法
2. 須回傳Strings of arguments

```java
@DisplayName("Test integer subtraction [minuend, subtrahend, expectedResult]")
@ParameterizedTest
@MethodSource("integerSubtractionInputParameters")
void integerSubtraction(int minuend, int subtrahend, int expectedResult) {
  int actualResult = calculator.integerSubtraction(minuend, subtrahand);
  assertEquals(expectedResult, actualResult,
              () -> minuend + "-" + subtrahend + " did not produce " + expectedResult);
}
private static String<Arguments> integerSubtractionInputParameters() {
  return Stream.of(
      Arguments.of(33, 1, 32),
      Arguments.of(54, 1, 53),
      Arguments.of(24, 1, 23)
  );
}
```

#### ParameterizedTest 以及 CsvSource

標註`@ParameterizedTest`除了可以和上述的 MethodSource 標註一起使用之外，也可以和另外一個標註型別`@CsvSource`一起使用

```java
@DisplayName("Test integer subtraction")
@ParameterizedTest
@CsvSource( {
        "33, 1, 32",
        "24, 1, 23",
        "54, 1, 53"
            } )
void integerSubtraction(int minuend, int subtrahend, int expectedResult) {
  // some codes
}
```

`@CsvSource` 如果傳入的是字串們，就會長得像下面這樣

注意:  to supply **empty String** value, we use single quote twice
            to supply **null**, simple do nothing, not even an empty space or single quotes

```java
@ParameterizedTest
@CsvSource({
  "apple, orange",
  "apple, ''",
  "apple,"
})
void testStrings(String strOne, String strTwo) {
  ...
}
```

#### ParameterizedTest 以及 CSVfile

先別不耐煩，這是倒數第二個組合😅

如果你要測試的參數清單一長串的話，那麽使用`@CsvFileSource` 搭配 `@ParameterizedTest` 會是很方便的解決辦法

至於這個檔案要放在哪裡呢？

1. 右鍵 `test`測試的根目錄 → new Package → 命名為 resources

2. 右鍵這包專案 → Open Module Settings

3. 在 Project Structure 視窗中，選左邊的 Project Settings > Modules  → 選 Calculator module

4. 在 Sources 頁籤底下，展開 test 目錄，選擇剛建立好的 resources package

5. 選好之後，找到 `Mark as: Test Resources`(mark directory as a test resources root)

6. 看到套件resources的左側圖示變更後，即可儲存關閉視窗

7. 在這個目錄 new File `integerSubtraction.csv` 並輸入以下參數存檔案

   ```
   33,1,32
   24,1,23
   54,1,53
   ```

   ```java
   @ParameterizedTest
   @CsvFileSource(resources = "/integerSubtraction.csv")
   void testIntegerSubtraction(int minuend, int subtrahend, int expectedResult) {
     // Some codes
   }
   ```

#### ParameterizedTest 以及 ValueSource

`@ParameterizedTest`還可以跟`@ValueSource` 一起使用，但只能接收一個傳入參數

以下方程式而言，這個單元測試會跑三次，1st測John，2nd測Kate，3rd測Alice

```java
@ParameterizedTest
@ValueSource(strings={"John", "Kate", "Alice"})
void valueSourceDemostraion(String firstName) {
  System.out.println(firstName);
  assertNotNull(firstName);
}
```

#### RepeatedTests

1. 在 test 目錄下的套件建立 `DemoRepeatedTest.java`，貼上 `CalculatorTest.java`裡面的其中一個測試方法、以`@BeforeAll`標註的 setup() 方法、還有以`@BeforeEach`標註的 beforeEachTestMethod()，

2. 接著 define Calculator property

3. 要讓此測試重複執行，須移除`@Test`標註，並使用重複的測試標註`@RepeatedTest(x)`

4. 括號內的數值 x 用來表示run一回合，會執行這個測試方法 x 次，預期這x次都能通過

   ```java
   public class DemoRepeatedTest {
     //define calculator property
     Calculator calculator;
     // BeforeAll
     @BeforeAll
     static void setup() {...}
     // BeforeEach
     @BeforeEach
     void beforeEachTestMethod() {...}
     // ATestMethod
     @DisplayName("Division by zero")
     @RepeatedTest(3)
     void testIntegerDivision_WhenDividedBy0_ShouldThrowAriEx(){...}
   }
   ```

5. 除此之外，還可以把重複信息物件注入到此測試方法，查看目前在第幾輪測試/此測試總共要運行幾輪測試

6. 也可以把測試信息物件注入到此測試方法，可讀取當前運行的測試方法名稱

7. 此外也可以變更每一輪測試結果的顯示名稱 (value, name)

   ```java
   public class DemoRepeatedTest {
     @DisplayName("Division by zero")
     //@RepeatedTest(3)
     @RepeatedTest(value=3, name="{displayName}, Repetition {currentRepetition} of "+"{totalRepetitions}")
     void testIntegerDivision_WhenDividedBy0_ShouldThrowAriEx(
         RepetitionInfo repetitionInfo,
         TestInfo testInfo) {
       //測試方法名稱
       System.out.println("Running " + testInfo.getTestMethod().get().getName());
       //測試跑到第幾輪了
       System.out.println("Repetition #" + repetitionInfo.getCurrentRepetition() + " of " + repetitionInfo.getTotalRepetitions());
       //Some code
     }
   }
   ```

#### Methods Order - by name

在測試類別上添加標註`@TestMethodOrder()`，此標註型別允許我們用方法順序接口

括號中傳入 MethodOrderer，有五個選項

```java
// 依據方法名稱排序(testA, testB, testC, testD)
@TestMethodOrder(MethodOrdered.MethodName.class) 
public class MethodOrderedByMethodNameTest {...}

@TestMethodOrder(MethodOrdered.OrderAnnotation.class)
public class MethodOrderedByOrderAnnotationTest {...}

@TestMethodOrder(MethodOrdered.DisplayName.class)
public class MethodOrderedByDisplayNameTest {...}

@TestMethodOrder(MethodOrdered.Alphanumeric.class)
public class MethodOrderedAlphanumericallyTest {...}

@TestMethodOrder(MethodOrdered.Random.class)
public class MethodOrderedRandomlyTest {...}
```

如果要自行指定單元測試執行的順序，就在類別掛上`@TestMethodOrder(MethodOrdered.OrderAnnotation.class)`，並在每個方法上標註`@Order(x)`，如下示例程式

```java
@Order(1)
@Test
void testD() {...}
@Order(2)
@Test
void testA() {...}
@Order(3)
@Test
void testC() {...}
@Order(4)
@Test
void testB() {...}
```

#### Order of Unit test classes

當一包 test 目錄下的 package 有多個測試類別，要如何制定哪個類別先執行，哪個後執行呢？

為什麼要控制單元測試類別的順序？為了確保這些測試類別之間沒有預料外的依賴關係，或者想要讓測試報告看起來比較有組織。

一樣也在類別上標註`@Order(x)`

#### Test Instance Lifecycle

- By dedault it's "per-method" test instance lifecycle
- The execution order is deterministic but intentionally non-obvious

如果想要讓JUnit在同一個instance實例上執行所有測試方法，可以在這個類別加上標註`@TestInstance(TestInstance.Lifecycle.PER_CLASS)`

#### TestInstance

- 這個標註用在測試類別上，預設為`@TestInstance(TestInstance.Lifecycle.PER_METHOD)`
- 現在有一個 UserServiceImplTest 的測試類別，其中有四個測試方法：
  1. 當接收到的資料有效時，建立一筆用戶並回傳UserId
  2. 當接收到的資料有效，更新該用戶信息，並回傳用戶信息
  3. 當提供的UserId有效，取得並回傳該Id對應的用戶信息
  4. 當透過提供的UserId刪除資料後，以id查詢確認無資料可回傳

#### Lifecycle Demo

```java
@TestMethodOrder(MethodOrderer.OrderAnnotation.class)
@TestInstance(TestInstance.Lifecycle.PER_CLASS)
public class UserServicImplTest {
  
  UsersDatabase usersDatabase;
  UserService userService;
  String createdUserId = "";
  
  @BeforeAll
  void setup() {
    // Create and initialize database
    usersDatabase = new UsersDatabaseMapImpl();
    usersDatabase.init();
    userService = new UserServiceImpl(usersDatabase);
  }
  
  @AfterAll
  void cleanup() {
    // Close connection
    // Delete database
    usersDatabase.close();
  }
  
  @Test
  @Order(1)
  @DisplayName("Create User works")
  void testCreateUser_whenProvidedWithValidDetails_returnsUserId() {
    // Arrange
    Map<String, String> user = new HashMap<>();
    user.put("firstName", "Jingle");
    user.put("lastName", "Halleju");
    // Act
    createdUserId = userService.createUser(user);
    // Assert
    assertNotNull(createdUserId, "User id should not be null");
  }
  
  @Test
  @Order(2)
  @DisplayName("Update user works")
  void testUpdateUser_whenProvidedWithValidDetails_returnsUpdatedUserDetails() {
    // Arrange
    Map<String, String> newUserDetails = new HashMap<>();
    newUserDetails.put("firstName", "Mr. Bean");
    newUserDetails.put("lastName", "Ms. Karmela");
    // Act
    Map updatedUserDetails = userService.updateUser(createdUserId, newUserDetails);
    // Assert
    assertEquals(newUserDetails.get("firstName"),
                 updatedUserDetails.get("firstName"), 
                 "Returned value of user's first name is incorrect");
    assertEquals(newUserDetails.get("lastName"), 
                 updatedUserDetails.get("lastName"), 
                 "The returned value of user's last name is incorrect");
  }
  
  @Test
  @Order(3)
  @DisplayName("Find user works")
  void testGetUserDetails_whenProvidedWithValidUserId_returnsUserDetails() {
    // Act
    Map userDetails = userService.getUserDetails(createdUserId);
    // Assert
    assertNotNull(userDetails, "User details should not be null");
  }
  
  @Test
  @Order(4)
  @DisplayName("Delete user works")
  void testDeleteUser_whenProvidedWithValidUserId_returnsUserDetails() {
    // Act
    userService.deleteUser(createdUserId);
    // Assert
    assertNull(userService.getUserDetails(createdUserId), 
               "User should not been found");
  }
}
```

### Test-Driven Development (TDD) --- Introduction

The test-driven development life cycle :  RED ➡️ GREEN ➡️ REFACTOR ➡️ REPEAT

1. **Red** - Write unit test that fails.
2. **Green** - Write application code to make unit test pass.
3. **Refactor** - Clean up. Improve unit test and application code.
4. **Repeat** - Repeat above steps until all your functionality is implemented.

```java
public interface UserService {
  User createUser(String firstName, String lastName, String email, String passwd, String repeatPasswd);
}
```

```java
package com.example.service;

public class UserServiceImpl implements UserService {
  @Override
  public User createUser(String firstName, 
                         String lastName, 
                         String email, 
                         String passwd, 
                         String repeatPasswd) {
    // some code implemenation
    return new User();
  }
}
```

```java
package com.example.model;
  
public class User {
  
}
```

#### Test Create User method

```java
import org.junit.jupiter.api.Test;

public class UserServiceTest {
  
  @Test
  void testCreateUser_whenUserDetailsProvided_returnsUserObject() {
    // Arrange
    UserService userService = new UserServiceImpl();
    String firstName = "Chelsea";
    String lastName = "Leon";
    String email = "lc@test.com";
    String passwd = "12345678";
    String repeatPasswd = "12345678";
    // Act
    User user = userService.createUser(firstName, lastName, email, passwd, repeatPasswd);
    // Assert
    assertNotNull(user, "the createUser() should not have returned null.");
  }
}
```

#### Test User object contains first name

```java
public class UserServiceTest {
  UserService userService;
  String firstName;
  String lastName;
  String email;
  String passwd;
  String repeatPasswd;
  
  @BeforeEach
  void init() {
    userService = new UserServiceimpl();
    firstName = "";
    lastName = "Minjinnie";
    email = "minji@test.com";
    passwd = "12345678";
    repeatPasswd = "12345678";
  }

  @Test
  void testCreateUser_whenUserCreated_returnedUserObjectContainsSameFirstName()   {
    // Arrange - as per BeforeEach
    // Act
    User user = userService.createUser(firstName, lastName, email, passwd, repeatPasswd);
    // Assert
    assertEquals(firstName, user.getFirstName(), "User's first name is incorrect");
  }

  @DisplayName("Empty first name causes correct exception")
  @Test
  void testCreateUser_whenFirstNameIsEmpty_throwsIllegalArgumentException() {
    // Arrange  - as per BeforeEach
    String firstName = "";
    String expectedExceptionMessage = "User's first name is empty";
    // Act & Assert
    IllegalArgumentException thrown = Assertions.assertThrows(IllegalArgumentException.class, () -> {
    userService.createUser(firstName, lastName, email, passwd, repeatPasswd);
  }, "Empty first name should have caused an Illegal Argument Exception");
    // Assert
    assertEquals(expectedExceptionMessage, 
                 thrown.getMessage(),
                 "Exception error message is not correct");
  }
}
```



### Introduction --- Mockito

Mockito 是什麼？

- 為Java創造的開源測試框架，Mockito允許我們在單元測試中建立 `test doubles` 物件。

Test Double 又是什麼？

- test double 是一個可以在測試中代替真實對象的物件，有點像是電影中的特技替身代表演員。

- 稱之為 Mock object, fake object, spied object, or a stub，這些不同類型的物件都被稱為 test doubles。

- 開發人員將其用來暫時替代一個真實 real 的物件。

- 為什麼要替代掉？以下面的方法為例，我們是要測試建立 user，而不是要測 save 方法

  ```java
  public User createUser(User user, DAO mySqlDAO) {
    // Code
    String userId = mysqlDAO.save(user);
    // More code
  }
  ```

- 因此會使用 Mockito 來替 mySqlDAO 建立一個模擬對象，並測試 save 方法回傳

  ​    (1) 有效用戶ID 或者  (2) null

- 因為可以模擬此 save 方法的行為，可以用來測試 create 方法



#### Adding Mocking to a project

1. 打開 mvnrepository.com 搜尋 Mockito JUnit Jupiter，依專案性質複製 maven 或者 gradle

2. 貼到 pom.xml 或者 build.gradle，再 reload 專案

3. userRepository

   ```java
   package com.example.data;
   
   import com.example.model.User;
   
   public interface UsersRepository {
     save(User user);
   }
   ```

4. userRepositoryImpl

   ```java
   public class UserRepositoryImpl implements UsersRepository {
     
     Map<String, String> users = new HashMap<>();
     
     @Override
     public boolean save(User user) {
       boolean returnValue = false;
       if (users.containsKey(user.getId())) {
         user.put(user.getId(), user);
         returnValue = true;
       }
       return returnValue;
     }
   }
   ```

#### Inject UsersRepository as Dependency

通常 service 呼叫 repository 會直接在create方法裡面 new 一個 repository 物件，再調用其方法

但如果這個create方法在對應的測試類別被呼叫的時候，new UserRepository() 就會出問題

這樣就不單元測試，而變成整合測試了

因此要改用 constructor based dependency injection，如下例

```java
public class UserServiceImpl implements UserService {
  
  UsersRepository usersRepository;
  public UserServiceImpl(UsersRepository usersRepository) {
    this.usersRepository = usersRepository;
  }
  
  @Override
  public User createUser() {
    // some codes
    boolean isUserCreated = usersRepository.save(user);
    // some else codes
    if (!isUserCreated) throw new UserServiceException("could not create user");
    return user;
  }
}
```

```java
public class UserServiceException extends RuntimeException {
  // 自定義例外，繼承RuntimeException
  public UserServiceException(String message) {
    super(message);
  }
}
```

#### Create a Mock Object

1. 首先，在測試類別上增加標註 `@ExtendWith(MockitoExtension.class)`
2. 建立一個 Mock Object，並在欄位上加標註 `@Mock`
3. 由於 `@BeforeEach` 方法會初始化 UserServiceImpl ，我們想要這個物件引用 Repository，就會加上`@InjectMocks`
4. Mockito 會給 UserServiceImpl 類別建立一個instance，並將usersRepository的Mock Object 注入到此 instance

```java
@ExtendWith(MockitoExtension.class)
public class UserServiceTest {
  @InjectMocks
  UserServiceImpl userService;
  
  @Mock
  UsersRepository usersRepository;
  String firstName;
  String lastName;
  String email;
  String passwd;
  String repeatPasswd;
  
  @BeforeEach
  void init() {
    firstName = "Sandie"
    lastName = "Karlie";
    email = "sk@test.com";
    passwd = "12345678";
    repeatPasswd = "12345678";
  }
}
```

#### Stub using built-in any() argument matcher

因為 UsersRepository 現在已經是一個 mock object 了，我們需要告訴 Mockito 他應有的行為

使用 `Mockito.when(...).thenReturn(...)`可以達成

when() 裡面放 mock object 要呼叫的方法；而 thenReturn() 有其他類似形態：

- `thenReturn(boolean)` // boolean
- `thenThrow(Class<? extends Throwable>)`
- `then(Answer<?>)`

而以下的 save 方法需要吃一個 User 物件參數，因為不是調用真正的 real object，mock object 並不會管你是不是真的放進了一個 User 物件或是一朵花，所以可以用 Mockito 內建的 argument matcher 達成，例如`Mockito.any(User.class)`。

```java
@DisplayName("User object created")
@Test
void testCreateUser_whenUserDetailProvided_returnsUserObject() {
  // Arrange
  Mockito.when(usersRepository.save(Mockito.any(User.class)))
    .thenReturn(true);
  // some Act codes
}
```

#### Verify method call

假設你要測試 createUser 方法，確實有呼叫一次 save 方法，而不是都沒呼叫或者call了兩次，這樣的情況可以使用 `Mockito.verify()`，它要吃兩個參數 `userRepository`以及其verification mode，接著呼叫被驗證的方法

如果要驗證 repository 的 save 方法只有被這個測試方法呼叫一次，assert可以這樣寫

```java
assertNotNull(user.getId(), "User id is missing");
Mockito.verify(usersRepository, Mockito.times(1))
  .save(Mockito.any(User.class));
```

另外還可以確認方法被呼叫至少或至多幾次，或者根本沒被呼叫到

```java
Mockito.atLeast(1) // int minNumberOfInvocations
Mockito.atMost(1)  // int maxNumberOfInvocations
Ｍockito.atLeastOnce()
Ｍockito.atMostOnce()
Mockito.never()
```

如果有太多 Mockito 的靜態方法，可以直接 `import static org.mockito.Mockito.*;`

#### Exception stubbing

假設當前要被測試的方法，在多種不同的情境下會拋出相對應的例外

```java
@DisplayName("if save() method causes RuntimeException, a UserServiceException is thrown")
@Test
void testCreateUser_whenSaveMethodThrowsException_thenThrowsUserServiceException() {
  // Arrange
  when(userRepository.save(any(User.class)))
    .thenThrow(RuntimeException.class);
  // Act
  assertThrows(UserServiceException.class, () -> {
    userService.createUser(firstName, lastName, email, passwd, repeatPasswd);
  }, "should have thrown UserServiceException instead");
}
```

#### Creating EmailNotificationService Class

```java
public class EmailVerificationServiceImpl implements EmailVerificationService {
  @Override
  public void scheduleEmailConfirmation(User user) {
    // put user details into email queue
  }
}
```

```java
public class UserServiceImpl implements UserService {
  
  UsersRepository usersRepository;
  EmailVerificationService emailVerificationService;
  
  public UserServiceImpl(UsersRepository usersRepository,
                        EmailVerificationService emailVerificationService) {
    this.usersRepository = usersRepository;
    this.emailVerificationService = emailVerificationService;
  }
  
  @Override
  public User createUser(String firstName,
                         String lastName,
                         String email,
                         String passwd,
                         String repeatPasswd) {
    if(firstName == null || firstName.trim().length() == 0) {
      throw new IllegalArgumentException("User's first name is empty");
    }
    if(lastName == null || lastName.trim().length() == 0) {
      throw new IllegalArgumentException("User's last name is empty");
    }
    User user = new User(firstName, lastName, email, UUID.randomUUID().toString());
    boolean isUserCreated;
    try {
      isUserCreated = usersRepository.save(user);
    } catch (RuntimeException ex) {
      throw new UserServiceException(ex.getMessage());
    }
    if(!isUserCreated) throw new UserServiceException("Could not create user");
    try {
      emailVerificationService.scheduleEmailConfirmation(); //⬅️
    } catch(RuntimeException ex) {
      throw new UserServiceException(ex.getMessage());
    }
    return user;
  }
}
```

#### Stub void method with Exception

- UserServiceTest.java

- `doNothing()` 與 `doThrow()`

  ```java
  
  public class UserServiceTest {
    @InjectMocks
    UserServiceImpl userService; 
    @Mock
    UsersRepository usersRepository;
    @Mock
    EmailVerificationServiceImpl emailVerificationService;
    String firstName;
    String lastName;
    String email;
    String passwd;
    String repeatPasswd;
    
    @Test
    @DisplayName("EmailNotificationException is handled.")
    void testCreateUser_whenEmailNotificationExceptionThrown_throwsUserServiceException() {
      // Arrange
      when(usersRepository.save(any(User.class))).thenReturn(true);
      /* when(emailVerificationService.scheduleEmailConfirmation(any(User.class)))
           .thenThrow(); 
         ➡️ The syntax doesn't work on method with void return */
      doThrow(EmailVerificationServiceException.class)
        .when(emailVerificationService)
        .scheduleEmailConfirmation(any(User.class));
     /* doNothing().when(emailVerificationService)
        .scheduleEmailConfirmation(any(User.class)); */
      // Act & Assert
      assertThrows(UserServiceException.class, () -> {
        userService.createUser(firstName, lastName, email, passwd, repeatPasswd);
      }, "Should have thronw UserServiceException instead");
      // Assert
      verify(emailVerificationService, times(1))
        .scheduleEmailConfirmation(any(User.class));
    }
  }
  ```

**Do nothing when method is called**

```java
 doNothing().when(emailVerificationService)
      .scheduleEmailConfirmation(any(User.class)); 
```

#### Call real method

如何讓 Mockito 調用真正的方法 --- `doCallRealMethod()`

這樣測試方法 run in debug mode 時，就可以在實際程式下中斷點，確認是不是有調用真方法

```java
@DisplayName("Schedule Email Confirmation is executed")
@Test
void testCreateUser_whenUserCreated_scheduleEmailConfirmation() {
  // Arrange
  when(userRepository.save(any(User.class))).thenReturn(true);
  doCallRealMethod().when(emailVerificationService)
    .scheduleEmailConfirmation(any(User.class));
  // Act
  userService.createUser(firstName, lastName, email, passwd, repeatPasswd);
  // Assert
  verify(emailVerificationService, times(1))
    .scheduleEmailConfirmation(any(User.class));
}
```

### Code Coverage --- Introduction

如何在執行單元測試時，生成程式碼覆蓋率的報告

- 執行單元測試時，調用了多少類別 class
- 專案中有多少方法已經覆蓋了單元測試

這份報告有助於找出還沒被測試到的方法

#### Generate code coverage report

找到專案的 test 目錄下的 測試類別，右鍵選 <u>Run 'xxxxTest' with Coverage</u>，或者找到圖示『shield with a play button』就可以產生程式碼覆蓋率報告

跑完測試之後，可以查看被測試的類別，如果有沒被測試到的方法，它左側就會有 red bar 醒目提示

如果覺得紅的太刺眼或者容易混淆，可以點擊這 bar ➡️ 板手圖示 (Edit  Coverage Colors) 

```
 - Line Coverage
    - Full
    - Partial
    - Uncovered	
```

#### Export Code Coverage Report

將測試覆蓋率報告輸出為 html 格式，首先點盾牌play圖示 (run xxxTest with Coverage)

在測試報告顯示的頁籤附近（通常是右上角）應該會有一個 "Export Test Results..." `Ｌ↗`

在Code Coverage Panel 也有一個長得一模一樣的按鈕 "Generate Coverage Report..."

點開之後要選擇輸出要存放的路徑，就可以產出報表了✅

#### Export Test Report using Maven

如果你的專案是 maven based 又使用 CICD的話，那麼使用 Maven 產出測試覆蓋率報表會很方便

1. 到 mvnrepository.com 網站搜尋 `maven surefire report plugin`

2. 複製 maven 頁籤的依賴，貼到 pom.xml 檔案 `<build>`標籤裡面

3. 將report 的 dependency 取代為 plugin

4. 另外要指定 maven 在 test phase 階段執行此 plugin，要在 **surefire-report-plugin** 增加 execution 配置

5. 如果要讓 maven 即便在測試其中一筆失敗時，仍然產出報告，要在 **surefire-plugin** 增加 testFailureIgnore 配置

6. 接著 Load Maven Changes (M圖示)

7. 這樣就可以打開 terminal，先 `ls` 確認跟 pom.xml 在同一層目錄下

8. 輸入 `mvn test`或者 `mvn clean test` 先清理 target directory 再 run test phase

9. 就可以在 terminal 看到跑了幾支測試程式，
   報告路徑`target/site/surefire-report.html`

   ```xml
   <dependencies> 
     <!--some dependencies-->
   </dependencies>
   <build>
     <plugins>
       <plugin>
         <groupId>org.apache.maven.plugins</groupId>
         <artifactId>maven-surefire-plugin</artifactId>
         <version>3.0.0-M6</version>
         <configuration> <!--＃5 即便有測試失敗，仍要產出測試報告-->
           <testFailureIgnore>true</testFailureIgnore>
         </configuration>
       </plugin>
       <plugin>
         <groupId>org.apache.maven.plugins</groupId>
         <artifactId>maven-surefire-report-plugin</artifactId>
         <version>3.0.0-M6</version>
         <!--＃4 測試報告執行階段的配置檔-->
         <executions>
           <execution>
             <phase>test</phase>
             <goals>
               <goal>report</goal>
             </goals>
           </execution>
         </executions>
       </plugin>
     </plugins>
   </build>
   ```

#### Jacoco --- Maven plugin for code coverage

1. 一樣，前往 mvnrepository.com 搜尋 jacoco maven plugin

   JaCoCo 是 Java Code Coverage 的簡寫

2. 複製 maven 頁籤，貼到 pom.xml 的 plugins 區塊裡

3. 要加入 execution `prepare-agent`，如下

4. 再加入另外一個 execution `report`，以及報告要在哪個階段執行

   ```xml
   <plugin>
     <groupId>org.jacoco</groupId>
     <artifactId>jacoco-maven-plugin</artifactId>
     <version>0.8.8</version>
     <executions>
       <!--#3-->
       <execution>
         <id>prepare-agent</id>
         <goals>
           <goal>prepare-agent</goal>
         </goals>
       </execution>
       <!--#4-->
       <execution>
         <id>report</id>
         <phase>test</phase>
         <goals>
           <goal>report</goal>
         </goals>
       </execution>
     </executions>
   </plugin>
   ```
   

接著 run `mvn clean test` 就可以產出報告

red diamond: the code is not covered in the unit test

yellow diamond: some conditions were not tested

green: fully tested

#### Introduction to Unit Testing Spring Boot Application

```
[Web/Presentation layer]    [Service layer]   [Data layer]
@RestController              @Service          @Repository
@GetMapping              ➡️                ➡️  
```

None of these annotatons will really be used, including service annotation in the device layer. Because I'm not going to start the web server, and because spring application context will not be created, and business logic and other layers will not even be involved. I will test the method of controller in isolation from all other layers.

#### Introduction to Integration Testing of Web Layer

`@SpringBootTest` will created an application context that is very similar to the one we use in production application, although by default it will not start a web server.

#### Adding Testing Support to Spring Boot Application

開啟 spring initializr io 的網站，找到 `spring web` 的依賴，這個依賴可以讓我們建立 spring web application 也可以建立 spring boot restful microservices。

在 mvnrepository.com 搜尋 spring boot starter test 也可以找到對應的依賴，往下看 compile dependencies 可以看到它也結合了 junit jupiter, mockito-core 以及 mockito-junit-jupiter

另外如果專案有使用 spring security 的話，也會有對應的 spring-security-test 依賴

```xml
<dependencies>
  <dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
  </dependency>
  <dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
  </dependency>
  <dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId>
    <scope>test</scope>
  </dependency>
  <dependency>
    <groupId>org.springframework.security</groupId>
    <artifactId>spring-security-test</artifactId>
  </dependency>
</dependencies>
```

@WebMvcTest and @AutoConfigureMockMvc

1. Inside of the test folder, create a new package the same name where you have the Controller.

2. Add class `UsersControllerWebLayerTest`

3. 加上標註 `@WebMvcTest`告訴 Spring 只需要掃描有限數量的類別

   只會掃描並建立 controllerBean，但不會建立資料存取層

   這樣一來，執行單元測試也比整合測試還要快

4. 如果要要限縮成只有一個控制器類：I can limit this annotation to work with one controller class that I need. 

   To add property `controllers = name of the controller class`

5.  如果被測試的此專案有使用 spring security，但測試時不想經過這些安全過濾器，可以加上標註`@AutoConfigureMockMvc(addFilters=false)` 來 exclude 

6. `@AutoConfigureMockMvc`實際上在 `＠WebMVCTest` 標註自己就有加了

   參考 [WebMvcTest reference](https://docs.spring.io/spring-boot/docs/current/api/org/springframework/boot/test/autoconfigure/web/servlet/WebMvcTest.html)

```java
package com.example.tutorials.junit.controller;

import org.springframework.boot.test.autoconfigure.web.serflet.WebMvcTest;

@WebMvcTest(controllers = UsersController.class)
@AutoConfigureMockMvc(addFilters = false)
public class UsersControllerWebLayerTest {
}
```

#### RequestBuilder

請見下方程式碼 MockMvcRequestBuilders 的部分 -- 

可以調用他的 post 方法，傳入參數 uri，由於建構者模式可以再加入 contentType 與 accept 方法

接著注入 MockMvc 來 perform 剛做好的 requestBuilder 物件，並取得 MvcResult

```java
package com.example.tutorials.junit.controller;

@WebMvcTest(controllers = UsersController.class)
@AutoConfigureMockMvc(addFilters = false)
public class UsersControllerWebLayerTest {
  
  @Autowired
  private MockMvc mockMvc;
  
  @Test
  @DisplayName("User can be created")
  void testCreateUser_whenValidUserDetailProvided_returnsCreatedUserDetails() throws JsonProcessingException {
    //Arrange
    UserDetailsRequestModel userDetailsRequestModel = new UserDetailsRequestModel();
    userDetailsRequestModel.setFirstName("Alley");
    userDetailsRequestModel.setLastName("Valeigh");
    userDetailsRequestModel.setEmail("valli@gmail.com");
    userDetailsRequestModel.setPasswd("12345678");
    userDetailsRequestModel.setRepeatPasswd("12345678");
    
    RequestBuilder requestBuilder = MockMvcRequestBuilders.post("/users")
      .contentType(MediaType.APPLICATION_JSON)
      .accept(MediaType.APPLICATION_JSON)
      .content(new ObjectMapper().writeValueAsString(userDetailsRequestModel));
    
    //Act
    MvcResult mvcResult = mockMvc.perform(requestBuilder).andReturn();
    String responseBodyAsString = mvcResult.getResponse().getContentAsString();
    UserRest createdUser = new ObjectMapper()
      .readValue(responseBodyAsString, UserRest.class)
    
    //Assert
    Assertions.assertEquals(userDetailsRequestModel.getFirstName(), createdUser.getFirstName(), "The returned user first name is most likely incorrect");
    Assertions.assertEquals(userDetailsRequestModel.getLastName(), createdUser.getLastName(), "The returned user first name is most likely incorrect");
    Assertions.assertEquals(userDetailsRequestModel.getEmail(), createdUser.getEmail(), "The returned user email is incorrect");
    Assertions.assertFalse(createdUser.getUserId().isEmpty(), "userId should not be empty");
  }
}
```

#### `@MockBean` --- Mocking service layer

1. 在 test 類別加入一個依賴注入 `@MockBean UserService userService`
2. 另外一種方法是直接在類別上加此標註 `@MockBean({UserServiceImpl.class, FundServiceImpl.class})` + `@Autowired`
3. 在 arrange 區域建立 userDto
4. 如果 UserDetailsRequestModel 有設定 `@Email`欄位驗證，那是因為 pom.xml 有加上 spring-boot-starter-validation 的依賴

```java
@WebMvcTest(controllers = UsersController.class, excludeAutoConfiguration = {SecurityAutoConfiguration.class})
//@MockBean({UserServiceImpl.class})
public class UsersControllerWebLayerTest {
  @Autowired
  private MockMvc mockMvc;
  @MockBean
  //@Autowired
  UsersService usersService;
  
  @Test
  @DisplayName("user can be created")
  void testCreateUser_whenValidUserDetailsProvided_returnsCreatedUserDetails() {
    //Arrange
    UserDetailsRequestModel userDetailsRequestModel = new UserDetailsRequestModel();
    userDetailsRequestModel.setFirstName("Alley");
    userDetailsRequestModel.setLastName("Valeigh");
    userDetailsRequestModel.setEmail("valli@gmail.com"); // #4 "email"
    userDetailsRequestModel.setPasswd("12345678");
    userDetailsRequestModel.setRepeatPasswd("12345678");
    
//    UserDto userDto = new UserDto();
//    userDto.setFirstName("Alley");
//    userDto.setLastName("Valeigh");
//    userDto.setEmail("valli@gmail.com");
//    userDto.setUserId(UUID.randomUUID().toString());
    UserDto userDto = new ModelMapper().map(userDetailsRequestModel, UserDto.class);
    userDto.setUserId(UUID.randomUUID().toString());
    
    // 🆙🆕
    when(usersService.createUser(any(UserDto.class)))
      .thenReturn(userDto);
    
    RequestBuilder requestBuilder = MockMvcRequestBuilders.post("/users")
      .contentType(MediaType.APPLICATION_JSON)
      .accept(MediaType.APPLICATION_JSON)
      .content(new ObjectMapper().writeValueAsString(userDetailsRequestModel));
    
    //Act
    MvcResult mvcResult = mockMvc.perform(requestBuilder).andReturn();
    String responseBodyAsString = mvcResult.getResponse().getContentAsString();
    UserRest createdUser = new ObjectMapper()
      .readValue(responseBodyAsString, UserRest.class)
    
    //Assert
    Assertions.assertEquals(userDetailsRequestModel.getFirstName(), createdUser.getFirstName(), "The returned user first name is most likely incorrect");
    Assertions.assertEquals(userDetailsRequestModel.getLastName(), createdUser.getLastName(), "The returned user first name is most likely incorrect");
    Assertions.assertEquals(userDetailsRequestModel.getEmail(), createdUser.getEmail(), "The returned user email is incorrect");
    Assertions.assertFalse(createdUser.getUserId().isEmpty(), "userId should not be empty");
  }
}
```

Assert for BAD_REQUEST

```java
@Test
@DisplayName("First name is not empty")
void testCreateUser_whenFirstNameIsNotProvided_returns400StatusCode() throws Exception {
  // Arrange
  UserDetailsRequestModel userDetailsRequestModel = new UserDetailsRequestModel();
  userDetailsRequestModel.setFirstName("");
  userDetailsRequestModel.setLastName("Valeigh");
  userDetailsRequestModel.setEmail("valli@gmail.com"); // #4 "email"
  userDetailsRequestModel.setPasswd("12345678");
  userDetailsRequestModel.setRepeatPasswd("12345678");
  
  RequestBuilder requestBuilder = MockMvcRequestBuilders.post("/users")
      .contentType(MediaType.APPLICATION_JSON)
      .accept(MediaType.APPLICATION_JSON)
      .content(new ObjectMapper().writeValueAsString(userDetailsRequestModel));
  
  // Act
  MvcResult mvcResult = mockMvc.perform(requestBuilder).andReturn();
  // Assert
  Assertions.assertEquals(HttpStatus.BAD_REQUEST.value(), 
                          mvcResult.getResponse().getStatus(), 
                          "Incorrect HTTP Status Code Returned");
}
```

#### Practice exercise solution

這邊要來驗證英文名字輸入小於兩個字的情況，其中 userDetailsRequestModel 在 `@BeforeEach` 階段已經先建立好了

```java
@BeforeEach
void setup() {
  userDetailsRequestModel = new UserDetailsRequestModel();
  userDetailsRequestModel.setFirstName("Alley");
  userDetailsRequestModel.setLastName("Haleigh");
  userDetailsRequestModel.setEmail("ha@gmail.com");
  userDetailsRequestModel.setPasswd("12345677");
}
@Test
@DisplayName("First name cannot be shorter than 2 characters")
void testCreateUser_whenFirstNameIsOnlyOneCharacter_returns400StatusCode() {
  // Arrange
  userDetailsRequestModel.setFirstName("a");  
  //@Size(min=2, message="FirstName must not be less then 2 characters")
  RequestBuilder requestBuilder = MockMvcRequestBuilders.post("/users")
    .content(new ObjectMapper().writeValueAsString(userDetailsRequestModel))
    .contentType(MediaType.APPLICATION_JSON)
    .accept(MediaType.APPLICATION_JSON);
    
  // Act
  MvcResult result = mockMvc.perform(requestBuilder).andReturn();
  
  // Assert
   Assertions.assertEquals(HttpStatus.BAD_REQUEST.value(),
                           result.getResponse().getStatus(), 
                           "HTTP Status code is not set to 400");
}
```

### Introduction --- Integration Test

接下來要寫會整合 RestController (web layer)、Service (service layer)、Repository (data layer) 的測試...

#### `@SpringBootTest` annotation

1. 建立一個UsersControllerIntegrationTest.java
2. 要使Spring能運行整合三個layers的測試，須在類別上添加註解 `@SpringBootTest`
3. 這個標註會要求springboot 在主要程式裡找到 main 方法，即是有被標註 `@SpringBootApplication` 的那個類別裡面的 main() ，並且建立三個layers對應的beans

```java
@SpringBootTest
public class UsersControllerIntegrationTest {
  
}
```

#### `@SpringBootTest` WebEnvironment Mock

可以指定 WebEnvironment.**DEFINED_PORT** | **MOCK** | **NONE** | **RANDOM_PORT**

如果沒有指定的話，預設是用 MOCK 環境，這樣 spring 會建立 mock server environment, this means it will not have the entire spring application context loaded, only beans related to web layer will be created.

```java
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.MOCK)
public class UsersControllerIntegrationTest {
  
}
```

#### Defined Port Number

1. `WebEnvironment.DEFINED_PORT` 自定義埠號有幾種方法

   1. 在 application.properties 裡面設定

      ```properties
      server.port=7777
      ```

      再寫個簡單的測試方法驗證它是不在 port 8888 啟動，可以在 console 看到 `Tomcat started on port(s): 7777 (htttp) with context path ''...`

      ```java
      @SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.DEFINED_PORT)
      public class UsersControllerIntegrationTest {
        @Test
        void contextLoads() {
          
        }
      }
      ```

   2. 另一種方法是直接把server埠號寫在括號內的屬性裡，如下圖，它會覆蓋掉寫在 application.properties 設定檔的內容，具有比較高的優先層級

      如果你要寫多個屬性，就用大括號括起來，每個屬性用雙引號括起來，再以逗號相隔

      ```java
      @SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.DEFINED_PORT, properties = {"server.port=7070", "hostname=192.168.0.2"})
      public class UsersControllerIntegrationTest {
        // some methods annotated with @Test
      }
      ```

#### `@TestPropertySource` 加載替代配置檔

有時候測試專案需要用到 alternative configuration，例如app可能需要在不同的埠號上運行，或者以其他的用戶身份連接到資料庫，甚至是連接到不同的資料庫，這些情況可以透過`@TestPropertySource` 標註來達成

要先從 `@SpringBootTest` 刪除 properties 屬性，加上 `@TestPropertySource`

@TestPropertySource 可以：

1. 手動覆蓋某些配置屬性
2. 加入另外一個配置檔案路徑，使用 locations

```java
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.DEFINED_PORT)
@TestPropertySource(locations = "/application-test.properties",
                   properties = "server.port=7170")
public class UsersControllerIntegrationTest {
  // some methods annotated with @Test
}
```

建立 application-test.properties，如下內容

```properties
server.port=2332
```

如果要能夠存取配置檔的值，可以將之inject在測試類別裡

@Value("${`_name_of_the_property_`}")

```java
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.DEFINED_PORT)
@TestPropertySource(locations = "/application-test.properties",
                   properties = "server.port=7170")
public class UsersControllerIntegrationTest {
  @Value("${server.port}")
  private int serverPort;
  @Test
  void contextLoads() {
    System.out.println("server.port=" + serverPort);
  }
}
```

測試結果會印出 port 號 7170 而非 2337，因此 properties 屬性值具有 higher precedence than 設定檔

#### Random port number

什麼樣的情境下會使用 SpringBootTest.WebEnvironment.RANDOM_PORT 呢？

- to make the embedded server always start on a random port number
  嵌入式伺服器一直在隨機埠號啟動
- to allow different integration to run on parallel 
  不同整合測試可以平行運行

使用 random_port 的時候，不用再配置檔案定義特定的埠號 (~~server.port~~)

將隨機的端口號 (埠號) 注入到測試類別 --- 使用標註 `@LocalServerPort`

```java
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
public class UsersControllerIntegrationTest {
  @Value("${server.port}")
  private int serverPort;
  @LocalServerPort
  private int localServerPort;
  @Test
  void contextLoads() {
    System.out.println("server.port=" + serverPort);
    System.out.println("local server port=" + localServerPort);
  }
}
```

結果：(1) 可以在 console 看到測試在哪個 port 號啟動

​              (2) 查看 contextLoads ，可看到 server.port 被 Spring 歸 0，local server port 同 console

#### Test create user --- user details JSON

以下程式不用 json string，而是建一個 JSONObject 物件再加入屬性

JSONObject 的 put() 方法可能會拋出 JSONException 例外 

##### prepare and perform HTTP post request

建立一個 HttpHeaders 物件，定義 contentType 以及 accept (list of headers)

接者將 http client side (RestTemplate 物件) 注入到測試類別 

TestRestTemplate v.s. RestTemplate 比較：

1. TestRestTemplate 並沒有繼承 RestTemplate，無法使用正規 RestTemplate 的一些方法
2. 但測試時使用 TestRestTemplate 較方便，尤其是當涉及用戶身份驗證時，需要包括用戶名和密碼
3. TestRestTemplate 的 postForEntity 要傳入三個參數
   1. 連結 uri
   2. 建立的 requestObject `HttpEntity<String>`
   3. The datatype of object that I want to get back as response
      如果放的是 String.class，那麼回傳的就會是 JSON string
      通常會回傳 JSON string，再包成 HttpResponse 給你
      可以改成讓他回傳與被測試的方法回傳一致的物件 (e.g. UserRest.class)
4. ResponseEntity<`_return_type_`> + postForEntity 傳入的第三個參數也改成`_return_type_`
5. 

```java
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
public class UsersControllerIntegrationTest {
  
  @Autowired
  private TestRestTemplate testRestTemplate;
  
  @Test
  @DisplayName("User can be created")
  void testCreateUser_whenValidDetailsProvided_returnsUserDetails() throws JSONException {
    // Arrange
    //String createUserJson = "{\n"+
    //  "    \"firstName\":\"Hallie\",\n" +
    //  "    \"lastName\":\"Chamber\",\n" +
    //  "    \"email\":\"chahaleigh@test.coco\",\n" +
    //  "    \"passwd\":\"12335677\",\n" +
    //  "    \"repeatPasswd\":\"12335677\"\n" +
    //  "}";
    JSONObject userDetailsRequestJson = new JSONObject();
    userDetailsRequestJson.put("firstName", "LingJin");
    userDetailsRequestJson.put("lasttName", "Lindsey");
    userDetailsRequestJson.put("email", "ll@test.coco");
    userDetailsRequestJson.put("passwd", "12335677");
    userDetailsRequestJson.put("repeatPasswd", "12335677");
    
    HttpHeaders headers = new HttpHeaders();
    headers.setContentType(MediaType.APPLICATION_JSON);
    heades.setAccept(Arrays.asList(MediaType.APPLICATION_JSON));
    
    HttpEntity<String> request = new HttpEntity<>(userDetailsRequestJson.toString(), headers);
    
    // Act
    ResponseEntity<UserRest> createdUserDetailsEntity = testRestTemplate.postForEntity("/users", request, UserRest.class);
    UserRest createdUserDetails = createdUserDetailsEntity.getBody();
    
    // Assert
    Assertions.assertEquals(HttpStatus.OK, createdUserDetailsEntity.getStatusCode());
    Assertions.asserEquals(userDetailsRequestJson.getString("firstName"),
                        createdUserDetails.getFirstName(),
                        "Returned user's first name seems to be incorrect");
  }
}
```

#### Test JWT is required

發送一個 http get request，使用 `/users` 並預期回傳一個users列表，通常這個 http request 是受保護的

意思是指，要與這個 API 溝通的話，http request 須要包含一個 valid authorization access token，如果不具備此 token則會得到狀態 403 Forbidden (即 server understands the request but it refuses to authorize it.)

如何取得 valid access token? 

首先可以寫測試驗證 API 端點是不是需要 access token

TestRestTemplate 的 exchange 接收四個參數：

1. uri

2. Get http request

3. requestEntity that was just prepared in arrange section

4. to specify that I want to get back a list of object of specific data type

   `ParameterizedTypeReference<>`

5. 

```java
@Test
@DisplayName("GET /users requires JWT")
void testGetUsers_whenMissingJWT_returns403() {
  // Arrange
  HttpHeaders headers = new HttpHeaders();
  headers.set("Accept", "application/json");
  
  HttpEntity requestEntity = new HttpEntity(headers);
  //HttpEntity requestEntity = new HttpEntity(null, headers); //➡️不含requestBody
  
  // Act
  ResponseEntity<List<UserRest>> response = TestRestTemplate.exchange("/users", 
                            HttpMethod.GET, 
                            requestEntity, 
                            new ParameterizedTypeReference<List<UserRest>>() {
                            });
  // Assert
  Assertions.assertEquals(HttpStatus.FORBIDDEN, 
                          response.getStatusCode(), 
                          "Http status code 403 should have been returned");
}
```



##### Test User Login works

建立一個 JSONObject 加入屬性，並放進 request 物件

比較 TestRestTemplate 的 `postForEntity()` 以及 `postForObject()` 方法：

1. 一樣都是 post request
2. postForEntity 回傳 ResponseEntity\<T>，可以讀取狀態碼(200 or 403)
   postForObject 回傳 T 

postForEntity 收三個參數

1. URL: 這裏的連結與 WebSecurity.java 裡面配置的 uri 相同

2. HttpEntity for request

3. expected return type

   這裏如果方法成功，會驗證 statusCode 是否為 200，
   且 HttpResponse 包含方法 successfulAuthentication() 加上的兩個 header
   但方法 successfulAuthentication() 回傳 void，所以填 null

```java
@Test
@DisplayName("/login works")
void testUserLogin_whenValidCredentialsProvided_returnsJWTinAuthroizationHeader() throws JSONException {
  // arrange
  String loginCredentialsJson = "{\n" +
    "    \"email\":\"himalaya@test.co\",\n" +
    "    \"passwd\":\"12335677\",\n" +
    "}";
  JSONObject loginCredentials = new JSONObject();
  loginCredentials.put("email", "himalaya@test.co");
  loginCredentials.put("passwd", "12335677");
  
  HttpEntity<String> request = new HttpEntity<>(loginCredentials.toString());
  
  // act
  ResponseEntity response = testRestTemplate.postForEntity("/users/login", 
                                                           request, 
                                                           null);
  // assert
  Assertions.assertEquals(HttpStatus.OK, 
                          response.getStatusCode(), 
                          "HTTP status code should be 200");
  Assertions.assertNotNull(response.getHeaders().
                           getValuesAsList(SecurityConstants.HEADER_STRING).get(0),
                           "Response should contain Authorization header with JWT");
  Assertions.assertNotNull(response.getHeaders().
                           getValuesAsList("UserID").get(0),
                           "Response should contain UserID in a response header");
}
```
Spring framework 給 FilterProcessesUrl 的預設值是 ("/login")，這裏專案作者改為 `/users/login`

```java
// com.exampleSergey.tutorial.security.WebSecurity.java
  @Bean
  public SecurityFilterChain configure(HttpSecurity http) throws Exception {
    // Configure AuthenticationManagerBuilder
    // ...
    // Get AuthenticationManager
    // ...
    http.headers().frameOptions().disable();
    return http.build();
  }
  protected AuthenticationFilter getAuthenticationFilter(AuthenticationManager authenticationManager) {
    final AuthenticationFilter filter = new AuthenticationFilter(authenticationManager);
    filter.setFilterProcesUrl("/users/login");
    return filter;
  }
  
```

以下是主程式生成 http access token 的程式碼	

```java
// com.exampleSergey.tutorial.security.AuthenticationFilter.java
  @Override
  protected void successfulAuthentication(HttpServletRequest req,
                                         HttpServletResponse res,
                                         FilterChain chain,
                                         Authentication auth) throws IOException, ServletException {
    String userName = ((UserDetails) auth.getPrincipal()).getUsername();
    String token = Jwts.builder()
      .setSubject(userName)
      .setExpiration(new Date(System.currentTimeMillis() + (long) 864000000))
      .signWith(SignatureAlgorithm.HS512, SecurityConstants.TOKEN_SECRET)
      .compact();
    UsersService userService = (UsersService) SpringApplicationContext.getBean("usersService");  
    UserDto userDto = userService.getUser(userName);
    
    res.addHeader(SecurityConstants.HEADER_STRING, SecurityConstants.TOKEN_PREFIX); //?
    res.addHeader("UserID", userDto.getUserId());
  }
```

```java
// com.exampleSergey.tutorial.security.SecurityConstants
public class SecurityConstants {
  public static final String TOKEN_PREFIX = "Bearer ";
  public static final String HEADER_STRING = "Authorization";
  public static final String TOKEN_SECRET = "qaWr3Dwa";
}
```

#### Order Test Methods

指定一個測試類別中，每個單元測試執行的順序

1. user to be created
2. peform user login and get access token
3. test communication with protected api

使用 `@TestMethodOrder`類別標註 + `@Order(x)`方法標註達成

```java
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
@TestMethodOrder(MethodOrdered.OrderAnnotation.class)
@TestInstance(TestInstance.Lifecycle.PER_CLASS) //authorizationToken變數才能test#3,4共用
public class UsersControllerIntegrationTest {
  private String authorizationToken;
  @Test
  @Order(1)
  void testCreateUser_xxx_returnXXX() {...}
  
  @Test
  @Order(2)
  void testGetUsers_whenMissingJWT_returns403() {...}
  
  @Test
  @Order(3)
  void testLogin_whenValidCredentialsProvided_returnsJWTinAuthorizationHeader() {
    // ...
    authorizationToken = response.getHeaders().
      getValuesAsList(SecurityConstants.HEADER_STRING).get(0);
    // ... 
  }
  
  @Test
  @Order(4)
  void testGetUsers_whenValidJWTProvided_returnsUsers() {
    // Arrange
    HttpHeaders headers = new HttpHeaders();
    headers.setAccept(Arrays.asList(MediaType.APPlICATION_JSON));
    headers.setBearerAuth(authorizationToken); 
    //authorizationToken 這個值在Test#3 取得，所以需要定義 class level variable
    /*❗️另外，Spring預設給每個測試方法一個instance，所以 test#4 開始時，
        這個類別變數又會是null，所以須要更改Test instance lifecycle */
    HttpEntity requestEntity = new HttpEntity(headers);
    // Act
    ResponseEntity<List<UserRest>> response = testRestTemplate.exchange("/users", 
            HttpMethod.GET, 
            requestEntity, 
            new ParameterizedTypeReference<List<UserRest>() {
    });
    
    // Assert
    Assertions.assertEquals(HttpStatus.OK, 
                            response.getStatusCode(), 
                            "HTTP status code should be 200");
    Assertions.assertTrue(response.getBody().size() == 1, 
                          "should be exactly 1 user in the list");
  }
}
```

--- END of course ✅ ---