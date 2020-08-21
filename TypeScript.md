# zTypeScript

## 初识TypeScript

**TypeScript 是 JavaScript 的一个超集**，主要提供了**类型系统**和**对 ES6+ 的支持**，它由 Microsoft 开发，代码[开源于 GitHub](https://github.com/Microsoft/TypeScript) 上 !

<img src="https://s1.ax1x.com/2020/05/13/YaFoDJ.png" alt="w" style="zoom:33%;" />



## TypeScript 的特点

TypeScript 主要有 3 大特点：

- **始于JavaScript，归于JavaScript**

TypeScript 可以编译出纯净、 简洁的 JavaScript 代码，并且可以运行在任何浏览器上、Node.js 环境中和任何支持 ECMAScript 3（或更高版本）的JavaScript 引擎中。

- **强大的类型系统**

**类型系统**允许 JavaScript 开发者在开发 JavaScript 应用程序时使用高效的开发工具和常用操作比如静态检查和代码重构。

- **先进的 JavaScript**

TypeScript 提供最新的和不断发展的 JavaScript 特性，包括那些来自 2015 年的 ECMAScript 和未来的提案中的特性，比如异步功能和 Decorators，以帮助建立健壮的组件。



> ## 总结
>
> TypeScript 在社区的流行度越来越高，它非常适用于一些大型项目，也非常适用于一些基础库，极大地帮助我们提升了开发效率和体验。



## 安装 TypeScript

命令行运行如下命令，全局安装 TypeScript：

```bash
npm install -g typescript
```

安装完成后，在控制台运行如下命令，检查安装是否成功(3.x)：

```bash
tsc -V 
```



## 第一个 TypeScript 程序

### 编写第一个-ts-程序编写第一个 TS 程序

src/hello.ts

```typescript
function welcome (person) {
  return `hello,${person},欢迎学习TS`
}

let user = 'Tom'

console.log(welcome(user))
```

### 手动编译代码

我们使用了 `.ts` 扩展名，该文件就是一个ts文件，只不过代码是 `JavaScript` 而已。 `.ts`文件是不可以被浏览器直接运行的，需要翻译成`.js`文件

在命令行上，运行 TypeScript 编译器：

```bash
tsc hello.ts
```

输出结果为一个 `hello.js` 文件，它包含了和输入文件中相同的 JavsScript 代码。 接下来你可以在`html`文件中引入这个js让浏览器去运行它，当然你也可以在Node环境下运行：

```bash
node hello.js
```

控制台输出：

```text
Hello, Tom
```

### vscode自动编译

```
1). 生成配置文件tsconfig.json
    tsc --init
2). 修改tsconfig.json配置
    "outDir": "./js",
    "strict": false,    
3). 启动监视任务: 
    终端 -> 运行任务 -> 监视tsconfig.json
```

### 小试牛刀——TS中的类型注解

接下来让我们看看 TypeScript 的高级功能。 给 `person` 函数的参数添加 `: string` 类型注解，如下：

```typescript
function greeter (person: string) {
  return 'Hello, ' + person
}

let user = 'Yee'

console.log(greeter(user))
```

TypeScript 里的类型注解是一种轻量级的为函数或变量添加约束的方式。 在这个例子里，我们希望 `greeter` 函数接收一个字符串参数。 然后尝试把 `greeter` 的调用改成传入一个数组：

```typescript
function greeter (person: string) {
  return 'Hello, ' + person
}

let user = [0, 1, 2]

console.log(greeter(user))
```

重新编译，你就看到产生了一个错误，提示你缺少了参数，类似地，尝试删除 `greeter` 调用的所有参数。 TypeScript 会告诉你使用了非期望个数的参数调用了这个函数。 在这两种情况中，TypeScript提供了静态的代码分析，它可以分析代码结构和提供的类型注解。

要注意的是尽管有错误，文件还是被创建了。 就算你的代码里有错误，你仍然可以使用 TypeScript。但在这种情况下，TypeScript 会警告你代码可能不会按预期执行。



> ## 总结
>
> 到这里，你已经对 TypeScript 有了一个大致的印象，那么下一章让我们来一起学习 TypeScript 的一些常用语法吧。



## 使用webpack打包TS

### 下载依赖

```text
yarn add -D typescript webpack webpack-cli webpack-dev-server html-webpack-plugin clean-webpack-plugin ts-loader cross-env
```

### 入口JS: src/main.ts

```typescript
// import './01_helloworld'

document.write('Hello Webpack TS!')
```

### 准备html文件: public/index.html

根目录下建立public/index.html

### config/webpack.config.js

```javascript
const {CleanWebpackPlugin} = require('clean-webpack-plugin')
const HtmlWebpackPlugin = require('html-webpack-plugin')
const path = require('path')

const isProd = process.env.NODE_ENV === 'production' // 是否生产环境

function resolve (dir) {
  return path.resolve(__dirname, '..', dir)
}

module.exports = {
  mode: isProd ? 'production' : 'development',
  entry: {
    app: './src/main.ts'
  },

  output: {
    filename: '[name].[contenthash:8].js'
  },

  module: {
    rules: [
      {
        test: /\.tsx?$/,
        use: 'ts-loader',
        include: [resolve('src')]
      }
    ]
  },

  plugins: [
    new CleanWebpackPlugin({
    }),

    new HtmlWebpackPlugin({
      template: './public/index.html'
    })
  ],

  resolve: {
    extensions: ['.ts', '.tsx', '.js']
  },

  devtool: isProd ? 'cheap-module-source-map' : 'cheap-module-eval-source-map',

  devServer: {
    contentBase: './dist', // 服务器加载资源的基础路径
    host: 'localhost', // 主机名
    stats: 'errors-only', // 打包日志输出输出错误信息
    port: 8081,
    open: true
  },
}
```

### 配置打包命令

```text
"dev": "cross-env NODE_ENV=development webpack-dev-server --config config/webpack.config.js",
"build": "cross-env NODE_ENV=production webpack --config config/webpack.config.js"
```

### 运行与打包

```text
yarn dev
yarn build
```



# TypeScript 常用语法

## 基础类型

TypeScript 支持与 JavaScript 几乎相同的数据类型，此外还提供了实用的枚举类型方便我们使用。

### 布尔值

最基本的数据类型就是简单的 true/false 值，在JavaScript 和 TypeScript 里叫做 `boolean`（其它语言中也一样）。

```typescript
let isDone: boolean = false;
isDone = true;
// isDone = 2 // error
```

### 数字

和 JavaScript 一样，TypeScript 里的所有数字都是浮点数。 这些浮点数的类型是 number。 除了支持十进制和十六进制字面量，TypeScript 还支持 ECMAScript 2015中引入的二进制和八进制字面量。

```typescript
let a1: number = 10 // 十进制
let a2: number = 0b1010  // 二进制
let a3: number = 0o12 // 八进制
let a4: number = 0xa // 十六进制
```

### 字符串

JavaScript 程序的另一项基本操作是处理网页或服务器端的文本数据。 像其它语言里一样，我们使用 `string` 表示文本数据类型。 和 JavaScript 一样，可以使用双引号（`"`）或单引号（`'`）表示字符串。

```typescript
let name:string = 'tom'
name = 'jack'
// name = 12 // error
let age:number = 12
const info = `My name is ${name}, I am ${age} years old!`
```

### undefined 和 null

TypeScript 里，`undefined` 和 `null` 两者各自有自己的类型分别叫做 `undefined` 和 `null`。 和 `void` 相似，它们的本身的类型用处不是很大：

```typescript
let u: undefined = undefined
let n: null = null
```

默认情况下 `null` 和 `undefined` 是所有类型的子类型。 就是说你可以把 `null` 和 `undefined` 赋值给 `number` 类型的变量。

### 数组

TypeScript 像 JavaScript 一样可以操作数组元素。 有两种方式可以定义数组。 第一种，可以在`元素类型后面接上[]`，表示由此类型元素组成的一个数组：

```typescript
let list1: number[] = [1, 2, 3]
```

第二种方式是使用数组泛型，`Array<元素类型>`：

```typescript
let list2: Array<number> = [1, 2, 3]
```

### 元组

元组（Tuple）类型允许表示一个已知元素数量和类型的数组，`各元素的类型不必相同`。 比如，你可以定义一对值分别为 `string` 和 `number` 类型的元组。

```typescript
let t1: [string, number]
t1 = ['hello', 10] // OK
t1 = [10, 'hello'] // Error
```

当访问一个已知索引的元素，会得到正确的类型：

```typescript
console.log(t1[0].substring(1)) // OK
console.log(t1[1].substring(1)) // Error, 'number' 不存在 'substring' 方法
```

### 枚举

枚举（enum）类型是对 JavaScript 标准数据类型的一个补充。 使用枚举类型可以`为一组数值赋予友好的名字`。 一般用于组织一些有相似之处的常量，让这些常量更规范、更统一。

> 枚举类型的特点：

1. 第一个开始自动赋值0，后来依次+1，
2. 枚举的键值对会被反转声明（反向映射）

```typescript
//定义百家姓的枚举
enum BookNames {
  Zhao,
  Qian,
  Sun,
  Li,
}
console.log(BookNames)
console.log(BookNames.Zhao)
console.log(BookNames[0])
```

默认情况下，从 `0` 开始为元素编号。 你也可以手动的指定成员的数值。 例如，我们将上面的例子改成从 `1` 开始编号：

```typescript
enum BookNames {Zhao = 1, Qian, Sun}
console.log(BookNames.Zhao)
```

或者，全部都采用手动赋值：

```typescript
enum BookNames {Zhao = 1, Qian = 2, Sun = 3}
let str : string = BookNames[1]
console.log(str)
```

枚举类型提供的一个便利是你可以由枚举的值得到它的名字。 例如，我们知道数值为 2，但是不确定它映射到 BookNames 里的哪个名字，我们可以查找相应的名字：

```typescript
enum BookNames {Zhao = 1, Qian = 2, Sun = 3}
let name2: string = BookNames[2]

console.log(name2)  // 'Qian'
```

### any

有时候，我们会想要为那些在编程阶段还不清楚类型的变量指定一个类型。 这些值可能来自于动态的内容，比如来自用户输入或第三方代码库。 这种情况下，我们不希望类型检查器对这些值进行检查而是直接让它们通过编译阶段的检查。 那么我们可以使用 `any` 类型来标记这些变量：

```typescript
let notSure: any = 4
notSure = 'maybe a string'
notSure = false // 也可以是个 boolean
```

在对现有代码进行改写的时候，`any` 类型是十分有用的，它允许你在编译时可选择地包含或移除类型检查。并且当你只知道一部分数据的类型时，`any` 类型也是有用的。 比如，你有一个数组，它包含了不同的类型的数据：

```typescript
let list: any[] = [1, true, 'free']

list[1] = 100
```

### void

某种程度上来说，`void` 类型像是与 `any` 类型相反，它`表示没有任何类型`。 当一个函数没有返回值时，你通常会见到其返回值类型是 `void`：

```typescript
/* 表示没有任何类型, 一般用来说明函数的返回值不能是undefined和null之外的值 */
function fn(): void {
  console.log('fn()')
  // return undefined
  // return null
  // return 1 // error
}
```

声明一个 `void` 类型的变量没有什么大用，因为你只能为它赋予 `undefined` 和 `null`：

```typescript
let unusable: void = undefined
```

### object

`object` 表示非原始类型，也就是除 `number`，`string`，`boolean`之外的类型。

```typescript
function fn2(obj:object):object {
  console.log('fn2()', obj)
  return {}
  // return undefined
  // return null
}
console.log(fn2([1,2,3]))
// console.log(fn2('abc') // error
console.log(fn2(Date))
```

### 联合类型

联合类型（Union Types）表示取值可以为多种类型中的一种
需求1: 定义一个函数，返回一个数字或字符串的字符串形式值

```typescript
function toString2(x: number | string) : string {
  return x.toString()
}
```

需求2: 定义一个函数，返回一个数字或字符串的长度

```typescript
function getLength(x: number | string) {

  // return x.length // error

  if (x.length) { // error
    return x.length
  } else {
    return x.toString().length
  }
}
```

### 类型断言

通过类型断言（Type Assertion）这种方式可以告诉编译器，“相信我，我知道自己在干什么”。 类型断言好比其它语言里的类型转换，但是不进行特殊的数据检查和解构。 它没有运行时的影响，只是在编译阶段起作用。 TypeScript 会假设你，程序员，已经进行了必须的检查。

类型断言有两种形式。 其一是“尖括号”语法, 另一个为 `as` 语法

```typescript
function getLength(x: number | string) {
  if ((<string>x).length) {
    return (x as string).length
  } else {
    return x.toString().length
  }
}
console.log(getLength('abcd'), getLength(1234))
```

### 类型推断

类型推断: TS会在没有明确的指定类型的时候推测出一个类型
有下面2种情况: 1. 定义变量时赋值了, 推断为对应的类型. 2. 定义变量时没有赋值, 推断为any类型

```typescript
/* 定义变量时赋值了, 推断为对应的类型 */
let b9 = 123 // number
// b9 = 'abc' // error

/* 定义变量时没有赋值, 推断为any类型 */
let b10  // any类型
b10 = 123
b1
```



## 接口

TypeScript 的核心原则之一是对值所具有的结构进行类型检查。我们使用接口（Interfaces）来定义对象的类型。`接口是对象的状态(属性)和行为(方法)的抽象(描述)`

### 对象接口

需求: 创建人的对象, 需要对人的属性进行一定的约束

```text
id是number类型, 必须有, 只读的
name是string类型, 必须有
age是number类型, 必须有
sex是string类型, 可以没有
```

下面通过一个简单示例来观察接口是如何工作的：

```typescript
// 定义人的接口
interface IPerson {
  id: number
  name: string
  age: number
  sex: string
}

const person1: IPerson = {
  id: 1,
  name: 'tom',
  age: 20,
  sex: '男'
}
```

类型检查器会查看对象内部的属性是否与IPerson接口描述一致, 如果不一致就会提示类型错误。

#### 可选属性

接口里的属性不全都是必需的。 有些是只在某些条件下存在，或者根本不存在。

```typescript
interface IPerson {
  id: number
  name: string
  age: number
  sex?: string
}
```

带有可选属性的接口与普通的接口定义差不多，只是在可选属性名字定义的后面加一个 `?` 符号。

可选属性的好处之一是可以对可能存在的属性进行预定义，好处之二是可以捕获引用了不存在的属性时的错误。

```typescript
const person2: IPerson = {
  id: 1,
  name: 'tom',
  age: 20,
  // sex: '男' // 可以没有
}
```

#### 只读属性

一些对象属性只能在对象刚刚创建的时候修改其值。 你可以在属性名前用 `readonly` 来指定只读属性:

```typescript
interface IPerson {
  readonly id: number
  name: string
  age: number
  sex?: string
}
```

一旦赋值后再也不能被改变了。

```typescript
const person2: IPerson = {
  id: 2,
  name: 'tom',
  age: 20,
  // sex: '男' // 可以没有
  // xxx: 12 // error 没有在接口中定义, 不能有
}
person2.id = 2 // error
```

##### readonly vs const

最简单判断该用 `readonly` 还是 `const` 的方法是看要把它做为变量使用还是做为一个属性。 做为变量使用的话用 `const`，若做为属性则使用 `readonly`。

### 函数接口

接口能够描述 JavaScript 中对象拥有的各种各样的外形。 除了描述带有属性的普通对象外，接口也可以描述函数类型。

为了使用接口表示函数类型，我们需要给接口定义一个调用签名。它就像是一个只有参数列表和返回值类型的函数定义。参数列表里的每个参数都需要名字和类型。

```typescript
interface findKeyWord {
  (source:string,keyWord:string):boolean
}
```

这样定义后，我们可以像使用其它接口一样使用这个函数类型的接口。 下例展示了如何创建一个函数类型的变量，并将一个同类型的函数赋值给这个变量。

```typescript
const mySearch:findKeyWord = function (source,keyWord) {
  return source.search(keyWord) > -1
}
```

### 类接口

#### 类实现接口

与 C# 或 Java 里接口的基本作用一样，TypeScript 也能够用它来明确的强制一个类去符合某种契约。

```typescript
//车轮接口
interface IWhell{
  whellType:string,
  roll():any
}

class Car implements IWhell {
  whellType = '防滑轮胎'
  roll(){
    console.log('车轮飞速滚动');
  }
}
```

#### 一个类可以实现多个接口

```typescript
//车轮接口
interface Whell {
  whellType:string
  roll():any
}

//灯光接口
interface Light {
  lightOn(): void;
  lightOff(): void;
}

//类实现接口(一个类可以实现多个接口)
class Car implements Whell,Light {
  whellType = '防滑轮胎'
  roll() {
    console.log('车轮飞速滚动');
  }
  lightOn(){
    console.log('灯亮了')
  }
  lightOff(){
    console.log('灯关了')
  }
}

const c = new Car()
c.roll()
c.lightOn()
c.lightOff()
```

### 接口继承接口

和类一样，接口也可以相互继承。 这让我们能够从一个接口里复制成员到另一个接口里，可以更灵活地将接口分割到可重用的模块里。

```typescript
//接口继承接口
interface Option extends Wheel,Light{
```



## 类

对于传统的 JavaScript 程序我们会使用`函数`和`基于原型的继承`来创建可重用的组件，但对于熟悉使用面向对象方式的程序员使用这些语法就有些棘手，因为他们用的是`基于类的继承`并且对象是由类构建出来的。 从 ECMAScript 2015，也就是 ES6 开始， JavaScript 程序员将能够使用基于类的面向对象的方式。 使用 TypeScript，我们允许开发者现在就使用这些特性，并且编译后的 JavaScript 可以在所有主流浏览器和平台上运行，而不需要等到下个 JavaScript 版本。

### 基本示例

下面看一个使用类的例子：

```typescript
/* 
类的基本定义与使用
*/
class Person {
  //声明属性
  name:string
  age:number
  
  //构造方法
  constructor(name,age){
    this.name = name
    this.age = age
  }

  //一般方法
  speak():void{
    console.log(`我的名字是${this.name},我的年龄是${this.age}`)
  }

}
//创建类的实例
const person1 = new Person('tom',19)
//调用实例的方法
person1.speak()
```

如果你使用过 C# 或 Java，你会对这种语法非常熟悉。 我们声明一个 `Person` 类。这个类有 4个成员：一个叫做 `name` 的属性，一个叫做 `age` 的属性，一个构造函数和一个 `speak` 方法。

你会注意到，我们在引用任何一个类成员的时候都用了 `this`。 它表示我们访问的是类的成员。

后面一行，我们使用 `new` 构造了 `Person` 类的一个实例。它会调用之前定义的构造函数，创建一个 `Person` 类型的新对象，并执行构造函数初始化它。

最后一行通过 `person1` 对象调用其 `speak` 方法

### 继承

在 TypeScript 里，我们可以使用常用的面向对象模式。 基于类的程序设计中一种最基本的模式是允许使用继承来扩展现有的类。

看下面的例子：

```typescript
class Animal {
  run (distance: number) {
    console.log(`动物奔跑了${distance}米`)
  }
}

class Dog extends Animal {
  cry () {
    console.log('汪汪汪！')
  }
}

const dog = new Dog()
dog.cry() 
dog.run(100) // 可以调用从父中继承得到的方法
```

这个例子展示了最基本的继承：类从基类中继承了属性和方法。 这里，`Dog` 是一个 派生类，它派生自 `Animal` 基类，通过 `extends` 关键字。 派生类通常被称作*子类*，基类通常被称作*超类*。

因为 `Dog` 继承了 `Animal` 的功能，因此我们可以创建一个 `Dog` 的实例，它能够 `cry()` 和 `run()`。

### 多态

多态指同一个实体同时具有多种形式。它是面向对象程序设计（OOP）的一个重要特征。如果一个语言只支持类而不支持多态，只能说明它是基于对象的，而不是面向对象的， 下面我们来看个更加复杂的例子：

```typescript
//定义Animal基类
class Animal {
  //声明属性
  name:string

  //构造方法
  constructor(name){
    this.name = name
  }

  //一般方法
  run (distance: number) {
    console.log(`动物前进了 ${distance}m`)
  }
}

class Snake extends Animal{
  //构造方法
  constructor(name){
    super(name)
  }
  //重写从父类继承过来的方法
  run(distance:number){
    console.log(`蛇前进了${distance}米`)
  }
}

class Horse extends Animal{
  //构造方法
  constructor(name){
    super(name)
  }
  //重写从父类继承过来的方法
  run(distance:number){
    console.log(`马奔跑了${distance}米`)
  }
  //自己的一般方法
  cry(){
    console.log('hou~hou~')
  }
}

// 父类型引用指向子类型的实例 ==> 多态
let tom:Animal

tom = new Snake('蟒蛇')
tom.run(100)

tom = new Horse('短腿马')
tom.run(300)
```

这个例子展示了一些上面没有提到的特性。 这一次，我们使用 `extends` 关键字创建了 Animal的两个子类：`Horse` 和 `Snake`。

与前一个例子的不同点是，派生类包含了一个构造函数，它 必须调用 `super()`，它会执行基类的构造函数。 而且，在构造函数里访问 `this` 的属性之前，我们 一定要调用 `super()`。 这个是 TypeScript 强制执行的一条重要规则。

这个例子演示了如何在子类里可以重写父类的方法。`Snake`类和 `Horse` 类都创建了 `run` 方法，它们重写了从 `Animal` 继承来的 `run` 方法，使得 `run` 方法根据不同的类而具有不同的功能。注意，即使 `tom` 被声明为 `Animal` 类型，但因为它的值是 `Horse`，调用 `tom.run(34)` 时，它会调用 `Horse` 里重写的方法。

### 公共、私有与受保护的修饰符

#### 默认为 public

在上面的例子里，我们可以自由的访问程序里定义的成员。 如果你对其它语言中的类比较了解，就会注意到我们在之前的代码里并没有使用 `public` 来做修饰；例如，C# 要求必须明确地使用 `public` 指定成员是可见的。 在 TypeScript 里，成员都默认为 `public`，当然你也可以明确的将一个成员标记成 `public`。

#### 理解 private

当成员被标记成 `private` 时，它就不能在声明它的类的外部访问。

```typescript
  /* 
    访问修饰符: 用来描述类内部的属性/方法的可访问性
      public: 默认值, 公开的外部也可以访问
      protected: 类内部、子类可以访问
      private: 只有类内部可以访问
  */
   //定义Person基类
   class Person {
    //声明属性
    private name:string //name属性加了private修饰，只能在类内部访问
    age:number //age属性没有加修饰，默认为public修饰

    //构造方法
    constructor(name:string,age:number){
      this.name = name
      this.age = age
    }
    //一般方法
    speak():string{
      return `我的名字是${this.name},我的年龄是${this.age}`
    }
  }

  let tom = new Person('tom',19)
  //console.log(tom.name) //属性“name”为私有属性，只能在类“Person”中访问
  console.log(tom.age) //19
```

#### 理解 protected

```typescript
    class Person {
    private name:string
    age:string

    constructor (name,age){
      this.name = name
      this.age = age
    }

    protected speak():string{
      return `我的名字是${this.name}，我的年龄是${this.age}`
    }
  }

  class Student extends Person {
    sex:string

    constructor (name,age,sex){
      super(name,age)
      this.sex = sex
    }

    study(){
      super.speak() //
      console.log('我在很努力的学习');
    }
  }


  let p1 = new Person('tom',19)
  console.log(p1.name);  // private私有的-不可见
  console.log(p1.age);  // public公开的-可见
  console.log(p1.speak());// protected受保护的-不可见
```

### readonly 修饰符

你可以使用 `readonly` 关键字将属性设置为只读的。 只读属性必须在声明时或构造函数里被初始化。

```typescript
class Person {
  readonly name: string = 'abc'
  constructor(name: string) {
    this.name = name
  }
}

let john = new Person('John')
// john.name = 'peter' // error
```

#### 参数属性

在上面的例子中，我们必须在 `Person` 类里定义一个只读成员 `name` 和一个参数为 `name` 的构造函数，并且立刻将 `name` 的值赋给 `this.name`，这种情况经常会遇到。 参数属性可以方便地让我们在一个地方定义并初始化一个成员。 下面的例子是对之前 `Person` 类的修改版，使用了参数属性：

```text
class Person2 {
  constructor(readonly name: string) {
		this.name = name
  }
}

const p = new Person2('jack')
console.log(p.name)
```

注意看我们是如何舍弃参数 `name`，仅在构造函数里使用 `readonly name: string` 参数来创建和初始化 `name` 成员。 我们把声明和赋值合并至一处。

参数属性通过给构造函数参数前面添加一个访问限定符来声明。使用 `private` 限定一个参数属性会声明并初始化一个私有成员；对于 `public` 和 `protected` 来说也是一样。

### 存取器

`TypeScript` 支持通过 `getters/setters` 来截取对对象成员的访问。 它能帮助你有效的控制对对象成员的访问。

```typescript
class Person {
  firstName: string = 'A'
  lastName: string = 'B'

  constructor(firstName,lastName){
    this.firstName = firstName
    this.lastName = lastName
  }

  get fullName(){
    return this.firstName + '-' + this.lastName
  }

  set fullName(value){
    const names = value.split('-')
    this.firstName = names[0]
    this.lastName = names[1]
  }
}
const p = new Person('zahng','san')
console.log(p.fullName);
```

### 静态属性

到目前为止，我们只讨论了类的实例成员，那些仅当类被实例化的时候才会被初始化的属性。 我们也可以创建类的静态成员，这些属性存在于类本身上面而不是类的实例上。

```typescript
class Person {
  name: string = 'petter'
  static info: string = 'some info'
}

console.log(Person.info)
console.log(new Person().name)
```

### 抽象类

抽象类做为其它派生类的基类使用。 它们不能被实例化。不同于接口，抽象类可以包含成员的实现细节。 `abstract` 关键字是用于定义抽象类和在抽象类内部定义抽象方法。

```typescript
  //定义一个抽象类
  abstract class Animal {
    abstract cry ():void //定义一个抽象方法cry
    run () { //定义一个方法run
      console.log('正在奔跑....')
    }
  }
  
  class Dog extends Animal {
    cry () {
      console.log('汪汪汪！')
    }
  }
  
  const dog = new Dog()
  dog.cry()
  dog.run()
```



## 函数

TypeScript 为 JavaScript 函数添加了额外的功能，让我们可以更容易地使用。

### 基本示例

和 JavaScript 一样，TypeScript 函数可以创建有名字的函数和匿名函数。你可以随意选择适合应用程序的方式，不论是定义一系列 API 函数还是只使用一次的函数。

通过下面的例子可以迅速回想起这两种 JavaScript 中的函数：

```javascript
// 命名函数
function add(x, y) {
  return x + y
}

// 匿名函数
let myAdd = function(x, y) { 
  return x + y;
}
```

### 函数类型

#### 为参数定义类型

让我们为上面那个函数添加类型：

```typescript
function add(x: number, y: number): number {
  return x + y
}

let myAdd = function(x: number, y: number): number { 
  return x + y
}
```

我们可以给每个参数添加类型之后再为函数本身添加返回值类型。TypeScript 能够根据返回语句自动推断出返回值类型。

#### 书写完整函数类型

现在我们已经为函数指定了类型，下面让我们写出函数的完整类型。

```typescript
let myAdd:(x:number,y:number)=>number = function(x: number, y: number): number { 
  return x + y
}
```

### 可选参数和默认参数

TypeScript 里的每个函数参数都是必须的。 这不是指不能传递 `null` 或 `undefined` 作为参数，而是说编译器检查用户是否为每个参数都传入了值。编译器还会假设只有这些参数会被传递进函数。 简短地说，传递给一个函数的参数个数必须与函数期望的参数个数一致。

JavaScript 里，每个参数都是可选的，可传可不传。 没传参的时候，它的值就是 `undefined`。 在TypeScript 里我们可以在参数名旁使用 `?` 实现可选参数的功能。 比如，我们想让 `lastName` 是可选的：

在 TypeScript 里，我们也可以为参数提供一个默认值当用户没有传递这个参数或传递的值是 `undefined` 时。 它们叫做有默认初始化值的参数。 让我们修改上例，把`firstName` 的默认值设置为 `"A"`。

```typescript
function buildName(firstName: string='A', lastName?: string): string {
  if (lastName) {
    return firstName + '-' + lastName
  } else {
    return firstName
  }
}

console.log(buildName('C', 'D'))
console.log(buildName('C'))
console.log(buildName())
```

#### 剩余参数

必要参数，默认参数和可选参数有个共同点：它们表示某一个参数。 有时，你想同时操作多个参数，或者你并不知道会有多少参数传递进来。 在 JavaScript 里，你可以使用 `arguments` 来访问所有传入的参数。

在 TypeScript 里，你可以把所有参数收集到一个变量里：
剩余参数会被当做个数不限的可选参数。 可以一个都没有，同样也可以有任意个。 编译器创建参数数组，名字是你在省略号（ `...`）后面给定的名字，你可以在函数体内使用这个数组。

```typescript
function info(x: string, ...args: string[]) {
  console.log(x, args)
}
info('abc', 'c', 'b', 'a')
```

### 函数重载

函数重载: 函数名相同, 而形参不同的多个函数
在JS中, 由于弱类型的特点和形参与实参可以不匹配, 是没有函数重载这一说的 但在TS中, 与其它面向对象的语言(如Java)就存在此语法

```typescript
// 重载函数声明
function add (x: string, y: string): string
function add (x: number, y: number): number

// 定义函数实现
function add(x: string | number, y: string | number): string | number {
  // 在实现上我们要注意严格判断两个参数的类型是否相等，而不能简单的写一个 x + y
  if (typeof x === 'string' && typeof y === 'string') {
    return x + y
  } else if (typeof x === 'number' && typeof y === 'number') {
    return x + y
  }
}

console.log(add(1, 2))
console.log(add('a', 'b'))
// console.log(add(1, 'a')) // error
```



## 泛型

泛型（generic）指在定义函数、接口或类的时候，不预先指定具体的类型，而在使用的时候再指定具体类型的一种特性。

### 引入

下面创建一个函数, 实现功能: 根据指定的数量 `count` 和数据 `value` , 创建一个包含 `count` 个 `value` 的数组 不用泛型的话，这个函数可能是下面这样：

```typescript
function createArray (count:number,value:any):any[]{
  const arr:any[] = []
  for (let index = 0; index < count; index++) {
    arr.push(value)
  }
  return arr
}

const arr01 = createArray(3,'hello')
const arr02 = createArray(3,100)

console.log(arr01[0].split('')) //运行不报错，但编码时没有提示
console.log(arr02[0].toFixed(3)) //运行不报错，但编码时没有提示
console.log(arr02[0].split('')) //运行报错，但编码时没有提示错误 
```

### 使用函数泛型

```typescript
function createArray <P>(count:number,value:P):P[]{
  const arr:P[]= []
  for (let index = 0; index < count; index++) {
    arr.push(value)
  }
  return arr
}

const arr03 = createArray<string>(3,'hello')
const arr04 = createArray<number>(3,100)
console.log(arr03[0].split(''))
console.log(arr04[0].toFixed(1))
console.log(arr04[0].split('')) //error 类型“number”上不存在属性“split”
```

### 多个泛型参数的函数

一个函数可以定义多个泛型参数

```typescript
function createArray <T,P> (a: T, b: P): [T, P] {
  return [a, b]
}

const result = createArray<string, number>('abc', 123)
console.log(result[0].length)
console.log(result[1].toFixed())
```

### 泛型接口

在定义接口时, 为接口中的属性或方法定义泛型类型
在使用接口时, 再指定具体的泛型类型

```typescript
// 定义一个CRUD操作的泛型接口
interface BaseCRUD<T> {

  data: T[] //保存内部所有数据对象的数组

  add: (t: T) => number // 添加一个新的数据对象 返回数据对象的id

  getById: (id: number) => T // 根据id查询对应的数据对象
}

/* 定义一个数据类型 */
class User {
  id?: number
  name: string
  age: number

  constructor (name: string, age: number) {
    this.name = name
    this.age = age
  }
}

// 定义操作User数据的实现类
class UserCRUD implements BaseCRUD<User> {
  data: User[] = [] 

  /* 
  添加一个新的数据对象 返回数据对象的id
  */
  add (user: User): number {
    const id = Date.now()
    user.id = id
    this.data.push(user)

    return id
  } 

  /* 
  根据id查询对应的数据对象
  */
  getById (id: number): User {
    return this.data.find(user => user.id===id)
  }
}

// 测试
const userCRUD = new UserCRUD()
const id1 = userCRUD.add(new User('tom', 12))
const id2 = userCRUD.add(new User('tom2', 13))
console.log(userCRUD.data, userCRUD.getById(id1), use
```

### 泛型类

在定义类时, 为类中的属性或方法定义泛型类型 在创建类的实例时, 再指定特定的泛型类型

```typescript
class Data<T> {
  value: T
  add: (x: T, y: T) => T
}

let data1 = new Data<number>()
data1.value = 0
data1.add = function(x: number, y: number) {
  return x + y 
}

let data2 = new Data<string>()
data2.value = 'abc'
data2.add = function(x: string, y: string) {
  return x + y 
}
```

### 泛型约束

如果我们直接对一个泛型参数取 `length` 属性, 会报错, 因为这个泛型根本就不知道它有这个属性

```typescript
// 没有泛型约束
function fn <T>(x: T): void {
  // console.log(x.length)  // error
}
```

我们可以使用泛型约束来实现

```typescript
interface Lengthwise {
  length: number;
}

// 指定泛型约束
function fn2 <T extends Lengthwise>(x: T): void {
  console.log(x.length)
}
```

我们需要传入符合约束类型的值，必须包含必须 `length` 属性：

```typescript
fn2('abc')
// fn2(123) // error  number没有length属性
```



## 其它

### 声明文件

当使用第三方库时，我们需要引用它的声明文件，才能获得对应的代码补全、接口提示等功能

什么是声明语句

假如我们想使用第三方库 jQuery，一种常见的方式是在 html 中通过 `<script>` 标签引入 `jQuery`，然后就可以使用全局变量 `$` 或 `jQuery` 了。

但是在 ts 中，编译器并不知道 $ 或 jQuery 是什么东西

```typescript
jQuery('#foo');
// ERROR: Cannot find name 'jQuery'.
```

这时，我们需要使用 declare var 来定义它的类型

```typescript
declare var jQuery: (selector: string) => any;

jQuery('#foo');
```

declare var 并没有真的定义一个变量，只是定义了全局变量 jQuery 的类型，仅仅会用于编译时的检查，在编译结果中会被删除。它编译结果是：

```typescript
jQuery('#foo');
```

一般声明文件都会单独写成一个 `xxx.d.ts` 文件

创建 `01_jQuery.d.ts`, 将声明语句定义其中, TS编译器会扫描并加载项目中所有的TS声明文件

```typescript
declare var jQuery: (selector: string) => any;
```

很多的第三方库都定义了对应的声明文件库, 库文件名一般为 `@types/xxx`, 可以在 `https://www.npmjs.com/package/package` 进行搜索

有的第三库在下载时就会自动下载对应的声明文件库(比如: webpack),有的可能需要单独下载(比如jQuery/react)

### 内置对象

JavaScript 中有很多内置对象，它们可以直接在 TypeScript 中当做定义好了的类型。

内置对象是指根据标准在全局作用域（Global）上存在的对象。这里的标准是指 ECMAScript 和其他环境（比如 DOM）的标准。

1. ECMAScript 的内置对象

> Boolean
> Number
> String
> Date
> RegExp
> Error

```typescript
/* 1. ECMAScript 的内置对象 */
let b: Boolean = new Boolean(1)
let n: Number = new Number(true)
let s: String = new String('abc')
let d: Date = new Date()
let r: RegExp = /^1/
let e: Error = new Error('error message')
b = true
// let bb: boolean = new Boolean(2)  // error
```

1. BOM 和 DOM 的内置对象

> Window
> Document
> HTMLElement
> DocumentFragment
> Event
> NodeList

```typescript
const div: HTMLElement = document.getElementById('test')
const divs: NodeList = document.querySelectorAll('div')
document.addEventListener('click', (event: MouseEvent) => {
  console.dir(event.target)
})
const fragment: DocumentFragment = document.createDocumentFragment()
```




# 基于TS的React、Vue

## React

### 官方在线文档

[官方在线文档](https://www.html.cn/create-react-app/docs/adding-typescript/)

### 创建支持TS的React项目

```text
npx create-react-app react-ts --typescript
```

### 运行项目

```text
yarn start
```

## Vue

### 创建支持TS的Vue项目

使用vue脚手架3或者4, 并选择手动指定配置

<img src="https://s1.ax1x.com/2020/05/17/YgDpxf.png" alt="vue-ts1.png" style="zoom: 50%;" /> 

<img src="https://s1.ax1x.com/2020/05/17/YgDSRP.png" alt="vue-ts2.png" style="zoom: 50%;" align="left" />



选择TS及相关的环境配置

<img src="https://s1.ax1x.com/2020/05/17/YgBzGt.png" align="left"  alt="vue-ts3.png" style="zoom:50%;" />

其它配置的选择

<img src="https://s1.ax1x.com/2020/05/17/YgBxPI.png" alt="vue-ts3.png"  />

### 运行项目

```bash
yarn serve
```



# VuePress搭建在线文档

## 在线文档

[VuePress官方在线文档](https://vuepress.vuejs.org/zh/)

## 搭建基本环境

```bash
# 将 VuePress 作为一个本地依赖安装
yarn add -D vuepress # 或者：npm install -D vuepress

# 新建一个 docs 文件夹
mkdir docs

# 新建一个 markdown 文件
echo '# Hello VuePress!' > docs/README.md

# 启动文档项目
npx vuepress dev docs

# 构建静态文件
npx vuepress build docs
  |-- docs
    |-- .vuepress
      |-- config.js
    |-- README.md
```

## 配置ts教程文档

1. 整体结构

```text
|-- dist
|-- dics
  |-- .vuepress
    |-- public
      |-- ts-logo.png
    |-- config.js
  |-- chapter1
    |-- 01_初识TS.md
    |-- 02_安装TS.md
    |-- 03_HelloWorld.md
  |-- chapter2
    |-- 1_type.md
    |-- 2_interface.md
    |-- 3_class.md
    |-- 4_function.md
    |-- 5_generic.md
    |-- 6_other.md
  |-- chapter3
    |-- 01_react.md
    |-- 02_vue.md
  |-- chapter4
    |-- README.md
  |-- README.md
|-- package.json
```

2. docs/.vuepress/config.js

```text
module.exports = {
  base: '/ts-study/', /* 基础虚拟路径 */
  dest: 'dist', /* 打包文件基础路径, 在命令所在目录下 */
  title: 'TypeScript 入门', // 标题
  description: '学习使用 TypeScript', // 标题下的描述
  themeConfig: { // 主题配置
    sidebar: [ // 左侧导航
      {
        title: '初识 TypeScript', // 标题
        collapsable: false, // 下级列表不可折叠
        children: [ // 下级列表
          'chapter1/01_初识TS',
          'chapter1/02_安装TS',
          'chapter1/03_HelloWorld'
        ]
      },
      {
        title: 'TypeScript 常用语法',
        collapsable: false,
        children: [
          'chapter2/1_type',
          'chapter2/2_interface',
          'chapter2/3_class',
          'chapter2/4_function',
          'chapter2/5_generic',
        ]
      },
    ]
  }
}
```

3. docs/README.md

```bash
---
#首页
home: true  
# 图标
heroImage: /ts-logo.png
# 按钮文本
actionText: 开始学习 →
# 按钮点击跳转路径
actionLink: /chapter1/01_初识TS
---
```

4. package.json

```json
"scripts": {
  "doc:dev": "vuepress dev docs",
  "doc:build": "vuepress build docs",
  "doc:deploy": "gh-pages -d docs/dist"
}
```

## 发布到gitpage

1. 使用git管理当前项目
2. 将打包的项目推送到gitpage

```bash
# 下载工具包
yarn add -D gh-pages
# 配置部署命令
"deploy": "gh-pages -d docs/dist"
# 执行打包命令
yarn build
# 执行部署命令
yarn deploy
```