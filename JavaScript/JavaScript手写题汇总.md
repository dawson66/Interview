<div align="center">
  <img height="60" src="https://img.icons8.com/color/344/javascript.png">
  <h1>JavaScript Coding</h1>
</div>

##### 数据类型



##### 原型

###### 继承

**原型链继承**

```javascript
function Person() {
  this.name = 'dawson'
}

Person.prototype.getName = function () {
  return this.name;
}

function Student() {}

// 原型链继承：子类构造函数指向父类实例
Student.prototype = new Person();

const student = new Student();

console.log(student.getName());  // dawson

// 引发的问题？
```

问题1：子类指向的是一个固定的实例对象，即所有Student实例都共享这个Person实例，它们操作的是同一个引用对象。

```javascript
function Person() {
  this.name = 'dawson';
  this.hobbies = ['backetball', 'football'];
}

Person.prototype.getName = function () {
  return this.name;
}

Person.prototype.addHobbies = function (...args){
  this.hobbies.push(...args);
}

function Student() {}

Student.prototype = new Person();

const student1 = new Student();
const student2 = new Student();
student1.addHobbies('music');
student2.addHobbies('dance');
console.log(student1.hobbies);  // ['backetball', 'football', 'music', 'dance']
```

问题2：子类创建时，不能向父类传参，因为子类实例化时，父类已经实例化好了。

```javascript
// 细想一下，子类实例化时，通用性属性一般super调用父类的构造函数来为子类初始化，然而，子类构造函数原型指向一个固定的对象，怎么也做不到传值的操作！
```

**构造函数继承**

```javascript
function Person(options) {
  this.age = options.age;
  this.name = 'dawson';
  this.hobbies = ['backetball', 'football'];
}

Person.prototype.getName = function () {
  return this.name;
}

Person.prototype.addHobbies = function (...args){
  this.hobbies.push(...args);
}

function Student(options) {
  // 所谓的构造函数继承就是在子类的构造函数中调用父类的构造函数并指定this
  Person.call(this, options)
}


const student1 = new Student({age: 20});
const student2 = new Student({age: 88});
console.log(student1.age);  // 20
console.log(student2.age);  // 88
student1.addHobbies('music');
student2.addHobbies('dance');
console.log(student1.hobbies); // Uncaught TypeError: student1.addHobbies is not a function

// 解决了什么问题？
// 1. 解决了原型链继承的传参问题

// 引发了什么问题？
// addHobbies报错说明原型上没有此方法，即子类未继承父类原型链上的属性和方法；那么你可能会说，可以将方法挂载到属性上而不是原型上，没错，这样是可以的
// 但是会引发一个问题，就是子类每次实例化时都会重复创建相同的方法，如addHobbies，这样就丧失了原型链或者面向对象的意义，因此引出下面思考

// 思考：未继承原型链上的属性，那么我们怎么才能继承父类原型链上的属性呢？
```

**组合继承**

```javascript
function Person(options) {
  this.age = options?.age || 20;
  this.name = 'dawson';
  this.hobbies = ['backetball', 'football'];
}

Person.prototype.getName = function () {
  return this.name;
}

Person.prototype.addHobbies = function (...args){
  this.hobbies.push(...args);
}

function Student(options) {
  Person.call(this, options);
  this.gender = options.gender;
}

// 借用原型继承的优点，子类原型指向父类实例，虽不能传参，但是能够继承原型链上的属性
Student.prototype = new Person();
// 需要修复构造函数指向，Student实例constructor属性需要指向其构造函数
// student1.constructor == Student   Student.prototype.constructor == Student
Student.prototype.constructor = Student;

const student1 = new Student({age: 20, gender: 'male'});
const student2 = new Student({age: 88, gender: 'female'});
console.log(student1);
console.log(student1.age);  // 20
console.log(student2.age);  // 88
student1.addHobbies('music');
student2.addHobbies('dance');
console.log(student1.hobbies); // ['backetball', 'football', 'music']
```

组合继承已经能够实现对象的继承问题，算是一种解决方案了，但是还不够完美。如果你将student实例打印并查看的话，你会发现，属性不仅在实例中有，		在原型对象上也有，因为我们的父类构造函数被调用了两次！那么如何减少一次构造函数的调用呢？

**寄生式组合继承**

```javascript
// 寄生式组合继承作为ES6之前的 继承的 较好实现
function Person(options) {
  this.age = options?.age || 20;
  this.name = 'dawson';
  this.hobbies = ['backetball', 'football'];
}

Person.prototype.getName = function () {
  return this.name;
}

Person.prototype.addHobbies = function (...args){
  this.hobbies.push(...args);
}

function Student(options) {
  Person.call(this, options);
  this.gender = options.gender;
}

function F(){

}

// 中间加一层，子类原型指向F实例，F实例原型之乡Person的原型，实现原型链的访问
F.prototype = Person.prototype;
// 这里试想一下，Student.prototype = Person.prototype 行不行？ 为何中间非要加一层？
Student.prototype = new F();
// 别忘了，constructor的正确指向
Student.prototype.constructor = Student;


const student1 = new Student({age: 850, gender: 'male'});
const student2 = new Student({age: 88, gender: 'female'});
console.log(student1);
console.log(student1.age);  // 20
console.log(student2.age);  // 88
student1.addHobbies('music');
student2.addHobbies('dance');
console.log(student1.hobbies); // ['backetball', 'football', 'music']
```

