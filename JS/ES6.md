# ES6

## 1. let、const、var的区别

| **区别**           | **var** | **let** | **const** |
| ------------------ | ------- | ------- | --------- |
| 是否有块级作用域   | ×       | ✔️       | ✔️         |
| 是否存在变量提升   | ✔️       | ×       | ×         |
| 是否添加全局属性   | ✔️       | ×       | ×         |
| 能否重复声明变量   | ✔️       | ×       | ×         |
| 是否存在暂时性死区 | ×       | ✔️       | ✔️         |
| 是否必须设置初始值 | ×       | ×       | ✔️         |
| 能否改变指针指向   | ✔️       | ✔️       | ×         |



## 2. 箭头函数与普通函数的区别

1. 箭头函数比普通函数更加简洁

   - 如果没有参数，就直接写一个空括号即可

   - 如果只有一个参数，可以省去参数的括号

   - 如果有多个参数，用逗号分割

   - 如果函数体的返回值只有一句，可以省略大括号

   - 如果函数体不需要返回值，且只有一句话，可以给这个语句前面加一个void关键字。最常见的就是调用一个函数：
     - let fn = () => void doesNotReturn();

2. 箭头函数没有自己的this：箭头函数不会创建自己的this， 所以它没有自己的this，它只会在自己作用域的上一层继承this，它所谓的this是捕获其所在上下⽂的 this 值，作为⾃⼰的 this 值。所以箭头函数中this的指向在它**在定义时**已经确定了，之后不会改变。

3. 箭头函数继承来的this指向永远不会改变，并且call()、apply()、bind()等方法也不能改变箭头函数中this的指向

    ```javascript
    var id = 'GLOBAL';
    var obj = {
      id: 'OBJ',
      a: function(){
        console.log(this.id);
      },
      b: () => {
        console.log(this.id);
      }
    };
    obj.a();    // 'OBJ'
    obj.b();    // 'GLOBAL'
    new obj.a()  // undefined
    new obj.b()  // Uncaught TypeError: obj.b is not a constructor
    //对象obj的方法b是使用箭头函数定义的，这个函数中的this就永远指向它定义时所处的全局执行环境中的this，即便这个函数是作为对象obj的方法调用，this依旧指向Window对象。需要注意，定义对象的大括号{}是无法形成一个单独的执行环境的，它依旧是处于全局执行环境中。
    
    let fun1 = () => {
        console.log(this.id)
    };
    fun1();                     // 'Global'
    fun1.call({id: 'Obj'});     // 'Global'
    fun1.apply({id: 'Obj'});    // 'Global'
    fun1.bind({id: 'Obj'})();   // 'Global'
    ```

  4. 箭头函数不能作为构造函数使用：构造函数在new的步骤在上面已经说过了，实际上第二步就是将函数中的this指向该对象。 但是由于箭头函数时没有自己的this的，且this指向外层的执行环境，且不能改变指向，所以不能当做构造函数使用。

  5. 箭头函数没有自己的arguments：箭头函数没有自己的arguments对象。在箭头函数中访问arguments实际上获得的是它外层函数的arguments值。

  6. 箭头函数没有prototype

  7. 箭头函数不能用作Generator函数，不能使用yeild关键字



##3. 扩展运算符的作用及使用场景

- 对象扩展运算符、数组扩展运算符
- 解构赋值
- 使用场景：浅拷贝复制、将数组转换为参数序列来传参、rest 收集参数、合并、将 字符串 或 arguments 或 Iterator 接口的对象转为真正的数组



## 4. Proxy、Reflect

Proxy：对象用于创建一个对象的代理，从而实现基本操作的拦截和自定义（如属性查找、赋值、枚举、函数调用等）,不兼容IE

Reflect：是一个内置的对象，它提供拦截 JavaScript 操作的方法。这些方法与proxy handlers的方法相同。Reflect不是一个函数对象，因此它是不可构造的,不兼容IE。与大多数全局对象不同Reflect并非一个构造函数，所以不能通过new运算符对其进行调用，或者将Reflect对象作为一个函数来调用。Reflect的所有属性和方法都是静态的，这些方法与proxy handler的命名相同，其中的一些方法与 Object相同。

