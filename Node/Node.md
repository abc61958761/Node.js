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
