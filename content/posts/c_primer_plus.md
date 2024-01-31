---
title: "C primer plus"
date: 2022-10-16T10:08:10+08:00
author: "celine"
tags: [""]
categories: ["StudyNote"]
---



成為 C++ pro 的幾個建議（摘錄自 Big Sam Tsai）

> 1. Read good C++ textbooks again and again
> 2. Learn from practice
> 3. Work in related field
> 4. Right mentor
> 5. Make learning thoughts public
> 6. At least **three years**

這樣看來我能做到的有 1. 2. 5. 6. ，~~4. 的話可以抓親友充數~~

## Chapter  2

### 宣告敘述與變數

```c++
#include <iostream>                          // a PREPROCESSOR directive

/* a C-style comment */
int main() {                                 // function header  { start of function body
    using namespace std;                     // make difinitions visible
    cout << "Come up and C++ me some time."; // message
    cout << endl;                            // start a new line 換行
    cout << "You won't regret it!" << endl;  // more output

    std::cout << "Come up and C++ me some time."; 
    // if to omit "using namespace std;", one needs to add prefix "std::"
    std::cout << std::endl;                  // << insertion operator 「插入運算子」
    /* nowadays "using namespace std;" is considered lazy approach, 
	   optimized declaration as below: */
    using std::cout;                // make cout available
    using std::endl;                // make endl available
    using std::cin;                 // make cin available

    cout << "Jupiter is a large planet.\n";        // displays sentence, goes to next line
    cout << "Jupiter is a large planet." << endl;  // displays sentence, goes to next line

    cout << "C++宣告變數的風格是，將變數宣告在最接近第一次使用的地方";
    return 0;                                // terminate main()
    // return(0); | return (0); --- valid
    // return0;                 --- invalid

}                                            // end of function body
```

---

### 指定敘述

```c++
#include <iostream>

int main() {
    using namespace std;
    int carrot = 4;
    int steinway = carrot;
    steinway -= 1;
    cout << steinway - 1;  // cout 必須將整數格式的數字轉成字元再印出, cout 可以辨識型別
    cout < endl;

    printf("Printing a string: %s\n", "25");  // %s - 列印字串
    printf("Pringting a integer: %d\n", 25);  // %d - 整數
}
```

---

### 其它的 C++ 敘述 — 使用 `cin`

```c++
#include <iostream>

int main() {
    using namespace std;
    int carrots;

    cout << "How many carrots do you have?" << endl;
    cin >> carrots;  // C++ input (pronounced as "see-in"
    cout << "Here are two more. ";
    carrots = carrots + 2;
    // the next line concatenates output
    cout << "Now you have " << carrots << " carrots." << endl;
    return 0;
}
```

+ C++ 將輸出視為從程式流出的字元串流，將輸入視為流入程式的字元串流

+ `<<` 運算子將字元送出到輸出串流

+ `>>` 運算子從輸入串流（input stream）取得字元

+ `cin` 和 `cout` 一樣聰明，會將從鍵盤輸入的字元序列，轉成接收變數可接收的形式

+ `<<` 運算子可以結合 (combine) 或連結 (concatenate) 輸出

  + `cout << "Now you have " << carrots << " carrots." << endl;`

  + 以上輸出相當於以下程式碼輸出

    + ```C++
      cout << "Now you have ";
      cout << carrots;
      cout << " carrots";
      cout << endl;
      ```

    + C++ 格式非常自由，語法單元之間加入換行字元和空白字元，並不會影響程式

    + ```c++
      cout << "Now you have "
           << carrots
           << " carrots."
           << endl;
      ```

### 有回傳值的函數

+ 函數呼叫 function call : `sqrt(6.25);`

+ 被呼叫的函數 called function: `sqrt` 的程式碼

+ 其它：有多個引數、沒有引數、沒有回傳值（傳送至螢幕）

+ 有些程式語言稱有回傳值的函數為**函數**（function），無回傳值者稱為**程序**（procedure）或**副程式**（subroutine）。但 C++ 和 C 都稱為函數。

  ```c++
  #include <iostream>
  void simon(int);  // 自定義函數原型：一般寫在 main() 定義之前
  
  int main()
  {
      using namespace std;
      simon(3);
      cout << "Pick an integer";
      int count;
      cin >> count;
      simon(count);
      cout << "Done!" << endl;
      return 0;
  }
  
  void simon(int n)  // 函數主體：最簡單方式是把它放在 main() 之後
  {
      using namespace std;
      cout << "Simon says touch your toes " << n << " times." <<endl;
  }
  ```

+ C 和 C++ 均不允許函數主體內再定義其它函數（Pascal 可），每個函數定義均須獨立

  ```c++
  #include <iostream>
  using namespace std;
  
  voin simon(int);   // 函數原型
  double taxes(double);
  
  int main()    // 函數 #1
  {
      ...
      return 0;
  }
  voin simon(int n)  // 函數 #2
  {
      ...
  }
  double taxes(double t)  // 函數 #3
  {
      ...
      return 2 * t;
  }
  ```

  

  + 為何 main() 被設計成有回傳值？(`int main()` 與 `return 0;`) 
    + 可以想像成電腦作業系統（如 UNIX 或 Windows）接收`main()`回傳值
    + UNIX 的 shell 描述檔和 Windows 命令列提示的批次檔，可執行程式並檢查其回傳值，這些值稱為**離開值**（exit value）
    + 慣例：正常結束回傳`0`，有問題時傳回非 0 值

### 在多重函數的程式中使用 using 指令

