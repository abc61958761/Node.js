## Socket.io 介紹
socket.io 用來處理 client, server 雙向訊息的傳遞

***
## 取得Socket.io
利用 npm 在 node.js project dependencies socket.io
```
npm install --save socket.io
```

***
## 建立 socket Object

### server 
透過傳入一個 Http 對象，初始化一個 socket.io Object ，listen connection 將訊息記錄到 console。 
```javascript
var express = require('express');
var app = express();
var server = require('http').createServer(app);
var io = require('socket.io')(server);

//當使用者開啟 listen port:3000 網頁啟動funciton
io.on('connection', function(socket){
  console.log('a user connection');
  // 當使用者關閉網頁時呼叫 function
  socket.on('disconnect', function(){
    console.log('usr disconnect');
  });
});

//監聽3000 port
server.listen(3000, function(){
  console.log('listening on *: 3000');
});
```

### client 

透過 var socket = io(); 建立一個 socket Object 
```javascript
$(function(){
  var socket = io();
});
```

## 發送、接收訊息

你可以在 socket.io 中發送任何訊息，可以編碼為 JSON 格式

### server
接收 client 發送的訊息，並且將訊息發送去
```javascript
io.on('connection', finction(){
  // 接收名為 chat message
  socket.on('chat message', function(msg){
    //接收訊息後透過 io.emit() 發送出去
    io.emit('chat message', msg);
  });
});
```


### client

透過 .emit()將使用者輸入資料發送出去，並接收 server 回傳的訊息
```javascript
$('form').submit(function(){
  // 發送名為 chat message 的訊息
  socket.emit('chat message', $('#m').val());
  $('#m').val('');
  return false;
});
//接收 server 回傳的訊息
socket.on('chat message', function(msg){
  $('#messages').append($('<li>').text(message));
});
```

**'chat message'是自定義的有對應關係**

