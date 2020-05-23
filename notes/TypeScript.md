# TypeScript 语言 #

## 一.课程概述 ##
- 是一门基于JavaScript之上的编程语言，重点解决JavaScript类型系统的问题
- 很大的提高代码的可靠程度

## 二.强类型与弱类型 ##
- 以类型安全的维度区分
- 1974年
- 强类型语言层面限制函数的实参类型必须与形参类型相同
- 弱类型语言层面不会限制实参的类型
- 由于这种强弱类型之分根本不是某一个权威机构的定义
- 强类型有更强的类型约束，而若类型中几乎没有什么约束
- 强类型语言中不允许任意的隐式类型转换
- 若类型语言则允许任意的数据隐式类型转换
- 语法层面限制
- 强类型和弱类型之间的区别就是是否随意的隐式类型转换
## 三.静态类型与动态类型 ##
- 以类型检查的维度区分
- 静态类型：一个变量声明时它的类型就是明确的，声明过后，它的类型就不允许再修改
- 动态类型：在运行阶段才能够明确变量类型，变量的类型随时可以改变
- 动态类型语言中的变量没有类型，变量中存放的值是有类型的
- 区别就是是否允许随时去修改变量的类型
## 四.JavaScript类型系统特征 ##
- 缺失类型系统的可靠性
- JS是一门脚本语言，没有编译环节
- JS是一门更灵活更多变的弱类型/动态类型语言

## 五.弱类型的问题 ##
- 在JS弱类型语言中必须要等到运行阶段才能发现代码中类型异常
- 
```
const obj = {}
obj.foo() //报错
setTimeout(() => {
	obj.foo()
},100000)
//第一个例子，因为弱类型的关系，需要等到运行时才能发现

function sun (a, b) {
	return a + b
}

console.log(sum(100, 100)) //200
console.log(sum(100, '100')) //100100
//类型不明确造成函数功能有可能发生改变

obj[true] = 100
console.log(obj['true'])
//对对象索引器错误的用法
```
## 六.强类型的优势 ##
- 错误更早暴露
- 代码更智能，编码更准确
- 重构更牢靠
- 减少不必要的类型判断
## 七.Flow ##
1. 概述
- JS静态类型检查器，弥补JS弱类型带来的弊端
- 添加类型注解
- 不必每个变量都需要注解

```
function sum(a: number, b: number) {
	return a + b
}
```
2. 快速上手
- 安装flow配置文件 (yarn flow init)
- 在需要检查的文件顶部加上@flow

3. 编译移除注解
- 安装 flow-remove-types
- yarn flow-remove-types src -d dist
- 把编写的代码跟实际运行的代码分开，中间加入了编译的环节
- 编辑最常见的工具babel
- @babel/core 核心模块 @babel/cli 命令行工具 @babel/preset-flow 转换插件
- 根目录下创建.babelrc 配置文件，并把preset-flow配置到该文件中
```
{
    "presets": ["@babel/preset-flow"]
}
```
- 执行 yarn babel src -d dist

4. 开发工具插件
- vscode安装Flow Language Support插件

5. 类型推断
```
//@flow
function square (n) {
	return n * n
}
```

6. 类型注解
```
//变量名加类型名称
let num: number = 100

//函数只允许返回指定的类型，没有返回值默认返回undefined，所以当没有返回值是类型写为void
function foo (): number {
	return 100
}
```

7. 原始类型
```
const a: string = ''
cosnt b: number = Infinity, NaN, 100
const c: boolean = false, true
cosnt d: null = null
const e: void = undefined
const f: symbol = Symbol()
```

8. 数组类型
```
//Array类型，需要泛型参数<number>,表示全部由数字组成的数组
const arr1: Array<number> = [1, 2]

const arr2: number[] = [1, 2]
//元祖
const foo: [string, number] = ['foo', 100]
```

9. 对象类型
```
const obj1: { foo: string, bar: number } = { foo: 'string', bar: 100}
//属性名后面加问号说明该属性可选
const obj2: { foo?: string, bar: number} = { bar: 100}
//obj3对象内的属性和值都必须是字符串类型
const obj3 = { [string]: string] }
obj3.key1 = 'value'
obj3.key2 = 100
```

10. 函数类型
- 对函数的参数类型和返回值类型进行约束，参数后面加类型注解，返回值函数的括号后面加类型注解
```
function foo (callback:(string, number) => void) {
	callback('string', 100)
}
```
11. 特殊类型

```
const a: 'foo' = 'foo'

const type: 'success' | 'warning' | 'danger' = 'success'

//自定义类型
type StringOrNumber = string | number

const b: string | number = 'string'

//问号表示变量处理接收number意外还接收null或undefined
const gender: ?number = undefined
想等于
const gender: number | null | void = undefined
```
12. Mixed 与 Any
- 所有类型的联合类型
```
function passMixed (value: mixed) {} //强类型
passMixed(可以是任何类型)
function passAng (value: any) {} //弱类型，尽量少用
passAng(可以是任何类型)
```
13. 类型小结
https://www.saltycrane.com/cheat-sheets/flow-type/latest/
全部类型
14. 运行环境API

## 八.TypeScript ##
1. 概述
2. 优点
- JS的超集，TypeScript 包含 JS，类型系统， ES6+
- 功能更为强大，生态更健全、更完善
- 前端领域中的第二语言
- 缺点
- 需要一定的学习成本，学习TypeScript需要理解接口，泛型，类等概念
- 短期可能会增加开发成本，不过对于一个长期维护的项目，TypeScript能减少维护成本
- 可能和一些库结合不是很完美

2. 快速上手
- 安装typescript模块
- yarn tsc 文件名 执行
```
const hello = name => {
	console.log(`hello, ${name}`)
}

hello('tom')
```
3. 配置文件
- yarn tsc --init
```

```
4. 原始类型
- 与flow区别，typescript的类型是允许为空
- 
5. 标准库声明
- 内置对象所对应的文件
```
const a: symbol = Symbol() //只能在ES2015中使用
解决方法1：修改tsconfig.json配置 target的值改为ES2015
解决办法2：增加属性lib:["ES2015", "dom"] 
```
6. 中文错误消息
7. 作用域问题
8. Object 类型
9. 数组类型
10. 元祖类型
11. 枚举类型
12. 函数类型
13. 任意类型
14. 隐式类型推断
15. 类型断言
16. 接口
17. 接口补充
18. 类的基本使用
19. 类的访问修饰符
20. 类的只读属性
21. 类与接口
22. 抽象类
23. 泛型
24. 类型声明