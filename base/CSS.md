## 盒模型
盒模型定义每个盒子的组成部分：`content` + `padding` + `border` + `margin`

标准盒模型：`width`，`height` 设置的是 `content`，加上 `padding`，`border` 后，表示盒子的大小

传统盒模型：`width`，`height` 设置的是可见宽高，即除 `content` 外，还包含 `padding`，`border`

**怎么设置盒子模型标准**

设置 `box-sizing`，默认使用标准模型（`box-sizing: content-box`）

| 值          | 描述                                                         |
| :---------- | :----------------------------------------------------------- |
| content-box | 这是由 CSS2.1 规定的宽度高度行为。宽度和高度分别应用到元素的内容框。在宽度和高度之外绘制元素的内边距和边框。 |
| border-box  | 为元素设定的宽度和高度决定了元素的边框盒，通过从已设定的宽度和高度分别减去边框和内边距才能得到内容的宽度和高度。 |
| inherit     | 规定应从父元素继承 box-sizing 属性的值。     

**块级盒子和内联盒子的区别**
1. 换行 

    每个块级盒子都会换行；行内盒子不会换行
2. 默认宽高

    块级盒子默认宽度为父盒子的宽度100%，默认高度取决于内容本身；
    内联盒子默认宽高都取决于内容本身。
3. 设置宽高

    块级盒子可设置属性 `width`和`height`控制宽高；
    内联盒子直接设置不起作用，需要设置 `display:block` 或 `display:inline-block` 变成块级盒子后，再设置宽高。
4. 内外边距及边框

   块级盒子设置内外边距，边框大小后，水平垂直方向都被应用，且能将其他元素按预期推开；内联元素设置这些属性后，水平垂直方向都被应用，但是只有水平方向会按预期将元素推开

5. 包含的元素

   块级元素可以包含块级元素和行内元素；

   行内元素中最好只包含行内元素，如包含块级元素，父行内元素并不会包含子块级元素


## `display:none` 和 `visibility:hidden` 的区别
相同点：都能隐藏元素
不同点：
1. 是否占据空间
`display:none`将元素的显示设为无，即在网页中不占任何的位置。
`visibility:hidden`将元素隐藏，但是在网页中该占的位置还是占着。
2. 是否具有继承属性
`display:none`，`display`不是继承属性，元素及其子元素都会消失。
`visibility:hidden`，`visibility`是继承属性，若子元素使用了`visibility:visible`，则不继承，这个子孙元素又会显现出来。
3. 是否渲染
`display:none`，会触发reflow（回流），进行渲染。
`visibility:hidden`，只会触发repaint（重绘），因为没有发现位置变化，不进行渲染。
4. 读屏器是否读取
读屏器不会读取`display：none`的元素内容，而会读取`visibility：hidden`的元素内容。比如在有序列表中 `visibility：hidden`是参与计数的。

## BFC
`BFC（Block Formatting Context）`块格式化上下文，指的是一个独立的渲染空间或隔离开的独立容器，将内部内容与外部上下文隔开，不会和外部的元素相互影响或重叠。

### BFC 的好处
① 阻止标准流元素被浮动元素覆盖
② 解决外边距的塌陷问题(垂直塌陷)
③ 利用BFC解决包含塌陷
④ 清除浮动
例子：[BFC实用例子](https://blog.csdn.net/sqLeiQ/article/details/125261564)

### 形成BFC的条件
* 根元素：`HTML`
* `float` 设为非 `none` 的值，如 `left`，`right`
* `overflow` 设为非 `visible` 的值，如 `hidden`，`auto`，`scroll`
* `position: absolute` 或 `position:fixed`
* 块级容器，如 `display: inline-block`, `table-cell`，`table-caption`，`flex`，`inline-flex`，`grid`，`inline-grid`

## 清除浮动
参考：[原理案例-详细解释](https://blog.csdn.net/weixin_45842954/article/details/125081389)

1. 在浮动元素后增加一个空元素，并设置CSS属性：`clear:both` 
代码少，简单，但不符合语义化，代码不优雅，后期维护不易
**原理**：添加一个空`div`，利用`css`提高的`clear:both`清除浮动，让父级`div`能自动获取到高度。
2. 父元素属性中设置 `overflow:hidden`或`overflow:auto`，触发BFC
内容增多时不会换行导致内容被隐藏掉，无法显示要溢出的元素。
3. 父元素 伪类:after中设置 （**推荐**）
```css
/* 原理跟第一种方法一样 */
.root:after{
    content: '';
    display: block;
    clear: both;
}
/* 兼容IE6以下，触发其hasLayout属性 */
.root{
    zoom: 1;
}
```

## CSS3新特性
CSS3的新特性大致分为以下六大类
- CSS3选择器
- CSS3边框与圆角
- CSS3背景与渐变
- CSS3过渡
- CSS3变换
- CSS3动画

