---
title: 【手写】56个JavaScript知识点
tags:
  - Interview
  - JavaScript
cover: /images/cover_JavaScript基础.jpeg
categories:
  - Interview
abbrlink: ce365bbb
date: 2022-03-12 23:35:32
---

---

###### 1. 实现原生ajax封装？

<details><summary><b>Anwser</b></summary>
<p>
</p> 
</details>

---

###### 2. 实现new过程？

<details><summary><b>Anwser</b></summary>
<p>
```javascript
const arr = [1,2,3]
```

</p> 
</details>

---

###### 3. 打乱一个数组？

<details><summary><b>Anwser</b></summary>
<p>

</p> 
</details>

---

###### 4. 防抖函数？

<details><summary><b>Anwser</b></summary>
<p>


</p> 
</details>

---

###### 5. 节流函数？

<details><summary><b>Anwser</b></summary>
<p>


</p> 
</details>

---

###### 6. 数组去重？

<details><summary><b>Anwser</b></summary>
<p>


</p> 
</details>

---

###### 7. setTimeout实现setIterval？
[为何要用setTimeout实现setInterval？](https://juejin.cn/post/6994969893141479454)
<details><summary><b>Anwser</b></summary>
<p>


</p> 
</details>


---

###### 8. setInterval实现setTimeout？

<details><summary><b>Anwser</b></summary>
<p>


</p> 
</details>

---

###### 9. compose函数？


```javascript
// 形如 g(f(x))
function compose(...fns) {
    return function (...args) {
        return fns.reduceRight((accumulate, currentValue) => currentValue(accumulate), ...args)
    }
}
const user = {
    name: 'Dawson',
    age: 20
}


// 实现，获取用户前两位的大写名称
function getName(user) {
    return user.name;
}

function upperCase(name) {
    return name.toUpperCase();
}

function subName(name) {
    return name.substring(0, 2);
}

const haveName = compose(subName, upperCase, getName);

const res = haveName(user);

console.log(res);  // DA  对于pipe函数，仅是执行顺序不同而已，该用reduce即可
```
---

###### 10. curring函数？


```javascript
function Curring(fn, ...params) {
    const length = fn.length;
    return function (...args) {
        const p = [...params, ...args];
        // 参数未满足
        if (p.length < length) {
            return Curring(fn, ...p)
        }
        return fn.apply(this, p);
    }

}

function add(a, b, c) {
    return a + b + c;
}

const myAdd = Curring(add);

const res = myAdd(2)(3)(4);
console.log(res);
```


---

###### 11. LRU算法？

<details><summary><b>Anwser</b></summary>
<p>


</p> 
</details>

---

###### 12. 发布订阅模式？

<details><summary><b>Anwser</b></summary>
<p>


</p> 
</details>

---

###### 13. DOM转对象？

<details><summary><b>Anwser</b></summary>
<p>


</p> 
</details>

---

###### 14. 对象转DOM？

<details><summary><b>Anwser</b></summary>
<p>


</p> 
</details>

---

###### 15. 判断对象环引用？

<details><summary><b>Anwser</b></summary>
<p>


</p> 
</details>

---

###### 16. 计算对象的层数？

<details><summary><b>Anwser</b></summary>
<p>


</p> 
</details>

---

###### 17. 对象扁平化？

<details><summary><b>Anwser</b></summary>
<p>


</p> 
</details>

---

###### 18. (a == 1 && a == 2 & a == 3)为true  求a？

<details><summary><b>Anwser</b></summary>
<p>


</p> 
</details>

---

###### 19. Promise并发器？

<details><summary><b>Anwser</b></summary>
<p>


</p> 
</details>

---

###### 20. lazyMan函数？

<details><summary><b>Anwser</b></summary>
<p>


</p> 
</details>

---

###### 21. add函数

要求：实现一个add方法，使计算结果能够满足如下预期：

* add(1)(2)(3)() = 6
* add(1,2,3)(4)() = 10

<details><summary><b>Anwser</b></summary>
<p>


</p> 
</details>

---

###### 22. 深拷贝实现？

<details><summary><b>Anwser</b></summary>
<p>


</p> 
</details>

---

###### 23. 计算localStorage总容量？

<details><summary><b>Anwser</b></summary>
<p>


</p> 
</details>

---

###### 24. 实现async/await？

<details><summary><b>Anwser</b></summary>
<p>


</p> 
</details>

---

###### 25. 手写Promise，解释其原理？

<details><summary><b>Anwser</b></summary>
<p>

</p> 
</details>

---

>**数组方法实现**

###### 26. forEach实现？

<details><summary><b>Anwser</b></summary>
<p>

</p> 
</details>


>**Promise方法**

###### 46. all？

<details><summary><b>Anwser</b></summary>
<p>

</p> 
</details>

---

###### 47. race

<details><summary><b>Anwser</b></summary>
<p>


</p> 
</details>

---

###### 48. allSettled

<details><summary><b>Anwser</b></summary>
<p>


</p> 
</details>

---

###### 49. any

<details><summary><b>Anwser</b></summary>
<p>

</p> 
</details>

---

###### 50. finally

<details><summary><b>Anwser</b></summary>
<p>

</p> 
</details>

---

>**函数**

###### 51. call

<details><summary><b>Anwser</b></summary>
<p>


</p> 
</details>

---

###### 52. apply

<details><summary><b>Anwser</b></summary>
<p>


</p> 
</details>



---

###### 53. bind

<details><summary><b>Anwser</b></summary>
<p>


</p> 
</details>

---

>**字符串**

###### 54. slice

<details><summary><b>Anwser</b></summary>
<p>


</p> 
</details>

---

###### 55. substr

<details><summary><b>Anwser</b></summary>
<p>


</p> 
</details>

---

###### 56. substring

<details><summary><b>Anwser</b></summary>
<p>


</p> 
</details>
