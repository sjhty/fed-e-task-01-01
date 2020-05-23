# Part 1 · JavaScript 深度剖析 #
## ES 新特性与 JS 异步编程、TypeScript ##

# ECMAScript 新特性 #

## 一.ECMAScript概述 ##
1. ECMAScript 是 JavaScript的语言本身
2. JavaScript的标准化规范
3. 只提供最基本的语法
4. JavaScript实现了ECMAScript的语言标准，并且在这个基础之上做了扩展，使我们可以在浏览器当中操作BOM和DOM,在node环境中做一些读写文件操作

    JavaScript @ web {web apis（BOM,DOM）}

	JavaScript @ node.js {node apis (fs net etc.)}

5. JavaScript 语言本身指的就是ECMAScript
6. 从ES2015开始以年份命名，并开始称为ES6

## 二.ES2015概述 ##

1. 新时代ECMAScript标准的代表版本
2. 用ES6泛指所有的新标准
3. async、awite函数是ES2017中指定的标准
4. 四大类变化
- 解决原有语法上的一些问题或者不足
- 对原有语法进行增强
- 全新的对象、全新的方法、全新的功能
- 全新的数据类型和数据结构

## 三.let和块级作用域 ##

1. 作用域：某个成员能够起作用的范围
2. 在ES2015之前ES中只有两种作用域（全局作用域、函数作用域），ES2015增加了块级作用域（大括号内的就是块级作用域）
3. let所声明的变量只能在所声明的代码块中被访问
4. let和var的区别let声明不会出现提升的情况
5. let不允许在相同作用域内，重复声明同一个变量

```
function func() {
  let a = 10;
  var a = 1; //报错
  let a = 1; //报错
}

```

## 四.const ##
1. const声明一个只读的常量
2. const一旦声明变量，就必须立即初始化，不能留到以后赋值
3. 一旦声明，常量的值就不能改变

```
const a = 1;
a = 2; //报错

const arr = [];
arr.push(1) //[1] 在声明引用型数据为常量时，const保存的是变量的指针，只要保证指针不变就不会报错

arr = [];//报错 因为是赋值行为。变量arr保存的指针改变了
```
总结： 不用var,主用const,配合let。避免开发陋习

## 五.数组解构 ##
- 允许按照一定模式，从数组和对象中提取值，对变量进行赋值，这被称为解构
```
const arr = [100, 200, 300]
const [foo, bar, baz] = arr
console.log(foo, bar, baz) //按数组顺序输出

//结合展开运算符,展开运算符只能放在最后一位使用
const [foo, ...rest] = arr
console.log(rest) 

//提取到的成员设置默认值,即使在原始数组中找不到提取的元素也不影响
cosnt [foo, bar, baz = 111, more = 'default'] = arr
console.log(bar, more)
```
## 六.对象解构 ##
- 对象的解构与数组有一个重要的不同
- 数组的元素是按次序排列的，变量的取值由它的位置决定
- 而对象的属性没有次序，变量必须与属性同名，才能取到正确的值

```
const obj = { name: 'tom', age: 18 }
const { name } = obj
console.log(name) // tom

//解构的变量名在当前作用域有同名的成员
const name = 'jack'
const { name } = obj
console.log(name) // 报错
//解决办法
const { name: objName } = obj
console.log(objName) //tom
```
## 七.模板字符串 ##
```
const str = `es2015, 
nihao.`

//插值表达式,
const age = 19
cosnt msg = `tom, 年龄${age}
console.log(msg)// tom, 年龄19
```
## 八.带标签的模板字符串 ##
```
const str = console.log`ni hao` //[ 'ni hao' ]
const name = 'tom'
const gender = true
function myTagFunc(strings, name, gender) {
	console.log(strings) //[ 'hey, ', ' is a ', '' ]
	const sex = gender ? 'man' : 'woman'
	return string[0] + name + string[1] + sex + string[2]
}
const result = myTagFunc`hey, ${name} is a ${gender}`
console.log(result) //hey, tom is a man
```
## 九.字符串的扩展方法 ##
- includes()
- startsWith()
- endsWith()
判断字符串当中是否包含指定内容
```
const message = 'Error: foo is not defined'

console.log(
	message.startsWith('Error') //true
	message.endsWith('.') //false
	message.includes('is') //true
)

