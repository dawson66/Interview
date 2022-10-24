# Babel

一些babel学习的知识点总结

## 手写一个babel

场景：dev环境下执行一段代码，生产环境下不执行，打包的代码也不产生如

```javascript
// DEBUG即为dev环境标识，若为false则该处代码也不出现在dist中
if(DEBUG){
  const a = 1;
  const b = 2;
  console.log(a+b);
}
```

分析：

1. 如何区分dev和prod环境？
2. 如何识别DEBUG？
3. 如何在ast中删除该if语句？

