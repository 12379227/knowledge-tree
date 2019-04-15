#### JSON基本概念

- JSON (JavaScript Object Notation)是一种轻量级的数据交换格式. 
  - 简史
    - JSON 是Douglas Crockford在2001年开始推广使用的数据格式
    - 2005-2006年正式成为主流的额数据格式, 雅虎和谷歌就在那时候开始广泛地使用JSON格式
  - 基本语法: 任何JS支持的类型都可以通过JSON来表示, 例如字符串, 数据, 对象,数组等. 但是对象和数据是比较特殊而且常用的两种类型:
    - 对象表示为键值对.  下面的例子中: 键为firstname, 值为rain
        ```javascript
        {"firstname":"rain"}
        ```
    - 数据由逗号分隔
    - 花括号保存对象
    - 方括号保存数组
