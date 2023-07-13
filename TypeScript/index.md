##### 1. type和interface的区别

##### 2. enum常规枚举和常量枚举的区别

##### 3. void定义的变量类型

##### 4. 如何表示interface的子集？

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

##### 5. 介绍一下Typescript中的`Utility Types`



