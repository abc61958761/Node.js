# 2.1 腳手架 vue-cli
當我們使用 Vue 建置一個原形的時候，需要做的通常就是透過 <script> 把 Vue.js 引用進來，然後就可以在頁面上直接進行編碼。這種情況
作為一個實驗性的嘗試是完全可以的，但真實的開發卻不能這樣做。

真正前端開發時，不可避免地要用到一大堆工具，例如模組化的套件管理工具，程式執行前的前置處理器，程式熱載入模組，程式驗證，還有各種的測試環境
與架構支援工具等，這些工具對一個需要長時間維護或不斷地反覆運算演進的應用都是必須的。

vue-cli 這個工具可以讓一個簡單的命令列工具來幫助我們快速建置一個足以支撐實際專案開發的 Vue 環境，並不像 Anguler or React 那樣要在 
Yoman 上找適合自己的協力廠商腳手架。vue-cli 的存在將專案環境初始化工作降到最低。

## 安裝 vue-cli 
```
npm install vue-cli -g
```

## 使用 vue-cli 初始化專案
```
指令：
  init 從指定範本中產生一個新專案
  list 列出所有可用的官方範本
  help [cmd] 顯示所有的[cmd](指令) 的幫助
```

使用 list 查看官方範本

1. browserify：擁有進階功能的 Browerify + vueify 用於正式開發
2. browserify-simple：擁有最基礎的 Browerify + vueify 用於快速原型開發
3. simple：適用於單頁應用程式開發的最小化設定
4. webpack：擁有進階功能的 webpack + vue-loader 用於正式開發
5. webpack-simple：擁有最基礎的 webpack + vue-loader 用於快速原型開發

browserify 做得比較簡陋，用於正式開發還是有些不足，建議初學者直接使用 webpack 的兩個範本，設定了最簡單的可直接支援 ES6
的 Vue.js 編譯環境，可以應對那些要求時間短，結構相對簡單的小型應用。

## 建立專案 
```
vue init webpack my-project
```

# 2.2 深入 vue-cli 的專案範本
對於多人開發專案，目錄的使用與檔案的命名都顯為重要，應在開發前做約定，實際約定如下：
1. 公共元件、指令、篩檢程式(多於三個檔案以上的參考)將分別儲存於 src 目錄下：
    - components
    - directives
    - filters
2. 以使用場景命名 Vue 的分頁檔
3. 當分頁檔具有私有元件、指令和篩檢程式時，則建立一個與頁面名稱相同的目錄，分頁檔改名為 index.vue ，將頁面與相關的依賴檔案放一起
4. 目錄全由小寫個名詞、動名詞或分詞命名，有兩個以上的片語以"-"進行分割
5. Vue 檔案統一以大駝峰命名法命名，僅入口檔案 index.vue 採用小寫
6. 測試檔案一律已測試目的檔案名稱 .spec.js 命名
7. 資源檔一律以小寫字元命名，有兩個以上的片語以"-"進行分割

# 2.3 Vue 專案的 webpack 設定與基本用法
webpack 是一個模組包裝工具，它的作用是把互相依賴的模組處理成靜態資源。

## webpack 的特點

- 程式分割：  
在 webpack 的依賴樹裡有兩種依賴，同步依賴和非同步依賴。非同步依賴會成為一個程式分割點，並組成一個新的程式區塊。
在程式區塊組成的樹被最佳化後，每個程式區塊都會儲存在一個單獨的檔案裡
- 載入器：  
webpack 原生只能處理 Javascript 的，載入器的作用是把其他的程式轉換成 Javascript，這樣一來所有種類的程式都能組成一個模組，
在程式內透過 import 將 webpack 套件裝的資源以模組的方式引入  
以下是 Vue 專案常用到的載入器：
    - vue-loader
    - vue-style-loader
    - style-loader
    - css-loader
    - less-loader
    - babel-loader
    - file-loader
    - url-loader
    - json-loader
- 智慧解析：  
webpack 幾乎能處理所有協力廠商函式庫，他甚至允許依賴裡出現這樣個願算式：
```
require("./components/" + name + ".vue")
```
能處理大多數的模組系統，CommonJS 和 AMD
- 外掛程式系統
webpack 有豐富的外掛程式系統，大多數內部的功能都是以這個外掛程式系統為基礎的。這也使我們可以訂製 webpack，把它打造成能滿足我們需求的工具，
並且把自己做的外掛程式開放原始碼出去。

## 用別名取代路徑參考

使用 require 有時需要參考一段很長的檔案路徑，透過 webpack 的 resolve 設定項目來解決。
在 webpack.base.config.js 中加入以下這個別名的定義：
```
module.exports = {
  entry: {...},
  output: {...},
  module: {...},
  resolve: {
    extensions: ['','js'],
    alias: {
      'bs-select': 'bower_components/bootstrap-select/dist/js/select.js'
    }
  }
}
```
這麼定義之後在使用 import 時就可以改為以下寫法
```
import Select from 'bs-select'
```

# 2.4 基於 Karma + Phantom + Mocha + Sinon + Chai 的單元測試環境

在有實際元件測試目標時的 Vue-TDD 的開發流程
- 撰寫元件測試：在單元測試中將設計元件的名稱、屬性介面、事件介面，用斷言工具確定衡量這個元件正確的標準
- 撰寫元件程式：以單元測試作為啟動程式，撰寫元件真實的現實程式，讓測試成功。
- 執行測試
- 重構

## [Karma](https://karma-runner.github.io/)
測試載入器，他能完成許多測試環境載入工作







