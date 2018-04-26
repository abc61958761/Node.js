如果想要把陣列或物件裡所有東西 print 出來，就需要用到迴圈來寫，在 Vue 中，
也有可以放在 HTML 元素中的循環指令，把陣列或物件的所有元素顯示出來，
官方稱這個叫做**列表渲染(List Rendering)**，
下面我們就來看看 Vue 的指令要如何做到跟迴圈一樣的效果。

## 列表渲染指令

用途：重複迭代顯示陣列或物件中的元素，類似loop的概念。
表達式：array、object、number、string
用法：alias in expression (別名 in 表達式)

1. 陣列循環 array 寫法

以下範例說明：data那邊宣告一個memberArray陣列[]，member是自定義的別名，
會暫存從memberArray陣列取出的元素，進而可以使用該屬性，member.id與member.name，index代表索引值。