```
## 十.参数默认值 ##
```
function foo (enable) {
    //enable = enable || true //短路运算如果传入flase 也会使用默认值 输出undefined
    enable = enable === undefined ? true : enable
    console.log(enable)
}

foo(false) //false
//========================
//带有默认值的参数要放在最后
function foo (enable = true) {
    console.log(enable)
}

foo(false) //false

```
## 十一.剩余参数 ##
```
//...剩余参数只能放在最后，而且只能使用一次
function foo (...args) {
	console.log(args) //[1, 2, 3]
}

foo(1, 2, 3)
```
## 十二.展开数组 ##
```
cosnt arr = ['foo', 'bar', 'baz']

//console.log.apply(console,arr) 
console.log(...arr) //foo, bar, baz
```
## 十三.箭头函数 ##
- “=>”定义函数

```
function inc (n) {
	return n + 1
}
const inc = (n, m) => n+1

//如果函数体内有多个表达式
const inc = (n, m) => {
	console.log(111)
	return n + m
}

const arr = [1, 2, 3, 4]
arr.filter(i => i % 2)

```
## 十四.箭头函数与this ##
- 箭头函数不会改变this的指向

```
cosnt person = {
	name: 'tom',
	sayHi: () => {
		console.log(`nihao, ${this.name}`) //nihao, undefined 箭头函数没有this的机制，不会改变this的指向，在箭头函数外面this是什么在里面拿到的就是什么，任何情况都不会改变
	},
	sayHiAsync: function () {
		//setTimeout会在全局被使用，所以取不到this
		需要给this设置变量 const _this = this
		setTimeout(function () {
			console.log(this.name) // tom
		},1000)

		//箭头函数里面的this指向的都是当前作用域里面的this
		setTimeout(() => {
			console.log(this.name) // tom
		},1000)
	}
}
person.sayHiAsync()
```
## 十五.对象字面量的增强 ##
- 变量名和对象里面的属性名相同，在对象里面可以直接使用属性名，无需赋值

```
cosnt bar = 666
const obj = {
	foo: 123,
	bar, //相当于bar: bar
	method () { //相当于 method: function () {
		console.log(this)
	}，
	//Math.random(): 111 //不能直接使用动态属性名
	[Math.random()]: 111 //ES2015后新特性，可以用方括号直接使用动态属性名
}
```
## 十六.对象的扩展方法 ##
- Object.assign:将多个源对象中的属性复制到一个目标对象中，从源对象中取往目标对象中放

```
const source1 = {
	a: 111,
	b: 222
}

const source2 = {
	b: 555,
	d: 666
}

const target = {
	a: 333,
	c: 444
}

cosnt result = Object.assgin(target, source1) //target 为目标对象，source1为源对象
console.log(result) //{a: 111, c: 444, b: 222}
cosnt result = Object.assgin(target, source1, source2)
console.log(result) //{a: 111, c: 444, b: 555, d: 666}

//可以直接用assgin修改对象内属性的值
const obj = {name: 'lagou'}
const funObj = Object.assgin({}, obj)
funObj.name = 'lagoujiaoyu'
console.log(funObj) // {name: 'lagoujiaoyu'}

```
- Object.is: 用来判断两个值是否相等
== 和 ===的区别
== 会在比较之前自动转换数据类型 
```
0 == false //true
```
=== 是严格取对比两者之间的数值是否相同
```
0 === fase //false
+0 === -0 //三等对比不分正负 true
NaN === NaN //false

Object.is(+0, -0) //false
```
## 十七.Proxy ##
- 专门为对象设置访问代理器，通过Proxy可以轻松监视到对象的读写过程

```
const person = {
	name: 'tom',
	age: 20 
}

cosnt personProxy = new Proxy(person, {
	get (target, property) { //target: 代理目标对象，property: 外部所访问的对象名
		//console.log(target, property) //{ name: 'tom', age: 20} name
		//return property
		//判断访问的属性是否出现在代理目标对象里面，如果没有返回default
		return property in target ? target[property] : 'default'
	},
	set (target, property, value) {
		if (property === 'age') {
			if (!Number.isInteger(value)) {
				throw new TypeError(`${value} is not an int`)
			}
		}

		target[property] = value
	}
})

personProxy.age = 'jack' //jack is not an int

personProxy.gender = true //{ name: 'tom', age: 20, gender: true }

