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

---

##### 4. css高度坍塌

**高度坍塌**：在文档流中，父元素的高度默认是由自元素的高度撑起来的，也就是子元素多高，父元素就多高。一旦子元素设置浮动脱离文档流，导致无法撑起父元素的高度，就会引起父元素高度塌陷的情况。例：

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

   
