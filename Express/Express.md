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
## 自訂路由(Routing)

### 路由方法
路由方法衍生自其中一個 http 方法，並且會附加到 express 類別實例中。
下列程式碼範例說明對應用程式根目錄提出 GET 與 POST 方法時所定義的路由
```javascript
// GET method route
app.get('/', function(req, res){
  res.send('Exprtee is excellent!!!');//接收連線請求 並回應客戶端
});

// POST method route
app.post('/', function (req, res) {
  res.send('POST request to the homepage');
});
```
Express 支援下列的路由方法，這些方法對應至 HTTP 方法：get、 post、put、head、delete、options、 trace、copy、lock、mkcol、move、purge、propfind、proppatch、unlock、report、mkactivity、checkout、merge、m-search、notify、subscribe、unsubscribe、patch、search，以及 connect。

在'Exprtee is excellent!!!'這個字串上加上```<h1>```Tag

```javascript
app.get('/', function(req, res){
  res.send('<h1>Exprtee is excellent!!!<h1>');
});
```

### app.all
app.all() 是一個特殊的路由方法，它不是衍生自任何Http方法。此方法用來在所有要求方法的路徑中載入中介軟體函數。不論你使用 GET、POST、PUT、DELETE
或 http 模組中支援的其他任何 HTTP 要求方法

```javascript
app.all('/secret', function(req, res, next){
   console.log("Accessing the secret section");
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
  res.send(req.param.name + 'is so' + req.param.nickname);
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

***
## 頁面呈現
### res.render
Express 可以透過 res.render 呈現 .ejs, .jade 頁面
```javascript
app.get('/', function(req, res){
   res.render('index.ejs');
});
```
### res.sendfile
Express 透過 res.sendfile 呈現 .html 頁面
```javascript
app.get('/', function(req, res){
   res.sendfile(__dirname + '/views/index.html'); // 給定絕對路徑
});
```
***
## Express 提供靜態資料
如果要提供影像、CSS 檔案和 javaScript 檔案等之類的靜態檔案，請使用 Express 中的 express.static 內建中介軟體函數

將含有靜態資產的目錄名稱傳遞給 express.static 中介軟體函數，就能直接開始提供檔案
```javascript
app.use(express.static('public'));
```

如果要為express.static 函數所提供的檔案，建立虛擬路徑字首(其中路徑事實上不存在於檔案系統中)，請為靜態目錄指定裝在路徑
```javascript
app.use('/public', express.static('public'));
//保守做法是使用絕對路徑
app.use('/public', express.static(__dirname + 'public'));
```

### 使用Express 載入css
除了提供css靜態資料外，還必須在 ejs 中加入 link
```html
<link rel="styleheets" href="/public/styleheets/style.css" >
```

***
## 使用Express,ejs 讀取靜態資料JSON

### 建立 JSON 檔案
在專案的根目錄建立 JSON 的檔案

```JSON
[
     {
       "title": "會濟報好大必者政下二",
       "id": "fdb61c16-4bba-11e4-9e35-164230d1df67",
       "content": "容有那一氣持地來於結主了友如頭......院還地入。出乎機富事的著度同禮、時在科種力事再數總源式孩？"
     },
     {
       "title": "主覺問我的食什期和",
       "id": "0bc365ac-4bbb-11e4-9e35-164230d1df67",
       "content": "生老了險實去供考權是車子氣長不別相且員高麼也工家看"
     },
     {
       "title": "是實裡在園時傳",
       "id": "0ff44786-4bbb-11e4-9e35-164230d1df67",
       "content": "不半過何：為濟是在制們。裡得一方出，處師取是你賣而陽"
     }
]
```
### app.js
```javascript
// express web framework
var express = require('express');
//讀取posts.json
var fs = require('fs');
var app = rexpress();

//定義網站標籤
app.locals.title = "Get json data using express web framework";

app.all('*', function(req, res, next){
   fs.readFile('posts.json', function(err, data){
      res.locals.posts = JSON.parse(data);
   });
});