+ 將 `using namspace std` 指令放在兩個函數的外面與上面

  ```c++
  #include <iostream>
  using namespace std;   // 影響此檔案中的所有函式
  void simon(int); 
  
  int main()
  {
      ...
      return 0;
  }
  
  void simon(int n) 
  {   
      ...
  }
  ```

+ 使用 std 名稱空間 的四種方式

  1. 將 `using namespace std;` 放在函式定義內容的上面，檔案中每個函式都能用

  2. 將 `using namespace std;` 放在某一個函數中，只有該函數可使用 std名稱空間 的所有內容 

  3. 某一函式中如果沒放 `using namespace std;` 可以這樣撰寫指令 `using std::cout;`

  4. 可以完全不用 `using`指令，只要在需要用 std名稱空間 的元素時，加入前置 `std::`即可

     ```c++
     std::cout << "I'm using cout and endl from the std namespace" << std::endl;
     ```

+ C++ 內建輸出入功能的物件（`cin`和`cout`）來自 istream 和 ostream類別 (under file `iostream`)

  + 插入運算子 `<<` ：`ostream`類別定義，將資料放入輸出串流

  + 萃取運算子 `>>`：從輸出串流讀取資訊
  
    

## Chapter 3

C++ 內建資料型態：基本型態（整數、浮點數）、複合型態（compound types）

### 簡單變數

+ 程式需紀錄三種基本屬性：
  1. 資訊儲存的位置
  2. 儲存的內容
  3. 資訊的類別

#### 變數名稱

+ C++ 命名規則

  1. 變數名稱可使用字母、數字、以及底線`_`字元
  2. 變數名稱第一個字元不可為數字
  3. 大小寫字元有區別
  4. 不可用 C++ 關鍵字當變數名稱
  5.  保留給系統使用：兩個底線，或一個底線開頭後接大寫字母
     + `__hazel_nut` 或 `_ChestNut` 
     + 保留給編譯程式或者保留給系統的全域識別字之用
  6. C++ 的變數名稱沒有長度限制，名稱中的所有字元都是有意義的
     + 相較 ANSI C(C99) C語言能識別的變數名稱最大長度為 63 字元

+ 變數名稱的命名

  1. 有些人會在變數名稱加入額外層次的資訊

     + `myWeight` 整數命名為 `nMyWeight`、`intMyWeight`(雖更明確卻令人詬病)

       |      | 以空字元結尾的字元字串 str, sz | 布林值 b  | 指標 p  | 單一字元 c |
       | ---- | ------------------------------ | --------- | ------- | ---------- |
       |      | `strRain`、`szRain`            | `bIsRain` | `pRain` | `cRain`    |

  2.  更多字首命名格式範例是否採用，以團隊 / 個人需求、喜好、風格選擇

     + 確保一致性與明確性

#### 整數型態

+ C++ 不同整數型態所需的記憶體大小也不相同 
+ 有號型態（signed type）可表達正負整數，無號型態（unsigned type）不能表達負數
+ **寬度 Width**：描述整數使用的記憶量
  + 寬度由小至大 `char` < `short` < `int` < `long`
  + 每一種都包含有號及無號兩種，一共八種整數型態

#### short、int、long 及 long long 整數型態

+ 因各家電腦設計不同，C++ 只規定最小容量的彈性標準

  + `short` 整數最少是 16 位元寬
  + `int` 整數最少要和 short 一樣大
  + `long` 整數最少是 32 位元寬，而且最少要和 int 一樣大
  + `long long` 整數最少是 64 位元寬，而且最少要和 long 一樣大

+ 位元（bit）是電腦記憶體的基本單位 ( 8 bit 位元 = 1 byte 位元組)

  > 每一位元有 2 種指定，八位元可表達 256 種數值
  > 2 X 2 X 2 X 2 X 2 X 2 X 2 X 2 
  >
  > 位元組 (byte) : 衡量電腦記憶體容量的度量單位
  > 1K = 1,024 位元組、1M = 1,024K 位元組

+ C++ 定義的位元組：

  + C++ 的位元組至少由可以表示執行系統的基本字元集（character set）的連續位元組成
    + ASCII、EBCDIC 字元集系統中，1個位元組等於 8 個位元
    + Unicode 系統的話，1 個位元組等於 16 個或 32 個位元

+ 知道系統整數最大範圍：

  1. `sizeof`運算子 取得型態或變數的位元組數
  2. 在標頭檔案 `climits`（或是較舊系統的`limits.h`標頭檔）中有整數型態範圍的資訊
     + 定義符號名稱來表示不同的範圍。
       + `INT_MAX `— int 的最大值 
       + `CHAR_BIT` — 位元組的位元數
       + **初始化**的方式：使用宣告敘述指定變數的初值

  ```C++
  // limits.cpp -- some integer limits
  # include <iostream>
  # include <climits>			   // use limits.h for older systems
  int main()
  {
      using namespace std;
      int n_int = INT_MAX;        // intialize n_int to max int value
      short n_short = SHRT_MAX;   // symbols defined in limits.h file
      long n_long = LONG_MAX;
      long long n_llong = LLONG_MAX;
      
      // sizeof operator yields size of type or of variable
      cout << "int is " << sizeof (int) << " bytes." << endl;
      cout << "short is " << sizeof n_short << " bytes." << endl;
      cout << "long is " << sizeof n_long  << " bytes." << endl;
      cout << "long long is " << sizeof n_llong << " bytes." << endl;
      cout << endl;
      
      cout << "Maximum values:" << endl;
      cout << "int: " << 
  }
  ```

   



​	
