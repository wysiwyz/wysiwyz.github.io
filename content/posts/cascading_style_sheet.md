---
title: "Cascading style sheet"
date: 2023-04-10T09:53:01+08:00
author: "celine"
tags: ["css"]
categories: ["StudyNote"]
---

> 這一兩天上班在研究既有程式碼的CSS寫法，因為之後的需求要拉皮
>
> 看到什麼就記下來，之後複習的話再分類

### overflow: hidden

直接隱藏超出範圍的文字

### overflow: auto

與 `overflow: scroll` 類似，但 `overflow:auto` 只有在有需要時才會出現 scroll bar

參考: [css overflow](https://www.w3schools.com/css/css_overflow.asp)

### list-style-type

設定清單的項目編碼符號

```css
ul {
    list-style-type: none;
    /*
    list-style-type: disc;   //實心黑圓點
    list-style-type: circle; //空心圓圈
    list-style-type: square; 
    list-style-type: decimal;
    list-style-type: georgian;
    list-style-type: "\1F44D"; //👍 
    */
}
```

###  `.` 以及 `#`

- `#foo {}`：id selector，像是`id=foo`
- `.foo {}`：class selector，例如`class="foo"`
  - 一個元素可以有多個class，用空格區別
    `class="foo bar"`

### #yIcon `>` img

只選擇 id 為 yIcon 元素底下的直接子對象

```css
#homeIcon > img,
#backIcon > img,
#fundHomeIcon > img {
    width: 30px;
    height: 44px;
    float: left;
}
```

> `#yIcon ul` 比較 `#yIcon > ul`:
>
> `#yIcon > ul`只會選中 id 名為 `yIcon` 的`div`下的直接子對象`ul`，不會指向`ul`下第一個`li`的子對象`ul`
>
> ```html
>    <div id="yIcon">
>       <ul>
>          <li> List Item
>            <ul>
>               <li> Excluded in > selector </li>
>            </ul>
>          </li>
>          <li> List Item </li>
>          <li> List Item </li>
>          <li> List Item </li>
>       </ul>
>    </div>
> ```

### #yIcon img

選擇 id 為 yIcon 元素筆項的圖片對象

```css
.nav_icon img {
    width: 20px;
    vertical-align: middle;
}
```

### ul `+` p

相鄰兄弟選擇器，只會選擇第一個`ul`之後的`p`段落標籤

```html
<style>
    ul + p {
        color: red;
    }
</style>
<body>
    <div id="container">
        <ul>
            <li>a</li>
            <li>j</li>
            <li>i</li>
            <li>n</li>
        </ul>
        <p>
            This paragraph will be selected and marked red per the css style.
        </p>
    </div>
</body>
```

### display: flex

+ display: flex | inline-flex;
  + 需要一開始宣告此，相關屬性才能有作用
  + inline-flex作用: flex + inline-block
+ flex-direction: column | column-reverse | row | row-reverse;
  + flex items由上而下
+ justify-content: center
  + 使 container 內的 flex items 置中對齊

參考：[A complete guide to Flexbox](https://css-tricks.com/snippets/css/a-guide-to-flexbox/) 、[Casper說CSS Flex屬性](https://www.casper.tw/css/2017/07/21/css-flex/)

```CSS
.agreeTitle {
    display: flex;
    flex-direction: column;
    justify-content: center;
    text-align: center;
    border-bottom: 1px solid rgb(149, 149, 149);
    padding: 15px 9px;
}
```

### 固定大小寫: text-transform

常用的有 capitalize | uppercase | lowercase | none | full-width(全型) ...

參考: [text-transform at MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/text-transform)

```css
.uppercase {
    text-transform: uppercase;
}

.lowercase {
    text-transform: lowercase;
}
```

### 元素內空白格：white-space

white-space: normal | nowrap | pre | pre-wrap | pre-line | break-spaces;

normal: 遇空白` `或`-`時換行，多個空格合併為一個

nowrap: 多個空格合併為一個，但不斷行(suppress text wrapping)

pre: 保留多個空白格，只有\<br>和new line character之後斷行

參考：[white-space at MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/white-space#collapsing_of_white_space)

```css
/* 強迫不斷行 */
.nowrap {
    white-space: nowrap;
}
```

### 排版: display

display: inline | inline-block | block;

+ inline: span標籤預設值
  + 可依照元素內容決定寬高
  + 不會占滿容器橫向剩餘空間
  + 可以與其他元素並列(同一行)
+ inline-block:
  + 可依照元素內容決定寬高
  + 不會占滿容器橫向剩餘空間
  + **可額外**透過 width、height、max-width、max-height 等 CSS 屬性來**更改該元素的寬高**
+ block:
  + 具有 inline-block 特性
  + 在元素後會自動增加一個 line-break (整個高度都是它的, 其餘留白)

```css
.bigRadiusBtn {
    margin: 0px auto;
    background-color: rgb(0, 170, 230);
    color: white;
    width: 120px;
    height: 1.8em;
    border-radius: 50px;
    font-weight: bold;
    display: inline-block;
}
```

### W:not(:checked) 、Y:checked、Z:after

+ `:checked`

  + pseudo class 的一種，用來設定radio、checkbox或者option元素選擇過的格式

+ `:not`與`:checked`合併

  + 設定還沒選擇時的格式，`input:not(:checked) {}`

+ `.w:after`

  + 在選擇的某元素(.w)之後插入一些東西

    ```html
    <style>
        .www:after {
            content: " - Good morning Good afternoon Good evening";
        }
    </style>
    <body>
        <p class="www">WaterMelonHun</p>
        <p class="www">AhaJingleCitrus</p>
    </body>
    ```

    > ##### `::` vs `:`
    >
    > `::`是比較新式的寫法，如果不需要IE8支援的話，可以用此
    >
    > 除了IE8以外的所有瀏覽器都支援`::`(CSS3語法)

```css
[type='checkbox']:not(:checked) + .showBlack:before,
[type='checkbox']:checked + .showBlack:before {
    border: 1px solid rgb(0, 0, 0);
    /*⚠️ 如果不是加 content: xx，那是怎麼互動的? 🚧*/
}

[type='checkbox']:not(:checked) + .showBlack:after,
[type='checkbox']:checked + .showBlack:before {
    color: rgb(0, 0, 0);
}
```

