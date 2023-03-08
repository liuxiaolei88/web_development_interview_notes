# Webstorage 总结

## Q1：webstorage优点和缺点
### 优点：
- 存储空间更大。在IE下每个独立存储空间为10M，其它浏览器存储空间略有不同，但可以肯定的是至少要比cookie要大很多。
- 存储内容不会与服务器发生任何交互，数据仅仅单纯地存储在本地。不用担心对服务器数据的影响！
- 独立的存储空间，每个域都有自己独立的存储空间，各个存储空间又完全是独立的，所以不会对数据千万混乱。
### 缺点
- 存储在本地的数据未加密且永远不会过期，容易造成隐私泄漏！
- 存储的数据类型只能是字符串！

## Q2：localStorage与sessionStorage
- localStorage：没有时间限制的存储方式。存储的时间是永久的！关闭浏览器数据不会随着消失，当再次打开浏览器时，数据依然可以访问！也就是说除非你主动删除数据，否则数据是永远不会过期的。
- sessionStorage：保存在session对象当中。用来保存的时间为用户与浏览器的会话时间。即从浏览页面到关闭浏览器为一个会话时间。关闭浏览器，所有的 session数据也会消失！

## Q3：sessionStorage设置和获取数据
```javascript
//保存数据有3种方法：
sessionStorage.setItem("key","value");
//或
sessionStorage.key="value";
//或
sessionStorage["key"]="value";

//读取数据的3种方法，将数据赋值给变量v
var v=sessionStorage.getItem("key");
//或
var v=sessionStorage.key;
//或
var v=sessionStorage["key"];
```

## Q4：localStorage设置和获取数据
```javascript
//保存数据有3种方法：
localStorage.setItem("key","value");
//或
localStorage.key="value";
//或
localStorage["key"]="value";

//读取数据的3种方法，将数据赋值给变量v
var v=localStorage.getItem("key");
//或
var v=localStorage.key;
//或
var v=localStorage["key"];
```

## Q5：常用API

### Web Storage是window对象的子对象。
```javascript
//保存数据userName值为zhangpeiyue
sessionStorage.set("userName","zhangpeiyue");
//也可以写为：
window.sessionStorage.set("userName","zhangpeiyue");
```

### length
localStorage.length 或 sessionStorage.length 为相应的数据条数

### localStorage.key(index)
将数据的索引值作为参数传入，可以得到localStorage中与这个索引号相对应的数据。sessionStorage.key(index)同理！故省略！

### removeItem
localStorage.removeItem(“key”):清除指定的localStorage数据。  
sessionStorage.removeItem(“key”):清除指定的localStorage数据。

### clear
localStorage.clear()：清除所有保存在localStorage的数据。  
sessionStorage.clear():清除所有保存在sessionStorage的数据。
## 参考链接
- https://zhuanlan.zhihu.com/p/364696625