# CSS面试题汇总

##### 1. BFC：什么是BFC？创建BFC的方法？BFC的作用？

**概念：**

* MDN：块格式化上下文（Block Formatting Context，BFC）是Web页面的可视CSS渲染的一部分，是块级盒子的布局过程发生的区域，也是浮动元素与其他元素交互的区域。详情参考[MDN](https://developer.mozilla.org/zh-CN/docs/Web/Guide/CSS/Block_formatting_context)
* 块级格式化上下文，是页面上用于渲染CSS的一个区域，相当于一个小型的布局，块级元素和浮动元素会根据这块区域进行布局。



**创建BFC方法：**1. display类， 2. overflow，3. 根元素、浮动元素和绝对定位元素

* display
  * `display`值为`flow-root`的元素 <span style="color: red;font-weight: bolder">最常用</span>
  * 行内块元素（`display`值为`inline-block`）
  * 表格单元格（`display`值为`table-cell`，HTML表格单元格默认值）
  * 表格标题（`display`值为`table-caption`，HTML表格标题默认值）
  * 匿名表格单元格元素（`display`值为`table`、`table-row`，`table-row-group`、`table-header-group`、`table-footer-group`、`inline-table`）
  * 弹性元素（`display`值为`flex`或`inline-flex`元素的直接子元素）
* overflow
  * `overflow`值不为`visible`、`clip`的块元素， <span style="color: red;font-weight: bolder">相对常用，</span>可能会遇到不希望出现的滚动条或阴影
* 根元素（`<html>`）
* 浮动元素（`float`值不为`none`）
* 绝对定位元素（`position`值为`absolute`或`fixed`）



**BFC作用：**

* **包含内部浮动**：计算父元素的高度时，浮动子元素也参与计算，从而能够解决高度坍塌的问题。
* **排除外部浮动**
* **阻止外边距重叠**

**注意：**格式化上下文影响布局，通常，我们会为定位和清除浮动创建新的BFC，而不是更改布局。

---

##### 2. 水平垂直居中实现方式有哪些？

###### 单行文本水平垂直居中

```html
<div class="text">我是单行文本<div>
```

```css
.text {
  widh: 100px;
  height: 100px;
  text-align: center;
  // 垂直居中：行高和div高度一致
  line-height: 100px;
}
```

---

###### 多行文本水平垂直居中

```html
<div class="text">我是多行文本我是多行文本我是多行文本我是多行文本我是多行文本我是多行文本</div>
```

借用[vertical-align](https://developer.mozilla.org/zh-CN/docs/Web/CSS/vertical-align)设置垂直对齐方式。

> CSS属性**vertical-align**用来指定行内元素（inline）或表格单元格（table-cell）元素的垂直对齐方式。

```css
.text {
  width: 100px;
  height: 100px;
  // 水平居中
  text-align: center;
  // 垂直居中
  display: table-cell;
  vertical-align: middle;
}
```

---

###### 块级元素固定宽高——水平垂直居中

```html
<style>
  html,body {
    width: 100%;
    height: 100%;
  }
  div {
    width: 200px;
    height: 200px;
    border: 1px solid black;
    background-color: #000;
  }
</style>

<body>
    <div id="container"></div>
</body>
```
1. 子元素固定宽高：绝对定位 + 负margin

   ```css
   #container {
     position: absolute; 
     width: 200px; 
     height: 200px; 
     background: green;
     top: 50%; 
     left: 50%;
     margin: -100px 0 0 -100px;
   }
   ```

---

###### 块级元素不固定宽高——水平垂直居

```html
<style>
  html,body {
    width: 100%;
    height: 100%;
  }
</style>

<body>
    <div id="container">块级元素不固定宽高</div>
</body>
```

1. **Flex布局**

   ```css
   body {
       display: flex;
       justify-content: center;
       align-items: center;
   }
   ```

2. **绝对定位 + transform:translate：**可以理解为是已知块元素宽高负margin的另一种方式。

   ```css
   #container {
     position: absolute;
     top: 50%;
     left: 50%;
     transform: translate(-50%, -50%);
   }
   ```

   **注意：**`translate(arg1, arg2)`参数为百分比时，其代表的是当前盒模型宽高的百分比。

3. **行内元素 + table-cell：**设置`display`为`table-cell`的元素，其宽高不能为百分比。

   ```html
   <div id="center">
     <div id="container">块级元素不固定宽高</div>
   </div>
   ```

   ```css
   #center {
     width: 500px;
     height: 500px;
   	background: yellow;
     display: table-cell;
     vertical-align: middle;
     text-align: center;
   }
   
   #container {
     background: green;
     // table-cell只对行内元素有效
     display: inline-block;
   }
   ```

4. **grid布局**

   ```css
   body {
     display: grid;
     // 由此可见：grid布局 这两个参数和flex一样
     justify-content: center;
     align-content: center;
     
     // 或者
     justify-items: center;
     align-items: center;
   }
   ```

---



##### 3. 用css画一个三角形？

用css绘制一个三角形或者梯形，主要利用`border`的单值样式：在一个无宽高的元素下，只设置四个边框的话，其每个边框交汇处会形成斜边，从而作为绘制三角形或者梯形的基础。利用不同方向的边框宽度和元素宽高可以绘制出不同方向的三角形或梯形。

1. 三角形

   ```css
   #container {
     width: 0;
     height: 0;
     border: 30px solid transparent;
     // 不同方向的三角形，更换不同方向的边框即可
     border-top-color: red;
   }
   ```

2. 梯形

   ```css
   #container {
     // 相较于三角形，元素需要指定宽高
     width: 50px;
     height: 50px;
     border: 30px solid transparent;
     border-top-color: red;
   }
   ```

3. 扇形

   ```css
   #container {
     width: 0;
     height: 0;
     border: 50px solid transparent;
     border-top-color: red;
     // 相较于三角形，元素多加了圆角
     border-top-left-radius: 50px;
     border-top-right-radius: 50px;
   }
   ```

---

##### 4. css高度坍塌

**高度坍塌**：在文档流中，父元素的高度默认是由子元素的高度撑起来的，也就是子元素多高，父元素就多高。一旦子元素设置浮动脱离文档流，导致无法撑起父元素的高度，就会引起父元素高度塌陷的情况。例：

```html
<div id="container">
  <div id="sub"></div>
</div>
```

```css
#container {
  border: 1px solid red;
}

#sub {
  background: red;
  width: 100px;
  height: 100px;
  // 此时设置了浮动，就会导致父元素坍塌
  float: right;
}
```

**解决方法：**

1. 父元素 + `overflow: hidden;`

   ```css
   #container {
     border: 1px solid red;
     overflow: hidden;
   }
   ```

   ​	`overflow`本意是处理元素溢出时所需要的行为；但是为什么能够清除浮动呢？其原因是产生了BFC。

2. 伪元素清除浮动

   ```css
   #container::after {
     display: block;
     content: "";
     clear: both;
   }
   ```

3. 底部添加空元素并清除浮动

   ```html
   <div id="container">
     <div id="sub"></div>
     <div id="clear"></div>
   </div>
   ```

   ```css
   #clear {
     clear: both;
   }
   ```

4. 父元素同样浮动，脱离文档流

   ```css
   // 会产生新的浮动问题，一般不采用
   #container {
     float: right;
   }
   ```

##### 5. CSS选择器及其优先级？

|     选择器     |                             格式                             | 优先级权重 |
| :------------: | :----------------------------------------------------------: | :--------: |
|    ID选择器    |                             #id                              |    100     |
|    类选择器    |                          .classname                          |     10     |
|   属性选择器   |                          标签[属性]                          |     10     |
|   伪类选择器   |  selector:pseudo-class，如用户行为伪类：`:hover`、`:focus`   |     10     |
|   标签选择器   |                           标签 {}                            |     1      |
|  伪元素选择器  | selector::pseudo-element，如`::before`、`::after`、`::first-line` |     1      |
| 相邻兄弟选择器 |     former_element + target_element { style properties }     |     0      |
|    子选择器    |                 元素 1 > 元素 2 { 样式声明 }                 |     0      |
|   后代选择器   |                    selector1 selector2 {}                    |     0      |
|  通配符选择器  |                              *                               |     0      |

**优先级总结：**

* `!important`声明的样式优先级最高
* 内联样式：1000
* id选择器：100
* 类选择器、伪类选择器、属性选择器：10
* 标签选择器、伪元素选择器：1

---

##### 6. 常见的伪类选择器有哪些？

> CSS伪类是添加到选择器的关键字，用于指定所选元素的特殊**状态**。

**常见的有：**

* :active
* :checked
* :disabled
* :empty
* :first
* :first-child
* :fullscreen
* :focus
* :hover
* :last-child
* :not()
* :nth-child()
* :nth-last-child()

---

##### 7. CSS3有哪些新特性？

* 新增各种CSS选择器；如伪类`not()`、`nth-child()`等等
* 圆角`border-radius`
* 多列布局（[Column layouts](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Layout_cookbook/Column_layouts)）
*  阴影和反射：`box-shadow`和`-webkit-box-reflect`
* 文字特效：`text-shadow`
* 文字渲染：`text-decoration`
* 线性渐变：`gradient`
* 变换：`transform`

---

##### 8. 说一下对CSS盒模型的理解？

CSS盒模型有两种：

* 标准盒模型
* IE盒模型

盒模型都是由四个部分组成的：margin、border、padding和content。

标准盒模型和IE盒模型的主要区别在于设置`width`和`height`时，所对应的范围不同：

* 标准盒模型的`width`和`height`属性只包含了content
* IE盒模型的`width`和`height`属性范围包含了border、padding和content

可以通过修改`box-sizing`属性来改变元素的盒模型：

* `box-sizing: content-box;`：默认值，标准盒模型
* `box-sizing: border-box;`：IE盒模型

---

##### 9. 如何判断元素是否到达可视区域？

以图片显示为例：

* `window.innerHeight`是浏览器的可视高度
* `document.body.scrollTop || document.documentElement.scrollTop`是浏览器滚动过的距离
* `element.offsetTop`是元素顶部距离文档顶部的高度，该高度包括滚动条的距离

因此，图片到达可视区域的条件是：

​	`img.offsetTop <  document.body.scrollTop + window.innerHeight`

---

##### 10. 常见的布局单位有哪些？并说一说各自的使用场景及区别

常用单位：`px`、`%`、`em/rem`和`vw/vh`

1. 像素：需要区分CSS像素和物理像素
2. 百分比：当浏览器的宽度或者高度发生变化时，通过百分比单位可以使得浏览器中的组件的宽和高随着浏览器的变化而变化，从而实现响应式的效果。一般认为子元素的百分比相对于直接父元素。
3. em/rem：相对于px更具有灵活性，它们都是相对长度单位，它们之间的区别是：em相对于父元素，rem相对于根元素。
   * em：文本相对长度单位。相当于当前对象内文本的字体尺寸。如果当前行内文本的字体尺寸未被人为设置，则相当于浏览器的默认字体尺寸（16px）——相对于父元素的字体大小倍数。
   * rem：rem是CSS3新增的一个相对单位，相对于根元素（html）的font-size倍数。一般会利用rem实现简单的响应式布局，可以利用html元素中字体的大小与屏幕间的比值来设置font-size的值，以此实现当屏幕分辨率变化时让元素也随之变化。
4. vw/vh：是视图窗口有关的单位。vw表示相对于视图窗口的宽度，vh表示相对于视图窗口高度。除了vw和vh外，还有vmin和vmax两个相关的单位。
   * vw：相对于视窗的宽度，视窗宽度是100vw；
   * vh：相对于视窗的高度，视窗高度是100vh；
   * vmin：vw和vh中的较小值
   * vmax：vw和vh中的较大值



---

##### 11. 什么是margin重叠？如何解决？

**重叠：**两个块级元素的上外边距和下外边距可能会合并为一个外边距，其中大小会取其中外边距最大的那个，这种行为就是外边距折叠，而折叠只会出现在垂直方向。值得注意的是，浮动的元素和绝对定位这种脱离文档流的元素的外边距不会折叠。

**重叠规则：**

* 如果两者都是正数，那么取最大者
* 如果一正一负，就取正值减去负值的绝对值
* 如果两个都是负数，用0减去两个中绝对值大的那个

**折叠主要发生场景：**

1. 同一层相邻元素之间
2. 没有内容将父元素后代元素分开
3. 空的块级元素

**解决办法：**

1. 兄弟之间重叠
   * 底部元素变为行内盒子：`display: inline-block`
   * 底部元素设置浮动：`float`
   * 底部元素设置定位：`position: absolute/fixed;`
2. 父子之间重叠
   * 父元素设置overflow：`overflow: hidden;`
   * 父元素设置透明border：`border: 1px solid transparent;`
   * 子元素变为行内盒子：`display: inline-block;`
   * 子元素设置浮动：`float`
   * 子元素设置定位：`position: absolute/fixed;`

---

##### 15. 对flex布局的理解及其使用场景？

[Flex 布局教程：语法篇](https://www.ruanyifeng.com/blog/2015/07/flex-grammar.html)

[Flex 布局教程：实例篇](https://www.ruanyifeng.com/blog/2015/07/flex-examples.html)

---
