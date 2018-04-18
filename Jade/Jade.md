## Jade 基本用法
Jade 的基本用法非常簡單，只要向後退一格，就表示該標籤在它的下層，基本上它看起來就像是一個樹狀結構，
同時只需寫起始標籤就好，當經由編譯工具後，就會自動將標籤給包覆起來，以下簡單小示範
```jade
doctype html
html
  head
    title helloJade
  body
    h1 Jade Basic
    p 這是基本用法
```

## Jade 文字(Plain Text)
在 Jade 中，當要輸入文字時，共有三種方式。
 ```
//第一種寫法：標籤後方加入空格
p 這是基本寫法

//第二種寫法：斷行後加入 |
p
  | 這是基本寫法
  
//第三種寫法：在標籤後加入.，通常用於多行文字。
p.
  這是基本寫法
  支援多行
  不用一直加入 |
```

## Jade 繼承(extends)
網頁模版的概念，透過以製作好的模板，再針對活動內容加以修改裡面的元素，如此一來就能讓網頁版型重複再利用，
同時可加快網頁的製作時間。  
再製作模板時，當可替換元素，使用 block 標示。

**tpl.jade**
```
doctype html
html
  head
    block webtitle
      title 無標題
  body
    block h1title
      h1 梅問題講堂
    block postcontent
      div 內容
```

**index.jade**
```
//繼承tpl.jade網頁
extends ./tpl.jade
//覆蓋tpl.jade 網頁所定義的block title
block webtitle
  title 有標題
block postcontent
  div.container
    | 內容2
```

## Jade 匯入(includes)
將內容直接匯入進來，像是複製貼上一樣，將版型拆成多支後，再把它合併起來

**head.jade**
```
head
  meta(charset='utf-8')
  meta(name='description', content='梅問題講堂')
  link(href='/public/stylesheets/style.css', rel="stylesheet")
  title jade includes
```
**footer.jade**
```
#footer
  p copyright (c) 2014-10-31
```
**index.jade**
```
doctype html
html
  include ./head.jade
  body
    h1 Hello World
    #contnet 好久不見
    include ./footer.jade
```








