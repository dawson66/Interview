# 一、Webpack面试题汇总

##### 1. 有哪些常见的Loader？你用过哪些？



##### 2. 有哪些常见的Plugin？你用过哪些？



##### 3. Loader和Plugin的区别？



##### 4. Webpack的构建流程是怎样的？



##### 5. 使用webpack时，你用过哪些可以提高效率的插件？



##### 6. source map是什么？生产环境怎么用？

定义：source map是将编译、打包、压缩后的代码映射回源代码的过程。打包压缩后的代码不具备良好的可读性，想要调试源码就需要source map。

map文件只要不打开开发者工具，浏览器是不会加载的。

线上环境一般有三种处理方案：

1. `hidden-source-map`：借助第三方错误监控平台Sentry使用。
2. `nosources-source-map`：只会显示具体行数以及源代码的错误栈。安全性比source map高。
3. `source map`：通过nginx设置将.map文件只对白名单开放（公司内网）。

注意：避免在生产中使用`inline-`和`eval-`，因为它们会增加bundle体积大小，并降低整体性能。

---

##### 7. 模块打包原理知道吗？



##### 8. 文件监听原理？



##### 9. Webpack的热更新原理（HMR）？





##### 10. 如何对bundle体积进行监控和分析？



##### 11. 如何优化Webpack的构建速度？

这个问题就像能不能说一说从url输入到页面显示发生了什么一样！！！

首先需要分析构建速度的快慢，分析打包问价你的大小，需要用到一些可视化插件，如`webpack-bundle-analyzer`

* 使用高版本的Webpack和Node.js
* **多进程/多实例**构建：HappyPack（不维护了）、thread-loader
* **压缩代码**
  * 多进程并行压缩
    * webpack-paralle-uglify-plugin
    * uglifyjs-webpack-plugin 开启parallel参数（不支持ES6）
    * terser-webpack-plugin 开启parallel参数
  * 通过mini-css-extract-plugin提取chunk中的css代码到单独的文件，通过css-loader的minimize选项开启cssnano压缩CSS。
* 图片压缩
  * 使用基于Node库的imagemin（很多定制选项、可以处理多种图片格式）
  * 配置image-webpack-loader
* 缩小打包作用域
* 提取页面公共资源
* DLL
* 充分利用缓存提升二次构建速度
* Tree shaking
* Scope hoisting
* 动态polyfill

参考：[构建性能](https://www.webpackjs.com/guides/build-performance/)

---

##### 12. 代码分割的本质是什么？有什么意义？



##### 13. 写过Loader没有？说一下编写Loader的思路？



##### 14. 写过Plugin没有？说一下编写Plugin的思路？



##### 15. 聊一聊Babel的原理吧？





参考：[掘金-再来一打Webpack面试题](https://juejin.cn/post/6844904094281236487)

---

##### 16. 你借助webpack优化过前端性能么？用了哪些方式，优化了哪些方面？



---

# Vite面试题汇总

1. 