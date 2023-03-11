# JavaScript总结

## Q1:数据类型相关

### 1、基本数据类型

- ES5：Number、String、Boolean、undefined、object、Null

- ES6：Symbol

  - Symbol()函数会返回symbol类型的值。该类型具有静态属性和静态方法。 

  - 每个从Symbol()返回的symbol值都是唯一的。一个symbol值能作为对象属性的标识符。
    参考链接

  - Symbol.for(key)

    作用：使用给定的key搜索现有的symbol。【key表示symbol中的description】
    返回值：如果找到，返回该symbol；否则将创建一个新的symbol，key作为description，添加到注册表中并返回。

    ```js
    //Symbol([description])  
    //description：对symbol的描述，可用于调试但不是访问symbol本身。
    
    var sym2 = Symbol('sym');
    var sym3 = Symbol('sym');           
    var sym4 = Symbol.for("sym")
    var sym5 = Symbol.for("sym")
    console.log(sym2 == sym3) // false
    console.log(sym4 == sym5) // true
         
    ```

    ###### Symbol.keyFor(symbol)

    作用：从symbol注册表中，返回指定symbol的description，没有则返回undefined。
    返回值：如果有description，返回；否则返回undefined。
    注意：
    1、如果使用`Symbol()`定义的symbol，则不会添加到注册表中；使用`Symbol.for()`定义的symbol则会添加到注册表中。
    2、`Symbol.for(key)`和`Symbol.keyFor(sym)`都是在**Symbol注册表**中进行查找。【不会找到`Symbol()`定义的symbol】

    ```js
    // 访问对象中的私有属性
                for (const key of Object.getOwnPropertySymbols(obj)) {
                  console.log(key); // Symbol(sym)
                }
                
                for (const key of Reflect.ownKeys(obj)) {
                // 访问对象的所有属性
                  console.log(key); // name 、Symbol(sym)
                }
    ```

- 谷歌67版本中还出现了一种 **bigInt**。是指安全存储、操作大整数。

### 2、引用数据类型

Object：function函数、array数组、date日期...等都归属于Object

### 3、基本数据类型与引用数据类型区别

 **声明变量时不同的内存分配**

- 基本数据类型：由于占据的空间大小固定且较小，会被存储在**栈**当中，也就是变量访问的位置
- 引用数据类型：存储在**堆**当中，变量访问的其实是一个指针，它指向存储对象的**内存地址**

**什么是堆？什么是栈？**

- 栈中数据的存取方式为先进后出。而堆是一个优先队列，是按优先级来进行排序的，优先级可以按照大小来规定。

- 在操作系统中，内存被分为栈区和堆区。栈区内存由编译器自动分配释放，存放函数的参数值，局部变量的值等。堆区内存一般由程序员分配释放，若程序员不释放，程序结束时可能由垃圾回收机制回收。

**在复制变量时结果也不一样**

- 基本数据类型复制后2个变量是独立的，因为是把值拷贝了一份

- 引用数据类型则是复制了一个指针，2个变量指向的值是该指针所指向的内容，一旦一方修改，另一方也会受到影响

  ```js
      var x = 3;
      var y = x;
      console.log(x,y); //3 3
      // 基本数据类型复制后2个变量是独立的
      x = 5; //修改一方，另一个不受影响
      console.log(x,y); //5 3 
      
      
      var a = [0,1,2,3];
      var b = a;
      console.log(a, b); //(4) [0, 1, 2, 3] (4) [0, 1, 2, 3]
      a[0] = 9
      console.log(a, b); //(4) [9, 1, 2, 3] (4) [9, 1, 2, 3]
  ```

### 4、深拷贝和浅拷贝

#### 什么是深拷贝、浅拷贝