//網頁主進入點
app.get('/', function(req, res){
   //指定 /views/index.ejs
   res.render('index.ejs');
});
```
### index.ejs
```javascript
<main role="main">
   <div class="container">
      <h3> <a href="/api/posts"></a></h3>
      <% posts.forEach(function(post) { %>
         <h3>
            <a href="/post/<%= post.id %>">
               <%= post.title %>
            </a>
         </h3>
         <div><%= post.content %></div>
      <% }); %>
   </div>
</main>
```

## Express 資料庫操作

### 建立連線 mySQL
首先專案必須要先 install mysql
```
npm install mysql
```

宣告一個 mysql 連線，使用 createConnection 方法將資料庫參數初始化
```javascript
var mysql = require('mysql');
var conn = mysql.createConnection({
   host: 'localhost',
   user: 'root',
   password: '0000',
   database: 'test',
   port: 3306
});
```

透過 qery 執行，測試連線是否成功
```javascript
conn.query('SELECT 12+34 AS result', function(err, rows, field){
   if(err) throw err;
   console.log('The is result', row[0].result);
});
```
**如有成功印出 46 代表連線已成功**

***
### 資料庫操作 查詢(Read)
```javascript
var mysql = require('mysql');
var db_option = {
   host: 'localhost',
   user: 'root',
   password: '0000',
   database: 'test',
   port: 3306
};
var sqlread = function(req, res){
   var conn = mysql.createConnection(db_option);
   var query = 'SELECT * FORM user';
   conn.query(query, function(err, rows, fields){//row取得sql資料
      if(err) throw err;
      res.render('index', {'items': rows})//將資料傳遞至前端
   });
}; 
```

***
### 資料庫操作 建立(Create)
在 express 4.x 將 body parser 分離了，想取得使用者輸入的資料專案必須要安裝 body-parser  
body-parser 作用是對於 post 請求的請求體進行解析
```
npm install body-parser
```

bodyParser 實現要點如下
1. 處理不同類型的請求體: 例如 text、json、urlencode等
2. 處理不同的編碼: 例如 utf8、gbk
3. 處理不同的壓縮類型: 例如 gzip、deflare
4. 其他邊界、異常處理
   
bodyParser提供了以下幾種方法
1. bodyParser.json();
2. bodyParser.raw();
3. bodyParser.text();
4. bodyParser.urlencoded();

```javascript
var express = require('express');
var app = express();
var body = require('body-parser');
var mysql = require('mysql');
var db_option = {
   host: 'localhost',
   user: 'root',
   password: '0000',
   database: 'test',
   port: 3306
}

var pool = mysql.createPool(db_option);

app.use(body.urlencode);//處理 post 請求,初始化req.body

var createPost = funtcion(req, res){
   pool.getConnection(function(err, conn){
      conn.query('INSERT INTO user SET ?',{//將使用者輸入資料，新增到資料庫
            name: req.body.name,
            age: req.body.age,
            sex: req.body.sex
        },function(err, fields){
            if(err) throw err;
        });

        conn.query('SELECT * FROM user',function(err, rows, field){//重新sql資料更新頁面
            if(err) throw err;
            res.render('index.jade', {title: '使用者介面', 'items': rows});
        });
        
        conn.release();
    });
};
```

***
### 資料庫操作 更新(Update)
```javascript
pool.getConnection(function(err, conn){
   conn.query('UPDATE user SET ? WHERE name=?',[{
      name: req.body.name,
      age: req.body.age,
      sex: req.body.sex
   },req.body.name], function(err, conn){
      if(err) throw err;
   });
   conn.query('SELECT * FROM user',function(err, rows, field){
      if(err) throw err;
         res.render('index.jade', {title: '使用者介面', 'items': rows});
   });

   conn.release();
});
```

***
### 資料庫操作 刪除(Delete)
```javascript
pool.getConnection(function (err, conn) {
   conn.query('DELETE FROM user WHERE id=?',[req.params.id],
      function(err, row, fields){
         if(err) throw err;
      });
   conn.query('SELECT * from user', function(err, rows, field){
      if(err) throw err;
         res.render('index.jade', {title: '使用者介面', 'items': rows});
      });
   conn.release();
});
```


















