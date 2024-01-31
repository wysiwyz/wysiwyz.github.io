---
title: "1Z0-819 題目KO"
date: 2023-08-05T12:13:34+08:00
author: "celine"
tags: ["Java", "1Z0-819"]
categories: ["StudyNote"]
---

>這裡放比較tricky的題目們

#### #62

```java
class Employee {
    private String name;
    public void setName(String name) {
        String title = "Dr. ";
        name = title + name;
    }
    public String toString() {
        return name;
    }
}
```

And

```java
public static void main(String[] args) {
    Employee p = new Employee();
    p.setName("Who");
    System.out.println(p);
}
```

What's the resuld?

A. Dr. Who

B. Dr. Null

C. An exception is thrown at runtime

D. null

> Analysis:
>
> 如果line 5改成以下程式碼，則答案為A
>
> ```java
> this.name = title + name;
> ```
>
> 否則是D. null

#### #63

Given:

```java
public class Exam {
    private int sum;
    public int calculate() {
        int x = 0;
        while (x < 3) {
            sum += x++;
        }
        return sum;
    }
    public static void main(String[] args) {
        Exam t = new Exam();
        int sum = t.calcuate();
        sum = t.calculate();
        t.calculate();
        System.out.println(sum);
    }
}
```

What is the result?

A. 9

B. An exception is thrown at runtime

C. 3

D. 6

> Analysis:
>
> line5-7程式碼與以下等價。答案為D
>
> ```java
> while (x < 3) {
>     sum += x;
>     ++x;
> }
> ```
>
> | line  | desc.                                                        |
> | ----- | ------------------------------------------------------------ |
> | 12    | sum 值 0 + 0 + 1 + 2 = 3                                     |
> | 13    | sum 值 3 + 0 + 1 + 2 = 6                                     |
> | 14~15 | Exam物件內的實例變數sum變成9，但是main方法裡面的區域變數sum並沒有改變，因此輸出6 |

#### #64

Given:

```java
package a;
public abstract class Animal {
    protected abstract void eat();
}
```

And:

```java
package b;
import a.Animal;

public abstract class Tiger extends Animal {
    // line1
}
```

Which two lines inserted in line 1 will allow the code to compile? (Choose two)

A. protected void eat() {}

B. void eat() {}

C. abstract void eat();

D. private void eat() {}

E. public abstract void eat();

> Analysis:
>
> 1. 子類別覆寫父類別方法，只能用相同或更大的存取修飾詞，因此default(選項B、C)、private(選項D)都無法通過編譯
> 2. 因為子類別也是抽象類別，不實作方法也可以，故選項E也對
>
> 答: AE

### #65

Given:

```java
public class Test {
    public static void main(String[] args) {
        int a = 4;
        int b = 2;
        System.out.println(a + b + "=(a+b)=" + a + b);
    }
}
```

What is the result?

A. An exception is thrown at runtime.

B. 42=(a+b)=42

C. 42=(a+b)=6

D. 6=(a+b)=42

E. 6=(a+b)=6

> Analysis
>
> `+`運算子可用於數字相加與字串相連，一旦表達式裡面出現字串，後續的數字也都以字串處理
>
> 答: D

#### #66

Given:

```java
public static void main(String[] args) {
    byte x = 8, y = 7;
    // line 1
    System.out.println(z);
}
```

Which expression when added at line 1 will produce the output of 1.14?

A. float z = (float)(Math.round(**(float)x/y*100)**/100);

B. float z = Math.round((int)(x/y),2);

C. float z = Math.round((float)x/y,2);

D. float z = Math.round((float)x/y*100)/(float)100;

> Analysis
>
> Math.round()方法簽名: 提供float或double參數，回傳int整數
>
> ```java
> public static int round(float a)
> ```
>
> 因為無法指定小數點位數，選項B、C無法編譯
>
> 需要先轉型，再數學計算
>
> ```java
> float f1 = (float) x/y;
> float f2 = ((float) x)/y; // 結果同line1
> float f3 = (float) (x/y); // xx
> ```
>
> 選項A的被除數與除數都是int，計算結果也只會是int，A計算結果為1
>
> 必須把被除數或除數其中之一轉型成float，才會得到期望結果
>
> 答: D
>
> ```java
> Math.round((float) x/y);         // =1
> Math.round((float) x/y * 100);   // =114
> Math.round((float) x/y * 100) / 100;   // =1
> Math.round((float) x/y * 100) / (float) 100;  // =1.14
> ```

#### #67

Given

```java
public class Exam {
    public static void main (String[] args) {
        int i = 0;
        for (String s : args) {
            System.out.print((i++) + ")" + s + " ");
        }
    }
}
```

executed with this command

```terminal
java Exam one two three
```

What is the output of this class?

A. The compilation fails.

B. 0)one 1)two 2)three

C. A java.lang.ArrayIndexOutOfBoundsException is thrown.

D. 0)one

E. nothing

> line5與以下等價，即是否有括號`()`(例如`(i++)`與`i++`)，對遞增或遞減運算子的效果沒有其他影響
>
> ```java
> System.out.print(i++ + ")" + s + " ");
> ```
>
> 會先執行`System.out.print()`之後，才執行i的遞增
>
> 答: B

#### #68

Which three initialization statements are correct? (Choose three)

A. int x = 12_21;

B. short sh = (short)'A';

C. String contact# = "(999 (111)";

D. boolean true = (2 == 2);

E. float x = 1.99;

F. int\[]\[] e = { { 3, 3 }, { 2, 2 } };

