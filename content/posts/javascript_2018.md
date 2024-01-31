---
title: "JavaScript - dashing dashshound"
date: 2023-03-15T10:09:41+08:00
author: "celine"
tags: ["JavaScript"]
categories: ["StudyNote"]
---

> 下周工作開始要用到 JavaScript, jQuery, HTML, CSS，特此複習

## 🦊Basic_1

#### 變數

第一個字母必須為英文字母、底線_、錢字號$

後面可接英文字母、底線_、錢字號$、數字

大小寫有區別

關鍵字、保留字不能當變數名稱，詳閱[MDN](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Lexical_grammar#ecmascript_2015_%E4%BF%9D%E7%95%99%E9%97%9C%E9%8D%B5%E5%AD%97)

JavaScript 1.3 起支援Unicode，可用中文(盡量避免)

ES6之後可用var(變數)、let(作用範圍與var不同)、const

**所有沒透過var宣告的變數，都會自動變成全域變數**

```javascript
m = 1;
console.log(m); //未宣告var會有後遺症
```

已經被 var 宣告過的變數可以再次被宣告 [合法]

```js
var m = 1;
var m = 2;
var m = 3;
```

透過 let 宣告的變數不能被再次宣告

```js
let m = 1;
let m = 2;
//⚠️Uncaught SyntaxError: Identifier 'm' has already been declared.
```

#### 變數資料型別

變數本身不帶有資料型別資訊，其中的值/物件才有

##### 變數沒有型別，值才有

JavaScript支援型別

1. 基本型別 Primitives
   + string, number, boolean, null, undefined
   + Symbol (ES6之後多的型別)
2. 物件型別 Object

可以用 `typeof` 運算子協助判斷型別

```javascript
typeof true;   // 'boolean'
typeof 'Jean'; // 'string'
typeof 123;    // 'number'
typeof { };    // 'object'
typeof [ ];    // 'object'
```

xxx例外

```js
typeof window.alert;  // 'function'
typeof null;          // 'object'
```

#### String 字串

沒有 char(字元)

字串用 '' 或者 "" 包夾住，單引號雙引號在JavaScript裡面沒啥差異

如果單引號裡面有包單引號，可用`\`跳脫字元或者改用雙引號包起來

```js
var str = 'Let's go!'; // error
var str = "Let's go!";
var str = 'Let\'s go!';
```

多組字串可用`+`連接

```js
var hola = 'Hola, ' + 'ikea';
```

多行字串可用`\`（反斜線）連接

```js
var hallo = ' first row \
			  second row \
			  third row ';
```

#### String literal 樣板字面值

使用反引號` 包覆字串，由一般字串、$、{ } 組成，提升以下靈活性

1. 支援多行字串

   ```js
   var bienvenueV1 = '1st row \
   				   2nd row \
   				   3rd row';
   var bienvenueV2 = `1st row
   				   2nd row
   				   3rd row`;
   // 不用再加 \ 符號換行
   ```

2. 允許變數直接嵌入字串

   可以用 `${}`取代使用`+`做字串連接

   也可以內嵌運算式

   ```js
   var age = 23;
   var str = `I am a ${age} year-old engineer.`;
   
   var a = 5;
   var b = 10;
   console.log(`Fifteen is ${a + b} and not ${2 * a + b}.`);
   ```

#### number 數字

1. 





#### boolean 布林值





#### null 以及 undefined





---

### 物件、陣列、型別判斷



#### Object 物件



#### 物件及屬性



#### 物件屬性存取



#### 屬性新增



#### 屬性刪除



#### 判斷屬性是否存在



#### Array陣列



#### typeof 型別判斷



#### 如何判斷是否為陣列





---

### 運算式與運算子

#### Expression 運算式 & Operator 運算子



#### 四則運算



#### 一元運算



#### 比較運算





---

## 🦊Basic_2

### 自動轉型規則





---

### 流程判斷迴圈





---

### 函式基本概念





---

### 傳值還是傳址





---

## JavaScript @ browser (1<sup>st</sup>)

### 瀏覽器的物件模型：DOM、BOM





### 透過 DOM API 查找&遍歷節點



#### `document.getElementsBy**`、`document.querySelector`/`document.querySelectorAll`差異

+ `document.getElementById`,`document.querySelector`取得的只會有一個元素/節點，不會有index與length屬性
+ `document.getElementsByXXX`回傳HTMLCollection, `document.querySelectorAll`回傳NodeList
  + HTMLCollection 收集HTML element節點
  + NodeList 除了收集HTML element節點，也包含文字節點/屬性節點
  + 這兩種都可以用陣列索引的方式存取內容
+ HTMLCollection/NodeList 大部分情況下是即時更新的，但透過document.querySelector/document.querySelectorAll 取得的 NodeList 為靜態的

Example

###### 1

```html
<body>
    <div id="outer">
        <div id="inner">inner</div>
    </div>
</body>
<script>
    //id為outer的標籤
    var outerDiv = document.getElementById('outer');
    //所有的<div>標籤
    var allDivs = document.getElementsByTagName('div');
    console.log(allDivs.length); // 2
    //清空<div id="outer">下的節點
    outerDiv.innerHTML = ''; 
    //1 - <div id="outer">下面的節點被清空
    console.log(allDivs.length); 
</script>
```

###### 2 改成document.querySelector寫法

```html
<body>
    <div id="outer">
        <div id="inner">inner</div>
    </div>
</body>
<script>
    //id為outer的標籤
    var outerDiv = document.getElementById('outer');
    //所有的<div>標籤
    var allDivs = document.querySelectorAll('div');
    console.log(allDivs.length); // 2
    //清空<div id="outer">下的節點
    outerDiv.innerHTML = ''; 
    //document.querySelector回傳靜態的 NodeList，不受outerDiv更新影響
    console.log(allDivs.length); 
</script>
```

### DOM Node 建立、刪除、修改、外觀

##### document.createElement(tagName)

建立一個新的元素，再搭配`appendChild()`、`insertBefore()`或`replaceChild()`等方法將新元素加入到指定位置

也可以對新元素設定屬性

```javascript
let newDiv = document.createElement('div');
newDiv.id = "anOrdinaryDiv";
newDiv.className = "aNormalClass";
```

##### document.createTextNode()

建立文字節點的方法，直接在方法加入字串

此方法跟`createElement()`一樣，在加入到某節點之前我們看不到

```js
let newDiv = document.createElement('div');
//建立 textNode 文字節點
let textNode = document.createTextNode("Hallo Jean!");
//將texNode加入到newDiv
newDiv.appendChild(textNode);
```

##### document.createDocumentFragment()

操作DocumentFragment與直接操作DOM最關鍵區別：

+ DocumentFragment不是真實的DOM結構
+ DocumentFragment的變動不會影響目前網頁文件，也不會導致回流(reflow)或引起任何影響效能的情況發生
+ 如果需要進行大量DOM操作，用DocumentFragment的效能會比直接操作DOM好很多

```html
<body>
    <ul id="myList"></ul>
</body>
<script>
    //取外層容器myList
    let ul = document.getElementById("myList");
    let fragment = document.createDocumentFragment();

    //建一個DocumentFragment, 可看成一個虛擬容器
    for (let i = 0; i < 3; i++) {
        //生成新的li,加入文字後置入fragment裡
        let li = document.createElement("li");
        li.appendChild(document.createTextNode("Item " + (i +1)));
        fragment.appendChild(li);
    }

    //將組合完成的fragment放進ul容器
    ul.appendChild(fragment);
</script>
```

##### document.write()

+ 可以輸入單純的字串或者HTML標籤

  ```java
  document.write("<h1>Jean Jing Jingle</h1>");
  ```

  也可以用來新增script標籤，但要注意結尾標籤寫法`<\/script>`

  ```javascript
  //</script>會變成目前script區塊的結束，導致錯誤
  document.write("<script type=\"text\javascript\" src=\"file.js\">" +
                "</script>");
  
  //修改方法如下
  document.write("<script type=\"text\javascript\" src=\"file.js\">" +
                "<\/script>");
  ```

+ [注意] 當網頁已經讀取完才執行 document.write()，則裡面的內容會**完全覆蓋掉目前網頁**

  ```js
  window.onload = function() {
      document.write("Jean Jin Jingle");
  };
  ```

  > `window.onload`表示網頁已載入完成，此時無論原本網頁有什麼內容，都會被 Jean Jin Jingle 覆蓋過去

#### DOM節點的修改與刪除

如何將新建好的DOM節點，置入到所指定的位置？

##### NODE.appendChild(childNode)

```html
<body>
    <ul id="myList">
        <li>Item 01</li>
        <li>Item 02</li>
        <li>Item 03</li>
    </ul>
</body>
<script>
    //取得容器
    let myList = document.getElementById('myList');
    //建立新的 <li> 元素
    let newList = document.createElement('li');
    //建立 textNode 文字節點
    let textNode = document.createTextNode("a5a5aa555oo.");
    
    // 透過 appendChild 將 textNode 加入至 newList
    newList.appendChild(textNode);

    // 透過 appendChild 將 newList 加入至 myList
    myList.appendChild(newList);
</script>
```

##### NODE.insertBefore(newNode, refNode)

> 將新節點 (newNode) 插入至指定的 refNode 節點的前面

##### NODE.replaceChild(newChildNode, oldChildNode)

> 將原本的 oldChildNode 替換成指定的 newChildNode

##### NODE.removeChild(childNode)

> 將指定的 childNode 子節點移除
>
> ```html
> <body>
>     <ul id="myList">
>         <li>Item 01</li>
>         <li>Item 02</li>
>         <li>Item 03</li>
>     </ul>
> </body>
> <script>
>     //取得容器
>     let myList = document.getElementById('myList');
>     //取得"<li>Item 02</li>"的元素
>     let removeNode = document.querySelectorAll('li')[1];
>     //將myList下的removeNode節點移除
>     myList.removeChild(removeNode);
> 
>     //將myList裡面的HTML內容一口氣清空
>     myList.innerHTML = '';
> </script>
> ```

#### DOM 節點的樣式

常見使用 JavaScript 修改 CSS 的方法有以下三種：

+ 直接修改元素的 style 屬性
+ 透過修改 className 屬性或 classList 提供的 api 修改元素的 class 屬性
+ 利用 JavaScript 直接寫入 CSS 樣式

##### (1) 修改元素 style 屬性

> 屬於修改行內樣式的類型
>
> ```html
> <body>
>     <div class="box"></div>
> </body>
> <script>
>     let box = document.querySelector('.box');
>     box.style.color = 'red';
> </script>
> ```
>
> 這種寫法是透過 DOM api 修改元素行內樣式，優先級會高於多數的其他樣式寫法(除了`!important`)
>
> ⭐注意，CSS屬性名稱`-`kebab-case 要改成駝峰式寫法，JavaScript的變數以及物件名稱不允許用破折號
>
> ```js
> // style.font-size Error ❌
> box.style.font-size = '20px';
> 
> // camel case ✅
> box.style.fontSize = '20px';
> ```

##### (2) 修改元素 class 屬性

> 修改元素style屬性(行內樣式)容易帶來不易維護/除錯的問題，基於關注點分離的原則，實務上還是會把預先定義的樣式統一在CSS的class內處理
>
> ⭐注意，因為`class`是JavaScript保留的關鍵字，所以是透過 **className** 操作

# 🥝page150🚧

##### 用 JavaScript 直接寫入 CSS 樣式





### BOM 與 window 物件



#### BOM(Browser Object Model 瀏覽器物件模型)



#### window 物件



#### 與網址列相關: location 與 history





---

## JavaScript @ browser (2<sup>nd</sup>)



### 事件機制原理



#### 事件流程

#### 事件冒泡

#### 事件捕獲

#### 事件註冊綁定

##### on-event處理器（HTML屬性）



##### on-event處理器（非HTML屬性）



#### 事件監聽 EventTarget.addEventListener()





#### 關於addEventListener的第三個參數





#### 非侵入式 JavaScript

在頁面載入到瀏覽器中，才去相應地執行事件設定。

是在原有基礎增加**行為層分離**，做到關注點分離，避免在HTML標籤裡面夾一對`onchange`、`onclick`屬性來註冊 Javascript 事件。

```js
// window 的 DOMContentLoaded 事件: 網頁上的 DOM 已經解析完畢
window.addEventListener("DOMContentLoaded", function(event) {
   var btn = document.getElementById('btn');
    btn.addEventListener('click', clickHandler, false);
});
```



### 網頁事件與生命週期



#### 事件種類



#### 網頁介面相關事件

+ load()事件: 網頁資源(CSS、JS、圖片)全數載入完畢後才觸發

+ unload()事件: 在離開頁面或重新整理時觸發

+ beforeunload()事件: 會跳出對話框詢問是否要離開目前頁面

+ error()事件: document或image載入錯誤時觸發

  + 大多數事件註冊建議使用非侵入式JavaScript，
    只有error事件適合以on-event handler寫法來處理

    ```html
    <img sroc="image.jpg" onerror="this.src='default.jpg'">
    ```

+ resize()事件: 瀏覽器`window`或指定元素`element`尺寸變更時

+ fullscreenchange()事件: 使用

+ scroll

+ DOMContentLoaded()

#### 滑鼠相關事件



#### 鍵盤相關事件



#### 表單相關事件



#### 特殊事件

##### Composition Event（組成事件）

##### 剪貼簿事件





#### 自定義事件



### 隱藏在事件之中的秘密

註冊事件的方法`addEventListener()`的三個參數

1. 事件名稱
2. 事件處理器 Event Handler
3. 捕獲或冒泡的機制切換

#### 隱藏在Handler中的"event"



#### 阻擋預設行為event.preventDefault()

用來取消預設行為，但不會阻止事件向上傳遞（事件冒泡）



#### 阻擋事件冒泡傳遞event.stopPropagation()



#### 在事件中找回自己



#### 事件指派（Event Delegation）









---

## 函式、物件、原型鍊（上）

### 再談函式與參數

#### 一級函式（First class functions）

可以將函式存在變數、物件甚至是陣列之中

```js
// 函式存進變數中
const funcA = function () {};
// 函式存進陣列裡
const funcB = [function () {}];
/* 函式存入物件屬性
   呼叫時執行 funcC.method() 或者
*/
const funcC = { method: function () {} };
```

函式是一種物件，值也是物件

```js
// 把函式當作參數，傳入到另外一個函式中
const funcD = function (func) {
    return func;
};

// 另一個函式: 存放的是 funcD，參數為一個匿名函式
const runFuncPassedToFuncD = funcD(function () { console.log("Hi"); });

// 呼叫另一個函式
runFuncPassedToFuncD();

// 函式是物件的一種，也可以有自己的屬性
const funcE = function () {};
funcE.answer = "yoshimite";
```

#### 函式的參數

\<函式名稱> \<小括號(> `參數`> \<小括號)>

#### ES6的箭頭函式

Arrow Function expression features:

+ 韓式寫法更簡短
+ this變數強制綁定

即使箭頭函式不需要參數時，前面的小括號也不能省略

```js
const bienvenue = () => console.log("Bonjour, bonsoir, bonne nuit");
bienvenue(); // "Bonjour, bonsoir, bonne nuit"
```

只有一個參數時可省略前面的小括號

```js
const willkommen = darcy => console.log(darcy);
willkommen("Mr. Darcy"); // Mr. Darcy
```



#### `call()` 與 `apply()`

1. call(): 
   + param1: this (可傳入null)
   + params ...: 呼叫函式的參數，以`, `相隔
2. apply():
   + param1: this 
   + param2: 呼叫函式的參數，用一個陣列包覆

> JavaScript不會檢查代入的參數數量，所以如果沒有傳入制，那麼未指定的參數預設就會是undefined



#### arguments 物件

arguments代表實際傳入的參數。

即使函式定義只有兩個參數，還是可以透過 arguments 取得多餘的傳入參數。

+ 雖然像是陣列，實際上只是帶有索引特性的物件，並且內建length屬性

+ 沒有陣列才有的`.map()`、`.filter()`方法

+ 內建另外一個屬性`callee`，指的是目前執行的函式

  + 執行遞迴(在函式內呼叫本函式)可透過`arguments.callee()`達成

+ 可以透過`slice()`或者 `Array.from`(ES6) 將arguments轉成陣列

  ```js
  const args = Array.prototype.slice.call(arugments);
  //or
  const args = [].slice.call(arguments);
  //or in ES6
  const args = Array.from(arguments);
  ```



#### 其餘參數(rest parameter)

`...args` : 

+ 當函式的最後一個參數以...作為開頭來命名，會被視為一個陣列，並且將所有參數存入這個陣列中

+ 可以使用 arguments 沒有的 reduce 方法，讓此 plus 函式更加簡短

  ```js
  const plus = (...args) => args.reduce((a, b) => a + b);
  plus(1,2);
  pluc(1,2,3,4);
  ```



#### 以物件作為參數

將多個參數用一個物件包裝起來

```js
var staff = {
    firstName: "Ginnelle",
    lastName: "Yeh",
    phone: "0912345678",
    email: "merde@1771.com",
    gender: "male",
    addres: "PseudoStreet"
};
addPerson(staff);
```



#### 參數的預設檢查

避免少傳遞的參數變成 undefined ，結果變成 NaN 的時候...

小花可以這樣做

```js
const plus = function (numA, numB) {
    numA = numA || 0;
    numB = numB || 0;
    
    return numA + numB;
};
```

比較嚴謹的寫法

```js
const plus = function (numA, numB) {
    numA = (typeof numA !== 'undefined') ? numA : 0;
    numB = (typeof numB !== 'undefined') ? numB : 0;
    return numA + numB;
}
```

更好的寫法 (after ES6) - 替參數指定預設值

```js
const plus = function (numA = 0, numB = 0) {
    return numA + numB;
};
```

---

### Callback Function 與 IIFE

# page 226🚧🚧🚧🥝🥝🥝

### 從 Callback 到 Promise



### 從 setTimeout 與 setInterval 理解 EventQueue













































