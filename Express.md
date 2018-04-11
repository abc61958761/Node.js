## Express介紹
Express 是 Node.js 中的開發框架，不但有簡潔靈活的特性，還有豐富的文件和完整的 API、Reference，令開發者能快速的搭建一個完整功能的網站

Express 中常用的功能，包括 Express路由機制(Routing)、靜態網頁伺服器(Static Web Server)如何載入圖片、文本等靜態檔案到瀏覽器上 

***
## 使用工具:編輯器、Node.js終端機與瀏覽器

主要用到三項工具
1. 編輯器(Editor)
2. 終端機(Terminal)
3. 瀏覽器(Browser)

安裝 Node.js，並在Terminal中輸入node -v確認是否安裝成功

建立一個node 專案
1. 打開Terminal cd 到 project 放置路徑，輸入npm init 為此專案創建一些設定，並將這些資訊存在 package.json 的檔案中
   在 package.json 中可以設定應用名稱(name)、版本號(version)、應用描述(description)、關鍵字(keyworde)、應用配置(config)
   、主頁(homepage)、作者(author)、版本庫(repository)、bug提交地址(bugs)、授權方式(licenses)等....
2. 安裝 Express 
   ```
   npm install express --save
   npm install express-generator -g //快速建立Express應用程式架構
   ```
   除了 insatll express 之外、還需要加上 --save ，這是什麼意思呢？在 npm 中， Dependency(依賴模組)是一個非常重要的概念，--save 
   的意思是將這個package設定為本次專安的 dependency
   npm 意即 node package manager、node套件管理器，npm默認設定將一個套件(package)安裝在 node 模組下
   
***
## 建立Express App -設定伺服器(Server)與路由(Route)

Routing 是把網址(URL)轉給特定程序處理的過程，幫主伺服器解析 URL、判斷 URL 路徑與相應的執行動作
從使用者角度來看，一個 Route 就是一個 URL ，比如 lyrasoft.net/tw/about 或 lyrasofe.net/tw/home
URL 之所以被稱為 Route，是因為他告訴使用者如何從伺服器拿到資料的路徑

建立簡易的 Express 伺服器
```javascript
var express = require('express');
var app = express();//建立一個Express伺服器
app.listen(3000);//告訴server聽取3000這個Port
```

***
## 自訂路由(Routing)-接收請求(Get Request)與回傳回應(Send Response)

Express Get Request

```javascript
app.get('/', function(req, res){
  res.send('Exprtee is excellent!!!');//接收連線請求 並回應客戶端
});
```

在'Exprtee is excellent!!!'這個字串上加上```<h1>```Tag

```javascript
app.get('/', function(req, res){
  res.send('<h1>Exprtee is excellent!!!<h1>');
});
```

瀏覽器在接收到回傳的字串，是把字串解析成 HTML 呈現出來

***
## 從URL取得資料

/ 代表從根目錄拿到資料、/about 代表可以拿到 About 頁面，也可以繼續設定路徑找到 about-me 頁面

```javascript
app.get('/', function(req, res){
  res.send('Send root');
});

app.get('/about', function(req, res){
  res.send('Send about');
});

app.get('/about/about-me', function(req, res){
  res.send('Send about/about-me');
});
```

伺服器會從 URL 找到並回傳字串，包括 HTML、JSON等都是字串一種

***
## 從req.params取得參數

可以進一步在 URL 做一些設定、並從 URL 拿到我們要的參數;這些參數可以由我們自己命名
req.params，當伺服器拿到 /post/:id 這個路徑時，回傳id這個參數(parameter)、寫作 req.params.id

```javascript
app.get('/post/:id', function(req, res){
  res.send(req.param.id);
});
```

req.params 會將命名過參數以 key-value 的形式存放;比如這邊舉例是 /ablut/Lynn/pretty 因為中間加了一個字串將兩參數串在一起，
可以發現瀏覽器的結果會有些不同


```javascript
app.get('/post/:name/:nickname', function(req, res){
  res.send(req.param.name + is so + req.param.nickname);
});
```

***
## 從req.query取得參數

透過 req.query 拿到路徑 ? 後面的資料， ? 後面開始是 JSON 的 key-value 設定
讓我們回傳一個 req.query 參數，req.query  會回傳一個 json 格式

```javascript
app.get('/hell', function(req, res){
  res.send(req.query);
});
```

如果使用 res.json(req.query);結果是一樣的
在傳送 javascript 物件(object)或陣列(array)時，res.send 和 res.json 的結果是一樣的

同時利用 req.params 和 req.query 、在URL上取得資料，由於 JSON format 無法與一般字串接在一起、會印不出來，
因此要先把 JSON 轉成字串(stringify)再印出來

```javascript 
app.get('/hey/:id', function(req, res){
  res.send(req.params.id + ' ' + JSON.stringify(req.query));
});
```