console.log(personProxy.name) //tom
```
- Proxy VS Object.defineProperty
- Object.defineProperty 只能监视属性的读写，Proxy能监视到更多的对象操作（如：delete操作，对对象方法操作）

```
const personProxy = new Proxy(person, {
	deleteProperty (target, property) { 
		console.log('delete', property) //delete age
		delete target[property]
	}
})

delete personProxy.age
console.log(person) // { name: 'tom'}
```
- Proxy对数组的操作
- 如何使用Proxy对象监视数组

```
cosnt list = []
const listProxy = new Proxy(list, {
	set(target, property, value) {
		console.log('set', property, value) //输出 set 0 100; 0是数组的下标

		target[property] = value
		return true
	}
})

listProxy.push(100)
console.log(list) //输出[ 100 ]
```
- Proxy 是以非侵入的方式监管了对象的读写

## 十八.Reflect ##
- Reflect属于一个静态类，不能new 
- Reflect内部封装了一系列对对象的底层操作
- Reflect成员方法就是Proxy处理对象的默认实现

```
cosnt obj = {
	foo: 111,
	bar: 222
}

cosnt proxy = new Proxy(obj, {
	get (target, property) {
		return Reflect.get(target, property)
	}
})

console.log(proxy.foo)
```
- Reflect最大的意义是同意提供一套用于操作对象的API

```
const obj = {
	name: 'tom',
	age: 18
}

console.log(Reflect.has(obj, 'name')) //判断是否存在某一个属性
console.log(Reflect.deleteProperty(obj, 'name')) //删除对象中的一个属性
console.log(Reflect.ownKeys(obj)) //获取对象当中所以的属性名
```
## 十九.Promise ##
- 一种更优的异步编程解决方案
- 通过链式调用的方式解决了传统异步编程中回调函数嵌套过深的问题

## 二十. Class类 ##
- 更容易理解，结构更加清晰

```
class Person {
	constructor (name) {
		this.name = name
	}

	say () {
		console.log(`hi, name is ${this.name}`)
	}
}

const p = new Person('tom')
p.say()  // hi， name is tom
```
## 二十一.静态方法 ##
- 类型中的方法一般分为实例方法和静态方法
- 实例方法需要通过这个类型构造的实例对象去调用
- 静态方法通过类型本身去调用
- ES2015 中新增添加静态成员的static关键词

```
class Person {
	constructor (name) {
		this.name = name
	}

	say () {
		console.log(`hi, name is ${this.name}`)
	}

	static create (name) {
		return new Person(name)
		//这里的this不会去指向实例对象，而是当前的类型
	}
}
const tom = Person.create('tom')
tom.say() //hi， name is tom
```
## 二十二.类的继承（extends） ##
```
class Person {
	constructor (name) {
		this.name = name
	}

	say () {
		console.log(`hi, name is ${this.name}`)
	}
}

class Student extends Person {
	constructor (name, number) {
		super(name)//super对象始终指向父类，调用super相当于调用了父类的构造函数
		this.number = number
	}

	hello () {
		super.say()
		console.log(`my school number is ${this.number}`)
	}
}

const s = new Student('tom', '10')
s.hello() //my school number is 10
```
## 二十三. Set数据结构 ##
- 可以理解为集合，与传统的数组相类似
- Set类型的成员是不允许重复的，每一个值在同一个Set中都是唯一的

```
const s = new Set()
s.add(1).add(2).add(3).add(2)

//forEach
s.forEach(i => console.log(i)) //1 2 3

//for of
for (let i of s) {
	console.log(i) //1 2 3
}

//size获取Set长度
s.size // 4

s.has() //判断集合当中是否存在某一个值
s.delete() //删除集合当中的某一个值
s.clear() //清空集合

//重新得到数组
Array.from(new Set(s))
const result = [...new Set(s)]
```
## 二十四. Map ##
- 与对象类似，键值对集合，键只能是字符串类型
- 如果对象的任何类型的属性名被获取的时候都会被转换为string
- Map用来映射两个任意类型数据之间的对应关系

```
const m = new Map()
const tom = {name: 'tom'}
m.set(tom,90)
console.log(m)
//m.has()
//m.delete()
//m.clear()
m.forEach(value, key) => {
	console.log(value, key)
}
```
## 二十五. Symbol ##
- 一种全新的原始数据类型
- 表示一个独一无二的值
- 对象的属性名可以用symbol来命名
- 最主要的作用就是为对象添加独一无二的属性名

```
const obj = {}
obj[Symbol()] = '123'
obj[Symbol()] = '456' //因为Symbol是唯一的，所以不会冲突

