### 概述

初衷：就是想探索Vue从头到尾过程中的各种原理，如何实现的，思路是什么，运用了哪些知识，比如借鉴了什么库？新的名词及含义等等。下面是一张例子图片，其中涉及到几个知识点，如模板语法、编译原理，编译过程中如何收集依赖的，转换成什么了，ast，render，h函数，virtual dom 算法原理等等。想到啥就写啥了，后续慢慢补充吧！

![image-20220927215643660](https://raw.githubusercontent.com/dawson66/BlogImage/master/data/image-20220927215643660.png)

### Vue数据绑定原理



---

### Vue响应式原理



---

### 模板编译原理

[AST编译render原理](https://tehub.com/a/8xsejSkmuw)



---

### Vue虚拟dom及Diff算法

#### 1. 什么是虚拟dom

> Virtual Dom，是由**普通的JS对象来描述的DOM对象**；因为不是真实的DOM对象，所以叫Virtual DOM。

如：

```javascript
{
  tag: 'div',
  attrs: {
    id: 'app'
  },
  key: undefined,
  text: undefined,
  children: [
    {
      tag: 'p',
      attrs: undefined,
      key: undefined,
      text: undefined,
      children: [
				...
      ]
    }
  ]
}
```

#### 2. 选择虚拟dom的原因

1. 保证性能下限

首先真实DOM上包含了太多无用的属性，使用虚拟dom可以大大减少不必要的属性存在，可以很好的释放内存的压力。

其次，操作真实的dom是非常消耗性能的一件事。由于JS是单线程的，当我们去操作dom的时候，无论是获取还是修改一些属性，都可能会引起浏览器的render；若是频繁或者不恰当的操作dom则会引起严重的性能问题，因此直接操作dom的代价是非常大的。采用虚拟dom可以将全部的修改一次性的更新到页面上，不用修改一次就可能导致一次render，相当于将多次渲染汇合成一次渲染，类似dom操作，可以使用`documentFragment`一样。

2. 提高开发体验

想想原来在JQuery时代前端被DOM支配的恐惧，不用操作DOM是多么幸福的事情，Vue释放了我们的双手，甚至不必纠结于DOM操作的一些API，区而用数据驱动视图的方式代替了。

3. 跨端

由于虚拟dom的存在，Vue已经可以适用于PC、移动端（小程序，weex）等领域。

---

### Vue抽象语法树AST

#### 1. 什么是抽象语法树

> 抽象语法树（Abstract Syntax Tree），简称AST，它是源代码语法结构的一种抽象表示。它以树状的形式表现编程语言的语法结构，树上的每个节点都表示源代码中的一种结构。

#### 2. 抽象语法树与虚拟节点有什么区别？

例如：一段html代码

```html
<div id="app">
  <p>
      {{name}}
  </p>
</div><
```

Vue中生成对应的ast

```javascript
{
  "type": 1,
  "tag": "div",
  "attrsList": [
    {
      "name": "id",
      "value": "app",
      "start": 5,
      "end": 13
    }
  ],
  "attrsMap": {
    "id": "app"
  },
  "rawAttrsMap": {
    "id": {
      "name": "id",
      "value": "app",
      "start": 5,
      "end": 13
    }
  },
  "children": [
    {
      "type": 1,
      "tag": "p",
      "attrsList": [],
      "attrsMap": {},
      "rawAttrsMap": {},
      "parent": "[Circular ~]",
      "children": [
        {
          "type": 2,
          "expression": "_s(name)",
          "tokens": [
            {
              "@binding": "name"
            }
          ],
          "text": "{{name}}",
          "start": 22,
          "end": 30,
          "static": false
        }
      ],
      "start": 19,
      "end": 34,
      "plain": true,
      "static": false,
      "staticRoot": false
    }
  ],
  "start": 0,
  "end": 41,
  "plain": false,
  "attrs": [
    {
      "name": "id",
      "value": "\"app\"",
      "start": 5,
      "end": 13
    }
  ],
  "static": false,
  "staticRoot": false
}
```







