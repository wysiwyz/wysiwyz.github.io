---
title: "1Z0-819 曾師筆記 (2/duology)"
date: 2023-08-05T12:11:34+08:00
author: "celine"
tags: ["Java", "1Z0-819"]
categories: ["StudyNote"]
---

	# 01 泛型和集合物件

## 泛型

Java 5 之後加入泛型，使型別使用多了另一種彈性。

集合物件（用來裝填物件）＋泛型，可以限制裝填物件的型別。

### 使用泛型的效益

1. 提供更彈性的「型別安全 type safety」檢查機制，原本在執行時才能發現的型別錯誤，現在在編譯時期就可以預發現
2. 在集合物件 Collections 裡大量使用，限制內涵物件之型別
3. 減少轉型 casting 需要，使程式碼更簡潔

### 使用泛型設計類別

+ 可以將程式碼裡的符號 `T`換成 `String`( 即 UseString())，或換成 `Shirt` (即 UseShirt())
+ 常見的符號及表示方式如下：
  1. **T** -「型別（type）」 
  2. **E** -「成員（element）」
  3. **K** -「鍵 - 值對裡的鍵（key）」
  4. **V** -「鍵 - 值對裡的值（value）」

```java
class UseAny<T> {
    private T t;
    public void add(T t) {
        this.t = t;
    }
    public T get() {
        return this.t;
    }
}

class UseString {
    private String message;
    public void add(String message) {
        this.message = message;
    }
    public String get() {
        return this.message;
    }
}

class UseShirt {
    private Shirt shirt;
    public void add(Shirt shirt) {
        this.shirt = shirt;
    }
    public Shirt get() {
        return this.shirt;
    }
}
```

+ Java 7 開始，取消「參考型別」和「建構子」都必須在<>符號內加上置換型別的規定，因為等號右側可以由前者推斷（inference）而知其型別

  ```java
  UseAny<String> shirt2 = new UseAny<Shirt>();
  UseAny<String> msg2 = new UseAny<String>();
  // see below for simplified code
  UseAny<String> shirt2 = new UseAny<>();
  UseAny<String> msg2 = new UseAny<>();

## 集合物件

### Collection 定義與種類

集合物件 *Collection* 相較陣列具備更多管理功能：

1. 以 interface Collection 為代表
2. 集合內物件 `elements`，簡寫為 `E`
3. 集合內物件必須為參考型別或基本型別的包裹類別 *wrapper class*
4. 有多種常見資料結構，例如 `stack`、`queue`、`dynamic array` 等
5. 大量使用泛型 `generic`
6. 都屬於 `java.util.*` package
7. interface Collection 繼承了 interface Iterable，因此所有集合物件都具備使用 Iterator (疊代器) 的能力

![image-20221214143907275](https://i.imgur.com/Dm02pU2.png)

### List

集合物件底下最常使用的介面，具備 index 能依照放入先後區分順序 *order*

1. 新增 element，使用 index 指定插入位置
2. 新增 element，直接加到尾端
3. 取得 element index
4. 使用 index 移除或覆寫成員
5. 取得 List 長度

### `ArrayList` : List 最常使用的一種實作類別

+ 特色：
  1. 行為和陣列 *array* 相近，但長度可以自動成長，又稱為「動態陣列」(dynamic array)
  2. 使用 index 新增 / 存取 / 修改 element
  3. 可以用 index 區分，所以允許重複成員
+ 未搭配泛型設計的 List
  1. 使用 Iterator 取出的物件必須轉型
  2. 錯放成員時，在執行時期才能知道 (e.g. add Integer, add Integer, and then add String)

### 自動裝箱 `Boxing`和開箱 `Unboxing`

1. Primitive → Wrapper Class : 把基本型別裝箱 *Boxing*

2. Wrapper Class → Primitive : 將基本型別由包裹類別的箱子裡取出，開箱 *Unboxing*

3. 在迴圈內使用 boxing / unboxing 會讓效能耗損增幅

   ```java
   Integer partNumObj = elements.next();
   int partNum = partNumObj.intValue();
     // see below for concised code
   int partNum = elements.next();
   ```

### Set

1. 其成員 element 必須為獨一無二（unique），不能重複

2. 沒有 index

3. 若放入重複 element，不會出錯，但無效

4. 常使用 `HashSet` 實作類別。`TreeSet` 類別會依物件特性自動排序

5. element 是否唯一，或是排序先後，取決於方法 `equals()` 和 `hashCode()` 的覆寫結果

   ```java
   public class TestSet {
       public static void testHashSet() {
           Set<String> set = new HashSet<>();
           set.add("uno");
           set.add("deux");
           set.add("trois");
           set.add("trois");
           for (String s : set) {
               System.out.println("item: " + s);
               // only prints "trois" once
           }
       }
       
       public static testTreeSet() {
           Set<String> set = new TreeSet<>();
           set.add("uno");
           set.add("deux");
           set.add("trois");
           set.add("trois");
           set.add("turban");
           for (String s : set) {
               System.out.println("item: " + s);
               // only prints "trois" once
               // print by String alphabetical order: d, tr-, tu-, u
           }
       }
   }
   ```

### Deque (Queue)

Interface Deque 繼承了介面 Queue，特色如下：

1. **Double-Ended Queue** : 具備兩端點的 Queue
2. 可同時使用於 Stack 和 Queue 兩種資料結構，只要呼叫不同方法
   + 使用`add()`、`remove()`時：Deque 物件表現出 Queue 資料結構的行為 (FIFO)
   + 使用`push()`、`pop()`時：Deque 物件表現出 Stack 資料結構的行為 (FILO)![image-20221214161745531](https://i.imgur.com/U7FO4qK.png)

3. `Deque<String> deque = new ArrayDeque<>(); `

## Map

+ Map 是 key - value 成對集合，但不屬於 Collection 集合物件家族
  1. **key 物件** : 用來尋找 value 物件，每個 key 物件須為獨特而不重複的
  2. **value 物件** : 和 key 物件有關連性（associative）

+ 在其它語言中，Map 又稱「關聯性陣列 associative arrays」，key 可構成一陣列，value 可構成另一個陣列，兩陣列有著關聯性

+ Map 泛型 <`K`, `V`>

+ Map 基本用法 

  + `map.values()` 取得 `Collection<T of V>` 
  + `map.keySet()` 取得 `Set<T of K>` (因為 keys 不能重複，回傳 Set 而非 Collection)

+ Map 並未繼承於 Collection interface，Map family 如下：

  ![image-20221214165500408](https://i.imgur.com/talB7Y5.png)

+ Map family 常用類別：

  1. **TreeMap** : keys 自動依順序排序

  2. **HashTable** : *執行緒安全*，且 *keys 和 values 不允許為 null*

  3. **HashMap** : ***非**執行緒安全*，且 *keys 和 values **可**為 null* 

     > 執行緒安全 : 
     >
     > 一個物件被一個執行緒使用和同時被多個執行緒使用時，行為或結果都一致，不會產生非預期結果

> 🍪☕ **LittleTips** : `Set` 和 `Map` 兩者都有可以支援排序的分支 
>
> 1. 支援排序的分支起源皆以 `Sorted` 作為前綴。例：**Sorted**Map 、 **Sorted**Set (interface)
> 2. 實作類別都是以 `Tree` 為開頭。例：**Tree**Map 、 **Tree**Set

## 集合物件成員的排序

### 排序作法

如何排序物件類別，如何定義順序，一個類別可以定義多個排序標準嗎？有以下兩個介面可選擇

1. Comparable 介面 - 實作 `compareTo()` 方法
2. Comparator 介面 - 實作 `compare()` 方法

兩種方法都回傳一個整數，表示比較結果：

1. 回傳整數 `= 0`：兩者相等
2. 回傳整數 `< 0`：表示「自己（this）」**小於（數值上）**或 **先於（順序上）**「方法參數物件」
3. 回傳整數 `> 0`：表示「自己（this）」**大於（數值上）**或 **後於（順序上）**「方法參數物件」

```java
Calendar today = Calendar.getInstance();
Calendar tomorrow = Calendar.getInstance();
tomorrow.add(Calendar.DATE, 1);
out.println(today.compareTo(tomorrow));   // 字串 today 先於 tomorrow，所以回傳-1
out.println("A".compareTo("B"));          // 字串"A"早於字串"B"，所以回傳-1
out.println(Integer.valueOf(5).compareTo(Integer.valueOf(6)));
```

### 使用 `Comparable` 介面排序

```java
public interface Comparable<T> {
    public int compareTo(T o);
}
```

特色：

1. 支援泛型設計

2. 必須實作 `compareTo()` 方法，比較自己（this）和方法參數物件

3. 一個 class 只能實作一次 Comparable 介面，所以只能提供單一方式排序，可用於 `TreeSet` 和 `TreeMap` 等實作類別，或需要物件之間比較的地方

4. 決定該物件類別要以哪一個條件決定 Student 物件的排序先後，再讓該類別實作 Comparable 介面

   1. 必須提供 `compareTo()` 方法的內容
   2. 只能提供一種排序選擇

   ```java
   public class Student implements Comparable<Student> {
       ...
       @Override
       public int compareTo(Student s) {
           // use method delegation 方法委派 
           int sortById = Long.valueOf(this.id).compareTo(s.id);
           int sortByName = this.name.compareTo(s.getName());
           int sortByScome = Double.valueOf(this.score).compareTO(s.score);
           return sortById;
       }
   }
   ```
   
   

### 使用 `Comparator` 介面排序

+ 實作 `Comparable` interface 的類別只有提供一次 `compareTo()` 方法內容的機會，故只有一種排序能力

+ 使用 `Comparator` interface 則可以提供多種選擇

  ```JAVA
  public interface Comparator<T> {
      int compare(T o1, T o2);
  }
  ```

+ 特色：

  1. 支援泛型設計

  2. 需實作 `compare()`，用以比較「第一個參數物件」和「第二個參數物件」

  3. 可藉由提供多種 Comparator 類別，達成多種排序方式。

     + 常用於搭配以下方法，以幫助 List 成員排序

       ```java
       public static <T> void sort(List<T> list, Comparator<? super T> c) {
           list.sort(c);
       }
       ```

       ```java
       class NameSorter implements Comparator<Student> {
           public int compare(Student s1, Student s2) {
               return s1.getName().compareTo(s2.getName());
           }
       }
       class ScoreSorter implements Comparator<Student> {
           public int compare(Student s1, Student s2) {
               return Double.valueOf(s1.getScore()).compareTo(s2.getScore());
           }
       }
       public class TestComparator {
           private static void showList(List<Student> studentList) {
               for (int i = 0; i < studentList.size(); i++) {
                   System.out.println("index#" + i + ": " + studentList.get(i));
               }
           }
           public static void main(String[] args) {
               List<Student> studentList = new ArrayList<>(3);
               studentList.add(new Student("Thomas", 1, 3.8)); 
               studentList.add(new Student("John", 2, 3.9)); 
               studentList.add(new Student("George", 3, 3.4)); 
               
               System.out.println("\n------ Original ------");
               showList(studentList);
               
               System.out.println("\n----- Sort by name -----");
               Comparator<Student> sortName = new NameSorter();
               Collections.sort(studentList, sortName);
               showList(studentList);
               
               System.out.println("\n----- Sort by score -----");
               Comparator<Student> sortScore = new ScoreSorter();
               Collections.sort(studentList, sortScore);
               showList(studentList);
           }
       }
       ```



## 使用 `of()` 與 `copyOf()` 方法建立 `List`、`Set` 與 `Map` 物件

### 使用`of()`方法建立 List, Set, Map 物件

+ 自 Java 9 開始，除了傳統使用 new 呼叫子類別建構子以建立 List, Set, Map 物件外

  + 導入靜態工廠方法 `of()` 來建立不可改變（immutable）的物件

    + `List.of()` | `Set.of()` | `Map.of()`

      ```java
      List<T> list = List.of(t1, t2, ...);
      Set<T> set = Set.of(t1, t2, ...);
      Map<K, V> map = Map.of(key1, value1, key2, value2, ...);
      ```

    + line 5, 15, 26 : 以 `of()` 建立物件後，若再嘗試新增成員（`list.add(...)`、`set.add(...)`、`map.put(...)`），都會拋出 ***java.lang.UnsupportOperationException***

    + line 9, 19, 30 : 如果要將不可更改的物件轉換為可更改物件，可將其作為 `ArrayList`、`HashSet`、`HashMap` 等建構子的參數，重新建立可更改物件

      ```java
      private static void createImmutablesByOf() {
          // List.of()
          List<String> list1 = List.of("i1", "i2", "i3");
          try {
              list1.add("i4"); // line 5 : UnsupportedException
          } catch (Exception e) {
              e.printStackTrace();
          }
          List<String> list2 = new ArrayList<>(list1);  // line 9 : mutable w/ ArrayList
          list2.add("i4");
          System.out.println(list2);
          // Set.of()
          Set<String> set1 = Set.of("i1", "i2", "i3");
          try {
              set1.add("i4"); // line 15 : UnsupportedException
          } catch (Exception e) {
              e.printStackTrace();
          }
          Set<String> set2 = new HashSet<>(set1);  // line 19 : mutable w/ HashSet
          set2.add("i4");  
          System.out.println(set2);
          // Map.of()
          Map<String, Employee> map1 = 
                  Map.of("jim", new Employee("jim"), "duke", new Employee("duke"));
          try {
              map1.put("bill", new Employee("bill")); // line 26 : UnsupportedException
          } catch (Exception e) {
              e.printStackTrace();
          }
          Map<String, Employee> map2 = new HashMap<>(map1);  // line 30 : mutable w/ HashMap
          map2.put("bill", new Employee("bill"));
          System.out.println(map2);
      }
      ```

      

### 使用`copyOf()`方法建立 List, Set, Map 物件

+ 從 Java 10 開始，又導入另一個靜態工廠方法 `copyOf()`，可以建立「**不可改變**」的「**副本物件**」

  ```java
  private static void createImmutablesByCopyOf() {
      // List.copyOf()
      List<String> list1 = List.of("i1", "i2", "i3");
      List<String> list2 = List.copyOf(list1);     // immutable copy
      try {
          list2.add("i4");     // UnsupportedOperationException 
      } catch (Exception e) {
          e.printStackTrace();
      }
      // Set.copyOf()
      Set<String> set1 = Set.of("i1", "i2", "i3");
      Set<String> set2 = Set.copyOf(set1);     // immutable copy
      try {
          set2.add("i4");     // UnsupportedOperationException
      } catch (Exception e) {
          e.printStackTrace();
      }
      // Map.copyOf()
      Map<String, Employee> map1 = 
              Map.of("jim", new Employee("jim"), "duke", new Employee("duke"));
      Map<String, Employee> map2 = Map.copyOf(map1);     // immutable copy
      try {
          map2.put("bill", new Employee("bill"));     // UnsupportedOperationException
      } catch (Exception e) {
          e.printStackTrace();
      }
  }
  ```

  

### 使用 `Arrays.asList()`建立 List 物件

List 另一種以「單個元素成員」作為參數建立 List 物件的方式

```java
List<T> list = Arrays.asList(t1, t2, ...);
```

使用 `Arrays.asList()` 建立的 List 物件和陣列（Array）相似，特性：

1. 長度固定，不可以新增/刪除成員（參考 line 6）

2. 可以修改成員（參考 line 4）

   ```java
   private static void createList() {
       List<String> list1 = Arrays.asList("i1", "i2", "i3");
       try {
           list1.set(0, "iil");    // line 4 : element updatable
           System.out.println(list1);
           list1.remove(0);        // line 6 : removing or adding not permitted
       } catch (Exception e) {
           e.printStackTrace();
       }
       List<String> list2 = new ArrayList<>(list1);
       list2.remove(0);
       System.out.println(list2);
   }
   ```

   

---

# 02 例外與斷言

## 例外

值得信賴的程式會優雅的處理例外狀況：

1. 處理目標是「exception（例外）」，非預期狀況
2. 例外必須處理以建立可信賴的程式
3. 發生原因可能是程式 bugs
4. 發生原因可能是程式無法處理的狀況
   + 資料庫無法連線
   + 硬碟毀損

C語言發生錯誤的話，通常以**回傳負值**表示，例如 `int x = printf("hi")`

Java 則在出現錯誤時，由 JVM 拋出例外物件，不同種類的例外，有不同處理方式

![image-20221215142633549](https://i.imgur.com/BHInvNP.png)

![image-20221215160311402](https://i.imgur.com/xYQBNxF.png)

+ 當類別方法呼叫其他類別方法時，如果被呼叫的方法已宣告有拋出 **checked exception** 的風險，編譯器會要求呼叫者方法必須處理（handle）或是也宣告（declare）可能發生的問題：
  1. **Handling Exception**：表示必須有程式碼區塊來處理異常狀況，此時使用「try-catch」敘述
  2. **Declaring Exception**：在方法上註記執行可能出現的錯誤，提醒使用的方法必須處理，此時使用「throws」宣告

### 使用`try-catch`程式碼區塊

#### 一般例外狀況

```java
public static void tryCatchTest() {
    try {
        System.out.println("Opening a file...");
        InputStream in = new FileInputStream("lostFile.txt");
        System.out.println("File is opened");
    } catch (Exception e) {
        e.printStackTrace();
    }
}
```

+ `catch`程式碼區塊必須傳入 `java.lang.Exception` 或 `java.lang.Throwable` 的子類別參考

+ 其中`java.lang.Throwable` 是例外始祖，如下：

  ```java
  try {
      // ...
  } catch (Exception e) {
      e.printStackTrace();
  }
  ```

+ 把 catch 當成一種方法，則後面的 () 代表要傳入的參數

+ 傳入大分類的型別（父類別或介面）為多型的應用，但不適用於此，例外處理應該對症下藥

+ catch 方法只讓 Java 在程式遇到錯誤時呼叫 / 傳入例外物件

+ 捕捉例外後：

  1. 紀錄錯誤訊息
  2. 重試一次
  3. 嘗試其它替代方案
  4. 離開（return）或結束程式（exit）

#### 複雜例外狀況

+ 一個程式碼區塊 / 方法，必須同時處理多種可能的例外狀況：

  1. 單一「try」搭配多個「catch」：例外子類別排序應該在父類別上面，避免所有例外一開始就被例外父類別（Exception, Throwable）攔截

  2. 捕捉（catch）物件 Exception 時，盡可能捕捉最特定（specific type）的例外子類別

  3. Java Persistence API（JPA）例外大部分繼承 RuntimeException，屬 unchecked exception

     + 慣例上為不用處理的意外，但正式環境裡還是應該處理

       ```java
       public void tryCatch() {
           try {
               System.out.println("Opening a file...");
               InputStream in = new FileInputStream("lostFile.txt");
               System.out.println("File is opened");
               int data = in.read();
               in.close();
           } catch (FileNotFoundException e) {
               e.printStackTrace();
           } catch (IOException e) {
               e.printStackTrace();
           } catch (Exception e) {
               e.printStackTrace();
           }
       }
       ```

#### 紀錄（Logging）錯誤內容

+ 正式環境中，應移除 `printStackTrace()` 或 `System.out.println(e.getMessage())`此類程式碼
+ 執行錯誤時，應該寫入紀錄 / 日誌檔（log file），相關函式庫
  1. Simple logging facade for Java SLF4J
  2. Apache's Log4j
  3. Built-in java.util logging framework

#### 使用`finally`敘述

+ 使用外部資源，例如開啟檔案或連線資料庫，應該在不使用時關閉資源

+ 如果在 try 區塊中關閉資源，可能因為執行錯誤而導致資源開了來不及關，此時可用 finally 敘述

  1. 不管是 try 或者 catch 執行結束，都一定會進入 finally 程式碼區塊

  2. 有時在 finally 區的程式碼也可能出錯，因此需要巢狀 try-catch 區塊來處理

     ```java
     public void tryCatchFinally() {
         InputStream in = null;
         try {
             System.out.println("Opening a file...");
             in = new FileInputStream("lostFile.txt");
             System.out.println("File is opened");
             int data = in.read();
             in.close();
         } catch (FileNotFoundException e) {
             e.printStackTrace();
         } catch (IOException e) {
             e.printStackTrace();
         } catch (Exception e) {
             e.printStackTrace();
         } finally {
             try {
                 if (in != null)
                     in.close();   // try to close file
             } catch (IOException e) {
                 System.out.println("Failed to close file");
             }
         }
     }
     ```

### 使用 `try-with-resources` 程式碼區塊和 `AutoCloseable` 介面

#### 使用 `try-with-resources` 敘述

+ Java 7 提供新的 `try-with-resources` 敘述，可自動關閉被開啟的「資源（resources）」

  ```java
  try ( 宣告並開啟資源 [; 宣告並開啟其它資源 ...] ) {
      // ...
  }
  // 在 try 程式碼區塊之後，資源將自動關閉
  ```

  1. 這裡定義的資源，必須為實作 `java.lang.AutoCloseable` 介面的類別

  2. 如果要開多個資源，可以使用「**;**」做區隔

  3. 自動關閉的順序將和使用資源的開啟順序相反

     ```java
     public void tryWithResource() {
         System.out.println("Opening a file...");
         try (InputStream in = new FileInputStream("lostFile.txt")) { //
             System.out.println("File is opened");
             int data = in.read();
             in.close();
         } catch (FileNotFoundException e) {
             e.printStackTrace();
         } catch (IOException e) {
             e.printStackTrace();
         } catch (Exception e) {
             e.printStackTrace();
         }
     }
     ```

比較 `tryCatchFinally` 與 `tryWithResource`，可以發現：

1. 移除 `finally` 程式碼區塊，如前範例行 16 - 23
2. 使用 `try-with-resource` 宣告要開啟的資源

#### 認識 `AutoCloseable`介面

```java
public abstract class InputStream implements Closeable {
```

```java
public interface Closeable extends AutoCloseable {
```

+ 資源（resource）要藉由 `try-with-resources‵ 敘述開啟和自動關閉，必須實作以下兩者任一

  1. 介面 *java.lang.AutoCloseable*

     + Java 7 新增

     + 唯一的抽象方法 `close()` 會拋出 Exception 物件

       ```java
       public interface AutoCloseable {
           void close() throws Exception;
       }
       ```

  2. 介面 *java.io.Closeable*

     + 早期的 Java 版本就存在，在 Java 7 中修改使其繼承介面 `AutoCloseable`

     + 唯一的抽象方法 `close()` 會拋出 `IOException` 物件

       ```java
       public interface Closeable extends AutoCloseable {
           public void close() throws IOException;
       }
       ```

**Idempotent method** - 即使重複執行多次，也不會有副作用（side effects）產生

+ `java.io.Closeable` 的 close() 方法必須滿足 idempotent 要求

+ `java.lang.AutoCloseable` 並未一致要求實作之 close() 必須比照辦理

  + 即便如此仍應該要做到反覆執行多次都沒有副作用產生

    ```java
    // 若資源為關閉（以resource == null 判定）
    If (resource != null) {
        // 關閉資源，並使 resource = null
    }
    ```

### Suppressed Exceptions

使用 try-with-resource 也衍伸出新的例外 exception 處理問題必須注意：

1. Java 開啟資源時拋出例外，未成功開啟資源
   + 程式碼直接跳到 catch 區，只拋出一個例外
2. Java 成功開啟資源，但在 try 區拋出例外，在背景關閉資源時又拋出例外
   + 共產生 2 個例外
   + catch 區必須同時接收 2 個例外物件 **衍伸問題**
3. Java 成功開啟資源，try 區塊內任務成功執行，但在關閉資源時出錯
   + 和一般情況一樣，只拋出 1 個例外

+ 當有兩個例外類別被先後拋出，Java會將 **後發生，同時和關閉資源有關** 的例外物件，隱匿/擠壓（suppressed）到「先發生、也是在 try 區塊造成」的例外物件裡，使其可被保留

+ Java 會將資源相關的例外隱匿 / 擠壓進程式碼的例外裡，只要知道如何將 suppressed 例外物件取出（參以下 line 3 ~ line 5）

  ```java
  } catch(Exception e) {
      System.out.println(e.getMessage());
      for (Throwable t : e.getSuppressed()) {  // line 3
          System.out.println(t.getMessage());
      }                                        // line 5
  }
  ```

+ 對例外物件呼叫 `e.getSuppressed()` 方法時，可回傳一個 Throwable 陣列 
  + 在背景被擠壓 / 隱匿的例外，即使再多都能被保存並取出

```java
class TryException extends Exception {
}
class FinallyException extends Exception {
}
public class SuppressedExceptions {
    public static void main(String[] args) {
        before7();
        after7();
    }
    private static void before7() {
        try {
            try {
                throw new TryException();   // This is lost.
            } finally {
                throw new FinallyException();
            }
        } catch (Exception e) {
            System.out.println("before 7: " + e.getClass());
        }
    }
    private static void after7() {
        try {
            Throwable t = null; // 保留第一個拋出的例外物件 TryException (1/2)
            try {
                throw new TryException();
            } catch (Exception e) {
                t = e;  // 保留第一個拋出的例外物件 TryException (2/2)
            } finally {
                FinallyException fe = new FinallyException();
                if (t != null) {
                    t.addSuppressed(fe); 
                    // 將 finallyException 隱匿至 t, 再拋出 tryException
                    throw t;
                } else {
                    throw fe; // 若無 tryException, 單獨拋出 finallyException
                }
            }
        } catch (Throwable e) {
            System.out.println("after7: " + e.getClass());
            for (Throwable t : e.getSuppressed()) {
                System.out.println("after7: " + t.getClass());
            }
        }
    }
}
```



### 使用 `multi-catch` 敘述

不建議直接捕捉例外的父類別，如 Exception 或 Throwable，因為：

1. 每種意外的處理方式應該不同
2. 要清楚知道究竟有多少個例外可能產生

如果每種例外處理方式皆相同，則可以使用 Java 7 `multi-catch` 敘述，優點：

1. 可清楚知道究竟有多少例外可能拋出
2. 多種例外，同一種方式處理，簡潔化程式碼
3. **不同例外以「|」區隔時，前後例外必須「沒有繼承關係」**
   + 例如 Exception class 不能和以 multi-catch 敘述聚集的例外類別放一起

### 使用 `throws` 宣告

+ 在類別方法上宣告 `throws ExceptionTypes`，讓呼叫該方法的 caller 處理

+ 覆寫子類別方法時，若父類別方法宣告拋出例外

  1. **checked exception**，子類別覆寫方法拋出的例外必須：

     + 例外型別必須相同，或者為其子類別（覆寫後有精進）
     + 數量相同或更少（表示問題已被處理 improved）

  2. **unchecked exception**：子類別覆寫方法時可不予理會，例如 `RuntimeException`

     ```java
     abstract class Father {
     	abstract void fatherMethod1() throws IOException;
     	abstract void fatherMethod2() throws RuntimeException;
     	abstract void fatherMethod3() throws SQLException;
     }
     
     class Child extends Father {
     	@Override
     	void fatherMethod1() throws IOException, FileNotFoundException {		
     	}  // FileNotFoundException 為 IOException 子類別，未超出 IOException 範圍
         
     	@Override
     	void fatherMethod2() {  
     	} // 父類別 RuntimeException-UncheckException，子類別可以不處理
         
     	@Override
     	void fatherMethod3() {		
     	} // SQLException 在方法內使用 try-catch 敘述，妥善處理不再拋出例外
     }
     ```

### 建立客製的 `Exception` 

#### 客製化的例外類別

建立客製化的例外子類別 DAOException，繼承 class Exception

```java
public class DAOException extends Exception {
    public DAOException() {
        super();
    }
    public DAOException(String message) {
        super(message);
    }
}
```

+ 標準 Java 不會主動拋出客製化例外子類別，必須先捕捉標準的例外類別，再拋出客製化例外子類別

  ```java
  try {
      // some codes that might cause error
  } catch (Exception e) {
      e.printStackTrace();
      throw new DAOException();
  }
  ```

#### 客製化包裹例外類別（Wrapper Exception）

+ 如果希望再拋出的客製化例外子類別也能保留最初被捕捉的例外類別訊息，可使用 wrapper 例外類別，將最初的例外類別包裹在客製例外類別中

  + 設計客製例外類別

    ```java
    public class DAOException extends Exception {
        public DAOException(Throwable cause) {
            super(cause);
        }
        public DAOException(String msg, Throwable cause) {
            super(msg, cause);
        }
    }
    ```

  + 將捕捉真實例外類別作為客製例外類別的建構子參數

    ```java
    try {
        // some codes might error
    } catch (Exception e) {
        e.printStackTrace();
        throw new DAOException(e);
    }
    ```

  + 使用 `getCause()` 方法取出被包裹的原始例外物件

    ```java
    try {
        // some codes might cause error
    } catch (DAOException e) {
        Throwable t = e.getCause();
    }
    ```

  + 使用包裹例外型別提供解決範例
  
    1. 先設計介面的抽象方法並宣告拋出 `DAOException`：
  
       ```JAVA
       Employee findById(int id) throws DAOException;
       ```
  
    2. File-based 的方法實作方式：
  
       ```java
       public Employee findById(int id) throws DAOException {
           try {
               
           } catch () {
               throw new DAOException(e);
           }
       }
       ```
  
    3. JDBC-based 的方法實作方式：
  
       ```java
       public Employee findById(int id) throws DAOException {
           try {
               return getEmployeeFromDatabase(int id);
           } catch (SQLException e) {
               throw new DAOException(e);
           }
       }
       ```



## 斷言

### Assertions 的簡介和語法

+ 斷言若失敗被認為是嚴重的問題，表示程式執行結果和預期有出入
+ 程式會拋出 AssertionError 並中斷程式執行，AssertionError 為 unchecked exception。

```java
// Syntax
assert <boolean_expression>;
assert <boolean_expression> : <detail_expression>;
```

1. <boolean_expression> 若為 false，將拋出 `AssertionError`
2. <detail_expression> 為捕捉 `AssertionError` 後呼叫 `getMessage()` 方法回傳的子串

#### Assertions 使用情境

+ 使用 Assertions 來驗證假設和方法的不變量（不會改變的數值或結果，invariant），通常情況：
  1. 內部的不變量（internal invariants）
  2. 流程控管的不變量（control flow invariants）
  3. 事後的狀態和類別不變量（post-conditions & class invariants）

#### Assertions 的使用注意事項

+ Assertions 的檢查預設關閉（disabled），使用前必須開啟

+ 要避免不當的使用方式：

  1. 不可用於類別方法的參數輸入檢查

  2. 不可影響程式正常流程

     ```java
     // 將物件的生成放在 assertion 的判斷敘述中
     SomeType s = null;
     assert (s = new SomeType()) != null;
     ```

### Assertions 的使用

#### 內部的不變量 *internal invariants*

+ 在 else block 加上 `assert == 0`，使 x 無負值的可能

  ```java
  if (x > 0) {
      // do if x > 0
  } else {
      assert (x == 0);
  }
  ```

#### 流程控管的不變量 *control flow invariants*

+ 為始 switch case 不可能進入 default block，直接在 default block 裡使用 assert false

  + 表示程式進到此不用再以 boolean 表達式判斷，而是馬上拋出 `AssertError`

    ```java
    private static void controlFlowInvariants(Gender g) {
        switch (g) {
            case MALE:
                // do something
                break;
            case FEMALE:
                // do something
                break;
            default:
                assert false: "Unknown gender!!";
                break;
        }
    }
    ```

#### 事後的狀態和類別不變量 *post-conditions & class invariants*

+ 無論類別欄位經過任何改變，都應該能通過方法 rule() 的檢驗

  ```java
  class MyTime {
      int hours;
      int minutes;
      int seconds;
      void rule() {
          assert(0 <= hours && hours < 24);
          assert (0 <= minutes && minutes < 60);
          assert (0 <= seconds && seconds < 60);
      }
  }
  ```

#### Assertions 的開啟與關閉

+ Assertions 預設是關閉，關閉後**完全不會執行**，和註解（comment）類似，不影響效能

  ```terminal
  java -enableassertions HelloWord
  java -ea HelloWorld
  ```

+ `-ea`即是 `-enableassertions`之縮寫

+ 若在 `-ea` 後加上其它選項，可控制 **Assertions 的啟用只在某個 package 或 class**

  [Enable and Disable Assertions](https://docs.oracle.com/javase/7/docs/technotes/guides/language/assert.html#enable-disable)

---

# 03 輸入與輸出

## I/O 基礎

+ 當輸入/輸出行為發生時，好比串流（stream）的流動，流入或流出某個地方
+ 串流需要有來源及目的，ex. 主控台視窗（console）、檔案、資料庫、網路、其它程式

### 何謂 I/O

基本認知：

1. 資料的進出像是水流 / 串流
   + 來源流向目的，具有方向性，Java 中稱之為 `stream (串流)`
   + 流動內容主要分為「位元（byte）」和「字元（character）」
2. 水流的流動，若提供管道，稱為 `channel`，若使用 `channel` 支援 I/O 會更有效率

#### 以 Java 程式為區分基準

1. 輸入串流 | 來源串流（input stream, source stream）

   ![image-20221218122158764](https://i.imgur.com/hz0S8PM.png)

2. 輸出串流 | 目的串流（output stream, sink stream）

   ![image-20221218122209539](https://i.imgur.com/2OgU1l1.png)

#### 以程式開發最常用三種端點區分

1. 檔案（files）和目錄（directories）
2. 主控台（console）：標準輸入（standard-in）和標準輸出（standard-out）
3. Socket 程式（連線遠端系統，需指定 port 通訊）

### 處理串流的類別

依串流內的資料分類、流動方向、處理類別分為四個抽象類別：

| 方向\串流內容 | 位元（byte）   | 字元（character） |
| ------------- | -------------- | ----------------- |
| 輸入 Java     | `InputStream`  | `Reader`          |
| 輸出 Java     | `OutputStream` | `Writer`          |

---

#### Class `InputStream`

| method   | name                                                         | feature                                                      |
| -------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 基本方法 | `int read()`                                                 | 每次讀取 1 個 byte                                           |
| -        | `int read(byte[] buffer)`                                    | 每次讀取一個 byte[]                                          |
| -        | `int read(byte[] buffer, int offset, int length)`            | 每次讀取一個 byte[]，可以指定偏移量（offset）和讀取長度（length） |
| 其它方法 | `void close()`                                               | 關閉 stream                                                  |
| -        | `int available()`                                            | 有多少的 bytes 可供讀取                                      |
| -        | `long skip(long n)`                                          | 讀取時略過 n 個 **bytes**                                    |
| -        | `boolean markSupported()`<br />`void mark(int readlimit)`<br />`void reset()` | 合併用於改變檔案中的讀取位置，特別是回到過去某個指定的讀取位置<br />又稱 push-back 操作 |

---

#### Class `OutputStream`

| method   | name                                                | feature                                                      |
| -------- | --------------------------------------------------- | ------------------------------------------------------------ |
| 基本方法 | `void write(int c)`                                 | 將 int 寫入 `OutputStream`                                   |
| -        | `void write(byte[] buffer)`                         | 將 byte[] 寫入 `OutputStream`                                |
| -        | `void write(byte[] buffer, int offset, int length)` | 寫入 byte[] 到 `OutputStream`，指定長度（length）和偏移量（offset） |
| 其它方法 | `void close()`                                      | 關閉 stream                                                  |
| -        | `void flush()`                                      | 強制將 `OutputStream` 中的資料寫入目的地                     |

---

#### Class `Reader`

| method   | name                                                         | feature                                                      |
| -------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 基本方法 | `int read()`                                                 | 每次讀取 1 個 **char**                                       |
| -        | `int read(byte[] buffer)`                                    | 每次讀取一個 char[]                                          |
| -        | `int read(byte[] buffer, int offset, int length)`            | 每次讀取一個 char[]，可以指定偏移量（offset）和讀取長度（length） |
| 其它方法 | `void close()`                                               | 關閉 stream                                                  |
| -        | `boolean ready()`                                            | 確認 stream 是否已經準備好進行資料讀取                       |
| -        | `long skip(long n)`                                          | 讀取時略過 n 個 **chars**                                    |
| -        | `boolean markSupported()`<br />`void mark(int readAheadLimit)`<br />`void reset()` | 合併用於改變檔案中的讀取位置，特別是回到過去某個指定的讀取位置<br />又稱 push-back 操作 |



---

#### Class Writer

| method   | name                                                | feature                                                      |
| -------- | --------------------------------------------------- | ------------------------------------------------------------ |
| 基本方法 | `void write(int c)`                                 | 將 int 寫入 **Writer**                                       |
| -        | `void write(char[] buffer)`                         | 將整個 char[] 寫入 Writer                                    |
| -        | `void write(char[] buffer, int offset, int length)` | 寫入 char[] 到 Writer，且指定長度（length）和偏移量（offset） |
| 其它方法 | `void close()`                                      | 關閉 stream                                                  |
| -        | `void flush()`                                      | 強制將 Writer 中的資料寫入目的地                             |



### 串流類別的串接

| 功能\串流內容                      | 字元串流　*Character Streams*                 | 位元串流 *Byte Streams*                           |
| ---------------------------------- | --------------------------------------------- | ------------------------------------------------- |
| Buffering（緩衝）                  | `BufferReader`<br />`BufferedWriter`          | `BufferedInputStream`<br />`BufferedOutputStream` |
| Filtering（過濾）                  | `FilterReader`<br />`FilterWriter`            | `FilterInputStream`<br />`FilterOutputStream`     |
| Conversion（位元轉換為字元）       | `InputStreamReader`<br />`OutputStreamWriter` |                                                   |
| Object serialization（物件序列化） |                                               | `ObjectInputStream`<br />`ObjectOutputStream`     |
| Data conversion（資料型態轉換）    |                                               | `DataInputStream`<br />`DataOutputStream`         |
| Counting（計算行數）               | `LineNumberReader`                            | `LineNumberInputStream`                           |
| Printing（列印）                   | `PrintWriter`                                 | `PrintStream`                                     |



### 使用 `java.io.File` 類別

`java.io.File` 常用方法：

| method                  | return type | description                               |
| ----------------------- | ----------- | ----------------------------------------- |
| `getName()`             | String      | 取得檔案或目錄的名稱                      |
| `getParent()`           | String      | 取得父目錄的名稱                          |
| `getParentFile()`       | File        | 取得代表父目錄的 File 物件                |
| `getPath()`             | String      | 取得檔案或目錄的路徑                      |
| `isAbsolute()`          | boolean     | 是否為絕對路徑                            |
| `getAbsolutePath()`     | String      | 取得絕對路徑                              |
| `canRead()`             | boolean     | 是否可讀取                                |
| `canWrite()`            | boolean     | 是否可修改                                |
| `exists()`              | boolean     | 是否存在                                  |
| `isDirectory()`         | boolean     | 是否為目錄                                |
| `isFile()`              | boolean     | 是否為檔案                                |
| `lastModified()`        | long        | 取得最後一次修改時間                      |
| `length()`              | long        | 取得檔案大小                              |
| `delete()`              | boolean     | 刪除檔案或目錄並回傳成功與否              |
| `list()`                | String[]    | 列舉目錄下的檔案與子目錄                  |
| `listFiles()`           | File[]      | 列舉目錄下的檔案與子目錄的 File 物件      |
| `mkdir()`               | boolean     | 建立目錄                                  |
| `mkdirs()`              | boolean     | 建立目錄 + 不存在但是需要一併建立的父目錄 |
| `renameTo(File)`        | boolean     | 重新命名                                  |
| `createTempFIle()`      | File        | 建立暫存檔案，有多載版本                  |
| `setLastModified(long)` | boolean     | 設定最後一次修改時間                      |
| `setReadOnly()`         | boolean     | 設定成唯讀                                |



## 由主控台讀寫資料

### 主控台的 I/O

`java.lang.System` 類別裡的三個 static 欄位

| 「欄位       | 欄位型別      | 功能                                                         |
| ------------ | ------------- | ------------------------------------------------------------ |
| `System.out` | `PrintStream` | 將訊息輸出至 console，又稱 **標準輸出**（standard output）<br />可接受由主控台再經`>` 或 `>>` 的「重新導向指令」，將輸出內容導向至另外一個檔案 |
| `System.in`  | `InputStream` | 由 console 接收來自鍵盤或其他來源的訊息輸入<br />又稱 **標準輸入**（standard input） |
| `System.err` | `PrintStream` | 和 `System.out` 一樣都是輸出訊息至 console<br />但主要用於輸出錯誤訊息，較為急迫必須立即顯示，因此「重新導向指令」無效，依然顯示在 console <br />使用 Eclipse IDE 的話，輸出顏色為紅色（警示用） |



### 使用標準輸出方法

+ `println()` 輸出換行符、`print()`不輸出換行符號
+ 以上兩方法對大部分基本型別以及參考型別都有多載方法支援
  + boolean, char, int, long, float, double、char[], `Object`, `String`
  + 其它參考型別則呼叫該物件的 `toString()` 方法

### `java.io.Console` 類別介紹

+ 除了使用前述 `System.in` 取得主控台標準輸入外，也可以用 `java.io.Console` 物件

  ```JAVA
  public class ConsoleInput {
      public static void main(String[] args) {
          Console cons = System.console(); 
          // 使用 System.console() 方法取得 Console 物件
          boolean userValid = false;
          if (cons != null) {
              String account;
              String passwd;
              do {
                  account = cons.readLine("%s", "Account: ");
                  // readline() 可取得指令列輸入的資料
                  passwd = new String(cons.readPassword("%s", "Password: "));
                  // readPassword() 除了可以取得指令列輸入的資料，
                  // 還能隱藏使用者輸入內容
                  if (account.equals("joanna") && passwd.equals("password")) {
                      System.out.println("Correct! System quits!");
                      userValid = true;
                  } else {
                      System.out.println("Wrong! Try again!");
                  }
              } while (!userValid);
              // 後測式迴圈：先輸入再驗證，失敗繼續，成功終止
          }
      }
  }
  ```

  ![image-20221218141013717](https://i.imgur.com/Q8HsrVi.png)

## Channel I/O

+ Channel : 可以指兩個設備之間傳送資訊經過的通路或連接
  + 導入於 JDK 1.4，屬於 `java.nio` package
  + 可以一次大量讀入位元和字元，不需要以迴圈每次讀取少量內容
  + 程式更簡潔，程式效能更好

```java
import java.io.*;
import java.nio.ByteBuffer;
import java.nio.channels.FileChannel;

public class CopyByteChannel {
	public static void main(String[] args) {
		String source = "", target = "";
		try (FileChannel in = new FileInputStream(source).getChannel();
				FileChannel out = new FileOutputStream(target).getChannel()) {
			ByteBuffer buff = ByteBuffer.allocate((int) in.size());
             // 建立和 source 檔案 size 大小相同的 ByteBuffer 物件
			in.read(buff);
             // 將實體檔案一次全數讀入到 ByteBuffer 物件中
			buff.position(0);
             // 將 ByteBuffer 裡的標示位置移到最前面
			out.write(buff);
       /* 將 ByteBuffer 裡的資料全數輸出至 FileChannel out，再由其輸出為檔案
          因為 FileChannel 的 read() 和 write() 方法都是透過 ByteBuffer 物件一次搞定
          所以不用迴圈（loop）分次處理 */
		} catch (FileNotFoundException f) {
			f.printStackTrace();
		} catch (IOException i) {
			i.printStackTrace();
		}
	}
}
```



## 使用序列化技術讀寫物件

### Java 裡的資料保存 *Persistence*

將資料儲存於永久性的儲存硬體中，稱為「**persistence**」

1. 支援 persistence 的 Java 物件可儲存於本機硬體，或經由網路到另一個硬體裝置
   + 未支援 persistence 的 Java 物件只能存活於執行中的 JVM
2. 序列化 *serialization*：Java 將記憶體中的物件狀態儲存於硬體，成為「**實體檔案**」的標準機制
   + 未來可用來建構物件的副本或是還原物件狀態
3.  支援序列化的物件必須實作 `java.io.Serializable` 介面
   + 該介面是一個「**maker interface**」，沒有任何需要實作的方法
   + 類別實作此介面只是讓 Java 知道其物件有具備序列化的能力，具體序列化步驟由 Java 掌控

### 序列化和物件圖譜

+ 當物件被序列化時，只有「**欄位值**」會被保留
  + 欄位值包含資料，也包含物件的狀態
  + 若物件的該欄位是參考型別，且被參考的物件也支援序列化，則該欄位物件也會一併被序列化
+ **物件圖譜 object graphs**：
  + 物件欄位參照其它物件，被參照的物件又可以再參照更多物件...所形成的樹狀結構

### 不需要參加序列化的欄位

+ 物件序列化時，預設所有欄位都會一併進行序列化

+ 如果欄位所屬類別沒有實作 `java.io.Serializable`，會中斷執行並丟出例外 `NotSerializableException`

+ 若物件欄位只是記錄當下系統狀態的某些資訊（ex. 目前時間），屬於「**短暫 *transient***」資訊

  + 不需要再序列化過程中被保留
  + 重建副本時也不需要回復
  + 此類欄位不需要參與序列化流程，可以加上「**transient**」宣告

+ 宣告「**static**」的欄位和物件狀態無關，其值在物件序列化過程中也不會被保留

  ```java
  public class Order implements Serializable {
      private Set<Shirt> shirts = new HashSet<>();
      static int staticField = 100;        // static
      transient int transientField = 100;  // transient
  }
  ```

  + 宣告 `transient` 以及 `static` 的欄位在「反序列化 / 還原」程序時
    1. static : 會得到類別內原本定義的宣告值
    2. transient : 會得到該型態的預設值

### 定義物件保存的版本號碼

+ **序列化**：保留物件「`當下狀態（欄位值）`」，並未保留類別架構

+ **反序列化**：將「`過去物件某個狀態`」，搭配「`目前類別架構`」進行還原

  + 若還原時發現「**過去物件狀態**」和「**目前類別架構**」不一致，可能產生不預期的問題或錯誤

    + 過去某欄位現在已經不存在
    + 類別後來新增必要欄位，序列化卻沒有該欄位

  + 為了能夠使序列化後的檔案可以順利的被反序列化還原成物件：

    1. 在可序列化之類別定義一個欄位 `serialVersionUID` 作為版本控管號碼

       + 每次增減類別內欄位時，都應該同步修改版本號碼並且記錄

       + 版本控管號碼必須宣告為 **static** 且 **long**

         `private static final long serialVersionUID = 1L;`

    2. `InvalidClassException` 預先保護機制

       + 假設目前版本號為 1，序列化後得到的檔案內存版本也會是 1
       + 後來類別增減了欄位，版本修改為 2
       + 如果以「版本#2的最新類別定義」來還原「序列化時版本#1的檔案」會拋出上述錯誤

    3. 若類別實作 `Serializable` 介面，但未宣告`serialVersionUID`

       + Java 預設將主動宣告並提供欄位值，該值將考慮開發環境因素（IDE工具或Java版本）計算出一個複雜的長整數

         `private static final long serialVersionUID =3696676879791539369L;`

       + 此長整數在每次編譯階段，即使只是某方法內字串微調，也可能因為環境再改變而自動改變版本號碼

       + 可能導致先前序列化的檔案因為版本號改變，而無法還原回物件

       + 建議開發者應該自己宣告 `serialVersionUID` 欄位

...

### 序列化和反序列化範例

以下範例將物件序列化產出檔案，再將檔案還原回物件，特殊情境：

+ class Order 底下含數個 class Shirt(s)，其中 Shirt(s) 類別內的價錢欄位會因時因地改變，序列化過程中不需要特別保存，**以 transient 宣告**
+ Java 給物件可以控制自身序列化和反序列化的流程
  + 序列化時可以額外寫入 `java.util.Date`物件，在反序列化的時候讀出，可知進行序列化時間點
+ 反序列化時，希望 Shirt(s) 類別內的價錢欄位可以參考成本價，再加上 50 元管銷費用

#### 1. class Shirt

```java
public class Shirt implements Serializable {

	private static final long serialVersionUID = 1L;
	private String brand;
	private int quantity;
	private double cost;
	private transient double price;

	public Shirt(String brand, int quantity, double cost) {
		this.brand = brand;
		this.quantity = quantity;
		this.cost = cost;
		this.price = 2 * cost;
	}

	// This method is called post-serialization
    // 要修改Java反序列化(將物件自檔案中讀出)的流程，必須定義本方法
	private void readObject(ObjectInputStream ois) throws IOException, ClassNotFoundException {
        // ois.defaultReadObject() 是物件原本的反序列化流程，仍須呼叫此
		ois.defaultReadObject();
		// perform other initiliazation 將檔案還原成物件後，price欄位+50元
		this.price = this.cost + 50;
	}

	@Override
	public String toString() {
		return "Shirt: " + this.brand + "\n" 
				+ "Quantity: " + this.quantity + "\n" 
				+ "Cost: " + this.cost + "\n"
				+ "Price: " + this.price + "\n"
				+ "------------------\n";
	}
}
```

#### 2. class Order

```java
public class Order implements Serializable {

	private static final long serialVersionUID = 1L;
	private List<Shirt> shirts = new ArrayList<>();
	static int staticField = 100;
	transient int transientField = 100;

	public Order(Shirt... shirts) {
		for (Shirt s : shirts) {
			this.shirts.add(s);
		}
		staticField = 99;
		transientField = 99;
		System.out.println("--- Constructor is launched ---");
	}

	private void writeObject(ObjectOutputStream oos) throws IOException {
        // 使用此方法修改Java序列化(物件寫入檔案)的流程
		oos.defaultWriteObject();
		// keep the serialization date
		Date now = new Date();
		oos.writeObject(now); // 將日期物件寫入檔案
		System.out.println("\nSerialized at: " + now + "\n");
	}
	
	// This method is called post-serialization (修改Java反序列化(檔案還原成物件)的流程)
	private void readObject(ObjectInputStream ois) throws IOException, ClassNotFoundException {
		ois.defaultReadObject(); // 進行物件原本的反序列化流程
		System.out.println("\nRestored from date: " + (Date) ois.readObject());
        // 讀出序列化時的日期物件，再轉型回Date型態
		System.out.println("Restored at: " + new Date() + "\n");
	}

	public String toString() {
		StringBuilder sb = new StringBuilder("Order Summary ===\n");
		for (Shirt s : shirts) {
			sb.append(s);
		}
		sb.append("staticField = " + staticField);
		sb.append("\ntransientField = " + transientField);
		sb.append("\n------------------");
		return sb.toString();
	}
}
```



### 3. Serialization and De-serialization

  ```java
  public class SerializeOrder {
  
  	public static void main(String[] args) {
  
  		String output = System.getProperty("user.dir") + "\\src\\course\\c03\\ser\\file\\Order.ser";
  
  		serialization(output);
  
  	//	System.out.println("\n-----------------------------------------\n");
  		
  	//	deSerialization(output);
  	}
  
  	private static void serialization(String output) {
  		// Create a shirts Order
  		Shirt s1 = new Shirt("Brand1", 100, 100);
  		Shirt s2 = new Shirt("Brand2", 100, 200);
  		Shirt s3 = new Shirt("Brand3", 100, 300);
  		Order o = new Order(s1, s2, s3);
  		
  		Order.staticField = 22;
  		// Write out the Order
  		try (FileOutputStream fos = new FileOutputStream(output);
  				ObjectOutputStream out = new ObjectOutputStream(fos)) {
  			out.writeObject(o);
  		} catch (IOException i) {
  			i.printStackTrace();
  		}
  		System.out.println("=== Before Serialization, " + o);
  	}
  
  	private static void deSerialization(String output) {
  		// Read the Order back in
  		try (FileInputStream fis = new FileInputStream(output); ObjectInputStream in = new ObjectInputStream(fis)) {
  			Order restoredOrder = (Order) in.readObject();
  			System.out.println("=== After Serialization, " + restoredOrder);
  		} catch (ClassNotFoundException | IOException i) {
  			i.printStackTrace();
  		}
  	}
  }
  ```

> 1. **Shirt** 物件生成時的 price 都是 cost 的 2 倍，還原時在 `readObject()` 方法被改成 `price = cost + 50`
> 2. **Order**物件序列化時，在`writeObject()`被特別寫入的日期，還原時在`readObject()`方法可一併輸出
> 3. **static 欄位** 會得到類別內原本定義的宣告值
> 4. **transient 欄位** 會得到該型態的預設值

---

# 04 `NIO.2`

## `NIO.2` 基礎

### `java.jo.File`限制

基礎 I/O 存在一些不方便的地方：

1. 很多方法遇到錯誤時回傳 *false*，而非丟出例外
2. 缺少很多存取檔案常用功能，ex. 複製 *copy*、移動 *move*
3. 不是每一個作業系統都支援重新命名
4. 不支援 *symbolic link* 類型的檔案
5. 對於「`metadata`（描述檔案的資料）」檔案的取得很有限
   + 例如：檔案權限、檔案擁有者、安全性設定
6. 存取「`metadata`（描述檔案的資料）」檔案沒有效率
   + 一次只能存取一個，每次呼叫都會轉呼叫系統指令 `system call`
7. 很多方法遇到檔案較大時，會呈現卡住狀態（*hang*），久無回應甚至當掉
8. 遞迴目錄結構時，遇到 *symbolic link* 類型的檔案無法適當處理
9. 遇到新型態作業系統或新檔案型態時，不易擴充 `API`

### Java I/O 套件發展歷史

| 功能                | `JSR` | 版本   | 套件（Package）   | note            |
| ------------------- | ----- | ------ | ----------------- | --------------- |
| I/O                 |       | Java 2 | `java.io.*`       |                 |
| New I/O (`NIO`)     | 51    | Java 4 | `java.nio.*`      | ex. Channel I/O |
| New I/O 2 (`NIO.2`) | 203   | Java 7 | `java.nio.file.*` |                 |



### 檔案系統、路徑和檔案

`NIO.2` 裡面的檔案 / 目錄都以路徑（path）來表達，可分為絕對路徑 *absolute path* 和相對路徑 *relative path*

1. **絕對路徑**
   + 包含根目錄，ex. 「`/`」或 Windows 的「`C:`」
   + 定位檔案位置必須
2. **相對路徑**
   + 必須再結合絕對路徑才能找到檔案真正位置

### Symbolic Link 檔案

+ 又稱為 `symlink` 或者 `soft link`，並不是捷徑（*short cut*）

+ 執行時以系統管理員身分開啟 cmd，輸入指令 

  ```TERMINAL
  mklink $(連結檔案) $(連結來源檔案)
  ```

  | comparison     | `symlink`                                      | `short cut`     |
  | -------------- | ---------------------------------------------- | --------------- |
  | 檔案類型不同   | .symlink<br />`.sl`                            | 捷徑<br />`.sc` |
  | 檔案大小不同   | 0 KB                                           | 2 KB            |
  | 分別複製兩者時 | 複製 **連結來源檔案**，<br />非 `symlink` 本身 | 複製捷徑本身    |

  

### `NIO.2` 的基本架構

+ Before JDK 7
  + `java.io.File` 是所有檔案 / 目錄的操作基礎
+ After JDK 7 (推出 `NIO.2`)
  + 改為三個基礎
    1. `java.nio.file.Path` : 用來**找出**檔案 / 目錄
    2. `java.nio.file.Files` : 用來**操作**檔案 / 目錄
    3. `java.nio.file.FileSystem` : 用來**建立** Path 或其它存取檔案系統的物件
  + `NIO.2` 所有方法都丟出 `IOException` 或其子類別

## 使用 Path 介面操作檔案/目錄

### Path 介面

+ `java.nio.Path` 介面是 `NIO.2` 架構的進入點

+ 取得 `Path` 物件有兩種方法，物件建立後不能修改狀態 (即不可變物件 immutable)

  1. 藉由 `FileSystem` 物件的 `getPath()` 方法

     ```java
     FileSystem fs = FileSystems.getDefault();
     Path p1 = fs.getPath("D:\\labs\\resources\\myFile.txt");
     ```

  2. 藉由 `java.nio.file.Paths` 類別的靜態 `get()` 方法

     ```java
     Path p0 = Paths.get("C:\\fakeFile\\pseudoDir\\temp.txt");
     Path p1 = Paths.get("C:/fakeFile/pseudoDir/temp.txt");
     Path p2 = Paths.get("D:", "fakeFile", "pseudoDir", "temp.txt");
     Path p3 = Paths.get("/temp/ferrero");
     Path p4 = Paths.get(URI.create("file:///~/someTempFile"));
     ```

     > Windows 檔案路徑接受以下兩種方向的斜線
     >
     > + `\\` ( `\` + escape character `\`) 
     >
     > + `/` 

### Path 介面主要功能

+ Path 介面用來找出檔案 / 目錄，常用方法

  | **分解路徑**                                                 | **操作路徑**                                                 | **比較路徑**                                     |
  | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------ |
  | 取得構成路徑的所有檔案 / 目錄。<br />主要分成根目錄 *root* 和名稱 *name* 兩種路徑成員。<br />root 路徑成員只有一個，name 路徑成員可以有多個。 |                                                              |                                                  |
  | `getFileName()`<br />`getParent()`<br />`getRoot()`<br />`getNameCount()` - 不含 root | `normalize()`<br />`toUri()`<br />`toAbsolutePath()`<br />`subpath()`<br />`resolve()`<br />`relativize()` | `startsWith()`<br />`endsWith()`<br />`equals()` |

  > **getNameCount()** 只計算 name 路徑成員個數，不包含 root，且索引從 0 ~ 2 
  >
  > ```java
  > Path p1 = Paths.get("D:/Temp/Foo/file1.txt");
  > System.out.format("getNameCount: %d%n", p1.getNameCount());
  > // getNameCount: 3
  > ```
  >
  > | **路徑組成** | `D:` | `Temp`      | `Foo`       | `file1.txt` |
  > | ------------ | ---- | ----------- | ----------- | ----------- |
  > | **成員分類** | root | name<br />0 | name<br />1 | name<br />2 |

  

### 移除路徑裡的多餘組成

+ 檔案系統目錄
  1. 「`.`」 當前目錄
  2. 「`..`」 上一層目錄
  
+ 多餘的組成 可以**使用 normalize() 方法** 移除多餘部分，像是「`./`」和「`directory/../`」
  + /home/**./** pseudo/himmel
  + /home/**acrawlingkitten/../** pseudo/himmel
  
  

### 建立子路徑 - `subpath()`

使用 `subpath()` 可取得路徑裡的部分路徑

```java
private static void testSubPath() {
    Path p1 = Paths.get("D:/Temp/finnish/schocolade");
    p1.subpath(1, 3);  // immutable test
    System.out.println(p1);  // D:\Temp\finnish\schocolade
    p1 = p1.subpath(1, 3);   // finnish\bar
    System.out.println(p1);
}
```

> `subpath(1, 3)` 表示由 index=1 開始取，不含 index=3 的成員，即取出成員 1 和 2

| D:   | Temp | finnish | schocolade |
| ---- | ---- | ------- | ---------- |
| root | 0    | 1       | 2          |



### 結合 2 個路徑 - `resolve()`

使用 `resolve()` 結合兩個路徑

1. 傳入「**相對路徑**」- 將該「**相對路徑**」，連接在「**原路徑**」之後

2. 傳入「**絕對路徑**」- 方法回傳該「**絕對路徑**」，忽略「**原路徑**」

   ```java
   private static void testResolve() {
       String p = "/home/clementine/fredrich";
       Path p1 = Paths.get(p).resolve("bones");
       System.out.println(p1);  // \home\clementine\fredrich\bones
       Path p2 = Paths.get(p).resolve("/home/clementine");
       System.out.println(p2);  // \home\clementine
   }
   ```

### 建立連接 2 個路徑的路徑 - `relativize()`

使用 `relativize()` 建構兩個路徑間的路徑，由原路徑到 `relativize()` 方法所傳入的路徑範例如下

```java
private static void testRelative() {
    Path p1 = Paths.get("peon");
    Path p2 = Paths.get("julia");
    Path p1Top2 = p1.relative(p2);  // 由p1到p2的走法  ..\julia
    System.out.println(p1Top2); 
    Path p2Top1 = p2.relative(p1);  // 由p2到p1的走法  ..\peon
    System.out.println(p2Top1);
}
```

### Hard Link

`hard link`類型的檔案，相對於 `soft link` 或 `symbolic link` 有更多限制：

1. 目標檔案一定要存在
2. 目標不可以是目錄，只能是檔案
3. 目標不可以跨磁碟，例如不能在 C磁碟建立 D磁碟的檔案 hard links
4. 行為、外觀、屬性和一般檔案相似，不容易判斷

### 處理 Symbolic Link

`NIO.2`類別可以感知 *link* 類型檔案的存在，稱為「*link aware*」。相關方法具備以下能力

1. 偵測是否遇到 symbolic link 檔案

2. 設定遇到 symbolic link 檔案時的處理方式

   ```java
   Files.createSymbolicLink(Path p1, Path p2, FileAttribute<?>);
   Files.createLink(Path p1, Path p2);  // 建立 hard link
   Files.isSymbolicLink(Path p1);
   Files.readSymbolicLink(Path p1);     // 找出 symbolic link 的 target
   ```

   ...

## 使用 Files 類別對檔案/目錄進行檢查、刪除、複製、移動

### 處理檔案

先使用 Path 物件定位檔案 / 目錄。再用 Files 類別操作 Path 物件，以達成：

1. 檔案與目錄的：
   + 檢查 check | 刪除 delete | 複製 copy | 移動 move
2. 管理屬性資料（`metadata`）
3. 讀 / 寫和建立檔案
4. 隨機存取檔案
5. 讀取目錄（`directory`）內的檔案

### 檢查檔案 / 目錄是否存在

+ Path 代表檔案 / 目錄位置

+ 存取之前應該先使用 Files 類別檢查是否存在（*symbolic link* 也算檔案），方法如下

  ```java
  Files.exists(Path p, LinkOption... option);
  Files.notExists(Path p, LinkOption... option);
  ```

+ 如果兩個方法測試結果都是 false，表示狀態無法確認（`unknown`），常見原因

  1. 沒有權限
  2. 離線磁碟機（Off-line Drive），例如 CD-ROM

### 檢查檔案 / 目錄屬性

檢查權限的使用方法：

```java
Files.isReadable(Path p);
Files.isWritable(Path p);
Files.isExecutable(Path p);
```

檢查是否為同一檔案的方法（常用於 symbolic link）

`isSameFile()`檢查一旦結束，就不再保證結果，因為檔案可能馬上被其它系統指令更改

```java
Files.isSameFile(Path p1, Path p2);
```

...

### 建立檔案 / 目錄

+ 建立檔案的方法 - `createFile()`

  ```java
  Files.createFile(Path file);
  ```

+ 建立單一目錄的方法 - `createDirectory()`

  ```java
  Files.createDirectory(Path dir);
  ```

+ 建立多重目錄的方法，通常用於將路徑裡缺少的 name 成員一次全部建立

  ```java
  Files.createDirectories(Path dir);
  ```

+ 假設只有 `D:/Temp` 目錄存在，則使用 `Path.get(...)` 可將缺少的目錄一次建立完成

  ```java
  Files.createDirectories(Paths.get("D:/Temp/foo/bar/example"));
  ```

### 刪除檔案 / 目錄

+ 刪除檔案 / 目錄使用的方法 `Files.delete(Path p);`

+ 失敗時可能丟出以下例外

  1. `java.nio.NoSuchFileException` : 要刪除的檔案不存在
  2. `java.nio.file.DirectoryNotEmptyException` : 要刪除的目錄不為空
  3. `java.io.IOException` : 其它錯誤

+ 也可以刪除檔案 / 目錄前先確認是否存在 - `deleteIfExists()`

  ```java
  Files.deleteIfExists(Path p);
  ```

+ 則檔案不存在就不會刪除，因此不會有 `NoSuchFileException`

### 複製和移動檔案 / 目錄

+ 複製和移動檔案 / 目錄的方法：

  ```java
  Files.copy(Path source, Path target, CopyOption...); 
        // source 來源路徑 (目錄 or 檔案)
  Files.move(Path source, Path target, CopyOption...);
        // target 目標路徑 (目錄 or 檔案)
  ```

+ `CopyOption` 介面，此傳入參數允許同時多個，有兩個列舉型別實作它

  | 介面         | 列舉型別（`enum`） | 列舉項目（`types`） |
  | ------------ | ------------------ | ------------------- |
  | `CopyOption` | `LinkOption`       | `NOFOLLOW_LINKS`    |
  | -            | `StandCopyOption`  | `REPLACE_EXISTING`  |
  | -            | `StandCopyOption`  | `COPY_ATTRIBUTES`   |
  | -            | `StandCopyOption`  | `ATOMIC_MOVE`       |

  

+ 比較 **複製** 與 **移動** 兩種操作的相同之處

  1. 如果目標路徑已經存在，但操作前沒有使用 `StandardCopyOption.REPLACE_EXISTING` 指定可以覆蓋的話，將失敗
  2. 如果目標路徑不存在，則操作後將自動建立
  3. 在操作**之前**來源和目標「非一致」是檔案 / 目錄，將不影響結果

+ 定義 **目標路徑** 需要注意

  1. 如果目標路徑是存在的「檔案」或「空目錄」，使用 **REPLACE_EXISTING** 或 **ATOMIC_MOVE**，可避免拋出 `java.nio.file.FileAlreadyExistsException`
  1. 若目標路徑是存在的「非空目錄」，用 **REPLACE_EXISTING** 還不夠，還是會拋出 `java.nio.file.DirectoryNotEmptyException`
  
+ 定義 **來源路徑** 需要注意

  1. 必須為存在的「檔案」「目錄」，否則拋出 `java.nio.file.NoSuchFileException`
  2. 來源路徑是「目錄」時
     + 即便「複製」成功，也無法複製內含檔案，只會產生新目錄，過程不會出錯
  3. 來源路徑是「目錄」時
     + 如果「移動」成功，內含的檔案 / 目錄將一併搬家

+ 複製或移動路徑時，可以在第三個參數開始傳入實作介面 `CopyOption` 的列舉型態，注意事項為

  1. 「複製」檔案 / 目錄時的注意事項
     + 來源路徑是 `symbolic link` 時，預設將複製「link 指向的檔案」
     + 列舉型態「`StandardCopyOption.COPY_ATTRIBUTES`」用於將檔案屬性一併複製。大部分屬性將依檔案系統不同而可能不被複製，但檔案最後修改時間（last-modified）將被支援
  2. 「移動」檔案 / 目錄時的注意事項
     + 使用列舉型態 `StandardCopyOption.ATOMIC_MOVE`
       + 若檔案系統不支援將丟出例外
       + 若支援則可避免移動過程中有其它系統程序存取檔案
       + 如果用於耗時較久的大檔案移動，可以保證接下來要存取該檔案的系統程序都可存取到完整的檔案
     + 若移動 symbolic link 檔案，不需要使用列舉型態 `LinkOption.NOFOLLOW_LINKS`

### Stream 和 Path 互相複製

+ 檔案複製來源或目標除了 Path 之外，也可以是基礎 I/O 提到的串流（Stream）物件

  ```java
  Files.copy(InputStream source, Path target, CopyOption... options);
  Files.copy(Path source, OutputStream target);
    // source: 檔案複製來源，使用 InputStream
    // target: 檔案複製後的輸出，使用 OutputStream
  ```

+ 以下示範如何將遠端網頁轉換成 `InputStream` 物件後，再複製為本機檔案

  ```java
  public class CopyInputStreamTest {
      public static void main(String[] args) throws IOException {
          Path to = Paths.get("dir/c04/oracle.html/").toAboslutePath();
          URL url = URI.create("http://www.oracle.com/").toURL();
          try ( InputStream from = url.openStream()) {
              Files.copy(from, to, StadardCopyOption.REPLACE_EXISTING);
              System.out.println("")
          }
      }
  }
  ```

### 列出目錄內容

+ `DirectoryStream` 介面可以找出目錄下所有檔案 / 目錄，但只限制在目錄下第一層

  ```java
  public static void main(String[] args) {
      Path dir = Paths.get("D:/");
      try (DirectoryStream<Path> stream = Files.newDirectoryStream(dir, "*")) {
          for(Path file : stream) {
              System.out.println(file.getFileName());
          }
      } catch (PatternSyntaxException | DirectoryIteratorException | IOException x) {
          System.err.println(x);
      }
  }
  ```

### 讀取和寫入檔案

+ Files 類別

  + `readAllBytes()` 和 `readAllLines()` 可以一次讀取檔案全部內容（但檔案不建議太大）

  + `write()` 則提供寫入檔案的功能

    ```java
    public static void main(String[] args) throws IOException {
        Path source = Paths.get("dir/c04/file.txt").toAbsolutePath();
        Charset cs = Charset.defaultCharset();
        List<String> lines = Files.readAllLines(source, cs);
        Path target = Paths.get("dir/c04/file2.txt").toAbsolutePath();
        Files.write(target, lines, cs,
                   StandardOpenOption.CREATE,
                   StandardOpenOption.TRUNCATE_EXISTING,
                   StandardOpenOption.WRITE);
        System.out.println("done...");
    }
    ```

    

---

## 使用 Files 類別操作 Channel I/O 和 Stream I/O 讀寫檔案

### 介面 Channel 和類別 `ByteBuffer`

+ 搭配使用 `Channel` interface 和 `ByteBuffer` class，可提高 I/O 效率
  1. **Stream I/O** 每次讀取一個位元或字元；**Channel I/O** 每次讀取一塊記憶體（buffer）
  2. `java.nio.channels.ByteChannel` interface 繼承 `Channel` interface，提供基本讀寫功能
  3. `java.nio.channels.SeekableByteChannel` interface 繼承 `ByteChannel`
     + 提供在 channel 中讀寫時紀錄目前位置，且改變讀寫位置的能力，讓 **隨機存取**（**random access**）變得可能
  4. 使用 `Files.newByteChannel(Path, OpenOption...)` 方法回傳 `SeekableByteChannel` 實例後，也可以再轉型為 `java.nio.channels.FileChannel` 類別

### 隨機存取檔案

+ `SeekableByteChannel`介面可進行檔案內容的「隨機存取」

  1. <sup>st</sup> step : 打開檔案
  2. <sup>nd</sup> step : 找到存取位置
  3. <sup>rd</sup> step : 開始讀寫

+ 常用方法（以下 channel 代表檔案）

  | 常用方法              | 敘述                             |
  | --------------------- | -------------------------------- |
  | **position()**        | 回傳在 `channel` 中的位置        |
  | **position(long)**    | 設定在 `channel` 中的位置        |
  | **read(ByteBuffer)**  | 由 `channel` 中將資料讀入 buffer |
  | **write(ByteBuffer)** | 將資料由 buffer 中寫入 `channel` |

  ```java
  // 使用 SeekableByteChannel 介面，將新增字串放在檔案指定位置
  	public static void main(String[] args) throws IOException {
  		Path path = Paths.get("dir/c04/file.txt").toAbsolutePath();
  		try (SeekableByteChannel sbc = Files.newByteChannel(path, StandardOpenOption.WRITE)) {
               /* 由 Files.newByteChannel() 取得 SeekableByteChannel 介面的實作物件
                  該物件指向 path 所在檔案，並指定屬性為 StandardOpenOption.WRITE 所以可寫入 */
  			long channelSize = sbc.size();
  			sbc.position(channelSize);     // 將檔案讀取位置移到最尾端
  			System.out.println("position: " + sbc.position());
  			ByteBuffer buffer = ByteBuffer.wrap(("\n" + "0").getBytes()); 
               // 建立 ByteBuffer 物件，內容: "\n" + "0"
  			sbc.write(buffer);
  			System.out.println("position: " + sbc.position());
  		}
  	}
  ```

  ...

### 對文字檔提供 Buffered I/O 方法

+ `NIO.2` 一樣可用 `BufferedReader / BufferedWriter` 物件提高檔案讀寫效率

  1. 使用 `Files.newBufferedReader()` 取得 `java.io.BufferedReader` 物件

     ```java
     BufferedReader reader = Files.newBufferedReader(path, charset);
     line = reader.readLine();
     ```

  2. 使用 `Files.newBufferedWriter()` 取得 `java.io.BufferedWriter` 物件

     ```java
     BufferedWriter writer = Files.newBufferedWriter(path, charset);
     writer.write(s, 0, s.length());
     ```

### 取得位元串流物件的方法

+ `NIO.2` 也可以取得 `InputStream` 以及 `OutputStream` 物件

  1. 使用 `Files.newInputStream()` 取得 `java.io.InputStream` 物件

     ```java
     InputStream in = Files.newInputStream(path);
     BufferedReader r = new BufferedReader(new InputStreamReader(in));
     String line = r.readLine();
     ```

  2. 使用 `Files.newOutputStream()` 取得 `java.io.OutputStream` 物件

     ```java
     Path path = Paths.get("dir/c04/logFile.txt").toAbsolutePath();
     String s = "Hi, Jim...";
     byte data[] = s.getBytes();
     try (OutputStream out = Files.newOutputStream(path, CREATE, APPEND);
         BufferedOutputStream bot = new BufferedOutputStream(out);) {
         out.write(data, 0, data.length);
     }
     ```

---

## 讀寫檔案/目錄的屬性

### 使用 Files 管理檔案的屬性資料

`Files` class 提供若干管理檔案 / 目錄屬性的方法

| Method                | Explanation              |
| --------------------- | ------------------------ |
| `size`                | 回傳檔案大小（bytes）    |
| `isDirectory`         | 判斷是否為目錄           |
| `isRegularFile`       | 判斷是否為檔案           |
| `isSymbolicLink`      | 判斷是否為 symbolic link |
| `isHidden`            | 是否為隱藏檔             |
| `getLastModifiedTime` | 取得最後修改時間         |
| `setLastModifiedTime` | 設定最後修改時間         |
| `getAttribute`        | 取得屬性                 |
| `setAttribute`        | 設定屬性                 |

```java
public static void main(String[] args) throws IOException {
   Path basic = Paths.get("dir/c04/metadata").toAbsolutePath();
    // 在相對路徑下，事先建立所有測試檔案
   out.println("basic path: " + basic);
   
   Path common = basic.resolve("file.txt");
   Path shortcut = basic.resolve("dir.shortcut");
   Path hidden = basic.resolve("hiddenFile");
   Path symlink = basic.resolve("dir.sl");
   
   out.println("size: " + Files.size(common));
   out.println("isDirectory: " + Files.isDirectory(common));
   out.println("isRegularFile: " + Files.isRegularFile(common));     
   out.println("isSymbolicLink(dir.sl): " + Files.isSymbolicLink(symlink));
   out.println("isSymbolicLink(dir.shortcut): " + Files.isSymbolicLink(shortcut));
   out.println("isHidden: " + Files.isHidden(hidden));    
   
   // 取得並修改LastModifiedTime
   out.println("getLastModifiedTime: " + Files.getLastModifiedTime(common));
   FileTime t = FileTime.fromMillis(new Date().getTime());
    // 要設定檔案的時間屬性，必須使用類別 FileTime
   Files.setLastModifiedTime(common, t);
   out.println("getLastModifiedTime: " + Files.getLastModifiedTime(common));
   // 使用Files.setAttribute()方法設定hidden屬性	
    public static void main(String[] args) throws IOException {
		Path basic = Paths.get("dir/c04/metadata").toAbsolutePath();
		out.println("basic path: " + basic);
		
		Path common = basic.resolve("file.txt");
		Path shortcut = basic.resolve("dir.shortcut");
		Path hidden = basic.resolve("hiddenFile");
		Path symlink = basic.resolve("dir.sl");
		
		out.println("size: " + Files.size(common));
		out.println("isDirectory: " + Files.isDirectory(common));
		out.println("isRegularFile: " + Files.isRegularFile(common));		
		out.println("isSymbolicLink(dir.sl): " + Files.isSymbolicLink(symlink));
		out.println("isSymbolicLink(dir.shortcut): " + Files.isSymbolicLink(shortcut));
		out.println("isHidden: " + Files.isHidden(hidden));	
		
		//取得並修改LastModifiedTime
		out.println("getLastModifiedTime: " + Files.getLastModifiedTime(common));
		FileTime t = FileTime.fromMillis(new Date().getTime());
		Files.setLastModifiedTime(common, t);
		out.println("getLastModifiedTime: " + Files.getLastModifiedTime(common));
		//設定hidden屬性
		Files.setAttribute (hidden, "dos:hidden", Boolean.valueOf(false));
		out.println("isHidden: " + Files.getAttribute(hidden, "dos:hidden"));
	}
   Files.setAttribute (hidden, "dos:hidden", Boolean.valueOf(false));
   out.println("isHidden: " + Files.getAttribute(hidden, "dos:hidden"));
   // 除了用isHidden()方法測試屬性是否為hidden，也可以用getAttribute()方法搭配字串"dos:hidden"
}
```

### 讀取檔案屬性

+ 過去 Java I/O 讀取檔案一次只能一個，每次呼叫都必須轉呼叫系統指令（system call）

+ `NIO.2` 改進以上問題，可用 `DosFileAttributes` interface 一次取回檔案 / 目錄的所有屬性

+ 以 Windows 之 DOS 為例，使用 Files class 取得物件實例

  ```java
  DosFileAttributes attrs = Files.readAttributes(path, DosFileAttributes.class);
  ```

+ 注意 Java 7 的 `DosFileAttributes` interface 只能讀取屬性，不能修改

  ```java
  public static void main(String[] args) throws IOException {
  	Path p = Paths.get("dir/c04/attributeTest.txt").toAbsolutePath();
  	DosFileAttributes attrs = Files.readAttributes(p, DosFileAttributes.class);
  
  	// basic
  	FileTime creation = attrs.creationTime();
  	FileTime modified = attrs.lastModifiedTime();
  	FileTime lastAccess = attrs.lastAccessTime();
  	if (!attrs.isDirectory()) {
  		long size = attrs.size();
  	}
  	boolean isDirectory = attrs.isDirectory();
  	boolean isRegularFile = attrs.isRegularFile();
  	boolean isSymbolicLink = attrs.isSymbolicLink();
  	boolean isOther = attrs.isOther();
  		
  	// only for DOS
  	boolean archive = attrs.isArchive();
  	boolean hidden = attrs.isHidden();
  	boolean readOnly = attrs.isReadOnly();
  	boolean systemFile = attrs.isSystem();
  }
  ```

  ![1](https://i.imgur.com/i4S6rSz.png)

### 修改檔案屬性

+ 建立檔案後可用 `Files` class 更改屬性

  ```java
  Files.createFile(path);
  Files.setAttribute(path, "dos:hidden", true);
  ```

+ `setAttribute()` 方法可設定 4 種 **DOS 屬性**，須指定屬性字串

  1. **dos:hidden**
  2. **dos:readonly**
  3. **dos:system**
  4. **dos:archive**

+ 介面 `DosFileAttributeView` 也提供設定屬性的相關方法

  1. `setHidden()`
  2. `setReadOnly()`
  3. `setSystem()`
  4. `setArchive()`

+ 可以使用 class Files 取得該介面的物件實例

  ```java
  DosFileAttributeView view = Files.getFileAttributeView(p, DosFileAttributeView.class);
  ```

+ `DosFileAttributeView` 有直接方法可以設定檔案屬性

  + 如果要讀取屬性，需先用 `readAttributes()` 方法取得 `DosFileAttributes`物件實例，分工：

    1. interface `DosFileAttributeView` : **改變** 檔案屬性

    2. interface `DosFileAttributes` : **讀取** 檔案屬性

       ```java
       Path p = Paths.get("dir/cel/attributeTest.md").toAbsolutePath();
       DosFileAttributeView view = Files.getFileAttributeView(p, DosFileAttributeView.class);
       // 取得 DosFileAttributeView 的方法
       
       view.setArchive(true);
       view.setReadOnly(true);
       view.setHidden(true);
       view.setSystem(true);
       
       FileTime lastModifiedTime = FileTime.fromMillis(new Date().getTime());
       FileTime lastAccessTime = FileTime.fromMillis(new Date().getTime());
       FileTime createTime = FileTime.fromMillis(new Date().getTime());
       view.setTimes(lastModifiedTime, lastAccessTime, createTime);    
       /* 設定檔案相關時間，Windows每個檔案屬性都有3個時間：建立日期(createTime)、
          最後修改日期(lastModifiedTime)、最後存取日期(lastAccessTime) */
       
       DosFileAttributes attrs = view.readAttributes();
       // 取得 DosFileAttributes 的方法
       ```

        

### DOS 之外的 File Attribute Views

除了 DOS 之外，`NIO.2` 其它可支援的 attribute views 還包含

1. `BasicFileAttributeView` : 提供所有檔案系統都支援的基本屬性

2. `PosixFileAttributeView` : 支援 `POSIX` 家族，例如 `UNIX`

3. `FileOwnerAttributeView` : 支援所有具備「**檔案擁有者**（file owner）」概念的檔案系統

4. `AclFileAttributeView` : 支援讀寫檔案的「**存取控制清單**（access control list）」

5. `UserDefinedFileAttributeView` : 讓使用者自行定義

   ![image-20221228143511740](https://i.imgur.com/IiluySs.png)

### `POSIX` 檔案系統的權限

+ `NIO.2` 可以在如 MacOS、Linux、Solaris 等 POSIX（Portable Operating System Interface）檔案系統中建立檔案 / 目錄。Windows 非 POSIX 相容的作業系統

  ```java
  // 如何取得目前作業系統支援的所有 AttributeView
  FileSystems.getDefault().supportedFileAttributeViews();
  ```

+ POSIX 檔案系統中，使用 ACL 進行檔案 / 目錄的權限控管

  + ACL（Access Control List 存取控制清單）: 提供對檔案 / 目錄擁有權相關的三種使用者群組
    1. owner（檔案擁有者）
    2. group（檔案擁有者所在群組）
    3. other（非 owner 和 group 的其它人）

+ 對檔案 / 目錄的 read、write、execute（讀寫執行）權限設定格式

  |          user           |        group         |        other         |
  | :---------------------: | :------------------: | :------------------: |
  | r    \|    w    \|    x | r    \|   w   \|   x | r   \|   w   \|    x |

  

---

## 遞迴存取目錄結構

### 對檔案目錄進行遞迴操作

+ `DirectoryStream` 物件可拜訪目錄下所有檔案 / 目錄，但被限制在以下一層

+ `Files.walkFileTree(Path start, FileVisitor<T>visitor)` 則可以遞迴辦訪所有層級的所有檔案 / 目錄，並對拜訪過的所有檔案 / 目錄採取「**特定動作**」，將由覆寫`FileVisitor`介面的方法來提供
  1. `preVisitDirectory()`：拜訪目錄**前**要做的事
  2. `visitFile()`：拜訪檔案時要做的事
  3. `postVisitDirectory()`：拜訪目錄**後**要做的事
  4. `visitFileFailed()`：拜訪檔案若**失敗**要做的事
  
+ 藉由每次拜訪檔案 / 目錄後的回傳值（列舉型別`FileVisitResult`的列舉項目）決定是否繼續拜訪其他檔案 / 目錄
  1. `CONTINUE`：繼續
  2. `SKIP_SIBLINGS`：略過同一層的檔案 / 目錄
  3. `SKIP_SUBTREE`：略過下一層檔案樹
  4. `TERMINATE`：結束

+ 實作 `FileVisitor` 介面的類別必須覆寫所有抽象方法，比較麻煩
  + 可考慮改繼承 `SimpleFileVisitor`類別
    + `SimpleFileVisitor`已實作`FileVisitor`介面所有抽象方法，且都回傳 CONTINUE，因此只要覆寫真正需要的方法
  
+ 範例

  1. 使用`Files.walkFileTree()`搭配`FileVistor`介面地回所有目錄

     ![11](https://i.imgur.com/ifqJI3C.png)

  2. 先以**系統管理員**身分開啟命令提示字元，再用`mklink`指令建立*symbolic link* 檔案與目錄

     ```terminal
     C:\java11\code\java11-ocp-2\dir\c06\walkFileTree>mklink dir.sl dir
     symbolic link created for dir.sl <<===>> dir
     
     C:\java11\code\java11-ocp-2\dir\c06\walkFileTree>mklink file.sl file.txt
     symbolic link created for file.sl <<===>> file.txt
     ```

```java
class SimplePrintTree extends SimpleFileVisitor<Path> {
}

class PrintTree implements FileVisitor<Path> {
    private int i;
    public FileVisitResult preVisitDirectory(Path dir, BasicFileAttributes attrs) throws IOException {
        System.out.println(++i + ". preVisitDirectory: " + dir);
        return FileVisitResult.CONTINUE;
    }
    public FileVisitResult visitFile(Path file, BasicFileAttributes attrs) throws IOException {
        System.out.println(++i + ".visitFile: " + file);
        if (attrs.isSymbolicLink()) {
            System.out.println("\t --> " + file.getFileName() + " is SymbolicLink");
        }
        return FileVisitResult.CONTINUE;
    }
    public FileVisitResult visitFileFailed(Path file, IOException exc) throws IOException {
        System.out.println(++i + ". visitFileFailed: " + file);
        return FileVisitResult.CONTINUE;
    }
    public FileVisitResult postVisitDirectory(Path dir, IOException exc) throws IOException {
        System.out.println(++i + ". postVisitDirectory: " + dir);
        return FileVisitResult.CONTINUE;
    }
}

public class WalkFileTreeExample {
    public static void main(String[] args) {
        Path path = Paths.get("dir/c04/walkFileTree").toAbsolutePath();
        try {
            Files.walkFileTree(path, new PrintTree());
            // Files.walkFileTree(path, new SimplePrintTree());
        } catch (IOException e) {
            System.out.println("Exception: " + e);
        }
    }
}
```

執行順序示意圖：

![11](https://i.imgur.com/HyZY1QQ.png)

---

## 使用 `PathMatcher` 類別找尋符合的檔案

### 搜尋檔案

+ 在某路徑下，若想找出所有`java`程式碼檔案，含搜尋子目錄，Windows下可使用指令`dir /s *.java`

+ Java 則使用 `java.nio.file.PathMatcher`介面，用來搜尋符合特定字串的路徑

  ```JAVA
  PathMatcher matcher = FileSystems.getDefault().getPathMatcher(String syntaxAndPattern);
  ```

+ 參數 `syntaxAndPattern` 的語法為「syntax:pattern」，有兩種 syntax

  1. glob 樣式（global command）
     + 較 regex 簡單許多，廣泛應用於檔案系統中的檔案搜尋
  2. regex 樣式（regular expression）
     + 正規表示式

### `glob` 樣式語法介紹

#### 常見字元或符號的代表意義

| 字元  | 使用方式                                                     |
| ----- | ------------------------------------------------------------ |
| `*`   | 任何個數的萬用字元，不跨目錄                                 |
| `**`  | 任何個數的萬用字元，跨目錄                                   |
| `?`   | 代表 1 個字元                                                |
| `\`   | 跳脫（Escape）符號                                           |
| `[ ]` | 找出符合的單一字元，例如<br />1. `[abc]` 表示 a 或 b 或 c<br />2. `[a-z]` 表示可以是 a~z 的任何一個字元<br />3. `[abce-g]` 表示 a 或 b 或 c 或 e 或 f 或 g<br />4. `[!a-c]` 表示非（a 或 b 或 c）<br />`[ ]` 裡面的 `*` 和 `?` 和 `\` 失去特殊意義<br />符號 `-` 如果排第一個，或僅次於`!`，也只代表符號本身 |
| `{ }` | `{ }` 內可以有多個 sub-pattern，使用「,」區隔，滿足一個就成立 |
| `.`   | 檔名前面以「`.`」開頭，例如「`.login`」，比較方式如同一般檔案<br />這類檔案通常都是 hidden，可以使用 `Files.isHidden` 測試 |

#### global pattern 使用範例

| 樣式內容         | 比對符合                                                |
| ---------------- | ------------------------------------------------------- |
| `*.java`         | 檔名以 `.java` 結尾                                     |
| `*.*`            | 檔名中間有 `.`                                          |
| `*.{java,class}` | 檔名以 `.java` 或 `.class` 結尾                         |
| `foo.?`          | 檔名以 `foo.`開頭，後面接1個字元                        |
| `C:\\`           | `C:\foo` 或 `C:\bar` 都符合，在Java中，樣式為 `C:\\\\*` |
| `/home/*`        | 滿足 `/home/gus`（未跨路徑）                            |
| `/home/*/*`      | 滿足 `/home/gus/data`（未跨路徑）                       |
| `/home/**`       | 滿足 `/home/gus` 和 `/home/gus/data`（跨路徑）          |

#### 相關 API 使用釋例

```java
publi static void main(String[] args) {
    FileSystem fs = FileSystems.getDefault();
    
    Path path = Paths.get("D:/1/2/3/Test.java");
    System.out.println(path);
    
    PathMatcher pathMatcher1 = fs.getPathMatcher("glob:D:/*.java");
    System.out.println(pathMatcher1.matches(path));
    
    PathMatcher pathMatcher2 = fs.getPathMatcher("glob:D:/*/*.java");
    System.out.println(pathMatcher2.matches(path));
    
    PathMatcher pathMatcher3 = fs.getPathMatcher("glob:D:/**/*.java");
    System.out.println(pathMatcher3.matches(path));
}
```

| Line# | Description                                                  |
| ----- | ------------------------------------------------------------ |
| 7     | 滿足 D 磁碟機下的 java 程式檔（未跨目錄）= true              |
| 10    | 滿足 D 磁碟機下且第一層目錄裡面的 java 程式檔（未跨目錄）= false |
| 13    | 滿足 D 磁碟機下且**跨目錄（需有目錄）**的 java 程式檔 = false |



#### 延伸案例

| glob 樣式內容              | `D:/*.java` | `D:/*/*.java` | `D:/**/*.java` |
| -------------------------- | ----------- | ------------- | -------------- |
| 路徑：`D:/Test.java`       | **TRUE**    | false         | false          |
| 路徑：`D:/1/Test.java`     | false       | **TRUE**      | **TRUE**       |
| 路徑：`D:/1/2/Test.java`   | false       | false         | **TRUE**       |
| 路徑：`D:/1/2/3/Test.java` | false       | false         | **TRUE**       |

+ 也可以用 `Files.walkFileTree()` 架構走訪所有檔案，搭配 `PathMather` 的 `FileVisitor` 介面實作類別，判斷檔名是否符合 glob 樣式

  ```java
  class Finder extends SimpleFileVisitor<Path> {
      PathMatcher matcher = FileSystems.getDefault().getPathMatcher("glob:*.java");
      int numMatches;
      
      private void find(Path file) {
          Path name = file.getFileName();
          if (name != null && matcher.matches(name)) {
              numMatches++;
          }
      }
      public FileVisitResult visitFIle(Path file, BasicFileAttributes attrs) {
          find(file);
          return CONTINUE;
      }
  }
  
  public class PathMatcherTest2 {
      public static void main(String[] args) {
          Path root = Paths.get("").toAbsolutePath();
          System.out.println("root: " + root);
          Finder finder = new Finder();
          try {
              Files.walkFileTree(root, finder);
          } catch (IOException e) {
              System.out.println("Exception: " + e);
          }
          System.out.println("----\n" + finder.numMatches + " found!!");
      }
  }
  ```

---

## 其它

### `FileStore` 類別

+ `FileStore` 用來提供檔案系統的使用狀況
+ 可以取得總容量、已用空間、未用空間等數據
  + 例如 Windows OS 的磁碟、磁區；Linux OS 的掛載點 mount point

```java
static void printFileStore(FileStore store) throws IOException {
    long toGB = 1024 * 1024 * 1024;
    long total = store.getTotalSpace() / toGB;
    long used = store.getTotalSpace() / toGB - store.getUnallocatedSpace() / toGB;
    
    long avail = store.getUsableSpace() / toGB;
    System.out.format("%-20s %12d(GB) %12d(GB) %12d(GB)\n",
                     store.toString(), total, used, avail);
}

public static void main(String[] args) throws IOException {
    System.out.format("%-20s %12s %12s %12s\n",
                     "Filesystem", "total", "used", "avail");
    if (args.length == 0) {
        for (FileStore store : FileSystems.getDefault().getFileStores()) {
            printFileStore(store);
        }
    } else {
        for (String file : args) {
            FileStore store = Files.getFileStore(Paths.get(file));
            printFileStore(store);
        }
    }
}
```

| Line# | Description                         |
| ----- | ----------------------------------- |
| 11    | 取得檔案系統裡所有 `FileStore` 物件 |
| 16    | 根據 `Path` 取得 `FileStore` 物件   |

### 使用 `WatchService`

介面 `WatchService` 可用來監控目錄 Path 內的檔案何時被新增、刪除、修改

```java
class MyWatchService implements Runnable {
    private WatchService ws;
    public MyWatchService(WatchService ws) {
        this.ws = ws;
    }
    
    @Override
    public void run() {
        try {
            WatchKey key = ws.take();
            while (key != null) {
                for (WatchEvent event : key.pollEvents()) {
                    System.out.printf("Received event: %s for file: %s\n", event.kind(), event.context());
                }
                key.reset();
                key = ws.take();
            }
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
public class WatchServiceTest {
    final static String DIRECTORY_TO_WATCH = "D://WatchServiceTest";
    public static void main(String[] args) throws Exception {
        
        Path watchPath = Paths.get(DIRECTORY_TO_WATCH);
        if (Files.exists(watchPath) == false) {
            Files.createDirectories(watchPath);
        }
        
        WatchService ws = watchPath.getFileSystem().newWatchService();
        MyWatchService fileWatcher = new MyWatchService(ws);
        Thread thread = new Thread(fileWatcher);
        thread.start();
        
        // register a file
        watchPath.register(ws, ENTRY_CREATE, ENTRY_MODIFY, ENTRY_DELETE);
        thread.join();
    }
}    
```

### 由基礎 I/O 轉換至 NIO.2

 JDK.7 - 在傳統的 `java.io.File` 類別中新增方法，使其可以轉換至 NIO.2

```java
Path path = file.toPath();
Files.delete(path);
```

Path 也可以轉換至傳統的 `java.io.File` 物件，方便從基礎 I/O 升級到 NIO.2

```java
File file = path.toFile();
```

---

# 05 執行緒

## 介紹

### 名詞說明

#### 先占式多工（Preemptive Multitasking）

+ 現代電腦要執行的程式個數經常遠多於 CPU 核數，為了讓程式都可以有機會執行，每個要執行的任務會被分配到一小段 CPU 時間（time slice），使每個任務都能分享到 CPU 資源來完成工作
+ CPU 時間通常以毫秒 `milliseconds` 來計算，一旦使用完畢，任務就暫停執行，等待下次分配

#### 任務排程（Task Scheduling）

+ 大部分作業系統都支援多工（multitasking），把 CPU 時間分配給所有執行程式
+ 程式兩個重要組成
  1. **程序 Process**
     + 擁有記憶體來儲存資料（data）和程式碼（code）
     + 使用執行緒接受分配 CPU 時間以執行程式
  2. **執行緒 Thread**
     + 程序可同時擁有多個執行緒各司其職
     + 這些執行緒共享程序的記憶體裡的資料

### 多執行緒的重要性

要讓程式快速執行，必須避免「效能瓶頸（*performance bottlenecks*）」，常見瓶頸：

1. **資源競爭**（**Resource Contention**）：多個任務搶奪同一獨佔資源，未搶到必須等待
2. **輸出/輸入操作阻礙**（**I/O Operations Blocking**）：通常為等待硬碟或網路傳輸資料
3. **CPU 資源未充分使用**（**Underutilization of CPUs**）：程式只用到單核 CPU

### 執行緒類別

Java 類別 Thread 的兩種建立方式：

| action               | benefits/perks                 |
| -------------------- | ------------------------------ |
| 直接繼承 Thread 類別 | 比較簡單                       |
| 實作 Runnable 介面   | 比較有彈性，可以再繼承其它類別 |

#### 建立執行緒：直接繼承 Thread 類別

```java
class ExampleThread extends Thread {  // #1 繼承 java.lang.Thread 類別
    @Override
    public void run() {  // #2 覆寫 run() 方法
        for (int i = 0; i < 100; i++) {
            System.out.println("i:" + i);
        }
    }
}
```

+ 啟用執行緒：

  + 要呼叫 `start()` 方法，Java 會啟動獨立執行緒執行 `run()` 方法內容

  + 若直接呼叫 `run()` 方法，將和一般方法無異

    ```JAVA
    public static void main(String[] args) {
        Thread t1 = new ExampleThread();
        t1.start();
    }
    ```

#### 建立執行緒：實作 Runnable 介面

```java
class ExampleRunnable implements Runnable {  // #1 實作 java.lang.Runnable 介面
    @Override   
    public void run() { // #2 覆寫 run() 方法
        for (int i = 0; i < 100; i++) {
            System.out.println("i:" + i);
        }
    }
}
```

+ 若要以實作 `Runnable` 的類別啟動執行緒，可將其物件放入 Thread 類別的建構子，並用 `start()` 啟動

  ```java
  public static void main(String[] args) {
      Runnable r1 = new ExampleRunnable();
      Thread t1 = new Thread(r1); // 將 ExampleRunnable 物件放入 Thread constructor
      t1.start();
  }
  ```



## 執行緒常見問題

執行緒常遇到問題的三類原因：

1. 使用分享的資料 *shared data*
2. 使用可分段的方法 *non-atomic function*
3. 使用快取的資料 *cached data*

### 使用 `Shared Data` 可能造成的問題

執行緒會潛在 static 和 instance 欄位，可能造成問題如下：

1. 執行緒物件目的在執行其 `run()` 方法

   + 若多個執行緒都要執行 `run()`，就要注意該方法共用的部分
   + 例如：物件實例欄位會被同時存取（concurrently accessed）

2. static 欄位原本就是分享的資料，也無法避免同時被存取的情況

   ```java
   public class ExampleRunnable implements Runnable {
       private int i;  // 將被共用!
       
       @Override
       public void run() {
           for (i = 0; i < 10; i++) {
               System.out.print("i:" + i + ", ");
           }
       }
       
       public static void main(String[] args) {
           ExampleRunnable r1 = new ExampleRunnable();
           Thread t1 = new Thread(r1);
           t1.start();
           Thread t2 = new Thread(r1);
           t2.start();
       }
   }
   ```

   ![image-20221218163741687](https://i.imgur.com/h3ufiAX.png)

   + 以上結果和預期結果有落差

     `i:0, i:1, i:2, i:3, i:4, i:5, i:6, i:7, i:8, i:9,`

   + 當前述類別（**static**）和物件實例（**instance**）欄位資料被多個執行緒共用，出現執行異常時，IDE 是無法警告的

   + 因此 ***安全地（safely）處理被分享的資料***，就成為程式設計師的義務

   + 資料若因為多個執行緒同時存取而產生錯誤，一般不好處理

     1. Thread 的分配由作業系統決定，程式設計師無法干預
     2. 每一台機器的 CPU 效能、個數不盡然相同
     3. 其它程式也會占用 CPU 時間

   + 因此可能有在測試環境無異常，但部署到正式環境後卻經常發生奇怪狀況

     + 盡可能使用執行緒安全的設計（thread-safe），減少使用 `shared data`

   + 類別內有些資料不會被多執行緒分享，永遠都 `thread-safe` 的例子

     1. 區域變數 *local variables*
     2. 方法參數 *method parameters*
     3. 例外處理參數 *exception handler parameters*

### 使用 `Non-Atomic Functions` 可能造成的問題

+ `atomic function` - 用原子的概念描述一個功能
  + 原子無法再分割，代表 `single function`，即該功能只有一個步驟
+ Java 裡，即使程式碼只有一句敘述，也不代表它就是 `atomic function`
  + 以整數 i 使用 遞增運算子的 `i++` 為例，Java 以 3 個步驟執行
    1. 對整數 `i` 建立暫時副本
    2. 暫時副本增加 1
    3. 將暫時副本的結果回寫 `i`
  + 另外有些 64 bit 變數的存取也可以使用 2 個 32 bit 的操作完成

### 使用 `Cached Data` 可能造成的問題

+ 執行緒 `thread` 因為程序 `process` 需要同時執行不同工作而產生
+ 為求執行效能，執行緒啟動時，會將程序中的 `main memory` 內的分享資料複製一份，放在自己的 `working memory` 作為快取複製（`cached copies`），工作結束後寫回，如此一來可以避免程式進行過程中，執行緒必須不斷向程序要求資料而造成的效率問題
+ 這樣的設計，讓執行過程中的每一執行緒各自努力，無法**即時**和其他執行緒分享工作成果，必須等到工作結束
+ 只有以下情況才能讓執行緒將各自 working memory 的異動結果寫回 main memory：
  1. 使用到 `volatile` 宣告的變數
  2. 使用到 `synchronized` 宣告的方法，亦即準備鎖定和解索物件 monitor
  3. 執行緒執行時的第一個動作或最後一個動作
  4. 執行緒啟動或執行緒結束時
+ 若程式設計需要多執行緒在工作過程仍然能互相溝通訊息，就必須善用上述四個條件
  + 尤其是使用 `volatile` 關鍵字宣告，其由來和使用方式：
    1. 程式設計中，如果資料經常維持不變，可以將之固定在記憶體裡，或複製出來使用，稱之為「**快取複製**（cached copies）」
    2. 單字 `volatile` 解釋為「**易變的、反覆無常的**」：
       + 加上此宣告相當於告訴 Java 該欄位經常有變化，不適合產生快取複製
       + 因此所有執行緒將皆存取同一份資料，例：`volatile int i;`
    3. 必須了解宣告 `volatile` 只是不產生快取複製，和執行緒安全是兩回事
       + 還是必須利用先前所提的作法來保證執行緒安全
       + volatile 宣告可以應用在「精準終止執行緒的執行」

```JAVA
class MyRunnable implements Runnable {
    public volatile boolean running = true;  // line 2
    @Override
    public void run() {
        System.out.println("Thread started");
        while (running) {
            // ...
        }
        System.out.println("Thread finishing");
    }
}

public class StopThreadExample {
    public static void main(String[] args) {
        MyRunnable r1 = new MyRunnable();
        Thread t1 = new Thread(r1);       // line 16
        t1.start();
        // ...
        r1.running = false; // line 19 : terminate the thread immediately
    }
```

| line# | desc                                                         |
| ----- | ------------------------------------------------------------ |
| 16    | thread `t1` 在啟動前**預設**會自己複製一份變數 running（值=true）作為快取複製 |
| 2     | 所以如果沒有宣告變數 running 為 `volatile`                   |
| 19    | 即便在 main 執行緒將 running 改為 false，執行緒 t1 不一定會馬上知道，必須等到有事件觸發，讓 working memory 和 main memory 同步，執行緒 `t1` 才會收到通知而停止 |

...

## 執行緒的 synchronized 與等待

### 使用 `synchronized` 關鍵字

+ 建立執行緒安全的程式，除了盡量使用執行緒安全的變數之外，就是使用「**synchronized**」關鍵字宣告*方法*或是*更小的程式碼區塊*：

  1. `synchronized` 和 `volatile` 宣告有類似功用：
     + 執行緒在執行該區塊的最初和最後時，會將變數值寫回 main memory
  2. 該區塊為獨占執行（*exclusive execution*）：
     + 亦即同一時間只允許一個執行緒使用
     + 可解決 `non-atomic`問題，所以區塊內為執行緒安全

+ 執行緒取得獨占執行權的機制：

  1. 每個 Java 物件都有一個「**object monitor**」，執行緒可以對它進行鎖定（lock）和解鎖（unlock）

     + 若鎖定成功，表示取得該物件的獨占執行權
     + 此時其它執行緒無法使用該物件的 `synchronized` 程式區塊，等同單一執行緒環境

  2. 要使用宣告 `synchronized` 的方法，就必須取得「**this**」的 object monitor

  3. 要使用宣告 `static` 的 `synchronized`方法，同理也必須取得類別的「**class monitor**」

  4. 要使用宣告 `synchronized` 區塊，必須指定要使用哪一個物件的 monitor

     ```java
     synchronized (this) {
         // ...
     }
     ```

  5. 使用 `synchronized` 區塊可以有巢狀結構，且可以使用不同的 object monitor

```java
class SynchronizedAll {
	private void sleep() {
		try {
			Thread.sleep(5000);
		} catch (InterruptedException e) {}
	}
	public synchronized void m1() {
		sleep();
		System.out.println("-- Run m1() at: " + new Date());
	}
	public synchronized void m2() {
		sleep();	
		System.out.println("-- Run m2() at: " + new Date());
	}
} /* 將兩個public方法都宣告為synchronized，如此建立出的物件，
     m1()和m2()方法將同時只能有一個被呼叫執行 */

class M1Runner extends Thread {
	SynchronizedAll o;
	M1Runner(SynchronizedAll o) {
		this.o = o;
	} 
    // 建立執行緒類別M1Runner，在建構子傳入一個SynchronizedAll物件後，呼叫m1()方法
    
	public void run() {
		o.m1();
	}
}

class M2Runner extends Thread {
	SynchronizedAll o;
	M2Runner(SynchronizedAll o) {
		this.o = o;
	}
    // 建立執行緒類別M2Runner，在建構子傳入一個SynchronizedAll物件後，呼叫m2()方法
    
	public void run() {
		o.m2();
	}
}

public class SynchronizedTest {
	public static void main(String[] args) {
		SynchronizedAll o = new SynchronizedAll();
         // 建立一個 SynchronizedAll 物件
        
		System.out.println("Start main at: " + new Date());
        
		Thread m1 = new M1Runner(o);
		m1.start();
        // 建立 M1Runner 執行緒物件，並傳入之前建立的 SynchronizedAll 物件
        
		Thread m2 = new M2Runner(o);
		m2.start();
        // 建立 M2Runner 執行緒物件，
        // 並傳入和 M1Runner 所傳入的相同的 SynchronizedAll 物件
        
		//m1.interrupt();
		System.out.println("End main at: " + new Date());
	}
}
```

> 因為 `SynchronizedAll` 內 `m1()` 及 `m2()` 方法都是 `synchronized`，要執行都要取得 this 的 object monitor，因此同時間只能有一個方法被呼叫

### 使用 `synchronized` 的時機

+ `java.util.ConcurrentModificationException` :
  + 如果 Java 偵測到集合物件的內容將被同時修改（不限定是否多執行緒所為），就會拋出 `ConcurrentModificationException`
    + 此為「**fail-fast**」行為模式，亦即對於錯誤或是可能造成錯誤的情況，馬上作出反應
    + 為了避免發生 **fail-fast** 狀況，應避免在執行 `getSummary()` 方法時，其它 2 個方法被同時呼叫
      + 可以將這三個方法都宣告為 `synchronized`，如此要呼叫方法前，必須取得該物件的唯一 **object monitor**，就不會有被同時執行的可能性
+ `ConcurrentModificationException` 不是只有在多執行緒的情況下才會發生
  + 以下的 **fail-fast_1** 或 **fail-fast_2** 會拋出 `ConcurrentModificationException`
    + 原因：使用 iterator 或進階 for-loop 走訪 map 物件成員，同時去刪除 map 成員
    + 避免方法：
      + 複製 map，讓新複製的 map 用於走訪成員
      + 再用取得的 key 刪除另外一個 map 物件的成員


```java
public class ConcurrentModificationExceptionTest {
    public static void main(String[] args) {
        Map<Integer, String> map = new HashMap<>();
        map.put(1, "a");
        map.put(1, "b");
        map.put(3, "c");
        
        // fail-fast 1
        try {
            Iterator<Integer> iter = map.keySet().iterator();
            while (iter.hasNext()) {
                Integer key = iter.next();
                if (key >= 2) {
                    map.remove(key);
                }
            }
        } catch (java.util.ConcurrentModificationException e) {
            e.printStackTrace();
        }
        
        // fail-fast 2
        try {
            for (Integer key : map.keySet()) {
                if (key >= 0) {
                    map.remove(key);
                }
            }
        } catch (java.util.ConcurrentModificationException e) {
            e.printStackTrace();
        }
    }
}
```

### 縮小 synchronized 的程式區塊

+ 物件裡所有 synchronized 方法在執行前，都必須取得 object monitor

+ 因此如果越多方法使用 synchronized，或是被 synchronized 的方法內容越長，都會造成「執行等待」

+ 盡可能使用 synchronized 程式區塊，而非直接 synchronized 整個方法

+ 減少被 synchronized 的程式碼，有助於減少「執行等待」的情況

  ```java
  public String getSummary() {
      StringBuilder note = new StringBuilder();
      synchronized (this) {              // line 3
          Iterator<Item> iter = cart.iterator();
          while (iter.hasNext()) {
              Item i = iter.next();
              note.append("Item:" + i.desc() + "\n");
          }
      }  // line 9
      return note.toString();
  }
  ```

  > 可以直接把 `getSummary()` 宣告為 synchronized 方法，
  >
  > 但比較好的做法是檢討方法內真正影響執行緒安全的程式碼。
  >
  > + line 3 ~ line 9 : synchronized 程式碼區塊可讓影響區域縮小
  >
  > + 根據 line 3 宣告 : 要執行本區塊必須取得 this 的 object monitor

### 其它執行等待的情況

除了 synchronized 程式區塊造成的執行等待，還有以下幾種情形必須預防

#### 1. **Starvation** : 因搶不到資源而排隊

+ 指兩個執行緒共搶一個資源，其中某一個經常可以取得資源（*貪心執行緒，greedy thread*）
+ 另外一個經常無法取得資源（*飢餓執行緒，starved thread*）

#### 2. Live Lock : 因太忙碌而排隊

+ 指多個執行緒因為等待資源而排隊，thread_B 等 thread_A、thread_C 等 thread_B、thread_D 等 thread_C
+ thread_A 完成可以輪到 thread_B，thread_B 完成輪到 thread_C，以此類推

#### 3. Dead Lock : 2 個執行緒互相絆住對方，導致永遠等待

+ thread_A : 必須先後取得「資源1」和「資源2」

+ thread_B : 必須先後取得「資源2」和「資源1」

+ Deadlock 情況 : A 持有資源 1，等待資源 2；此時 B 持有資源 2，等待資源 1

  + 除非 Thread-A 或 Thread-B 其中有一方讓出資源或被終止，才能結束 Deadlock

  ![image-20221219105806769](https://i.imgur.com/yoRNlvk.png)

> 🍪☕ **Little Tips**
>
> 要產生 dead lock 需要 2 個執行緒和 2 個被搶奪的資源物件。
>
> 資源可以是任何物件，使用 `new Object()` 都可以。e.g.:
>
> ```java
> final String resource1 = "jim1";
> final String resource2 = "jim2";
> ```
>
> 因為字串池有重複使用相同內容字串的機制，若改為以下內容，則兩個變數**實際指向同一個字串物件**：
>
> ```java
> final String resource1 = "jim";
> final String resource2 = "jim";
> // 因為被搶奪的資源只有一個，無法構成 dead lock
> ```

...

## 其它執行緒方法介紹

### 使用 `interrupt()` 方法

除了利用 `volatile` 宣告的變數來停止執行中的執行緒，也可以用 `interrupt()` 方法

執行中的執行緒可以藉由 Thread.interrupted() 方法不斷確認是否收到中斷指令，若是，就中斷目前執行工作

```java
class InterruptedRunnable implements Runnable {
    @Override
    public void run() {
        System.out.println("Thread started");
        while (!Thread.interrupted()) {
            /* 執行中的執行緒可藉由 interrupted 方法不斷確認是否收到中斷指令
               若是，就中斷目前執行工作 */
            System.out.println("I am running...");
        }
        System.out.println("Thread finishing");
    }
}
public class InterruptThreadExample {
    public static void main(String[] args) {
        InterruptedRunnable r1 = new InterruptedRunnable();
        Thread t1 = new Thread(r1);
        t1.start();
        try {
            Thread.sleep(1);
        } catch (InterruptedException e) {}
        t1.interrupt();
        // 對執行中的執行緒下達 interrupt 指令
    }
}
```

### 使用 `sleep()` 方法

若要使執行緒暫停一段時間，可以呼叫 class Thread 的靜態 `sleep()` 方法

```java
public static native void sleep(long millis) throws InterruptedException;
```

+ 呼叫 `Thread.sleep(4000)` 即暫停執行 4 秒鐘，4 秒之後再等待 CPU 分配時間

  + 拿到才能繼續執行任務，停止時間「**至少**」為 4 秒鐘

+ 休眠中的執行緒隨時有被叫醒而中斷休眠的可能，所以被要求必須處理 `InterruptedException`

  + 並在 catch block 中決定被終止休眠後要做的事

    ```java
    long start = System.currentTimeMillis();
    try {
        Thread.sleep(4000);
    } catch (InterruptedException ex) {
        // what to do?
    }
    long time = System.currentTimeMillis() - start;
    System.out.println("Slept for " + time + "ms");
    ```

    

### 使用其它方法

Class Thread 的其它常用方法：

1. `setName(String)`, `getName()`, `getId()`: 和執行緒的識別有關
2. `isAlive()`: 判斷執行緒是否已經結束
3. `isDaemon()` & `setDaemon(boolean)`: 
   + 可以將執行緒設為 daemon 和判斷是否為 daemon
   + 執行緒預設是 **non-daemon**，JVM會等待執行中的 non-daemon 執行緒都結束，才會結束；
   + 此時若還有其他 daemon 的執行緒正在執行，一樣會結束 JVM
4. `join()`: 插隊到目前執行緒的前面，執行完後才輪到目前執行緒
5. `Thread.currentThread()`: 取得執行中的執行緒

以下 3 個方法繼承自 Object class：

1. `wait()`：不限時間的等待，等候 `notify()` 被呼叫後醒過來

2. `notify()` 和 `notifyAll()` : 通知 `wait()` 中的執行緒

   > 🍪☕ **Little Tips**
   >
   > `daemon` 
   >
   > + 在多執行緒的情形下，無法影響 JVM 的結束
   >
   > + 是一種抽象、虛無的表徵，很難證明其實質影響力

以下示範 `join()` 和 `setDaemon()` 之使用方式與其影響

```Java
class ThreadTest extends Thread {
    public void run() {
        try {
            System.out.println("Thread T is starting...");
            for (int i = 0; i < 3; i++) {
                Thread.sleep(1000);
                System.out.println("Thread T is running...");
            }
            System.out.println("Thread T is ending...");
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}

public class ThreadMethodTest {
    public static void main(String[] args) {
        System.out.println("Thread main is starting...");
        testNormal();
        // testJoin();
        // testDaemon();
        System.out.println("Thread main is ending...");
    }
    
    /** 
    * #1 thread main 和 thread t 啟動後各走各的，互不影響 
    * 兩者都結束後，程式結束，JVM關閉
    */
    private static void testNormal() {
        Thread t = new ThreadTest();
        t.start();
    }
    
    /** 
    * #2 thread main 啟動 thread t，在執行 t.join() 時，
    * thread t 會插隊到 main 前面，只有 thread t 結束後才會執行 thread main
    */
    private static void testJoin() {
        Thread t = new ThreadTest();
        t.start();
        try {
            t.join();  
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
    
    /** 
    * 將執行緒 t 設為 daemon 後，一旦 non-daemon 的 thread main 結束，JVM 就關閉
    * thread main 結束後，即便 thread t 尚未結束，JVM 也會關閉，和先前2種測試明顯不同 
    */ 
    private static void testDaemon() {
        Thread t = new ThreadTest();
        t.setDaemon(true);
        t.start();
        // t.setDaemon(true);  // java.lang.IllegalThreadStateException
    }
}
```

... 

### 不建議使用的方法

+ class Thread 一些不建議使用的方法：

  1. 可能造成問題，避免使用

     + `setPriority(int)`
     + `getPriority()`

  2. 已經 **deprecated**，不該使用

     + `destroy()`
     + `resume()`
     + `suspend()`
     + `stop()`

     > 使用 deprecated 描述表示方法可能
     >
     > + 寫法不好
     > + 命名不符合傳統，不建議再使用
     > + 未來可能移除 ex.  `@Deprecated`、~~stop~~() {...}、~~resume~~();
     >
     > ```java
     > // 註記方式
     > @Deprecated
     > public final void stop() {
     >     SecurityManager security = System.getSecurityManager();
     >     if (security != null) {
     >         checkAccess();
     >         if (this != Thread.currentThread()) {
     >             security.checkPermission(
     >                 SecurityConstants.STOP_THREAD_PERMISSION);
     >         }
     >     }
     >     // A zero status value corresponds to "NEW", it can't change to
     >     // not-NEW because we hold the lock.
     >     if (threadStatus != 0) {
     >         resume(); // Wake up thread if it was suspended; no-op otherwise
     >     }
     >     // The VM can handle all thread states
     >     stop0(new ThreadDeath());
     > }
     > ```

---

# 06 執行緒與並行 API `Concurrency API`

## 使用並行 API

### 並行 API 介紹

+ Concurrency API 是套件 `java.util.concurrent` 下的相關類別和介面。
+ 導入於 Java 5，陸續擴充，用於支援多執行緒執行，亦即 concurrent programming，包含
  1. 執行緒安全（thread-safe）的集合物件
  2. 取代傳統 *synchronization* 和 *locking* 的替代方案
  3. 執行緒池（thread pools），又分成：
     + 執行緒數量「固定」或「浮動」的執行緒池
     + 分進合擊的 *Fork-Join Framework*

### `AtomicInteger` 類別

+ class `AtomicInteger` 位於`java.util.concurrent.atomic`套件裡

+ 提供執行緒安全又不需要使用 *synchronization* 和 *locking* 機制來控管的物件

+ 此類別裡的方法都是 atomic function，例如 `compareAndSet()`、`getAndIncrement()`

  ```java
  AtomicInteger ai = new AtomicInteger(5);
  if (ai.compareAndSet(5, 42)) {  // 🥨
      System.out.println("after compareAndSet(): " + ai);  // 42
  }
  ai.getAndIncrement();  // 🥨
  System.out.println("after getAndIncrement(): " + ai);   // 43
  ```

  | 🥨                    | name                                                         |
  | -------------------- | ------------------------------------------------------------ |
  | `compareAndSet(a,b)` | 判斷數值是否為 a，若是則設定為 b，亦即將 a 取代為 b          |
  | `getAndIncrement()`  | 取得數值後加 1。作用同遞增運算子，但 atomic function 不須使用 synchronized block |

### `ReentrantReadWriteLock` 類別

+ 有別於`synchronization`與`monitor`機制，`ReentrantReadWriteLock`提供另一種鎖定（locking）

+ 可根據不同情況（conditions）調整執行緒等待（wait）的架構

  1. 過去的 monitor 未分類，一個執行緒取得 monitor 之後，其它執行緒必須等待鎖定該 monitor

  2. 使用 `ReentrantReadWriteLock`，將原本由每個物件唯一的 object monitor，改提供 **read lock** 和 **write lock** 兩種鎖定機制

     + 有 thread 先取得 **read lock** 時，其它執行緒可以同時再取得 read lock
       + 允許多個執行緒同時 read，但沒有執行緒可以取得 write lock
     + 一旦有執行緒取得 **write lock**，將排擠其它執行緒取得 read lock 和 write lock

  3. 「**Reentrant**」

     + 如果有執行緒已經使用某個 synchronized 方法（= 取得某物件的 object monitor），則該執行緒可繼續進入其它使用相同 object monitor 的任何一個 synchronized 方法

     + 申請一把鑰匙之後，可以繼續打開每一個使用相同鑰匙的門，不用每次開門後都繳回，也不用為了公平而重新申請排隊

       > 1️⃣ 允許多執行緒同時「holding read lock」
       >
       > 2️⃣ 同一時間一旦有一個執行緒「holding write lock」，
       >
       > ​      就不會再有其它執行緒「holding read lock」或「holding write lock」

### 執行緒安全的集合物件

`java.util.*` 下的集合物件預設非執行緒安全，如果要 thread-safe，必須特別處理：

1. 對所有修改集合物件的程式碼，都必須放在 *synchronize* 區塊

2. 使用特定類別及方法建立 *synchronized wrapper* class

   ```java
   java.util.Collections.synchronizedList(List<T>)
   ```

3. 改用套件 `java.util.concurrent` 套件下的集合物件

   **注意**：即便是執行緒安全的集合物件，不代表其成員也是

常用執行緒安全的集合物件：

| `java.util.concurrent.*` | `java.util.*` |
| ------------------------ | ------------- |
| `CopyOnWriteArraySet`    |               |
| `CopyOnWriteArrayList`   | `ArrayList`   |
| `ConcurrentHashMap`      | `HashMap`     |
| `ConcurrentSkipListMap`  | `TreeMap`     |

`Queue` family 的執行緒安全集合物件：

![1](https://i.imgur.com/E0u3aAs.png)

> 🍪☕ **Little Tips**
>
> 支援執行緒安全的集合物件裡，常可以看到 `CopyOnWrite` 的命名方式，暗示該集合物件如何支援執行緒安全
>
> 1. 當該集合物件要增加成員時，不直接添加，而是
>    + 先將當前集合物件複製出一個新的集合物件，然後在新的集合物件裡增加成員
> 2. 添加完成員之後，再將原集合物件的物件參考指向新的集合物件
> 3. 好處：集合物件可以讀寫並行，不需要在修改的時候排除其它行為，因為當前集合物件不會添加任何元素
>    + `CopyOnWrite` 集合物件是一種讀寫分離的思想實踐，對不同的集合物件讀取和寫入

### 常用的同步器工具類別

套件 `java.util.concurrent` 下，提供數種支援特殊情境的同步器（*synchronizers*）類別

| class                       | description                                                |
| --------------------------- | ---------------------------------------------------------- |
| `Semaphore`                 | 傳統的 concurrency（平行執行）工具                         |
| `CountDownLatch`            | 暫停 thread 直到某種情境達成，例如信號數量、事件、預設條件 |
| `CyclicBarrier`（循環路障） | 於平行執行時提供同步點，可循環使用                         |
| `Phaser`                    | 更有彈性的 `CyclicBarrier`                                 |

```java
public static void main(String[] args) {
	int stopUntil = 2;  // 設定多少個 thread 抵達才放行的條件
	int totalThreadCnt = 3;  // 任意調整啟動的 thread 個數，測試 CyclicBarrier 類的效果
	final CyclicBarrier barrier = new CyclicBarrier(stopUntil);
    // 宣告並初始化 class Cyclic 
	for (int i=1; i<=totalThreadCnt; i++) {
		new Thread() {
			public void run() {
				try {
					System.out.println("before await");
					barrier.await();  // line
					System.out.println("after await");
				} catch (BrokenBarrierException | InterruptedException ex) {
				}
			}
		}.start();
	}
}
```

> `await()` 方法：像是柵欄，平時放下。
>
> 只有滿足 `CyclicBarrier` 建構子設定的 `stopUntil` 個數的 thread 抵達後，才會放行

## 使用 `ExecutorService` 介面同時執行多樣工作

### 使用更高階的多執行緒執行方案

多執行緒程式架構可同時執行工作，提升效率，但也容易衍生問題，必須小心操控。

傳統 `API` 不容易被適當使用，可考慮使用以下兩個更高階的替代方案：

1. **執行者服務**（*java.util.concurrent.ExecutorService*）
   + 建立並重複使用多執行緒
   + `ExecutorService` 介面
     + 除了可以使用過去的 `Runnable` 介面定義工作內容
     + 也可用新的 `Callable` 介面定義工作內容，允許在未來工作結束後檢視結果
2. **分進合擊程式框架**（*Fork-Join Framework*）
   + Java 7 推出的特殊「**工作竊取**（work-stealing）」平行運算架構
   + 用於多執行緒執行，是一種特化的 ExecutorService
   + 程式運行時，除了不斷將整體工作進行切割外，也讓**有能力、較有餘裕的執行緒在做完份內工作後，可以竊取（stealing）別人的工作來執行**
     + 支援 *能者多勞* 理念的多執行緒執行架構

### `ExecutorService`概觀

+ `ExecutorService` 介面是執行緒池（thread pool）的概念，使用過的執行緒皆可回收池內繼續下次使用
+ 執行緒池也會負責管理所有執行緒的生命週期，使用 `ExecutorService`執行多執行緒工作時：
  1. 不需要自己建立和管理多執行緒，且可以平行執行
  2. 可分成兩種任務
     + `java.lang.Runnable`
     + `java.util.concurrent.Callable`
  3. 使用 class `Executors` 可以取得 interface `ExecutorService` 的實作，常用兩種
     + 快取式執行緒池 *cached thread pool*
     + 固定式執行緒池 *fixed thread pool*

#### 快取式執行緒池（cached thread pool）

+ 建立方式

  ```java
  ExecutorService es = Executors.newCachedThreadPool();
  ```

+ 特點

  | feature      | description                                           |
  | ------------ | ----------------------------------------------------- |
  | 數量控制     | 執行緒數量由執行緒池**自動調控**                      |
  | 是否重複使用 | 執行緒工作完成後，回收重複使用                        |
  | 工作量大時   | 遇到需要大量 CPU 運算的工作，**執行緒可能會一直增生** |
  | 生命週期     | 預設閒置超過 **60 秒**，就終止生命週期                |

#### 固定式執行緒池（fixed thread pool）

+ 建立方式

  ```java
  int cpuCount = Runtime.getRuntime().availableProcessors();
  // 參考主機 CPU 數量建立執行緒數量
  ExecutorService es = Executors.newFixedThreadPool(cpuCount);
  ```

+ 特點

  | feature/category | description                          |
  | ---------------- | ------------------------------------ |
  | 數量控制         | 執行緒數量固定                       |
  | 是否重複使用     | 執行緒工作完成後，回收重複使用       |
  | 工作量大時       | 工作太多時，必須等待忙碌的執行緒釋出 |
  | 生命週期         | 數量固定，不會主動終止生命週期       |

### 使用 `java.util.concurrent.Callable`

+ `ExecutorService` 可以協助管理執行緒，但還是需要自己定義執行緒的工作內容

+ 除了使用 `java.lang.Runnable` 介面，也可以使用另外一個 `java.util.concurrent.Callable` 介面實作類別來定義要執行的工作

  | Runnable                                                     | Callable                                                     |
  | ------------------------------------------------------------ | ------------------------------------------------------------ |
  | public interface `Runnable` {<br />       void run();<br />} | public interface `Callable<V>` {<br />       V call() throws Exception;<br />} |

+ `Callable` 和 `Runnable` 相似，主要不同地方

  1. 可以回傳結果（使用泛型）
  2. 可以拋出 Exception

+ 若使用傳統 **Runnable** 介面實作物件作為 `ExecutorService` 的執行工作內容，可用下例執行工作

  ```java
  static void useRunnable(ExecutorService es, Runnable runnable) {
      es.submit(runnable);
  }
  ```

+ 若使用 **Callable** 介面實作物件作為 `ExecutorService` 的執行工作內容，則用下例

  ```java
  static<V> void useCallable(ExecutorService es, Callable<V> callable) {
      Future<V> future = es.submit(callable);
      try {
          V result = future.get();
          System.out.println(result.toString());
      } catch (Exception ex) {
          ex.printStackTrace();
      }
  }
  ```

  > `ExecutorService.submit()` 方法傳入 Callable 實作物件時，可以回傳 **Future** 類別的物件

  ...

### 使用 `java.util.concurrent.Future`

+ 介面 `ExecutorService` 執行 Callable 工作後的回傳結果屬於 `Future<V>` class：

  ```java
  Future<V> future = es.submit(callable);
  ```

+ 但是真正工作結果隱藏在 Future 物件裡面，必須再呼叫 `get()` 去取得結果。回傳型態 **V** 必須和`Callable<V>`裡定義的泛型物件型態一致：

  ```java
  V result = future.get();
  ```

+ 原因：main thread 在前景（foreground）執行工作，而不是 main 的 thread 都在背景（background）執行工作

> 🍪☕ **Little Tips**
>
> 餐館內用座位 & 外帶領取號碼牌
>
> 號碼牌：隱含一種「**未來交貨**」之意
>
> | noodle shop        | java program                                                 |
> | ------------------ | ------------------------------------------------------------ |
> | 「號碼牌」         | Java 裡的 `Future` 物件                                      |
> | 拿「麵」需要號碼牌 | 包含在 `Future` 裡的泛型物件，呼叫 `Future` 的 `get()` 方法可取得該泛型物件 |
> | 「煮麵」           | `Callable` 介面裡的 `call()` 方法定義的內容                  |
> | 「煮麵的師傅」     | `ExecutorService`裡的執行緒                                  |
> | 「餐館」           | `ExecutorService`                                            |
> | 「您」             | `main` 執行緒                                                |
>
> 如果買東西需要等待，大部分商家`Executor Service` 都會給您一張號碼牌 `Future`，等時間差不多再回來領取（`get`）購買的真正商品。
>
> 領取號碼牌，使用 `Callable`。
>
> 不領取號碼牌，即是使用 `Runnable`。

+ 呼叫 `Future.get()` 方法時，若結果尚未出爐，main 執行緒就會進入等待狀態，比較好的做法：
  1. 呼叫 `get()` 方法前，應該先丟出（**submit**）所有工作
     + 因為呼叫 `get()`之後只能耐心等結果
  2.  呼叫 `get()` 方法前也可以先呼叫 `isDone()`方法，確認是否工作完成
     + 取貨前先打電話確認是否完成，以免到達後還需要等待
     + 呼叫多載的另外一版本 `get(long timeout, TimeUnit unit)` 方法，明確指定等待時間

### 關閉 `ExecutorService`

+ 因為 `ExecutorService`所建立的執行緒都是 **non-daemon**，JVM 會因為這些執行緒存在，而導致永遠不會結束

+ 若要結束程式，必須呼叫 `ExecutorService` 介面的 `shutdown()` 方法

  ```java
  es.shutdown();
  /* shutdown() 方法會在所有執行緒工作都結束後，關閉 ExecutorService 
     並終止所有執行緒生命週期，讓程式結束  */
  try {
      es.awaitTermination(5, TimeUnit.SECONDS);
      /* 若已執行 shutdown() 指令，但執行緒工作結束後還是無法關閉，
         可以使用 awaitTermination(5, TimeUnit.SECONDS) 方法傳入等待時間，時間到後會強制關閉*/
  } catch (InterruptedException ex) {
      System.out.println("Stopped waiting early");
  }    
  ```

### `ExecutorService`完整範例

+ 應用 `Callable`介面和`Future`類別的 `ExecutorService`使用方式

  ```java
  class CallableTask implements Callable<String> {
      @Override
      public String call() throws Exception {
          Thread.sleep(20000);
          System.out.println(new Date() + ": finish job");
          return (new Date() + ": done");
      }
  }
  
  public class ExecutorServiceTest {
      public static void main(String[] args) {
          ExecutorService es = Executors.newCacheThreadPool();
          // ExecutorService es = Executors.newFixedThreadPool(5);
          
          Callable<String> task = new CallableTask(); // 建立Callable實作物件
          Future<String> future = es.submit(task); 
          // 讓es分派執行緒進行Callable實作物件定義的工作內容，並取得號碼牌Future物件
          try {  // line 17
              String result = future.get(); 
              System.out.println(result);
          } catch (Exception ex) {
              ex.printStackTrace();
          } // ln17 ~ 22若呼叫get()方法時工作尚未結束，就會進入等待狀態
          
          es.shutdown(); // shutdown()在所有執行緒工作結束後，才關閉自己
          System.out.println(new Date() + ": service shutdown");
          
          try {
              es.awaitTermination(5, TimeUnit.SECONDS);
              /* 若已執行shutdown()，但所有執行緒工作結束後還是無法關閉，則可以使用
                 awaitTermination(5,TimeUnit.SECONDS) 方法傳入等待時間，時間到後強制關閉*/
          } catch (InterruptedException ex) {
              System.out.println("Stopped waiting early");
          }
      }
  }
  ```

### `ExecutorService` 進階範例

![image-20221221144232769](https://i.imgur.com/lXaXD7Y.png)

+ Java Socket 程式架構提供網路世界兩台獨立主機上的 Java 程式互相連線的機制，一般分為
  1. **主機端**（server）：提供網路連線的程式
  2. **用戶端**（client）：要求網路連線的程式
+ 成功建立連線有以下條件：
  1. 主機端必須提供一個固定位置（包含 IP、port 號），用戶端要連接的話需要先知道這個位置
     + **IP** : 網路世界的住址，可幫助用戶端在網際網路裡找到單一主機
     + **Port** : 
       + 主機裡可能有諸多正在執行的程式，可用 post 
       + 可以是 0-65535 之間的任一整數，0-1024的port號已被作業系統保留
       + 其它程式通常選擇1024之後的編號作為自己程式的 port 號
  2. Java 的 Socket 主要使用 `ServerSocket` 以及 `Socket` 兩個類別
     + 主機端用 `ServerSocket` 物件的 **listen()** 方法監聽來自用戶端的連線請求
     + 用戶端用 `Socket` 物件和主機端做連接
     + 主機端用 `ServerSocket` 物件的 **accept()** 方法來串接用戶端的 `Socket` 物件
  3. 主機端和用戶端連接之後，就可以用以下方法來傳輸資料
     +  `Socket` 物件的 `getInputStream()` 
     +  `Socket` 物件的 `getOutputStream()` 
  4. 資料傳輸完成後，記得呼叫 `Socket` 物件的 `close()` 方法結束相關資源

#### 範例一：`SocketServersStartup.java` 扮演主機端程式的角色

+ 啟動五個執行緒，分別使用 port# 10000 - 10004 扮演主機端角色，等待/傾聽用戶端連線的要求

+ 如果用戶端連線成功，主機端在停頓五秒後，將輸出 `feedback_from_port 號` 的訊息給客戶端

```java
public class SocketServerStartup {
    public static void main(String[] args) {
        for (int port = 10000; port < 10005; port++) {
            new Thread(new MySocketServer(port)).start();
        }
    }
}
class MySocketServer implements Runnable {
    int port;
    MySocketServer(int port) {
        this.port = port;
    }
    @Override
    public void run() {
        System.out.println("Server " + port + ": Listening...");
        while (true) {
            try {
                // 聆聽 port 是否被呼叫
                ServerSocket serverSock = new ServerSocket(port);
                // 取得 Socket 連線
                Socket clientSock = serverSock.accept();
                // 暫停 5 秒
                try {
                    Thread.sleep(5000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                // 輸出訊息給 Socket 的用戶端
                PrintWriter pw = new PrintWriter(clientSock.getOutputStream(), true);
                pw.println("feedback_from_" + port);
                // 關閉 Socket 網路連線
                serverSock.close();
                clientSock.close();
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
    }
}
```

#### 範例二 : 建立類別 `SingleThreadTest` 驗證使用單一執行緒依序訪問五個 Socket 主機

+ 使用單一執行緒逐 port 連線主機的結果，必須使用 `5sec * 5 = 25sec` 的時間

```java
public static void main(String[] args) {
    out.println("SingleThread starts at: " + new Date());
    String host = "localhost";
    for (int port = 10000; port < 10005; port++) {
        try (Socket sock = new Socket(host, port); 
               Scanner scanner = new Scanner(sock.getInputStream());) {
            String feedback = scanner.next();
            out.println("Call " + host + ":" + port +
                       ", and get: " + feedback + " at " + new Date());
        } catch (NoSuchElementException | IOException ex) {
            out.println("Error talking to " + host + ":" + port);
        }
    }
}
```

#### 範例三 : 建立類別 `SocketClientCallable` 實作 `Callable` 介面

+ 用以定義多執行緒訪問 `Socket` 主機的工作內容

```java
public class SocketClientCallable implements Callable<String> {
    private String host;
    private int port;
    public SocketClientCallable(String host, int port) {
        this.host = host;
        this.port = port;
    }
    @Override
    public String call() throws IOException {
        try (Socket sock = new Socket(host, port);
              Scanner scanner = new Scanner(sock.getInputStream()); ) {
            String feedback = scanner.next();
            return feedback;
        }
    }
}
```

#### 範例四 : 建立類別 `MultiThreadTest` 驗證使用多執行緒訪問 5 個 Socket 主機的情況

```java
public class MultiThreadTest {
    public static void main(String[] args) {
        System.out.println("MultiThreadTest starts at: " + new Date());
        ExecutorService es = Executors.newCachedThreadPool();
        String host = "localhost";
        
        Map<Integer, Future<String>> callables = new HashMap<>();
        // 依不同 port 號送出callable工作給執行緒池執行
        for (int prot = 10000; port < 10005; port++) {
            SockeClientCallable callable = new SocketClientCallable(host, port);
            Future<String> future = es.submit(callable); 
            // 定義future物件承接執行結果,但先不呼叫get()方法
            callables.put(port, future); 
            // 將future物件先存在callables，以Integer型別的port作為鍵值
        }
        
        // 關閉 ExecutorService
        es.shutdown();
        try {
            es.awaitTermination(5, TimeUnit.SECONDS);
        } catch (InterruptedException ex) {
            System.out.println("Stopped waiting ")
        }
        
        // 取回結果
        for (Integer port : callables.keySet()) {  // ln 26
            Future<String> future = callables.get(port);
            try {
                String feedback = future.get();
                System.out.println("Call " + host + ":" +
                    port + ", and get: " + feedback + " at " + new Date());
            } catch (ExecutionException | InterruptedException ex) {
                System.out.println("Error talking to " + host + ":" + port);
            }
        } // ln 26~: 在callables中找出所有Future物件，並呼叫get()取回結果
    }
}
```

...

## 使用 Fork-Join 框架同時執行多樣工作

### 平行處理的策略

+ 為了使多個CPU運算效能最佳化，可以讓程式同時執行工作，「平行處理（`parallelism`）」策略很重要

  1. 將工作分切成小段，各自完成後工作就可以解決

     + 稱為 `divide and conquer` 處理策略
     + 使用前需確認這些小段工作可以平行處理

  2. 分割時注意硬體效能問題，切割太細可能有反效果

     + 如果工作內容需要大量 CPU 計算而非 I/O 存取，需考慮 CPU 數量

       ```java
       int count = Runtime.getRuntime().availableProcessors();
       ```

  > 平行處理策略第一步：切割資料讓多執行緒可以平行執行
  >
  > 如何切割？
  >
  > 最理想情況：讓所有 CPU 可以充分被所有執行緒利用，直到工作結束

#### 使用「平均分配」方式發揮 CPU 計算量能

+ 讓每一個執行緒各自占用不同的 CPU 處理相同分量的工作，容易因為以下原因導致無法發揮硬體最高性能
  1. 每一個 CPU 可能效率不同
  2. 某些 CPU 可能也在執行其它程式
+ 讓十位同學打掃相同面積的空地，因為每個人效率不同，不會同時完成

#### 使用「工作竊取」方式發揮 CPU 計算量能

+ 稱為「工作竊取」的平行運算架構：平均切割工作，但數量遠多於執行緒個數
  1. 工作不會馬上完成，每一個執行緒會有很多待辦工作在自己的佇列
  2. 若某執行緒已完成自己的工作，可以竊取其它人的工作
  3. 合適的切割份量不容易達成
     + 切太多：切割本身即是負擔
     + 切太少：無法充分利用 CPU 資源
  4. 能者多勞理念的實踐 — Java 7 推出 `Fork-Join`框架

### 套用 `Fork-Join` 框架

#### 範例一：使用單一執行緒處理

```java
public class SingleThreadTest {
    public static void maind(String[] args) {
        int[] bigData = new int[1024 * 1024 * 256];
        for (int i = 0; i < bigData.length; i++) {
            bigData[i] = ThreadLocalRandom.current().nextInt();
        }
        int max = Integer.MIN_VALUE;
        for (int value : bigData) {
            if (value > max) {
                max = value;
            }
        }
        System.out.println("Found max value: " + max);
    }
}
```

#### 範例二：使用 `Fork-Join` 框架平行處理

+ `ExecutorService`使用 cached thread pool 或 fixed thread pool 執行 `Callable` 介面定義的工作內容

+ `Fork-Join` 框架則可以直接使用 `ExecutorService` 特化的子類別 `java.util.concurrent.ForkJoinPool` 執行抽象類別 `java.util.concurrent.ForkJoinTask<V>` 定義的工作內容

  1. `ForkJoinTask`物件代表需要執行的工作，可解釋為分進合擊

     + 一開始分頭進行，逐漸會師合併結果

  2. `ForkJoinTask`物件包含要處理的資料和處理方式，類似 `Runnable` 以及 `Callable`

  3. 巨大量工作可以由 Fork-Join pool 內少數執行緒處理

     + 每個執行緒會工作滿檔，可以發揮硬體的極致性能

  4. 開發者通常繼承 `ForkJoinTask`子類別，再實作 `compute()` 方法

     + `RecursiveAction` 的 *compute()* 方法沒有回傳結果

       ```java
       public abstract class RecursiveAction extends ForkJoinTask<Void> {
           protected abstract void compute();
       }
       ```

     + `RecursiveTask` 的 *compute()* 方法需要回傳結果
     
       ```java
       public abstract class RecursiveTask<V> extends ForkJoinTask<V> {
           protected abstract V compute();
       }
       ```

#### 方法 `compute()` 的處理邏輯

+ 建立類別 `FindMaxTask` 繼承 `RecursiveTask`，使用 `Fork-Join` 框架在 `1G` 的 `int` 陣列裡找出最大的數字 

  ```java
  protected RESULT compute() {    // pseudocode
      if (DATA_SMALL_ENOUGH) {
          PROCESS_DATA
          return RESULT;    
      } else {
          SPLIT_DATA_INTO_LEFT_AND_RIGHT_PARTS
          TASK t1 = new TASK(LEFT_DATA);
          // 建立一個稱為t1的ForkJoinTask處理左半部工作
          t1.fork();
          // 呼叫 t1.fork() 做非同步處理（分進、分頭行事）
          TASK t2 = new TASK(RIGHT_DATA);
          // 建立另一個稱為t2的ForkJoinTask處理右半部工作
          return COMBINE(t2.compute(), t1.join());
      }
  }
  ```

##### 圖解分進合擊

1. 原始資料處理量太大，分兩部分派給 `t1`、`t2`兩個 `ForkJoinTask`

   ![image-20221222102832964](https://i.imgur.com/kCNCU6i.png)

2. 呼叫方法：

   + **分進** : 呼叫 `t1.fork()` 方法調用一個執行緒處理資料

     **合擊** : 呼叫 `join()` 時取回結果
     → 目前累計一個計算結果未取回

   + 呼叫 `t2.compute()`時候，若資料處理量仍太大，持續進行切割工作

     ![image-20221222102858500](https://i.imgur.com/U9RbOV0.png)

3. 累計 2 個計算結果未取回

   ![image-20221222102925858](https://i.imgur.com/ySLWUCM.png)

4. 每次切割出去的左半部資料處理結果（累計 3 個未取回）

   + 且右側資料已經夠少，將呼叫 `t2.compute()`不再切割，直接計算得到一個結果

   ![image-20221222103013124](https://i.imgur.com/GIRMKac.png)

5. 開始將處理結果合併

   + 要取得左半部資料處理結果，需要呼叫 `t1.join()` 方法

     ![image-20221222104039589](https://i.imgur.com/hcA97yC.png)

6. 承上步驟，繼續合併結果

   ![image-20221222104024864](https://i.imgur.com/6VVcebU.png)

7. 完全合併後，得到結果

   ![image-20221222103938211](https://i.imgur.com/OgdMxoQ.png)

```java
class FindMaxTask extends RecursiveTask<Integer> {
	static int counter = 0;
	private static final long serialVersionUID = 1L;
	private final int threshold;
	private final int[] data;
	private int begin;
	private int end;

	public FindMaxTask(int[] data, int begin, int end, int threshold) {
		this.data = data;
		this.begin = begin;
		this.end = end;
		this.threshold = threshold;
	}

	@Override
	protected Integer compute() {
		if (end - begin < threshold) {
			System.out.printf("%02d", ++counter);
			System.out.print(": " + Thread.currentThread().getName());
			System.out.println(" |Range: " + begin + " ~ " + end);
			// 顯示目前是第N個進行的運算,由哪個執行緒執行,本例切為16等分,N<=16

			int max = Integer.MIN_VALUE;
			for (int i = begin; i <= end; i++) {
				int n = data[i];
				if (n > max) {
					max = n;
				}
			} // 計算資料陣列裡最大值
			return max;
		} else {
			int mid = (end - begin) / 2 + begin;
			FindMaxTask a1 = new FindMaxTask(data, begin, mid, threshold);
			a1.fork();
			FindMaxTask a2 = new FindMaxTask(data, mid + 1, end, threshold);
			return Math.max(a2.compute(), a1.join());
			// 不斷切割資料到門檻值
		}
	}
}

public class ForkJoinMultiThreadTest {
	public static void main(String[] args) {
		Date begin = new Date();
		// 製作資料陣列
		int[] bigData = new int[1024 * 1024 * 256]; // 1G
		for (int i = 0; i < bigData.length; i++) {
			bigData[i] = ThreadLocalRandom.current().nextInt();
		}
		/* 使用fork-join框架
		   建立Fork-Join框架的工作內容物件FindMaxTask，且資料陣列會切割為16等分，才進行運算 */
		FindMaxTask task = new FindMaxTask(bigData, 0, bigData.length - 1, bigData.length / 16);
		ForkJoinPool pool = new ForkJoinPool();
		Integer max = pool.invoke(task);
		// 使用forkJoinPool的invoke()方法傳入要執行的工作，該方法會呼叫FindMaxTask的compute()方法
		System.out.println("\nMax value found:" + max);
		// 計時
		long t = new Date().getTime() - begin.getTime();
		System.out.println("Complete task within " + t + " milliseconds");
	}
}
```

...

### `Fork-Join` 框架的使用建議

Fork-Join 框架幾個需要注意的地方：

1. 預設每核 CPU 會建立一個對應的執行緒執行工作
2. 使用時應該先排除 `I/O` 或是其它可能卡住執行緒工作的瓶頸
3. 了解自己的硬體
   + **單個 CPU 時，使用 Fork-Join 框架反而會比較慢**
   + 有些 CPU 只使用單核時，會比使用多核快，因此使用 Fork-Join 框架的成效感覺更少
4. 相較於單一執行緒的循序執行，**平行執行會有先切割工作的額外負擔，延長執行時間**

---

# 07 使用 JDBC 建立資料庫連線

## 了解 Database、DBMS 和 SQL

Java 裡保存資料 : 物件序列化技術、使用 I/O 將資料儲存於檔案中、儲存於資料庫中

#### 基本名詞介紹

1. **Database**：資料庫，放置電子資料的地方

2. **DBMS**（Database Management System）：

   + 為管理資料庫設計的電腦軟體系統，具儲存、擷取、安全保障、備份等基礎功能

   + 可以依據所支援的資料庫模型來作分類，例如關聯式、XML 等等

3. **SQL**（Structured Query Language）：結構化查詢語言，特殊目的的程式語言，用於存取資料庫中的資料

4. **JDBC**（Java Database Connectivity）：

   + Java 規範用戶端程式如何存取資料庫的應用程式介面
   + 提供了諸如查詢和更新資料庫中資料的方法

5. **Table**：

   + 資料庫中呈現資料的邏輯性作法

### 使用`SQL`存取資料庫

常用的兩大類 SQL（結構化查詢語言）

1. `DDL` (Data Definition Language)，常用於
   + 建立、修改、刪除資料表
   + 描述資料庫中的資料，包括欄位、型態、資料結構

2. `DML`(Data Manipulation Language)，用於
   + 操作資料表
   + 允許使用者存取或處理資料庫中的資料，內容包括
     + 擷取資料庫中的資訊、新增、刪除、更新資料庫中的資料

#### Create/insert 新增

```sql
CREATE TABLE EMPLOYEE (
    ID INTEGER NOT NULL,
    FIRSTNAME VARCHAR(40) NOT NULL,
    LASTNAME VARCHAR(40) NOT NULL,
    BIRTHDATE DATE,
    SALARY REAL,
    PRIMARY KEY (ID)
);
```

```sql
INSERT INTO EMPLOYEE VALUES (1, 'Troy', 'Hammer', '1966-03-31', 100000.00);
INSERT INTO EMPLOYEE VALUES (2, 'Michael', 'Walton', '1966-08-25', 90000.20);
```

#### Read/query/select 查詢

```SQL
SELECT * FROM EMPLOYEE;
SELECT ID, SALARY FROM EMPLOYEE WHERE ID = 1;
```

#### Update 修改

```SQL
UPDATE EMPLOYEE SET SALARY = 0 WHERE ID = 1; --將ID為1的員工資料的SALARY欄位更改為0
```

#### Delete 刪除

```SQL
DROP TABLE EMPLOYEE;  --刪除employee表格
```

```SQL
DELETE FROM EMPLOYEE WHERE ID = 1;  --刪除 ID為1 的員工資料
```

...

### Derby 資料庫介紹

+ Apache 軟體基金組織的 Derby - 純 Java 開發的關聯式資料庫

+ 原為 Cloudscape，為 IBM 所有但是 2004 被捐給了 Apache軟體基金組織

+ 特色

  1. 100% 以 Java 開發
  2. 輕量級，大小 35.5 MB
  3. 支援 JDBC 4.0 以上版本
  4. 支援大部分 ANSI SQL 92 標準
  5. 有 Table 和 View
  6. 支援 BLOB 和 CLOB 資料類型
  7. 支援預存程序（stored procedure）

+ Derby 運行模式

  1. **內嵌模式**（embedded mode）
     + Derby 資料庫與應用程式共用`JVM`，應用程式會在啟動和關閉時，分別自動啟動或停止資料庫
     + 使用「`derby.jar`」支援 Derby 資料庫引擎和嵌入式`JDBC`驅動程式
  2. **網路伺服器模式**（network server mode）
     + Derby 資料庫獨占一個 `JVM`，作為伺服器上的一個獨立程序（process）運行
       此模式下允許有多個應用程式來連線同一個 Derby 資料庫
     + 使用 `derbyclient.jar` 支援 Derby Network Server

+ Derby 主要指令檔案（放在 `db-derby-10.14.2.0-bin / bin` 目錄下）用途

  | 指令                       | 用途                                               |
  | -------------------------- | -------------------------------------------------- |
  | `startNetworkServer.bat`   | 可啟動網路伺服器的批次檔                           |
  | `stopNetworkServer.bat`    | 可停止網路伺服器的批次檔                           |
  | `ij.bat`                   | 互動式 `JDBC` 批次檔工具                           |
  | `dblook.bat`               | 可檢視資料庫全部或部分 `DDL` 的批次檔              |
  | `sysinfo.bat`              | 可顯示有關環境版本資訊的批次檔                     |
  | `NetworkServerControl.bat` | 可在 `NetworkServerControl API` 上執行指令的批次檔 |

  ...

### 操作 Derby 資料庫

####  啟動/關閉 Network Server

要使用 Derby 資料庫，必須先啟動 Network Server

1. *啟動*：點擊`startNetworkServer.bat`，以啟動 Network Server
2. *關閉*：`ctrl` + `C` 或直接關閉啟動時出現的視窗

#### 與 Network Server 互動

啟動 Network Server 之後，點擊 `ij.bat`，出現對話主控台（console），可輸入指令來和 Network Server 互動

指令包含建立 Derby 資料庫以及執行 DDL、DML、查詢等指令

1. 連線 Derby 資料庫

   ```
   connect                             //要求連線Derby資料庫
   'jdbc:derby://localhost:1527/myDB;  //表述資料庫連線位址, 使用JDBC URL, 含位址、port、
   create=true;                        //表示若資料庫不存在, 將自動建立, 建立在bin目錄下
   user=root;                          //提供建立連線的帳號
   password=sa';                       //提供建立連線的密碼
   ```

2. 連線到目標資料庫`myDB`後，使用`DDL`建立表格，並使用`DML`進行資料新刪改查

## Eclipse 連線並存取資料庫

+ 使用`ij.bat`雖可以連線並操作資料庫，但並非視窗畫面

+ Eclipse 提供 **模組DTP** 連線各式資料庫，雖然距離專業資料庫操作軟體還有距離，但已經很實用

### 連線資料庫

1. 開啟 `Database Development perspectives`，點選 `Database Development`

2. 視景變為 `Data Source Explorer` 以及出現 `SQL Results` 和 `Execution Plan` 頁籤

3. 右鍵 `Database Connections` 資料夾，`New ...` → `Derby` (next) 

4. `New Derby Connection Profile` 點選右側的 `New Driver Definition` 建立連線資料庫 Derby 的驅動程式

5. 在「Name/Type」頁籤裡，選擇第一筆「Derby Client JDBC Driver」

6. 切到「JAR List」頁籤，點預設的「`derbyclient.jar`」檔案，再點選「`Remove Jar/Zip`」移除之

7. 在此頁籤 `Add JAR/Zip`，彈出選擇連線 Derby 資料庫 JAR 檔的視窗

8. 在路徑 `db-derby-10.14.2.0-bin\lib` 內，找到檔案 `derbyclient.jar`，點擊**開啟**按鈕

9. 點選 OK，再鍵入要連線的 Derby 資料庫相關資訊

10. 先以 `startNetworkServer.bat` 啟動 Derby 資料庫，再點 `Test Connection` 測試與資料庫的連線

    ![image-20230118140615040](https://i.imgur.com/b7TgyDw.png)

### 存取資料表

1. 建立連線後，「Data Source Explorer」會出現目前連線的資料庫狀態與架構

2. 切到 Java EE 視景，開啟 `xxxTable.sql`，再切回 Database Development perspective

3. 在 `xxxTable.sql` 檔案上方 Connection Profile 設定列裡選擇要連線的資料庫名稱，再右鍵選執行方式 

   ```
   Execute All
   Execute Selected Text
   Execute Selected Text As One Statement
   Execute Current Text
   ```

4. 點選 `SQL Results` 頁籤，查看 Status | Operation，出現 Status Result 1 頁籤可看到以框格方式查看結果

...

## 使用 `JDBC`

### `JDBC API`  概觀

+ 主要由 1 個 class 和 3 個 interface 組成，除了 `DriverManager` 之外皆為介面	

  ![image-20230118151147434](https://i.imgur.com/GUGklX3.png)

  1. 使用 `DriverManager` 取得 `Connection`
  2. 使用 `Connection` 取得 `Statement`
  3. 使用 `Statement` 取得 `ResultSet`

+ `JDBC` 只定義抽象介面，連線資料庫的機制與實作類別由各別廠商提供

  + 例：Derby 資料庫需要的 JAR 檔案，亦即驅動程式為 `derbyclient.jar`，

    位於路徑 `db-derby-10.14.2.0-bin\lib`內

### 專案引用資料庫驅動函式庫

+ 要使用 Java 的 `JDBC` 程式直接連線 Derby 資料庫，需要在 Eclipse 專案裡引入該 JAR 檔
  1. 在專案中新建 Folder，慣例上命名為 `lib`
  2. 將 JAR 檔 `derbyclient.jar` 以拖拉方式複製到新建的 lib 目錄中
  3. `FileOperation` 選擇 `Copy files`
  4. 複製 JAR 檔至 Eclipse 專案步驟完成
  5. 右鍵專案節點，開啟 Project 頁籤的 `Properties` 選項
  6. 點視窗左側清單中的 `Java Build Path` → `Libraries` → `Classpath` → `Add JARs` → 選擇 `derbyclient.jar`
  7. 設定完後可看到 `derbyclient.jar` 出現在專案 Libraries 中，此為專案類別路徑 `Classpath`

### 認識 `JDBC` 驅動函式庫 JAR 的組成

用 7-zip 開啟 `derbyclient.jar`檔案之後，可看到內容基本結構

+ 逐層點開 `org`目錄後，可以看到許多 `*.class` 的類別檔 (`derbyclient.jar\org\apache\derby\jdbc`)
+ 因此除了把此 jar 檔放到專案類別路徑，還必須告訴 Java 如何在一堆類別檔案裡，找到主要入門/入口類別。
  方式在 `JDBC` 4.0 前後有很大不同

#### 1. `JDBC 4.0` 之前

+ 呼叫 `DriverManager.getConnection()`前，必須用字串明確指出驅動程式主類別為何，如範例行2

  + 以 Derby drivers 為例，如以下範例行3

+ 因為有可能類別字串寫錯，或忘記將 JAR 檔加入 Eclipse 專案類別路徑中而找不到，因此必須處理例外 `ClassNotfoundException`

  ```java
  try {
      //java.lang.Class.forName("{fully qualified path of the driver}");  //ln#2
      java.lang.Class.forName("org.apache.derby.jdbc.ClientDriver");      //ln#3
  } catch (ClassNotfoundException c) {
      // do something
  }
  ```

+ 或是在指令列裡指定驅動程式的主類別字串

  ```terminal
  java -djdbc.drivers={fully qualified path to the driver} {class to run}
  ```

#### 2. `JDBC 4.0` 之後

+ 驅動程式的主類別已註記再 JAR 的 `META_INF/services/java.sql.Driver` 檔案內，為 `org.apache.derby.jdbc.ClientDriver`

+ 因此在 `DriverManager.getConnection()` 時，就不需要在程式碼裡特別註記驅動程式主類別

  ![image-20230118162247728](https://i.imgur.com/kDzXmEA.png)

> 🍪☕ **Little Tips:** Derby 資料庫再版本 10.14.2.0 與 10.15.2.0 的差別
>
> | Derby 資料庫版本 10.14.2.0                                   | Derby 資料庫版本 10.15.2.0                                   |
> | ------------------------------------------------------------ | ------------------------------------------------------------ |
> | 支援 Java 8                                                  | 建議 Java 9 以上（使用模組化技術）                           |
> | 版本驅動程式入口類別<br />`org.apache.derby.jdbc.ClientDriver` | 後來是 <br />`org.apache.derby.client.ClientAutoloadedDriver` |
> | 設定 Eclipse 連線 Derby 資料庫的 `New Driver Definition` 時，<br/>第3個頁籤 Properties 之屬性「Driver Class」預設值是 `org.apache.derby.jdbc.ClientDriver` | 需要改為 `org.apache.derby.client.ClientAutoloadedDriver`    |
> | 加入驅動程式到 Java 專案時，<br />早期版本引用 `derbyclient.jar` | 需引用 `derbyclient.jar` 與 `derbyshared.jar`                |

...

### 開發 JDBC 程式

#### JDBC 程式的組成

開發 `JDBC` 程式的主要步驟

1. 指定 URL (Uniform Resource Locator)

   + URL 用來指出連線資料庫時使用的 driver 名稱/種類、資料庫位置（IP + Port）或其他建立連線時需要一併提供的屬性名稱與值

     ```
     jdbc:<driver>:[sub_protocol:] [databaseName] [;attribute=value]
     ```

   + 以 Derby 資料庫為例

     ```
     String url = "jdbc:derby://localhost:1527/EmployeeDB";
     ```

   + 以 Oracle 資料庫為例

     ```
     String url = "jdbc:oracle:thin:@//myhost:1521/orcl";
     ```

2. 使用 `DriverManager` 取得 `java.sql.Connection` 的物件，該物件將建立與資料庫的連線 (session)：

   ```java
   Connection con = DriverManager.getConnection(url, username, password);
   ```

3. 使用 `java.sql.Connection` 取得 `java.sql.Statement` 物件

   ```java
   Statement stmt = con.createStatement();
   ```

4. 使用 `java.sql.ResultSet` 取得 `jaav.sql.Statement` 執行 SQL 後的查詢結果

   ```java
   String query = "SELECT * FROM Employee";
   ResultSet rs = stmt.executeQuery(query);
   ```

#### `ResultSet`的特性與使用方式

`ResultSet`物件代表 `SQL` 查詢資料庫之後得到的結果，內部用游標（cursor）的移動代表目前所讀取的資料列

1. 游標最初指向第 0 筆資料

2. 呼叫 `ResultSet` 的 `next()` 方法可移動游標，取得指向某筆資料的游標

3. 若回傳 false，表示已無資料可以讀取

   ![image-20230119103204455](https://i.imgur.com/6Am5js2.png)

另外，`ResultSet` 物件具有多種屬性可以設定

| 分類依據    | 屬性                      | 用途                               |
| ----------- | ------------------------- | ---------------------------------- |
| Concurrency | `CONCUR_READ_ONLY`        | 指向資料是唯讀                     |
| Concurrency | `CONCUR_UPDATABLE`        | 指向資料可修改                     |
| Type        | `TYPE_FORWARD_ONLY`       | 游標只能往前                       |
| Type        | `TYPE_SCROLL_INSENSITIVE` | 游標可往前往後，無法感知資料被修改 |
| Type        | `TYPE_SCROLL_SENSITIVE`   | 游標可往前往後，可以感知資料被修改 |

必須在建立 Statement 物件時設定 `ResultSet` 屬性

```java
Statement stmt = con.createStatement(ResultSet.TYPE_SCROLL_INSENSITIVE, ResultSet.CONCUR_UPDATABLE);
ResultSet rs = stmt.executeQuery("SELECT a, b FROM TABLE2");
```

設定後的實際情況，必須視該種資料庫的廠商是否實作該屬性而定

`ResultSet` 可以使用回傳各種型態的 `getter` 方法，來取得每筆一資料的每個欄位內容

#### `JDBC`程式完整範例

結合使用 `DriverManager`、`Connection`、`Statement` 與 `ResultSet` 的完整範例如下，可輸出資料庫 myDB 的資料表 Employee 的所有資料

```java
public static void main(String[] args) throws ClassNotFoundException {
	String url = "jdbc:derby://localhost:1527/myDB";
	String username = "root";
	String password = "sa";
	String query = "SELECT * FROM Employee";
	try (Connection con = DriverManager.getConnection(url, username, password);
			Statement stmt = con.createStatement();
			ResultSet rs = stmt.executeQuery(query)) {
		while (rs.next()) {
			int empID = rs.getInt("ID");
			String first = rs.getString("FirstName");
			String last = rs.getString("LastName");
			Date birthDate = rs.getDate("BirthDate");
			float salary = rs.getFloat("Salary");
			System.out.println(empID + "\t" + first + "\t" + last 
                               + "\t" + birthDate + "\t" + salary);
		}
	} catch (SQLException e) {
		System.out.println("SQL Exception: " + e);
	}
}
```

### 結束 `JDBC` 相關物件的使用

+ `JDBC` 存取資料庫的主要物件，`Connection`、`Statement`、`ResultSet` 都實作了 `java.lang.AutoCloseable` 介面

+ 皆屬於外部資源，使用後都必須呼叫 `close()` 方法予以關閉。如下原則

  1. 關閉 Connection 物件，會自動關閉相關 Statement 物件；

     + 關閉 Statement 物件，也會自動關閉相關 `ResultSet` 物件；
     + 但 `ResultSet` 對應的相關資源並未被自動關閉或釋出，必須等 Java 啟動 `GC` 機制
     + 只有明確呼叫 `ResultSet` 的 `close()` 方法，才能馬上釋放相關資源

  2. 如果使用相同 `Statement` 物件重新執行查詢，則原先已開啟的 `ResultSet` 將自動關閉，再使用該 `ResultSet` 就會出錯

  3. 應該明確呼叫 `Connection`、`Statement` 和 `ResultSet` 的 `close()` 方法，或利用 **try-with-resource** 敘述

     + 關閉資源的順序和開啟時順序相反

  4. 只有在 **try-with-resource** 區塊裡明確宣告的物件，才會被自動關閉

     + 以下作法只有 `ResultSet` 物件會被自動關閉

       ```java
       try ( ResultSet rs = DriverManager
            			   .getConnection(url, username, password)
           			   .createStatement()
           			   .executeQuery(query)) { 
           // ...
       }
       ```

...

### 開發可攜式的 `JDBC` 程式碼

+ `JDBC` 相關 `API` 設計目的是讓 Java 程式碼可以依賴於 `JDBC` 建立的抽象層

+ 而不是和底層資料庫綁定太深，太依賴資料庫

+ 未來如果要抽換資料庫，可以影響最小

+ 系統架構分層（insulating layer）概念：
  `Connection`、`Statement`、`ResultSet` 都是介面，由資料庫廠商實作
  
+ 美國國家標準學會（American National Standards Institute, ANSI）定義的 **SQL-92 Entry-level specification** 
  + 提倡所有資料庫廠商都能支援 **SQL-92** 的標準查詢語法，盡量讓相同語法可以在不同資料庫之間使用

+ 可以用 `DatabaseMetaData` 介面的 `supportsANSI92EntryLevelSQL()` 方法回傳 true 或 false，確認使用中的資料庫有沒有支援 SQL-92 語法

  ```java
  private static void showDatabaseMetaData(Connection con) throws SQLException {
      DatabaseMetaData dbm = con.getMetaData();
      System.out.println("Support for entry-level SQL-92 standard: " 
                         + dbm.supportsANSI92EntryLevelSQL());
  }
  ```

+ 使用資料庫原生 native SQL 的程式碼，將造成日後轉換資料庫的困難

  + MS SQL(TSQL) : `SELECT TOP 10 * FROM [some_table]`
  + Oracle (PLSQL) : `SELECT * FROM [some_table] WHERE ROWNUM <= 10`


### 使用 `java.sql.SQLException` 類別

+ `SQLException`類別和一般 Exception 不同，可得到更多存取資料庫產生的各種錯誤訊息

  ```java
  catch (SQLException ex) {
      while (ex != null) {
          System.out.println("SQLState: " + ex.getSQLState());
          System.out.println("Error Code in DB: " + ex.getErrorCode());
          System.out.println("Message: " + ex.getMessage());
          Throwable throwable = ex.getCause();
          while (throwable != null) {
              System.out.println("Cause: " + throwable);
              throwable = throwable.getCause();
          }
          ex = ex.getNextException();
      }
  }
  ```

### Statement 介面與 `SQL` 敘述的執行

+ 執行 SQL 敘述 statement 時，必須有 Statement 的物件。

+ 即 Statement 介面是 SQL statement 的包覆類別（wrapper class）

  ```java
  Statement statement = connection.createStatement();
  String sqlStatement = "SELECT * ...";
  ResultSet resultSet = statement.executeQuery(sqlStatement);
  ```

+ 根據 SQL statement 不同種類，有不同執行方式

  | SQL敘述種類                    | 方法                 | 回傳                               |
  | ------------------------------ | -------------------- | ---------------------------------- |
  | SELECT                         | `executeQuery(sql)`  | `ResultSet`                        |
  | INSERT、UPDATE、DELETE、DDL... | `executeUpdate(sql)` | int<br />表示影響的資料筆數        |
  | 任何 SQL 指令                  | `execute(sql)`       | boolean<br />表示是否有`ResultSet` |

### 使用 `ResultSetMetaData` 介面

使用 `ResultSetMetaData` 介面取得 `ResultSet` 的

1. 欄位數量 - 使用 `getColumnCount()` 方法

2. 欄位名稱 - 使用 `getColumnName()` 方法

3. 欄位型態 - 使用 `getColumnTypeName()` 方法

   + 須注意指定欄位的 **由「1」起算**，非「0」

     ```java
     private static void showRsMetaData(ResultSet rs) throws SQLException {
         int numCols = rs.getMetaData().getColumnCount();
         String[] colNames = new String[numCols];
         String[] colTypes = new String[numCols];
         for (int i = 0; i < numCols; i++) {
             colNames[i] = rs.getMetaData().getColumnName(i + 1);
             colTypes[i] = rs.getMetaData().getColumnTypeName(i + 1);
             // 資料表欄位位置由1起算，但迴圈i由0開始，所以必須是 i+1
         }
     }
     ```

...

### 取得查詢結果的資料筆數

+ 可以利用游標（cursor）前後移動，取得查詢結果的資料筆數，在這之前須先設定屬性

  ```java
  Statement statement = connection.createStatement(ResultSet.TYPE_SCROLL_INSENSITIVE, 
                                                   ResultSet.CONCUR_READ_ONLY);
  ```

+ **方法一**：執行查詢取得`ResultSet`物件後，可以由以下範例取得結果的資料筆數

  ```java
  private static int rowCountByCursor(ResultSet rs) throws SQLException {
      int rowCount = 0;
      int currRow = rs.getRow(); //紀錄目前 cursor 位置
      if (!rs.last())
          return -1;  //使用last()方法將游標移到最後一筆資料,如發現沒資料時回傳false
      rowCount = rs.getRow();  // 因為游標已移動到最後一筆資料,該所在位置即為資料筆數
      if (currRow == 0)        // ln#7
          rs.beforeFirst();
      else
          rs.absolute(currRow); // ln#7 - 10 將 cursor 移回原先位置
      return rowCount;
  }
  ```

+ **方法二**：使用 SQL 語法的 `count()` 函數，直接取得滿足條件的資料筆數

  + 不用調整 cursor，用預設的 `Statement` 物件即可

    ```java
    private static int rowCountBySQL(Connection con, String money) throws SQLException {
        String query = "SELECT COUNT(*) FROM Employee WHERE Salary > " + money;
        try (Statement stmt = con.createStatement(); 
                ResultSet rs = stmt.executeQuery(query)) {
            rs.next();
            /* 每個Statement物件只能執行一次SQL，不能重複使用
               所以傳入Connection物件，用它重新產生Statement物件，再執行SQL */
            int count = rs.getInt(1);
            return count;
        }
    }
    ```

### 控制 `ResultSet` 每次由資料庫取回的筆數

+ 使用 Statement 物件執行 Query 後取得 `ResultSet`物件，並不是由資料庫將資料一次全部拿回來

  ```java
  ResultSet rs = stmt.executeQuery(query);
  while (rs.next()) {
      /* Java 會從資料庫中 "每次抓回一定的筆數" 才進行迴圈，減少對DB的頻繁I/O
         筆數預設由 JDBC driver 驅動程式控制 */
  }
  ```

+ 要自行控制的作法 `rs.setFetchSize(25);`

  + 每次只拿 25 筆資料，第 26 筆時再連線資料庫撈取下一批 25 筆資料
  + 避免一次取太多資料而影響 JVM


### 使用 `PreparedStatement`

先來看 Oracle SQL 使用**繫結變數（bind variables）**的作法，以**?**取代欄位內容，先送SQL，後送欄位內容，可避免該SQL敘述因為每次欄位內容被反覆編譯，造成資料庫主機負載

```sql
SELECT * FROM [Employee] WHERE [Salary] > ?;
```

這種作法在 JDBC 則稱為使用 `PreparedStatement` 介面，該介面繼承 `Statement` 介面，使預先編譯 pre-compiled SQL 可以再和傳入的參數配合

```java
private static void runPreparedStatement(Connection con, double value) throws SQLException {
    String query = "SELECT * FROM Employee WHERE Salary > ? ";
    PreparedStatement pStmt = con.prepareStatement(query);  //
    pStmt.setDouble(1, value);
    ResultSet rs = pStmt.executeQuery();
    printResultSet(rs);
}
```

Note:

1. SQL 裡的每一個 **?** 都必須有相應數值
2. 藉由 `setXXX(index, value)` 的方法帶入 value，index 從 1 起算，配合 **?** 的出現順序
3. 可避免每次執行 SQL 時候，DB重新編譯 SQL 耗費的資源

#### SQL injection

使用 `PreparedStatement` 另一個原因是可以避免 *SQL injection* 網路駭客攻擊

```java
private static void runSqlInjection(Connection con, String value) throws SQLException {
    String query = "SELECT * FROM Employee WHERE Salary > " + value;
    Statement stmt = con.createStatement(); // 容易被攻擊
    ResultSet rs = stmt.executeQuery(query);
    printResultSet(rs);
}
```

與前一個方法（傳入 **double** value）相比，此方法（傳入 **String** value）如果有駭客剛好瞭解 Employee 的資料表結構，或故意在傳入字串後面加上 `or 1=1`，會帶出 table Employee 內所有資料，讓公司營業機密或個資外洩

- `SELECT * FROM Employee WHERE Salary > 100;`
- `SELECT * FROM Employee WHERE Salary > 100 or 1=1;`

## 使用 `JDBC` 進行交易

### 何謂資料庫交易

+ 將多個對資料庫的存取行為視為同一個，而且同進退：一起發生（commit）或一起未發生（rollback）

+ 一個交易裡的行為（query, delete, insert, update）可以跨資料庫
+ 交易的 ACID 特色
  1. Atomicity 原子性： 一個交易中的所有行為一起完成，或一起未完成（回復至未進行交易的狀態）
  2. Consistency 一致性：系統原來一致性的狀態 ---[交易]---> 另一個一致性的狀態
  3. Isolation 獨立性：兩個同時發生的交易，彼此互相不影響
  4. Durability 持久性：已完成的交易繼續保持，如果系統毀損可以用交易紀錄日誌 transaction log 還原
+ commit (確認交易) 、rollback (取消交易)

### 使用 JDBC 的交易 

+ 當建立 connection 物件時，預設為「auto commit」模式，此時單一 SQL 會被視為獨立交易，完成後自動 commit

+ 如果要將兩個以上的 SQL 作成交易群組，必須先關 auto commit 模式

  ```java
  con.setAutoCommit(false);
  ```

+ 接著完成交易時必須呼叫方法

  ```java
  con.commit();
  ```

+ 也可以取消交易 

  ```java
  con.rollback();
  ```

關於啟動交易，JDBC 沒有明確的方法，依據 JDBC JSR (221) 綱要：

1. 以關閉 auto commit 模式的時候開始，接下來的所有 SQL 都算成同一個交易，一直到 commit 或者 rollback 被執行
2. 如果交易進行中 auto commit 模式被改變，則交易將自動 commit

...

## 使用 JDBC 4.1 的 RowSetProvider 和 RowSetFactory

Java 7 的新版 `RowSet 1.1` 使用 `javax.sql.rowset.RowSetProvider`取得 `RowSetFactory` 物件，預設實作 `com.sun.rowset.RowSetFactoryImpl`

```java
RowSetFactory myRowSetFactory = RowSetProvider.newFactory();
```

回傳的 `RowSetFactory` 則用來建立 `RowSet 1.1` 中的 `RowSet` 物件，常見如下：

| 介面             | 功能                                                         |
| ---------------- | ------------------------------------------------------------ |
| `CachedRowSet`   | 可以將資料庫取得的資料儲存在記憶體中，避免經常連線           |
| `FilteredRowSet` | 繼承`CachedRowSet`，可以有過濾資料的功能                     |
| `JdbcRowSet`     | 是 `ResultSet` 的 wrapper 物件，讓 `ResultSet` 行為像 `JavaBean`<br />也可以和資料庫保持連線狀態 |
| `JoinRowSet`     | 可以把兩個不同 `RowSet` 合併成一個 `JoinRowSet`，功能像SQL表格的 join |
| `WebRowSet`      | 支援將 `RowSet` 以標準的 XML 格式表現                        |

### `JdbcRowSet`介面示範

```java
public class JdbcRowSetTest {
    public static void main(String[] args) throws SQLException {
        String url = "jdbc:derby://localhost:1527/myDB";
        String username = "root";
        String password = "sa";
        RowSetFactory myRowSetFactory =
            RowSetProvider.newFactory();
        try (JdbcRowSet jdbcRs = myRowSetFactory.createJdbcRowSet()) {
            jdbcRs.setUrl(url);
            jdbcRs.setUsername(username);
            jdbcRs.setPassword(password);
            jdbcRs.setCommand("SELECT * FROM Employee");
            jdbcRs.execute();
            while (jdbcRs.next()) {
                int empID = jdbcRs.getInt("ID");
                String first = jdbcRs.getString("FirstName");
                String last = jdbcRs.getString("LastName");
                Date birthDate = jdbcRs.getDate("BirthDate");
                float salary = jdbcRs.getFloat("Salary");
                System.out.println(
                    "ID: " + empID + "\t" +
                    "Employee Name: " + first + " " + last + "\t" +
                    "Birth Date: " + birthDate + "\t" +
                    "Salary: " + salary);
            }
        }
    }
}
```

...

## 回顧 DAO 設計模式

幫 `EmployeeDAO` 介面增加實作類別 `EmployeeDAOJDBCImpl`

+ `EmployeeDAO` *interface*
  + `addEmployeeDAO()` : void
  + `update(Employee e)` : void
  + `delete(int id)` : void
  + `findById(int id)` : Employee
  + `getAllEmployees()` : Employee[]
+ `EmployeeDAOMemoryImpl` implements `EmployeeDAO`
+ `EmployeeDAOJDBCImpl` implements `EmployeeDAO`
+ `EmployeeDAOFileImpl` implements `EmployeeDAO`

---

# 08 Java 的區域化 *Localization*

## 了解 Java 的軟體區域化作法

Java 的軟體區域化 localization 方式：藉由增加和「特定地區 / 地域」相關的元件和翻譯文字，使軟體可呈現「特定地區 / 地域」的語言文字、日期、數字、幣別等與文化相關的特殊格式

Java 滿足軟體區域化和支援多國語系的需要，不是藉由 (複製 -> 修改文字呈現相關 codes) 的方式，這會讓程式碼越來越多份，違背 DRY 法則。

Java 作法：事先準備多份各國語系的文字檔，依需求載入 JVM，再嵌入（plug-in）文字呈現的畫面或功能中。需要三個核心元件 -

1. `Locale` 類別 - 代表特定地區 / 地域
2. 多國語系文字檔（i.e. 資源綁定檔案）- 存放各國文字，檔案各自獨立
3. `ResourceBundle`類 - 用來對應多國語系文字檔，建立物件時，檔案內容自動載入到物件裡

### 使用 Locale 類別

Java 用 Locale（場所、場域）不用國別決定不同語言，主要因為有些國家幅員廣大（可能有多個語言），所以用 Locale 類代表特定語言和國家的組合

1. **語言**
   + 使用 alpha-2 或 alpha-3 ISO 639 編碼
   + 小寫 (e.g. de for German，en for English，fr for French，zh for Chinese)
2. **國家**
   + 使用 ISO 3166 alpha-2 country 編碼或 UN M.49 numeric area 編碼
   + 大寫 (e.g. DE for Germany, US for United States, FR for France, CN for China)

常見建構 Locale 物件的方式：

1. 使用 Locale 類別已經定義的常數

   ```java
   Locale twLocale1 = Locale.TAIWAN;
   ```

2. 提供 language 和 country 代碼字串作為建構子的輸入參數

   ```java
   Locale twLocale2 = new Locale("zh", "TW");
   ```

   

### 建立多國語系文字檔

多國語系文字檔（資源綁定檔案_resource bundle files）的製作方式

1. 以 `.properties` 作為副檔名
2. 針對程式需要支援的每種語系建立獨立檔案
   + 每個檔案的主要檔名相同，再加上*語言* 和 *國家* 代碼做區隔
   + 即需要對系統會用到的 locale 建立對應的檔案
   + 如檔案上都沒有 *語言* 和 *國家* 代碼，則為預設檔（程式找不到對應多國語系文字檔的最後防線）
3. 檔案內含許多成對的 key、value
   + 每個檔案的 key 數量、內容皆一致，會被使用於程式碼中
   + value - 各 locale 的當地文字

主要檔名為 `MessageBundle`，所以預設檔案為 `MessageBundle.properties`，再依需求建立文字檔，規則

```note
MessageBundle_xx_YY.properties
/* xx：語言代碼，小寫
   YY：國家代碼，大寫 */
```

+ `MessageBundle.properties`

  ```properties
  menu1 = Set to English
  menu2 = Set to French
  menu3 = Set to Chinese
  menu4 = Set to Russian
  menu5 = Show the Date
  menu6 = Show the money
  menuq = Enter q to quit
  ```

+ `MessageBundle_fr_FR.properties`

  ```properties
  menu1 = Régler à l'anglais 
  menu2 = Régler au français 
  menu3 = Réglez chinoise 
  menu4 = Définir pour la Russie 
  menu5 = Afficher la date 
  menu6 = Montrez-moi l'argent! 
  menuq = Saisissez q pour quitter 

+ 以上檔案可以放在 Eclipse 的 src 路徑底下

  ![image-20230204002143295](https://i.imgur.com/5uE1dOx.png)

### 使用 `ResourceBundle` 類別

+ 使用 `ResourceBundle`（資源綁定）綁定像是多國語系文字檔的資源
+ 此類別又稱為 **資源綁定檔案（resource bundle files）**
+ 資源還可以是 `.class`，只是一般比較少使用

使用 `ResourceBundle` 類別建立物件時，必須提供

1. 多國語系文字檔的主要檔案名稱，例如 `MessageBundle`

2. 物件 `Locale`：代表某一個「語言」和「國家」的組合，例如 `zh` 和 `TW`

   ```java
   Locale twLocale = new Locale("zh", "TW");
   ResourceBundle bundle = ResourceBundle.getBundle("MessageBundle", twLocale);
   ```



## 使用 `DateFormat` 類別提供日期的區域化顯示

Java 使用 `DateFormat` 類別搭配 `Locale` 物件，以提供日期的區域化顯示，如下步驟：

1. 取得 `java.util.Date` 日期物件
2. 搭配 `Locale` 取得 `DateFormat` 物件，並挑選格式
3. 呼叫 `DateFormat` 物件的 `format()` 方法，並傳入 `java.util.Date` 日期物件

### 日期格式選項

日期格式選項可以是：

1. 由類別 `DateFormat` 提供的常數指定
   + **SHORT**：「12.13.52」、「3:30 pm」
   + **MEDIUM**：「Jan 12, 1952」
   + **LONG**：「January 12, 1952」、「3:30:32 pm」
   + **FULL**：「Tuesday, April 12, 1952 AD」、「3:30:42 pm PST」
2. 類別 `DateFormat` 的子類別 `SimpleDateFormat` 指定特定格式
   + `yyyy/MM/dd HH:mm:ss`
   + `yyyy/MMM/dd HH:mm:ss`
   + `yyyy/MMMM/dd HH:mm:ss`

## 使用 `NumberFormat` 類別提供幣別區域化顯示

使用 `NumberFormat` 類別搭配 `Locale` 物件，提供幣別的區域化顯示，如下步驟：

1. `Locale`物件傳入 `NumberFormat` 類別的 static 工廠方法中，以取得幣別物件實例
2. 呼叫幣別物件實例的 `format()` 方法，並傳入數字金額
   +  數字金額可以為基本型別或者其包覆類別

```java
public static void main(String[] args) {
    NumberFormat nf;
    
    nf = NumberFormat.getCurrencyInstance(Locale.US);
    System.out.println("Locale.US: " + nf.format(1000));
    
    nf = NumberFormat.getCurrencyInstance(Locale.TAIWAN);
    System.out.println("Locale.TAIWAN: " + nf.format(1000.00));
    
    nf = NumberFormat.getCurrencyInstance(Locale.JAPAN);
    System.out.println("Locale.JAPAN: " + nf.format(1000.00));
}
```



---

# 09 Lambda 表示式的應用

## 使用 Lambda 表示式

### 匿名類別與功能性介面回顧

匿名內部類別（anonymous Inner Class）使用時機：

1. 只使用一次，所以不需要特別定義類別，可減少程式碼撰寫
2. 希望把相關程式碼擺在同一地方
3. 增加封裝程度
4. 提高程式碼可讀性

Java 8 功能性介面（functional interface）的特色：

1. 只有一個抽象方法的介面
2. 該介面可以標註 `@FunctionalInterface`

功能性介面範例

```java
@FunctionalInterface
public interface StringAnalyzer {
    public boolean analyze(String target, String keyStr);
}
```

實作以上介面的類別

```java
public class ContainsAnalyzer implements StringAnalyzer {
    public boolean analyze(String target, String keyStr) {
        return terget.contains(keyStr);
    }
}
```

分析目標字串裡面是否含有關鍵字串，在 `StringAnalyzerTest` 建立以下方法

輸入字串陣列、關鍵字串和實作 `StringAnalyzer` 的子類別

```java
static void searchArr(String[] strArr, String keyStr, StringAnalyzer analyzer) {
    for (String str : strArr) {
        if (analyzer.analyze(str, keyStr)) {
            System.out.println(str);
        }
    }
}
```

也可以直接建立單一類別處理，不實作介面，拋棄多型

```java
public class StringAnalyzeTool {
    public boolean contains(String target, String searchStr) {
        return target.contains(searchStr);
    }
}
```

+ 但是如果未來需要有其他方法，像是「是否由某字串開頭（startsWith）」、「是否由某字串結尾（endsWith）」，則使用單一類別就必須不斷改程式以增加其他類似方法→違反 OCP法則、程式歡迎擴充、拒絕修改 *open for extension, close for modification*

比較是否使用「匿名內部類」的差異

```java
static void test1() {
    String[] strArr = { "", "", "" };
    searchArr(strArr, "b", new ContainsAnalyzer()
             );
}
static void test2() {
    String[] strArr = { "abc", "bcd", "efg" };
    searchArr(strArr, "b",
             new StringAnalyzer() {
                 public boolean analyze(String target, String keyStr) {
                     return target.contains(keyStr);
                 }
             }
    );
}
```



### Lambda 表示式語法回顧





## 使用內建的功能性介面

Java 8 導入功能性介面的使用

1. 只能有一個抽象方法需要實作
2. Lambda 表示式必須搭配這類型的介面

因為功能性介面只有一個抽象方法，能夠預期它的使用方式。Java 8 在 `java.util.function` 下內建許多功能性介面，可直接使用。基礎如下四種：

1. **評斷型（predicate）**：使用泛型傳入參數，回傳 boolean
2. **消費型（consumer）**：使用泛型傳入參數，沒有回傳（void）
3. **功能型（function）**：將傳入的參數由 T 型別轉換成 U 型別
4. **供應型（supplier）**：如同工廠方法，提供 T 型別的實例 / 物件

### 評斷型功能性介面 Predicate

代表介面是 Predicate

```java
package java.util.function;
public interface Predicate<T> {
    public boolean test(T t);
}
```

`Predicate<T>`：需要提供一個型別 T 滿足泛型，唯一方法 `test` 使用T型別作為參數

方法內容通常和 **測試T型別的某些欄位或方法** 有關，結果回傳 true 或 false

```JAVA
Predicate<Person> olderThan23 = p -> p.getAge() >= 23;
for (Person p : Person.createList()) {
    if (olderThan23.test(p)) {
        System.out.println(p);
    }
}
```

以上程式碼和以下的匿名類別是一樣的意思

```java
Predicate<Person> olderThan23 = new Predicate<Person>() {
    public boolean test(Person p) {
        return p.getAge() >= 23;
    }
};
```

### 消費型功能性介面 Consumer

代表介面是 Consumer

```java
package java.util.function;
public interface Consumer<T> {
    public void accept(T t);
}
```

`Consumer<T>`：提供一個 T 型別滿足泛型，並使用 T 型別作為參數，內容通常和 T 型別的某欄位/方法有關

```java
Consumer<Person> printPerson = p -> p.printPerson();
for (Person p : Person.createList()) {
    printPerson.accept(p);
}
```

以上程式碼和以下的匿名類別是一樣的意思

```java
Consumer<Person> printPerson = new Consumer<Person>() {
    public void accept(Person t) {
        p -> p.printPerson();
    }
};
```

### 功能型功能性介面 Function

代表介面是 Function

```java
package java.util.function;
public interface Function<T, R> {
    public R apply(T t);
}
```

`Function<T, R>`：提供兩個型別 T、R 滿足泛型。唯一方法傳入 T 參數，回傳 R 型別 (R for result or reply)

```java
public class FunctionDemo (
    public static void main(String[] args) {
        Function<Person, String> getNameFromPerson = p -> p.getName();
        for (Person p : Person.createList()) {
            System.out.println(getNameFromPerson.apply(p));
        }
    }
)
```

以上程式碼和以下的匿名類別是一樣的意思

```java
Function<Person, String> getNameFromPerson = 
    new Function<Person, String>() {
        public String apply(Person p) {
            return p.getName();
        }
};
```

### 供應型功能介面 Supplier

代表介面是 Supplier

```java
package java.util.function;
public interface Supplier<T> {
    public T get();
}
```

`Supplier<T>`：提供型別 T 滿足泛型。唯一方法沒有傳入參數，回傳 T 型別

```java
Supplier<Person> personSupplier = () -> new person("Newt", "Scamander@fantastic.beasts.org", 21);
System.out.println(personSupplier.get());
```

以上代碼相當於下面的匿名內部類

```java
Supplier<Person> personSupplier = new Supplier<Person>() {
    public Person get() {
        return new Person("Newt", "Scamander@fantastic.beasts.org", 21);
    }
};
```

...

## 在泛型內使用萬用字元

泛型使用萬用字元 `wildcards`的符號為 `?`

1. `<?>` 
   + 可以是任何型別，沒有上/下限
2. `<? extends T>` 
   + 泛型型別必須是 **型別T** 或者 **T的子型別**
   + **以型別T為上邊界**，但沒有下邊界
3. `<? super T>`
   + 泛型型別必須是 **型別T** 或者 **T的父型別**
   + **以型別T為下邊界**，但沒有上邊界

### 在泛型裡使用多型

![image-20230214204322390](https://i.imgur.com/tUxMGOF.png)

```java
// ✅
List<A> listA = new ArrayList<A>();
List<B> listB = new ArrayList<B>();
```

```java
// ❌
listA = listB; // fail to compile
List<A> listA = new ArrayList<B>(); // fail to compile
```

> 將宣告為 `List<A>` 的變數，指向 `ArrayList<B>` 的物件實例
>
> (宣告: List內可放A、B、C物件) = (物件實例: 實際只能放入B的物件)
>
> ...

```java
// ❌
listB = listA; // fail to compile
List<B> listB = new ArrayList<A>(); // fail to compile
```

> 將宣告為 `List<B>` 的變數，指向 `ArrayList<A>` 的物件實例
>
> (只能拿出B的物件) = (物件實例: 可能拿出A、B、C的物件)
>
> ...

#### 如果要在泛型使用繼承或多型，必須用 `<? super T>` 或 `<? extends T>`

```java
List<?> listUnknown0 = new ArrayList<A>();
// List裡可以是任何物件

List<? extends A> listUnknown1 = new ArrayList<A>();
List<? extends A> listUnkown1Of1 = new ArrayList<B>();
// 必須是 A 或者 A 的子類別

List<? super A> listUnknown2 = new ArrayList<A>();
List<? super A> listUnknown2Of1 = new ArrayList<AA>();
List<? super A> listUnknown2Of2 = new ArrayList<B>();
// 必須是 A 或者 A 的父類別
```

 

### 存取使用 `<?>` 的泛型的集合物件

1. 使用`<?>`時的注意事項

   + 不允許使用`add()` 方法加入物件

   + 可以使用 Object class 作為參照型別

     ```java
     private void processElements(List<?> elements) {
         // elements.add(new A());
         // elements.add(new B());
         // elements.add(new C());
         // elements.add(new Object());
         for (Object o : elements) {
             System.out.println(o);
         }
     }
     
     void testProcessElements() {
         List<A> listA = new ArrayLIst<A>();
         processElements(listA);
         List<B> listB = new ArrayLIst<B>();
         processElements(listB);
         List<C> listC = new ArrayLIst<C>();
         processElements(listC);
     }
     ```

2. 使用 `<? extends A>` 時的注意事項

   + 表示傳入的物件泛型必須是A或A的子類別

   + 因為無交集，不允許使用 `add()` 方法放入物件

   + 因為 List<?> 內物件一定是A或者A的子類別，可以用A類別作為參照型別

     ```java
     private void processExtendsElements(List<? extends A> list) {
         // elements.add(new A());
         // elements.add(new B());
         // elements.add(new C());
         // elements.add(new Object());
             for (A a : elements) {
             System.out.println(o);
         }
     }
     
     void testProcessExtendsElements() {
         List<A> listA = new ArrayLIst<A>();
         processExtendsElements(listA);
         List<B> listB = new ArrayLIst<B>();
         processExtendsElements(listB);
         List<C> listC = new ArrayLIst<C>();
         processExtendsElements(listC);
     }
     ```

3. 使用 `<? super A>` 時的注意事項

   + 表示傳入的物件泛型必須是A或A的父類別

     + 傳入實例 `ArrayList<AA>`：可以放物件A、B、C
     + 傳入實例 `ArrayList<A>`：可以放物件A、B、C

   + 允許使用 `add()` 方法加入 A 或 A的子類別

   + 只能用Object類別作為物件參考，因為可能是 A、AA、其他A的父類別、或Object類

     ```java
     private void insertElements(List<? super A> list) {
         list.add(new A());
         list.add(new B());
         list.add(new C());
         /*
         for (A a : list) {
             System.out.println(a.getClass().getName());
         }
         */
         Object object = list.get(0);
     }
     
     void testInsertElements() {
         List<A> listA = new ArrayLIst<A>();
         insertElements(listA);
         List<AA> listAA = new ArrayLIst<AA>();
         insertElements(listAA);
         List<Object> listObject = new ArrayLIst<Object>();
         insertElements(listObject);
     }
     ```

| Generics | `<?>`                  | `<? extends T>`         | `<? super T>`                   |
| -------- | ---------------------- | ----------------------- | ------------------------------- |
| 檢視成員 | 只能用Object類檢視成員 | 可用T或其父類別檢視成員 | 只能用Object型別檢視成員        |
| 增加成員 | 無法                   | 無法                    | 可以增加T或者其**子類別**的成員 |



## 使用其它內建功能性介面

常見以下三類

#### 1. 四個基礎功能性介面的基本型別變形

方法傳入參數或回傳物件的其中一個或全部改成基本型別，例如 `DoubleFunction`、`ToDoubleFunction`

#### 2. Binary（二運算元相關）及其基本型別變形

將方法參數由一個增加為兩個，例如`BiPredicate`

#### 3. Unary（單一運算元相關）及其基本型別變形

繼承介面`Function<T, T>`，但泛型數量由兩個降成一個

`UnaryOperator` - 其方法傳入和回傳的型別一致

### 基於4個基礎功能性介面的基本型別變形版

| functional interface | input -> output | ...                                    | 基本型別變形版                                               |
| -------------------- | --------------- | -------------------------------------- | ------------------------------------------------------------ |
| `Predicate<T>`       |                 | int, long, double -> boolean           |                                                              |
| `Consumer<T>`        | T -> void       | int, long, double -> void              | **Int**Consumer<br />**Long**Consumer<br />**Double**Consumer |
| `Function<T, R>`     | T -> R          | int, long, double -> R                 | **Int**Function`<R>`<br />**Long**Function`<R>`<br />**Double**Function`<R>` |
| `Function<T, R>`     | T -> R          | T -> int, long, double                 | To**Int**Function`<T>`<br />To**Long**Function`<T>`<br />To**Double**Function`<T>` |
| `Function<T, R>`     | T -> R          | int, long, double -> int, long, double | **Long**To**Double**Function<br />**Long**To**Int**Function<br />**Double**To**Int**Function<br />**Double**To**Long**Function<br />**Int**To**Double**Function<br />**Int**To**Long**Function |
| `Supplier<T>`        | () -> T         | () -> boolean, int, long, double       | **Boolean**Supplier<br />**Int**Supplier<br />**Long**Supplier<br />**Double**Supplier |

#### 介面 `ToDoubleFunction`

使用時，須提供一個T型別作為泛型，方法`applyAsDouble()`傳入T型別物件，回傳double基本型別

```java
package java.util.function;
public interface ToDoubleFunction<T> {
    public double applyAsDouble(T t);
}
```

使用範例

```java
List<Person> p1 = Person.createList();
Person first = p1.get(0);
ToDoubleFunction<Person> convertAgeToDouble = p -> p.getAge();
System.out.println(convertAgeToDouble.applyAsDouble(first));
```

跟以下匿名類別同價

```java
ToDoubleFunction<Person> convertAgeToDouble = new ToDoubleFunction<Person>() {
    public double applyAsDouble(Person p) {
        return p.getAge();
    }
};
```

---

#### 介面 `DoubleFunction`

使用時需要提供R型別作為泛型，傳入double型別，回傳R型別的物件

```java
package java.util.function;
public interface DoubleFunction<R> {
    public R apply(double value);
}
```

使用範例

```java
DoubleFucntion<String> calc = t -> String.valueOf(t * 10);
String result = calc.apply(3.1415926);
System.out.println("New value is: " + result);
```

與以下匿名類別意思一樣

```java
DoubleFunction<String> calc = new DoubleFunction<String>() {
   public String apply(double v) {
       return String.valueOf(v * 10);
   }
};
```

---

### 基於 Binary（二運算元相關）以及其基本型別變形版

以下介面的方法都有兩個傳入參數

| Functional Interface  | input -> output   | primitive variant..       | ...                                                          |
| --------------------- | ----------------- | ------------------------- | ------------------------------------------------------------ |
| `BinaryOperator<T>`   | (T, T) -> T       | 將T換成 int, long, double | **Int**BinaryOperator<br />**Long**BinaryOperator<br />**Double**BinaryOperator |
| `BiPredicate<L, R>`   | (L, R) -> boolean |                           | None                                                         |
| `BiConsumer<T, U>`    | (T, U) > void     | 將U換成 int, long, double | Obj**Int**Consumer`<T>`<br />Obj**Long**Consumer`<T>`<br />Obj**Double**Consumer`<T>` |
| `BiFunction<T, U, R>` | (T, U) -> R       | 將R換成 int, long, double | To**Int**BiFunction<T, U><br />To**Long**BiFunction<T, U><br />To**Double**BiFunction<T, U> |

#### 介面 `BiPredicate`

使用時須提供一個T型別和一個U型別作為泛型，方法`test()`用T型別/U型別作為參數，回傳true/false

```java
public interface BiPredicate<T, U> {
    public boolean test(T t, U u);
}
```

使用範例

```java
List<Person> p1 = Person.createList();
Person first = p1.get(0);
String testName = "john";
BiPredicate<Person, String> nameBiPred = (p, s) -> p.getName().equalsIgnoreCase(s);
System.out.println("Is the first john? " + nameBiPred.test(first, testName));
```

與以下匿名類別等價

```java
BiPredicate<Person, String> nameBiPred = new BiPredicate<Person, String>() {
    public boolean test(Person p, String s) {
        return p.getName().equalsIgnoreCase(s);
    }
};
```

---

### 基於 Unary（單運算元相關）及其基本型別變形版

`UnaryOperator<T>`

+ 繼承介面 `Function<T,T>`，但泛型數量降為一個
+ 只傳入一個物件，傳入和回傳的型別一致
+ 過程通常會改變物件T的某些型態 

| functional interface | input -> output | primitive variants        | ...                                                          |
| -------------------- | --------------- | ------------------------- | ------------------------------------------------------------ |
| `UnaryOperator<T>`   | T -> T          | 把T換成 int, long, double | **Int**UnaryOperator<br />**Long**UnaryOperator<br />**Double**UnaryOperator |

介面定義

```java
package java.util.function;
public interface UnaryOperator<T> extends Function<T,T> {
    public T apply(T t);
}
```

使用範例

```java
List<Person> p1 = Person.createList();
Person first = p1.get(0);
UnaryOperator<String> unaryStr = s -> s.toUpperCase();
System.out.println("Before: " + first.getName());
System.out.println("After: " + unaryStr.apply(first.getName()));
```

與以下匿名類一樣意思

```java
UnaryOperator<String> unaryStr = new UnaryOperator<String>() {
    public String apply(String s) {
        return s.toUpperCase();
    }
};
```

---

## 使用方法參照

Lambda 匿名方法須包含三個部分

1. 方法參數（argument list）
2. 箭頭符號（arrow token），即`->`
3. 方法內容（body）

如果方法內只是呼叫另外一個方法(如委派 delegation)，可以再把lambda表達式簡化為方法參照（method reference），依被呼叫的方法種類與來源分為以下類型

1. 方法是 **類別方法**
2. 方法是 **物件方法**，物件參考來自Lambda表示式之外
3. 方法是 **物件方法**，物件參考來自Lambda表示式之內
4. 使用new呼叫建構子，且建構子**不帶參數**
5. 使用new呼叫建構子，且建構子**帶少量參數**
6. 使用new呼叫建構子，且建構子**帶多個參數**

#### 示範情境

1. background: `Arrays.sort()`

   ```java
   public static<T> void sort(T[] a, Comparator<? super T> c) {
       ...
   }
   ```

2. background: `Comparator<T>`

   ```java
   @FunctionalInterface
   public interface Comparator<T> {
       int compare(T o1, To2);
       // ...
   }
   ```

3. 輔助類別 `StringUtil` 與 `Employee`

   ```java
   public class StringUtil {
       // 類別方法: 大寫S結尾，用以區分方法以 static 宣告
       static int compareS(String s1, String s2) {
           return s1.compareToIgnoreCase(s2);
       }
       
       // 物件方法
       int compare(String s1, String s2) {
           return s1.compareToIgnoreCase(S2);
       }
   }
   ```

   ```java
   public class Employee {
       String name;
       public Employee() {
       }
       public Employee(String name) {
           this.name = name;
       }
       public String getName() {
           return name;
       }
       public void setName(String name) {
           this.name = name;
       }
   }
   ```

### 方法參照 - 使用類別方法

```java
ContainingClass::staticMethodName
```

如下示範

```java
static void byClassMethod(String[] arr) {
 // Arrays.sort(arr, (a, b) -> StringUtil.compareS(a, b)); // lambda expr
    Arrays.sort(arr, StringUtil::compareS);                // 方法參照
    printArray(arr);
}
```

---

### 方法參照 - 使用物件方法，且物件參考來自 Lambda 表示式之外

```java
objectReference::instanceMethodName
```

如下示範

```java
static void byOutsideObjectMethod(String[] arr) {
    StringUtil util = new StringUtil();
 // Arrays.sort(arr, (a, b) -> util.compare(a, b));  // lambda expr
    Arrays.sort(arr, util::compare);                 // 方法參照
    printArray(arr);
}
```

---

### 方法參照 - 使用物件方法，且物件參考來自 Lambda 表示式之內

```java
ObjectReferenceType::instanceMethodName
```

如下示範

```java
static void byInsideObjectMethod(String[] arr) {
 // Arrays.sort(arr, (a, b) -> a.compareToIgnoreCase(b));
    Arrays.sort(arr, String::compareToIgnoreCase);
    printArray(arr);
}
```

> ☕🍪 兩種使用物件參考的實例方法
>
> 1. 物件參考來自 lambda expr 表示式外面，要傳進來只能用原來的變數名稱
>
> 2. 剩餘的不能再使用變數名稱，要改用類別名稱
>
>    這樣很像使用靜態方法，都是把類別名稱放前面
>
>    本例String類的 `compareToIgnoreCase()` 不是static，還是可以區分

---

### 使用 new 呼叫建構子，且建構子不帶參數

```java
ClassName::new
```

可以改用方法參照，以new呼叫建構子

建構子不帶參數時，可讓介面`Supplier<T>`作為物件提供者的角色

1. `T` Generic 為建構子建立的物件型態
2. 以方法參照定義產生物件的方式，如下 line 3
3. 在 line 4 使用`Supplier<T>`的 `get()` 方法可直接提取新建物件 

```java
static void byConstructorWithSupplier() {
    // Supplier<Employee> supplier1 = () -> new Employee();
    Supplier<Employee> supplier = Employee::new;
    Employee emp = supplier2.get();
    emp.setName("Jim");
    System.out.println(emp.getName());
}
```

### 使用 new 呼叫建構子且建構子帶少量參數

當建構子帶參數時，可改用介面`Function<T,R>`作為物件提供者的角色

1. `T`為建構子參數，`R`為建構子建立的物件型態
2. 以方法參照定義產生物件的方式，如 line 3
3. line 4 使用 Function 的 `apply(T)` 方法，傳入建構子參數，回傳新建物件型態 `R`

```java
static void byConstructorWithFunction() {
    // Function<String, Employee> factory1 = (s) -> new Employee(s);
    Function<String, Employee> factory2 = Employee::new;
    Employee emp = factory2.apply("Jim");
    System.out.println(emp.getName());
}
```

### 使用 new 呼叫建構子且建構子帶多參數

如果建構子帶多參數，例如 class Student：

```java
public class Student {
    String name;
    int age;
    public Student(String name, int age) {
        this.name = name;
        this.age = age;
    }
    @Override
    public String toString() {
        return "Student [name=]" + name + ", age=" + age + "]";
    }
}
```

可自訂功能性介面作為物件提供者的角色：

```java
@FunctionalInterface
public interface StudentFactory {
    Student createStudent(String name, int age);
}
```

用方法參照取代 Lambda expression：

```java
static void byConstructorWithCustomFunction() {
    // StudentFactory factory = (name, age) -> new Student(name, age);
    StudentFactory factory = Student::new;
    Student s = factory.createStudent("Jim", 10);
    System.out.println(s);
}
```



---

# 10 使用 `Stream API`

## 建構者設計模式和方法鏈結

All-args-constructors 在欄位漸多的時候可能會產生以下問題

1. 某些類別會依傳入欄位不同而建構出不同功能性的物件（Overloaded建構子）
2. 建構子參數可能很多
3. 漸購子參數若有多個屬於相同型別，會造成組合複雜且設計困難
4. 須判斷 null 情況

**建構者設計模式（builder design pattern）**：改用建構者`builder`類別產生物件，抽出建構物件的邏輯

```java
public class Person {
    private String name, email;
    private int age;
    private Person(Builder builder) {
        this.name = builder.name;
        this.age = builder.age;
        this.email = builder.email;
    }
    public String getName() {
        return name;
    }
    public int getAge() {
        return age;
    }
    public String getEmail() {
        return email;
    }
    @Override
    public String toString() {
        return "Name=" + name + ", Age=" + age + ", email=" + email + "\n";
    }
    public void printPerson() {
        System.out.println(this);
    }
    
    /**
     * 靜態巢狀類別 Person.Builder 的定義
     * Builder 的 setter() 方法，因為是 Builder 類，慣例上方法命名不以 set 開頭，
     **/
    public static class Builder {
        private String name, email;
        private int age;
        public Builder name(String name) {
            this.name = name;
            return this;
        }
        public Builder age(int val) {
            this.age = val;
            return this;
        }
        public Builder email(String val) {
            this.email = val;
            return this;
        }
        public Person build() {
            return new Person(this);
        }
    }
}
```

範例方法 `createPersonList()`-

使用 `Person.Builder` 類別建構 Person 物件

```java
public static List<Person> createPersonList() {
    List<Person> people = new ArrayList<>();
    people.add(
        new Person.Builder()
        .name("Bob")
        .age(21)
        .email("bob@x.com")
        .build()
    );
    people.add(
    	new Person.Builder()
        .name("Jane")
        .age(25)
        .email("jane@x.com")
        .build()
    );
    people.add(new Person.Builder()
              .name("John").age(25).email("john@x.com").build());
    people.add(new Person.Builder()
              .name("Phil").age(55).email("phil@x.com").build());
    people.add(new Person.Builder()
              .name("Betty").age(85).email("betty@x.com").build());
    return people;
}
```

建構者設計模式讓物件可以用「方法鏈結（method chaining）」的方式進行

這是Java 8 API 開始推廣的程式碼撰寫風格，特色如下

1. 多個方法可以用單一行程式碼表達，更容易理解程式碼
2. 物件建立方式更有彈性
3. 每一個設定屬性欄位的 `setter()` 方法都回傳物件自己
4. 程式碼更加流暢（fluent）

## 使用 Optional 類別

### 使用 null 造成的困擾

情境：有個方法允許輸入不同數量整數，計算平均值。如果呼叫該方法時沒有輸入任何整數，應該回傳什麼？

如果回傳0，要如何區分**輸入多個零**以及**什麼都沒有輸入**的兩種情境？

通常會直接回傳null，同時將方法回傳的值改用基本型別的包覆類別（wrapper class）

```java
public static Double averageWithNull(int... scores) {
    if (scores.length == 0)
        return null;
    int sum = 0;
    for (int score : scores)
        sum += score;
    return (double) sum / scores.length;
}
```

如果沒有任何整數個數傳進 `averageWithNull()` 方法，會馬上回傳 null

處理 null 的測試方法

```java
public static void testNull() {
    char str[] = { 'D', 'u', 'k', 'e' };
    String s = null;
    for (char c : str) {
        s = s + c;
    }
    System.out.println(s);  // 和字元相連 nullDuke
    Object o = null;
    System.out.println(o);  // 處理物件, 遇到 null 就印出 null
    // System.out.println(null); // can't compiled
}
```

### 類別 Optional 的使用情境

`Optional<T>`類別於 Java 8 推出，支援泛型，概念用法如下：

1. 屬於 `java.util` 套件
2. 使用上像是「容器/箱子」，`<T>`表示箱子裡可存放 T物件也可以是空的（empty, i.e. null）
3. 方法 `isPresent()` 確認內容物T是否存在
   若回傳 true，則可用 `get()` 方法取得內容物件T
4. 和功能性介面一樣，有其他支援基本型別的擴充版本
   + `Optional`Double
   + `Optional`Int
   + `Optional`Long

建立 Optional 物件的幾種方式

| 方法釋例                            | 物件內容                     | 備註             |
| ----------------------------------- | ---------------------------- | ---------------- |
| static `Optional.empty()`           | 沒有內容物件                 | 空Optional物件   |
| static `Optional.of(value)`         | 內含物件 value               | 該物件不可為null |
| static `Optional.ofNullable(value)` | 可能有/可能沒有內容物件value | 結合前兩種方式   |

```java
public static <T> Optional<T> ofNullable(T value) {
    return value == null ? empty() : of(value);
}
```

接著把之前示範過的 `averageWithNull()` 方法由可能回傳 null 改成回傳 Optional

```java
public static Optional<Double> averageWithOptional(int... scores) {
    if (scores.length == 0)
        return Optional.empty();
    int sum = 0;
    for (int score; scores)
        sum += score;
    return Optional.of( (double) sum / scores.length );
}
```

如果回傳內容有可能為 null 時，都用 `Optional<T>` 型態回傳，那就不用每次都加 `if(x != null)`事先檢查

### 類別 Optional 的常用方法

#### Java 8 的 Optional API

以下是 Java 8 剛推出 `Optional<T>` 時常用的方法

| 方法簽名與回傳                                  | 有內含物時                               | 內含物為null時                   |
| ----------------------------------------------- | ---------------------------------------- | -------------------------------- |
| `T get()`                                       | 回傳內含物                               | throw `NoSuchElementException`   |
| `void ifPresent(Consumer)`                      | 執行Consumer定義的方法                   | 啥也不做                         |
| `void isPresent()`                              | 回傳 true                                | 回傳 false                       |
| `T orElse(T other)`                             | 回傳內含物                               | 回傳指定的 other 物件            |
| `T orElseGet(Supplier)`                         | 回傳內含物                               | 回傳 Supplier 定義的方法執行結果 |
| `T orElseThow(Supplier)`                        | 回傳內含物                               | 拋出 Supplier 定義的方法例外     |
| `Optional<U> map(Function<T, U>)`               | 回傳 Function 定義的方法的 Optional 結果 | 回傳 `Optional.empty()`          |
| `Optional<U> flatMap(Function<T, Optional<U>>)` | 回傳 Function 定義的方法的 Optional 結果 | 回傳 `Optional.empty()`          |

程式碼示例：`get()`、`ifPresent()`、`isPresent()`、`orElse()`、`orElseGet()`、`orElseThrow()`

```java
private static void testOptionalOfJava8() {
    out.println("show01: " + averageWithOptional(90, 100));
    out.println("show02: " + averageWithOptional());
    
    Optional<Double> optOK = averageWithOptional(90, 100);
    if (optOK.isPresent()) {
        out.println("show03: " + optOK.get());
    }
    Optional<Double> optNG = averageWithOptional();
    try {
        optNG.get();
    } catch (NoSuchElementElement e) {
        err.println("show04 throws: " + e.g)
    }
    
    Optional<Double> opt1 = averageWithOptional(90, 100);
    opt1.ifPresent(d -> out.println("show05: " + d));
    
    // there is value in Optional
    Optional<Double> opt2 = averageWithOptional(90, 100);
    out.println("show06: " + opt2.orElse(Double.NaN));
    out.println("show07: " + opt2.orElseGet(() -> Math.random()));
    out.println("show08: " + opt2.orElseThrow(() -> new MyOptionalException()));
    
    // there is no value in Optional
    Optional<Double> opt3 = averageWithOptional();
    System.out.println("show09: " + opt3.orElse(Double.NaN));
    out.println("show10: " + opt3.orElseGet(() -> Math.random()));
    try {
        opt3.orElseThrow(() -> new MyOptionalException());
    } catch (MyOptionalException e) {
        err.println("show11 throws: " + e.getClass());
    }   
}
```

---

程式碼示例：`map()`、`flatMap()`

+ 因為都需要傳入一個功能性介面 **Function** 的 Lambda 表示式

+ Lambda 表示式這裡用方法參照取代
  1. 建立line 1~3 的 `getLength4map()` 方法，再以方法參照用於 `map()` 方法
  2. 建立line 4~6 的 `getLength4flatMap()` 方法，再以方法參照用於 `flatMap()` 方法
  
  ```java
  private static Integer getLength4map(String in) {
      return in.length();
  }
  private static Optional<Integer> getLength4flatMap(String in) {
      return Optional.ofNullable(in).map(s -> s.length());
  }
  private static void mapAndFlatMap() {
      String str = "jim";
      Optional<Integer> oil = Optional.ofNullable(str).map(OptionalDemo::getLength4map);
      Optional<Integer> oi2 = Optional.ofNullable(str).flatMap(OptionalDemo::getLength4flatMap);
  }
  ```

+ `map()` 與 `flatMap()` 使用方式相同/差異

  | 方法簽名與回傳                                  | 相同               | 差異                                                   |
  | ----------------------------------------------- | ------------------ | ------------------------------------------------------ |
  | `Optional<U> map(Function<T, U>)`               | 回傳 `Optional<U>` | 作為參數的的功能性介面<br />Function 方法回傳          |
  | `Optional<U> flatMap(Function<T, Optional<U>>)` | 回傳 `Optional<U>` | 作為參數的功能性介面 Function 的方法回傳 `Optional<U>` |

  

#### Java 8 之後的新增的 Optional API

`Optional<T>` 在 Java 8 之後推出較常用的方法如下

| 方法簽名與回傳                             | 有內含物                                | 內含物為null                                        |
| ------------------------------------------ | --------------------------------------- | --------------------------------------------------- |
| `void ifPresontOrElse(Consumer, Runnable)` | 使用內含物執行 Consumer 定義的方法      | 執行 Runnable 定義的方法                            |
| `Optional<T>` or `(Supplier)`              | 回傳自己（this），即原本的`Optional<T>` | 執行 Supplier 定義的方法，回傳型態須為`Optional<T>` |
| `Stream<T> stream()`                       | 將內含物T以`Stream.of(T)`型態回傳       | 回傳 `Stream.empty()`                               |
| `T orElseThrow()`                          | 回傳內含物                              | 拋出例外 `NoSuchElementException`                   |
| `boolean isEmpty()`                        | 回傳 false                              | 回傳 true                                           |

程式碼示例：`ifPresentOrElse()`、`or()`、`orElseThrow()`、`isEmpty()`

```java
private static void testOptionalAfterJava8() {
    Optional<String> o1 = Optional.of("value");
    Optional<String> o2 = Optional.empty();
    // ifPresentOrElse()
    o1.ifPresentOrElse {
        s -> System.out.println("Found " + s),
        () -> System.out.println("Not found")};
    o2.ifPresentOrElse {
        s -> System.out.println("Found " + s),
        () -> System.out.println("Not found")};
    // or()
    o1 = o1.or(() -> Optional.of("default"));
    System.out.println(o1);
    o2 = o2.or(() -> Optional.of("default"));
    System.out.println(o2);
    // orElseThrow()
    System.out.println(o1.orElseThrow());
    System.out.println(o2.orElseThrow());
    // isEmpty()
    System.out.println(o1.isEmpty());
    System.out.println(o2.isEmpty());
}
```

Optional 的 `stream()` 方法

+ 有內含物：`Stream.of(T)` 型態回傳
+ 無內含物：`Stream.empty()`中斷串流，方便處理來源可能為null的情況

```java
private static List<User> findUsersByName(String name) {
    return List.of(new User("jim1", 1000),
                  new User("jim2", 1000),
                  new User("duke", 1000))
        .stream()
        .filter(user -> user.getName().contains(name))
        .collect(Collectors.toList());
}

/* 以下兩個方法都可接受輸入參數為null的情況，但getTotalSalary()的執行效率略優於getTotalSalary2()，可以在確認輸入參數為null時就停止串流 */
private static Integer getTotalSalary(String name) {
    return Optional.ofNullable(name)
        .stream()
        .map(OptionalDemo::findUsersByName)
        .flatMap(Collection::stream)
        .map(User::getSalary)
        .mapToInt(Integer::valueOf)
        .sum();
}

private static Integer getTotalSalary2(String name) {
    return Optional.ofNullable(name)
        .map(OptionalDemo::findUsersByName)
        .stream()
        .flatMap(Collection::stream)
        .map(User::getSalary)
        .mapToInt(i -> i)
        .sum();
}
```

比較三個在Collection領第一個物件之欄位name的字串

```java
private static void printFirstUserName1() {
    try {
        String name = users.stream()
            .findFirst()  //NullPointerException
            .map(User::getName)
            .orElse("user not found");
        System.out.println("Optional= " + name);
    } catch (Exception e) {
        System.err.println("Option1 throws " + e.getClass());
    }
}

private static void printFirstUserName2(List<User> users) {
    try {
        String name = Optional.ofNullable(users)
            .map(list -> list.get(0)) //ArrayIndexOutOfBoundsException
            .map(User::getName)
            .orElse("user not found");
        System.out.println("Option2 = " + name);
    } catch (Exception e) {
        System.err.println("Option2 throws " + e.getClass());
    }
}

/* 可同時處理空集合物件或者null的情況 */
private static void printFirstUserName3(List<User> users) {
    try {
        String name = Optional.ofNullable(users) // prevent null
            .stream()
            .flatMap(Collection::stream)
            .findFirst() // prevent empty collection
            .map(User::getName)
            .orElse("user not found");
        System.out.println("Option3= " + name);
    } catch (Exception e) {
        System.err.println("Option3 throws " + e.getClass());
    }
}
```



## Stream API 介紹

### 介面 Iterable 和 Collection 的擴充

+ interface `Iterable.forEach()` 允許傳入實作 Consumer 功能性介面的參考物件

  ```java
  List<Persion> p1 = createPersonList();
  p1.forEach(p -> System.out.println(p));
  ```

+ interface `Collection.stream()` 幫 Collection 容器物件裝水龍頭，流出 Stream 物件，取代用迴圈存取 Collection 成員物件的舊方法

#### Stream 

跟建構者設計模式（builder）一樣可以流暢的使用方法鏈結（method chaining）

1. `filter()`: 
   + 接受實作 Predicate 介面的參考物件
   + 對流過的集合物件成員使用 `Predicate.test()` 方法進行篩選
2. `forEach()`:
   + 接受實作 Consumer 介面的參考物件
   + 對流入的集合物件成員操作 `accept()` 方法

```java
List<Apple> apples = getAllApples();
apples.stream()
    .filter(a -> a.getDiameter() >= 17 && a.getDiameter() <= 23)
    .forEach(a -> System.out.println(a));
```

> 增加程式的重複使用性：
>
> 將 Lambda 表示式改用參考變數的方式呈現
>
> ```java
> List<Apple> apples = getAllApples();
> Predicate<Apple> criteria = a -> a.getDiameter() >= 17 && a.getDiameter() <= 23;
> Consumer<Person> action = a -> System.out.println(a);
> apples.stream()
>     .filter(criteria)
>     .forEach(action);
> ```

### Stream API

1. 套件 `java.util.stream`
2. 方法鏈結 chaining methods
3. 比較 Collection 與 Stream 介面
   + `Collection`依照成員物件屬性（List, Set, Queue）提供不同管理和存取方式
   + `Stream`沒有提供直接存取特定成員的方式，只是以宣告式描述做法對 Stream 來源進行操作

#### Stream 特性

1. 不可改變的 immutable
2. Stream 介面中，定義鏈結方法的作用方式
   + 連續的（serial / sequential）- 預設作用方式
   + 平行的（parallel）- 多執行緒
3. 鏈結方法又稱為管線操作（pipeline operations）/串流方法



#### 管線操作（pipeline operations）的特性

1. Stream 在管線裡面傳輸
2. 管線分多段，來源（source）定義後，每一段代表一個作業（operation）
   + 來源（Source）- Collection 物件、檔案、Stream 物件
   + 中間作業（Intermediate Operation）- 可以零或多個
   + 終端作業（Intermediate Operation）- 只有一個
   + 短路型終端作業（Short-Circuit Terminal Operation）- 只有一個
3. **Lazy情況（懶加載）**
   + Java 會順著每段管線依序向下執行，但會先確認終端作業的方式，才會回頭要求 Stream 開始輸送資料
   + 只在開始執行時才要求輸送資料
4. 搭配短路型終端作業時，相較迴圈處理而言，有效能上優勢



---

## Stream API 操作

categories of pipeline operations

| 分類                                                 | 常用方法                                                     |
| ---------------------------------------------------- | ------------------------------------------------------------ |
| Intermediate Operation<br />中間作業                 | `filter()`, `map()`, `peek()`, `sorted()`, `flatMap()`       |
| Terminal Operation<br />終端作業                     | `forEach()`, `count()`, `sum()`, `average()`, `min()`, `max()`, `collect()` |
| Short-Circuit Terminal Operation<br />短路型終端作業 | `findFirst()`, `findAny()`, `anyMatch()`, `allMatch()`, `noneMatch()` |

### 中間作業

#### 1. 使用 `map()` 轉換 Stream 內容

Method declaration

```java
<R> Stream<R> map(Function<? super T, ? extends R> mapper);
```

+ 傳入某型別，經過某些流程之後，回傳另一種型別

+ 使用 Function 介面的實作物件當參數

  ```java
  Function<Integer, Integer> timesTwoFunc = n -> 2 * n;
  Stream<Integer> mapResult = Stream.of(1, 2, 3, 4)
      .map(timesTwoFunc);
  Object[] arr = mapResult.toArray();
  List<Object> list = Arrays.asList(arr);
  System.out.println(list); // [2, 4, 6, 8]
  ```

+ 其它基本型別的擴充版

  1. `mapToInt()`
  2. `mapToLong()`
  3. `mapToDouble()`

  ```java
  ToIntFunction<String> mapper = Integer::parseInt;
  /* 
  ToIntFunction<String> mapper = new ToIntFunction<String>() {
      @Override
      public int applyAsInt(String value) {
          return Integer.parseInt(value);
      }
  };
  */
  IntStream mapResult = Stream.of("a1", "a2", "a3")
      .map(s -> s.substring(1))
      .mapToInt(mapper);
  mapResult.forEach(i -> System.out.print(i + ", "));
  ```

  

#### 2. 使用 `peek()` 窺視 Stream 內容

Method declaration

```java
Stream<T> peek(Consumer<? super T> action);
```

以 `peek()` 方法窺視 Stream 內容

1. 使用`Consumer`介面表示需要對資料成員套用的方法為

   + 可以傳入參數，且沒有回傳（void）
   + 方法結束後，成員回歸 Stream

2. `peek()`方法主要用於 debug

   + 使用情境：需要了解當 Stream 成員經過其他中間作業之後的變化情況

3. 若管線沒定義終端作業，則不會啟動 peek()

   + 反映 Stream 物件的 Lazy 特質

   ```java
   Stream.of("a", "j", "y", "c")
       .filter(i -> i.length() > 3)
       .peek(n -> System.out.println("Filtered value: " + e))
       .map(String::toUpperCase)
       .peek(g -> System.out.println("Mapped value: " + e))
       .forEach(System.out::println);
   ```

4. `peek()` 也可以更改資料成員，但不建議用來修改資料成員

   + 平行執行時可能會有執行緒安全（thread safe）問題

     ```java
     Consumer<Integer> action = System.out::println;
     /* line 相當於以下程式
     Consumer<Integer> action = new Consumer<Integer>() {
         public void accept(Integer t) {
             System.out.print(t);
         }
     };
     */
     Stream<Integer> stream = Stream.of(1, 2, 3, 4).peek(action);
     System.out.println("Length: " + stream.toArray().length);
     /* 
     line2 的 peek() 不會被觸發，
     只有執行到 toArray() 終端作業後，
     才會觸發 peek() 方法
     */
     ```



#### 3. 使用 `sorted()` 做基本排序

Method Declaration - two ways

1. 依自然順序重新排序

   ```java
   Stream<T> sorted();
   ```

2. 依 Comparator 定義的順序重新排序

   ```java
   Stream<T> sorted(Comparator<? super T> comparator);
   ```

```java
public static void testSorted() {
    List<String> lt = Arrays.asList("a2", "a1", "b1", "c2", "c1");
    lt.stream()
        .sorted()
        .forEach(s -> System.out.print(s + ", "));
    System.out.println();
    lt.stream()
        .sorted(String::compareTo)
        .forEach(s -> System.out.print(s + ", "));
    System.out.println();
    lt.stream()
        .sorted((s1, s2) -> s1.compareTo(s2) + -1)
        .forEach(s -> System.out.print(s + ", "));
    
}
```



#### 4. 搭配 Comparator 進行多段式排序

常見的三段式排序情境

1. 先比較：成員特定欄位/特定條件

   ```java
   comparing(Function<? super T, ? extends U> keyExtractor)
   ```

   

2. 再比較：成員額外欄位/額外條件
   (視情況而使用，通常是步驟一比較不出結果時會用的)

   ```java
   thenComparing(Function<? super T, ? extends U> keyExtractor)
   ```

   

3. 最後：視情況倒置比較結果

   ```java
   reversed()
   ```

```JAVA
public static void testComparing() {
    List<Apple> apples = Arrays.asList(
        new Apple("Fuji", 18),
        new Apple("Gourmet", 23),
        new Apple("Gala", 23),
    	new Apple("Anna", 12));
    Function<Apple, String> getAppleCultivar = Apple::getCultivar;
    Function<Apple, Integer> getAppleDiameter = Apple::getDiameter;
    Comparator<Apple> comp = 
        Comparator.comparing(getAppleDiameter)
        		  .thenComparing(getAppleCultivar);
    apple.stream()
        .sorted(comp)
        .forEach(s -> System.out.print(s + ", "));
    // Anna, Fuji, Gala, Gourmet
    
    apple.stream()
        .sorted(comp.reversed())
        .forEach(s -> System.out.print(s + ", "));
    // Gourmet, Gala, Fuji, Anna
}
```



#### 5. 使用 `flatMap()` 展開 Stream 成員成子 Stream 物件

Method Declaration

```java
flatMap(Function<? super T, ? extends Stream<? extends R> mapper)
```

+ 使用 Function 介面將 Stream 成員欄位以 Stream 形式再展開/呈現
+ 有層層展開再將之攤平（flat）的效果

Examples:

```java
public class Item {
    String name;
    Item(String name) {
        this.name = name;
    }
    public String toString() {
        return this.name;
    }
}
```

```java
public class Order {
    String name;
    Order(String name) {
        this.name = name;
    }
    List<Item> items = new ArrayList<>();
    public String toString() {
        return this.name;
    }
}
```

```java
public class DataSource {
    static List<Order> getOrdersAndItems() {
        List<Order> orders = new ArrayLIst<>();
        IntStream.range(1, 4)
            .forEach(i -> orders.add(new Order("Order_" + i)));
        /* ⭐ range(1,4) 可以決定對 forEach()操作幾次
               forEach() 的 i 變數會從1開始 ~ 3結束，不含4 */
        orders.forEach(
          order -> 
              IntStream
              .range(1, 4)
              .forEach(
                  i ->
                    order.items.add(
                        new Item("Item_" + i + " , from <" + order.name + ">")
                    )
                  // orders內每個Order物件中，爛位 items 各新增3個Item物件
              )
        );
        return orders;
    }
}
```

> `flatMap()`的示例程式碼

```java
long qty = DataSource.getOrdersAndItems().stream() // Stream<Order>
    //.peek(System.out::println)
      .flatMap(order -> order.items.stream())  // Stream<Item>
    //.peek(System.out::println)
      .count();
System.out.println(qty);
```

> 以下範例：
>
> 1. 將檔案 flatMap.txt 裡所有資料行讀入 `Stream<String>` 物件
>
> 2. 再用 `flatMap()` 以每行字串裡的空白作為切割符號，**攤平**成更長的 `Stream<String>` 物件
>
> 3. 最後以 `filter()` 濾出包含關鍵字的成員並計算數量
>
>    用 `peek()`查看每段轉換成果

##### flatMap.txt

```txt
my apple is bought for making pies
your apple is purchased for writing codes
his/her apple is in the eye of beholder
```

##### Example

```java
public static void flatMapDemo2() throws IOException {
    // 取得路徑
    Path p = Paths.get("src/.../.../.../flatMap.txt") .toAbsolutePath(); 
    // 傳回符合值
    long matches = Files.lines(p) 
        // .peek(System.out::println) 
        .flatMap(line -> Stream.of(line.split(" "))) 
        // .peek(System.out::println) 
        .filter(word -> word.contains("apple")) 
        // .peek(System.out::println) 
        .count(); 
    // 印出結果 - # of Matches: 3
    System.out.println("# of Matches: " + matches); 
}
```



###  終端作業

#### 1. 使用 `count()` 計算 Stream 成員數量

Method Declaration

```java
count();
```

Example

```java
public static void testCount() {
    long cnt = Stream.of("Hello", "World").count();
    System.out.println(cnt);
}
```

#### 2. 使用 `max()` 和 `min()` 取出 Stream 成員的最大值與最小值

Method Declaration

```java
max(Comparator<? super T> comparator);
min(Comparator<? super T> comparator);
```

+ 取得最大值 & 最小值
+ 回傳值以 Optional 包裹，因為 Stream 可能沒有成員

```java
public static void testMaxMin() {
    Comparator<String> comparator = String::compareTo;
    
    Optional<String> os = Stream.of("x", "y").max(comparator);
    System.out.println(os);  
    
    List<String> list = new ArrayList<String>();
    Optional<String> empty = list.stream().max(comparator);
    System.out.println(empty);
    
    OptionalInt oi = Stream.of(1, 2, 3)
        				   .mapToInt(i -> i)
                           .min();
    System.out.println(oi);
}
```



#### 3. 使用 `average()` 和 `sum()` 計算 Stream 成員的平均值與加總

Method Declaration

```java
average();
sum();
```

必須為基礎型別的擴充型 Stream，才能用數學計算相關方法，例如`average()`、`sum()`

1. DoubleStream
2. IntStream
3. LongStream

##### Example - `average()`

```java
public static void testAverage() {
    OptionalDouble avg = Stream.of(1, 2, 3, 4)
                        	   .mapToInt(i -> i)
        					   .average();
    System.out.println(avg);
    System.out.println(avg.getAsDouble());
    
    IntStream is = Arrays.stream(new int[] {});
    OptionalDouble emptyAvg = is.average();
    System.out.println(emptyAvg);
    
}
```

> IntStream、LongStream、DoubleStream 具備 average() 方法

##### Example - `sum()`

```java
public static void testSum() {
    // Stream.of(1, 2, 3, 4).sum(); // 編譯失敗 - Stream<Integer> 沒有此方法
    
    // IntStream    10
    int iSum = Stream.of(1, 2, 3, 4).mapToInt(i -> i).sum();
    System.out.println(iSum);
    
    // LongStream   10
    long iSum = Stream.of(1, 2, 3, 4).mapToLong(i -> i).sum();
    System.out.println(iSum);
    
    // DoubleStream 10.0
    double dSum = Stream.of(1, 2, 3, 4).mapToDouble(i -> i).sum();
    System.out.println(dSum);
    
    // Empty Stream 0
    int zero = IntStream.of().sum();
    System.out.println(zero);
}
```



### 終端作業 `collect()` 與 Collectors API

Method Declaration

```java
collect(Collector<? super T, A, R> collector); 
```

`java.util.stream.collect()` 方法可以彙整或轉化 Stream 成員

常搭配 Collectors 類的靜態方法，把以下方法傳入 collect() 方法就可以

1. Collectors.toList()、Collectors.toSet()
2. Collectors.toMap()
3. Collectors.averagingDouble()
4. Collectors.joining()
5. Collectors.groupingBy()
6. Collectors.partitioningBy()
7. Collectors.mapping()、Collectors.flatMapping()
8. Collectors.filtering()

##### Example reference

```java
public class Apple {
    String cultivar;
    int diameter;
    // others ...
}
```

```java
public class TerminalOpCollectDemo {
    public static List<Apple> getAppleList() {
        List<Apple> apples = Arrays.asList(
            new Apple("Fuji", 18),
            new Apple("Gourmet", 23),
            new Apple("Gala", 23),
            new Apple("Anna", 12));
        return apples;
    }
}
```

#### 1. `Collectors.toList()`、`Collectors.toSet()`

API definition

```java
public static <T> Collector<T, ?, List<T>> toList() {...}
public static <T> Collector<T, ?, Set<T>> toSet() {...}
```

> 轉存為List之後，會保留所有字串
>
> 轉存為Set之後，會自動移除重複的字串

#### 2. `Collectors.toMap()`

API definition

```java
public static <T, K, U> Collector<T, ?, Map<K, U>> toMap(
    			Function<? super T, ? extends K> keyMapper,
    			Function<? super T, ? extends U> valueMapper) {...}
```

Example - 指定 k, v 來源後，轉存為 Map 物件

```java
public static void testToMap() {
    Map<String, Integer> map = getPersonList().stream()
        .collect(Collectors.toMap(Person::getName, Person::getAge));
    System.out.println(map);
}
```



#### 3. `Collectors.averagingDouble()`

API Definition

```java
public static <T> Collector<T, ?, Double> averagingDouble(ToDoubleFunction<? super T> mapper) {...}
```

+ `ToDoubleFunction`定義的方法 - 將輸入物件轉換成 Double

Example

> 使用 collect() 方法，傳入 `Collectors.averagingDouble(ToDoubleFunction)`，可以將眾多 stream 成員的特定屬性轉換成 double，求得平均值

```java
public static void testAveragingDouble() {
    Double averageAge = getPersonList().stream()
        .collect(Collectors.averagingDouble(p -> p.age));
    System.out.println(averagAge);
}
```



#### 4. `Collectors.joining()`

API Definition (3 Overloaded methods)

```java
public static Collector<CharSequence, ?, String> joining() {...}
public static Collector<CharSequence, ?, String> joining(
    CharSequence delimiter) {...}
public static Collector<CharSequence, ?, String> joining(
    CharSequence delimiter, CharSequence prefix, CharSequence suffix) {...}
```

Example

```java
public static void testJoining() {
    List<String> s1 = Arrays.asList("a", "b", "c", "d");
    
    String s1Join = s1.stream().collect(Collectors.joining());
    System.out.println(s1Join); // abcd
    
    String s2Join = s1.stream().collect(Collectors.joining("-"));
    System.out.println(s2Join); // a-b-c-d
    
    String s3Join = s1.stream().collect(Collectors.joining("-", "/*", "*/"));
    System.out.println(s3Join); // /*a-b-c-d*/
}
```



#### 5. `Collectors.groupingBy()`

API Definition (3 overloaded)

```java
public static <T, K, A, D> Collector<T, ?, Map<K, D>> groupingBy(
	Function<? super T, ? extends K> classifier, 
	Collector<? super T, A, D> downstream) {...}

public static <T, K> Collector<T, ?, Map<K, List<T>>> groupingBy(
    Function<? super T, ? extends K> classifier) {...}

public static <T, K, D, A, M extends Map<K, D>> Collector<T, ?, M> groupingBy(
    Function<? super T, ? extends K> classifier, 
    Supplier<M> mapFactory, 
    Collector<? super T, A, D> downstream) {...}
```

> 用 collector() 方法傳入 `Collectors.groupingBy()`，可將 Stream 成員分類（grouping），如下步驟
>
> 1. 參數1使用 Function，以輸入型別T取得另一種型別K
>    + K(分類的鍵值): 可能是T的屬性，或是T物件經處理後的某個結果
> 2. 參數2決定分類後的Stream成員儲存型態
>    + 使用 Collector interface 定義
>    + 如果是 `Collector.toList()`表示分類後以List集合物件儲存
>    + `Collector.toList()`即為預設值，可省略此參數
> 3. 結合前2參數可得分類後的key & value

```java
public static void testGroupingBy() {
    Function<Person, Integer> classifier = Person::getAge;
    Map<Integer, List<Person>> personsByAge = getPersonList().stream().collect(Collectors.groupingBy(classifier));
    
    personsByAge.forEach(
        (age, personList) -> System.out.format("age %s: %s\n", age, personList)
    );
}
```



#### 6. `Collectors.partitioningBy()`

API Definition (2 overloaded)

```java
public static <T, D, A> Collector<T, ?, Map<Boolean, D>> partitioningBy(
    Predicate<? super T> predicate, 
    Collector<? super T, A, D> downstream) {...}

public static <T> Collector<T, ?, Map<Boolean, List<T>>> partitioningBy(
    Predicate<? super T> predicate) {...}
```

> 使用 collect() 方法，傳入`Collectors.partitioningBy()`，可以將 stream 成員依照 Predicate 定義的方法分成兩類（滿足/不滿足），如下步驟
>
> 1. 使用 Predicate 定義之方法進行測試，再依照測試結果分成兩類(key-true/false)
> 2. 參數2決定分類後的Stream成員儲存型態
>    + 使用 Collector interface 定義
>    + 如果是 `Collector.toList()`表示分類後以List集合物件儲存
>    + `Collector.toList()`即為預設值，可省略此參數
> 3. 結合前2參數可得分類後的key & value，回傳 Map<Boolean, Object>

```java
public static void testPartitioningBy() {
    Map<Boolean, List<Person>> personByAge = getPersonList().stream().collect(Collectors.partitioningBy(s -> s.age > 20));
    System.out.println("Is age > 20 ?");
    personsByAge.forEach(
        (key, val) -> System.out.println(
                    key + ":\t"
            			+ val.stream().map(s -> s.name)
                             .collect(Collectors.joining(", "))
                )
    );
}
```



#### 7. `Collectors.mapping()`、`Collectors.flatMapping()`

`mapping()` 與 `flatMapping()` 功能相次但參數的 Function 介面定義不同，`flatMapping()` 必須回傳 Stream 物件

```java
public static <T, U, A, R> Collector<T, ?, R> mapping(
    Function<? super T, ? extends U> mapper, 
    Collector<? super U, A, R> downstream) {...}

public static <T, U, A, R> Collector<T, ?, R> flatMapping(
    Function<? super T, ? extends Stream<? extends U>> mapper, 
    Collector<? super U, A, R> downstream) {...}
```

> `Collectors.mapping()`、`Collectors.flatMapping()` 與 `Collectors.groupingBy()`此三者目的相似，都是要進行分類
>
> 1. 對於分類的**依據欄位的定義方式**皆相同
> 2. 對於分類的**對象**與**結果**則各有特色

```java
public static void testMappingFlatMapping() {
    List<Blog> blogs = Blog.getBlogs();
 // 1. groupingBy 
    /*
    參數1=分類的欄位依據, 參數2=分類之後以List儲存不同類的Blog物件
    key=Blog物件的authorName, val=List<Blog>
    */
    Map<String, List<Blog>> authorByName = blogs.stream().collect(Collectors.groupingBy(Blog::getAuthorName, Collectors.toList()));
    System.out.println(authorByName);
 // 2. groupingBy + mapping
    /*
    分類的對象改由 Collectors.mapping()決定，可以不再是整個Blog物件，
    而是Blog物件裡的某個欄位值
    分類依據(key)=Blog.authorName
    分類對象=Blog物件的comments欄位，型態為List<String>
      又因為分類後以List存放，所以value=List<List<String>>  
    */
    Map<String, List<List<String>>> authorComments1 = blogs.stream().collect(Collectors.groupingBy(Blog::getAuthorName, Collectors.mapping(Blog::getComments, Collectors.toList())));
    System.out.println(authorComments);
 // 3. groupingBy + flatMapping
    Map<String, List<String>> authorComments2 = blogs.stream().collect(
        Collectors.groupingBy(
            Blog::getAuthorName, 
            Collectors.flatMapping(
                blog -> blog.getComments().stream(), //(param1)分類的對象
                Collectors.toList()))); 
                /* (param2)被分類的不是Blog物件的原始 List<String> comments，
                   而是攤平後的眾多String，再以List存放，所以value=List<String>*/
    System.out.println(authorComments2);
}
```

> `Collectors.flatMapping()` 使用方式類似 `Collectors.mapping()`
>
> + 最大差異是分類的對象欄位必須可以產生 Stream 物件，因為API定義
>
>   `Function<? super T, ? extends Stream<? extends U>> mapper`
>
> + 好處: 如果遇到前例分類的回傳結果為`Map<String, List<List<String>>>`時，可攤平為`Map<String, List<String>>`
>
>   + `{Celia=[[Not bad, Ok, Just fine, all right]], Aaron=[[Nice, Very Nice, Great]]}` 
>   + `{Celia=[Not bad, Ok, Just fine, all right], Aaron=[Nice, Very Nice, Great]} `



#### 8. `Collectors.filtering()`

API Definition - 跟`Stream.filter()`差不多，都用於過濾

```java
public static<T, A, R> Collectors<T, ?, R> filtering(
    Predicate<? super T> predicate, 
    Collector<? super T, A, R> downstream) {...}
```

Example - 不同方法, 相同結果

> 1. 先用`Stream.filter()`過濾，再用`collect(Collectors.toList())`彙整
>
> 2. 使用`Stream.collect()`，同時以`Collectors.filtering()`、`Collectors.toList()`指定過濾條件&彙整方式
>
> ```java
> //1.
> List<Person> filter = persons.stream()
>     .filter(p -> p.getAge() > 20)
>     .collect(Collectors.toList());
> 
> //2.
> List<Person> filtering = persons.stream().collect(
>     Collectors.filtering(p -> p.getAge() > 20, Collectors.toList()));
> ```

Example 

> 1. 先`Stream.filter()`，再`Collectors.groupingBy()`指定分組依據（`Person::getName`）、分組對象以`Collectors.counting()`表示符合的 Person 物件數量
>
> 2. 使用`Stream.collect()`與`Collectors.groupingBy()`指定分組依據為`Person::getName`
>
>    分組過程同時以`Collectors.filtering()`指定過濾條件與`Collectors.counting()`計算符合的 Person 物件數量
>
> ```java
> //1. Stream.filter() + Collectors.groupingBy()
> Map<String, Long> filter4Grouping = persons.stream()
>     .filter(p -> p.getAge() > 20)
>     .collect(
>     	Collectors.groupingBy(
>             Person::getName, 
>             Collectors.counting() ));
> // {Pamela=1, Peter=1}
> 
> //2. Collectors.filtering() + Collectors.groupingBy()
> Map<String, Long> filtering4Grouping = persons.stream()
>     .collect(
>     	Collectors.groupingBy(
>             Person::getName, 
>             Collectors.filtering(
>                 p -> p.getAge() > 20,
>                 Collectors.counting() )));
> // {Pamela=1, Max=0, David=0, Peter=1}
> ```

### 短路型終端作業

在Stream所有成員都被接觸/處理之前，能因為某些情況而提前終止

作業目的：以搜尋為主，可以用最小成本執行並結束工作

`Stream<T>`依搜尋後回傳結果可分為兩類：

| return       | method                                                       |
| ------------ | ------------------------------------------------------------ |
| boolean      | boolean **allMatch**(Predicate<? super T> predicate);<br />boolean **noneMatch**(Predicate<? super T> predicate);<br />boolean **anyMatch**(Predicate<? super T> predicate);<br />*以Predicate.test()判斷是否滿足搜尋條件的基準* |
| Optional\<T> | Optional\<T> **findFirst**();<br />Optional\<T> **findAny**(); |



#### 1. `allMatch()`

API Definition

```java
boolean allMatch(Predicate<? super T> predicate);
```

> 1. 如果所有成員都滿足Predicate介面定義的條件，回傳true
> 2. 一旦找到不滿足條件的成員，就結束搜尋，回傳false
> 3. 若Stream為空，不會執行`Predicate.test()`，直接回傳true
>
> ```java
> public static void testAllMatch() {
>     List<String> list = Arrays.asList("jean1", "jean2", "jean3", "jean4");
>     boolean containsJean = list.stream().allMatch(p -> p.contains("jean"));
>     // 所有成員都包含"jean"，回傳true
>     boolean contains1 = list.stream().allMatch(p -> p.contains("1"));
>     // 第二筆資料不含"1"，故為false
> }
> ```



#### 2. `noneMatch()`

API Definition

```java
boolean nonMatch(Predicate<? super T> predicate);
```

> 1. 如果全部成員**「皆不」**滿足Predicate介面定義的條件，回傳true
> 2. 一旦發現滿足條件的成員，就結束搜尋，回傳false
> 3. 若Stream為空，不會執行`Predicate.test()`，直接回傳true
>
> ```java
> List<String> list = Arrays.asList("jean1", "jean2", "jean3", "jean4");
> boolean contains5 = list.stream().noneMatch(p -> p.contains("5"));
> ```



#### 3. `anyMatch()`

API Definition

```java
boolean anyMatch(Predicate<? super T> predicate);
```

> 1. 發現任何一個成員滿足Predicate介面定義的條件，就回傳true，結束搜尋
> 2. 若Stream為空，不會執行`Predicate.test()`，直接回傳false
>
> ```java
> boolean lengthOver5 = Stream.of("two", "three", "eighteen")
>     	.anyMatch(s -> s.length() > 5); // true
> ```



#### 4. `findFirst()`

API Definition

```java
Optional<T> findFirst();
```

> 1. 找到`Stream<T>`裡面**第一個**成員，就回傳`Optional<T>`，結束程式
> 2. 每次找到的結果**都為固定**，稱之為「**決定性（deterministic）**」
> 3. 沒成員時，回傳 empty 的 Optional 物件
>
> ```java
> Optional<String> val = Stream.of("one", "two").findFirst();
> // Optional[one]
> ```



#### 5. `findAny()`

API Definition

```java
Optional<T> findAny();
```

> 1. 找到`Stream<T>`裡面**任何一個**成員，就回傳`Optional<T>`，結束程式
> 2. 每次找到的結果**不一定相同**，稱之為「**非決定性（non-deterministic）**」
>    尤其是平行執行時，如果要得到固定結果，需改用`findFirst()`
>
> ```java
> List<String> list = Arrays.asList("jean1", "jean2", "jean3", "jean4");
> Optional<String> val = list.stream().findAny();
> // Optional[jean1]
> ```

---

## Stream API 和 NIO.2

`java.nio.file.Files`有一些方法支援 Stream API，使 NIO.2 也可以用流暢的語法撰寫

#### 1. `list()`

Method Declaration

```java
/* list() 方法可以列出 Path dir 下的所有檔案，
   但只在第一層，
   即並不是以遞迴方式列出各層的所有檔案和目錄 */
public static Stream<Path> list(Path dir)
```

Example

```java
void testList() throws IOException {
    try (Stream<Path> stream = Files.list(Paths.get("src/cource/c10"))) {
        stream
            .filter(path -> path.toString().endsWith(".txt"))
            .forEach(System.out::println);
    }
}
```



#### 2. `find()`

Method Declaration

```java
/* 
   find() 方法可以找出符合條件的目錄或檔案
   1. Path start - 搜尋起始目錄
   2. int maxDepth - 搜尋指定的層數，非遞迴搜尋全部
   3. BiPredicate<Path,BasicFileAttributes> matcher - 以此定義的方法當作搜尋條件
   4. FileVisitOption... options - 定義其他搜尋條件(如果有需要)
*/
public static Stream<Path> find(
    Path start, 
    int maxDepth, 
    BiPredicate<Path, BasicFileAttributes> matcher, 
    FileVisitOption... options
)
```

Example

```java
void testFind() throws IOException {
    try (Stream<Path> stream = 
         Files.find(Path.get("src"),
                    4,
                    (path, attr) -> path.toString().endsWith(".txt"))) {
        stream.forEach(System.out::println);
    }
}
```



#### 3. `walk()`

Method Declaration (2 overloaded)

```java
//1. 指定要遞迴的層數
public static Stream<Path> walk(Path start,
                                int maxDepth,
                                FileVisitOption... options) {...}

//2. 不指定層數，要遞迴全部
public static Stream<Path> walk(Path start,
                                FileVisitOption... options) {...}
```

方法概念類似 NIO.2 裡面的方法，都能遞迴拜訪相關層級的所有檔案/目錄

```java
Files.walkFileTree(Path start, FileVisitor<T> visitor);
```

但如果要對拜訪的檔案目錄採取特定動作時

1. `Files.walkFileTree()`：由 FileVisitor interface 的實作決定
2. `Files.walk()`：開啟 Stream 後，再由其管線方法 (pipeline) 決定

```java
//只遞迴4層，如果附檔名為.txt，就將其輸出
try (Stream<Path> stream = Files.walk(Paths.get("dir/NIO2Demo"), 4)) {
    stream
        .filter(path -> path.toString().endsWith(".txt"))
        .forEach(System.out::println);
}

//遞迴所有層數，如果附檔名為.txt，就將其輸出
try (Stream<Path> stream = Files.walk(Paths.get("dir/NIO2Demo"))) {
    stream
        .filter(path -> path.toString().endsWith(".txt"))
        .forEach(System.out::println);
}
```



#### 4. `lines()`

Method Declaration

```java
public static Stream<String> lines(Path path, Charset cs) {...}
public static Stream<String> lines(Path) {...}    // 預設UTF-8 charset
```

Example

```java
try (Stream<String> stream = Files.lines(Paths.get("data.txt"))) {
    stream
        .map(String::toLowerCase)
        .forEach(System.out::println);
}
```

| method                   | notes                                                        |
| ------------------------ | ------------------------------------------------------------ |
| `Files.lines()`          | 搭配管線作業只會處理需要的內容<br />lazy、效能較佳<br />回傳 Stream\<String> |
| `Files.readAllLines()`   | 把所有檔案內容一次載入JVM<br />回傳 List\<String>            |
| `BufferedReader.lines()` | 回傳 Stream\<String>                                         |

Example - BufferedReader

```java
try (BufferedReader reader = Files.newBufferedReader(Paths.get("data.txt"))) {
    reader
        .lines()
        .map(String::toLowerCase)
        .forEach(System.out::println);
}
```

> NOTE:
>
> Stream也有實作AutoCloseable介面，可以放在try-with-resource的程式區塊裡
>
> 1. 如果Stream用在Collection：不用特別在使用完後關閉
> 2. Stream搭配NIO.2開啟檔案：**必須**在結束時主動關閉，或者用try-with-resource架構處理



---

## Stream API 操作平行化

### 平行化的前提

Stream feature:

1. 無法更改內容 immutable：一旦更改都會回傳新物件，或拋出Exception
2. 無法重複使用：要使用就必須再產生新物件
3. 可以使用
   + 循序處理 sequential
   + 平行處理 parallel

Stream API 支援建構者設計模式的流暢撰寫風格

#### 撰寫風格_1：指令式編程（imperative programming）

> 特色
>
> 1. 迴圈必須經歷所有集合成員
> 2. 知道迴圈做了哪些事（how），但不是很清楚目的（what）
> 3. 迴圈中必須有其他變數，例如sum
> 4. 不容易以平行處理提高效能
>
> ```java
> double sum = 0;
> for (Employee e : getEmployees()) {
>     if (e.name.startsWith("Jim") && e.salary >= 1500) {
>         e.show();
>         sum += e.salary;
>     }
> }
> System.out.print(sum);
> ```
>
> 

#### 撰寫風格_2：流暢式編程（streaming programming）

> 特色
>
> 1. 程式本身清楚陳述目的（what）
> 2. 不需要額外變數
> 3. 可藉由「懶人（lazy）優化機制」提升效能
> 4. 可以平行處理提高效能
>
> ```java
> double sum = getEmployee().stream()
>     .filter(e -> e.name.startsWith("Jim"))
>     .filter(e -> e.salary >= 1500)
>     .peek(e -> e.show())
>     .mapToDouble(e -> e.salary)
>     .sum();
> System.out.print(sum);
> ```
>
> 

#### 撰寫風格_3

> 此作法少了Stream管線操作的好處（如：懶人優化法、平行化處理），故不建議
>
> ```java
> void streamingProgramming3() {
>     Stream<Employee> s1 = getEmployees().stream();
>     Stream<Employee> s2 = s1.filter(e -> e.name.starsWith("Jim"));
>     Stream<Employee> s3 = s2.filter(e -> e.salary >= 1500);
>     Stream<Employee> s4 = s3.peek(e -> e.show());
>     DoubleStream s5 = s4.mapToDouble(e -> e.salary);
>     double sum = s5.sum();
>     System.out.print(sum);
> }
> ```
>
>  

### 平行化的作法	

#### 基本原則

平行處理（parallel）：啟動多執行緒來減少執行時間

1. 建議硬體要具備多核心CPU或GPU

2. 底層用Fork/Join架構，但不建議開發者直接使用，應該用高階API

3. 有許多因素可影響平行執行的加速效果

   + **平行處理** 不是每次都比 **循序處理** 快
   + 影響因素：資料大小、拆解方法、結果聚合方式、CPU核心數

4. 可以藉由以下方式啟動

   1. 從 Collection（集合物件）發動，使用`parallelStream()`方法

   2. 由 Stream（串流物件）發動，使用`parallel()`方法

      ```java
      double sum = getEmployees().stream()
          .filter(e -> e.name.startsWith("Jean"))
          .filter(e -> e.salary >= 1500)
          .peek(e -> e.show())
          .mapToDouble(e -> e.salary)
          .parallel()
          .sum();
      ```

      

5. 未呼叫`.parallel()`時，預設`.sequential()`

6. Stream（串流物件）發動平行化處理時，如果要**各段管線操作都可以平行化**，必須在尾端呼叫

7. 過程中不可以修改來源物件（例如 Collection）



#### 管線操作的變數必須是「沒有狀態（stateless）」

```java
public static void statefullStreaming() {
    List<Employee> eList = getEmployee();
    List<Employee> blockList = new ArrayList<>();
    eList.parallelStream()
        .filter(e -> e.name.starts)
        .forEach();
    /* 無法平行化加速，因為變數blockList有狀態(成員持續增加)
       導致多執行緒無法分頭進行 */
}
```

> 如果有此類需求，建議改用 collect() + `Colllectors.toList()`
>
> + Java視需要自動調度，程式設計師不用介入物件狀態維護
>
>   (何時建立List物件、新增成員、merge成員...)
>
> ```java
> List<Employee> eList = getEmployees();
> List<Employee> nonblockList = eList.parallelStream()
>     .filter(e -> e.name.startsWith("Jean"))
>     .collect(Collectors.toList());
> ```



#### 平行處理可能讓結果不同

##### 決定性演算法（Deterministic Algorithm）

只要輸入相同參數，無論執行幾次結果都會相同。對Stream發動平行處理時，大部分結果都會固定。

###### `sum()`方法無關乎平行處理時的順序先後，結果都一定相同

```java
List<Employee> eList = getEmployees();
double r1 = eList.stream()
    .filter(e -> e.name.startsWith("Jean"))
    .mapToDouble(Employee::getSalary)
    .sequential()
    .sum();
double r2 = eList.stream()
    .filter(e -> e.name.startsWith("Jean"))
    .mapToDouble(Employee::getSalary)
    .parallel()
    .sum();
```

###### `findAny()`方法只要找出一個就結束，平行化處理可能每次結果不同

```java
List<Employee> eList = getEmployees();
Optional<Employee> e1 = eList.stream()
    .filter(e -> e.name.startsWith("Jean"))
    .sequential()
    .findAny();
Optional<Employee> e2 = eList.stream()
    .filter(e -> e.name.startsWith("Jean"))
    .parallel()
    .sum();
```



### Reduction 操作

#### Reduction 的基礎操作

Reduction Operation - 歸納或簡化操作

1. 接受一連串項目（items）的輸入
2. 將輸入項目，經由逐一、反覆使用某結合功能（combining function）之後，得到單一結果。過程會讓原先輸入項目逐漸減少，所以稱之為`reduce`

Stream 物件的 `reduce()` 方法 - 以 sum() 改用 reduction 概念實作

1. 以數字 0 為基礎值（base value）

2. 使用 + 運算子作為結合功能（combining function）

   ```mathematica
   sum = a1 + a2 + ... + an
   sum = ((((0 + a1) + a2) + ...) + an)
   ```

整數加總 - 使用介面 IntStream 的 reduce() 方法

```java
int reduce(int identity, IntBinaryOperator op);
```

介面 `IntBinaryOperator` 的定義

```java
@FunctionalInterface
public interface IntBinaryOperator {
    int applyAsInt(int left, int right);
}
```

將整數 `sum()` 以 IntStream 介面的 `reduce()` 方法詮釋 + Lambda 表示式如下:

```java
.reduce(0, (a, b) -> a + b);
//or
.reduct(0, (sum, element) -> sum + element);
```

Example

```java
public static void testReduceInSequential() {
    int result = IntStream.rangeClosed(1, 4)
        .reduce(0, (sum, element) -> sum + element);
}
```

IntStream的`rangeClosed(start, end)`以及`range(start, end)`兩個靜態方法差別在於end參數是否被包含

1. `rangeClosed(int startInclusive, int endInclusive)`範圍包含參數 end，等同於

   ```java
   for (int i = startInclusive; i <= endInclusive; i++) {
       //...
   }
   ```

2. `range(int startInclusive, int endExclusive)`範圍**不**包含參數 end，等同於

   ```java
   for (int i = startInclusive; i < endExclusive; i++) {
       //...
   }
   ```

![以reduction概念實作 sum() 方法的分解示意圖](https://i.imgur.com/7iZnwOs.png)

Reduction概念除了可以用在 sum()，max()、min() 以及方法參照(method reference) 也可以使用，使程式更簡潔

```java
public static void testReduceWithCompactly() {
    int sum = IntStream.rangeClosed(1, 4)
        .reduce(0, Integer::sum);
    System.out.println("sum = " + sum);
    
    int max = IntStream.rangeClosed(1, 4)
        .reduce(0, Integer::max);
    System.out.println("max = " + max);
    
    int min = IntStream.rangeClosed(1, 4)
        .reduce(0, Integer::min);
    System.out.println("min = " + min);
}
```



#### Reduction 的平行操作

如果「結合功能（combining function）」是「可組合的（associative）」(==個別項目沒有特定關係，其順序不影響結果)，就能使用平行化處理

如果不為可組合的，那使用 reduce() 會得到錯誤結果，其中`count()`就是`sum()`小變形，把 IntStream 裡面所有項目都轉化為1之後求總和

```java
.map(item -> 1).sum()
```

> 將原本的一根管線分流，加快處理速度

Example

```java
public static void testReduceInParallel() {
    int result = IntStream.rangeClosed(1, 8)
        .parallel()
        .reduce(0, (sum, element) -> sum + element);
    System.out.println("Result = " + result);
}
```

管線操作的平行化處理，底層是 Fork/Join 架構

1. 先將所有參與加總的整數進行*切割 & 分組*（decomposition）
2. 加上 reduce() 處理架構，所有整數會*歸納 & 加總*（merging）



#### 平行化處理的注意事項

1. 平行處理效能不一定比較快，有時甚至會比循序處理慢。須有硬體支援，像是多核CPU和GPU
2. 平行處理必須考量**最初拆解**、**最終合併**作法是否合適。中間作業（例如`filter()`）也會影響拆解與合併的效能
3. 因為自動 **開箱/裝箱（boxing/unboxing）**會降低執行效率，直接用基本型別的變形Stream會有比較好的效能表現，例如`IntStream`、`LongStream`、`DoubleStream`

---

# 11 Date/Time API

## Date & Time 相關類別演進

#### 日期（Date）與時間（Time）的重要性

需要表現日期/時間，或者用於計算的程式情境，像是

1. 取得當地（local）的現在、過去、未來的日期/時間
2. 比較兩個時間點的差異，by years, months, days, hours, minutes, seconds
3. 不同國家的時差（time zone）
4. 日光節約時間（daylight savings time）調整
5. 描述日期/時間的**區間**
   + `Duration`：描述時間區間, hours, minutes, seconds
   + `Period`：描述日期區間, years, months, days
6. 閏年（leap year）時 2月的天數
7. 日期時間的顯示格式（format）

#### Java 8 之前的日期時間 API

`java.util.Date` 和 `java.util.Calendar` 不足之處

1. 不支援流暢語法（fluent），無法用類似建構者模式的方式撰寫
2. 物件實例皆為mutable，與Lambda表示式不相容
3. 非執行緒安全（not thread-safe）
4. API 種類不多

#### Java 8 之後的日期時間 API

Java 8 之後使用不同類別，分開表達日期與時間，優勢：

1. 類別/方法的使用相當直覺化
2. 可流暢語法
3. 物件實例皆 immutable，相容於 Lambda 表示式
4. 定義日期時間是依ISO標準
5. 執行緒安全
6. API種類多，且方便自行再擴充
7. `toString()` 回傳有意義、可讀性高的說明

---

## 當地日期與時間

Java 8用`java.time`套件底下API定義**當地**的日期和時間，即不含時區（time zone）概念

| ...                 | LocalDate                                     | LocalTime                                                    | LocalDateTime            |
| ------------------- | --------------------------------------------- | ------------------------------------------------------------ | ------------------------ |
| def                 | 儲存years, months, days<br />只有日期不含時間 | 儲存hours, minutes, seconds, nanoseconds<br />只有時間不含日期 | 結合前兩者，包含日期時間 |
| `toString()` return | ISO8601格式的<br />YYYY-MM-DD                 | ISO8601格式的<br />HH:mm:ss.SSSS                             |                          |

### 類別 `LocalDate`

可以得到以下問題的答案

1. 某日期是否屬於過去或為來
2. 是否為閏年leap year
3. 是一週裡面的哪一天
4. 是一個月裡面的哪一天
5. 下周六是哪一天

Example

```java
LocalDate now = LocalDate.now();

LocalDate jeanBday = LocalDate.of(2000, 2, 16);

boolean isInThePast = jeanBday.isBefore(now);

boolean isInALeapYear = jeanBday.isLeapYear();

DayOfWeek dayOfWeek = jeanBday.getDayOfWeek();
// Enum: MONDAY, TUESDAY, etc.

int dayOfMonth = jeanBday.getDayOfMonth();
// from 1 to 31

int dayOfYear = jeanBday.getDayOfYear();
// from 1 to 366  

LocalDate oneMonthAfterNow = now.plusMonths(1);

LocalDate nextMonday = now.with(TemporalAdjusters.next(DayOfWeek.MONDAY));
```

> `java.util.Date`的某些時區日光節約時間下沒有午夜12點，但開發者有時候會用午夜12點來表達某一天，造成一些問題，較不適用

### 類別 `LocalTime`

用途

1. 儲存一天之內的時間
2. 從午夜12點（midnight）開始算
3. 使用24小時制顯示
4. 可得以下答案
   + 何時可用餐?
   + 用餐時間結束?
   + now + 1hr30min 之後是幾點
   + 還要幾分鐘幾小時之後才是用餐時間
   + 如何個別用hours和minutes來追蹤時間

Example

```java
LocalTime.now();

LocalTime nowPlus1hr15mins = now.plusHours(1).plusMinutes(15);

LocalTime nowHrsMins = now.truncatedTo(ChronoUnit.MINUTES); 
/* set the second of minute and the nano of second field to zero
   16:55:50.883581 -> 16:55 */

int secondsAfterMidNight = now.toSecondOfDay();

LocalTime lunch = LocalTime.of(12, 5);

boolean isLunchOver = lunch.isBefore(now);

long minsUntilLunch = now.until(lunch, ChronoUnit.MINUTES);

LocalTime bedtime = LocalTime.of(23, 20);
long hoursToBedtime = now.until(bedtime, ChronoUnit.HOURS);
```

### 類別 `LocalDateTime`

結合 LocalDate & LocalTime，可更精準描述事件發生的時間點

1. 幾點開會
2. 何時放假
3. 會議延期到周五的話，會是何日何時
4. 周一早上8點~週五下午5點，這樣一共幾小時

Example

```java
LocalDate flightDate = LocalDate.of(2022, Month.JULY, 2);
LocalTime flightTime = LocalTime.of(21, 45);
LocalDateTime flight = LocalDateTime.of(flightDate, flightTime);

LocalDateTime seminarStart = LocalDateTime.of(2022, Month.JULY, 2, 9, 30);
LocalDateTime seminarEnd = seminarStart.plusDay(2).plusHours(8);

long seminarHrs = seminarStart.until(seminarEnd, ChronoUnits.HOURS);
```

---

## 時區和日光節約時間

### 時區和日光節約時間簡介

名詞簡介

#### GMT（Greenwich Mean Time，格林威治標準時間）

英國的海上霸權擴張計劃，以通過格林威治的子午線劃分地球為東西兩半球

#### UTC（Universal Time Coordinated，國際協調時間）

1967年國際度量衡大會把秒定義為銫原子進行固定震盪次數的時間

搭配平均太陽時、地軸運動修正後的新時標、與國際原子時綜合精算成的時間

UTC比GMT精準，但兩者誤差須在0.9秒以內，超過的話會由國際地球自轉事務中央局發布閏秒

#### 全球 24 個時區

每隔經度15degree，時差一小時

經線180degree定為國際換日線

#### 日光節約時間

##### 1. 日光節約時間的定義

Daylight Saving Time(D.S.T.)在夏季充分使用光照資源，提前一小時，減少照明用電

每年三月的第二個星期日開始 ~ 每年十一月的第一個星期日

##### 2. 日光節約時間的調整

基於和UTC的差量（offset）計算DST。

例如標準時間：紐約UTC-5hrs、DST情況：紐約UTC-4hrs

##### 3. 日光節約時間的影響

+ 啟用DST會讓時間跳空一小時，結束DST會讓時間重複一小時
+ 因為DST的調整會造成調整前時間不存在，加上每個地方調整時間的始點不同。
  如果調整在午夜凌晨，會無法使用 midnight 代表當天

### Java 在時區和日光節約時間的應用

Java 8 支援時區 Time Zones 的應用類別如下：

#### 類別 ZoneId

> 使用此類別 static 方法取得特定時區物件 ZoneId，再以ZoneId取得ZoneRules
>
> ```java
> ZoneId taipei = ZoneId.systemDefault();
> ZoneId newYork = ZoneId.of("America/New_York");
> ZoneRules taipeiRules = taipei.getRules();
> ```

#### 類別 ZoneRules

此類別擁有的方法可取得該時區的相關規則（rules），像是

##### 1. `isDaylightSavings(Instant)`

​	確認傳入的Instant時間是否為DST時間
​	`getStandardOffset(instand).equals(getOffset(instand)) == false`

##### 2. `getStandardOffset(Instant)`

​	依據傳入時間判斷和UTC的時間差，不考慮是否為DST，回傳ZoneOffset物件

##### 3. `getOffset(Instand)`

​	依據傳入時間判斷和UTC的時間差，是否是DST也會影響結果，回傳ZoneOffset物件

> JDK 8 會幫每個時區（ZoneId）內建相關ZoneRules，再由 ZoneRulesProvider 類提供時區的 ZoneRules
>
> + 若`System.getProperty("java.time.zone.DefaultZoneRulesProvider")`可得設定值，就由該設定決定
> + 如果無法取得，則由`TzdbZoneRulesProvider`類別提供
>
> ```java
> public abstract class ZoneRulesProvider {
>     ...
>     public Object run() {
>         String prop = System.getProperty("java.time.zone.DefaultZoneRulesProvider");
>         if (prop != null) {
>             try {
>                 Class<?> c = Class.forName(prop, true, ClassLoader.getSystemCLassLoader());
>                 ZoneRulesProvider provider = ZoneRulesProvider.class.cast(c.newInstance());
>                 registerProvider(provider);
>                 loaded.add(provider);
>             } catch (Exception x) {
>                 throw new Error(x);
>             }
>         } else {
>             registerProvider(new TzdbZoneRulesProvider());
>         }
>         return null;
>     }
> }
> ```

由 IANA Time Zone Database（TZDB）定義時區資訊，檔案位置(JDK ver.8)位於`jdk1.8.0/jre/lib/tzdb.dat`，在JDK ver.11 則改為`jdk-11.0.13\lib\tzdb.dat`，如下:

```java
public TzdbZoneRulesProvider() {
    try {
        String libDir = System.getProperty("java.home") + File.separator + "lib";
        try (DataInputStream dis = new DataInputStream(
            new BufferedInputStream(new FileInputStream(
                new File(libDir, "tzdb.dat"))))) {
            load(dis);
        }
    } catch (Exception ex) {
        throw new ZoneRulesException("Unable to load TZDB time-zone rules", ex);
    }
}
```

#### 類別 ZoneOffset

代表該時區和UTC時間的差量（offset），繼承ZoneId類，所以也具備ZoneId的欄位與方法

#### 綜合示範

```java
ZoneId taipei = ZoneId.systemDefault();
taipei.getId();

// ZoneId >> ZoneRules
ZoneRules taipeiRules = taipei.getRules();

// US started DST
Instant beforeUsDST = Instant.parse("2023-03-10T00:00:00Z");
Instant inUsDST = Instant.parse("2023-03-12T00:00:00Z");
Instant now = Instant.now();

// zoneRules.isDaylightSavings()
// zoneRules.getDaylightSavings(instant).toHours()
// zoneRules.getOffset()
// zoneRules.getStandardOffset()
```

#### 類別 ZonedDateTime

LocalDateTime 類只能處理當地、不含時區概念的日期和時間

ZonedDateTime 物件可以結合 LocalDateTime、ZoneId 與 ZoneOffset 的資訊

```java
LocalDateTime tpNow = LocalDateTime.now()
    .truncatedTo(ChronoUnit.MINUTES);

ZoneId newYork = ZoneId.of("America/New_York");
ZonedDateTime nyNow = ZonedDateTime.now(newYork)
    .truncatedTo(ChronoUnit.MINUTES);

nyNow.getOffset(); // -04:00
nyNow.getZone();   // America/New_York

ZonedDateTime zdTime1 = ZoneDateTime.of(tpNow, newYork);
/*     YYYY-MM-ddThh:mm{getOffset}{getZone}
   eg. 2022-06-29T16:42-04:00[America/New_York] */
    
ZonedDateTime zdTime2 = zdTime1.plusDays(1).minusMinutes(15);
```

> `ZonedDateTime`也可以在時間跨過 DTS 時，正確處理：
>
> 1. 當地時間（LocalDateTime）沒有改變
> 2. 和 UTC 的時差可以被正確的被管理
>
> ```java
> ZoneId usEast = ZoneId.of("America/New_York");
> // DST Begins: 2022/03/13
> LocalDateTime beforeStartDTS = LocalDateTime.of(2022, 03, 12, 16, 00);
> ZonedDateTime timeS1 = ZonedDateTime.of(beforeStartDTS, usEast); 
> //🥝 2022-03-12T16:00-05:00[America/New_York] 
> ZonedDateTime timeS2 = timeS1.plusDays(1);
> //🥝 2022-03-13T16:00-04:00[America/New_York] 
> ZonedDateTime timeS3 = timeS1.plusHours(24);
> //🥝 2022-03-13T17:00-04:00[America/New_York] 
> 
> //DST Ends: 2022/11/06
> LocalDateTime beforeEndDTS = LocalDateTime.of(2022, 11, 16, 00);
> ZonedDateTime timeE1 = ZonedDateTime.of(beforeEndDTS, usEast);
> //🥝 2022-11-05T16:00-04:00[America/New_York] 
> ZonedDateTime timeE2 = timeE1.plusDays(1);
> //🥝 2022-11-06T16:00-05:00[America/New_York] 
> ZonedDateTime timeE3 = timeE1.plusHours(24);
> //🥝 2022-11-06T15:00-05:00[America/New_York] 
> ```

#### 類別 ZoneOffsetTransition

ZoneRules >> ZoneOffsetTransition，可判斷啟動或結束DTS時發生的時間斷層或時間重疊

1. 啟動DTS時：快轉1小時，造成時間 **斷層（gap）**
2. 結束DTS時：倒回1小時，造成時間 **重疊（overlap）**

```java
private static void gapOrOverlap(ZoneId usEast, LocalDateTime dt) {
    ZoneOffsetTransition zot = usEast.getRules().getTransition(dt);
    // ZoneRules.getTransition(LocalDateTime)
    if (zot != null) {
        if (zot.isGap())
            System.out.println("gap");
        	// 凌晨02時快轉到03時，造成時間斷層
        if (zot.isOverlap())
            System.out.println("overlap");
            // 凌晨02時調回到01時，造成時間重疊
    } else {
        System.out.println("-- ");
    }
}
```

#### 類別 OffsetDateTime

此類別可以處理跨時區的問題

```java
LocalDateTime meeting = LocalDateTime.of(2023, 02, 16, 10, 30);

ZoneId zoneTaipei = ZoneId.systemDefault();
ZonedDateTime host = ZonedDateTime.of(meeting, zoneTaipei);
OffsetDateTime offset = host.toOffsetDateTime();  //

ZoneId zoneLondon = ZoneId.of("Europe/London");
ZonedDateTime callLondon = offset.atZoneSameInstant(zoneLondon);

ZoneId zoneNewYork = ZoneId.of("America/New_York");
ZonedDateTime callNewYork = offset.atZoneSameInstant(zoneNewYork);
```



---

## 描述日期與時間的數量

### 類別 Instant

Instant 類用來儲存時間軸上*一剎那的時間*，分成2部分儲存

1. **epoch-seconds(long)**

   + 是指從UTC/GMT的1970-01-01T00:00:00Z 開始起算後經歷的時間

     + 因為認為此時間是Unix作業系統的時間起算點，又稱為

       + Unix epoch
       + Unix time
       + POSIX time
       + Unix timestamp

     + 大於此時間為正值，在這以前的時間為負值

       

2. **nanosecond-of-second(int)**

   + 儲存值在 0 ~ 999,999,999 之間
   + 其依賴於 EPOCH 時間的狀況，與以下接近
     + `System.currentTimeMillis()`的方法內容
     + 建構`java.util.Date`物件的方式

Example

```java
System.out.println(Instant.now().getEpochSecond());
// 取得EPOCH時間，以秒計: 1656407085
System.out.println(new java.util.Date().getTime());
// 取得EPOCH時間，以毫秒計: 1468131459758
```

Example - Instant

```java
Instant now = Instant.now();
Thead.sleep(0, 1); // long milliseconds, int nanoseconds
Instant later = Instant.now();
boolean isNowBeforeLater = now.isBefore(later);

Instant epoch = Instant.parse("1970-01-01T00:00:00Z");
// 1970-01-01T00:00:00Z 
```

### 類別 Period 和 Duration

1. Period:
   + 以 years, months, days 建構日期的差量，依 ISO-8601 規範
     API文件 -> `This class models a quantity or amount of time in terms of years, months, and days.`
   + 使用 `plus()` 與 `minus()` 方法時，皆以天為概念，可保留日光節約時間的變化
2. Duration:
   + 以 seconds, nanoseconds 建構時間的差量，也可以換算成 hours 和 minutes
     API文件 -> `This class models a quantity or amount of time in terms of seconds and nanoseconds. It can be accessed using other duration-based units, such as minutes and hours.`
   + 沒有日光節約時間的概念，*每一天* 被 *24小時* 的概念取代





可以使用以下方式計算兩個日期的差距

1. `ChronoUnit.DAYS.between(LocalDate, LocalDate)` 回傳差距的總天數

2. `Period.between(LocalDate, LocalDate)`

   + `getMonths()` 回傳差幾個月
   + `getDays()` 回傳差幾天

   ```java
   LocalDate christmas = LocalDate.of(2023, 12, 25);
   LocalDate today = LocalDate.now();
   System.out.println("Today is " + today);
   long days = ChronoUnit.DAYS.between(today, christmas);
   
   Period untilXMas = Period.between(today, christmas);
   // untilXMas.getMonths()
   // untilXMas.getDays())
   ```

### 使用流暢（fluent）的程式風格

```java
public class FluentDemo {
    public static void main(String[] args) {
        LocalDate myDay0 = LocalDate.of(1977, 6, 11);            // 一般語法
        LocalDate myDay1 = Year.of(1977).atMonth(06).atDay(11);  // 流暢語法
        
        /* 以下line6-19同TimeZoneAcrossDemo，但是使用流暢語法 */
        LocalDateTime meeting = LocalDate.of(2022, 07, 10).atTime(11, 30);
        
        ZonedDateTime host = meeting.atZone(ZoneId.systemDefault());
        System.out.println(host); 
        // 2023-02-24T13:27+08:00[Asia/Taipei]
        
        ZonedDateTime meetingUK = host.withZoneSameInstant(ZoneId.of("Europe/London"));
        System.out.println(meetingUK);
        // 2023-02-24T06:27+01:00[Europe/London]
        
        ZonedDateTime meetingSF = host.withZoneSameInstant(ZoneId.of("America/New_York"));
        System.out.println(meetingSF);
        // 2023-02-24T01:27-04:00[America/New_York]
    }
}
```



---

# 12 標註型別 *Annotation*

## 認識標註型別  

通常用於metadata（元資料、元數據），標註型別從Java1.5引入

### 認識Metadata

屬性欄位: 構成門票Ticket類別的基本資訊，像是價格、有效期間、購買的數量

metadata: 比較不直接的關聯

+ 顧客必須至少購買一張票
+ 限制每人每天最多購買五張票

> Little Tips🍪☕
> metadata規則的值並不需要在程式碼中直接定義，即規則的值不用寫死在程式碼中
>
> 可以在程式碼中定義規則和關係，但從其他地方讀取值(eg. 資料庫、設定檔)

### 標註型別的目的

標註型別可以將metadata資訊安插給類別、方法、實例變數，或其他Java類型(interface or enum)

Example

```java
public @interface ZooAnimal {
}
```

#### 目的與功能_1

```java
public abstract class Mammal {
}
```

```java
public abstract class Bird {
}
```

```java
@ZooAnimal
public class Lion extends Mammal {
}
```

```java
@ZooAnimal
public class Eagle extends Bird {
}
```

> 標註型別具有與介面相似的目的，但如果改用 ZooAnimal 的介面或父類別，並與Lion類建立關係，必須變更類別繼承結構
>

###### 使用標註型別首要功能是可以在**不改變其繼承結構的情況下，將實體類別分門別類**

#### 目的與功能_2

| types        | diff                                                         |
| ------------ | ------------------------------------------------------------ |
| 介面或父類別 | 只能套用在類別層級的宣告                                     |
| 標註型別     | 可應用於任何宣告，包括類別、方法、表達式、實例變數，甚至用來標註其他標註型別<br />另外還可以在建立annotation時包含稱為元素element的屬性名稱與值 |

Example - 屬性字串的預設值

```java
public @interface ZooAnimal {
    String habitat() default "";
}
```

> 若未宣告habitat屬性預設為空字串，則先前用`@ZooAnimal`標註的類別都會被要求加上habitat屬性，造成編譯失敗

Example - override habitat field

```java
public class Veterinarian {
    @ZooAnimal(habitat = "Infirmary")
    private Lion sickLion;
    @ZooAnimal(habitat = "Forest")
    private Lion healthyLion;
}
```

> 定義兩個實例變數，都以@ZooAnimal標註，也都有一個相關的棲息地屬性值
>
> 棲息地屬性值不會因為實例變數指向其他Lion物件而改變，像變數名稱一樣屬於變數宣告的一部分
>
> 沒有標註型別的作法:
> Lion類別須新增habitat屬性欄位，在實例化Lion之後設定habitat屬性值

###### 使用標註型別的第二個功能：讓系統的 metadata 維護工作變得更容易

> Little Tips☕🍪
>
> 在沒有標註型別之前，Java通常用另一個class、interface、xml、json等文件管理metadata，通常是分開的，可看成單一責任制法則（SRP, single responsibility principle）的應用
>
> 缺點：系統變複雜時，就需要增加維護成本。
> 常見範例是部署描述檔 Deployment Descriptor 的變革 (web.xml -> annotation)
> 詳 *Java RWD Web 企業網站開發指南 | 使用 Spring MVC 與 Bootstrap*

#### 目的與功能_3

Example - 可用來標註方法，並且指示何時該執行

```java
public @interface ZooSchedule {
    String[] hours();
}
```

```java
@ZooAnimal
public class Lion extends Mammal {
    @ZooSchedule(hours = { "9am", "5pm", "10pm" })
    void feed() {
        System.out.print("Time to feed the lions!");
    }
}
```

```java
@ZooAnimal
public class Eagle extends Bird {
    @ZooSchedule(hours = { "4am", "5pm" })
    void clean() {
        System.out.print("Time to sweep up!");
    }
}
```

> 雖然 feed() 與 clean() 在不同類別，但標註型別所帶資訊意義相似

###### 標註型別第三個功能：可將需要的metadata標註在完全不同的目標，即便類別、實例變數或方法不相關



#### 目的與功能_4

###### 第四個功能(last)：標註型別本身為Optional

非必要的、不做任何事情，類似 marker interface。表示可以刪除一個專案裡所有標註型別，但不影響編譯，只是執行期間可能會出錯，或有不同行為/結果

因為使用標註型別的地方通常不會是在標註的地方，通常是在底層框架或程式其他地方

例如 @ZooSchedule 未標註時間會導致該方法不會被執行，或不知道何時該被執行

但是如果新增標註型別到不適合的地方，可能會導致編譯錯誤。例:`@Override`必須可以覆寫父類別方法

> Little Tips☕🍪
>
> 最受認可、最先普及以及使用標註型別的平台是Spring framework，將標註型別應用於依賴注入技術，以用於解耦合

---

## 建立自定義標註型別

### 建立標註型別

> 指定運動型態的 metadata，全部小寫-@interface，駝峰法則-型別名稱Exercise
>
> 也可以用巢狀類別的方式宣告標註型別
>
> ```java
> public @interface Exercise {
> }
> 
> ```

###### 標記型標註型別（marker annotation）

+ 不包含任何元素，和marker interface不具備任何成員相似

```java
@Exercise()
class Cheetah {
}
@Exercise
class Sleep {
}
@Exercise
class ZooEmployee {
}
```

> ln#1，使用時可以加上`()`，不影響編譯。但如果有宣告必要元素的化，就必須使用`()`，裡面囊括元素與其值
>
> ln#8，標註型別如同介面，可以應用於和一開始需求不一致、不相關的類別

一個類別也可以同時套用多個標註型別，會作用在宣告位置的下一個**非**標註類的Java型態

```java
@AnnotationA
@AnnotationB("value1")
@AnnotationC("value2")
@AnnotationD @AnnotationE @AnnotationF
@AnnotationG public class MarieJeanClass {
}
```



### 定義必要（required）元素

```java
public @interface Exercise {
    // element
    int hoursPerDay();
}
```

Example

```java
@Exercise(hoursPerDay = 3)  // compile success
class Cheetah {
}
@Exercise hoursPerDay=0     // compiled fail
class Sleep {
}
@Exercise
class ZooEmployee {         // compiled fail
}
```

> 建立標註型別時，只要沒用 default 關鍵字建立元素預設值，就算是必要元素(**required**)

### 定義非必要（optional）元素

如果元素是非必要的，就必須要包含預設值

```java
public @interface Exercise {
    int hoursPerDay();
    int startHour() default 6;
}
```

簡單規則

1. Annotation如果有多個元素值，使用**逗號**分隔

2. 每一個元素編寫的語法**元素名稱=元素值**

3. 元素先後順序不影響編譯

   ```java
   @Exercise(startHour = 5, hoursPerDay = 3)
   class Cheetah {
   }
   @Exercise(hoursPerDay = 0)
   class Sleep {
   }
   @Exercise(hoursPerDay = 7, startHour = "8") // compile error
   class ZooEmployee {
   }
   ```

   

#### 定義預設元素值

Annotation的預設值不能為任意值，必須是「非空常量表達式（non null constant expression）」

```java
public @interface MehAnnotation {
    String name() default new String(""); // compile error,not constant expr
    String address() default "";
    String title() default null;          // compile error, cannot be null
}
```

### 定義元素型態（type）

元素也有限制型態

1. primitive data type 基本型別
2. String
3. Class
4. Enumeration 列舉型別
5. Another annotation 另一個標註型別
6. 1D array of above types 以上的一維陣列

Example

```java
/** 回報bug時可參照的文章id編號 */
@interface Reference {
    String id();
}
/** 定義bug狀態:未確認,已確認,已修復,非bug */
enum Status {
    UNCONFIRMED, CONFIRMED, FIXED, NOTABUG
}
public @interface BugReport {
    /** 是否可修復 */
    boolean repeatable();  
    /** 指定bugfix的工程師 */
    String assignedTo();
    /** 回報bug的測試人員 */
    String[] reportedBy();
    /** 驗證bug的測試案例 */
    Class<?> testCase();
    Status status();       //enumeration
    Reference ref();       //another annotation
}
```

```java
class MyTestCase {
}

@BugReport(
    assignedTo = "Jim", ref = @Reference(id="101"),
    repeatable = false, reportedBy = { "Jean", "Tony" },
    status = Status.UNCONFIRMED, testCase = MyTestCase.class )
public class BugReportUsage {
}
```

### 定義元素修飾詞

@interface源於介面interface，在Java機制裡，標註型別的元素會被轉為介面方法

與抽象介面方法一樣，標註型別元素為 public abstract

如果有明確宣告的關鍵字也不能與 abstract public 衝突

```java
public @interface ModifierLab {
    int element1();
    public abstract int element2();
    protected int element3();  // conflict against 'public'
    private int element4();    // conflict against 'public'
    final int element5(); // conflict against 'abstract'
}
```

### 定義常數

在annotation裡定義變數，跟在介面裡定義變數的限制一致，預設是 public static final

如果有明確宣告的關鍵字也不能與 public static final 衝突

```java
public @interface ConstantVarLab {
    int VAR1 = 1;
    public static final int VAR2 = 2;
    protected int VAR3 = 3; // compile fail
    private int VAR4 = 4;   // compile fail
}
```

```java
@interface ConstantVar {
    int VAR1 = 999;
}
public class ConstantVarLab2 {
    public static void main(String args[]) {
        System.out.println(ConstantVar.VAR1);  
        // 使用時不用實際建立標註型別，同介面常數使用一致
    }
}
```

---

## 標註型別的應用

### 在宣告時使用標註型別

除了應用於類別和方法，標註型別也可以應用於任何Java宣告

1. 宣告類別(class)、介面(interface)、列舉型別(enum)、模組(module)
2. 宣告靜態變數(static)、實例變數(instance)、區域變數(local)
3. 宣告方法和建構子
4. 宣告參數(方法、建構子、Lambda表示式)
5. 宣告轉型表示式
6. 宣告其他標註型別

```java
@interface Anno {
}
@interface Anno2 { 
}
@FunctionalInterface
interface Runner {
    void go(String name);
}
@Anno
@Anno2  //宣告【類別】時可以使用多個標註型別
class Dog {
    @Anno @Anno2 //宣告【建構子】時可以使用多個標註型別
    public Dog(@Anno Integer age) { //宣告【建構子參數】時可以使用標註型別
    }
    @Anno @Anno2 //宣告【方法】時可以使用多個標註型別
    public void eat(@Anno String input) { //宣告【方法參數】可以在型態前方使用
        @Anno //宣告【區域變數】可以在型態前方使用
        String m = (@Anno String) "test"; //宣告【轉型表示式】可以在型態前方使用
        Runner r1 = new @Anno Runner() { 
            public void go(@Anno @Anno2 String name) {
                System.out.print(name);
            }
        };
        //宣告Lambda表示式時，可以用標註型別
        Runner r2 = (@Anno String n) -> System.out.print(n);
    }
}
```

> 實務上可以使用`@Target`指定該標註型別可以應用於哪種宣告類型
>
> ```java
> @Target(ElementType.METHOD)
> @Retention(RetentionPolicy.SOURCE)
> public @interface Override {
> }
> ```

### 定義名稱為 value() 的元素

有些標註型別帶有元素值，但沒有元素名稱

```java
@Hurt("neck")
public class Giraffe {}
```

##### 簡略版標註型別表達式的成立條件

1. 建立此Annotation必須定義一個名為value()的元素，可為M/O

2. 建立時，**不得**再定義其他**必要**元素，非必要元素(default)則不受限制

3. 使用時，不得再為任何其他元素提供值

   ```java
   @Hurt("neck", age=2)
   public class Giraffe {}
   ```

   ```java
   public class Elephant {
       @Hurt("Legs")
       public void fallDown() {
       }
       @Hurt(value = "Legs")
       public void fallOver() {
       }
       @Hurt
       String injuries[];
   }
   ```

> Little Tips 🍪☕ 正確使用 value() 元素
>
> value() 應該要與標註型別的名稱相關，例如@Hurt的value()元素即為受傷部位
>
> 使用 value()作為元素名稱時:
>
> + value()元素不一定要有default值
> + 其他元素**一定**都要有default值，否則無法使用

### 元素值為陣列的應用方式

Example

```java
public @interface Music {
    String[] types();
}
```

> 有兩種方式可以只提供一個值給陣列，如下ln# 2、ln# 4
>
> 空元素值的表現方式如下ln#6、ln#8
>
> ```java
> public class Monkey {
>     @Music (types = { "Rock and roll" })
>     String dance;
>     @Music (types = "Alternative")
>     String sleep;
>     @Music (types = {})
>     String dislike1;
>     @Music (types ="")
>     String dislike2;
> }
> ```

---

## 自定義標註型別時使用的內建標註型別

以下是自定義標註型別時，可能會用到的內建標註型別（即 metadata 的 metadata）

### 以`@Target`限制使用標的

限制標註型別可以應用的宣告類型

```java
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.ANNOTATION_TYPE)
public @interface Target {
    ElementType[] value();
}
```

ElementType enum

```java
public enum ElementType {
    TYPE,            //
    FIELD,           //實例變數,靜態變數,列舉項目
    METHOD,          //方法宣告
    PARAMETER,       //建構子參數,方法參數,lambda expr參數
    CONSTRUCTOR,     //建構子
    LOCAL_VARIABLE,  //區域變數
    ANNOTATION_TYPE, //annotation
    PACKAGE,         //用在package-info.java裡面的package宣告
    TYPE_PARAMETER,  //泛型、泛型的參數化型態符號(parameterized types)
    TYPE_USE,        //任何宣告或使用Java型態的地方
    MODULE           //用在模組的宣告
}
```

> 某些 ElementType 的使用範圍是重疊的，例如要建立可用於其他標註型別的標註型別，可以用 `ANNOTATION_TYPE` 或者 `TYPE` 宣告

#### 了解 TYPE_USE 值

+ `ElementType.TYPE_USE`幾乎可以應用在任何使用Java型態的地方，**幾乎**可以涵蓋ElementType的其他列舉項目值
+ 例外: 只能用於具有回傳值的方法，無法用於void方法宣告

### 以 @Retention 決定作用範圍

編譯器將的 *.java 程式碼轉為 .class位元組碼時，捨棄某些和型態有關的資訊，發生在泛型，就稱為 型態抹除 type erasure

```java
@Documented
@Retention(RetentionPolicy.RUNTINE)
@Target(ElementType.ANNOTATION_TYPE)
public @interface Retention {
    RetentionPolicy value();
}
```

```java
public enum RetentionPolicy {
    SOURCE,  // annotation只存於 *.java，編譯時將被抹除
    CLASS,   // annotation可存在於 *.class，執行時將被抹除(未指定時預設為此)
    RUNTIME  // annotation可存在於 *.class，執行時期也存在
}
```

Example

```java
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;

@Retention(RetentionPolicy.SOURCE)
@interface Anno1{
}
@Retention(RetentionPolicy.CLASS)
@interface Anno2{
}
@Retention(RetentionPolicy.RUNTIME)
@interface Anno3{
}
```

### 以 @Documented 支援API文件顯示

是標記型（marker）標註型別，不存在任何元素

```java
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.ANNOTATION_TYPE)
public @interface Documented {
}
```

```java
@Documented
public @interface Hunter {
}
```

```java
@Hunter
public class Lion {
}
```

在Hunter.java與Lion.java類別所在目錄下執行指令`javadoc *.java`，產出的API文件可以發現具備annotation資訊

> Little Tips☕🍪 和指令javadoc有關的標註型別
>
> javadoc用來識別的標註型別，例如`@param`、`@return`、`@exception`。注意不要將javadoc標註型別Java標註型別混淆
>
> 通常javadoc標註型別都是**小寫字母開頭**，Java標註型別都以**大寫字母開頭**

### 以@Inherited取得父類別標註型別

標記型(marker)標註型別，不存在任何元素

```java
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.ANNOTATION_TYPE)
public @interface Inherited {
}
```

當父類別MySuper使用以@Inherited標註的自定義標註型別@InheritedAnno時，子類別MySub即便沒有直接用@InheritedAnno，一樣能取得此標註型別資訊

Example

```java
@Inherited
@Retention(RetentionPolicy.RUNTIME)
public @interface InheritedAnno {
}
```

```java
@InheritedAnno
public class MySuper {
}
```

```java
public class MySub extends MySuper {
}
// 使用映射技術在執行期間找出子類別MySub所有標註型別
public static void main(String[] args) {
    for (Annotation anno : MySub.class.getAnnotations()) {
        System.out.println(anno.annotationType().getName());
        // lab.annotation.xxx.InheritedAnno
    }
}
```



### 以@Repeatable支援重複使用同一標註型別

為何會需要可重複的標註型別？當需要應用具有不同元素值的相同標註型別時，就會使用可重複的標註型別

```java
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.ANNOTATION_TYPE)
public @interface Repeatable {
    Class<? extends Annotation> value();
    /* 另一個『以可重複標註型別的陣列作為唯一元素value()值』
       的自定義annotation的類別型態 */
}
```

Example

```java
public @interface RiskFactor {
    String desc();
    int level() default 1;
}
```

```java
@RiskFactor(desc = "Aggressive", level = 5) //failed to compile
@RiskFactor(desc = "Violent", level = 10)   //failed to compile
public class Monkey {
}
```

> 因為@RiskFactor沒有標註@Repeatable，一個標註型別只能應用一次
>
> 但如果要在@RiskFactor直接標註@Repeatable，還需要先建立另一個 **以`RiskFactor[]`為唯一元素value()型態的自定義標註型別**

```java
public @interface RiskFactors {
    RiskFactor[] value();
}
```

> 再以@RiskFactors的類別型態`RiskFactors.class`當作@Repeatable唯一元素值，然後標註在@RiskFactor上

```java
@Repeatable(RiskFactors.class) 
public @interface RiskFactor {
    String desc();
    int level() default 1;
}
```

> Little Tips🍪☕ 沒有@Repeatable時的作法
>
> Java 8 之前的自定義 annotation 沒有內建 `@Repeatable`，重複使用的做法如下
>
> ```java
> @RiskFactors({
>     @RiskFactor(desc="Aggressive", level=5),
>     @RiskFactor(desc="Violent", level=10) })
> public class Dragon {
> }
> ```
>
> 以上作法，@RiskFactor就不用標註@Repeatable

### 本章內建標註型別總結

| 內建annotation | 是否為標記型annotation | 元素value()型態   | 未使用時的行為或者選項                 |
| -------------- | ---------------------- | ----------------- | -------------------------------------- |
| @Target        | No                     | `ElementType[]`   | TYPE_USE和TYPE_PARAMETER之外的任意目標 |
| @Retention     | No                     | `RetentionPolicy` | `RetentionPolicy.CLASS`                |
| @Documented    | Yes                    | -                 | annotation的資訊不會出現在API文件中    |
| @Inherited     | Yes                    | -                 | 無法取得父類別的annotation             |
| @Repeatable    | No                     | 另一個annotation  | annotation不可重複                     |

---

## 開發一般程式碼經常使用的內建標註型別

上一章是自定義標註型別時，會用到的Java內建annotation。本章則是平常就會使用到的內建標註型別

### 使用 @Override 標註覆寫的方法

+ 標記型標註型別，指示一個方法正在覆寫一個繼承的方法（來自介面或父類別）
+ 覆寫方法必須有相同簽名、相同或者更廣泛的存取修飾詞（access modifier）與回傳類型，且不拋出任何新的或更廣泛的例外

```java
interface MyInterface {
    void myMethod();
}
```

```java
class MySuper implements MyInterface {
    @Override
    public void myMethod() {
        System.out.println("from MySuper"); 
    }
}
class MySub extends MySuper {
    @Override
    public void myMethod() {
        System.out.println("from MySub"); 
    }
}
```

> @Override **並非**一定要出現在覆寫的方法上，
> 但如果標註@Override在*不是覆寫的方法中*會導致編譯失敗
>
> Pros:
>
> 1. 提供更直觀的程式碼內容，提高程式碼質量
> 2. 幫助發現開發時的錯誤

### 使用 @FunctionalInterface 宣告介面

只有一個抽象方法的介面，違反原則就會編譯失敗

```java
@FunctionalInterface
interface MyInterface1 {
    int method1();
}

// failed to compile - 只能用於介面
@FunctionalInterface
abstract class MyClass {
    abstract String getName();
}

// failed to compile - 因為不包含任何抽象方法
@FunctionalInterface
interface MyInterface2 {
}

@FunctionalInterface
interface MyInterface3 {
    boolean method3();   // ✅PASS: 只包含一個抽象方法
}

// failed to compile - 包含兩個抽象方法，其中一個繼承自MyInterface3
@FunctionalInterface
interface MyInterface4 extends MyInterface3 {
    void method4();
}

@FunctionalInterface
interface MyInterface5 extends MyInterface3 {
    boolean equals(Object unused);  
    /* PASS: 雖然包含兩個抽象方法，但equals()與
       Object.equals()方法簽名一致，為可以移除的 */
}
```

### 使用 @Deprecated 停用程式碼

改寫既有方法(修正bug、JDK升級、提升效能)，如果方法變化太大，可能需要建立一個完全不同簽名的新版本，但不一定要刪除舊版本，因為如果該方法直接消失，可能會造成函式庫/程式呼叫者的編譯問題

合理作法：通知新版本，給予合理時間調整，再刪除舊方法，此情境下可用`@Deprecated`

```java
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(value={CONSTRUCTOR, FIELD, LOCAL_VARIABLE, METHOD, PACKAGE, MODULE, PARAMETER, TYPE})
public @interface Deprecated {
    String since() default "";
    boolean forRemoval() default false;
}
```

JDK 5 --- 標記型標註型別、無任何元素

JDK 9 --- 新增2個非必要元素: `since()`、`forRemoval()`

Example

```java
/**
 * Design and plan stuff.
 *
 * @deprecated Use EnhancedPlanner instead.
 */
@Deprecated(since="1.8", forRemoval=true)
public class Planner {
    public int getTaskQty(List<String> tasks) {
        return tasks.size();
    }
}
```

###### 1. String since()

​	由哪一個版本開始棄用，預設空字串

###### 2. boolean forRemoval()

​	將來是否會完全刪除棄用程式碼，預設false

### 使用 @SupressWarnings 忽略警告

忽略編譯器警告的淺在問題，硬是要執行特定操作，且問題實際上不會發生時，用`@SuppressWarnings`標註型別

```java
@Target({TYPE, FIELD, METHOD, PARAMETER, CONSTRUCTOR, LOCAL_VARIABLE, MODULE})
@Retention(RetentionPolicy.SOURCE)
public @interface SuppressWarnings {
    String[] value();
}
```

@SupressWarnings 可以用在類別、方法、類型等宣告上

| 元素值      | 忽略的警告對象                                               |
| ----------- | ------------------------------------------------------------ |
| deprecation | 使用以`@Deprecated`標註的類型/方法                           |
| removal     | 使用以`@Deprecated`標註，並指定forRemoval元素值的類型或方法  |
| rawtypes    | 使用原始類型（raw types），例如使用List卻未使用List\<T>      |
| unchecked   | 無法檢查（check）型態安全的程式碼，如使用List卻未使用List\<T> |
| all         | 所有的警告對象                                               |

Example - 泛型

```java
@SupressWarnings("removal")
public class PlannerUser {
    public static void main(String[] args) {
        Planner p = new Planner();
        @SupressWarnings({ "rawtypes", "unchecked" })
        int qty = p.getTaskQty(new ArrayList());
        System.out.println(qty);
    }
}
```

> 應該謹慎使用`@SuppressWarnings`標註型別，只有在不得已的情況下（程式重構很麻煩的時候，需要拿來忽略編譯器提醒的編碼問題）使用才適合

### 使用 @SafeVarargs 保護參數

可變動參數個數(varargs)：

1. 以符號`...`指示方法可傳遞零或多個相同類型的參數
2. 一個方法最多可以有一個可變動個數參數，固定放最後一個

```JAVA
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target({ElementType.CONSTRUCTOR, ElementType.METHOD})
public @interface SafeVarargs {
}
```

`@SafeVarargs`

1. 指示被標註的方法的程式邏輯不會對其varargs參數執行任何潛在的不安全操作
2. 只能應用於不能被覆寫的建構子或方法，即宣告為private, static或final的方法

Example

```java
public class UnSafeVarargsLab {
  //@SafeVarargs
    final Integer unsafeOperation(List<Integer>... manyIntegerList) {
        Object[] objArray = manyIntergerList;
        objArray[0] = Arrays.asList("error");
        return manyIntegerList[0].get(0);
        //會取得String, 非Integer, 拋出ClassCastException
    }
   
    public static void main(String[] a) {
        var carrot = new ArrayList<Integer>();
        new UnSafeVarargsLab().unsafeOperation(carrot);
    }
}
```

> Warnings:
>
> 1. line#3 方法參數警告訊息 "Type safety: Potential heap pollution via varargs parameter manyIntegerList"
> 2. line#12 使用方法警告訊息 "Type safety: A generic array of List\<Integer> is created for a varargs parameter"
>
> 加上@SafeVarargs除了是告訴編譯器此方法沒有執行任何不安全的操作，也抑制編譯器對varargs參數未經檢查的警告

Example_error

```java
public class SafeVarargsLab {
    @SafeVarargs  // 缺少可變動參數，不能標
    public static void method1(int param) {
    }
    @SafeVarargs  // 未宣告為private, static或者final
    protected void method2(String... param) {
    }
    @SafeVarargs // 為宣告為private, static或者final
    void method3(boolean... param) {
    }
}
```



### 本章內建標註型別總結

| 內建annotation       | 是否為標記annotation | 元素value()型態 | 非必要元素                               |
| -------------------- | -------------------- | --------------- | ---------------------------------------- |
| @Override            | Yes                  | -               | -                                        |
| @FunctionalInterface | Yes                  | -               | -                                        |
| @Deprecated          | No                   | -               | String since()<br />boolean forRemoval() |
| @SuppressWarnings    | No                   | String[]        | -                                        |
| @SafeVarargs         | Yes                  | -               | -                                        |

| 標註型別             | 標註目標         | 無法標註的狀況                                               |
| -------------------- | ---------------- | ------------------------------------------------------------ |
| @Override            | 方法             | 不滿足覆寫的定義                                             |
| @FunctionalInterface | 介面             | 介面不只有一個抽象方法                                       |
| @Deprecated          | 大部分的Java宣告 | -                                                            |
| @SuppressWarnings    | 大部分的Java宣告 | -                                                            |
| @SafeVarargs         | 方法<br />建構子 | 參數不是可變動個數，或者方法未以private、static或者final宣告 |



---

# 13 Java 平台模組系統

## 認識 Java 模組化

java 9 開始支援模組分類（module）

### 介紹模組（Module）

#### 模組化需求

一個大型專案會把數百/上千個類別規劃為套件，套件又再群組為JAR（Java archive），副檔名為`.jar`

要使用Spring框架或JUnit測試函式庫任何一種，都要先確保執行時擁有相關JAR的相容版本。其複雜的依賴關係和最低版本常被開源社群稱為JAR地獄，因為一旦載入錯誤版本，可能會有ClassNotFoundException，甚至是隨機異常，無法釐清是否為函式庫bug或版本不相容。

Java 9導入的Java Platform Module System(JPMS)，藉由分類並群組相關的套件，以向開發人員提供一組特定功能。比如是一個可以讓開發人員設定開放那些套件的更大的JAR檔案。

JPMS包括：

1. 模組化的JAR檔案格式
2. 模組化JDK套件
3. 提供模組化相關指令列（command-line）

### 模組化的效益

雖然要不要模組化不是強制要求，但模組提供以下方法

#### 1. 更好的存取控制

除了private, package(default), protected, public，模組可以讓A模組中的套件只對B模組的套件開放，對C模組、D模組的套件拒絕存取

將模組作為第五個層級的存取控制，可以將模組化的JAR中的套件只公開給特定套件，是更強的封裝形式

#### 2. 更清晰的依賴管理

函式庫之間常見互相依賴，像是JUnit測試函式庫搭配Hamcrest函式庫，以改進測試斷言的可讀性

通常函式庫之間的相依性只有在閱讀使用文件，或者執行到相依流程時，發現函式庫不在類別路徑上，噴ClassNotFoundException錯（JAR地獄）時才會發現

不過在完全模組化環境中，每一個開源專案都會在module-info.java檔案中指定專案依賴項目。啟動程式時，Java會告知相依函式庫不在 module-path 中，可以馬上清楚知道

#### 3. 自定義Java構建（build）內容

JDK(Java Development Kit/Java開發工具包)檔案很大，即便是底下的JRE(Java Runtime Environment/Java執行環境)都不小

為了讓行動與嵌入式裝置都能安裝Java，SE8使用「compact profile」或簡稱「profile」，以完整Java SE平台API為基礎，精簡出三個層級的子集合

| 最精簡   | 多一點API | 完整Java SE API |
| -------- | --------- | --------------- |
| compact1 | compact2  | compact3        |

> 然而籠統三個層級的方式還是缺乏靈活性，因為每個compact所需API種類是Java自己定義的，不是開發者依專案性質不同需求所設計。例如：
>
> Java Native Interface(JNI)---處理特定於作業系統的程式
>
> JDBC---資料庫存取

##### JPMS 指令工具 jlink

使開發人員能自定義自己需要的API，還可用來打包更小的執行映像檔（runtime image），另外也提高了安全性。

假設不使用AWT套件，且AWT存在安全漏洞，那就打包不含AWT的執行映像檔的應用程式，即不存在AWT的安全漏洞。

#### 4. 提升效能

因為Java知道哪些模組有需要，所以載入類別時，只關注所需模組就好了

Pros: 改善大型專案啟動時間、減少記憶體浪費

#### 5. 避免套件重複

JAR地獄其中一情境就是一個套件出現在多個JAR裡面

+ 有可能是因為JAR被重新命名，所以專案內有兩個實質相同的JAR
+ 也有可能是因為類別路徑上有兩個內容相同版本不同的JAR

JPMS可避免上述情況，讓一個套件只由一個模組提供，就不會有套件重複問題

---

## 建立和執行模組化程式

### 建立模組專案

#### 建立套件、類別與模組資訊檔案

1. 建立專案 zoo.animal.feeding、套件zoo.animal.feeding、簡單類別Task.java

   ```java
   package zoo.animal.feeding;
   public class Task {
       public static void main(String... args) {
           System.out.println("All are fed!");
       }
   }
   ```

2. 建立模組資訊檔案 module-info.java，此檔案和Java類別主要區別如下

   1. module-info.java必須位於**模組的根目錄**中，Java類別在套件中
   2. module-info.java內容宣告模組時，使用關鍵字module而不是class/interface/enum
   3. 模組名稱命名規則遵循套件名稱，中間通常包含`.`

   ```java
   module zoo.animal.feeding {
   }
   ```

3. 在與src目錄的同一階層建立一個 `mods` 目錄，用來存放與自身模組相依的其他模組。可以任意命名，但`mods`是比較通用的名稱

   ```
   -zoo.animal.feeding    	  | -zoo.animal.feeding
     -src					  |   -bin		
       -zoo.animal.feeding	  |   -mods		[navigator]
         -Task.java		  |   -src
       -module-info.java	  |     -zoo
     -mods					  |       -animal
     						  |         -feeding
     						  |           -Task.java
       [explorer]         	  |       -module-info.java
   ```

   > 通常Eclipse建立的專案，編譯後的 *.class檔會存在專案內bin目錄中

> `module-info.java` 檔案可以被清空，不會有編譯錯誤問題
>
> 編譯器看到檔案沒內容會直接結束編譯工作，也不會建*.class檔

### 編譯模組專案

```terminal
javac --module-path mods -d src src/zoo/animal/feeding/*.java src/module-info.java
```

+ `--module-path` 指示任何自定義模組檔案`*.jar`的位置(mods目錄)
+ `-d` 指定放置編譯完成的類別檔案的目錄
+ 結尾是要編譯的Java檔案清單，可單獨列出，以空白區隔，也可以使用萬用字元`*.java`

> Little Tips☕🍪 傳統類別路徑（classpath）的選項
>
> java指令使用類別路徑引用專案相依的JAR檔案的三種語法
>
> + `-cp`
> + `--class-path`
> + `-classpath`

也可以在指令列使用縮寫，例如 `--module-path` -> `-p`

```terminal
javac -p mods -d src src/zoo/animal/feeding/*.java src/module-info.java
```

> 執行指令必須在src資料夾

可以詳閱 *Spring Boot 情境式網站開發指南:使用Spring Data JPA, Spring Security, Spring Web Flow*

### 執行模組專案

執行模組的語法

​	⭐指定的模組名稱後接`/`之後才是完整的類別名稱

```terminal
java --module-path mode --module lab.module/org.some.Lab
                模組路徑        模組名稱/套件名稱.類別名稱
```

Example 

編寫指令執行zoo.animal.feeding套件中的Task類

模組名稱經常和套件完整名稱相同，或是取套件開頭的幾個名稱空間

```terminal
java --module-path src --module zoo.animal.feeding/zoo.animal.feeding.Task
```

> `--module-path` 可以使用 `-p`：用來指定模組路徑
>
> `--module` 可以使用 `-m`：用來指定執行對象

### 打包模組專案

打包模組讓該模組可以在其他地方執行，或是給其他模組使用

```terminal
jar -cvf mods/zoo.animal.feeding.jar -C src/ .
```

> `-cvf`：指定要打包為JAR檔案
>
> `mods`：JAR檔案的產出目錄，須事先建立
>
> `zoo.animal.feeding.jar`：JAR檔案名稱
>
> `-C`：指定編譯好的`*.class`檔案位置
>
> `src/ .`：打包路徑`src`內的所有檔案

以打包後的模組化JAR檔案執行程式

```terminal
java --module-path mods --module zoo.animal.feeding/zoo.animal.feeding.Task
```

> 指定的模組路徑mods目錄存在已經打包好的`zoo.animal.feeding.jar`
>
> src目錄只存放編譯好的鬆散`*.class`檔案

---

## 建立相依模組程式

### 使用exports開放模組內的套件

在建立其他模組前，要先開放zoo.animal.feeding模組提供給其他模組相依，可由修改module-info.java達成需求

```java
module zoo.animal.feeding {
    exports zoo.animal.feeding;
}
```

> `exports`：指示讓其他外部模組可以使用列舉的套件

接著重新編譯和打包

```terminal
javac -p mods -d src src/zoo/animal/feeding/*.java src/module-info.java
jar -cvf mods/zoo.animal.feeding.jar -C src/ .
```

就可以更新jar檔案了

### 使用requires相依外部模組的套件

#### 建立`zoo.animal.care`模組

在此模組底下建立兩個套件

+ `zoo.animal.care.medical` 開放給其他模組使用的類別和方法
+ `zoo.animal.care.details` 不對外開放，只供模組內部使用

Examples

```java
package zoo.animal.care.medical;
public class Drug {
}
```

```java
package zoo.animal.care.details;
import zoo.animal.feeding.Task;
public class TigerBirthday {
    private Task task;
}
```

```java
module zoo.animal.care {
    exports zoo.animal.care.medical; 
    //指出要公開的套件
    requires zoo.animal.feeding;     
    //指定care模組將相依於feeding模組
}
```

####  編譯和打包`zoo.animal.care`模組

使用 javac 指令編譯 `*.java` 檔案

```terminal
javac --module-path mods -d src src/zoo/animal/care/details/*.java src/zoo/animal/care/medical/*.java src/module-info.java
```

進行打包

```terminal
jar -cvf mods/zoo.animal.care.jar -c src/ .
```

更新專案(refresh)後如下

```
-zoo.animal.care
  -bin
  -mods
    -zoo.animal.care.jar✅
    -zoo.animal.feeding.jar
  -src
    -zoo
      -animal
        -care
          -details
            -TigerBirthDay.class✅
            -TigerBirthday.java
          -medical
            -Drug.class✅
            -Drug.java
    module-info.class✅
    module-info.java
```

#### 建立、編譯、打包`zoo.animal.shows`模組

zoo.animal.shows模組相依於feeding & care模組

shows底下三個套件全部開放

```java
module zoo.animal.shows {
    requires zoo.animal.feeding;     
    requires zoo.animal.care;     
}
```

1. content package

   ```java
   package zoo.animal.shows.content;
   public class ParrotScript {
   }
   ```

   ```java
   package zoo.animal.shows.content;
   public class LionScript {
   }
   ```

2. media package

   ```java
   package zoo.animal.shows.media;
   public class Advertisement {
       public static void main(String[] args) {
           System.out.println("We will be having shows");
       }
   }
   ```

   ```java
   package zoo.animal.shows.media;
   public class SuperStar {
   }
   ```

3. schedule package

   ```java
   package zoo.animal.shows.schedule;
   public class Weekday {
   }
   ```

   ```java
   package zoo.animal.shows.schedule;
   public class Weekend {
   }
   ```

4. 模組資訊檔 `module-info.java`

   ```java
   module zoo.animal.shows {
       exports zoo.animal.shows.content;
       exports zoo.animal.shows.media;
       exports zoo.animal.shows.schedule;
       requires zoo.animal.feeding;
       requires zoo.animal.care;
   }
   ```

接著在zoo.animal.shows底下src目錄同一層建立一個`mods`目錄，把 care.jar、feeding.jar 放進去

```
-zoo.animal.shows
  -bin
  -mods
    -zoo.animal.care.jar
    -zoo.animal.feeding.jar
  -src
    -zoo
      -animal
        -shows
          -content
            -LionScript.java
            -ParrotScript.java
          -media
            -Advertisement.java
            -SuperStar.java
          -schedule
            -Weekday.java
            -Weekend.java
      module-info.java
```

接著執行以下指令編譯
```terminal
javac --module-path mods -d src src/zoo/animal/shows/content/*.java src/zoo/animal/shows/media/*.java src/zoo/animal/shows/schedule/*.java src/module-info.java
```

執行以下指令打包

```terminal
jar -cvf mods/zoo.animal.shows.jar -C src/ .
```

就可以看到每個.java產生對應的.class檔案，且mods目錄內還有`zoo.animal.shows.jar`檔案

#### 建立、編譯、打包`zoo.staff`模組

```java
module zoo.staff {
    requires zoo.animal.feeding;
    requires zoo.animal.care;
    requires zoo.animal.shows;
}
```

編譯指令

```terminal
javac --module-path mods -d src src/zoo/staff/*.java src/module-info.java
```

打包指令

```terminal
jar -cvf mods/zoo.staff.jar -C src/ .
```

### 使用 Eclipse 設定專案的模組相依關係

各模組間依賴關係表

| 模組                 | 依賴模組                                                     |
| -------------------- | ------------------------------------------------------------ |
| `zoo.animal.feeding` | 無                                                           |
| `zoo.animal.care`    | `zoo.animal.feeding`                                         |
| `zoo.animal.shows`   | `zoo.animal.feeding`<br />`zoo.animal.care`                  |
| `zoo.staff`          | `zoo.animal.feeding`<br />`zoo.animal.care`<br />`zoo.animal.shows` |

> 在編譯新的模組前，都要先把相依的模組jar檔放到每個專案的mods資料夾內
>
> 接著使用javac命令列指令，`--module-path`指定相依模組JAR檔目錄、`-d`指定程式碼目錄

可以直接在Eclipse設定每個專案相依的模組

#### 專案`zoo.animal.feeding`

無相依的模組，略

#### 專案`zoo.animal.care`

專案right click Properties -> Java Build Path -> Projects label -> Modulepath -> Add -> Required Project Selection -> check `zoo.animal.feeding` -> Apply and Close

#### 專案`zoo.animal.shows`

步驟同上，打勾兩個 (care module & feeding module)

#### 專案`zoo.staff`

依循前例，打勾要相依的專案 (feeding, care, shows)

---

## 認識 `module-info.java` 的宣告關鍵字

這裡介紹 module-info.java 編寫的宣告指令，像是 exports, requires, provides, uses, opens 等出現和使用的時機

> Little Tips🍪☕ **模組宣告指令exports和requires是Java關鍵字嗎**
>
> `exports`、`requires`、`module`屬於編寫模組資訊檔 module-info.java 內的關鍵字，一旦不在此範圍就不是，所以類別或者介面仍然可以用這些字當作變數名稱
>
> 為了要向前相容（Java 8編寫的程式碼，可以通過Java 11的編譯）不能隨意增加關鍵字，所以 exports, requires, provides 只定義在`module-info.java`範圍內，以因應Java 9才出現的模組化功能

### 使用exports

exports package-name 可以將一個套件公開或導出給其他模組使用，也可以將套件導出給特定模組使用

```java
module zoo.animal.shows {
    exports zoo.animal.shows.content;
    // others
}
```

```java
module zoo.animal.shows {
    exports zoo.animal.shows.content to zoo.staff;
    // 限制倒出對象只給zoo.staff模組
}
```

當export某一個套件時，該套件下所有public類別、介面、列舉型別的public/protected成員都將允許被其他模組使用。private和default層級的成員存取控制不受到模組化的影響。

| 存取層級  | 模組內部                               | 模組外部                                              |
| --------- | -------------------------------------- | ----------------------------------------------------- |
| private   | 只能讓同一類別內部的其他成員存取       | 無法存取                                              |
| default   | 只能讓同一套件內的其他類別存取         | 無法存取                                              |
| protected | 同一套件或具有繼承關係的子類別可以存取 | 如果exports套件，則套件內具備繼承關係的子類別可以存取 |
| public    | 不受限制                               | 如果exports套件則不受限制                             |

### 使用requires和requires transitive

`requires <module>`：當前模組依賴於指定的\<module>

`requires transitive <module>`：任何需要當前模組的其他模組也將依賴於\<module>

+ transitive - 可傳遞的，可理解為把這層依賴關係也傳遞出去

+ 常見於開源程式設計，例如B模組引用C模組，A模組要引用B模組時，也要引用模組C

  ```java
  module B {
      exports B;
      requires C;
  }    
  ```

  ```java
  module A {
      requires B;
      requires C;  //因為B有引用C,但A沒有直接引用C
  }
  ```

  改用requires transitive的話，可簡化為

  ```java
  module B {
      exports B;
      requires transitive C;
  }    
  ```

  ```java
  module A {
      requires B;
  }
  ```

#### 1. 修改模組`zoo.animal.feeding`

```java
module zoo.animal.feeding {
    exports zoo.animal.feeding;  
}
```

沒有變動

#### 2. 修改模組`zoo.animal.care`

```java
module zoo.animal.care {
    exports zoo.animal.care.medical;
    requires transitive zoo.animal.feeding;
    /*加上transitive之後，其他模組要引用care模組時，
      就不用另外再參照模組zoo.animal.feeding */
}
```

#### 3. 修改模組`zoo.animal.shows`

```java
module zoo.animal.shows {
    exports zoo.animal.shows.content to zoo.staff;
    exports zoo.animal.shows.media;
    exports zoo.animal.shows.schedule;
    requires transitive zoo.animal.care;
    /*不用再requires feeding, care模組引用也加上transitive*/
}
```

#### 4. 修改模組`zoo.staff`

```java
module zoo.staff {
    requires zoo.animal.shows;
    // 不用再requires care & shows module
}
```

完成以上步驟之後，都要重新編譯javac與打包jar指令，放置產出的模組JAR檔案到mods目錄，或者修改個別模組專案的module-info.java，並調整Eclipse Modulepath

> Little Tips☕🍪 模組資訊檔 module-info.java 內的敘述如果重複會如何?
>
> 對同一個套件的exports、requires敘述不允許重複，也不允許同一套件同時宣告requires以及requires transitive，否則會編譯失敗

### 使用provides、uses、opens

+ `uses`指令：用於指示該模組相依於一個服務(service)，通常是interface

  ```java
  module service.consumer {
      uses some.serviceApi;
  }
  ```

+ `provides`指令：用於指示該模組提供一個服務的實作(implementation)

  ```java
  module service.provider {
      provides some.serviceApi with some.serviceApiImpl;
  }
  ```

+ `opens`指令：和Java的映射技術(reflection)有關

  使用映射技術時，程式呼叫端在編譯時期不需要知道物件參考型別，但在執行時期依然可以執行指定的物件方法

Example

1. 映射技術的被呼叫端模組-`lab.reflection.provider`

   ```java
   package lab.reflection.provider.api;
   public class HelloWorld {
       public String getGreeting() {
           return "hi, greeting from lab.reflection.provider.api";
       }
   }
   ```

   ```java
   module lab.reflection.provider {
       exports lab.reflection.provider.api;
   }
   ```

2. 呼叫端模組-`lab.reflection.consumer`專案建立，設定模組相依關係

3. 建立專案模組資訊檔，宣告此專案依賴模組 `lab.reflection.provider`

   ```java
   module lab.reflection.consumer {
       requires lab.reflection.provider;
   }
   ```

4. 建立套件`lab.reflection.consumer.user`與類別`AccessByNormal`

   ```java
   package lab.reflection.consumer.user;
   import lab.reflection.provider.api.HelloWorld;//類別引用
   
   public class AccessByNormal {
       public static void main(String args[]) {
           try { //建立物件與物件參考
               HelloWorld om = new HelloWorld();
                 //呼叫物件參考的方法
               System.out.println(om.getGreeting());
           } catch (Throwable e) {
               e.printStackTrace();
           }
       }
   }
   ```

5. 建立映射技術的呼叫者類別`AccessByReflection`，匯入的`java.lang.relect.Method`用於映射技術

   ```java
   package lab.reflection.consumer.user;
   import java.lang.reflect.Method;
   public class AccessByReflection {
       public static void main(String args[]) {
           try {
               Class<?> c = Class.forName("lab.reflection.provider.api.HelloWorld");
               Method m = c.getMethod("getGreeting");
               System.out.println(m.invoke(c.getDeclaredConstructor().newInstance()));
           } catch (Exception e) {
               e.printStackTrace();
           }
       }
   }
   ```

> 以上範例未曾建立類別HelloWorld物件並呼叫方法`getGreeting()`，只有將類別名稱和方法名稱以字串表示
>
> 只要更換字串內容，就可以呼叫不同類別方法

6. 把**被呼叫端模組**的模組資訊檔由exports宣告改成opens

   ```java
   module lab.reflection.consumer {
       opens lab.reflection.provider;
   }
   ```

   > 使用宣告指令opens只開放**執行**時期使用
   >
   > exports則開放**編譯**和**執行**時期使用

---

## 在命令列 `command line` 使用模組指令選項

Java 9 開始，JDK內建類別也模組化了，可使用使令了解模組

### 使用java指令

`java`指令除了可以執行Java SE類的main()方法，還有模組相關選項

| java指令選項               | 作用                 |
| -------------------------- | -------------------- |
| `--describe-module`        | 描述模組內容         |
| `--list-modules`           | 列舉可用模組清單     |
| `--show-module-resolution` | 解析模組執行時的步驟 |

#### 1. 使用選項 --describe-module

除了解壓縮JAR檔案，並瀏覽`module-info.java`檔案，有一種更簡單的方法

```terminal
java -p mods --describe-module zoo.animal.feeding
```

> `--describe-module`可以使用`-d`簡化

```
zoo.animal.care file:///C:/java11/code/zoo.animal.care/mods/zoo.animal.care.jar

exports zoo.animal.care.medical
requires java.base mandated
requires zoo.animal.feeding transitive
contains zoo.animal.care.details
```

> 模組中沒有使用 exports 公開的套件，會使用contains宣告，表示提供模組**內部**使用

#### 2. 使用選項 --list-modules

除了描述模組，還可以使用java指令列出可用模組

```terminal
java --list-modules
```

會輸出Java內建所有模組以及其版本號的列表

如果指令中包含zoo專案的所有模組JAR檔案

```terminal
java -p mods --list-modules
```

就會列出Java內建所有模組+版本號 + 四個專案內模組的JAR檔案

#### 3. 使用選項 --show-module-resolution

這個選項可視為debug模組的一種手段，會執行模組、輸出過程、最後輸出執行結果

```terminal
java --show-module-resolution -p src -m zoo.animal.feeding/zoo.animal.feeding.Task
```

會列出根模組（root）、`java.base`模組所含的多行套件、所有具相依關係的模組、最後輸出指定類別`zoo.animal.feeding.Task`的執行結果

### 使用jar指令

jar指令也可以描述一個模組

```terminal
jar --file mods/zoo.animal.feeding.jar --describe-module
```

> `--file`選項也可以使用 `-f` 取代
>
> `--describe-module`選項也可以使用 `-d` 取代，描述模組JAR檔內容

```
zoo.animal.feeding jar:file:///C:/java11/code/zoo.animal.care/mods/zoo.animal.feeding.jar/!module-info.class
exports zoo.animal.feeding
requires java.base mandated
```

> `--describe-module`可以同時用於指令java和jar

### 使用jdeps指令

jdeps指令提供有關模組內依賴項目的資訊

相較於java或jar的選項`--describe-module`，jdeps指令除了檢視模組資訊檔之外，還查看程式碼，可以反映更詳實的結果

#### -summary

使用選項`-summary`提供模組JAR檔的依賴項目概略說明

```terminal
jdeps -summary mods/zoo.animal.feeding.jar
```

> `-summary`也可以使用`-s`簡化

Example_1

> ```terminal
> jdeps -summary mods/zoo.animal.feeding.jar
> zoo.animal.feeding -> java.base
> ```
>
> 以上結果 line#2 輸出顯示只有一個套件，並依賴於內建的java.base模組
>
> 如果沒用`-summary`則可以得到完整結果
>
> ```terminal
> jdeps mods/zoo.animal.feeding.jar
> ```

Example_2

檢視一個具備更複雜模組依賴關係的`zoo.animal.care`

```terminal
jdeps -summary --module-path mods mods/zoo.animal.care.jar
```

> 這裡的`--module-path`不能用`-m`或`-p`取代
>
> 結果如下
>
> ```terminal
> zoo.animal.care -> java.base
> zoo.animal.care -> zoo.animal.feeding
> ```

去除`-summary`改用完整模式下執行

```terminal
jdeps --module-path mods mods/zoo.animal.care.jar
```

> 結果如下
>
> ```
> zoo.animal.care 
>  [file:///C:/java11/code/zoo.staff/mods/zoo.animal.care.jar]
>  requires mandated java.base (@11.0.12)
>  requires transitive zoo.animal.feeding
> zoo.animal.care -> java.base
> zoo.animal.care -> zoo.animal.feeding
>  zoo.animal.care.details   -> java.lang    java.base
>  zoo.animal.care.details   -> zoo.animal.feeding  zoo.animal.feeding
>  zoo.animal.care.medical   -> java.lang         java.base
> ```
>
> ln#5-6 輸出結果等同於`-summary`選項結果
>
> ln#7-9 則輸出相依的套件與模組細節

#### --list-deps

可以列舉相依的模組，也會列出有使用到的JDK內部API

```terminal
jdeps --list-deps mods/zoo.animal.feeding.jar
```

> 輸出結果:
>
> ```
> java.base
> ```

相依關係較複雜的模組，指令:

```terminal
jdeps --list-deps --module-path mods mods/zoo.animal.care.jar
```

>輸出結果:
>
>```
>java.base
>zoo.animal.feeding
>```

### 使用jmod指令

Java 9 的JAR檔案提升為可以支援模組化JAR，為了封裝模組，引入了兩種新的檔案格式：`JMOD` 與 `JIMAGE`

只需要知道的基本概念

1. Oracle建議大多數開發模組任務依然使用JAR檔案，只有少數情形用JMOD

   [jmod api](https://docs.oracle.com/javase/9/tools/jmod.htm#JSWOR-GUID-0A0BDFF6-BE34-461B-86EF-AAC9A555E2AE)

   ![image-20230312222018283](https://i.imgur.com/eaM7y2d.png)

2. `jmod`指令只用於處理 JMOD 檔案

   | 選項     | 功能                             |
   | -------- | -------------------------------- |
   | create   | 新建JMOD檔案                     |
   | extract  | 由JMOD檔案中提取檔案，類似解壓縮 |
   | describe | 描述模組內容                     |
   | list     | 列出JMOD檔案中的檔案清單         |
   | hash     | JMOD檔案的雜湊字串               |

### 指令彙整

#### 1. 指令列操作比較表 

| 功能操作           | 範例或語法                                                   |
| ------------------ | ------------------------------------------------------------ |
| 編譯非模組化程式碼 | `javac -cp <lib/*> <src/lab/Test.java>`<br />`javac -cp <lib/*> -d <src/lab/Test.java>`<br />`javac --class-path <lib/*> -d <src/lab/Test.java>`<br />`javac -classpath <lib/*> -d <src/lab/Test.java>` |
| 執行非模組化程式碼 | `java -cp <./src;./lib/*> <lab.Test>`<br />`java -cp <./bin;./lib/*> <lab.Test>`<br />`java --class-path<./bin;./lib/*> <lab.Test>`<br />`java -classpath <./bin;./lib/*> <lab.Test>` |
| 編譯模組化程式碼   | `javac -p <mods> -d <src> <src/zoo/animal/feeding/*.java> <src/module-info.java>`<br />`javac --module-path <mods> -d <src> <src/zoo/animal/feeding/*.java> <src/module-info.java> ` |
| 執行模組化程式碼   | `java -p <mods> -m <zoo.animal.feeding/zoo.animal.feeding.Task>`<br />`java --module-path <mods> --module <zoo.animal.feeding/zoo.animal. feeding.Task> ` |
| 描述模組內容       | `java -p <mods> -d <zoo.animal.feeding> `<br />`java --module-path <mods> --describe-module <zoo.animal.feeding> `<br />`jar --file <mods/zoo.animal.feeding.jar> --describe-module `<br />`jar -f <mods/zoo.animal.feeding.jar> -d ` |
| 列舉模組清單       | `java --module-path mods --list-modules `<br />`java -p <mods> --list-modules`<br />`java --list-modules` |
| 檢視模組關聯       | `jdeps -summary --module-path <mods> <mods/zoo.animal.care.jar>` <br />`jdeps -s --module-path <mods> <mods/zoo.animal.care.jar>`<br />`jdeps --list-deps --module-path <mods> <mods/zoo.animal.care.jar>` |
| 解析模組執行步驟   | `java --show-module-resolution -p <src> -m <zoo.animal.feeding/zoo.animal.feeding.Task>`<br />`java --show-module-resolution --module-path <src> --module <zoo.animal.feeding/zoo.animal.feeding.Task>` |



#### 2. 指令javac常用選項列表

| 選項                                                         | 說明                          |
| ------------------------------------------------------------ | ----------------------------- |
| -cp \<classpath><br />-classpath \<classpath><br />--class-path \<classpath> | 非模組化程式指定JAR檔案位置   |
| -d \<dir>                                                    | 指定產生 *.class 的資料夾     |
| -p \<path><br />--module-path \<path>                        | 模組化程式指定模組JAR檔案路徑 |



#### 3. 指令java常用選項列表

| 選項                                  | 說明                        |
| ------------------------------------- | --------------------------- |
| -p \<path><br />--module-path \<path> | 模組化程式中指定JAR檔案路徑 |
| -m \<name><br />--module \<name>      | 指定要執行的模組名稱        |
| -d<br />--describe-module             | 描述模組內容                |
| --list-modules                        | 列舉模組清單但未執行模組    |
| --show-module-resolution              | 解析模組執行時步驟          |

#### 4. 指令jar常用選項列表

| 選項                      | 說明                          |
| ------------------------- | ----------------------------- |
| -c<br />--create          | 建立JAR檔案                   |
| -v<br />--verbose         | 執行JAR檔案時輸出細節         |
| -f<br />--file            | 指定JAR檔案名稱               |
| -C                        | 指定資料夾內的檔案要產生JAR檔 |
| -d<br />--describe-module | 描述模組內容                  |



#### 5. 指令jdeps常用選項列表

| 選項                  | 說明                                   |
| --------------------- | -------------------------------------- |
| --module-path \<path> | 模組化程式中指定JAR檔案路徑            |
| -s<br />-summary      | 輸出概括性描述                         |
| --list-deps           | 列舉相依模組，若使用JDK內部API也會列出 |



---

# 14 模組化應用程式

## 回顧模組指令

指令宣告（）接續上一章

| command                             | description                                                |
| ----------------------------------- | ---------------------------------------------------------- |
| exports \<package>                  | 允許所有模組存取*<套件>*                                   |
| exports \<package> to \<module>     | 允許特定*<模組>*存取*<套件>*                               |
| requires \<module>                  | 表示模組依賴於另一個*<模組>*                               |
| requires transitive \<module>       | 表示特定模組、和使用該模組的所有模組都依賴於另一個*<模組>* |
| uses \<interface>                   | 表示模組使用*<服務介面>*                                   |
| provides \<interface> with \<class> | 表示模組提供*<服務介面>*的*<實作>*                         |

## 比較模組類型

命名模組(named modules)：之前提的都是

自動模組(automatic modules)、未命名模組(unnamed modules)

>Little Tips🍪☕ 類別路徑 class path 和模組路徑 module path
>
>Java執行時期能夠使用**類別路徑**和**模組路徑**中的類別和介面型態，兩者規則差異如下
>
>1. Java程式可以依存取修飾詞（access modifiers, eg. public, protected）的定義，存取「**類別路徑**」裡的型態
>
>2. 「**模組路徑**」裡的public型態，跟類別路徑裡面的public型態不同，並非預設或者自動公開給其他程式存取
>
>   除了依循存取修飾詞定義，該型態還必須位於由定義它的模組所exports的套件中(公開給其他模組使用)，此外使用該型態的模組需要設定對要使用的模組的`requires`依賴關係

### 命名模組

命名模組：包含module-info.java檔案的模組，會與一個或多個套件一起出現在JAR檔案的根目錄下

一般談論模組時，預設就是指命名模組。命名模組應位於模組路徑(Modulepath)，而不是類別路徑上；如果模組檔案不在模組路徑上，將不被視為命名模組。

命名模組名稱定義在module-info.java裡面

##### Example - 命名模組的JAR檔案內容

```
Module Path
named.module.jar
some.package1
some.package2
module-info.class
```

### 自動模組

自動模組也出現在模組路徑上，但不包含module-info.java

它只是一個放在模組路徑上，並被視為模組的一般JAR檔案，Java會自動確定模組名稱

##### Example - 帶有兩個套件的自動模組

```
automatic.module.jar
some.package1
some.package2
```

#### `MANIFEST.MF`清單檔案

JAR檔是個帶有名稱為META-INF的特殊目錄的zip檔案

該目錄包含一個清單檔案`MANIFEST.MF`與其他檔案

```
-shirt
  -META-INF
  	-MANIFEST.MF
  Shirt.class
  ShirtTest.class
```

MANIFEST.MF帶有JAR檔案的相關資訊，檔案中每一行都是以冒號進行鍵值對的區隔

```manifest
Manifest-Version: 1.0
Created-By: 11.0.12 (Oracle Corporation)
Main-Class: ShirtTest
```

#### 自動模組的命名規則

如果MANIFEST.MF檔沒有設定Automatic-Module-Name屬性值，最終就以JAR的檔名作為模組名稱的參考

Example: 將`company-calender-1.0.0.jar`JAR檔案轉換成模組名稱

1. 移除副檔名`.jar`
2. 移除版本資訊，通常位於JAR檔案名稱末尾`-1.0.0`
3. 把`.`置換成 `-`
4. 把英數字以外的符號取代為`.`，重複/相鄰/位於開頭結尾的`.`都會被自動移除

| 以JAR檔名為基礎決定模組名稱                 | example_1                     | example_2      |
| ------------------------------------------- | ----------------------------- | -------------- |
| 0_原始JAR檔名                               | commons2-x-1.0.0-SNAPSHOT.jar | util_$-1.0.jar |
| 1_由JAR檔名中移除副檔名                     | commons2-x-1.0.0-SNAPSHOT     | util_$-1.0     |
| 2_由名稱末尾移除版本相關資訊                | commons2-x                    | util_$         |
| 3_用`.`取代除了英文字母和數字以外的其他字元 | commons2.x                    | util..         |
| 4_連續兩個以上的`.`字元只留一個             | commons2.x                    | util.          |
| 5_移除開頭結尾的`.`                         | commons2.x                    | util           |

> 還是會有少數JAR檔案不依慣例命名，e.g. `1.2.0-category-1.2.2-name-1.jar`

### 未命名模組

未命名模組跟自動模組一樣是一般的JAR檔案

但自動模組使用在模組路徑（Modulepath），未命名模組則用在類別路徑（Classpath），即未命名模組屬於遺留的舊程式碼（legacy）

未命名模組通常不含`module-info.java`

### 比較模組類型

| feature                             | 命名模組                             | 自動模組        | 未命名模組             |
| ----------------------------------- | ------------------------------------ | --------------- | ---------------------- |
| 包含module-info.java?               | YES                                  | NO              | NO<br />即便存在也忽略 |
| export套件到其他模組?               | 以module-info.java定義要export的套件 | exports所有套件 | 不會exports任何套件    |
| 可被位於模組路徑的其他模組檔案存取? | YES                                  | YES             | NO                     |
| 可被位於類別路徑的其他JAR檔案存取?  | YES                                  | YES             | YES                    |

#### 1. 命名模組

只有具備模組名稱的**命名模組**和**自動模組**才能被命名模組存取

###### 命名模組存取其他模組的結果

| 存取對象   | 結果 | 說明                                                         |
| ---------- | ---- | ------------------------------------------------------------ |
| 命名模組   | OK   | 同種類模組可以互相存取                                       |
| 自動模組   | OK   | 自動模組自動exports所有套件<br />有推導的模組名稱            |
| 未命名模組 | NG   | 未命名模組無法exports套件，沒有模組名稱，無法被命名模組存取<br />解法：將未命名模組的JAR檔案從類別路徑放到模組路徑 |

#### 2. 未命名模組

在類別路徑上的舊版本JAR檔案，從Java 9開始會被轉換成未命名模組

沒有模組資訊檔案、沒有模組名稱、無法匯出套件

可以被**未命名模組**或**自動模組**存取

###### 未命名模組存取其他模組的結果

| 存取對象   | 結果 | 說明                                                         |
| ---------- | ---- | ------------------------------------------------------------ |
| 命名群組   | OK   | 可以存取命名模組 exports 的套件<br />套件重複在不同模組時，以命名模組的套件優先 |
| 自動模組   | OK   | 皆為舊種類的JAR檔，只是放在不同的模組與類別路徑上<br />自動模組自動exports所有套件 |
| 未命名模組 | OK   | 同種類模組之間可以互相存取                                   |

#### 3. 自動模組

放在模組路徑上的未命名模組，就會被轉換成自動模組

自動模組沒有模組資訊檔案，但會自動匯出所有套件，可供命名模組和同類型的自動模組存取其套件

雖然未命名模組不能匯出任何套件，但因為舊JAR檔案相容性，自動模組仍然可以存取未命名模組。命名模組則無法存取未命名模組。

###### 自動模組存取其他模組的結果

| 存取對象   | 結果 | 說明                                                         |
| ---------- | ---- | ------------------------------------------------------------ |
| 命名模組   | OK   | 可以存取命名模組exports的套件<br />套件重複在不同模組時，以命名模組的套件為優先 |
| 自動模組   | OK   | 同種類模組之間可互相存取                                     |
| 未命名模組 | OK   | 都是舊種類JAR檔案，只是放在不同路徑上                        |

---

## 分析 JDK 依賴關係

### 識別內建模組

因為 `java.base` 模組的基礎性如同 `java.lang` 套件，故預設使用，不需要特別以 requires 指令宣告（有宣告也不會有問題）

###### 常用模組

| 模組名稱     | 包含內容                                  | isDiscussed |
| ------------ | ----------------------------------------- | ----------- |
| java.base    | Collections, Math, IO, NIO.2, Concurrency | Yes         |
| java.desktop | Abstract Windows, Toolkit(AWT), Swing     | X           |
| java.logging | Logging                                   | X           |
| java.sql     | JDBC                                      | Yes         |
| java.xml     | Extensible Markup Language(XML)           | X           |

###### java.* 內建模組 - 開發程式相關

| -                   | -                  | -                   |
| ------------------- | ------------------ | ------------------- |
| java.base           | java.naming        | java.smartcardio    |
| java.compiler       | java.net.http      | java.sql            |
| java.datatransfer   | java.prefs         | java.sql.rowset     |
| java.desktop        | java.rmi           | java.transaction.xa |
| java.instrument     | java.scripting     | java.xml            |
| java.logging        | java.se            | java.xml.crypto     |
| java.management     | java.security.jgss |                     |
| java.management.rmi | java.security.sasl |                     |

###### jdk.* 內建模組 - JDK功能相關

(略)

### 使用 JDEPS <sub>(用於識別模組依賴關係)</sub>

`jar -cvf mods/zoo.legacy.jar`

`jdeps mods/zoo.legacy.jar`

+ 一般模式

`jdeps -s mods/zoo.legacy.jar`

+ 摘要模式

`jdeps --jdk-internals mods/zoo.legacy.jar`

+ `--jdk-internals`選項可以提供和JDK內部API關聯的詳細資訊，並提供開發參考資訊

> Little Tips☕🍪 **sun.misc.Unsafe**
>
> 這命名是昇陽公司為了避免在JDK開源程式碼以外的地方使用他
>
> jdeps指令可幫助查看當甲骨文最後停用此類別時，是否會遇到任何問題
>
> ```java
> import java.time.LocalDate;
> import java.util.List;
> import sun.misc.Unsafe;
> 
> public class UnsafeBean {
>     
>     private List<String> list;
>     private LocalDate date;
>     
>     public UnsafeBean(List<String> list, LocalDate date) {
>         this.list = list;
>         this.date = date;
>     }
>     
>     public void unsafeMethod() {
>         Unsafe unsafe = Unsafe.getUnsafe();
>     }
> }
> ```

---

## 模組化既有應用程式

### 確定函式庫相依順序



### 使用由下而上的模組化策略

最簡單的模組化方法「由下而上」移轉，如下步驟

1. 選擇尚未移轉的關聯圖內**最低層**JAR專案，優先模組化
2. 在該JAR專案新增一個module-info.java檔案(未命名->命名模組)
   + 使用exports匯出需要給較高層別JAR檔用的套件
   + 使用requires新增依賴的套件(由較低層別的JAR提供)
3. 將新移轉的命名模組從Classpath搬到Modulepath
4. 確保尚未移轉的JAR專案在Classpath中，保留為未命名模組
5. 反覆模組化直到完成

### 使用由上而下的模組化策略

使用情境：當無法對應用程式每個JAR專案都有掌控能力時，使用由上而下的模組化策略比較有用

步驟：

1. 把所有JAR檔案都搬到模組路徑Modulepath，變成自動模組
2. 從最高層別JAR專案優先進行模組化
3. 新增 module-info.java 到該專案(自動模組->命名模組)
   + 編寫exports指令
   + requires指令可以用自動模組名稱
4. 反覆模組化直到完成

### 解構與模組化單體應用程式

模組關係規劃有一個關鍵原則「模組系統不允許循環依賴」，即兩個模組不可以間接/直接地相互依賴，避免執行時無窮迴圈

+ 即module.A requires module.B時，module.B不能同時requires module.A

常見解決方式是在兩個模組間再建立第三個模組，抽出其他兩個模組共用的程式碼，排除循環依賴的問題

### 循環的相依性導致無法編譯

在yyy.module 的 module-info.java 撰寫 requires xxx.module -> 右鍵 yyy.module 專案 -> Properties -> Java Build Path -> Projects -> Modulepath [Add] -> 加入 xxx.module [打勾] -> yyy.module 的模組資訊檔通過編譯

#### 驗證兩個模組的依賴關係出現循環現象將編譯失敗

yyy.module 跟 xxx.module 互相依賴的話，會提示錯誤訊息警告模組間的依賴關係出現循環

```
Cycle exists in module dependencies, Module yyy.module requires itself via xxx.module
```

---

## 模組化 Java 服務結構

> ⚠️這一段筆記寫的很差，有心複習的話要找原文本⚠️

服務由以下三者組成

1. 服務提供者的介面 Service Provider Interface (SPI)
2. 該介面引用的類別
3. 取得該介面實作的機制，即「服務定位器」(service locator)

SPI常見的應用

1. Java Database Connectivity
2. Java Cryptography Extension
3. Java Naming and Directory Interface
4. Java API for XML Processing
5. Java Business Integration
6. Java Sound
7. Java Image I/O
8. Java File System

#### ServiceLoader\<S> 的 load() 方法

```java
public final class ServiceLoader<S> implements Iterable<S> {
    public static <S> ServiceLoader<S> load(Class<S> service) {
        // implementation content
    }
    // other methods
}
```

> 使用SPI架構提供服務
>
> 只要將服務提供者介面的型態傳遞給它的`load()`方法，即可回傳找到的服務實作
>
> 實務上使用ServiceLoader的成本相對比較高，建議把搜尋結果快取在記憶體內
>
> ```java
> public class TourFinder {
>     public static Tour findTour() {
>         ServiceLoader<Tour> loader = ServiceLoader.load(Tour.class);
>         for (Tour tour : loader) {
>             return tour;
>         }
>         return null;
>     }
>     public static List<Tour> findAllTours() {
>         ServiceLoader<Tour> loader = ServiceLoader.load(Tour.class);
>         for (Tour tour : loader) {
>             tours.add(tour);
>         }
>         return tours;
>     }
> }
> ```

建立服務提供者介面實作的模組時，模組資訊檔這樣寫

```java
module travel.agency {
    requires travel.api;
    provides travel.api.Tour with travel.agency.TourImpl;
 /* ⭐沒有直接exports完成後的服務實作，
       改用指令provides指示該模組提供一個滿足服務介面travel.api.Tour
       的實作travel.agency.TourImpl，
       以藉由服務定位器找出服務實作
 */
}
```

#### ServiceLoader\<S> 的 stream() 方法

stream()方法回傳`Stream<Provider<S>>`物件，Provider是ServiceLoader的內部靜態介面，同時有提供它的內部靜態實作 ProviderImpl

取得 ProviderImpl之後，可再呼叫 get() 取得服務的實作

```java
module travel.mix {
    requires travel.api;
    uses travel.api.Tour; // TourImpl2 實作介面Tour
    provides travel.api.Tour with travel.mix.TourImpl2;
}
```

#### Conclusion

| 分類                                           | 服務一部分 | 模組專案            | module-info keyword                   |
| ---------------------------------------------- | ---------- | ------------------- | ------------------------------------- |
| Service Provider Interface <br/>服務提供者介面 | YES        | travel.api          | exports                               |
| Service Locator<br/>服務定位器                 | YES        | travel.reservations | requires<br />exports<br />uses       |
| Service Provider<br/>服務提供者(實作)          | NO         | travel.agency       | requires<br />provides with           |
| Consumer<br/>服務使用者                        | NO         | travel.buyer        | requires                              |
| 服務定位器<br/>服務提供者(實作)<br/>服務使用者 | N/A        | travel.mix          | requires<br />uses<br />provides with |

---

# 15 開發安全的 Java 程式

## 設計安全物件

保護物件免受駭客攻擊的方法：存取控制、可繼承性、驗證和建立不可改變物件

### (1) 限制可存取性

設計類別時，應該使用「最小權限（least privilege）」原則，建議改為private，且設計安全的存取方式

+ private > default > public

  ```java
  public class PasswdManagerV2 {
      private Map<String, String> passwdRepo;
      public boolean is PasswdValid(String account, String passwd) {
          var pwd = passwdRepo.get(account);
          return passwd.equals(pwd);
      }
  }
  ```

如果程式本身使用Java模組系統，又需要exports模組，也應該exports套件給需要此套件的模組

```java
module some.security {
    exports some.security to zoo.staff;
    // 表示只有 zoo.staff 可以使用 some.security 套件內的 public 類別
}
```

### (2) 限制可繼承性

延續上個存取授權漏洞，駭客可能改用反覆試驗（trial and error）方法逐一破解帳號密碼，作法是建立PasswordManagerV2的惡意子類別

```java
public class MaliciousPasswdManager extends PasswdManagerV2 {
    @Override
    public boolean is PasswdValid(String account, String passwd) {
        var valid = super.isPasswdValid(account, passwd);
        if (valid) {
            // email the passwd to Hacker
        }
        return valid;
    }
}
```

只要將機敏類別宣告為 final，就可以防止被繼承

```java
public final class PasswdManagerV3 {
    private Map<String, String> passwdRepo;
    public boolean isPasswdValid(String account, String passwd) {
        var pwd = passwdRepo.get(account);
        return passwd.equals(pwd);
    }
}
```

### (3) 建立不可更改（immutable）物件

建立不可更改物件有助於編寫安全程式碼，原因：不用擔心值發生變化、被竄改、處理多執行緒時還簡化了程式碼。

不可更改物件像是：String, Path, List.of(), Set.of(), Map.of()回傳的物件

不可更改物件的寫法

1. 類別宣告為final

   阻止任何人建立可更改的子類別

2. 所有實例化變數為private

   提供良好封裝

3. 不定義任何setter方法，欄位宣告final

   確保類別使用者和本身不會更改實例變數

4. 不允許類別參照到的其他物件被修改

   可能不能定義getter方法，如下類別不能算immutable

   ```java
   public final class AnimalV1 {
       private final List<String> foods;
       public AnimalV1 {
           this.foods = new ArrayList<>();
           this.foods.add("Apples");
       }
       public List<String> getFoods() {
           return foods;
       }
   }
   ```

   原因如下，如果能改變其物件內容狀態，就不是immutable物件

   ```JAVA
   AnimalV1 a = new AnimalV1();
   a.getFoods().clear();        // 🚧
   a.getFoods().add("poison");  // 🚧
   ```

   如果沒提供getter方法，可以藉由方法委派(method delegation)減少提供的資料

   ```java
   public final AnimalV2 {
       private final List<String> foods;
       public AnimalV2() {
           this.foods = new ArrayList<>();
           this.foods.add("Apples");
       }
       public int getFoodsCound() {
           return foods.size();
       }
       public String getFoodsElement(int index) {
           return foods.get(index);
       }
   }
   ```

   除了方法委派，還能透過回傳**副本**的方式，使駭客只能竄改副本

   ```java
   public final class AnimalV3 {
       private final List<String> foods;
       public AnimalV3() {
           this.foods = new ArrayList<>();
           this.foods.add("Apples");
       }
       public List<String> getFoods() {
           return List.copyOf(this.foods);
       }
   }
   ```

5. 使用建構子設定物件所有屬性，需要時，可將傳入的物件參考予以複製，以避免違反前述原則

   ```java
   public final class AnimalV4 {
       private final List<String> foods;
       public AnimalV4(List<String> foods) {
           if (foods == null)
               throw new RuntimeException("food is required");
           this.foods = foods;
       }
       public int getFoodsCount() {
           return foods.size();
       }
       public String getFoodElement(int index) {
           return foods.get(index);
       }
   }
   ```

   如下`.clear()`使其不再為immutable類別

   ```java
   var favorites = new ArrayList<String>();
   favorties.add("Apples");
   var animal = new AnimalV4(favorites);
   System.out.println(animal.getFoodsCount());
   favorites.clear();
   System.out.println(animal.getFoodsCound());
   ```

   解決方法：建構子傳入物件後立即複製，稱為**防禦性複製（defensive copy）**

   ```java
   public AnimalV4(List<String> foods) {
       if (foods == null)
           throw new RuntimeException("food is required");
    // this.foods = fooods;
       this.foods = new ArrayList<String>(foods);
   }
   ```

### 複製（clone）物件

#### 介面Cloneable

Cloneable interface 跟介面 Serializable 都是標記型介面，標記類別產生的物件是否具備複製的能力

```java
package java.lang;
public interface Cloneable {
}
```

#### 類別Object的clone()方法

使用clone()會回傳Object型態的物件，還需要給他轉型(casting)

1. 一般來說
   + 任何物件x的表達式`x.clone() != x`執行結果為true 
   + `x.clone().getClass() == x.getClass()` 也為true
   + `x.clone().equals(x)`



##### 複製流程

```
 （myObject.clone()）
          |
          ↓
<implements Clonable?> ——No--> [throws CloneNotSupportedException]
          |
          | Yes
          ↓
[overrides clone()?] ——No--> [shallow copy]
          |
          | Yes
          ↓
     [deep copy]
  *implementation 
     dependent*
```

#### 使用clone()方法進行防禦性複製

final 類別實作介面Cloneable，並覆寫 clone() 方法

```java
public final class AnimalV5 implements Cloneable {
    private final List<String> foods;
    
    public AnimalV5(List<String> foods) {
        if (foods == null)
            throw new RuntimeException("food is required");
        this.foods = foods;
    }
    public List<String> getFoods() {
        return foods;
    }
    @Override
    protected Object clone() throws CloneNotSupportedException {
        return super.clone();
    }
    public static void main(String[] args) throws Exception {
        List<String> food = new ArrayList<>();
        food.add("grass");
        AnimalV5 animal = new AnimalV5(food);
        AnimalV5 clone = (Animalv5) animal.clone();
        System.out.println(animal == clone); // FALSE
        System.out.println(animal.getFoods() == clone.getFoods()); // TRUE
    }
}
```

###### 淺層複製

+ 欄位資料複製後的物件參考依然指向原物件欄位的記憶體位置

###### 深層複製

+ 修改`clone()`實作

+ 建立一個新的 ArrayList 物件

+ 所以更改複製物件的欄位/狀態時，不會影響原始物件

  ```java
  @Override
  public AnimalV5 clone() throws CloneNotSupportedException {
      if (foods instanceof ArrayList) {
          List<String> cloned = (List) ((ArrayList) foods).clone();
          return new AnimalV5(cloned);
      } else {
          throw new CloneNotSupportedException();
      }
  }
  ```

---

## 注入 *injection* 攻擊與輸入 *input* 驗證

漏洞利用（exploit）: 利用安全性弱點所進行的攻擊。

只要不是直接由自己產生的資料，都應該被視為可疑資料而進行輸入驗證，例如使用者輸入、從檔案中讀取、從資料庫中查詢取得。

#### 使用 `PreparedStatement` 避免 SQL 注入攻擊

#### [不安全] 使用Statement物件查詢

```java
public int getOpening1(Connection conn, String day) throws SQLException {
    String sql = "SELECT opents FROM hours WHERE day ='" + day + "'";
    try (var stmt = conn.createStatement(); 
            var rs = stmt.executeQuery(sql)) {
        if (rs.next())
            return rs.getInt("opens");
    }
    return -1;
}
```

Hacker input:

```java
// malicious statement
String maliciousInput = "monday' OR day IS NOT NULL OR day = 'sunday";
```

> 使用開發者不預期的查詢條件，暴露更多資料 (SQL injection攻擊手法)



#### 使用PreparedStatement物件查詢

> 使用PreparedStatement時，必須一併使用綁定變數(binding variable)，否則無效

```java
public static int getOpening2(Connection conn, String day) throws SQLException {
    String sql = "SELECT opens FROM hours WHERE day = '" + day + "'";
    try (var stmt = conn.prepareStatement(sql);
         var rs = stmt.executeQuery()) {
        if (rs.next())
            return rs.getInt("opens");
    }
    return -1;
    // not safe enough, incorrect prepareStatement
}

public static int getOpening3(Connection conn, String day) throws SQLException {
    String sql = "SELECT opens FROM hours WHERE day = ?";
    try (var ps = conn.prepareStatement(sql)) {
        ps.setString(1, day);
        try (var rs = ps.executeQuery()) {
            if (rs.next())
                return rs.getInt("opens");
        }
    }
    return -1;
}
```



### 使用輸入驗證（Input Validation）過濾無效輸入

注入攻擊類型: (1) SQL注入攻擊、(2) 指令注入_command injection

##### 指令注入

  利用不周延程式設計，駭客透過輸入 `..\private` 取得機敏的log檔案（位於private目錄）

##### 解決方式

  採取輸入驗證（input validation），在程式中指定允許存取的**白名單**

```java
private static void invulnerable(String dirName) throws IOException {
    if (!(dirName.equalsIgnoreCase() || 
          dirName.equalsIgnoreCase("module2"))) {
        System.out.println("=== illegal input & terminate! ===");
        System.exit(0);
    }
    Path path = Paths.get("lab/log/common/").resolve(dirName);
    try (Stream<Path> stream = Files.walk(path)) {
        Stream
            .filter(p -> p.toString().endsWith(".txt"))
            .forEach(System.out::println);
    }
}
```

> Little Tips☕🍪 安全性政策的**白名單**、**黑名單**
>
> 黑名單: 
>
> + 不允許事項的列表
> + 提出關鍵在於開發者必須比駭客更清楚攻擊手法並防患未然
>
> 白名單:
>
> + 允許的事項列表
> + 不需要預判所有可能的安全漏洞與攻擊手法，安全性較優
> + 但可能會經常更動白名單內容，維護成本較高

## 處理機敏資訊

系統常見機敏資訊

| Category                                                     | Items                                                        |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 登入資訊                                                     | 使用者帳號、<br />使用者密碼、<br />使用者密碼雜湊(hash)     |
| （帳務）付款                                                 | 信用卡號碼、<br />存款餘額、<br />信用評分(credit score)     |
| 個人識別資訊（PII, <br />personal identifiable information） | 社會安全碼(social security number)、身分證字號、<br />母親婚前姓氏(mother's maiden name)提示安全問題與答案 |

### 保護機敏資料的輸出

首先要避免將機敏資訊放入`toString()`方法中，確保資訊不會記錄在不預期出現的地方

以下地方也要注意有無機敏資訊接露

1. log 日誌記錄檔案
2. 程式異常輸出的Exception或者其軌跡堆疊stack trace
3. System.out與System.err輸出資訊
4. 程式有寫入資料的檔案

有時候專案會有揭露機敏資訊的必要，記得只處理使用者需求項目即可，別擅自擴大



### 保護記憶體中的資料

記憶體內的緩衝快取（buffer cache）也要注意有無儲存機敏資訊

Example

讀取密碼 `readPassword()`方法回傳char[]而不是String的兩個安全性考量

1. String儲存的話，會被Java放在字串池中，程式碼執行結束，密碼字串依然在記憶體中
2. char[]的話，使用完畢後可以用`Arrays.fill()`將陣列元素覆蓋為其他值，不用等GC機制

```java
Console cons = System.console();
boolean userValid = false;
char[] ans = new char[] { 'p', 'a', 's', 's', 'w', '@', 'r', 'd'};
if (cons != null) {
	do {
		String account = cons.readLine("%s", "Input account: ");
        char[] password = cons.readPassword("%s", "Input password: ");
        if (account.equals("jim") && Arrays.equals(password, ans)) {
            					//比較正確密碼與使用者輸入密碼是否相同
            System.out.println("Correct! System quits!");
			userValid = true;
			Arrays.fill(password, 'x');
			//將陣列成員全部以字元'x'覆蓋
			ans = null;
            //將陣列指向null
			// System.out.println(password);
		} else {
			System.out.println("Wrong! Try again!\n");
		}
	} while (!userValid);
}
```

## 序列化與反序列化物件

是經常用於系統函式庫或框架的底層技術，透過網路傳資料、儲存資料到資料庫/硬碟、呼叫遠端程式(RPC、RMI)都會用到序列化技術

反序列化資安漏洞 OWASP資安組織也列為前十大熱門攻擊手法之一

可藉由指定序列化欄位以及控制序列化本身的過程，使序列化更安全

### 指定序列化的物件欄位

+ 方式一: 以`transient`宣告可以避免機敏資訊被序列化

  ```java
  private transient int age;
  ```

+ 方式二: 宣告一個靜態類別常數，並以陣列成員指定要序列化的欄位

  ```java
  private static final ObjectStreamField[] serialPersistentFields = { new ObjectStreamField("name", String.class) };
  ```

> transient - 欄位宣告避免序列化(黑名單)
>
> ObjectStreamField[] - 陣列欄位列舉要序列化的欄位(白名單)

### 客製序列化流程

Example

```java
public class Employee implements Serializable {
    private static final long serialVersionUID = 1L;
    private int age;
    private String name;
    private String ssn;
    // 略: constructor with fields
    
    private static final ObjectStreamField[] serialPersistentFields = {
        new ObjectStreamField("name", String.class),
        new ObjectStreamField("ssn", String.class)
    }; 
    
    private static String encrypt(String input) {
        return input;
    }
    private static String decrypt(String input) {
        return input;
    }
    // 由物件序列化為檔案
    private void writeObject(ObjectOutputStream s) throws Exception {
        ObjectOutputStream.PutField fields = s.putFields();
        fields.put("name", name);
        fields.put("ssn", encrypt(ssn));
        s.writeFields();
    }
    
    // 檔案反序列化為物件
    private void readObject(ObjectInputStream s) throws Exception {
        ObjectInputStream.GetField fields = s.readFields();
        this.name = (String) fields.get("name", null);
        this.ssn = decrypt((String) fields.get("ssn", null));
    }
    // 略: toString()
}
```





### 改變序列化與反序列化的結果



#### 使用`readResolve()`方法改變反序列化還原物件的結果



#### 使用 `writeReplace()` 方法改變序列化寫入檔案的內容



#### 彙整序列化與反序列化的相關方法

| order | return | name             | param              | goal                                 |
| ----- | ------ | ---------------- | ------------------ | ------------------------------------ |
| 1     | Object | `writeReplace()` | 無                 | 發生在序列化之前，可改變原始物件     |
| 2     | void   | `writeObject()`  | ObjectInputStream  | 使用PutField選擇序列化欄位           |
| 3     | void   | `readObject()`   | ObjectOutputStream | 反序列化時使用GetField取出欄位       |
| 4     | Object | `readResolve()`  | 無                 | 發生在反序列化之後，可改變復原的物件 |

## 建立保護機敏資料的安全物件

## 避免服務阻斷 *denial of service* 攻擊

---

# 16 Mock exam & analysis









