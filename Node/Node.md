## Node.js Modules介紹及載入

### node.js modules
modules 在 node.js 裡是由一些簡單或複雜經過組織化的 javascript 寫成的功能，
它可已分別放在不同的 javascript 檔，被 node.js 應用

### node.js Type
Node.js 有三大類模組
1. Core Modules(原生模組)
2. Local Modules(自訂模組)
3. Third Party Modules(第三方模組)

### node.js Core Modules
node.js 是一個輕量的 framework ，它的原生模組功能，可以被編譯成二進制位元存取，也
可以單刀直入做一些本地自動化功能操作

有幾個重要的
http:用來建立 http server 的一些類別、方法、及事件。
url:包含可以解析 url 的一些方法
querystring:可以處理由 client 端傳來querystring 的一些方法
path:處理檔案或資料夾路徑的方法
fs:檔案存取/操作的一些類別，方法及事件
util:包含可供程序者適用的效能函式

### 如何載入Core Modules
為了使用 Core Modules ，需要做載入動作，如何載入？
```javascript
var module = require('module_name');
```

也就是說，上述每一個 module 都可以使用 require() 載入，根據不同的 module，它回傳的將會是一個物件，
函式，屬性，或是其他的 javascript 型別

## 自建模組(Local Modules)
我們可以自建立一些模組，方便我們在應用時使用。自建模組也可以包成 npm 供 node.js 社群使用。  
在 node.js 中，一個 javascript 的檔案即是一個模組！



## Node.js 檔案基本操作
```javascript
var fs = require('fs');

//讀取訊息
fs.readFile('test.txt', 'utf-8', function(err, data){
    if(err) {
        throw err;
    } else {
        console.log(data);
    }
});

var data1 = "寫入訊息";

//寫入訊息
fs.writeFile('content.txt', data1, function(err){
    if(err) throw err;
});

var data2 = "的主計林車修該自是";

//附加檔案內容
fs.appendFile('content.txt', data2, function(err){
    if(err) throw err;
});

var oldname = "content.txt";
var newname = "content2.txt";

//更名檔案
fs.rename(oldname, newname, function(err){
   if(err) throw err;
});

//顯示檔案狀態
fs.stat('content.txt', function(err, stats){
    if(err) throw err;
    console.log('stats:' + JSON.stringify(stats));
});

//刪除檔案
fs.unlink('test.txt', function (err) {
    if (err) throw err;
    console.log('successfully deleted test.txt');
});
```
