一.请说出下列最终的执行结果，并解释为什么？
```
var a = [];
for (var i = 0; i < 10; i++) {
	a[i] = function () {
		console.log(i);
	}
}

a[6]();

```

答案：10
因为for循环中的i使用的var声明，所以这里的i是一个全局变量，循环中a[i]是一个匿名方法，此匿名方法中控制台输出的i是全局作用域中的i,在循环执行完成过后i已经被累加到了10，所以无论执行a数组中的哪一个成员的方法，这里输出的答案都为10


二.请说出下列最终的执行结果，并解释为什么？
```
var tmp = 123;
if (true) {
	console.log(tmp);
	let tmp;
}
```
答案： 报错。'tmp is not defined'
使用let命令声明的变量会绑定到变量所在的块级作用域中，不收外界影响，使用let命令声明变量之前，该变量都是不可用的
这里if块级作用域里面的tmp使用let命令声明的变量出现在使用变量后，所以这里会报错

三.结合ES6新语法，用最简单的方式找出数组中的最小值
```
var arr = [12, 34, 32, 89, 4]

//Math.min()方法求最小值，该方法的参数是一个数值列表，而不是数组，
所以这里用es6的扩展运算符...把arr转换成列表
const minNum = Math.min(...arr)

console.log(minNum) // 4

```

四.请详细说明var, let, const三种声明变量的方式之间的具体差别
1.在浏览器中var声明的变量是一个全局变量，是挂载在window上的，而let和const声明的变量不能挂载
```
var a = 100;
console.log(a, window.a); //web环境下 100 100
let b = 100;
console.log(b, window.b); //web环境下 100 undefined
const c = 100;
console.log(c, window.c); //web环境下 100 undefined
```
2.let和const 不存在变量的提升，而var可以
```
console.log(a) //node环境下undefined  a已经声明还没赋值，默认undefined
var a = 1
console.log(b) //node环境下报错： b is not defined
let b = 1
console.log(c) //node环境下报错： c is not defined
const c = 1
```
3.let和const声明的变量只能在块级作用域内访问
```
if (true) {
	var a = 1;
	let b = 2;
	const c = 3;
}
console.log(a) // 1
console.log(b) // 报错： b is not defined
console.log(c) // 报错： c is not defined
```
4.同一作用域下let和const不能声明同名变量，而var可以
```
var a = 1;
var a = 10;
console.log(a) // 10
=========================
let a = 1;
let a = 10;
console.log(a) //报错，变量名需唯一
```
5.暂存死区
```
var tmp = 123;
if (true) {
	console.log(tmp);
	let tmp;
}
```
6.const
1) const一旦声明变量，就必须立即初始化，不能使用null占位，不能留到以后赋值
2）一旦声明，常量的值就不能改变
3）如果声明的是复合类型数据，可以修改其属性
```
const a; //报错，语法未通过
a = 123
console.log(a) 

const list = []；
list[0] = 10;
console.log(list) // [10]
```

总结： 不用var,主用const,配合let。避免开发陋习

五.请说出下列最终的执行结果，并解释为什么？
```
var a = 10;
var obj = {
	a: 20,
	fn () {
		setTimeout(() => {
			console.log(this.a)	
		})
	}
}

obj.fn()
```
答案： 20
fn方法中的计时器使用的是箭头函数，箭头函数内本身没有this的机制，所以this始终指向当前函数的作用域，当前作用域的a重新赋值为20，所以在计时器里面this.a的值为20

六.简述Symbol类型的用途？
1.使用Symbol来作为对象属性名
```
const symbol_name = Symbol()
const symbol_age = Symbol()

let obj = {
	[symbol_name]: 'lagou'
}

obj[symbol_age] = 10

console.log(obj) { [Symbol()]: 'lagou', [Symbol()]: 10 }

=========================================
//使用Symbol来作为对象属性名的对象属性只有Object.getOwnPropertySymbols()能取出
let obj = {
	[Symbol('name')]: 'tom',
	age: 18,
	sex: 'man'
}

console.log(Object.keys(obj)) //[ 'age', 'sex']
for (let item in obj) {
	console.log(item) // age sex
}
console.log(Object.getOwnPropertyNames(obj)) //[ 'age', 'sex']
console.log(JSON.stringify(obj)) //{"age":18,"sex":"man"}

//使用Object的API
console.log(Object.getOwnPropertySymbols(obj)) // [Symbol(name)]
// 使用新增的反射API
console.log(Reflect.ownKeys(obj)) //[ 'age', 'sex', Symbol(name) ]
```
2.使用Symbol结合iterator实现可迭代接口
```
const todos = {
	name: ['tom','jack','rose'],
	learn: ['java', 'js', 'php'],
	work: ['click_code'],

	[Symbol.iterator]: function * () {
		const all = [...this.name, ...this.learn, ...this.work]
		for (const item of all) {
			yield item
		}
	}

}

for (const item of todos) {
	console.log(item)
}
```


