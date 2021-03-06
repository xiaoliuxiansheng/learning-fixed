#### 全局属性
| 属性	    | 描述                     |
| ---       | ---                     |
|Infinity	| 代表正的无穷大的数值。|
|NaN	    | 指示某个值是不是数字值。|
|null       | 特指对象的值未设置。|
|undefined	| 指示未定义的值。|
|globalThis | 可以获取全局对象。|

#### 全局函数

全局函数可以直接调用，不需要在调用时指定所属对象，执行结束后会将结果直接返回给调用者。

| 属性	             | 描述                     |
| ---                | ---                     |
| eval(string)	                 | 计算 JavaScript 字符串，并把它作为脚本代码来执行。|
| uneval(object)                 | 函数创建一个代表对象的源代码的字符串。|
| decodeURI(encodedURI)	         | 解码某个编码的 URI。只操作空格和参数|
| decodeURIComponent(encodedURI) | 解码一个编码的 URI 组件。|
| encodeURI(URI)	             | 把字符串编码为 URI。只操作空格和参数|
| encodeURIComponent(URI)        | 把字符串编码为 URI 组件。|
| isFinite(value)	             | 检查某个值是否为有穷大的数。|
| isNaN(value)	                 | 检查某个值是否是数字。|
| Number(object)	             | 把对象的值转换为数字。|
| parseInt((string, radix)	     | 解析一个字符串并返回一个整数。|
| parseFloat(string)	         | 解析一个字符串并返回一个浮点数。|
| String(object)                 | 把对象的值转换为字符串。|
| escape()	 Q                   | 对字符串进行编码。|
| unescape() Q                   | 对由 escape() 编码的字符串进行解码。|

### Function 构造函数

创建一个新的Function对象。 在 JavaScript 中, 每个函数实际上都是一个Function对象。

```js
new Function ([arg1[, arg2[, ...argN]],] functionBody)
```
#### 属性和方法

全局的Function对象没有自己的属性和方法, 但是,
因为它本身也是函数，所以它也会通过原型链从`Function.prototype`上继承部分属性和方法。


#### Function 原型对象属性

| 属性	             | 描述                     |
| ---                | ---                     |
| Function.length	 | 函数的形参个数。 |
| Function.name	     | 返回函数实例的名称。 |
| Function.prototype | 存储了 Function 的原型对象。 |
| Function.prototype.constructor| 声明函数的原型构造方法，详细请参考 Object.constructor 。 |

#### Function 原型对象方法

| 属性	                       |语法                                            |描述                     |
| ---                          | ---                                            | ---                     |
| Function.prototype.call()    | func.apply(thisArg, \[argsArray])              |使用一个指定的 this 值和一个数组（或类似数组对象）提供的参数。来调用一个函数。 |
| Function.prototype.apply()   | func.call(thisArg, arg1, arg2, ...)            |使用一个指定的 this 值和单独给出的一个或多个参数。来调用一个函数。 |
| Function.prototype.bind()	   | function.bind(thisArg\[,arg1\[,arg2\[, ...]]]) |创建一个新的函数，在bind()被调用时，这个新函数的this指向bind的第一个参数，其余的参数将作为新函数的参数供调用时使用。 |
| Function.prototype.toString()| function.toString()                            | 返回一个表示当前函数源代码的字符串。|

> 注意：call()方法的作用和apply()方法类似，区别就是call()方法接受的是参数列表，而apply()方法接受的是一个参数数组

```js
/* 找出数组中最大/小的数字 */
var numbers = [5, 6, 2, 3, 7];

/* 应用(apply) Math.min/Math.max 内置函数完成 */
var max = Math.max.apply(null, numbers); /* 基本等同于 Math.max(numbers[0], ...) 或 Math.max(5, 6, ..) */
var min = Math.min.apply(null, numbers);

// 连接数组，其实concat也可以
var array = ['a', 'b'];
var elements = [0, 1, 2];
array.push.apply(array, elements);
console.info(array); // ["a", "b", 0, 1, 2]

```
```js
function Product(name, price) {
  this.name = name;
  this.price = price;
}

function Food(name, price) {
  Product.call(this, name, price);
  this.category = 'food';
}

function Toy(name, price) {
  Product.call(this, name, price);
  this.category = 'toy';
}

var cheese = new Food('feta', 5);
var fun = new Toy('robot', 40);




function greet() {
  var reply = [this.animal, 'typically sleep between', this.sleepDuration].join(' ');
  console.log(reply);
}

var obj = {
  animal: 'cats', sleepDuration: '12 and 16 hours'
};

greet.call(obj);  // cats typically sleep between 12 and 16 hours
```
```js
this.x = 9;    // 在浏览器中，this指向全局的 "window" 对象
var module = {
  x: 81,
  getX: function() { return this.x; }
};

module.getX(); // 81

var retrieveX = module.getX;
retrieveX();   
// 返回9 - 因为函数是在全局作用域中调用的

// 创建一个新函数，把 'this' 绑定到 module 对象
// 新手可能会将全局变量 x 与 module 的属性 x 混淆
var boundGetX = retrieveX.bind(module);
boundGetX(); // 81


function list() {
  return Array.prototype.slice.call(arguments);
}

function addArguments(arg1, arg2) {
    return arg1 + arg2
}

var list1 = list(1, 2, 3); // [1, 2, 3]

var result1 = addArguments(1, 2); // 3

// 创建一个函数，它拥有预设参数列表。
var leadingThirtysevenList = list.bind(null, 37);

// 创建一个函数，它拥有预设的第一个参数
var addThirtySeven = addArguments.bind(null, 37); 

var list2 = leadingThirtysevenList(); 
// [37]

var list3 = leadingThirtysevenList(1, 2, 3); 
// [37, 1, 2, 3]

var result2 = addThirtySeven(5); 
// 37 + 5 = 42 

var result3 = addThirtySeven(5, 10);
// 37 + 5 = 42 ，第二个参数被忽略
```
### 箭头函数

箭头函数表达式的语法比函数表达式更简洁，并且没有自己的this，arguments，super或 new.target。这些函数表达式更适用于那些本来需要匿名函数的地方，并且它们不能用作构造函数。

* 基础语法
```js
(参数1, 参数2, …, 参数N) => { 函数声明 }

// 相当于：(参数1, 参数2, …, 参数N) =>{ return 表达式; }
(参数1, 参数2, …, 参数N) => 表达式（单一） 

// 当只有一个参数时，圆括号是可选的：
(单一参数) => {函数声明}
单一参数 => {函数声明}

// 没有参数的函数应该写成一对圆括号。
() => {函数声明}
```
* 高级语法
```js
//加括号的函数体返回对象字面表达式：
参数=> ({foo: bar})

//支持剩余参数和默认参数
(参数1, 参数2, ...rest) => {函数声明}
(参数1 = 默认值1,参数2, …, 参数N = 默认值N) => {函数声明}

//同样支持参数列表解构
let f = ([a, b] = [1, 2], {x: c} = {x: a + b}) => a + b + c;
f();  // 6
```

### 函数默认参数

```js
function [name]([param1[ = defaultValue1 ][, ..., paramN[ = defaultValueN ]]]) { 
    statements 
}
```

### 方法的定义

```js
var obj = {
  property( parameters… ) {},
  *generator( parameters… ) {},
  async property( parameters… ) {},
  async* generator( parameters… ) {},

  // with computed keys:
  [property]( parameters… ) {},
  *[generator]( parameters… ) {},
  async [property]( parameters… ) {},

  // compare getter/setter syntax:
  get property() {},
  set property(value) {}
};
```
### 剩余参数

剩余参数语法允许我们将一个不定数量的参数表示为一个数组。

```js
function(a, b, ...theArgs) {
  // ...
}
```
### Arguments 对象

arguments 是一个对应于传递给函数的参数的类数组对象。

arguments对象是所有（非箭头）函数中都可用的局部变量。
```js
arguments[0]
arguments.length
```

### getter

get语法将对象属性绑定到查询该属性时将被调用的函数。

```js
{get prop() { ... } }
{get [expression]() { ... } }
```

### setter

当尝试设置属性时，set语法将对象属性绑定到要调用的函数。

```js
{set prop(val) { ... } }
{set [expression](val) { ... } }
```