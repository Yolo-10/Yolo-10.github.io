## js数据类型
### 基本数据类型
存储在栈
7种：`number`、`string`、`boolean`、`null`、`underfined`、`symbol(独一无二)`、`BigInt(安全地存储和操作大整数)`
###  引用数据类型
存储在堆
`Object`，`Object`的对象子类型(`array`、`function`、`date`)

### 判断数据类型
- `null`，`undefined` 直接使用严格相等 === 判断
- 非 `null`，`undefined` 的基本类型及函数使用 `typeof` 判断
- 剩余内置类型使用 `Object.prototype.toString`判断
- 自定义类型使用`instanceof`


## 页面的 `load` 和 `ready` 事件
- `load`是当所有资源(包括DOM文档树、CSS文件、JS文件、图片资源)全部加载完成后，执行一个函数。
当图片资源较多，加载时间较长，`onload`需要等待较长时间。
- `ready`是当DOM文档树加载完成后执行一个函数（不包含CSS、图片等）,所以会比`load`更快执行
>原生JS中并不包含`ready`方法，只有`load`方法就是`onload`事件

## 闭包
[参考博客](https://blog.csdn.net/weixin_44823731/article/details/105887722)
闭包：有权访问另一个函数作用域中的变量的函数。
作用：正常的函数，在执行完之后，函数里面声明的变量就会被**垃圾回收**处理掉。但是闭包可以让一个函数作用域中的变量，在执行完之后依旧没有被垃圾回收处理掉。 会造成**内存泄露**。

### 经典闭包例子
```js
var temp = "小芬";
function a(){
    var temp = "小明";
    return function(){
        console.log(temp);
    }
}
a()(); //小明
```

### 经典面试题
```js
for (var i = 0; i < 4; i++) {
  setTimeout(function() {
    console.log(i);
  }, 300);
} 
// 4 4 4 4 
// setTimeout()是一个异步函数，所以这道题的代码执行顺序时，先执行for循环完，此时i的值时4，再执行setTimeout种的方法。以至于打印出来的都是4
```
**如何使输出变为 0,1,2,3**
```js
//闭包
for (var i = 0; i < 4; i++) {
    setTimeout(function() {
        var tem = i;
        return function(){
            console.log(tem);
        }
    }(), 300);
  } 

//简写闭包
for (var i = 0; i < 4; i++) {
    setTimeout(function(i) {
        return function(){
            console.log(i);
            //从内往外找
        }
    }(i), 300);
} 

//let块级作用域
for (let i = 0; i < 4; i++) {
    setTimeout(function() {
        var tem = i;
        return function(){
            console.log(tem);
        }
    }(), 300);
} 
```

## 异步任务
> js代码执行顺序：
    没有异步任务时：代码从上往下执行。
    有异步任务是：代码先执行主线程，执行完之后才执行异步任务。


## 作用域

> 红宝书中也称作用域为执行环境

> **LHS查询**—查找目标
> 变量出现在赋值操作的左侧时
> 查找变量 并试图为变量赋一新值
>
> **RHS查询**—查找源头
> 变量出现在赋值操作的右（非左）侧时
> 查询并获取变量的值
>
> —— 《你不知道的 JS》

> 活动对象和变量对象其实是一个东西
>
> VO：变量对象是与执行上下文相关的数据作用域，存储了在上下文中定义的变量和函数声明；是规范上的或者说是引擎实现上的，不可在 JavaScript 环境中访问
>
> AO：只有到当进入一个执行上下文中，这个执行上下文的变量对象才会被激活，能访问该对象的各种属性。被激活的变量对象即活动对象

### 1. JS 作用域分为哪几类，作用域大小怎么定义？

> 作用域是根据名称查找变量的一套规则
>
> 欺骗词法作用域的方法有：`eval`，`with`

JavaScript 采用词法作用域模型，即作用域在写代码时进行静态确定，主要关注在何处声明

> 函数的作用域在函数定义的时候就决定了，与何处调用无关

![图例](https://static001.geekbang.org/resource/image/21/39/216433d2d0c64149a731d84ba1a07739.png)

JavaScript 中的作用域包含：

* 全局作用域
* 函数作用域：属于这个函数的全部变量都可以在整个函数的范围内使用及复用
* 块级作用域：将变量绑定到所在的任意作用域中（通常是{ .. }内部）



### 2. let, var, const 区别

首先，从作用域角度看，`var` 声明的变量可以使用在**全局作用域以及函数作用域**中，而 `let` 与 `const` 声明的变量是限制在**块级作用域**中的。所以，在最程序顶部声明时，`var` 会**向全局对象添加属性**，`let` 不会。

其次，`var` 会进行**变量提升**处理，因此将声明放在其所在作用域的任一行都可以，在被赋值前返回值为 `undefined`，但 `let` 不会，在声明前是不可访问的。受 `let` 这一特点的影响，在执行到 `let` 初始化语句前，存在**暂存死区**，如果使用该变量会抛出 `ReferenceError`，提示不能在初始化前取到该变量。

最后，`var` 在同个作用域内可**重复声明**，后面的值会覆盖前面的值，但 `let` 在同个作用域内如重复声明，会抛出语法错误，提示不要重复声明。这个在写 `switch` 语句时会碰到，因为 ``switch``语句为一个块作用域。`const`声明的是常量，`const`需在**定义变量时就声明**，不可修改。


### 3. 何为提升

这里我认为可以分为广义和狭义理解

广义上讲，提升，指的是所有声明都会被移动到所在作用域的顶部。

这是因为 JavaScript 引擎在执行代码前会进行编译，在这过程中将声明的变量关联其所在作用域，而赋值及其他逻辑会等到运行时进行。

狭义上，指的是 `var` 所具有的变量提升的特点。

这是因为在处理`var` 声明时，除绑定作用域外，还进行了初始化，设为 `undefined`。而`let`，`const`  没有初始化，所以没有变量提升。

在这部分还需要注意的是，函数表达式的表现和变量声明相似，只会提升声明本身，不提升赋值，但函数声明会全部提升，包含函数体。类 `class` 虽然本质是一个函数，但只会提升声明，需要在初始化后再创建实例，否则也会抛出错误。



### 4. 为什么需要块级作用域

[ECMAScript 6 入门](https://es6.ruanyifeng.com/#docs/let#%E4%B8%BA%E4%BB%80%E4%B9%88%E9%9C%80%E8%A6%81%E5%9D%97%E7%BA%A7%E4%BD%9C%E7%94%A8%E5%9F%9F%EF%BC%9F)

主要是因为，原有全局作用域和函数作用域的设计会带来一些使用上的问题。

在循环语句中，使用 `var` 声明的计数变量在循环外也可以使用，变量**污染外部函数作用域**。

同时，`var` 变量提升，可重复声明的特点可能导致**内部变量覆盖外部变量**，引发意料外的错误。

```javascript
var tmp = 1;

function f() {
    console.log(tmp);
    if (false) {
        var tmp = 'test';
    }
}

f(); // undefined
```

此时主要的问题在于可维护性。

如果**涉及异步代码**，就会未取到当次循环数据的情况。因为当处理回调函数时，变量取所在函数作用域中的值，此时为循环结束的数值。比如循环 0 到 3，最后输出 3 次 3。如使用块作用域，变量会使用块级作用域中的值，因此还是按序输出。

```javascript
for (var i = 0; i < 3; i++) {
    setTimeout(function() {
        console.log(i);
    }, 0);
}
// 3 3 3
```

```javascript
for (let i = 0; i < 3; j++) {
    setTimeout(function() {
        console.log(i);
    }, 0);
}
// 0 1 2
```



### 5. ES6 前如何使用块级作用域

[MDN - with](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/with)

**第一种**，可以使用 `with`。

`with` 将一个对象添加到作用域链顶部，处理为一个被隔离的词法作用域。

其括号中语句的赋值操作，会对属性进行 LHS查询。

在非严格模式下，当整个作用域链中都不存在该变量时，会在全局作用域中创建该变量，并进行赋值。

ES5 严格模式下已被禁用，会抛出语法错误。

它是一种欺骗词法作用域的方式。

**第二种**，是使用 `try/catch` 语句，其中 `catch` 分句会创建一块作用域 。

**第三种**，使用匿名函数

Babel 处理这部分转换是比较灵活的，如：

* 处理循环中带异步时，除 `let` 声明变为 `var` 外，将相关异步操作变为一函数，控制在新的函数作用域内
* 在重复声明方面，`let` 处理为 `var` 时，如变更会引起重复声明，则修改一方的变量名

```javascript
// 匿名函数
(function(){
	// 块级
})()
```

```javascript
// with - 传入对象中有对应属性
var o1 = {
    a: 1
}

function f(obj) {
    with(obj) {
        a = 2
    }
}

f(o1)
console.log(o1.a) // 2
```

```javascript
// with - 传入无对应属性
var n1 = 1

function f(obj) {
    with(obj) {
        a = 2
    }
}

f(n1)
console.log(n1, a) // 1 2
```

```javascript
// with - LHS查询，延作用域链查找
var o1 = {
    a: 1
}

function f(obj) {
    var b = 2
    with(obj) {
        b = 3
    }
    console.log(b)
}

f(o1)
console.log(o1.a) // 3 1
```

```javascript
// ES6
let a = 2;
console.log(a);
{
	let a = 1;
  console.log(a);
}

// Babel处理后
var a = 2;
console.log(a);
{
  var _a = 1;
  console.log(_a);
}

```

```javascript
// ES6 代码见前一问示例

// Babel处理后
var _loop = function _loop(i) {
  setTimeout(function () {
    console.log(i);
  }, 0);
};

for (var i = 0; i < 3; j++) {
  _loop(i);
}
```



### 6. 作用域链

[JavaScript深入之作用域链](https://github.com/mqyqingfeng/Blog/issues/6)

> 函数有 [[scope]]，[[scope]] 就是所有父变量对象的层级链
>
> 执行上下文有 Scope，作用域链

当查找变量的时候，会先从当前上下文的变量对象中查找

如果没有找到，就会从父级(词法层面上的父级)执行上下文的变量对象中查找

一直找到全局上下文的变量对象，也就是全局对象。

这样由多个执行上下文的变量对象构成的链表就叫做作用域链。


## ES6新特性
[详细文档](https://blog.csdn.net/qq_49075665/article/details/123910836)
- let、const、var
- 箭头函数
- 拓展运算符
- 解构赋值
- Array拓展方法：Array.from、find、findIndex、includes、展开操作符
- String扩展方法：startWith、endWith、repeat、模板字符串
- 新的数据结构Set, add、delete、has、clear、forEach
- class、get、set
- 模块脚本、import、export
- promise

## 防抖
> 时间多次触发，只有当n秒内不触发才执行，否则重新计时

适用场景如：文本编辑器实时保存，防止多次提交

```js
function debounce(func,delay){
    let timeout;
    return function(...args){
        clearTimeout(timeout);
        timeout = setTimeout(()=>{
            func.apply(this,args);
        },delay)
    }
}
```

## 节流
> 一段时间只执行一次

适用场景如：获取滚动条位置 scroll，拖拽，缩放

```js
function throttle(func,delay){
    let canUse = true;
    return function(...args){
        if(canUse){
            func.apply(this,args);
            canUse = false;
            setTimeout(()=>canUse=true,delay)
        }
    }
}
```


## 原型链
任何对象都有原型对象,也就是`prototype`属性,任何原型对象也是一个对象,该对象就有`proto`属性,这样一
层一层往上找,就形成了一条链,我们称此为原型链
[原型链](images/原型链.png)

## this指向
- 函数调用时决定的，一般指向调用者
- 通过new这个函数，把它当成构造函数使用时，`this`指向实例对象
- 当成普通函数使用时，`this`就会指向全局指向，在浏览器中是`window`，在`node`中就是`global`, 严格模式下`use strict`是`underfined`

### 为什么react函数式组件中没有`this`指向
- 这是经过`babel`翻译(将jsx语法翻译成js)的结果
- Babel是在严格模式`use strict`下执行的，不允许`this`指向`window`,所以`this`指向为`underfined`

### 改变`this`指向的方法
#### call 
```js
function fn(x, y) {
    console.log(this); //{name: 'andy'}
    console.log(x + y); //3
}
var o = {
    name: 'andy'
};
fn.call(o, 1, 2);//调用了函数此时的this指向了对象o
```

#### apply
```js
var o = {
    name: 'andy'
}
function fn(a, b) {
    console.log(this); //{name: 'andy'}
    console.log(a+b) //3
};
fn.apply(o,[1,2])
```

#### bind
bind() 方法不会调用函数,但是能改变函数内部this 指向,返回的是原函数改变this之后产生的新函数
```js
var o = {
    name: 'andy'
};
function fn(a, b) {
    console.log(this);//{name: 'andy'}
    console.log(a + b);//3
};
var f = fn.bind(o, 1, 2); //此处的f是bind返回的新函数
f();//调用新函数 this指向的是对象o 参数使用逗号隔开
```


#### call、apply、bind三者的异同
- 共同点 : 都可以改变this指向
- 不同点:
    1. call 和 apply 会调用函数, 并且改变函数内部this指向.
    2. call 和 apply传递的参数不一样,call传递参数使用逗号隔开,apply使用数组传递
    4. bind 不会调用函数, 可以改变函数内部this指向.
- 应用场景
    1. call 经常做继承.
    2. apply经常跟数组有关系. 比如借助于数学对象实现数组最大值最小值
    3. bind 不调用函数,但是还想改变this指向. 比如改变定时器内部的this指向.



## `=`,`==`,`===`有什么区别？
`=`:赋值运算符
`==`: 判断值是否相等
`===`: 判断引用地址指向内存是否相等
