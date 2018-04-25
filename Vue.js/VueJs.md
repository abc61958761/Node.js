

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
```html
<div id="app">
    <ul class="menulist">
        <li><a href="#" :class="{ active: isActive, hasError: isError }">Home</a></li>
        <li><a href="#">About</a></li>
        <li><a href="#">Work</a></li>
        <li><a href="#">Contact</a></li>
    </ul>
</div>
```
```javascript
var vm = new Vue({
    el: '#app',
    data: {
        isActive: true,
        isError: true
    }
});
```

2. 物件 object 寫法(集合)
```html
<div id="app">
    <ul class="menulist">
        <li><a href="#" :class="classObject">Home</a></li>
        <li><a href="#">About</a></li>
        <li><a href="#">Work</a></li>
        <li><a href="#">Contact</a></li>
    </ul>
</div>
```
```javascript
var vm = new Vue({
    el: '#app',
    data: {
        classObject: {
          active: true,
          hasError: true
        }
    }
});
```
使用```computed```計算屬性來自動計算：
> ```computed```會在後面有一篇詳細介紹，這邊先看一下寫法。

```javascript
var vm = new Vue({
    el: '#app',
    data: {
        isActive: true,
        isError: true
    },
    computed: {
        return {
            active: isActive,
            hasError: isError
        }
    }
});
```

3. 陣列 array 寫法
```html
<div id="app">
    <ul class="menulist">
        <li><a href="#" :class="[activeClass, errorClass]">Home</a></li>
        <li><a href="#">About</a></li>
        <li><a href="#">Work</a></li>
        <li><a href="#">Contact</a></li>
    </ul>
</div>
```
```javascript
var vm = new Vue({
    el: '#app',
    data: {
        activeClass: 'active',
        errorClass: 'hasError'
    }
});
```
4. 加上判斷的三元運算式
```html
<div id="app">
    <ul class="menulist">
        <li><a href="#" :class="[isActive ? activeClass : '', errorClass]">Home</a></li>
        <li><a href="#">About</a></li>
        <li><a href="#">Work</a></li>
        <li><a href="#">Contact</a></li>
    </ul>
</div>
```
```javascript
var vm = new Vue({
    el: '#app',
    data: {
        isActive: true,
        activeClass: 'active',
        errorClass: 'hasError'
    }
});
```









































