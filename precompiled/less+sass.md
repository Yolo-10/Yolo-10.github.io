## less 和 sass 的区别
两者的主要区别是**实现方式**不同
less是基于Javascript的，是在客户端处理的
sass是基本Ruby的，是在服务端处理的
前者在解析上略慢于后者
less的变量用`@`,sass的变量用`$`


## less 和 sass 的共性
- 混入
- 运算
- 函数
- 命名空间
- 作用域
- 嵌套
- 变量赋值

## less
[less官方文档](https://less.bootcss.com/#%E6%A6%82%E8%A7%88)

### 变量声明
`@变量名: 变量值;`

### 混合
>混合（Mixin）是一种将一组属性从一个规则集包含（或混入）到另一个规则集的方法。
```less
.bordered {
  border-top: dotted 1px black;
  border-bottom: solid 2px black;
}
#menu a {
  color: #111;
  .bordered();  //这里这里
}
```

### 嵌套语法 
`{}` 与 `&`
`&` 表示当前选择器的父级
** @规则嵌套和冒泡 ** ,@ 规则会被放在前面，同一规则集中的其它元素的相对顺序保持不变。这叫做冒泡（bubbling）
```less
.component {
  width: 300px;
  @media (min-width: 768px) {
    width: 600px;
    @media  (min-resolution: 192dpi) {
      background-image: url(/img/retina2x.png);
    }
  }
  @media (min-width: 1280px) {
    width: 800px;
  }
}

// 编译为
.component {
  width: 300px;
}
@media (min-width: 768px) {
  .component {
    width: 600px;
  }
}
@media (min-width: 768px) and (min-resolution: 192dpi) {
  .component {
    background-image: url(/img/retina2x.png);
  }
}
@media (min-width: 1280px) {
  .component {
    width: 800px;
  }
}
```



### 运算
1. 可进行 数值、颜色、变量的预算
2. +、- 带单位运算一般以左边数值单位为主，先进行单位换算，再进行运算；若单位换算没有意义，则直接进行数值运算。
```less
// 所有操作数被转换成相同的单位
@conversion-1: 5cm + 10mm; // 结果是 6cm
@conversion-2: 2 - 3cm - 5mm; // 结果是 -1.5cm

// conversion is impossible
@incompatible-units: 2 + 5px - 3cm; // 结果是 4px

// example with variables
@base: 5%;
@filler: @base * 2; // 结果是 10%
@other: @base + @filler; // 结果是 15%
```
3. *、/ 的带单位运算没有意义，直接进行运算
```less
@base: 2cm * 3mm; // 结果是 6cm
```
4. `calc`函数
`calc`函数并不对数学表达式进行计算，而是对变量和数学表达式进行计算

### 转义
转义（Escaping）允许你使用任意字符串作为属性或变量值。原样输出。
```less
@min768: (min-width: 768px);
.element {
  @media @min768 {
    font-size: 1.2rem;
  }
}

// --->
@media (min-width: 768px) {
  .element {
    font-size: 1.2rem;
  }
}
```

### 函数
超多内置函数，[less函数手册](https://less.bootcss.com/functions/)


### 命名空间和访问符
```less
#bundle() {
  .button {
    ....
  }
  .tab { ... }
  .citation { ... }
}

#header a {
  color: orange;
  #bundle.button();  // 还可以书写为 #bundle > .button 形式
}
```

### 映射
```less
#colors() {
  primary: blue;
  secondary: green;
}

.button {
  color: #colors[primary];
  border: 1px solid #colors[secondary];
}
```

### 作用域
在本地查找变量和混合（mixins），如果找不到，则从“父”级作用域继承。
混合（mixin）和变量的定义不必在引用之前事先定义。
```less
@var: red;

#page {
  #header {
    color: @var; // white
  }
  @var: white;
}
```

### 注释
```less
/* 一个块注释
 * style comment! */
@var: red;

// 这一行被注释掉了！
@var: white;
```

### 导入
```less
@import "library"; // library.less
@import "library.less"; // library.less
```



## sass

### 变量声明： $变量名: 变量值;

### 局部作用域
在本地查找变量和混合（mixins），如果找不到，则从“父”级作用域继承。

### 全局变量声明 `!global`
```scss
$myColor: red;
h1 {
  $myColor: green !global;  // 全局作用域
  color: $myColor;  //green
}
p {
  color: $myColor; //green
}
```

### 嵌套规则与属性
```sass
nav {
  ul {
    margin: 0;
    padding: 0;
    list###style: none;
  }
}

// nav ul {
//   margin: 0;
//   padding: 0;
//   list###style: none;
// } 

text: {
  align: center;
  transform: lowercase;
  overflow: hidden;
}

//text###align: center;
//text###transform: lowercase;
//text###overflow: hidden;
```

//TODO:待续
https://www.runoob.com/sass/sass-import-partials.html

