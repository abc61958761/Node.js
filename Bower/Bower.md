## Bower 介紹
bower 由 twitter 研發，用來管理前端開發時所需要的相依套件

## Bower 安裝
安裝 bower 前必須要確認 node.js、npm 已安裝
```
$npm install bower -g
```

## 使用 bower 安裝相依套件
```
//搜尋 package 
$bower search [package name 例如 jquery,bootstrap]
//安裝需要的 package
$bower install [package name 例如 jquery,bootstrap]
```

安裝後會看到  
bower_components/jquery/*、bower_components/bootstrap/*

透過 bower init 安裝 bower.json，可以在dependencies的部分輸入套件名稱與版本號

在建立好的 express 框架下的 app.js ，把剛才建立好的套件資料夾綁入基本靜態套件路徑。
```javascript
app.use('/bower_components', express.static('bower_components'));
```

接著使用 script tag 將檔案引入
```html
<script src="../bower_components/jquery/dist/jquery.min.js"></script>
```

