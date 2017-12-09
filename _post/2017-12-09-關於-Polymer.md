---

tags: 網站

---


# 序言

目前還很少人用 webComponent / Polymer 這個網站框架技術，網路上找到的資源超少的，所以想寫一些中文的東西。

# 簡介

## 為什麼要用 Polymer

webComponent 讓每個人可以自行定義新的 html 元件，過去 html 主要用到的是 `<div>`, `<h1>`, `<li>` 等這些東西。在 webComponent 的框架下，你只要 import 別人的東西，就可以使用新的 html 元件。

## HelloWorld



# 用途

# 原理

Polymer 或稱 webComponent 是一個很新的 web 技術，它很像 react.js, Angular.js, Vue.js 這些時下很紅的框架，但又有點不一樣，它結合了最新瀏覽器共同訂立的一些新技術，包括 customElement, shadowDom 等等，使我們可以藉這個技術建立一些能在多網站、平台上呈現一致的模組。

# 環境安裝
你的電腦可以透過一些設定，來打造快速撰寫 webComponent  的環境，要裝的東西如下：

* nodejs, npm, bower, polymer-cli (前三個主要是為了裝 polymer-cli)

# 設定

# 手把手寫一個 element

# 手把手寫一個 app

# 附註

## element 在 javascript 一些注意事項

### 如何取到 ShadowDOM 裡面的東西？

使用 `Polymer.dom` 函式可以取得 Dom 元件
假設 Item 是一個包含 ShadowDOM 的物件 
`Polymer.dom(Item)`和`Polymer.dom(Item.root)` 不一樣

`Polymer.dom(Item)` 會取到外部的東西，稱之為 **LightDOM**

`Polymer.dom(Item.root)` 會取到 template 的東西，稱為 **ShadowDOM**

另外，如果寫在 template 裡面時的物件有加 `id="myElement"` 這個屬性，在 Polymer 建構類型的函式裡，除了 constructor 之外，都可以用 `this.$.myElement` 取到該物件唷。

### 在  Polymer 建構式可以用的 function：

參考：https://developer.mozilla.org/en-US/docs/Web/Web_Components/Custom_Elements

1. constructor 構築元件的，這時還不能用 `this.$`
2. connectedCallback 
3. attributeChangedCallback(attributeName, oldValue, newValue, namespace)

### 在 imported html  裡面的東西，怎麼取到它的 host 

假設有個 item  在一個 shadowRoot 裡面，它可以用 `item.getRootNode()` 取到它的第一個父層 shadowRoot，
但這時還不是 Polymer 元件，再取 `item.getRootNode().host` 才是 shadowRoot 外包的一層元件。


### 關於事件

一開始是這樣寫的

```html
<people-item data-item=[[item]] on-click="doClick"></people-item>
```

用了 on-click,  onmouseover 那麼久，覺得還是都不要用這些比較好，因為有的時候函式是寫在新建立的 Polymer 下面，但有的時候又不是，疑似是在 `window` 。為避免這種情況，請直接用
```js
Polymer.dom(this.root).querySelector('xxx-xxx').addEventListener('click',xxxfunc)
```
或是 
```js
this.$.someItemm.addEventListener('click',xxxfunc)
```
`addEventListener` 有個好處，它裡面的 function 取 this 就會直接取到該元件，我寫在 on-click 讀了半天都只取到寫 function 的那個物件。

P.S. 如果是 dom-repeat, dom-if 這種動態加入的，不會加到 `this.$` 裡面喔


## 在 Drupal 可以用的做法


## 參考


> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTgwMzQ0NTk3XX0=
-->