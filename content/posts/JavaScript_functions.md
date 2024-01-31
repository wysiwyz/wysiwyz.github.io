---
title: "JavaScript 常見內建方法"
date: 2023-04-11T10:50:51+08:00
author: "celine"
tags: ["JavaScript"]
categories: ["StudyNote"]
---

> 這裡紀錄上班看遇到的常用 JavaScript 內建方法 ╚(•⌂•)╝

### Object.prototype.hasOwnProperty(param)

回傳 boolean 值

+ true: 如果該param是該物件的直接屬性(direct property)，即便值為 null 或者 undefined
+ false: 如果屬性是繼承來的，或從沒被宣告

```javascript
_self._getCurrencyFlagPath = (function () {
    var imageList = Object.freeze({
        AUD: 'images/lilyRose/flag_AUD.svg', 
        CAD: 'images/lilyRose/flag_CAD.svg',
        EUR: 'images/lilyRose/flag_EUR.svg',
        JPY: 'images/lilyRose/flag_JPY.svg',
        TWD: 'images/lilyRose/flag_TWD.svg',
        NTD: 'images/lilyRose/flag_TWD.svg'
    });
    return function (currency) {
        return imageList.hasOwnProperty(currency) ? imageList[currency] : '';
    }
})();
```

### Object.freeze() 淺凍結

Freezing an object prevents extensions and makes existing properties non-writable and non-configurable.

也就是該物件的屬性無法被修改、新增、移除 

```js
var activeList = Object.freeze({
    _A0101_01: 'tops',
    _A0201_01: '',
    _A0419_01: '',
    _A0420_01: '',
    _A0420_02: '',
    _A0421_01: ''
});
```

### Object.assign()

🚧

```js
var result = Object.assign(value, {
    fundName: value.fundName,
    applyDate: value.applyDate,
    tradeNo: value.certNo,
    modStatusi18n: getModSomething(value.modSth)
});
```







---

## JQuery

#### .removeClass(\<要移除的class名>)

```js
var set = {
    modal: {
        showCloseBtn: false,
        showConfirmBtn: false,
        showCancelBtn: false,
        confirmAddOnClass: false,
        cancelAddOnClass: false,
        confirmText: '',
        cancelText: ''
    },
    uniqueId: '',
    modalTItle: '',
    modalContent: ''
};
...
var $selector = jq('#' + set.uniqueId);
```

從被選取的元素中移除一個或多個className

如果 removeClass() 沒有傳入任何參數，則從被選元素中刪除所有className



#### .addClass()

```js
$selector.find('.confirm').addClass('on');
```

從被選的元素中加入一個或多個className，

加多個className的話，使用空格隔開



#### .text()

```js
$selector.find('.cancel').text(set.modal.cancelText);
```

與.html() 差異在於，`.text()`可以用在**XML**和**HTML**文件

#### .html()

```js
$selector.find('.alert_popup_content.remind_content').html(set.modalContent);
```

> ##### 比較 `.html()`、`.text()`、`.val()`
>
> html(): 獲得標籤裡面所有嵌套的標籤以及內容
>
> ​		用來讀取元素的HTML內容（包括HTML標籤）
>
> ​		如果應用在多個元素上，只能讀第一個元素
>
> text(): 獲得標籤裡面所有的內容，包括子標籤
>
> ​		用來讀取元素的純文本內容，包含其後代元素
>
> ​		如果應用在多個元素上，會讀取所有選中元素的文本內容
>
> val(): 針對具有value屬性的標籤，一般為input標籤
>
> ​		只能使用在表單元素上，用來讀取表單元素的value值
>
> ​		如果應用在多個元素上，只能讀第一個表單元素的value值
>
> ⭐ 這三種方法都是用來替換選中元素的內容
>
> ⭐ 這三種方法都可以用callback func回調函數的返回值，動態改變多個元素的內容

#### .stopPropagation()

🚧停止事件

```js
var $cc = $queryBean.find('.cc');
$cc.off().on('click', function (e) {
    e.stopPropagation();
    /* 以下略 */
});
```





















