```js
/*
Proxy：
new Proxy(sourceObj,handler）

handler.apply//函数调用劫持。
handler.construct//new 操作符劫持
handler.defineProperty//Object.defineProperty调用劫持。
handler.deleteProperty//delete 操作符劫持。
handler.get//获取属性值劫持。
handler.getOwnPropertyDescriptor//Object.getOwnPropertyDescriptor 调用劫持。
handler.getPrototypeOf//Object.getPrototypeOf调用劫持。
handler.has// 操作符劫持。
handler.isExtensible//Object.isExtensible调用劫持。
handler.ownKeys//Object.getOwnPropertyNames 和Object.getOwnPropertySymbols调用劫持。
handler.preventExtensions//Object.preventExtensions调用劫持。
handler.set//设置属性值劫持。
handler.setPrototypeOf//Object.setPrototypeOf调用劫持。


Reflect：
Reflect.apply(target, thisArgument, argumentsList) //对一个函数进行调用操作，同时可以传入一个数组作为调用参数。和 Function.prototype.apply() 功能类似。
Reflect.construct(target, argumentsList[, newTarget]) //对构造函数进行 new 操作，相当于执行 new target(...args)。
Reflect.defineProperty(target, propertyKey, attributes) //和 Object.defineProperty() 类似。如果设置成功就会返回 true
Reflect.deleteProperty(target, propertyKey) //作为函数的delete操作符，相当于执行 delete target[name]。
Reflect.get(target, propertyKey[, receiver]) //获取对象身上某个属性的值，类似于 target[name]。
Reflect.getOwnPropertyDescriptor(target, propertyKey) //类似于 Object.getOwnPropertyDescriptor()。如果对象中存在该属性，则返回对应的属性描述符,  否则返回 undefined.
Reflect.getPrototypeOf(target) //类似于 Object.getPrototypeOf()。
Reflect.has(target, propertyKey) //判断一个对象是否存在某个属性，和 in 运算符 的功能完全相同。
Reflect.isExtensible(target) //类似于 Object.isExtensible().
Reflect.ownKeys(target) //返回一个包含所有自身属性（不包含继承属性）的数组。(类似于 Object.keys(), 但不会受enumerable影响).
Reflect.preventExtensions(target) //类似于 Object.preventExtensions()。返回一个Boolean。
Reflect.set(target, propertyKey, value[, receiver]) //将值分配给属性的函数。返回一个Boolean，如果更新成功，则返回true。
Reflect.setPrototypeOf(target, prototype) //设置对象原型的函数. 返回一个 Boolean， 如果更新成功，则返回true。

//target：目标对象
//prototype：属性名
//receiver：this

*/
```

Vue2中使用Object.defineProperty()对属性的各个setter和getter进行劫持：

```js
let data={}
Object.defineProperty(data,'name',{
	value:'初始名字',
	set(newValue){
		this._name = newValue
	},
	getter(){
		return this._name
	}
})
```

Vue3中使用Proxy、Reflect对属性的各个setter和getter进行劫持：

```js
let onWatch = (obj, setBind, getLogger) => {
  let handler = {
    get(target, property, receiver) {
      getLogger(target, property)
      return Reflect.get(target, property, receiver)
    },
    set(target, property, value, receiver) {
      setBind(value, property)
      return Reflect.set(target, property, value)
    }
  }
  return new Proxy(obj, handler)
}
let obj = { a: 1 }
let p = onWatch(
  obj,
  (v, property) => {
    console.log(`监听到属性${property}改变为${v}`)
  },
  (target, property) => {
    console.log(`'${property}' = ${target[property]}`)
  }
)
p.a = 2 // 监听到属性a改变
p.a // 'a' = 2
```



## 5. 字符串模板、新方法

新增字符串模板、新增字符串新方法

```
/*
*字符串新方法：
* 1.判断开头String.startsWith(str)
* 2.判断结尾String.endsWith(str)
* 3.头部补全String.padStart(str)
* 4.尾部补全String.padEnd(str)
* 5.判断是否包含指定字符串String.includes(str)
* 6.自动重复,被连续复制多次：'str'.repeat(3)
* A.String.trim() 删除字符串两端的空白字符。 （ES5）
* B.charAt()  返回字符串中指定索引（位置）的字符。（也可以用：str[0] 对字符串进行属性访问;） （ES5）
*
* 字符串模板：字符串连接
* 1.用反单引号表示字符串模板(``)
* 2.用${}表示变量
* */
```



## 6.Map、Set对象

### map和Object的区别

|          | Map                                                          | Object                                                       |
| -------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 意外的键 | Map默认情况不包含任何键，只包含显式插入的键。                | Object 有一个原型,  原型链上的键名有可能和自己在对象上的设置的键名产生冲突。 |
| 键的类型 | Map的键可以是任意值，包括函数、对象或任意基本类型。          | Object 的键必须是 String 或是Symbol。                        |
| 键的顺序 | Map 中的 key 是有序的。因此，当迭代的时候， Map  对象以插入的顺序返回键值。 | Object 的键是无序的                                          |
| Size     | Map 的键值对个数可以轻易地通过size 属性获取                  | Object 的键值对个数只能手动计算                              |
| 迭代     | Map 是 iterable 的，所以可以直接被迭代。                     | 迭代Object需要以某种方式获取它的键然后才能迭代。             |
| 性能     | 在频繁增删键值对的场景下表现更好。                           | 在频繁添加和删除键值对的场景下未作出优化。                   |



### map和weakMap的区别

1. WeakMap只接受对象作为key，如果设置其他类型的数据作为key，会报错。
2. WeakMap的key所引用的对象都是弱引用，只要对象的其他引用被删除，垃圾回收机制就会释放该对象占用的内存，从而避免内存泄漏。即垃圾回收机制不将该引用考虑在内。因此，只要所引用的对象的其他引用都被清除，垃圾回收机制就会释放该对象所占用的内存。也就是说，一旦不再需要，WeakMap 里面的键名对象和所对应的键值对会自动消失，不用手动删除引用。
3. 由于WeakMap的成员随时可能被垃圾回收机制回收，成员的数量不稳定，所以没有size属性。
4. 没有clear()方法
5. 不能遍历



