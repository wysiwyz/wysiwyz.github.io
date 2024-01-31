---
title: "1Z0-819 曾師筆記 (1/duology)"
date: 2023-08-05T12:10:22+08:00
author: "celine"
tags: ["Java", "1Z0-819"]
categories: ["StudyNote"]
---

## # 03 OOP 程式設計思維

## 統一塑模語言 Unified Modeling Language

### 類別圖 class diagram

![image-20221210101243322](https://i.imgur.com/Ifdkbbm.png)

### Has-A 關聯

1. 關聯 Dependency : 兩者關係一般，有功能上的連結
2. 聚合 Aggregation : 兩者關係較強， ClassD 可能是 ClassC 的一部分
   1. 像沙子集中一處，但也可以另外集結
3. 組合 Composition : 類似聚合，但關係更強。一旦 ClassE 消失，ClassF 也消失，兩者生命週期一致
   1. 關係比較密實

![image-20221210102539135](https://i.imgur.com/2gtLevi.png)

### Is-A 關聯

1. 繼承 Inheritance : 箭頭方向指向父類別，使用實線
2. 具現 Realization : 箭頭方向指向介面，使用虛線

---

# 01 使用 java 指令執行程式

該檔案位於 Java 安裝目錄的 bin 資料夾內，要執行的類別必須有 main 方法作為程式進入點

```terminal
java JacketTest   // java 程式編譯檔(不能加 class 的副檔名)
```

注意：

1. 程式碼附檔名必須為 `.java`，否則編譯器將拒絕編譯
2. class 檔案內可以有多個沒有 public 修飾的 class，檔案名稱不一定要與那些 classes 名稱相同。編譯時，檔案內宣告的每一個 class 都會各自產生附檔名為 `.class` 的編譯檔
3. 一個程式碼檔案內只允許有一個 public class，且檔名必須與該 public class 的名稱相同

---

### 使用 jar 指令打包程式後再執行

1. JAR - Java ARchive 縮寫，是一個 ZIP 壓縮檔

2. 使用 Java 內建指令 `jar.exe` 製作 JAR 檔會比使用 7-ZIP 更有效率

   1. 和 `javac.exe`、`java.exe` 一樣都放在 Java 安裝目錄的 bin 資料夾內

   2. 指令語法分成五段：

      ```terminal
      jar -cfe {JAR檔案} {具備main()方法的類別} {程式編譯檔}
      ```

      1. **jar 指令**
      2. **指令選項**
         1. c - create，建立新 JAR 檔案
         2. f - file，選項後要提供 JAR 檔名稱
         3. e - entry point，程式進入點，要指定具備 main() 方法的類別名稱
      3. **要產生的 JAR 檔案名稱**
      4. **具備 main() 方法的類別名稱**
      5. **程式編譯檔**，即 `*.class`。因此在使用 jar 指令前，要先完成編譯

   3. 若要把 ShirtTest.class 和 Shirt.class 打包成 shirt.jar 可用以下指令：

      ```terminal
      jar -cfe shirt.jar ShirtTest *.class
      ```

   4. 或者清楚指定 `*.class` 包含哪些檔案

      ```terminal
      jar -cfe shirt.jar ShirtTest Shirt.class ShirtTest.class
      ```

   5. 打包完後，可用以下指令執行 `shirt.jar` 檔案

      ```terminal
      java -jar shirt.jar
      ```

   6. 會得到與 `java ShirtTest` 一樣的結果

---

# 05 變數與 Java 基本型別

## Java 基本型別 (primitive data type)

| 類型                       | 型別    | 位元組<br />(bytes) | 位元數<br />(bits) | 最小值          | 最大值            |
| -------------------------- | ------- | ------------------- | ------------------ | --------------- | ----------------- |
| 整數                       | byte    | 1                   | 8                  | -128            | 127               |
| Integral                   | short   | 2                   | 16                 | -2<sup>15</sup> | 2<sup>15</sup> -1 |
| ---                        | int     | 4                   | 32                 | -2<sup>31</sup> | 2<sup>31</sup> -1 |
| ---                        | long    | 8                   | 64                 | -2<sup>63</sup> | 2<sup>63</sup> -1 |
| 浮點數<br />Floating point | float   | 單精確度            | 32-bit 浮點數      | 依IEEE 754標準  | 依IEEE 754標準    |
| ---                        | double  | 雙精確度            | 64-bit 浮點數      | 依IEEE 754標準  | 依IEEE 754標準    |
| 字元<br />Textual          | char    | 2                   | 16                 | `\u0000` -      | `\uffff`          |
| 布林值<br />Logical        | boolean | 1                   | 8                  | true, false     | ---               |



### 字面常量

直接寫下 「1、1.0、3.14159、'T'」而未經變數宣告與初始化，稱之為**字面常量 / 常數**（literal constant），可以是：

1. 字元
2. 字串
3. 整數 : 預設 int，若是 `90l`或者`90L`則為 long 型別
4. 浮點數 : 預設 double，若是 `0.01f`或者`0.01F` 則為 float 型別
5. 符號
6. true/false

### 變數的有效範圍

1. 實例變數 instance variable，即類別屬性或欄位 field :

   + 有效範圍在整個物件實例 instance 內

   + 宣告型態前可以有修飾詞 (eg. public)

   + 使用前若未給值或初始會，Java 會給以下預設值 :

     | 型別分類 | 基本型別               | 預設值                                |
     | -------- | ---------------------- | ------------------------------------- |
     | 整數     | byte, short, int, long | 0                                     |
     | 浮點數   | float, double          | 0.0                                   |
     | 字元     | char                   | 空字元, <br />Unicode 表達為 `\u0000` |
     | 邏輯     | boolean                | false                                 |

2. 區域變數 local variable

   1. 有效翻為在宣告方法或特定程式碼區塊 `{}` 內
   2. 宣告型態前不能再有修飾詞等
   3. 使用前若未給值 (==未初始化)，將編譯失敗

🍪☕ Little Tips:

Java 的變數在使用前無論實例變數或區域變數都「必須有值」，差別：

+ **實例變數**：若沒先給值，Java 會給預設值，可通過編譯
+ **區域變數**：若沒先給值，就是沒值，無法通過編譯

### 使用二進位的字面常量的方式顯示整數

+ 若數值以 `0b/0B` 開頭，表示將以二進位的書寫方式表現數字，例如 `0b101011`

### 使用底線提高數字常量的可讀性

+ 以 `_` 區隔數字，增加數字常量 (numeric literals) 的可讀性

## 常用運算子

+ 算數運算子 : + - * / %
+ 簡潔運算子 : `+=` `-=` `*=` `/=` `%=`
+ 遞增 / 遞減運算子 : `i++` `i--` `++i` `--i`
  + `i++` : 先執行整個敘述之後，再將 i 的值加 1 (先賦值再+1)
  + `++i` : 先將 i 的值加 1，再執行整個數 (先+1再賦值)
+ 順序
  + `括號內 ()` > `前置遞增遞減` > `乘除算數` > `加減算術` > `關係` > `條件` > `三元` > `指派` > `後置遞增遞減`

```java
int count = 20;
int a, b, c, d;
a = count++;   // a = 20 | count = 21
b = count;     // b = 21
c = ++count;   // c = 22 | count = 22
d = count + 1; // d = 23 
System.out.println(a + b + c + d);                // 86 
System.out.println("Result=" + a + b + c + d);    // Result=20212223
System.out.println(a + b + "Result" + c + d);     // 41Result=2223
System.out.println("Result=" + a + (b + c) + d);  // Result=204323
```

## 型別升等與轉型

### 型別自動升等 automatic promotion

1. 運算式內成員型別不一致時，小型別自動提升型別，使其與大型別變數的型別一致
2. 小型別的值指定給大型別時，自動提升小型別值的型別，滿足大型別宣告要求
3. 整數型態 (byte / short / int / long) 指定給浮點數 (float / double) 時，會自動提升型態為浮點數

```JAVA
/* Automatic Promotion for assigning small to large */
byte x1 = 10;
short x2 = x1;
int y1 = 10;
long y2 = y1;

float z1 = 10.0f;
double z2 = z1;

/* Automatic Promotion for assigning integer to floating point */
float e = 2;
double f = 2;
float g = 2L;
double h = 2L;

/* Can not assign floating point to integer! 
   It will cause the lost of decimal point! */
int a = 2.34f;  // compilation error
long b = 2.34f; // compilation error
int c = 2.34;   // compilation error
long d = 2.34;  // compilation error
```

### 型別轉型 casting

在數值前加一個 `()`，裡面決定要轉型的目標。例如：`(target_type) value`

通常用於將大型別轉成小型別，可以減少記憶體的使用，但是要注意型別是否和先前相同

```java
/* example 1: cast to bigger type to get 3.333 ... */
int number = 10;
System.out.println((double)number / 3); 

/* example 2 */
int i1 = 53;
int i2 = 47;
byte b3;

b3 = i1 + i2;  
// compilation error - can't assign 4 bytes int to 1 byte byte

b3 = (byte) (i1 + i2);
System.out.println(b3);
```

---

### 暫存空間對算數運算子的影響

+ 算數運算時，Java 會先將指派運算子 `=` 右側的「運算過程及結果」放在「暫存空見」後，才會丟給指派運算子左側的變數

+ 該運算子空間最小為 `int`(4 bytes) 的大小、最大為參與計算中最大的資料型別

  ```java
  int x = 3 * 4;
  System.out.println(x);  // =12
  
  // 55555 * 66666 = 3703629630
  int a = 55555 * 66666;
  System.out.println(a);    // -591337666 tempSpace overflow
  
  long b = 55555 * 66666;
  System.out.println(b);    // 將溢位的結果指定給 long 型別的變數 b
  
  long c = (long) (55555 * 66666);
  System.out.println(c);    // 將溢位的結果轉型成 long 不影響輸出結果
  
  long d = ((long) 55555) * 66666;
  System.out.println(d);    // correct
  ```

---

# 06 參考型別操作物件與 var 宣告

## 使用物件參考

### 由類別建構物件

1. 宣告 declaration

   `Classname reference` `Shirt myShirt;` {類別名稱}{空格}{物件參考變數}

2. 實例化 instantiation

   `new Classname;` `new Shirt();`

3. 將實例指定給物件參考，完成初始化 initialization

   `reference = new Classname();` `myShirt = new Shirt();`

> 取得物件參考 (object reference) 後，等同取得遙控器，可控制實例化時在記憶體產生的物件
>
> ```java
> int shirtId = myShirt.shirtId;
> myShirt.display();
> ```

|              | 型別    | 變數      | 指派運算子 | 記憶體內的實例 |
| ------------ | ------- | --------- | ---------- | -------------- |
| 基本型別範例 | `int`   | `x`       | `=`        | `10`           |
| 參考型別範例 | `Shirt` | `myShirt` | `=`        | `new Shirt()`  |

### 不同物件，使用不同物件參考

```java
Shirt myShirt1 = new Shirt();
myShirt1.display();

Shirt myShirt2 = new Shirt();
myShirt2.display();

Trousers myTrousers = new Trousers();
myTrousers.display();
```

+ 多型 : 一種遙控器可以控制同種但不同品牌的電子產品

### 物件參考與 null

+ 當使用空的遙控器時不會有問題，但呼叫指向 null 的物件參考的方法，會出 NullPointerException

  ```java
  Shirt nullShirt = null;
  System.out.println(nullShirt); // null
  
  String s1 = null + "Hi";
  System.out.println(s1);        // nullHi
  
  String s2 = "Hi" + null;
  System.out.print(s2);          // Hinull
  
  System.out.println(nullShirt.price); 
  // Exception in thread "main" java.lang.NullPointerException
  ```

### JVM 的記憶體分類

1. **Global (全域)** : 保存 static 的類別成員變數
2. **Stack (堆疊)** : 
   + 保存基本型別（primitive type）的變數和變數內容（value）的地方
   + 保存參考型別（reference type）的變數的地方
3. **Heap (堆積)** : 保存參考型別（reference type）的變數內容（instance）的地方

![image-20221210174113119](https://i.imgur.com/GJk0N8K.png)

| 分類     |    變數 & 變數值 | Stack（堆疊） | Heap（堆積） |
| -------- | ---------------: | :-----------: | :----------: |
| 基本型別 |             變數 |       ⭐       |              |
| -        |               值 |       ⭐       |              |
| 參考型別 | 變數（物件參考） |       ⭐       |              |
| -        |   值（物件實例） |               |      ⭐       |

+ 將 shirt1 物件參考變數，改指向 shirt 的物件實例，如下圖藍箭頭 (兩台遙控器指向同一台電視)

  `shirt1 = shirt2;`

  ![image-20221210190123989](https://i.imgur.com/24Yd5fO.png)

  ```java
  Shirt shirt1 = new Shirt();
  Shirt shirt2 = new Shirt();
  shirt1 = shirt2;  // to the same heap
  shirt1.price = 1000;
  shirt2.price = 500;
  System.out.println("Shirt price: " + shirt1.price); 
  // console → Shirt price: 500.0
  ```

---

## 使用 String 類別

### String 類別支援非標準語法

+ String 類屬於參考型別，使用時會產生物件

+ 為避免太多重複 String 物件造成記憶體空間浪費，建立的 String 物件會儲存於字串池 (string pool) 中

  + 在某些條件下，相同內容的字串可直接由字串池中重複取用
  + 用畢歸還，避免內容相同的 String 物件一再產生

+ String 物件也可以用 `new` 進行實例化，但不建議，因為會建立新的 String 物件

  ```java
  String s3 = new String("Java");
  ```

### String 物件是不可改變的 (*immutable*)

+ 字串相連常見兩種方式

  ```java
  String name1 = "Jim";
  String name2 = name1 + " is teaching";
  name1 = name1.concat(" is teaching");
  // 將 name1 的內容複製到 name3，再連接字串 " is teaching"
  name1 = name1 + "!"; 
  // 此時 "Jim is teaching" 拷貝到 Heap address_3
  // 原位址 address_2 則回歸字串池
  ```

+  重要：String 類別一旦建立物件實例（instance），**就無法變更其物件狀態** ⚠

  + name1、name2 均參照到不同的記憶體位址

### String 類別的其它方法

+ `name.length()` - 取得字串長度
+ `toUpperCase()` 或 `toLowerCase()` 將字串內字元全部轉大寫/全部小寫
+ `trim()` - 去除字串前後空白
+ `substring()` - 由字串內取出部分字串
+ `endsWith()` - 判斷字串結尾

---

## 使用 StringBuilder 類別 

### StringBuilder 基本特性

1. 大部分都回傳自己的參照，沒有實例化的成本
2. 必須使用 `new` 關鍵字進行物件實例化
3. 提供字串存取的擴充方法，例如 `append()`、`insert()`、`delete()`等
4. 若開發時已約略知道字串最大長度，建立物件時可以提供最佳化的 *initial capacity* 設定，一次預留足夠長度，可以避免字串增長時造成的效能損失

String 類別不可缺少的原因：

1. 使用 immutable 物件比較安全
2. String 類別在 Java 一推出時即存在，有眾多類別需要此
3. 擁有的方法比 StringBuilder 還多

   [library](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/StringBuilder.html)

---

## 使用 `var` 宣告變數

### 使用 var 宣告的時機

+ 正式名稱「區域變數型別推斷」（*local variable type inference*）

+ 無法使用在實例變數，由等號右側的變數值推斷（inference）變數型別（type）

+ 可使用在 lambda 表示式、迴圈區塊、try-with resources

+ 以`var`宣告的變數在「升等/轉型」運用上，和以非 `var`宣告的變數一致

  ```java
  short s = (short) 10;
  s = (byte) 5;
  s = 1_000_000; // compilation error
  
  var s = (short) 10;
  s = (byte) 5;
  s = 1_000_000; // compilation error
  ```

+ 以 `var` 宣告但無法推斷型別時，將編譯失敗

+ var 宣告的變數指向 null 時，也會編譯失敗 ``

  ```java
  public void doesThisCompile(boolean check) {
      var question;  // compilation error
      question = 1;
      var answer;    // compilation error
      if (check) {
          answer = 2;
      } else {
          answer = 3;
      }
      System.out.println(answer);
  }
  
  public void varAsNull() {
      var n = null;
      // Cannot infer type for local variable initialized to 'null'
  }
  ```

+ **複合宣告（compound declaration）不適用於 var 變數**

  1. 同時有多個變數進行宣告：開頭宣告型別，行間不能有其它型別，以`,`區隔變數，`;`結尾
  2. 個別變數可自行決定是否初始化給值

  ```java
  public void compoundDeclarationWithNonVar() {
      int a, b = 3;
      int c = 2, d = 3;
      int e, f;
      int g =, h;
      int i, int j;    // Compilation error, not compound declaration
      int k, double l; // Compilation error, not compound declaration
  }
  
  public void compoundDeclarationWithVar() {
      var a, b = 3;     // compilation error  
      var c = 2, d = 3; // compilation error
      int e, var f = 3; // compilation error, NON-compound declaration   
  }
  
  public void notCompoundDeclartion() {
      int a; int b = 3;
      int c; var d = 3;  
      // 只要用 ; 區隔變數，就不再是複合宣告
  }
  ```

+ 雖然 `var` 不能以 null 值推斷型別，但可以在宣告成功後再指向到 null

  ```java
  public void testVarAndNull() {
      var n = "myData";
      n = null;
      var m = 4
      m = null; // compilation error
      var o = (String) null;
  }
  ```

+  方法參數不能用 `var` 宣告

  ```JAVA
  public int addition(var a, var b) {
      return a + b;   // 'var' is not allowed here
  }
  ```

+ 討論：以下 `var` 可否通過編譯？

  ```java
  package var;
  
  public class Var {
      public void var() {
          var var = "var";
      }
      
      public void Var() {
          var var = new Var();
      }
  }
  ```

  🍪☕ **說明**：

  | #      | desc                                                         |
  | ------ | ------------------------------------------------------------ |
  | Line 1 | 套件名稱使用 var 合法                                        |
  | Line 3 | Java 區分大小寫，因此 Var 作為類別名稱沒問題，若改為 var 則無法通過編譯<br />雖然 var 非關鍵字，Java 也不能以小寫字母作為類別名稱開頭 |
  | Line 4 | 方法名稱使用 var 合法                                        |
  | Line 5 | 變數名稱使用 var 合法                                        |

+  實務上變數以 var 宣告的情境

  + 使程式碼看起來比較精簡：

    ```java
    SomeClassWithVeryVeryVeryVeryLongName x = new SomeClassWithVeryVeryVeryVeryLongName();
    ```

    ```java
    var x = new SomeClassWithVeryVeryVeryVeryLongName();
    ```

---

# 07 選擇結構和相關運算子

## 選擇結構簡介

+ 在某些情境下，做出某些反應，就是**選擇結構**要介紹的重點
  + 在電梯已經最底層時執行 `x.down()`
  + 忘了先關門就執行 `x.up()`、`x.down()`
  + 還沒關門就又要開門 `x.open(); x.down(); x.open();`

### 關係運算子與條件運算子

| 運算子 | 情境      | 範例 |
| ------ | --------- | ---- |
| `==`   | 是否 相等 |      |
| `!=`   | 是否 相等 |      |
| `<`    | 是否 相等 |      |
| `<=`   | 是否 相等 |      |
| `>`    | 是否 相等 |      |
| `>=`   | 是否 相等 |      |
| `&&`   | 且（）    |      |
| `||`   | 或（）    |      |
| `!`    | 非（）    |      |

### 字串比較

1. 使用 `==` 比較字串是否**指向相同記憶體位址**

2. 使用 `equals()` 比較字串是否為**相同內容**

   ```java
   String s1 = "yorushika";
   String s2 = "yorushika";
   String s3 = new String("yorushika"); // Java 產生新字串物件
   
   System.out.println(s1 == s2);      // true  
   System.out.println(s1 == s3);      // false  //#
   
   System.out.println(s1.equals(s2)); // true  //#    
   System.out.println(s1.equals(s3)); // true  //# 
   ```

   + line 5 : 字串物件都在字串池內，因為不可更改且可以重複使用，所以 s1 與 s2 指向同一物件實例

   + line 6 : 因為使用 `new` 關鍵字，Java 強制在字串池內生成新的字串物件 "jim"，s3 將指向該新生物件，所以和 s1、s2 不同

     `s1 == s2 != s3`

   + line 8, line 9 : s1, s2, s3 的字串物件內容都相同

　### 選擇結構

+ if-else

+ 三元運算子 ternary `(boolean_expression) ? value if true : value if false`

+ switch

  + 相較 if 選擇結構，程式碼較為工整

    ```java
    switch (variable) {
        case literal_value:
            <code_block>
            [break;]
        case another_literal_value:
            <code_block>
            [break;]
        [default:]
            <code_block>
    }
    ```

    1. **variable** : 要測試的變數。可以是 `byte`、`short`、`char`、`int`、`String`
    2. **literal_value** : 變數可能的值（字面常量）
    3. **default** : 不符任一 case 變數值時，則進入本區塊，相當於 if 結構的 else
    4. **break** : 非必要，用以離開該 case code block

---

# 08 陣列

## 陣列 (1-D and 2-D)

### 基本定義

1. 陣列（array）：一種容器物件（container object），可裝載**多個**且**單一型態**的基本型別 / 參考型別

2. 陣列裡的內容物稱之為**成員**（element）

3. 建立陣列時必須指定**長度**，即**成員數量**，一旦建立，長度就不能改變

4. 陣列成員使用數字化的**索引**（index）存取，索引從 0 開始

   > 陣列用於處理多個同種物件或基本型別變數

5. 建立陣列的完整程序：

   1. **宣告（Declaring）**

      ```java
      type[] array_identifier;
          // type: 陣列成員型別
          // []: 表示宣告陣列
          // array_identifier: 陣列名稱
      
      // Primitive array
      char[] chars;
      int[] ints;
      
      // Reference array
      Shirt[] shirts;
      String[] strings;
      ```

   2. **建構實例（Instantiating）**

      ```java
      array_identifier = new type[length];
          // array_identifier: 陣列名稱
          // type: 陣列成員型別
          // length: 陣列長度
      chars = new char[20];
      ```

      + 若未對陣列成員初始化，Java 也會給與物件成員一致的預設值
        + 整數基本型別：`0`
        + 浮點數基本型別：`0.0`
        + 字元基本型別：空字元，`' '`或 `\u0000`
        + 邏輯基本型別：`false`
        + 參考型別：`null`

   3. **初始化（Initializing）**

      ```java
      array_identifier [index] = value;
          // array_identifier: 陣列名稱
          // index: 成員位置，從0開始，最大為長度-1
      ints[0] = 13;
      strings[0] = "Hola!";
      ```

      > 一起完成宣告、實例化、初始化
      >
      > ```java
      > type[] array_identifier = {element_1, element_2, ...};
      > int[] ints = {10, 20, 30, 40};
      > String[] strings = {"Hola", "Hallo", "Ciao", "Ohayo"};
      > ```
      >
      > **可一氣呵成，但不能分行**
      >
      > ```java
      > int[] ints;
      > ints = {10, 20, 30, 40};  // compilation error
      > ```

### 建立二維陣列

+ 第一層陣列裡的成員是第二層陣列（二維）

  + 第 N-1 層陣列裡的成員是第 N 層陣列（N維）

+ 建立過程：

  1. 宣告

     ```java
     type[][] array_identifier;
         // array_identifier 陣列名稱
         // type 陣列成員型別
     int[][] rowColumns;
     ```

  2. 實例化

     ```java
     array_identifier = new type[number_of_arrays][length];
         // array_identifier 陣列名稱
         // type 陣列成員型別
         // number_of_arrays : 內含的一維成員陣列個數，不可為空
         // length : 每個成員陣列的成員個數，可以為空
     rowColumns = new int[3][2]; // 三個陣列，每個陣列兩個成員
     ```

  3. 初始化二維陣列

     ```java
     array_identifier[index_1][index_2] = value;
         // array_identifier 陣列名稱
         // index_1 指定成員陣列
         // index_2 指定成員陣列裡的成員位置
     rowColumns[0][0] = 10;
     ```

     > 一起完成前述三個部分
     >
     > ```JAVA
     > type[][] array_identifier = {{}, {}, ...}; // 以,區隔成員陣列
     > 
     > int[][] int2d = {{6, 77}, {88}, {9, 10}};
     > String[][] string2d = {{"swift", "sword"}, {"Guten"}, {"Morgen"}};
     > ```

### 比較多維陣列的建立

```java
public class ArrayCreateTest {
    public static void main(String[] args) {
        int[] a1 = new int[5];
        int a2[] = new int[5];
        // int a3[5] = new int[];
        int[][] a4 = new int[5][];
        int[] a5[] = new int[5][3];
        int[][][] a6 = new int[5][][];
        int a7[][][] = new int[5][3][2];
    }
}
```

| #      | Desc                                                         |
| ------ | ------------------------------------------------------------ |
| line 3 | 宣告陣列時，[ ] 可以在**型別**後方                           |
| line 4 | 宣告陣列時，[ ] 也可以在**變數**後方                         |
| line 5 | 宣告陣列時，`=` 左側的 [ ] 裡面不能加上長度，只能在右側的 [ ] 內 |
| line 6 | 宣告二維陣列時，第一層陣列必須有長度宣告                     |
| line 7 | 宣告二維陣列時，`=` 左右兩側的 [ ] 個數加總必須各自為2，(兩個[ ] = 兩個 [ ])<br />第二層的陣列長度非必要 |
| line 8 | 宣告三維陣列時，左右兩側的 [ ] 個數加總必須各自為2，(三個 [ ] = 三個 [ ]) |
| line 9 | 用於宣告的 [ ] 可在變數前後，第一層陣列長度為必填，其它陣列長度非必要 |

> 🍪☕ **Little Tips** — 第一維陣列的長度為必要
>
> 1. 陣列是容器物件，建立陣列如同蓋房子給人住
>
> 2. 建構房子時，一定要事先確認幾個房間
>
>    + 和建物主體結構有關，蓋好了就不能改變
>
> 3. 每個房間有一個房客，房間編號由 0 開始，拜訪房客必須指定編號
>
>    + 二維陣列 - 就像二房東再出租，可以只是木造隔間，無關房子結構
>
>      + 不需要特別限制每個二房東要區隔的房間數
>
>      + 二維陣列的第二層長度並未強制要求
>
>        `int[][] ints = new int[3][];`

---

## 存取陣列內容

### 陣列成員的基本讀寫

+ 寫入陣列

  ```java
  chars[0] = 'H';
  ints[2] = 80;
  strings[3] = "Willkommen";
  ```

+ 讀出陣列

  ```java
  char c = chars[0];
  int i = ints[2];
  String s = strings[3];
  ```

### 陣列成員為基本型別的記憶體配置

```java
char size = 'S';
// char 的變數名稱 size 和其值都在 Stack 區
char[] sizes = {'X', 'M', 'L'};
// char[] 變數名稱 sizes 在 Stack 區，值在 Heap 記憶體區
```

### 陣列成員為參考型別的記憶體配置

```java
Shirt shirt = new Shirt();
// shirt 變數名稱在 Stack 記憶體，值在 Heap 記憶體
Shirt[] shirts = {new Shirt(), new Shirt()};
// shirts 變數名稱在 Stack 記憶體
// 陣列在 Heap 記憶體，該陣列記憶體只放成員物件的記憶體位置
```

> [address_2] { address_3, address_4} 各有遙控器指向另外的 Heap 記憶體位址 
>
> 🍪☕ **Little Tips** :
>
> 1. 基本型別成員住在陣列的房間裡
> 2. 參考型別成員只是掛戶籍，不住在裡面；可先尋訪房間，找到真正居住地址

---

## 使用指令列 args 陣列參數

+ `-d` : 指定編譯後產生 `*.class` 檔案的目錄（directory），在此例為 `bin`
  + `src/course/c08/CommandArgsTest.java` - 要編譯的 `*.java` 檔相對於專案根目錄的位置
+ `-cp` : 指定存放  `*.class` 檔案的類別路徑（class path），在此例為 `bin`

![image-20221212170100240](https://i.imgur.com/ZUUapRj.png)

+ Eclipse IDE → right click `CommandArgsTest.java` → click `Run As` → choose `Run Configurations...` → On `Main` tab, check project class → On `Arguments` tab, fill in Program arguments `Hola Joanna` → click `Run`
+ 即使傳入的參數為數字型態，進入 main() 方法後仍會被轉成字串陣列成員
  + 必須以字串型別取出再做轉換 (`int i1 = Integer.parseInt(args[0]);`)

## 使用 ArrayList 類

### 陣列 `array` 缺點

+ 無法自動增加長度，若有需要必須自行處理
  1. 紀錄每個加入陣列的元素的索引
  2. 追蹤並記錄陣列長度
  3. 若長度不足，則另外建立一個足夠長度的新陣列，複製原陣列成員，再捨棄原陣列

### ArrayList 簡介

1. 只存放參考型別的物件，不接受基本型別

2. 可以管理成員物件的方法，例如 `add()`、`remove()`、`indexOf()`、`get()`

3. 建構 ArrayList 物件不用設定長度大小，加入更多成員物件時，ArrayList 會自動成長

4. 建構 ArrayList 時可設定 **initial capacity**，但非必要

   > 🍪☕ Knowledge bean:
   >
   > 1. StringBuilder 與 ArrayList 建構時都可以指定 initial capacity，但非必要
   > 2. 這兩者的最底層還是陣列，規定不能改變，所以 StringBuilder / ArrayList 作法如下：
   >    + 預先建立一定長度的陣列
   >    + 若持續加入字串或物件導致超出預建陣列長度範圍，Java 將自動建立更長的陣列，再將原陣列成員都複製一份後轉移過去
   > 3. 以上作法會大量使用 CPU 與記憶體資源，所以建議在建構時就設定適當初始長度 initial capacity


### 使用 import 與 package 關鍵字

+ 使用 ArrayList 的方式（二擇一）
  1. 套件名稱加上類別名稱：`java.util.ArrayList`
  2. import 敘述：`import.java.util.ArrayList`

### 使用泛型指定 ArrayList 成員型態

+ 泛型符號 `<>`，在 Java 5 時導入。例 `ArrayList<String> list = new ArrayList<String>();`
+ Java 7 之後開始移除後方 `<>` 內的型別。例 `ArrayList<String> list = new ArrayList<>();`

---

# 09 重複結構

## 迴圈架構

+ 迴圈架構（loop constructs）：
  + 符合特定條件（expression）時，重複某些行為（code block），分三種型態
    1. while 迴圈：若滿足 expression = true，則持續進行
    2. do-while 迴圈：執行一次後，如滿足 expression = true，則持續進行
    3. for 迴圈：重複特定次數

### `while`迴圈

```java
while (boolean_expression) {
    code_block;
}   // 滿足 boolean_expression 時，反覆執行 code_block
```

### 使用迴圈以二分逼近法求平方根

```java
public static void getSquareRoot() {
    float input = 10;
      // 計算10的平方根
    float squareRoot = input;
      // 假設答案就是10
    float accurate = 0.01f;
      // 定義計算值和真實值之間的可接受誤差範圍
    while (squareRoot * squareRoot - input > accurate) {
             // 定義計算值和真實值的可接受誤差範圍
        squareRoot = (squareRoot + input / squareRoot) / 2;
          // 2分逼近法公式
        System.out.println("trying... " + squareRoot);
    }
    System.out.println("The square root of " + input + " (accurate="
                                  + accurate + ") => " + squareRoot);
}
```

![image-20221213103232214](https://i.imgur.com/7jg2ZVP.png)

### 使用迴圈計算複利年息

```java
public static void doubleYourMoney() {
    double money = 500;      // 本金
    double interest = 0.18;  // 年息18% 🙃
    int years = 0;
    while (money <= 1000) {        // 2倍本金
        money += money * interest;   // 複利計息
        years++;
        System.out.println("Year " + years + ": " + money);
    }
}
```

![image-20221213103323861](https://i.imgur.com/lVlHxBQ.png)

### 使用迴圈製作註解區塊

```java
public static void commentBlock() {
    System.out.println(" /*");
    int counter = 0;
    while (counter < 4) {
        System.out.println(" *");
        counter++;
    }
    System.out.println(" */");
}
```

### `for` 迴圈

有清楚的初始條件、滿足條件、變動條件

```java
for (initialize[,initialize]; boolean_expression; update[,update]) {
    code_block;
}
// initialize: 初始條件
// boolean_expression: 滿足條件
// update: 變動條件
```

while 迴圈：

```java
int i = 0;      // initialize
while(i < 7) {  // boolean_expression
    System.out.println("$");
    i++;        // update
}

for (int i = 0; i < 7; i++) {
    System.out.println("$");
}
```

Note:

1. 合併多個 **滿足條件** 後，必要為 true 才能執行 code_block 

2. **初始條件** 和 **變動條件** 不必然只能一個，也可以多個，並以`,`區隔

   ```java
   for (String i = "$", t = "-"; i.length() < 5; i += "$", t+= "-") {
       System.out.println(i + t);
   }
   ```


### 巢狀迴圈 nested loop

##### 印三角形

```java
private static void NestedLoopTest() {
	int num = 5;
	for (int i = 0; i < num; i++) {
        for (int j = 0; j <= i; j++) {
            System.out.print('*');
        }
        System.out.println();
    }
}
```

##### 隨機猜英文單字

```java
private static void guessWord() {
    String word = "jim";
    String guessWord = "";
    int tryCounts = 0;
    while (!guessWord.equals(word.toUpperCase())) {  
        guessWord = "";
        while (guessWord.length() < word.length()) {  
            char c = (char) (Math.random() * 26 + 65);
            // 隨機產生 65 ~ 91 (A~Z大寫英文)
            guessWord = guessWord + c;
        } // line 7 - 11 內迴圈: 製作和原始長度一樣的新單字
        System.out.println(guessWord);
        tryCounts++;
    }  // line 5 - 14 外迴圈: 比對新單字是否和原始單字一樣
    System.out.println(word + " was found!");
    System.out.println("After " + tryCounts + " tries!!");
}
```

## `for` 迴圈存取陣列

### 設定陣列內容並顯示

```java
public static void loopArray() {
    long[] longArray = new long[9];
    // set up array content
    for (int i = 0; i < longArray.length; i++) {
        longArray[i] = Math.round(Math.random() * 100);
    }
    // access array content
    for (int i = 0; i < longArray.length; i++) {
        System.out.println(i + ": " + longArray[i]);
    }
}
```

### 使用 `for` 迴圈進階型

+ for 迴圈進階型（enhanced）可用於存取 Java 集合物件（Collections）和陣列（Array）

```java
for(declaration : expression) {
    code_block;
}
// declaration: 宣告集合物件(Collection)或陣列(Array)的成員型態
// expression: 欲存取的集合物件(Collection)或陣列(Array)變數

public static void enhancedLoopArray() {
    int[] intArray = { 99, 88, 77, 66, 55, 44, 33, 22, 11 };
    for (int element : intArray) {
        System.out.println(element);
    }
    System.out.println("-----");
    String[] names = { "alex", "bijork", "carlo", "dyana", "elliot", "fanni" };
    for (String name : names) {
        System.out.println(name);
    }
}

public static void enhancedLoopArrayList() {
    ArrayList<String> names = new ArrayList<>();
    names.add("alice");
    names.add("bob");
    names.add("cassandra");
    names.add("daniel");
    names.add("elia");
    names.add("ferrero");
    for (String name : names) {
        System.out.println(name);
    }
}
```

### 使用 `break` 和 `continue` 敘述

目的：改變迴圈流程

1. 使用 break 敘述結束迴圈，break 敘述以下的迴圈內程式碼不執行
2. 使用 continue 敘述將流程回到迴圈起始點繼續執行

```java
public static void useBreak() {
    int passScore = 60;
    int[] scores = { 40, 20, 55, 59, 63, 33, 98 };
    int passAt = 0;
    for (int s : scores) {
        passAt++;
        if (s > passScore) {
            break; // found, job complete
        }
    }
    System.out.println("Finally pass at: " + passAt);
}

public static void useContinue() {
    int passScore = 60;
    int[] scores = { 40, 20, 55, 59, 63, 33, 98 };
    for (int s : scores) {
        if (s > passScore)
            continue; // 跳過, check next 
        System.out.println("the score: " + s + " failed to pass.");
    }
}
```

## 使用 `do-while` 迴圈

```java
do {
    code_block;
} while (boolean_expression); //  ⚠ 注意結尾加上 ;
```

比較 `do-while` 與 `while` 迴圈

1. `while` 必須滿足條件才能執行，又稱為前測式迴圈
2. `do-while` 至少可以執行一次，第一次之後就需要檢查條件，又稱為後測式迴圈

## 比較重複結構

| 迴圈種類   | 執行次數           |
| ---------- | ------------------ |
| `while`    | 執行 0 到多次      |
| `do-while` | 執行 1 到多次      |
| `for`      | 執行事先定義的次數 |

---

#  10 方法設計與使用

## 使用方法 `method`

### 宣告方法

```java
[modifiers] return_type method_identifier( [arguments]) {
    method_code_block
}
/*
  method_identifier - 方法名稱，必要
  return_type - 回傳型別，必要
  [modifiers] - 修飾詞，非必要
  [arguments] - 輸入參數，非必要
*/
```

### 呼叫方法

取得物件參考 / 遙控器後，使用 `.` 運算子呼叫該方法 

```java
Shirt myShirt = new Shirt();
myShirt.display(); // caller 呼叫者
```

```java
public void display() {
    // method_code_block 
    // worker 實際工作的方法/被呼叫者
}
```

如果呼叫者和被呼叫者都是自家人，就不需要物件參考 / 遙控器；或是使用 `this` 關鍵字，想像為指向自己的另類遙控器

```java
public class MyElevator2 {
    public boolean open = false;
    // other blocks of code
    public boolean isDoorOpen() {
        return this.open;
    }
    public void open() {
        if (!this.isDoorOpen()) {
            // ...
        }
    }
}
```

### 使用方法的關注點與好處

1. 方法是物件的行為
2. 方法應具備獨立的功能或邏輯性，以利於維護及維持可讀性
3. 增加程式的可重複使用性，減少重複程式碼
4. 透過呼叫者與被呼叫者之間的呼叫，讓不同物件互動

## 宣告 `static` 方法和變數

![image-20221214003311040](https://i.imgur.com/39YiMyr.png)



🚧🚧🚧🚧🚧🚧🚧🚧🚧🚧

## 建立多載 *Overloaded* 的方法

## 認識 Java 傳遞變數的機制

---

# 11 物件導向程式設計（一）

## 封裝

## 建構子

## 繼承

## 繼承和建構子的關係

## 父類別和子類別

## 多型

## 介面

## 物件始祖 - class Object

---

# 12 物件導向程式設計（二）

## 再談封裝

## 子類別和繼承關係

## 多載方法與使用可變動參數個數的方法

## 多型



---

# 13 程式執行異常處理

## 執行時異常與例外物件 *Exception*

## 例外傳播與處理

## 例外繼承結構

## 處理例外的好習慣



---

# 14 類別設計 1/2

## 存取控管機制 : public, protected, private, default

## 覆寫（Override）方法

## 認識轉型機制

## 覆寫 Object 類別的方法





---

# 15 類別設計 2/2

## 抽象類別

## static 宣告

## final 宣告

## 實作獨體設計模式

## 使用列舉型別

## 使用巢狀類別



---

# 16 介面設計

## 介面與 abstract 方法

## 介面與設計模式

## 使用複合 *Composition*

## 以 default 與 static 宣告介面方法並實作內容



---

# 17 Lambda 表示式入門

## Lambda 表示式介紹

## 功能性介面 *Functional Interface* 與 Lambda 表示式

## 使用內建的功能性介面



---

# 18 Mock exam
