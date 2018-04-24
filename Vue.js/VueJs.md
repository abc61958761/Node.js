## 什麼是 Vue.js 
在這個網頁開發的工程，Google 下的 AngularJS、Facebook 下的 ReactJS，因為將前端開發系統化，
開發上比較方便，所以越來越多人開始使用，而在這個資訊的時代下，變化速度很快，
目前又出現一款集 AngularJS 與 ReactJS 優點於一身的 Vue.js ， 這是一個相當輕量級的 open source JavaScript 前端框架，
將整個網頁框架元件化，管理方便而且也好上手。

## Vue.js 特色
1. 輕量
2. MVC 架構
3. Virtual DOM: V-Node(Vue.js 2.0 後才有)
4. Directives(模板語法指令)
5. Two-way Binding(資料雙向綁定)

## Web元件化系統
Web 元件化系統是 Vue.js 一個很重要的概念，我們使用官方提供的一張圖來做詳細解釋

![vuejs](https://ithelp.ithome.com.tw/upload/images/20171221/20107673a7ts98h2N5.png "Vue.js")

右圖是一個樹狀結構，Vue.js 嚮往的就是先建立好根實體(Vue instance)，再開發好底下每個小元件(Components)，慢慢往上
組合成一個完整頁面，最後全部搭起來成為一個完整專案。

每個元件部分獨立運作或相連，目的就是希望個元件之間互相擾程度越小越好，如果是父層子層互相傳遞資料或則是重用以使用過的元件，
Vue.js 都有寫好的方法可以套用

通常一個元件的html、 css、 js會全部寫在一個.vue為副檔名的檔案當中，vue-loader會編譯這個.vue的檔案，
將結果顯示出來。一個.vue檔會有三個部分，<template></template>裡面寫html、<script></script>這個tag裡面寫js、<style></style>裡面寫css。

![vuejs](https://ithelp.ithome.com.tw/upload/images/20171221/20107673VtnTXEn76d.png "Vue.js")

### 認識 Vue Instance
根實體(Vue Instance)是樹狀結構中最上面的那個點，每個Vue App都是從創建一個vue instance開始，Vue Instance是透過Vue Constructor(建構式)所產生，
在實體化時，可傳入一個選項物件(Options)，此物件包含這個vue instance需要用到的屬性，
像是掛載點(el)、資料(data)、方法(methods)、模板(template)、生命週期鉤子(hooks)等等。

```javascript
var vm = new Vue({
  // option
})
```

上方為宣告一個vue instance，變數名稱為vm，vm是view model的簡稱，Vue Instance的設計概念來自
[MVVM Pattern](https://msdn.microsoft.com/en-us/library/hh848246.aspx)。

Vue的MVVM架構如下圖，View與Model之間的溝通就透過ViewModel來互相傳遞資訊：

![vuejs](https://ithelp.ithome.com.tw/upload/images/20180117/20107673DANCbzeqVu.png "Vue.js")

Web元件化系統是Vue.js最大的特色，Vue在執行創建到銷毀Vue Instance的時候會跑一個Lifecycle。

## Instance Lifecycle

### 認識 Instance Lifecycle 與 Instance Lifecycle Hooks

![vuejs](https://ithelp.ithome.com.tw/upload/images/20171222/20107673ckgtStpvnc.png "Vue.js")

上圖為官網所繪製的Instance Lifecycle(生命週期)，Vue其實在我們下執行命令後，會做很多事情，因為它將資料(data)與UI樣板(template)綁在一起，
開發者只需要宣告好資料、填入正確的UI components以及router的path設定好後，結果就會呈現。

而在我們執行後，Vue在這個Lifecycle中，會建立Vue Instance、綁定資料、事件配置、編譯樣板、經過無限修改更新資料等步驟，
直到整個Vue Instance被銷毀(destroyed)，這個Vue Instance底下的資料、樣板、事件、元件才會解除綁定，完成一整個Lifecycle。

那什麼是Instance Lifecycle Hooks(生命週期鉤子)呢？看到上面的Lifecycle diagram，鉤子就是上圖虛線延伸出去白底紅字的8個方法，
這些鉤子的用意是在Vue在Lifecycle中做每件事情的時機點前後，可以讓你有選擇處理的方式，相當客製化。

這8個鉤子的資料型態皆為function，以下我們就介紹這8個鉤子分別可以使用的時機

#### beforeCreate
在初始化vue instance並開啟整個Lifecycle後，資料綁定與事件配置之前。目前階段還無法調用$data。
應用場景：loading進頁面的事件

#### created
vue instance創建完成，$data已可以取得，屬性與事件也已綁定好。目前階段尚未掛載el，DOM也尚未生成。

#### beforeMount
在掛載el開始之前。目前階段是相關render函式首次被調用，尚未被DOM給綁定。

#### mounted
el被剛創建好的vm.$el替換取代，並且掛載到vm上。目前階段已被DOM綁定。
應用場景：對後端發出請求或讀取新資料

#### beforeUpdate
在資料更新時調用，Virtual DOM重新render與patch之前，可以在這個階段變更資料狀態。目前階段還不會繪製view。

#### updated
資料更新後會使Virtual DOM重新render頁面。目前階段會繪製出正確的view。

#### beforeDestroy
在vue instance被銷毀前調用。目前階段還可以完全使用這個vue instance。

#### destroyed
vue instance銷毀後可以調用，調用後這個vue instance底下的資料與樣板會解除綁定，事件會取消監聽，所有子元件也會被銷毀。

**Virtual DOM是用JavaScript物件來模擬DOM Tree，操作物件以提升效能。**

## Virtual DOM & V-Node
### 認識 Virtual DOM 

**DOM**(Document Object Model)的中文翻成「文件物件模型」，是HTML、XML、SVG文件可以使用的一組API。它提供了文件結構化的表示法(樹狀結構)，
並定義讓程式可以存取並改變文件架構、風格和內容的方法，目的是為了搭起網頁與程式碼之間溝通的橋樑，將網頁與程式碼(或script)連結在一起。

**Virtual DOM**(虛擬DOM)是以JavaScript物件模擬特定DOM Tree，也就是不直接操作DOM，而是改用模擬結構的方式，
達到優化效能的目的，讓頁面刷新載入的速度變快。

下面用一張圖來看看Virtual DOM的操作原理：

![vuejs](https://ithelp.ithome.com.tw/upload/images/20180117/20107673y8HiTX3yGP.png "vuejs")

Virtual DOM不會讓瀏覽器掃描整個DOM Tree，也就是不會刷新整個頁面，它會使用DOM diff這個演算法去比較這一次跟上一次Virtual DOM的差異，
接著處理有差異的部分，然後更新需要被更新的元件。

### Vue.js實現的Virtual DOM：VNode
Vue在版本2.0之後才加入Virtual DOM，Vue的Virtual DOM是VNode，一個VNode的結構包含以下這些屬性：

1. tag：該節點的html標籤
2. data：該節點的數據資料
3. children：該節點底下的子節點
4. text：該節點的文本
5. elm：當前虛擬節點對應的真實DOM節點
6. ns：該節點的namespace
7. context：編譯範圍
8. functionalContext：函數化元件的編譯範圍
9. key：該節點的key屬性，用來辨識該節點
10. componentOptions：創建vue instance會用到的資訊
11. child：該節點對應的vue instance
12. parent：該節點的父節點
13. raw：raw html
14. isStatic：該節點是否為靜態節點
15. sRootInsert：該節點是否作為根節點插入tree，值為false
16. isComment：該節點是否作為註釋節點
17. isCloned：該節點是否為克隆節點
18. isOnce：該節點是否有v-once指令

而我們透過new實體化的VNode可分為以下幾類，下面為比較常見的五個分類，還有其他分類，就不細項列出。

1. EmptyVNode：沒有內容的註釋節點
2. TextVNode：文本節點
3. ElementVNode：普通元素節點
4. ComponentVNode：元件節點
5. CloneVNode：克隆節點，可生成上面任意類型一模一樣的副本節點
在Vue.js的實作中，如果想透過JavaScript來操作元件，我們可以使用render function來操作Virtual DOM。

## 建立環境與新增專案

### 建立環境
開始之前，電腦需要有Node.js環境與NPM(Node Package Manager)套件管理工具

裝好node環境與npm後，即可使用套件管理工具npm安裝好**Vue環境**
```
npm install vue
```

### 新增專案

透過npm全域安裝vue-cli，安裝完後即可使用vue指令
```
npm install -g vue-cli
```

初始化專案
```
vue init [template] [project_name]
```

[template] 可以採用官方提供的幾種template，若不知道有什麼template，可以下指令查看
```
vue list
```
![vuejs](https://ithelp.ithome.com.tw/upload/images/20171224/201076739P81kCGDjJ.png "vuejs")

以下使用webpack / webpack-simple來做範例專案。

### 快速建立專案：使用webpack / webpack-simple樣板

#### 認識webpack與vue-loader
1. webpack是一個前端打包工具
2. vue-loader是一個webpack的載入器(loader)，可將vue組件(.vue檔)轉換為JavaScript模組

#### 選用webpack的原因
1. 找出各項靜態資源之間的關聯性與整合，使用vue-loader將它們模組化，產出優化過後的code
2. Hot-loader：Hot-loader可以在修改與存檔好code後，就直接更新畫面，不需要按重新整理，相較live-reloader來說，方便許多

#### 建立新專案步驟
使用webpack初始化專案
```
vue init webpack [project_name]
```

進入專案資料夾
```
cd [project_name]
```

在專案資料夾底下，安裝所需要的模組
```
npm install
```

啟動http server
```
開發版，本地開發(localhost)適用，進入http://localhost:8080可看到結果
$ npm run dev
```

打開瀏覽器，網址輸入http://localhost:8080

## 熟悉Webpack專案架構
### 專案整體架構
我們使用webpack樣板初始化一個完整的vue專案，該專案資料夾內基本架構如下圖：

![vuejs](https://ithelp.ithome.com.tw/upload/images/20180117/20107673BCdwJt7xaM.png "vuejs")

> static資料夾內存放的是“真正的”靜態資源，他們不會被webpack處理。

### 認識babel與ESLint
1. babel是一個轉碼器，因為目前瀏覽器對於新型態JavaScript語法支援度不高，babel-loader可將ES6或ES7語法轉為支援度高的ES5的語法。
2. ESLint目的是改善程式碼品質，發現與修正程式碼的問題並達到一制性，所以有安裝此套件的話，
常常會在編譯執行時看到很多warning訊息，如果安裝之後想要忽略ESLint檢查，可以到.eslintignore文件中做修改。

### src資料夾底下架構
而我們主要編輯的code檔案會放在src這個資料夾內，src資料夾內架構如下

![vuejs](https://ithelp.ithome.com.tw/upload/images/20180117/2010767362LeB88tPF.png "vuejs")

從上圖可以了解到，main.js是整個專案的主要入口點，他會去連接到這個專案主要的根實體App.vue。

## Webpack 專案運作流程
在大致了解以webpack樣板建置的專案架構後，我們接下來來了解整個app運作流程。

當我們下npm run dev這個指令後，啟動http server，這個指令會同時開啟根目錄下的index.html與src資料夾內的main.js這兩個檔案

![vuejs](https://ithelp.ithome.com.tw/upload/images/20180117/201076733G2mB9i68k.png "vuejs")
![vuejs](https://ithelp.ithome.com.tw/upload/images/20180117/201076736HIAtMBSGd.png "vuejs")

而main.js會同時運行App.vue以及在router資料夾內的index.js

![vuejs](https://ithelp.ithome.com.tw/upload/images/20180117/20107673WBhdrsy9Fj.png "vuejs")

<router-view/>是路由器顯示標籤，為vue-router使用，在index.js下Router函數中所使用的UI元件皆會套用至這個標籤當中。

index.js中，可以在Router這個函數內，自定義url路徑名稱(path)，components下可以放入寫好的UI元件。

![vuejs](https://ithelp.ithome.com.tw/upload/images/20180117/20107673nKqrjUECc9.png "vuejs")

因此，如果我們要產生新的UI元件，就要寫一個.vue檔，可放置在components資料夾之下；如果我們要將這個vue元件顯示出來，
就需要到index.js中修改路由配置，下面舉例實作看看。

### 實作：新增一個顯示Hello Vue的component
在components資料夾內先新增一個testhello.vue的檔案

![vuejs](https://ithelp.ithome.com.tw/upload/images/20180117/20107673zxhxFLE1yR.png "vuejs")

然後修改index.js，新增一個新的url路徑與components

![vuejs](https://ithelp.ithome.com.tw/upload/images/20180117/20107673Ihrka25D2E.png "vuejs")

最後下指令啟動server後，瀏覽器輸入http://localhost:8080/#/testhello。

## [Directives] 資料綁定(Data Binding)
### 推薦好用的工具：Vue.js devtools

這篇介紹開始之前，最近發現一個開發vue時還不錯用的chrome插件，想推薦大家使用，這個插件是Vue.js devtools，
它可以列出components還有vue instance的屬性，方便開發及debug，當你安裝之後，在chrome的開發者工具上面的tab會多一個Vue tab，
執行vue環境並點選，點下去即可看見以下的畫面。

![vuejs](https://ithelp.ithome.com.tw/upload/images/20180117/20107673rUnGZUf1Er.png "vuejs")

### 為什麼要做資料綁定？

我們在創建vue instance與components時，通常會各自丟data在裡面，然後前端會寫一些html語法來接收這些data，
或者使用者會在browser輸入一些資訊，如果這之間沒有做資料綁定，使用者就無法在前端browser中看到你想要呈現的資訊，
也就無法進一步再操作，因此Vue提供一些指令可以來做資料綁定。

那在了解資料綁定指令有哪些之前，前面文章我們提到過創建vue instance時可以傳入選項物件(Options)，物件中可以寫入一些屬性，
這邊我們先花一些篇幅認識一個常用的屬性，```data```(資料)。

### 選項物件屬性：data
```data```可用來儲存元件內部狀態或資料，其資料型態可以是```object```或```function```，但需要注意的是，各元件檔(.vue)為各自獨立非共用，
所以元件中的```data```只能是```function```型態。範例如下：

```html
<div id="app">
    {{ message }}
</div>
```
```javascript
var vm = new Vue({
    el: '#app',
    data: {
        message: 'Hello Test!'
    }
});
```

元件中的data只能是```function```型態：
```javascript
export default {
    data () {
        return {
            message: 'Hello Test!'
        }
    }
}
```
了解data屬性的操作後，以下我們來介紹vue常用的資料綁定指令。

### 資料綁定指令

***v-text***  
1. 用途：更新被指定元素的textContent，也就是該元素的整個內容都會被更新，如果想要更新部分內容，就需要使用雙大括號{{ }}。
2. 表達式：string
3. 用法：
```html
<div id="app">
    <span v-text="msg"></span>
</div>
```

```javascript
var vm = new Vue({
    el: '#app',
    data: {
        msg: 'Hello Test!'
    }
});
```
如果只要更新部分內容，可以使用雙大括號{{ }}。
```html
<div id="app">
    <span>{{ msg }}</span>
</div>
```
以上兩個範例的結果會是一樣的，通常我們會使用雙大括號的方法，因為他除了更改部分內容外，也比較有使用彈性。

***v-html***  
1. 用途：更新被指定元素的innerHTML，內容以普通HTML語法插入，不會作為Vue模板來進行編譯。
2. 表達式：string
3. 用法：
```html
<div v-html="html"></div>
```
注意不要使用此指令任意接受其他不可信任的HTML，很容易會導致有XSS攻擊的風險。

若單一元件(.vue)中css style有寫scoped，v-html的html內容不會套用有寫scoped的css style，因為v-html的html內容並沒有被Vue模板編譯器編譯過。

在元件檔裡面的style寫scoped的用途是讓這style標籤內的所有樣式只會套用在這個元件檔裡面的所有元素。寫法如下：

```html
<style scoped>
    /*write down your css here*/
</style>
```

***v-model***  
v-model是一個常用的指令，通常會用來做表單資料的雙向綁定(Two-way Binding)，意思就是說將View與資料綁在一起，
當使用者輸入資料到輸入框後，會自動將資料存在一個變數中，並即時更新資料到綁定的View當中，下面我們介紹v-model的用途及用法

1. 用途：對表單元素做雙向綁定，並即時將輸入資料更新到綁定的View中。
2. 用法：只能在表單元素或自訂Vue元件上使用。

### 單行輸入框 input text

```html
<div id="app">
    <input type="text" v-model="message">
    <div>{{ message }}</div>
</div>
```
```javascript
var vm = new Vue({
    el: '#app',
    data: {
        message: 'Hello Test!'
    }
});
```
> 可以試試看在結果那邊的input框內新增或刪除文字，看看會有什麼變化。

#### 多行輸入框 textarea
```html
<div id="app">
    <textarea v-model="message"></textarea>
    <div>{{ message }}</div>
</div>
```
```javascript
var vm = new Vue({
    el: '#app',
    data: {
        message: 'Hello Test!'
    }
});
```

#### 修飾符號 Modifiers

***.lazy***  
當v-model與input事件綁定時，使用者輸入的內容會同步更新輸入框的值和資料，但是如果加上.lazy，
會改成onChange事件監聽，讓輸入框失去焦點，也就是說，當我們輸入內容進input時，輸出資料不會立即反應，
要將滑鼠點至input框外，才會看到結果。

```html
<input type="text" v-model.lazy="message">
```

***.number***  
v-model預設得到的值，型態為string，而.number可以做到的是把字串強制轉為數字。

```html
<input type="text" v-model.number="message">
```
> 在結果那邊如果輸入數字1,2,3，型態應該是number，如果將html語法裡面的.number刪除再執行，並輸入數字，看看會有什麼變化。


***.trim***  
.trim可用來去除input框內容中的首尾空格，如此一來，就不需要使用js中的trim()函式處理這部分了。它是onChange事件監聽，
所以不會立即同步，將滑鼠點至input框外，才會看到結果。

```html
<input type="text" v-model.trim="message">
```
上面介紹Options的data屬性以及Vue資料綁定的指令與用法，下一篇我們即將介紹屬性綁定的相關指令。

## [Directives] 屬性綁定(Class and Style Binding)

前一篇介紹的指令可以將資料與vue instance之間做綁定，那假設我們想讓HTML元素中的屬性和vue instance做綁定，
Vue有提供一個指令v-bind，就可以做到這樣的功能。

### 屬性綁定指令

***v-bind***
1. 用途：將HTML元素中的屬性與vue instance做綁定。
2. 縮寫：:
3. 用法：
```html
<div id="app">
    <p>讓滑鼠到連結上hover</p>
    <!-- 上下兩者結果一樣，下面為縮寫寫法 -->
    <a v-bind:href="url" v-bind:title="hint">link1</a>
    <br>
    <a :href="url" :title="hint">link2</a>
</div>
```
```javascript
var vm = new Vue({
    el: '#app',
    data: {
        url: 'http://www.google.com/',
        hint: '連到google網站'
    }
});
```

### 修飾符號 Modifiers

***.prop***
如果再```v-bind```加入```.prop```這個修飾符號，他綁定的就不是 HTML 元素的屬性(attribute)，
而是 DOM 的屬性(property)

```html
<a :href="url" :title.prop="hint">link</a>
```
> HTML的屬性(attribute) 與 DOM(property) 之間的區別  
> **Attribute**  
> attribute 由 HTML 定義，所有出現在 HTML 標籤內的描述節點都是 attribute。總是取得字串類型  
> **Property**  
> property 屬性屬於 DOM 對象，實質就是 javascript 中的對象。我們可以跟在 js 中操作普通對象一樣獲取，
> 設置 DOM 對象的屬性，並且 property 屬性可以是任意類型

***.sync***(2.3.0版本後才有)  
這邊寫理解一個概念，如果父組件與子組件要做溝通，需使用prop語法，而在Vue版本1.x時有.sync這個修飾符號，
用途是當一個子組件改變一個帶有.sync的prop值時，父組件綁定的值也會跟著改變，也就是可以對prop做雙向綁定，
但是因為debug比較複雜的結構時不方便，所以版本2.0後就移除.sync了。

但是Vue在版本2.3.0後又加回.sync，它現在是作為一個編譯時存在的語法糖，可以擴展成為自動更新父組件屬性的```v-on```監聽器。

### 應用：Class & Style Binding
上面我們了解到 v-bind 可以將 HTML 元素屬性跟 vue instance 做綁定，在 HTML 元素中我們常用到 class 或 style 這兩個屬性，
Vue 有特別對這兩種屬性做加強功能，表達式除了可以是 string 以外，還可以使用 object 或 array，這樣的應用稱為 Class & Style Binding。

Vue 為什麼要特別讓 v-bind 也可以接受 object 或 array 的值呢？

因為通常我們會在 class 或 style 給予比較多的值，如此一來，要綁定的值會很多，如果表達式又只有 string，
會造成 vue instance 不太好管理這些變數，也因此為了管理方便，Vue 讓 v-bind 可以接受 object 或a rray，看看下面的範例可能會更加理解。

#### 綁定多個Class屬性
1. 物件 object 寫法：



















