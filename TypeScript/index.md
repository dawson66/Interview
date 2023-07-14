# TypeScript面试题汇总

##### 1. 说一说你对TypeScript的理解？与JavaScript有什么区别？

##### 2. 说一说TypeScript的数据类型？

##### 3. 说一说对TypeScript中枚举的理解？

##### 4. 说一说对TypeScript中函数的理解？

##### 5. 说一说对TypeScript中接口的理解？与`Type`有什么区别？如何表示interface的子集？



###### interface的子集表示：

```typescript
interface UserInfo {
  name: string,
  age: number,
  address: string,
}

// 现在有一个方法，参数为UserInfo的子集
function getUserInfo(user: Partial<UserInfo>) {
  return user.age;
}
```

---

##### 6. 说一说对TypeScript中类的理解？

##### 7. 说一说你对TypeScript中泛型的理解？

> 泛型（Generics）是指在定义函数、接口或类的时候，不预先指定具体的类型，而在使用的时候再指定类型的一种特性。

##### 4. 说一说对TypeScript中命名空间及模块的理解？

##### 4. type和interface的区别？解释一下类型别名和接口

##### 5. 说一说你对TypeScript枚举类型的理解以及应用场景？enum常规枚举和常量枚举的区别？

##### 6. void定义的变量类型

##### 7. 如何表示interface的子集？

##### 8. 介绍一下Typescript中的`Utility Types`？

##### 



