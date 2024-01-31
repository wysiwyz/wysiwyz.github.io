---
title: "Think in REACT"
date: 2023-10-25T21:50:51+08:00
author: "celine"
tags: ["React"]
categories: ["StudyNote"]

---

### 0. 建立 React 靜態頁面的入門方法

###### \index.html

+ 在 head 加入 react 以及 react-dom 的 script 連結 [referred doc](https://legacy.reactjs.org/docs/cdn-links.html?)

+ 在 head 加入 babel.min.js 的 script 連結 [referred doc](https://babeljs.io/docs/babel-standalone)

+ ✏️ script 除了加上 `index.js` 的檔案來源，也要記得加上 type 屬性 `text/babel`，不然無法順利 `ReactDOM.render()` 喔！

```html
<html>
    <head>
        <link rel="stylesheet" href="index.css">
        <script crossorigin src="https://unpkg.com/react@18/umd/react.development.js"></script>
        <script crossorigin src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>
        <script src="https://unpkg.com/babel-standalone@6/babel.min.js"></script>
    </head>
    <body>
        <div id="root"></div>
        <script src="index.js" type="text/babel"></script>
    </body>
</html>
```

###### \index.js

+ `ReactDOM.render()` 函式要傳入兩個參數

  1. Param1：你要渲染的內容 (what to render)
  2. Param2：要渲染在哪裡 (where would it be rendered at)

+ Param1 可以裝變數也可以直接裝 html，如果裝html的話，不接受 siblings，
  解決之道是給他們一個 parent div (只接受 single parent element)

  ```react
  const navbar = (
      <nav>
          <h1>Hailey's Howntown</h1>
          <ul>
              <li>Park</li>
              <li>School</li>
              <li>Church</li>
          </ul>
      </nav>
  )
  ReactDOM.render(navbar, document.getElementById('root'));
  
  ReactDOM.render(
      <div>
          <h1>Benjamin's Boho Island</h1>
          <p>Welcome, want some ice tea or sangria?</p>
      </div>, 
      document.getElementById('rootSec'))
  ```

  

+ 比較原生VanillaScript 與 React 寫法 (imperative v.s. declarative)

  + Imperative: 把完成任務的每個步驟描述出來，只負責照著完成

    ```javascript
    const h1 = document.createElement("h1");
    h1.textContent = "This is an imperative way to program";
    h1.className = "header";
    console.log(h1)  
    // <h1 class="header">This is an imperative way to program</h1>
    document.getElementById("root").append(h1);
    ```

  + Declarative: 告訴我要做什麼任務，我自己思考應該如何完成

    ```react
    const element =<h1 class="header">This is an imperative way to program</h1>;
    console.log(element)
    /** 
    * 印出的是 JSX 處理後的 plain old java object
    * {$$typeof: Symbol(react.element), type: 'h1', key: null, props:  {…}, …} 
    */
    ReactDOM.render(element, document.getElementById("root"));
    ```

### 1. 比較正規的 React.js 專案建立步驟

###### \index.html

1. 移除`<head>`底下的三個 unpkg 連結，index.js 後面的 `type="text/babel"`

2. 加入 package.json 的依賴，大概長得像這樣

   ```json
   {
       "name": "screaming_bob_react",
       "private": true,
       "dependencies": {
           "react": "^17.0.2",
           "react-dom": "^17.0.2"
       }
   }
   ```

3. 在 index.js 引入 react 以及 react-dom
   React - 給你寫 JSX 用的
   ReactDOM - 給你寫 ReactDOM.render() 用的

   ```js
   import React from "react"; 
   import ReactDOM from "react-dom";
   ```

   

   ##### Quiz.

   1. Why do we need to `import React from "react"` in our files?

      React is what defines JSX

   2. If I were to `console.log(page)` in index.js, what would show up?

      A JavaScript object. React elements that describe what React should eventually add to the real DOM for us.

   3. What's wrong with this code:

      ```react
      const page = (
        <h1>Hello</h1>
        <p>This is my website!</p>
      )
      ```

      We need our JSX to be nested under a single parent element.

   4. What does it mean for something to be "declarative" instead of "imperative"?

      Declarative means I can tell the computer WHAT to do and expect it to handle the details. 
      Imperative means I need to tell it HOW to do each step.

   5. What does it mean for something to be "composable"?

      We have small pieces that we can put together to make something larger/greater than the individual pieces.

##### 建立客製化的component

+ Write function that can be executed over and over again

+ Example:

  1. 要用 Pascal case
  2. function名稱用角括號包起來 `RenderDOM(<TemporaryName />, getElementById("root"))`

  ```react
  import React from "react"
  import ReactDOM from "react-dom"
  
  function TemporaryName() {
    return (
      <div>
        <img src="./react-logo.png" width="40" />
        <h1>React is fun</h1>
        <ul>
          <li>React was first released in 2013</li>
          <li>React was originally created by Jordan Walke</li>
          <li>Has well over 100K stars on GitHub</li>
          <li>Is maintained by Facebook</li>
          <li>Powers thousands of enterprise apps, including mobile apps</li>
        </ul>
      </div>
    )
  }
  ReactDOM.reander(<TemporaryName />, document.getElementById("root"))
  ```

  

  ##### Quiz.

  1. What is a React component?

     A function that returns React elements. Sometimes people refer to this as UI.

  2. What's wrong with this code?

     Any component that you created has to be in PascalCase.

     ```react
     function myComponent() {
       return (
         <small> I'm tiny text!</small>
       )
     }

  3. What's wrong with this code?

     The correct way to *call your component*, or *create an instance of your component* is to put the function name within angle brackets, such as `<Header />`

     ```react
     function Header() {
       return (
         <header>
           <nav>
             <img src="./react-logo.png" width="40px" />
           </nav>
         </header>
       )
     }
     ReactDOM.render(Header(), document.getElementById("root"))
     ```

##### 將 header 元素從 Page 組件中拆解出來

```react
function Header() {
  return (
    <header>
      <nav>
        <img src="./react-logo.png" width="40px"/>
      </nav>
    </header>
  )
}
function MainContent() {
  return (      
    <div>
      <h1>Reasons I'm excited to learn React</h1>
      <ol>
        <li>popular library</li>
        <li>hirable skillset</li>
      </ol>)
    </div>
}
function Footer() {
  return (
    <footer>
      <small>©2023 wysiwyz development. All rights reserved.</small>
    </footer>
  )
}
function Page() {
  return (
    <div>
      <Header />
			<MainContent />
      <Footer />
    </div>
  )
}
```

#### 在組件裡面設定樣式

+ 一樣用 class attribute，但在 function 裡要寫成 `className`

  ```react
  function Header() {
    return (
      <header>
        <nav className="nav">
          <img src="./react-logo.png" className="nav-logo" />
          <ul className="nav-items">
            <li>Pricing</li>
            <li>About</li>
            <li>Contact</li>
          </ul>
        </nav>
      </header>
    )
  }
  ```

  Vanilla javacript 也是用 className，如下範例

  ```javascript
  const ul = document.createElement("ul")
  ul.className = "nav-items"
  ```

  ```css
  .nav {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
  .nav-items {
    list-style: done;
    display: flex;
  }
  .nav-items > li {
    padding: 10px;
  }
  .nav-logo {
    width: 60px;
  }
  ```

  

  to be continued...