G. byte b = 10; char c = b;

> Analysis:
>
> 1. `C` - 變數名稱不能有特殊字元 #
> 2. `D` - 變數名稱不可以是關鍵字 true
> 3. `E` - 變數值 1.99 預設為 double，需轉型
> 4. `G` - char變數可以直接指向數字常量，但指向*數字變數*則需要轉型

#### #69 ⭐

Given:

```java
public class Test {
    static void print(int... arr) {
        System.out.println("int[]...");
    }
    static void print(long l1, long l2) {
        System.out.println("long, long");
    }
    static void print(Integer i1, Integer i2) {
        System.out.println("Integer, Integer");
    }
    public static void main(String[] args) {
        int i = 9;
        print(i, i);
    }
}
```

What is the result of compiling and running the following code?

A. Does not compile

B. Prints int[]...

C. Prints long, long

D. Prints Integer, Integer

E. Throws Exception

F. None of these

> Analysis:
>
> 本題3個方法都可執行，以參數相似度決定Overloading選擇順序
>
> 1. 參數**型態**相符程度，完全一樣最好。*同是基本型別或參考型別優先度高*，因此`print(long l1, long 12)` 優先於 `print(Integer i1, Integer i2)`
> 2. 參數**數量**相符程度，完全一樣最耗，因此 `print(long l1, long l2)` 優先於 `print(int... arr)`
>
> 答: C

#### #70

Given a class Exam with instance field integers, and many constructor options like below:

```java
public class Exam {
    private Integer[] integers;
    // Option A
    public Exam(List<Integer> integers) {
        this.integers = integers;
    }
    // Option B
    public Exam (Integer... integers) {
        integers = integers;
    }
    // Option C
    public Exam (Integer... integers) {
        this.integers = integers;
    }
    // Options D
    public Exaxm (Integer integers) {
        integers = integers;
    }
    // Option E
    public Exam (Integer[] integers) {
        this.integers = integers;
    }
}
```

Which two constructors independently will compile and set the class field integers? (Choose two)

A. Opt.A

B. Opt.B

C. Opt.C

D. Opt.D

E. Opt.E

> Analysis
>
> 1. Opt.A 實例變數欄位(List\<Integer>)與建構子參數型態(Integer[])不符
> 2. Opt.B、Opt.D 未有效設定實例變數欄位，應使用 this.integers
>
> 答: CE

#### #71 ⭐

Given

```java
public static void main(String[] args) {
    String s1 = new String("Java");
    String s2 = "Java";
    String s3 = s1.intern();
    System.out.print((s1 == s2) + " ");
    System.out.print((s2 == s3) + " ");
    System.out.print((s1 == s3) + " ");
}
```

What is the result?

A. false true true

B. true false false

C. false false true

D. false true false

> Analysis
>
> 1. line2 `new String("Java")` 將產生新的String物件，而且和建構子參數字串的字元內容順序一致
>
>    ```
>    Initializes a newly created String object so that it represents the same sequence of characters as the argument; 
>    in other words, the newly created string is a copy of the argument string.
>    ```
>
> 2. line3 `String s2 = "Java"` 在建構字串物件之後，因為不可更改(immutable)，會存放在字串池中等待其他重複使用的機會
>
> 3. line4 `String s3 = s1.intern()`，方法`intern()`會先去字串池中以 `equals()` 方法尋找相同的字串物件
>
>    + 如果存在，返回該物件的參照位址(遙控器)
>    + 如果不存在，則在字串值中建立新的String物件，並返回該物件的參照位址
>
>    ```
>    When the intern method is invoked, if the pool already contains a string equal to this String object as determined by the equals(Object) method, then the string from the pool is return.
>    Otherwise, this String object is added to the pool and a reference to this String object is returned.
>    ```
>
> 因為**物件參考s1不涉及字串池，s2與s3都涉及字串池可重複使用**，所以 `s1 != s2` `s1 != s3` `s2==s3`

#### #72 ⭐

Given:

```java
public static void main(String[] args) {
    var x = 10;
    var y = 5;
    x += (y * 5 + y) / x - 2;
    System.out.println(x);
}
```

What is the result?

A. 5

B. 3

C. 23

D. 25

E. 11

> Analysis
>
> `x += (y * 5 + y) / x - 2`運算子處理順序如下
>
> ```java
> x += (y * 5 + y) / x - 2;
> x += 30 / 10 - 2;
> x += 1;
> x = x + 1; // 10+1
> ```
>
> 答: E

#### #73 ⭐

Given:

```java
public class Exam {
    private int a;
    private static int b;
    public static void main(String[] args) {
        Exam t1 = new Exam();
        t1.a = 2;
        Exam.b = 3;
        Exam t2 = new Exam();
        t2.a = 4;
        t2.b = 5;
        System.out.print(t1.a + "," + t1.b + " ");
        System.out.print(t2.a + "," + Exam.b + " ");
        System.out.print(t2.a + "," + t1.b + " ");        
    }
}
```

What is the result?

A. 2,3 4,3 4,5

B. 2,3 4,5 4,5

C. 2,5 4,5 4,5

D. 2,3 4,5 4,3

> Analysis
>
> 1. static變數b是所有Exam物件實例共享，因此ln7的值`3`會被ln10的`5`取代
> 2. static變數b可以使用類別Exam和物件參考t2關聯
>
> 答: C

#### #74

Given

```java
interface MyFunInterface {
    double getFunValue();
}
```

And

```java
public class Test {
    
}
```