class继承

```javascript
class Person {
  constructor(options){
    this.age = options?.age || 20;
    this.name = 'dawson';
    this.hobbies = ['backetball', 'football'];
  }

  getName() {
    return this.name;
  }

  addHobbies(...args){
    this.hobbies.push(...args);
  }
}

class Student extends Person {
  constructor(options){
    super(options)
    this.gender = options.gender
  }
}

const student3 = new Student({age: 890, gender: 'male'})
console.log(student3);
```





##### 对象相关

###### 1. 实现new过程？

```javascript
function myNew(fn, ...args) {
  let obj = Object.create(fn.prototype);
  let res = fn.call(obj, ...args);
  if (res && typeof res === 'object') {
    return res;
  }
  return obj;
}

function Person(options) {
  this.name = options.name;
  this.age = options.age;
  this.getName = function () {
    return this.name;
  }
  // return {name: 'test', age: 18}
}

Person.prototype.isAdult = function () {
  return this.age < 20;
}

// ES5
const person1 = new Person({
  name: 'dawson',
  age: 20
})

// myNew
const person2 = myNew(Person, {
  name: 'dawson',
  age: 20
})

console.log(person1);
console.log(person2);
```

---



###### 2. 深拷贝实现？

```javascript
// 只考虑了array,null,object和循环引用， 思考如何实现其它引用数据类型？如set,map,function。。。
function deepClone(source, map = new WeakMap()) {
  if (typeof source === 'object') {
    if (map.has(source)) {
      return map.get(source);
    }
    let cloneObj = Array.isArray(source) ? [] : {}
    map.set(source, cloneObj);
    if (source) {
      for (const key in source) {
        cloneObj[key] = deepClone(source[key])
      }
      return cloneObj;
    } else {
      return souce;
    }
  } else {
    return source;
  }
}

const obj = {
  name: 'dawson',
  age: 20,
  married: false,
  hobbies: ['basketball', 'footaball', { name: 'swiming', time: 5 }],
  car: {
    price: 40000,
    brand: 'bwa'
  },
}

const copyObj = deepClone(obj);
// 验证
obj.hobbies[2].name = 'singing';
console.log(obj.hobbies[2].name);    // singing
console.log(copyObj.hobbies[2].name); // swiming
```

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

###### 51. bind

先来看一个例子

```javascript
// 例一
var name = 'global'

let obj = {
  name: 'dawson'
}

function getName() {
  return this.name;
}

const copyGetName = getName.bind(obj)
console.log(getName());      // global
console.log(copyGetName());  // dawson

// 例二
function add(a, b, c) {
  return a + b + c;
}
const copyAdd = add.bind(null, 1);
console.log(copyAdd(2, 3));   // 6
```

结论：

* 返回一个函数
* 绑定this
* 柯里化特性（偏函数）
* 构造函数特性

```javascript
// 实现上述三点，第四点是最难的，也请思考！
Function.prototype._bind = function (context) {
  if (typeof this !== 'function') {
    throw new Error('bind只能被函数调用')
  }
  const fn = this;
  const args = Array.prototype.slice.call(arguments, 1);
  return function () {
    const bindArgs = Array.prototype.slice.call(arguments);
    return fn.call(context, ...args, ...bindArgs);
  }
}
```



---

###### 52. call

顺着第一反应所想，看看你能写成啥样

```javascript
Function.prototype._call = function (thisArg, ...argsArray){
  if(typeof this !== 'function') {
    throw new Error('call必须为函数调用')
  }
  const self = this;
  const fn = self.bind(thisArg);
  return fn(...argsArray)
}
// 例
let name = 'smith';
var age = 20;

let obj = {
  name: 'dawson',
  age: 25
}

function getNameAndAge() {
  return `My name is ${this.name} and my age is ${this.age}`;
}        

console.log(getNameAndAge());           // My name is smith and my age is 20, from undefined
console.log(getNameAndAge._call(obj, '清华'));  // My name is dawson and my age is 25, from 清华
```

仔细想想特性以及要补充的吧！

---



###### 53. apply

apply与call基本相同，同样试着写一下

```javascript
// 参数为数组或类数组对象
Function.prototype._apply = function (thisArg, argsArray){
  if(typeof this !== 'function') {
    throw new Error('call必须为函数调用')
  }
  const self = this;
  const fn = self.bind(thisArg);
  const args = argsArray?Array.from(argsArray) : [];
  return fn(...args)
}

// 例
let name = 'smith';
var age = 20;

let obj = {
  name: 'dawson',
  age: 25
}

function getNameAndAge() {
  return `My name is ${this.name} and my age is ${this.age}`;
}       

console.log(getNameAndAge());           // My name is smith and my age is 20, from undefined
console.log(getNameAndAge._apply(obj, ['清华']));  // My name is dawson and my age is 25, from 清华
```

仔细想想特性以及要补充的吧！

---



###### 4. debounce



---



###### 5. throttle





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

