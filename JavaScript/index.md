#### 对象相关

###### 2. 实现new过程？



---

###### 15. 判断对象环引用？



---



###### 22. 深拷贝实现？



---



###### 17. 对象扁平化？



---



###### 16. 计算对象的层数？



---



#### 数组相关

###### 3. 打乱一个数组？



---



###### 6. 数组去重？



---



###### 26. forEach实现？

**注意forEach的问题**



#### 函数相关

###### 51. call



---

###### 52. apply



---

###### 53. bind

<details><summary><b>Anwser</b></summary>
<p>



###### 4. 防抖函数？





---

###### 5. 节流函数？





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

###### 21. add函数

要求：实现一个add方法，使计算结果能够满足如下预期：

* add(1)(2)(3)() = 6
* add(1,2,3)(4)() = 10



###### 20. lazyMan函数？





---



###### 7. setTimeout实现setIterval？

[为何要用setTimeout实现setInterval？](https://juejin.cn/post/6994969893141479454)



---



### Promise相关

###### 25. 手写Promise，解释其原理？



---



###### 46. Promise.all()

---



###### 47. Promise.race()

---



###### 19. Promise并发器？

---



#### 其他

###### 1. 实现原生ajax封装？




---



###### 11. LRU算法？



---



###### 12. 发布订阅模式？



---



###### 23. 计算localStorage总容量？



---