//模拟实现对象的私有成员
const name = Symbol()
const person = {
	[name]: 'tpm',
	say() {
		console.log(this[name])
	}
}

console.log(person.name) //undefined
person.say()
```
Symbol补充
```
const s1 = Symbol.for('foo')
//在方法内部维护的是字符串的对应关系，不管什么类型都转换为字符串

const obj = {
	[Symbol.toStringTag]: 'XObject'
}

//Symbol定义的对象属性名是无法拿到的
可以通过Object.getOwnPropertySymbols(obj)获取Symbol定义的属性名

```
## 二十六.for...of循环 ##
- for适用于普通的数组循环
- for...in 适用于键值对
- for...of 作为所有数据结构的统一方式
- 可以使用break关键词终止循环

```
const arr = [100, 200, 300]
for (const item of arr) {
	console.log(item)
	if (item > 100) {
		break
	}
}

//遍历Map获取键值
for (const [key, value] of map)
```

- 可迭代接口
- ES中能够表示有结构的数据类型越来越多
- iterable给各种各样的数据结构提供统一的遍历方式
- 实现iterable接口就是for...of的前提
- 

```
const set = new Set(['foo', 'bar', 'baz'])
const iterator = set[Symbol.iterator]()
console.log(iterator.next()) // {value: 'foo', done: false}
```
- 实现可迭代接口

```
//可迭代接口iterable
const obj = {
	store: ['foo', 'bar', 'baz'],
	//[Symbol.iterator]返回迭代器的iterator方法
	[Symbol.iterator]: function () {
		let index = 0
		const _this = this
		return {
			//实现了迭代器接口 iterator
			next: function () {
				//迭代结果对象iterableResult
				const result = {
					value: _this.store[index],
					done: index >= _this.store.length
				}
				index++
				return result
			}
		}
	}
}

for (const item of obj) {
	console.log('循环体',item) //循环体 foo 循环体 bar 循环体 baz
}
```
- 迭代器模式

```
const todos = {
	life: ['吃饭','睡觉','打豆豆'],
	learn: ['语文', '数学', '英语'],
	work: ['码代码'],

	// each: function (callback) {
	// 	const all = [].concat(this.life, this.learn, this.work)
	// 	for (const item of all) {
	// 		callback(item)
	// 	}
	// },
	[Symbol.iterator]: function () {
		const all = [...this.life, ...this.learn, ...this.work]
		let index = 0
		return {
			next: function () {
				return {
					value: all[index],
					done: index++ >= all.length
				}
			}
		}
	}

}

// todos.each(function(item){
// 	console.log(item)
// })

for (const item of todos) {
	console.log(item)
}
```
## 二十七.生成器（Generator） ##
- 避免异步编程中回调嵌套过深
- 惰性执行

```
function foo () {
	console.log('zce')
	return 100
}
const result = foo()
console.log(result.next())

function * foo () {
	console.log('111')
	yield 100  //第一次调用
	console.log('222')
	yield 200  //第二次调用
	console.log('333')
	yield 300 //第三次调用
}

const generator = foo()

console.log(generator.next())
```
- 生成器应用
```
function * createIdMaker () {
	let id = 1
	while (true) {
		yield id++
	}
}

const idMaker = createIdMaker()

console.log(idMaker.next().value)

const todos = {
	life: ['吃饭','睡觉','打豆豆'],
	learn: ['语文', '数学', '英语'],
	work: ['码代码'],

	[Symbol.iterator]: function * () {
		const all = [...this.life, ...this.learn, ...this.work]
		for (const item of all) {
			yield item
		}
	}

}

for (const item of todos) {
	console.log(item)
}
```
## 二十八. Modules ##

## 二十九. ECMAScript2016 ##
- includes() 检查数组成员是否存在
- 指数运算符

```
console.log(Math.pow(2, 10)) //1024
console.log(2 ** 10) //1024
```

## 三十. ECMAScript2017 ##
- Object.values  
- Object.entries 
- Object.getOwnPropertyDescruptors
- padStrat
- padEnd