- **浅拷贝** ：只复制指向某个对象的指针，而不复制对象本身，新旧对象还是共用一个内存块
- **深拷贝**：新建一个一模一样的对象，该对象与原对象不[共享内存](https://so.csdn.net/so/search?q=共享内存&spm=1001.2101.3001.7020)，修改新对象也不会影响原对象

#### 赋值和浅拷贝有什么不同

- 赋值：赋值的是内存地址，而不是数据。赋值前后的两个对象共享一片内存。故不管是改变对象中的哪一个属性（基本or引用）都会改变原来对象
- 浅拷贝：逐个成员一次拷贝。如果属性是基本类型，则拷贝值。如果是引用类型，则赋值的地址。故若改变拷贝对象的基本类型值，对原对象没有影响。如果改变的是引用类型值，对原对象有影响。

```js
var obj1 ={
    name:'name',
    age:18,
    arr:[1,2,3]
}
// 赋值
var obj3 = obj1
obj3.age= 20
console.log(obj1)//{ name: 'name', age: 20, arr: [ 1, 2, 3 ] }
console.log(obj3)//{ name: 'name', age: 20, arr: [ 1, 2, 3 ] }

//模拟浅拷贝
function shallowCopy(obj) {
    var newObj = {}
    for(var key in obj){
        newObj[key] = obj[key]

    }
    return newObj
}
var obj2 = shallowCopy(obj1)
console.log(obj2)//{ name: 'name', age: 20, arr: [ 1, 2, 3 ] }


obj2.name = 'new name'
obj2.arr[0]=666

console.log(obj1)//{ name: 'name', age: 20, arr: [ 666, 2, 3 ] }
console.log(obj2)//{ name: 'new name', age: 20, arr: [ 666, 2, 3 ] }

```

#### 浅拷贝的实现方式

- 对象
  - let obj2 = Object.assign({},obj1)
- 数组
  - let arr1 = arr.slice()
  - let arr1 = arr.concat()
  - let bb = {...aa};

#### 深拷贝的实现方式

-  let arr1 = JSON.parse(JSON.stringify(arr))，但当对象里面有函数的话，深拷贝后，函数会消失

- 手写递归函数

  思路：先判断是不是引用类型，如果不是引用类型直接赋值，如果是引用类型则需要判断是不是数组，然后对引用类型遍历，使用递归的方式赋值。如果是基本类型，相当于直接取值，如果不是则会进入下一轮递归。

```js
var obj1 ={
    // name:'name',
    // age:18,
    arr:[1,2,3,[11,11]],
    fn:function () {
        console.log('hi')
    },
    inObj:{
        name:'name',
        age:18,
    }
}


function deepCopy(obj) {
    let newObj = null
    if(typeof obj === 'object' && obj!==null){
        newObj = obj instanceof Array ?[]:{}
        // 如果是数组，key为数组下标
        for(var key in obj){
            // 递归调用
            // 如果是基本类型，再调用一次相当于取出value值赋值给newObj[key]
            // 如果是引用类型，则进入下一层
            newObj[key] = deepCopy(obj[key])
        }
    }else {
        newObj = obj
    }
    return newObj
}

var obj2 = deepCopy(obj1)
console.log(obj2)
```

- 第三方库lodash

```js
var _ = require('lodash');
        var obj1 ={
            name:'jack',
            age:25,
        }
        let obj2 =_.cloneDeep(obj1)
```

### 5、数据类型判断

- **typeof**

  - 只返回了处于其原型链最顶端的 Object 类型

  - 对于基本类型，除 null 以外，均可以返回正确的结果。
  - 对于引用类型，除 function 以外，一律返回 object 类型。
  - 对于 null ，返回 object 类型。
  - undeclared（或者 not defined ）变量typeof 会返回 "undefined"。

- instanceof

  - instanceof 是用来判断 A 是否为 B 的实例，表达式为：A instanceof B，instanceof 检测的是原型

  - 当 A 的 proto 指向 B 的 prototype 时，就认为 A 就是 B 的实例

    ![](https://images2015.cnblogs.com/blog/849589/201601/849589-20160112232510850-2003340583.png)

  - instanceof 只能用来判断两个对象是否属于实例关系， 而不能判断一个对象实例具体属于哪种类型。

  - 故数组里有Array.isArray() 方法检测是不是数组，本质上检测的是对象的 [[Class]] 值

- **constructor**

  - 当一个函数 F被定义时，JS引擎会为F添加 prototype 原型，然后再在 prototype上添加一个 constructor 属性，并让其指向 F 的引用。

  - 当执行 var f = new F() 时，F 被当成了构造函数，f 是F的实例对象，此时 F 原型上的 constructor 传递到了 f 上，因此 f.constructor == F

  - F 利用原型对象上的 constructor 引用了自身，当 F 作为构造函数来创建对象时，原型上的 constructor 就被遗传到了新创建的对象上， 从原型链角度讲，构造函数 F 就是新对象的类型。这样做的意义是，让新对象在诞生以后，就具有可追溯的数据类型。

    ![](https://images2015.cnblogs.com/blog/849589/201705/849589-20170508131800457-2091987664.png)

  - null 和 undefined 是无效的对象，因此是不会有 constructor 存在的，这两种类型的数据需要通过其他方式来判断。

  - 函数的 constructor 是不稳定的，这个主要体现在自定义对象上，当开发者重写 prototype 后，原有的 constructor 引用会丢失，constructor 会默认为 Object.prototype 被重新赋值的是一个 { }， { } 是 new Object() 的字面量，因此 new Object() 会将 Object 原型上的 constructor 传递给 { }，也就是 Object 本身。因此，为了规范开发，在重写对象原型时一般都需要重新给 constructor 赋值，以保证对象实例的类型不被篡改。

- **toString()**

  - toString() 是 Object 的原型方法，调用该方法，默认返回当前对象的 [[Class]] 。这是一个内部属性，其格式为 [object Xxx] ，其中 Xxx 就是对象的类型。
  - 对于 Object 对象，直接调用 toString() 就能返回 [object Object] 。而对于其他对象，则需要通过 call / apply 来调用才能返回正确的类型信息。


### 6、内部属性[[Class]]

- 所有 typeof 返回值为 "object" 的对象都包含一个内部属性 [[Class]]。这个属性无法直接访问，一般通过 Object.prototype.toString().call(value)来查看。
- 我们自己创建的类就不会有[[Class]]，如果要有，可以通过`get [Symbol.toStringTag]() `创建

### 7、内置对象

- 定义：在程序执行前存在全局作用域里的由 js 定义的一些全局值属性、函数和用来实例化其他对象的构造函
  数对象。

- 标准内置对象的分类

  - 值属性：这些全局属性返回一个简单值，这些值没有自己的属性和方法，例如 Infinity、NaN、undefined、null 字面量


  - 函数属性：全局函数可以直接调用，不需要在调用时指定所属对象，执行结束后会将结果直接返回给调用者，例如 eval()、parseFloat()、parseInt() 等


  - 基本对象：基本对象是定义或使用其他对象的基础。基本对象包括一般对象、函数对象和错误对象。例如 Object、Function、Boolean、Symbol、Error 等


  - 数字和日期对象：用来表示数字、日期和执行数学计算的对象。例如 Number、Math、Date


  - 字符串：用来表示和操作字符串的对象，例如 String、RegExp


  - 可索引的集合对象：这些对象表示按照索引值来排序的数据集合，包括数组和类型数组，以及类数组结构的对象。例如 Array


  - 使用键的集合对象：这些集合对象在存储数据时会使用到键，支持按照插入顺序来迭代元素。例如 Map、Set、WeakMap、WeakSet


  - 矢量集合：SIMD 矢量集合中的数据会被组织为一个数据序列。例如 SIMD 等


  - 结构化数据：这些对象用来表示和操作结构化的缓冲区数据，或使用 JSON 编码的数据。例如 JSON 等


  - 控制抽象对象： Promise、Generator 等


  - 反射： Reflect、Proxy


  - 国际化：为了支持多语言处理而加入 ECMAScript 的对象。例如 Intl、Intl.Collator 等


  - WebAssembly


  - 其他：例如 arguments

### 8、undeclared、 undefined、null

- undefined：
  - 定义：在作用域中声明但还没有赋值的变量。
  - undefined 在 js 中不是一个保留字，故undefined 可以作为变量名，这会影响我们对 undefined 值的判断。所以需要安全获得的 undefined 值，比如说 void 0。

- undeclared ：
  - 定义：没有在作用域中声明过的变量。
  -  undeclared（或者 not defined ）变量typeof 会返回 "undefined"。

- null 代表的含义是空对象，主要用于赋值给一些可能会返回对象的变量作为初始化



## Q2:JS开发基本规范

- 一个函数作用域中所有的变量声明应该尽量提到函数首部，用一个 var 声明，不允许出现两个连续的 var 声明，声明时，如果变量没有值，应该给该变量赋值对应类型的初始值，便于他人阅读代码时，能够一目了然的知道变量对应的类型值。
- 代码中出现地址、时间等字符串时需要使用常量代替。
- 在进行比较的时候吧，尽量使用'===', '!=='代替'!='。
- 不要在内置对象的原型上添加方法，如 Array, Date。
- switch 语句必须带有 default 分支。
- for 循环和if 语句必须使用大括号。

## Q3:原型&原型链

### 1、函数对象的prototype属性

- 每个函数对象都有一个prototype属性，这个属性是一根指针，指向原型对象，用途是包含实例共享的属性和方法

- 为原型对象添加方法

  ```js
  function Person(){
  
  }
  
  // 为原型对象添加方法
  Person.prototype.sayName = function(){
      alert(this.name);
  }
  ```

  ![](https://cavszhouyou-1254093697.cos.ap-chongqing.myqcloud.com/peitu18-2.png)

  

### 2、原型对象的constructor 属性

- 原型对象有 constructor 属性，这个属性是一个指针，指向 prototype 所在的函数对象。

  ![](https://cavszhouyou-1254093697.cos.ap-chongqing.myqcloud.com/peitu18-3.png)

### 3、对象的 _proto _属性

- 调用构造函数创建一个新实例后，在这个实例的内部将包含一个指针_proto_，指向构造函数的原型对象

  ```js
  var student = new Person();
  
  console.log(student.__proto__ === Person.prototype); // true
  ```

  ![](https://cavszhouyou-1254093697.cos.ap-chongqing.myqcloud.com/peitu18-5.png)

- isPrototypeOf()

  - 用于测试一个对象是否存在于另一个对象的原型链上。
  - `console.log(Person.prototype.isPrototypeOf(student)); // true`
- Object.getPrototypeOf()

  - 用于返回实例对象的原型
  - `console.log(Object.getPrototypeOf(student) === Person.prototype); // true`

### 4、原型属性

- 属性访问
  - 先在实例本身上搜索，若找不到则往上一层查找原型对象，以此类推。
  - 在使用 for-in 循环时，其中包括了存在于实例中和原型中的属性。
- 属性判断
  - 判断属性是在实例上还是原型对象上：`student.hasOwnProperty("name")`
  - 判断属性是否存在：`name in object`
- 属性获取
  - 可枚举的实例属性，可以使用 Object.keys() 
  - 无论可不可以枚举：Object.getOwnPropertyNames()

### 5、原型链

- 构造函数的 prototype 对象等于另一个类型的实例，此时的 prototype 对象因为是实例，因此将包含一个指向另一个原型的指针 _ proto_

  ![](https://cavszhouyou-1254093697.cos.ap-chongqing.myqcloud.com/peitu18-7.png)

- Object.prototype 就是原型链的终点了，返回的是一个 null 空对象，意味原型链结束。

## Q4:数据表示与操作

### 1、不同进制的表达

- 以 0X、0x 开头的表示为十六进制。
- 以 0、0O、0o 开头的表示为八进制。
- 以 0B、0b 开头的表示为二进制格式。

### 2、 整数的安全范围

- 定义：安全整数指的是在这个范围内的整数转化为二进制存储的时候不会出现精度丢失
- 最大整数是 2^53 - 1，在 ES6 中被定义为 Number.MAX_SAFE_INTEGER。
- 最小整数：1-2^-52，被定义为 Number.MIN_SAFE_INTEGER。
- 超过安全返回会转为 Infinity，并且无法参与下次运算
- 判断一个数是不是有穷的：isFinite（）

### 3、NaN

- 定义：not a number，用于指出数字类型中的错误情况

- typeof NaN; // "number"

- NaN 是一个特殊值，它和自身不相等，是唯一一个非自反（自反，reflexive，即 x === x 不成立）的值。 NaN != NaN为 true。

- isNaN 和 Number.isNaN

  - 函数 isNaN 接收参数后，会尝试将这个参数转换为数值，任何不能被转换为数值的的值都会返回 true，因此非数字值传入也会返回 true ，会影响 NaN 的判断。

    函数 Number.isNaN 会首先判断传入参数是否为数字，如果是数字再继续判断是否为 NaN ，这种方法对于 NaN 的判断更为准确。


### 4、Array

- 构造函数
  - Array（value），value为数组长度

### 5、数值转换

#### 转字符串toString

-  Null 和 Undefined 类型 ，null 转换为 "null"，undefined 转换为 "undefined"，
- Boolean 类型，true 转换为 "true"，false 转换为 "false"。
- Number 类型的值直接转换，不过那些极小和极大的数字会使用指数形式。
- Symbol 类型的值直接转换，但是只允许显式强制类型转换，使用隐式强制类型转换会产生错误。
- 对普通对象来说，除非自行定义 toString() 方法，否则会调用 toString()（Object.prototype.toString()）来返回内部属性 [[Class]] 的值，如"[object Object]"。如果对象有自己的 toString() 方法，字符串化时就调用该方法并使用其返回值。
- [] 的 valueOf 结果为 [] ，toString 的结果为 ""
- {} 的 valueOf 结果为 {} ，toString 的结果为 "[object Object]"

#### 转成数字值toNumber。

- Undefined 类型的值转换为 NaN。
- Null 类型的值转换为 0。
- Boolean 类型的值，true 转换为 1，false 转换为 0。
- String 类型直接转成数字，如果包含非数字值则转换为 NaN，空字符串为 0。
- Symbol 类型的值不能转换为数字，会报错。
- 对象（包括数组）会首先被转换为相应的基本类型值，如果返回的是非数字的基本类型值，则再遵循以上规则将其强制转换为数字。
- 为了将值转换为相应的基本类型值，抽象操作 ToPrimitive 会首先（通过内部操作 DefaultValue）检查该值是否有valueOf() 方法。如果有并且返回基本类型值，就使用该值进行强制类型转换。如果没有就使用 toString() 的返回值（如果存在）来进行强制类型转换。如果 valueOf() 和 toString() 均不返回基本类型值，会产生 TypeError 错误。
- 解析字符串parseInt()和数据类型强制转换Number（）的区别： parseInt() 中含有非数字字符，解析按从左到右的顺序，如果遇到非数字字符就停止。 Number ()）不允许出现非数字字符，否则会失败并返回 NaN。

#### 转成布尔类型

- 假值：
  • undefined
  • null
  • false
  • +0、-0 和 NaN
  • ""
  
- 真值：其他

- 布尔值的强制转换

  （1） if (..) 语句中的条件判断表达式。
  （2） for ( .. ; .. ; .. ) 语句中的条件判断表达式（第二个）。
  （3） while (..) 和 do..while(..) 循环中的条件判断表达式。
  （4） ? : 中的条件判断表达式。
  （5） 逻辑运算符 ||（逻辑或）和 &&（逻辑与）左边的操作数（作为条件判断表达式）。

### 6、假值对象

- 浏览器在某些特定情况下，在常规 JavaScript 语法基础上自己创建了一些外来值，这些就是“假值对象”。
- 假值对象看起来和普通对象并无二致（都有属性，等等），但将它们强制类型转换为布尔值时结果为 false 最常见的例子是 document.all，它是一个类数组对象，包含了页面上的所有元素，由 DOM（而不是 JavaScript 引擎）提供给 JavaScript 程序使用。

### 7、逻辑运算符

#### & 按位与

- 按位与，全都是1才是1

  `console.log(12 & 5);  //返回值4`

  ![](http://c.biancheng.net/uploads/allimg/190827/6-1ZRG5503E94.gif)

#### | 按位或

- 按位或，只有全是0才是0，其他都是1

  `console.log(12 | 5);  //返回值13`

  ![](http://c.biancheng.net/uploads/allimg/190827/6-1ZRG5594I36.gif)

#### ^ 按位异或

- 1和0才是1

  `console.log(12 ^ 5);  //返回值9`

  ![](http://c.biancheng.net/uploads/allimg/190827/6-1ZRG63913W0.gif)

#### ~ 按位取反

- ～x相当于 -（x+1）

```
var temp = ~5;
/*5 二进制 101，补满 32位
00000000000000000000000000000101
按位取反
11111111111111111111111111111010
由于32位开头第一个是1，所以这是一个负数，将二进制转换成负数，需要先反码
00000000000000000000000000000101
之后，再+1
00000000000000000000000000000110
转换成十进制为6，加上符号变成负数 -6
*/
alert(temp);//-6
```

### 8、String

- ‘+’ 拼接字符串，只要有一个是字符串，另一个也会转成字符串





## Q5:正则表达式

## Q6:随机数

## Q7:创建对象

## Q8:继承

## Q9:作用域链

## Q10:this

## Q11:事件

## Q12:闭包

## Q13:js模块化

## Q14:垃圾回收

## Q15:内存泄露

## Q16:节流与防抖

## Q17:事件循环

## Q18:call、apply 及 bind

## Q20:8. 函数柯里化

## Q21:异步编程





## 参考链接

- https://www.cnblogs.com/libo-web/p/15746881.html
- https://www.jianshu.com/p/3ac9a471e63d····
- https://blog.csdn.net/weixin_45753447/article/details/124290929
- https://www.cnblogs.com/onepixel/p/5126046.html
- https://github.com/CavsZhouyou/Front-End-Interview-Notebook/blob/master/JavaScript/JavaScript.md
- http://cavszhouyou.top/JavaScript深入理解之原型与原型链.html
- http://c.biancheng.net/view/5469.html