七.说说什么是浅拷贝，什么是深拷贝？
浅拷贝：就是拷贝源对象里面的数据，但是不拷贝源对象里面的子对象
- 不是和原数据指向同一对象
- 改变数据时不会改变原始数据，里面的子对象可以改变
```
//Object.assign实现浅拷贝
const a = {
	name: 'tom',
	books: ['java', 'php', 'js']
}
const b = Object.assign({}, a) 
a.name = 'jack' 
a.books[0] = 'c++'
console.log(b.name) //tom 不改变原始数据
console.log(b.books) //[ 'c++', 'php', 'js'] 改变原数据子对象
==================================
//...扩展运算符实现浅拷贝
const a = {
	name: 'tom',
	books: ['java', 'php', 'js']
}
const b = { ...a }
a.books.push('c++')
console.log(b.books) //[ 'java', 'php', 'js', 'c++' ]
```
深拷贝：克隆出一个对象，数据相同，但是引用地址不同(就是拷贝源对象里面的数据，而且也拷贝它里面的子对象)
- 不是和原数据指向同一对象
- 改变数据时不会改变原始数据，里面的子对象也不会改变
```
//JSON来实现深拷贝
const a = {
	name: 'tom',
	books: ['java', 'php', 'js'],
	jobs: {
		work1: '前端',
		work2: '销售'
	}
}
const b = JSON.parse(JSON.stringify(a))
b.name = 'jack'
b.books.push('c++')
b.jobs.work2 = 'java工程师'
console.log(a.name) //tom
console.log(a.books) //[ 'java', 'php', 'js' ]
console.log(a.jobs) // { work1: '前端', work2: '销售' }
//该方法有几种局限性
会忽略undefined
会忽略symbol
不能序列化函数
```


八.谈谈你是如何理解js异步编程的，EventLoop是做什么的，什么是宏任务，什么是微任务？
- js最大的特点就是单线程，在同一时间只能做同一件事，未执行的事件就得继续等待
- 当js执行事件中如果有一个事件需要事件等待，那整个线程就在那个事件执行等待，这样会占用线程，所以异步编程可以解决这个问题
- 但是异步编程也体现了回调函数在错误处理和嵌套的副作用，所以异步解决方案一直在发展中，从callback、 promise、 generator、 async/await

EventLoop是做什么的
在js上，所有同步任务都在主线程上执行，可以理解为存在一个执行栈
主线程外，还有一个消息队列，主要是等待异步任务的结果，一旦异步任务有了运行结果，就会加入到消息队列中
EventLoop监听消息队列里面是否有异步任务的执行结果，如果有就把结果放到执行栈中继续执行任务，不断的循环监听

什么是宏任务
回调队列里等待的任务称之为宏任务
宏任务执行过程中可以临时加上一些额外的需求，这些需求可以作为一个新的宏任务进到回调队列排队
setTimeout()的回调会作为宏任务执行
目前绝大多数异步调用都是作为宏任务执行

什么是微任务
直接在当前任务结束过后立即执行的任务叫微任务，不需要重新排队
Promise的回调会作为微任务执行


九.将下面异步代码使用Promise改进
```
setTimeout(function () {
	var a = "hello ";
	setTimeout(function () {
		var b = "lagou ";
		setTimeout(function () {
			var c = "I ♥ U";
			console.log(a + b + c);
		}, 10)
	}, 10)
}, 10)

===================================
Promise.resolve('hello ')
	.then((data) => {
		return data
	})
	.then((data) => {
		console.log(data)
		return data + 'lagou '
	})
	.then((data) => {
		console.log(data + "I ♥ U")
	})
//Promise对象的then方法会返回一个全新的Promise对象
后面的then方法就是在为上一个then返回的Promise注册回调
前面then方法中的回调函数的返回值会作为后面then方法回调的参数
```

十.请简述TypeScript与JavaScript之间的关系
- TypeScript 是 JavaScript 的超集，在 JavaScript 的原有基础上多了一些扩展特性(更强大的类型系统，对ES6新特性的支持)，
- TypeScript 对任何一种 JavaScript 运行环境都支持


十一.请谈谈你所认为的TypeScript优缺点
TypeScript优点

1. 增加了代码的可读性和可维护性
- 可以在编译阶段发现大部分错误
- 强大的类型系统
- 增强编辑器和IDE的功能，包括代码补全、接口提示、重构等
2. 包容性强
- TypeScript 是 JavaScript 的超集，.js 文件可以直接重命名为 .ts 即可
- 即使不显示定义类型，也能自动做出类型推断
- 可以定义一切类型
- 兼容第三方库
3. 拥有活跃的社区，生态更健全完善 
- 大部分第三方库都有提供给TypeScript的类型定义文件
- Angular2 和 VUE3使用TypeScript编写
- TypeScript 完全使用ES6规范

TypeScript缺点
- 需要一定的学习成本，学习TypeScript需要理解接口，泛型，类等概念
- 短期可能会增加开发成本，不过对于一个长期维护的项目，TypeScript能减少维护成本
- 可能和一些库结合不是很完美