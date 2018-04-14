## Socket.io 介紹
socket.io 用來處理 client, server 雙向訊息的傳遞

***
## 取得Socket.io
利用 npm 在 node.js project dependencies socket.io
```
npm install --save socket.io
```

***
## 在 server 建立 socket Object
```javascript
var express = require('express');
var app = express();
var server = require('http').createServer(app);
var io = require('socket.io')(server);

//當使用者開啟 listen 網頁啟動funciton
io.on('connection', function(socket){
  console.log('a user connection');
});

//監聽3000 port
server.listen(3000, function(){
  console.log('listening on *: 3000');
});
```