## 7.Symbol

ES6中新增的原始数据类型

**特点**：

1. Symbol属性对应的值是**唯一**的,解决命名冲突问题

2. Symbol 值与其他数据**不能进行计算**,包括同字符串拼串

3. for in, for of遍历时**不会遍历**symbol属性。

```js
//使用：
//1、调用Symbol函数得到Symbol值
let symbol = Symbol();
let obj = {};
obj[symbol] = 'hello'

//2、传参标识
let symbol = Symbol('one');
let symbol2 = Symbol('two');
console.log(symbol);
console.log(symbol2);
```

**内置Symbol值**：除了定义自己使用的symbol值以外,ES6还提供了11个内置的Symbol值,指向语言内部使用的方法

**Symbol.iterator**：对象的Symbol.iterator属性,指向该对象的默认遍历器方法



## 8.迭代

### Iterator

Iterator是一种接口机制，为各种不同的数据结构提供统一的访问机制

**作用：**

- 为各种数据结构，提供一个统一的、 简便的访问接口;

- 使得数据结构的成员能够按某种次序排列

- ES6创造了一种新的遍历命令for...of循环,Iterator接口，主要供for...of消费，当数据结构上部署了Symbol.iterator接口,该数据就是可以用for of遍历，当使用for of去遍历目标数据的时候,该数据会自动去找Symbol.iterator属性（Symbol.iterator属性指向对象的默认遍历器方法）

**工作原理：**

- 创建一个指针对象(遍历器对象)，指向数据结构的起始位置。
- 第一次调用**next方法**,指针自动指向数据结构的第一个成员
- 接下来不断调用next方法,指针会一直往后移动，直到指向最后一 个成员
- 每调用next方法返回的是一 个包含value和done的对象，{value: 当前成员的值,done: 布尔值}
- value 表示当前成员的值，done 对应的布尔值表示当前的数据的结构是否遍历结束。
- 当遍历结束的时候返回的value值是undefined, done值为false
- 原生具备Iterator接口的数据(可使用for of遍历)：
  1. Array
  2. arguments
  3. set容器
  4. map容器
  5. String



### for...in和for...of的区别：

for…of 是ES6新增的遍历方式，允许遍历一个含有iterator接口的数据结构（数组、对象等）并且返回各项的值，和ES3中的for…in的区别如下

- for…of 遍历获取的是对象的键值，for…in 获取的是对象的键名；
- for… in 会遍历对象的**整个原型链**，性能非常差不推荐使用，而 for … of 只遍历当前对象不会遍历原型链；
- 对于数组的遍历，for…in 会返回数组中所有**可枚举**的属性(包括原型链上可枚举的属性)，for…of 只返回数组的下标对应的属性值；

**总结：** for...in 循环主要是为了遍历对象而生，不适用于遍历数组；for...of 循环可以用来遍历数组、类数组对象，字符串、Set、Map 以及 Generator 对象。



### forEach和map方法有什么区别

这方法都是用来遍历数组的，两者区别如下：

- forEach()方法会针对每一个元素执行提供的函数，对数据的操作会**改变原数组**，该方法**没有返回值**；
- map()方法**不会改变原数组**的值，**返回一个新数组**，新数组中的值为原数组调用函数处理之后的值；



### 如何使用for...of遍历对象：

就给对象添加一个[Symbol.iterator]属性，并指向一个迭代器即可

```js
//方法一：
var obj = {
    a:1,
    b:2,
    c:3
};

obj[Symbol.iterator] = function(){
	var keys = Object.keys(this);
	var count = 0;
	return {
		next(){
			if(count<keys.length){
				return {value: obj[keys[count++]],done:false};
			}else{
				return {value:undefined,done:true};
			}
		}
	}
};

for(var k of obj){
	console.log(k);
}


// 方法二：使用Generator
var obj = {
    a:1,
    b:2,
    c:3
};
obj[Symbol.iterator] = function* (){
    var keys = Object.keys(obj);
    for(var k of keys){
        yield [k,obj[k]]
    }
};

for(var [k,v] of obj){
    console.log(k,v);
}
```



## 9.异步编程：Promise、generator、Async - await



## 10.Class类



## 11.模块化：ES6模块和CommonJS模块

ES6 Module和CommonJS模块的区别：

- CommonJS是对模块的浅拷⻉，ES6 Module是对模块的引⽤，即ES6 Module只存只读，不能改变其值，也就是指针指向不能变，类似const；
- import的接⼝是read-only（只读状态），不能修改其变量值。 即不能修改其变量的指针指向，但可以改变变量内部指针指向，可以对commonJS对重新赋值（改变指针指向），但是对ES6 Module赋值会编译报错。

ES6 Module和CommonJS模块的共同点：

- CommonJS和ES6 Module都可以对引⼊的对象进⾏赋值，即对对象内部属性的值进⾏改变。

























