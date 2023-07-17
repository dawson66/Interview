## 一、全局配置、全局API
##### 1. 描述一下Vue的基本原理

##### 2. 谈谈你对vue的理解？

官方：Vue是一套用于构建用户界面的**渐进式框架**，Vue的核心库只关注视图层。

![image-20230708100159205](https://raw.githubusercontent.com/dawson66/BlogImage/master/data/image-20230708100159205.png)

1. **声明式框架** 

   Vue的核心特点：用起来简单。那我们就有必要知道**命令式和声明式**的区别！

   * 早在JQ的时代编写的代码都是命令式的，命令式框架的重要特点就是关注过程。
   * 声明式框架更加关注结果。命令式的代码封装到了Vuejs中，过程靠Vuejs来实现。

   **即：声明式代码更加简单，不需要关注实现，按照要求填写代码就可以（给上原材料就出结果）**

   ```javascript
   // 命令式编程
   let numbers = [1, 2, 3, 4, 5];
   let total = 0;
   for(let i = 0; i < numbers.length; i++) {
     total += numbers[i];
   }
   console.log(total);
   
   // 声明式编程
   let total2 = numbers.reduce((memo, current)=> memo + current, 0);
   console.log(total2);
   ```

2. **MVVM模式**

   说起MVVM，就要知道另一个模式-MVC，为什么要有这些模式呢？

   目的：职业划分，分层管理

   ![image-20230708105541795](https://raw.githubusercontent.com/dawson66/BlogImage/master/data/image-20230708105541795.png)

   对于前端而言，就是如何将数据同步到页面上，也是借鉴后端思想。

   * 前端MVC模式：Backbone + underscore + jquery

     > 对于前端而言，数据变化无法同步到视图中。需要将逻辑聚拢在controller层中。

   * MVVM模式：映射关系的简化（隐藏controller）

     ![image.png](https://raw.githubusercontent.com/dawson66/BlogImage/master/data/9cf1cd566c8e40c99d781e10e4b69d9f~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp)

     * [MVVM和MVC分别是什么?有什么区别?使用场景是什么?](https://www.itheima.com/news/20211015/165923.html)
     * [MVC，MVP 和 MVVM 的图示](https://www.ruanyifeng.com/blog/2015/02/mvcmvp_mvvm.html)

     > 虽然没有完全遵循MVVM模型，但是Vue的设计也受到了它的启发。因此在文档中经常会使用`vm`这个变量名表示Vue实例。

3. **采用虚拟DOM**

   传统更新页面，拼接一个完整的字符串innerHTML全部重新渲染，添加虚拟DOM后，可以比较新旧虚拟dom节点，找到变化在进行更新。虚拟DOM就是一个对象，用来描述真实的DOM：[vnode](https://github.com/vuejs/vue/blob/main/src/core/vdom/vnode.ts)

   虚拟dom两个作用：

   * 减少操作dom的频率
   * 跨平台

4. **区分编译时（打包）和运行（浏览器）时**

   * Vue的渲染核心就是调用render方法，将虚拟DOM渲染成真实的DOM；缺点就是虚拟DOM的编写比较麻烦
   * 专门写个编译时可以将模版编译成虚拟DOM（在构建时候进行编译性能更高，不需要在运行的时候进行编译）

5. **组件化**

   实现高内聚、低耦合、单向数据流

   * 组件化开发能大幅度提高应用开发效率、测试性、复用性等
   * 降低更新范围，只重新渲染变化的组件

---



##### 3. 说一说前端路由？

##### 4. 谈谈你对SPA的理解？他们的优缺点分别是什么？

1. **概念**

   * SPA（single-page application）单页应用；默认情况下我们编写Vue、React都只有一个html页面，并且提供一个挂载点，最终打包后会再此页面中引入对应的资源。（页面的渲染全部由JS动态进行渲染）。切换页面时通过监听路由变化，渲染对应的页面。**Client Side Rendering，客户端渲染——CSR**。
   * MPA（multi-page application）多页应用，多个html页面。每个页面必须重复加载js、css等相关资源。（服务端返回完整的html，同时数据也可以再后端进行获取，并返回“模版引擎”）。多页应用跳转需要整页资源刷新。**Server Side Rendering，服务端渲染——SSR。**

2. **优缺点**

   |                 |   单页应用（SPA）    |        多页面应用（MPA）         |
   | :-------------: | :------------------: | :------------------------------: |
   |      组成       | 一个主页面和页面组件 |          多个完整的页面          |
   |    刷新方式     |       局部刷新       |             整页刷新             |
   | SEO搜索引擎优化 |       无法实现       |             容易实现             |
   |    页面切换     | 速度快，用户体验良好 | 切换加载资源，速度慢，用户体验差 |
   |    维护成本     |       相对容易       |             相对复杂             |

   总结：

   * SPA用户体验好、响应快，内容的改变不需要重新加载整个页面，服务端压力小。
   * SPA应用不利于搜索引擎的抓取。
   * 首次渲染速度相对较慢（第一次返回空的html，只有一个div，需要请求js将div内容填充），白屏时间长。

3. **解决方案**

   * 静态页面预渲染（Static Site Generation）SSG，在构建时生成完整的html页面。（就是在打包的时候，先将页面放到浏览器中运行一下，将html保存起来），仅适合静态页面网站——变化率不高的网站。
   * `SSR`+`CSR`的方式，首屏采用服务端渲染的方式，后续交互采用客户端渲染方式。（NuxtJS。。。）

---



##### 5. Vue为什么需要虚拟DOM？

1. **概念**

   基本上所有框架都引入了虚拟DOM来对真实DOM进行抽象，也就是现在大家所熟知的VNode和VDOM。

   * Virtual DOM就是用JS对象来描述真实的DOM，是对真实DOM的抽象；由于直接操作DOM性能低而JS层的操作效率高，因此可以将DOM操作转换为js对象操作。最终通过diff算法对比差异进行更新DOM（减少了对真实DOM的操作）。具体为什么直接操作DOM会很消耗性能请参考：[为什么说JS的DOM操作很耗性能](https://shangzhuanlan.zhihu.com/p/86153264)
   * 虚拟dom不依赖真实平台环境从而也可以实现跨平台。

2. VDOM是如何生成的？

   * 在Vue中我们常常会为组件编写模版——template
   * 这个模版会被编译器编译为渲染函数——render
   * 在接下来的挂载过程中会调用render函数，返回的对象就是虚拟DOM
   * 会在后续的patch过程中进一步转化为真实的DOM

3. **VDOM是如何做diff的？即diff算法**

   * 挂载过程结束后，会记录第一次生成的VDOM——oldVnode
   * 当响应式数据发生变化时，将会引起组件重新render，此时会生成新的VDOM——newVnode
   * 使用oldVnode与newVnode做diff操作，将更改的部分应到真实DOM上，从而转化为最小量的dom操作，高效更新视图。

---

##### 6. 谈一谈对Vue组件化的理解？

谈到组件化就会想到另一个词——模块化，这里先说一下组件化与模块化的区别：组件化具有对UI的封装，模块化是对业务逻辑的封装。他们的共同目的都是为了更好的复用从而实现高内聚。

> webComponent提到过组件化的核心组成有：模板、属性、事件、插槽、生命周期

组件化好处：高内聚、可重用、可组合

* 组件化开发能大幅提高应用开发效率、测试性、复用性等；
* 降低更新范围，只重新渲染变化的组件；



注意：Vue中有一个很重好的特性：组件级更新

* Vue中的每个组件都有一个渲染函数watcher、effect
* 数据是响应式的，数据变化后会执行watcher或者effect
* 组件要合理的划分，如果不拆分组件，那更新的时候整个页面都要重新更新。
* 如果过分的拆分组件会导致watcher、effect产生过多，也会造成性能浪费。

---

##### 7. 既然Vue通过数据劫持可以精准探测数据变化，为什么需要虚拟DOM进行diff检测差异？

Vue内部设计原因导致，vue设计每个组件对应一个watcher（渲染watcher），没有采用一个属性对应一个watcher，因为这样会导致大量watcher的产生而且浪费内存。如果粒度过低也无法精准检测变化，所以采用了 **diff算法 + 组件级watcher**。

##### 说一说全局管理状态？

##### MVVM、MVC、MVP的区别？

##### Vue单页应用与多页应用的区别？

##### Vue.nextTick的原理及作用？

##### 描述一下你对对React和Vue的理解，以及异同，Vue的优点有哪些

##### assets和static的区别？

##### delete和Vue.delete删除数组的区别？

##### extend有什么用？

##### MVVM的优缺点？

---
## 二、选项 
### 1. 数据
1. 说一下Vue的双向数据绑定原理

2. 使用Object.defineProperty()来进行数据劫持有什么缺点？

3. Computed和Watch的区别，computed和Methods的区别？

4. data为什么是一个函数而不是一个对象？

5. Vue中给data中的对象属性添加一个新的属性会发生什么？如何解决？

6. Vue中封装的数组方法有哪些？其是如何实现页面更新的？

7. Vue是如何收集依赖的？

8. Vue如何监听对象或者数组某个属性的变化？

### 2. DOM
1. Vue template到render的过程？

2. Vue data中某一个属性的值发生改变后，视图会立即同步执行重新渲染吗？

3. Vue模板编译原理？

4. template和JSX有什么区别？

### 3. 生命周期钩子
1. 说一下Vue的生命周期？

2. Vue子组件和父组件的执行顺序？

3. created和mounted的区别？

4. 一般在哪个生命周期请求异步数据？

5. keep-alive中的生命周期有哪些？

### 4. 资源
1. 过滤器的作用？如何实现一个过滤器？

2. 

### 5. 组合
1. 简述mixin、extends是什么及覆盖逻辑？

2. mixin和minxins有什么区别？

### 6. 其它

---
## 三、实例 
### 1. 属性

### 2. 方法
#### 2.1 数据

#### 2.2事件
1. 常见的事件修饰符有哪些？请说明其作用

   **事件修饰符：**

   * .stop
   * .prevent
   * .self
   * .capture
   * .once
   * .passive

   **按键修饰符：**

   * .enter
   * .tab
   * .delete
   * .esc
   * .space
   * .up
   * .down
   * .left
   * .right

   **系统按键修饰符：**

   * .ctrl
   * .alt
   * .shift
   * .meta

   **exact修饰符：**

   * .exact

   **鼠标按键修饰符：**

   * .left
   * .right
   * .middle

2. 子组件可以 直接更改父组件的数据吗？

3. 组件通信的方式有哪些？

#### 2.3生命周期


---
## 四、指令
1. v-if、v-show、v-html的原理？

2. v-if和v-show的区别？

3. v-model是如何实现的？该语法糖实际是什么？

4. v-model可以被用在自定义组件上吗？如果可以，如何使用呢？

5. 描述一下Vue自定义指令及使用场景或案例有哪些？

---
## 五、特殊属性


---
## 六、内置组件
##### 1. slot是什么？有什么作用？原理是什么？

##### 2. 对keep-alive的理解，它是如何实现的？具体缓存的是什么？

##### 3. 如何保存当前页面的状态？比如有一个list，我切换到第几页再回来，如何保持在第几页？或者页面进入详情后，退回来，如何保持之前的信息？

> 试想一下，如果左侧为list，右侧为list对应item的详情页，这种情况下不会出现题目说的问题。这种问题主要是发生了页面切换，即页面从A跳到了B，返回A的时候仍然要保持A之前的状态，那么这里的A即为List组件。这种情况，肯定是发生了路由跳转。正常情况下，想要让一个组件保持状态，只需要再外面用内置组件keep-alive包裹即可。但是这里面肯定发生了路由跳转，因为需要将router-view结合keep-alive来做。
>
> 在vue2.0中，router-view外面包裹一层keep-alive即可；但是在vue3.0中，router-view不能直接嵌在keep-alive中，需要结合v-slot使用；
>
> 除此之外，还需要考虑缓存组件的灵活性，即动态缓存，只对需要缓存的组件应用Keep-alive；如何解决呢？

###### 组件：

```vue
// List.vue
<template>
  <div class="container">
    <el-button @click="increment">{{ count }}</el-button>

    <div v-for="item in data.slice(currentPage * pageSize, (currentPage + 1) * pageSize)" :key="item.id">
      <span>{{ item.id }} —— </span>
      <router-link :to="{ name: 'info', params: { description: item.description } }">{{ item.name }}</router-link>
    </div>
    <el-pagination layout="prev, pager, next" :page-size="pageSize" :total="data.length"
      @current-change="paginationChange" />
  </div>
</template>

<script setup lang="ts">
import { ref, reactive, onBeforeMount, onMounted, onBeforeUnmount, onUnmounted } from 'vue'

const count = ref(0);

function increment() {
  count.value++;
}
  
// 简单的分页
const currentPage = ref(0);
const pageSize = 3;

function paginationChange(val: any) {
  console.log(typeof val);
  currentPage.value = val - 1;
}

// mock data
const data = reactive([
  {
    id: 0,
    name: 'foo',
    description: '你好啊，foo'
  },
  {
    id: 1,
    name: 'bar',
    description: '你好啊，bar'
  },
  {
    id: 2,
    name: 'dawson',
    description: '你好啊，dawson'
  },
  {
    id: 3,
    name: 'jason',
    description: '你好啊，jason'
  },
  {
    id: 4,
    name: 'ruiqiu',
    description: '你好啊，ruiqiu'
  },
  {
    id: 5,
    name: 'luo',
    description: '你好啊，luo'
  },
])

</script>

<script lang="ts">
// 注意：这里采用keep-alive inclue 包含具名组件来条件应用keep-alive
export default {
  name: 'layout'
}
</script>
```

###### 路由

```vue
// 外层包裹 router-view 组件
<template>
  <router-view v-slot="{ Component }">
    <keep-alive :include="['layout']">
      <component :is="Component"></component>
    </keep-alive>
  </router-view>
</template>
```

###### 总结

**综上所述：keep-alive结合router-view实现特定组件状态缓存，结合Keep-alive inclue指定具体哪些组件被缓存。但是这种方式不太好，组件name 被固定在条件上，是否可以借助路由meta来指定需要缓存的组件？**

---

##### 4. 有没有封装过组件？原则、方法是什么？

---
## 七、VNode

---
## 八、服务端渲染
1. 对SSR的理解？


## 九、Vue性能优化
1. VUe的性能优化有哪些？



2. Vue初始化页面闪动问题，你如何解决？
