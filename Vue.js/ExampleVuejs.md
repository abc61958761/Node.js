# 1.1 內插(Interpolation)
Vue 的視圖範本是以 DOM 實現為基礎的。表示所有的 Vue 範本都是可解析有效的 HTML ，而且它對一些特殊的特性做了增強。

首先，在元件指令稿定義中使用 data 定義用於內部存取的資料模型：
```javascript
export default {
  data () {
    title: "vue-todos"
  }
}
```
data 可以是一個回傳 Object 物件的函數，也可以是一件屬性，也就是說，可以寫成以下的方式：
```javascript
export default {
  data : {
    title: "vue-todos"
  }
}
```
使用函數傳回是為了可以具有更高的靈活性，例如對內部資料進行一些初始化的處理，官方推薦的用法是採用回傳 Object 物件的函數。

在 template 中參考 data.title 資料時我們並不需要寫上 data ，這只是 Vue 定義時的內部資料容器，透過 Vue 模組的內插法
方式直接寫上 title 即可：
```html
<h1>{{ title }}</h1>
```
用雙大括號 {{}} 引住的內容被稱為 "Mustache" 語法，Mustache 標籤會被對應資料物件的 title 屬性的值取代。每當這個屬性變化時他也會更新

> 從 Vue2 開始，元件範本必須且只能有一個頂層元素，如果在元件模組內設定多個頂層元素將引發編譯例外

# 1.2 資料綁定(data binding)
我們需要一個稍微複雜一點的資料模型來表述 Todo ，它的結構應該是這樣的：
```javascript
export default {
  data () {
    return {
      title: "vue-todos",
      todos: [
        { value: "閱讀一本關於前端開發的書", done: false },
        { value: "補充範例程式", done: true },
        { value: "寫心得", done: false }
      ]
    }
  }
}
```
data 有什麼作用？我們可以將 Vue 實例定義看作一個類別的定義，data 相當於這個類別的內部欄位屬性的定義區域。在 Vue 實例內的其他地方可以直接
用 this 參考 data 內定義的任何屬性，例如 this.title 就是參考了 data.title

我們要顯示 todos 資料就要使用 Vue 範本中最常使用的 v-for 指令標記，將陣列資料繪製成一個列表。使用與 JS 類似的語法 item in items，items 是
資料陣列，item 是目前陣列元素名

```html
<ul>
  <li v-for="todo in todos">
    <label>{{ todo.value }}</label>
  </li>
</ul>
```
如果需要輸出序號，可以使用 v-for 中隱藏的 index 值來進行輸出，實際用法如下：
```html
<ul>
  <li v-for="(todo,index) in todos" :id="index">
    <label>{{index + 1}}. {{ todo.value }}</label>
  </li>
</ul>
```
只需要用括號括住傳址參數，最後一個值就是循環的索引。索引由 0 開始計數的，輸出序號應從 1 開始正好我們使用
一個 javascript 的運算室內插來輸出一個 index + 1 的從 1 開始計數的序號。

除了使用內插綁定，還可使用屬性綁定語法，```:id="index"```，這樣的寫法是一種縮寫法，意思是將 index 的直輸出到 DOM 的 id
，如果沒有加上 ":"，那麼 Vue 就會認為我們正為 id 屬性指定一個字串。

> v-for 不只可以繪製陣列，還可以繪製物件屬性，例如：
> ```
> <ul>
>   <li v-for="value in object">
>     {{ value }}
>   </li>
> </ul>
> data () {
>   return {
>     object {
>       firdt_name: "Ray",
>       last_name: "Liang"
>     }
>   }
> }
> ```
> 輸出
> ```
> "Ray"
> "Liang"
> ```

# 1.3 樣式綁定(style binding)

在 /assets/ 中增加一個 todos.css 檔案，並在 App.vue 的元件定義內引用 css 樣式表：

```javascript
import './assets/todos.css'

export default {
  // ..... 省略
}
```

import 將樣式表直接匯入到程市效果是：webpack 的 css-loader 會產生一些程式，在頁面執行的時候將編譯後的 css
程式產生到 <style> 標籤內並自動插入到頁面的 <head> 中，這種做法是全癒的，樣式會長期駐留到頁面的根 (root)。

可以使用 <style scoped> ，然後使用 CSS @import 匯入樣式
```
<style scoped>
@import './assets/todos.css'
<style>
```

樣式綁定和屬性的綁定是一樣的，我們這裡就將 done==true 的待辦事項 <li> 綁定一個 checked 的樣式類別：

```html
<li v-for="(todo,index) in todos" :class="{'checked': todo.done}">
  <!-- 省略 -->
</li>
```
Vue 的屬性綁定法是透過 v-bind 實現的，完整寫法是這樣：

```html
<li v-for="(todo,index) in todos" v-bind:class="{'checked': todo.done}">
  <!-- 省略 -->
</li>
```
v-bind 可使用縮寫 ":" 來表示，建議使用縮寫會更直觀

:class="{'checked': todo.done}" 的意思是當 todo.done 為 true 時，在 <li> 元素的 class 增加 checked 樣式類別


# 1.4 篩檢程式
我們在待辦事項的右側增加時間欄位 created ，並用 <time> 元素表示：
```html
<template>
  <div>
    <h1>{{ title }}</h1>
    <ul>
      <li v-for="(todo,index) in todos" :id="index" :class="{'checked': todo.done}">
        <label>{{index + 1}}. {{ todo.value }}</label>
        <time>{{ todo.created }}</time>
      </li>
    </ul>
  </div>
</template>

<script>

export default {
  name: 'App',
  data () {
    return {
      title: "vue-todos",
      todos: [
        { value: "閱讀一本關於前端開發的書", done: false, created: Date.now() + 300000 },
        { value: "補充範例程式", done: true, created: Date.now() },
        { value: "寫心得", done: false, created: Date.now() - 300000 }
      ]
    }
  }
}
</script>

<style scoped>
  @import './assets/todos.css';
</style>
```
 
在這裡 <time>{{ todo.created }}</time> 會輸出一個整數，此時我們可以使用時間格式化專用套件 moment.js，先安裝：
```
npm install moment -s
```

使用篩檢程式將 <time>{{ todo.created }}</time> 輸出格式化，首先引用 moment 並設定區域
```
import moment from 'moment'
import 'moment/locale/zh-tw'
moment.locale('zh-tw')
```

然後加入 date 篩檢程式
```javascript
export default {
  // .....省略
  filters: {
    date (val) {
      return moment(val).calendar()
    }
  }
}
```

最後在 template 篩檢程式：
```html
<time>{{ todo.created | date }}</time>
```






