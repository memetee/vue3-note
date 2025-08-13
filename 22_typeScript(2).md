## 类型缩小

什么是类型缩小呢？ 

- 类型缩小的英文是 Type Narrowing； 
- 我们可以通过类似于 typeof padding === "number" 的判断语句，来改变TypeScript的执行路径； 
- 在给定的执行路径中，我们可以缩小比声明时更小的类型，这个过程称之为 缩小; 
- 而我们编写的 typeof padding === "number 可以称之为 类型保护（type guards）；

常见的类型保护有如下几种： 

- typeof 
- 平等缩小（比如===、!==） 
- instanceof 
- in 
- 等等...





## typeof

在 TypeScript 中，检查返回的值typeof是一种类型保护：因为 TypeScript 对如何typeof操作不同的值进行编码。

```ts
type ID = number | string
function printId(id: ID) {
    if (typeof id === 'string') {
        // 类型保护
        console.log(id.toUpperCase())
    } else {
        console.log(id)
    }
}
```





## 平等缩小

我们可以使用Switch或者相等的一些运算符来表达相等性（比如===, !==, ==, and != ）：

```ts
type Direction = 'left' | 'right' | 'center'
function turnDirection(direction: Direction) {
    switch(direction) {
        case 'left':
            console.log('调用left方法')
            break;
        case 'right':
            console.log('调用right方法')
            break;
        case 'center':
            console.log('调用center方法')
            break;
        default:
            console.log('调用默认方法')
    }
}
```

当然也可以用if来写

```ts
if (direction === 'left') {
    
} else {
    
}
```





## instanceof

JavaScript 有一个运算符来检查一个值是否是另一个值的“实例”：

案例一：

```ts
function printValue(date: Date | string) {
    // 判断date是不是Date的实例
    if (date instanceof Date) {
        console.log(date.toLocaleString())
    } else {
        console.log(date)
    }
}
```

案例二：

```ts
class Student {
    studying() {}
}
class Teacher {
    teaching() {}
}

function work(p: Student | Teacher) {
    if(p instanceof Student) {
        p.studying()
    } else {
        p.teaching()
    }
}
```





## in

Javascript 有一个运算符，用于确定对象是否具有带名称的属性：in运算符

- 如果指定的属性在指定的对象或其原型链中，则`in`运算符返回true；

```ts
type Fish = { swim: () => void }
type Dog = { run: () => void }

function move(animal: Fish | Dog ) {
    // 判断swim有没有在animal里面
    if ('swim' in animal){
        animal.swim()
    } else {
        animal.run()
    }
}
```





## TypeScript函数类型

在JavaScript开发中，函数是重要的组成部分，并且函数可以作为一等公民（可以作为参数，也可以作为返回值进行传递）。 

那么在使用函数的过程中，函数是否也可以有自己的类型呢？ 

- 我们可以编写函数类型的表达式（Function Type Expressions），来表示函数类型；



```ts
// 这个就是函数类型
type CalcFunc = () => void
type CalcFunc = (num1: number, num2: number) => void
```



案例：

```ts
// 这里可以写void的话，返回任何东西都是可以的，但是我们下面实际上应该返回的是number，会更严谨一些
type CalcFunc = (num1: number, num2: number) => void

function calc(fn: CalcFunc) {
    console.log(fn(20, 30))
}

funciton sum(num1: number, num2: number) {
    return num1 + num2
}

function mul(num1: number, num2: number) {
    return num1 * num2
}

calc(sum)
calc(mul)
```





## TypeScript函数类型解析

在上面的语法中 (num1: number, num2: number) => void，代表的就是一个函数类型： 

- 接收两个参数的函数：num1和num2，并且都是number类型； 
- 并且这个函数是没有返回值的，所以是void；

在某些语言中，可能参数名称num1和num2是可以省略，但是TypeScript是不可以的：

![image-20250811164615672](./assets/22_typeScript(2).assets/image-20250811164615672.png)





## 参数的可选类型

我们可以指定某个参数是可选的：

```ts
function foo(x: number, y?: number) {
    console.log(x, y)
}
```

这个时候这个参数x依然是有类型的，它是什么类型呢？

number | undefined（联合类型）

![image-20250811164629803](./assets/22_typeScript(2).assets/image-20250811164629803.png)

另外可选类型需要在必传参数的后面：

![image-20250811164635142](./assets/22_typeScript(2).assets/image-20250811164635142.png)





## 默认参数

从ES6开始，JavaScript是支持默认参数的，TypeScript也是支持默认参数的：

```ts
// 这个顺序可以不固定，但是默认值参数一般放在最后面
function foo(x: number, y: number = 6) {
    console.log(x, y)
}
foo(10)
```

这个时候y的类型其实是 undefined 和 number 的联合类型。





## 剩余参数

从ES6开始，JavaScript也支持剩余参数，剩余参数语法允许我们将一个不定数量的参数放到一个数组中。

```ts
function sum(...nums: number[]) {
    let total = 0
    for(const num of nums) {
        total += num
    }
    return total
}

const result1 = sum(10, 20, 30)
console.log(result1)

const result2 = sum(10, 20, 30, 40)
console.log(result2)
```





## 可推导的this类型

this是JavaScript中一个比较难以理解和把握的知识点： 

- 我在公众号也有一篇文章专门讲解this：https://mp.weixin.qq.com/s/hYm0JgBI25grNG_2sCRlTA； p因为this在不同的情况下会绑定不同的值，所以对于它的类型就更难把握了；

那么，TypeScript是如何处理this呢？我们先来看一个例子：

```ts
const info = {
    name: 'wts',
    sayHello() {
        console.log(this.name)
    }
}
info.sayHello()
```

上面的代码是可以正常运行的，也就是TypeScript在编译时，认为我们的this是可以正确去使用的： 

- TypeScript认为函数 sayHello 有一个对应的this的外部对象 info，所以在使用时，就会把this当做该对象。



![image-20210725121037863](C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725121037863.png)





## 不确定的this类型

但是对于某些情况来说，我们并不知道this到底是什么？

```ts
function sayHello () {
    console.log(this.name)
}
const info = {
    name: 'wts',
    sayHello
}
info.sayHello()
```

这段代码运行会报错的： 

- 这里我们再次强调一下，TypeScript进行类型检测的目的是让我们的代码更加的安全； 
- 所以这里对于 sayHello 的调用来说，我们虽然将其放到了info中，通过info去调用，this依然是指向info对象的； 
- 但是对于TypeScript编译器来说，这个代码是非常不安全的，因为我们也有可能直接调用函数，或者通过别的对象来 调用函数；





## 指定this的类型

这个时候，通常TypeScript会要求我们明确的指定this的类型：

```ts
type NameType = {
    name: string
}

function sayHello(this: NameType) {
    console.log(this.name)
}
```

![image-20210725121453645](C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725121453645.png)





## 函数的重载

在TypeScript中，如果我们编写了一个add函数，希望可以对字符串和数字类型进行相加，应该如何编写呢？ 

我们可能会这样来编写，但是其实是错误的：

![image-20250811164714025](./assets/22_typeScript(2).assets/image-20250811164714025.png)

那么这个代码应该如何去编写呢？ 

- 在TypeScript中，我们可以去编写不同的重载签名（overload signatures）来表示函数可以以不同的方式进行 调用； 
- 一般是编写两个或者以上的重载签名，再去编写一个通用的函数以及实现；





## sum函数的重载

比如我们对sum函数进行重构： 

- 在我们调用sum的时候，它会根据我们传入的参数类型来决定执行函数体时，到底执行哪一个函数的重载签名；

```ts
function sum(a1: number, a2: number): number;
function sum(a1: string, a2: string): string;
function sum(a1: any, a2: any): any {
    return a1 + a2
}

console.log(sum(20, 30))
console.log(sum('aaa', 'bbb'))
```

但是注意，有实现提的函数，是不能直接被调用的：

```ts
sum({name: 'wts'}, {age: 18})
```





## 联合类型和重载

我们现在有一个需求：定义一个函数，可以传入字符串或者数组，获取它们的长度。 

这里有两种实现方案： 

- 方案一：使用联合类型来实现；

```ts 
function getLength(a: string | any[]) {
    return a.length
}
```

- 方案二：实现函数重载来实现；

```ts
function getLength(a: string): number;
function getLength(a: any[]): number;
function getLength(a: any) {
    return a.length
}
```





在开发中我们选择使用哪一种呢？ 

- 在可能的情况下，尽量选择使用联合类型来实现；

![image-20210725172434918](C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725172434918.png)

![image-20210725173357335](C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725173357335.png)

![image-20210725173528169](C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725173528169.png)

![image-20210725173846584](C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725173846584.png)

![image-20210725174921646](C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725174921646.png)





## 认识类的使用

在早期的JavaScript开发中（ES5）我们需要通过函数和原型链来实现类和继承，从ES6开始，引入了class关键字，可以 更加方便的定义和使用类。 

TypeScript作为JavaScript的超集，也是支持使用class关键字的，并且还可以对类的属性和方法等进行静态类型检测。 

实际上在JavaScript的开发过程中，我们更加习惯于函数式编程： 

- 比如React开发中，目前更多使用的函数组件以及结合Hook的开发模式； 
- 比如在Vue3开发中，目前也更加推崇使用 Composition API； 

但是在封装某些业务的时候，类具有更强大封装性，所以我们也需要掌握它们。

类的定义我们通常会使用class关键字： 

- 在面向对象的世界里，任何事物都可以使用类的结构来描述； 
- 类中包含特有的属性和方法





## 类的定义

我们来定义一个Person类： 

使用class关键字来定义一个类； 

我们可以声明一些类的属性：在类的内部声明类的属性以及对应的类型

- 如果类型没有声明，那么它们默认是any的； 
- 我们也可以给属性设置初始化值； 
- 在默认的strictPropertyInitialization模式下面我们的属性是必须初始 化的，如果没有初始化，那么编译时就会报错；
  - 如果我们在strictPropertyInitialization模式下确实不希望给属性初 始化，可以使用 name!: string语法；

类可以有自己的构造函数constructor，当我们通过new关键字创建一个 实例时，构造函数会被调用； 

- 构造函数不需要返回任何值，默认返回当前创建出来的实例；

类中可以有自己的函数，定义的函数称之为方法；

```ts
class Person {
    name!: string
    age: number
    constructor(name: string, age: number) {
        // this.name = name
        this.age = age
    }
    
    running() {
        console.log(this.name + ' running')
    }
    
    eating() {
        console.log(this.name + ' eating')
    }
}
```





## 类的继承

面向对象的其中一大特性就是继承，继承不仅仅可以减少我们的代码量，也是多态的使用前提。 

我们使用extends关键字来实现继承，子类中使用super来访问父类。 

我们来看一下Student类继承自Person：

- Student类可以有自己的属性和方法，并且会继承Person的属性和方法； 
- 在构造函数中，我们可以通过super来调用父类的构造方法，对父类中的属性进行初始化；

```ts
class Student extends Person {
    sno: number
    constructor(name: string, age: number, sno: number) {
        super(name, age)
        this.sno = sno
    }
    studying() {
        console.log(this.name + ' studying')
    }
    eating() {
        console.log('student eating')
    }
    running() {
        super.running();
        console.log('student running')
    }
}
```





1、

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725200504412.png" alt="image-20210725200504412" style="zoom:50%;" />



2、

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725200654232.png" alt="image-20210725200654232" style="zoom:50%;" />

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725200551437.png" alt="image-20210725200551437" style="zoom:50%;" />



类的多态

![image-20210725204951487](C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725204951487.png)

![image-20210725205108366](C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725205108366.png)





## 类的成员修饰符

在TypeScript中，类的属性和方法支持三种修饰符： public、private、protected 

- public 修饰的是在任何地方可见、公有的属性或方法，默认编写的属性就是public的； 
- private 修饰的是仅在同一类中可见、私有的属性或方法；
- protected 修饰的是仅在类自身及子类中可见、受保护的属性或方法；

public是默认的修饰符，也是可以直接访问的，我们这里来演示一下protected和private。

```ts
class Person {
    protected name: string
    constructor(name: string) {
        this.name = name;
    }
}

class Student extends Person {
    constructor(name: string) {
        super(name)
    }
    running() {
        console.log(this.name + ' running')
    }
}
```



```ts
class Person {
    private name: string
    
    constructor(name: string) {
        this.name = name
    }
}

const p = new Person('wts')
// Property 'name' is private and only accessible within
// console.log(p.name)
```



<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725210553103.png" alt="image-20210725210553103" style="zoom:50%;" />

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725210739883.png" alt="image-20210725210739883" style="zoom:50%;" />

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725211257753.png" alt="image-20210725211257753" style="zoom:50%;" />

![image-20210725212502755](C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725212502755.png)





## 只读属性readonly

如果有一个属性我们不希望外界可以任意的修改，只希望确定值后直接使用，那么可以使用readonly：

```ts
class Person {
    readonly name: string
    constructor(name: string) {
        this.name = name
    }
}

const p = new Person('wts')
console.log(p.name)
// Cannot assign to 'name' because it is a read-only property.
// p.name = 'coderwts'

export {}
```



![image-20210725213505626](C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725213505626.png)





## getters/setters

在前面一些私有属性我们是不能直接访问的，或者某些属性我们想要监听它的获取(getter)和设置(setter)的过程， 这个时候我们可以使用存取器。

```ts
class Person {
    private _name: string
    
    set name(newName) {
        this._name = newName
    }
    get name() {
        return this._name
    }
    constructor(name: string) {
        this.name = name
    }
}

const p = new Person('wts')
p.name = 'coderWts'
console.log(p.name)
```



<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725215018727.png" alt="image-20210725215018727" style="zoom: 50%;" />





## 静态成员

前面我们在类中定义的成员和方法都属于对象级别的, 在开发中, 我们有时候也需要定义类级别的成员和方法。 

在TypeScript中通过关键字static来定义：

```ts
class Student {
    static time: string = '20:00'
    
    static attendClass() {
        console.log('去上课')
    }
}

console.log(Student.time)
Student.attendClass()
```

![image-20210725215448627](C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725215448627.png)





## 抽象类abstract

我们知道，继承是多态使用的前提。 

- 所以在定义很多通用的调用接口时, 我们通常会让调用者传入父类，通过多态来实现更加灵活的调用方式。 
- 但是，父类本身可能并不需要对某些方法进行具体的实现，所以父类中定义的方法,，我们可以定义为抽象方法。

什么是 抽象方法? 在TypeScript中没有具体实现的方法(没有方法体)，就是抽象方法。 

- 抽象方法，必须存在于抽象类中； 
- 抽象类是使用abstract声明的类；

抽象类有如下的特点： 

- 抽象类是不能被实例的话（也就是不能通过new创建） 
- 抽象方法必须被子类实现，否则该类必须是一个抽象类；





## 抽象类演练

```ts
abstract class Shape {
	abstract getArea(): number
}

class Circle extends Shape {
    private r: number
    constructor(r: number) {
        super()
        this.r = r
    }
    
    getArea() {
        return this.r * this.r * 3.14
    }
}

class Rectangle extends Shape {
    private width: number
    private height: number
    
    constructor(width: number, height: number) {
        super()
        this.width = width
        this.height = height
    } 
    getArea() {
        return this.width * this.height
    }
}

const circle = new Circle(10)
const rectangle = new Rectangle(20, 30)

function calcArea(shape: Shape) {
    console.log(shape.getArea())
}
calcArea(circle)
calcArea(rectangle)
```





```ts

function makeArea(shape: Shape) {
  return shape.getArea()
}


// 用抽象类的目的是，让makeArea的参数shape的类型是Shape，也就是说，如果想用这个方法，必须继承自Shape下面这个类，这样就实现了，不是所有参数都接受，更安全
abstract class Shape {    //抽象类  抽象类不能被实例化    new Shape()  就会报错
  abstract getArea(): number    //抽象方法，可以没有函数体，抽象方法必须在抽象类里面  抽象类里面的方法必须被子类实现
}


class Rectangle extends Shape {
  private width: number
  private height: number

  constructor(width: number, height: number) {
    super()
    this.width = width
    this.height = height
  }

  getArea() {
    return this.width * this.height
  }
}

class Circle extends Shape {
  private r: number

  constructor(r: number) {
    super()
    this.r = r
  }

  getArea() {
    return this.r * this.r * 3.14
  }
}

const rectangle = new Rectangle(20, 30)
const circle = new Circle(10)

console.log(makeArea(rectangle))
console.log(makeArea(circle))
// makeArea(new Shape())

// makeArea(123)
// makeArea("123")
```

![image-20210725224129840](C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725224129840.png)





## 类的类型

类本身也是可以作为一种数据类型的：

```ts
class Person {
    name: string;
    constructor(name: string) {
        this.name = name;
    }
    running() {
        console.log(this.name + ' running');
    }
}
const p1: Person = new Person('wts')
const p2: Person = {
    name: 'kobe',
    running: function () {
        console.log(this.name + ' running')
    }
}
```



<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725224621306.png" alt="image-20210725224621306" style="zoom:50%;" />



## 接口的声明

在前面我们通过type可以用来声明一个对象类型：

```ts
type Point = {
    x: number
    y: number
}
```



对象的另外一种声明方式就是通过接口来声明：

```ts
interface Point {
    x: number
    y: number
}
```

他们在使用上的区别，我们后续再来说明。 

接下来我们继续学习一下接口的其他特性。

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725225714213.png" alt="image-20210725225714213" style="zoom:50%;" />

## 可选属性

接口中我们也可以定义可选属性：

```ts
interface Person {
    name: string
    age?: number
    friend?: {
        name: string
    }
}
const person: Person = {
    name: 'wts',
    age: 18,
    friend: {
        name: 'kobe'
    }
}
console.log(person.name)
console.log(person.friend?.name)
```





## 只读属性

接口中也可以定义只读属性： 

- 这样就意味着我们再初始化之后，这个值是不可以被修改的；

```ts
interface Person {
    readonly name: string
    age?: number
    readonly friend?: {
        name: string
    }
}

const person: Person = {
    name: 'wts',
    age: 18,
    friend: {
        name: 'kobe'
    }
}

// person.name = 'coderwts' // 不可以设置
// person.friend = {} // 不可以设置
// 下面的代码是可以执行的
if (person.friend) {
    person.friend.name = '123'
}
```





## 索引类型

前面我们使用interface来定义对象类型，这个时候其中的属性名、类型、方法都是确定的，但是有时候我们会遇 到类似下面的对象：

```ts
interface FrontLanguage {
    [index: number]: string
}
const frontend: FrontLanguage = {
    1: 'HTML',
    2: 'CSS',
    3: 'JavaScript'
}

interface LanguageBirth {
    [name: string]: number
    Java: number
}
const language: LanguageBirth = {
    'Java': 1995,
    'JavaScript': 1996,
    'C': 1972
}
```

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725230509636.png" alt="image-20210725230509636" style="zoom:50%;" />





## 函数类型

前面我们都是通过interface来定义对象中普通的属性和方法的，实际上它也可以用来定义函数类型：

```ts
interface CalcFunc {
    (num1: number, num2: number): number
}

const add: 
```



```ts
interface CalcFunc {
    (num1: number, num2: number): number
}

const add: CalcFunc = (num1, num2) => {
    return num1 + num2
}

const sub: CalcFunc = (num1, num2) => {
    return num1 - num2
}
```

当然，除非特别的情况，还是推荐大家使用类型别名来定义函数：

```ts
type CalcFunc = (num1: number, num2: number) => number
```

![image-20210725231404101](C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725231404101.png)





## 接口继承

接口和类一样是可以进行继承的，也是使用extends关键字： 

- 并且我们会发现，接口是支持多继承的（类不支持多继承）

```ts
interface Person {
    name: string
    eating: () => void
}

interface Animal {
    running: () => void
}

interface Student extends Person, Animal {
    sno: number
}

const stu: Student = {
    sno: 110,
    name: 'wts',
    eating: function () {},
    running: function () {}
}
```

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725232400420.png" alt="image-20210725232400420" style="zoom:50%;" />





## 接口的实现

接口定义后，也是可以被类实现的： 

- 如果被一个类实现，那么在之后需要传入接口的地方，都可以将这个类传入； 
- 这就是面向接口开发；

```ts
interface ISwim {
    swimming: () => void
}

interface IRun {
    running: () => void
}

class Person implements ISwim, IRun {
    swimming() {
        console.log('swimming')
    }
    running() {
        
    }
}

function swim(swimmer: ISwim) {
    swimmer.swimming()
}
const p = new Person()
swim(p)
```





## **交叉类型**

前面我们学习了联合类型： 

- 联合类型表示多个类型中一个即可

```ts
type Alignment = 'left' | 'right' | 'center'
```

还有另外一种类型合并，就是交叉类型（Intersection Types）： 

- 交叉类似表示需要满足多个类型的条件； 
- 交叉类型使用 & 符号；

我们来看下面的交叉类型： 

- 表达的含义是number和string要同时满足； 
- 但是有同时满足是一个number又是一个string的值吗？其实是没有的，所以MyType其实是一个never类型；

```ts
type MyType = number & string
```





## 交叉类型的应用

所以，在开发中，我们进行交叉时，通常是对对象类型进行交叉的：

```ts
interface Colorful {
    color: string
}

interface IRun {
    running: () => void
}

type NewType = Colorful & IRun

const obj: NewType = {
    color: 'red',
    running: function () {
        
    }
}
```





## interface和type区别

我们会发现interface和type都可以用来定义对象类型，那么在开发中定义对象类型时，到底选择哪一个呢？ 

- 如果是定义非对象类型，通常推荐使用type，比如Direction、Alignment、一些Function；

如果是定义对象类型，那么他们是有区别的： 

- interface 可以重复的对某个接口来定义属性和方法； 
- 而type定义的是别名，别名是不能重复的；

```ts
interface IPerson {
    name: string
    running: () => void
}

interface IPerson {
    age:number
}

type Person = {
    name: string
    running: () => void
}

// error:Duplicate identifier 'Person'.ts(2300)
type Person = {
    age: number
}
```





## 字面量赋值

我们来看下面的代码：

```ts
interface IPerson {
    name: string
    eating: () => void
}

// Object literal may only specify known properties, and 'age' does not exist
const p: IPerson = {
    name:'wts',
    age:18,
    eating: function () {}
}
```



```ts
interface IPerson {
    name: string
    eating: () => void
}

const obj = {
    name: 'wts',
    age: 18,
    eating: function () {}
}

const p: IPerson = obj
```



这是因为TypeScript在字面量直接赋值的过程中，为了进行类型推导会进行严格的类型限制。 

- 但是之后如果我们是将一个 变量标识符 赋值给其他的变量时，会进行freshness擦除操作。

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210726220337576.png" alt="image-20210726220337576" style="zoom:50%;" />

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210726220345735.png" alt="image-20210726220345735" style="zoom: 50%;" />

把这个对象赋值给一个变量，再传就可以



## TypeScript枚举类型

枚举类型是为数不多的TypeScript特性有的特性之一： 

- 枚举其实就是将一组可能出现的值，一个个列举出来，定义在一个类型中，这个类型就是枚举类型； 
- 枚举允许开发者定义一组命名常量，常量可以是数字、字符串类型；

```ts
enum Direction {
    LEFT,
    RIGHT,
    TOP,
    BOTTOM
}

function turnDirection(direction: Direction) {
    switch(direction) {
        case Direction.LEFT:
            console.log('转向左边~')
            break;
        case Direction.right:
            console.log('转向右边~')
            break;
        case Direction.TOP:
            console.log('转向上边~')
            break;
        case Direction.BOTTOM:
            console.log('转向下边~')
            break;
        default:
            const myDirection: never = direction
    }
}
```



<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210726221737061.png" alt="image-20210726221737061" style="zoom:50%;" />

## 枚举类型的值

枚举类型默认是有值的，比如上面的枚举，默认值是这样的： 

当然，我们也可以给枚举其他值：

- 这个时候会从100进行递增；

我们也可以给他们赋值其他的类型：

```ts
enum Direction {
    LEFT = 0,
    RIGHT = 1,
    TOP = 2,
    BOTTOM = 3
}
```



```ts
enum Direction {
    LEFT = 100,
    RIGHT,
    TOP,
    BOTTOM
}
```



```ts
enum Direction {
    LEFT,
    RIGHT,
    TOP = 'TOP',
    BOTTOM = 'BOTTOM'
}
```





## 认识泛型

软件工程的主要目的是构建不仅仅明确和一致的API，还要让你的代码具有很强的可重用性： 

- 比如我们可以通过函数来封装一些API，通过传入不同的函数参数，让函数帮助我们完成不同的操作； 
- 但是对于参数的类型是否也可以参数化呢？

什么是类型的参数化？ 

- 我们来提一个需求：封装一个函数，传入一个参数，并且返回这个参数；

如果我们是TypeScript的思维方式，要考虑这个参数和返回值的类型需要一致：

```ts
function foo(arg: number): number {
    return arg
}
```

上面的代码虽然实现了，但是不适用于其他类型，比如string、boolean、Person等类型：

```ts
function foo(arg: any): any {
    return arg
}
```





## 泛型实现类型参数化

虽然any是可以的，但是定义为any的时候，我们其实已经丢失了类型信息： 

- 比如我们传入的是一个number，那么我们希望返回的可不是any类型，而是number类型； 
- 所以，我们需要在函数中可以捕获到参数的类型是number，并且同时使用它来作为返回值的类型；

我们需要在这里使用一种特性的变量 - 类型变量（type variable），它作用于类型，而不是值：

```ts
function foo<Type>(arg: Type): Type {
    return arg
}
```



这里我们可以使用两种方式来调用它： 

- 方式一：通过 <类型> 的方式将类型传递给函数； 
- 方式二：通过类型推到，自动推到出我们传入变量的类型：
  - 在这里会推导出它们是 字面量类型的，因为字面量类型对于我们的函数也是适用的

```ts
function foo<Type>(arg: Type): Type {
    return arg
}
foo<string>('abc')
foo<number>(123)

foo('abc')
foo(123)
```





## 泛型的基本补充

当然我们也可以传入多个类型：

```ts
function foo<T, E>(a1: T, a2: E) {

}
```



平时在开发中我们可能会看到一些常用的名称： 

- T：Type的缩写，类型 
- K、V：key和value的缩写，键值对 
- E：Element的缩写，元素 
- O：Object的缩写，对象



## 泛型接口

在定义接口的时候我们也可以使用泛型：

```ts
interface IFoo<T> {
    initialValue: T,
    valueList: T[],
    handleValue: (value: T) => void
}

const foo: IFoo<number> = {
    initialValue: 0,
    valueList: [0, 1, 3],
    handleValue: function (value: number) {
        console.log(value)
    }
}
```



```ts
interface IFoo<T = number> {
    initialValue: T,
    valueList: T[],
    handleValue: (value: T) => void
}
```





## 泛型类

我们也可以编写一个泛型类：

```ts
class Point<T> {
    x: T
    y: T
    
    constructor(x: T, y: T) {
        this.x = x
        this.y = y
    }
}
const p1 = new Point(10, 20)
const p2 = new Point<number>(10, 20)
const p3: Point<number> = new Point(10, 20)
```





## 泛型约束

有时候我们希望传入的类型有某些共性，但是这些共性可能不是在同一种类型中： 

- 比如string和array都是有length的，或者某些对象也是会有length属性的； 
- 那么只要是拥有length的属性都可以作为我们的参数类型，那么应该如何操作呢？

```ts
interface ILength {
    length: number
}

function getLength<T extends ILength>(args: T) {
    return args.length
}

console.log(getLength('abc'))
console.log(getLength(['abc', 'cba']))
console.log(getLength({length: 100, name: 'wts'}))
```





## 模块化开发

TypeScript支持两种方式来控制我们的作用域： 

- 模块化：每个文件可以是一个独立的模块，支持ES Module，也支持CommonJS； 
- 命名空间：通过namespace来声明一个命名空间

```ts
export function add(num1: number, num2: number) {
    return num1 + num2
}

export function sub(num1: number, num2: number) {
    return num1 - num2
}
```





## 命名空间namespace

命名空间在TypeScript早期时，称之为内部模块，主要目的是将一个模块内部再进行作用域的划分，防止一些命名 冲突的问题。

```ts
export namespace Time {
    export function format(time: string) {
        return '2022-02-22'
    }
}

export namespace Price {
    export function format(price: number) {
        return '222.22'
    }
}
```



<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210727230226008.png" alt="image-20210727230226008" style="zoom:50%;" />

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210727230250007.png" alt="image-20210727230250007" style="zoom:50%;" />









## 类型的查找

之前我们所有的typescript中的类型，几乎都是我们自己编写的，但是我们也有用到一些其他的类型：

```ts
const imageEl = document.getElementById('image') as HTMLImageElement
```

大家是否会奇怪，我们的HTMLImageElement类型来自哪里呢？甚至是document为什么可以有getElementById的方 法呢？ 

- 其实这里就涉及到typescript对类型的管理和查找规则了。

我们这里先给大家介绍另外的一种typescript文件：.d.ts文件 

- 我们之前编写的typescript文件都是 .ts 文件，这些文件最终会输出 .js 文件，也是我们通常编写代码的地方； 
- 还有另外一种文件 .d.ts 文件，它是用来做类型的声明(declare)。 它仅仅用来做类型检测，告知typescript我们有哪 些类型；

那么typescript会在哪里查找我们的类型声明呢？ 

- 内置类型声明； 
- 外部定义类型声明； 
- 自己定义类型声明；



下载axios

`npm install axios`

![image-20210728224956360](C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210728224956360.png)

调这个接口

![image-20210728225047873](C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210728225047873.png)

下载lodash

`npm install lodash`

![image-20210728225208523](C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210728225208523.png)

这里会报错

原因是我们没有声明过lodash   就像 `codeyWhy.getElementById` 我们有声明过codeywhy吗？没有，所以过不了编译

那么为什么axios可以，但是lodash不行呢？

那么如何声明呢？





## 内置类型声明

内置类型声明是typescript自带的、帮助我们内置了JavaScript运行时的一些标准化API的声明文件； 

- 包括比如Math、Date等内置类型，也包括DOM API，比如Window、Document等；

内置类型声明通常在我们安装typescript的环境中会带有的； 

- https://github.com/microsoft/TypeScript/tree/main/lib

document.getElement类型

![image-20210729222439630](C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210729222439630.png)



## 外部定义类型声明和自定义声明

外部类型声明通常是我们使用一些库（比如第三方库）时，需要的一些类型声明。 

这些库通常有两种类型声明方式： 

方式一：在自己库中进行类型声明（编写.d.ts文件），比如axios 

方式二：通过社区的一个公有库DefinitelyTyped存放类型声明文件

- 该库的GitHub地址：https://github.com/DefinitelyTyped/DefinitelyTyped/ 
- 该库查找声明安装方式的地址：https://www.typescriptlang.org/dt/search?search= 
- 比如我们安装react的类型声明： npm i @types/react --save-dev

什么情况下需要自己来定义声明文件呢？ 

- 情况一：我们使用的第三方库是一个纯的JavaScript库，没有对应的声明文件；比如lodash 
- 情况二：我们给自己的代码中声明一些类型，方便在其他地方直接进行使用；

axios在这里声明的

![image-20210729222915424](C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210729222915424.png)

安装lodash的声明文件

![image-20210729223833946](C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210729223833946.png)

![image-20210729223935891](C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210729223935891.png)



删除掉自己定义

![image-20210729224344426](C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210729224344426.png)

![image-20210729224609314](C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210729224609314.png)



![image-20210729224701897](C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210729224701897.png)



![image-20210729225839056](C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210729225839056.png)

![image-20210729225913451](C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210729225913451.png)



这样才能认识

![image-20210729225957490](C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210729225957490.png)

![image-20210729230217900](C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210729230217900.png)



![image-20210729231204629](C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210729231204629.png)



![image-20210730223954978](C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210730223954978.png)



![image-20210730224127107](C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210730224127107.png)

![image-20210730224337145](C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210730224337145.png)

命名空间和模块是有区别的，比如我们在这里声明的命名空间，引用了jquery以后，是没办法直接用$的，需要先声明命名空间



## 声明变量-函数-类

```ts
let wName = 'coderwts'
let wAge = 18
let wHeight = 1.88

function wFoo() {
    console.log('wFoo')
}
function wBar() {
    console.log('wBar')
}
function Person(name, age) {
    this.name = name
    this.age = age
}
```



```ts
declare let wName: string;
declare let wAge: number;
declare let wHeight: number;

declare function wFoo(): void
declare function wBar(): void
declare class Person {
    name: string
    age: number
    
    constructor(name: string, age: number) {}
}
```





## 声明模块

我们也可以声明模块，比如lodash模块默认不能使用的情况，可以自己来声明这个模块：

```ts
declare module 'lodash' {
    export function join(args: any[]): any
}
```

声明模块的语法: declare module '模块名' {}。 

- 在声明模块的内部，我们可以通过 export 导出对应库的类、函数等；



## declare文件

在某些情况下，我们也可以声明文件： 

- 比如在开发vue的过程中，默认是不识别我们的.vue文件的，那么我们就需要对其进行文件的声明； 
- 比如在开发中我们使用了 jpg 这类图片文件，默认typescript也是不支持的，也需要对其进行声明；

```ts
declare module '*.vue' {
    import { DefineComponent } from 'vue'
    const component: DefineComponent
    
    export default component
}

declare module '*.jpg' {
    const src: string
    export default src
}
```





## declare命名空间

比如我们在index.html中直接引入了jQuery： 

- CDN地址： https://cdn.bootcdn.net/ajax/libs/jquery/3.6.0/jquery.js

我们可以进行命名空间的声明：

```ts
declare namespace $ {
    function ajax(settings: any): void
}
```

在main.ts中就可以使用了：

```ts
$.ajax({
    url: 'http://123.207.32.32:8000/home/multidata',
    success: (res: any) => {
        console.log(res)
    }
})
```





## tsconfig.json文件

tsconfig.json是用于配置TypeScript编译时的配置选项： 

- https://www.typescriptlang.org/tsconfig

我们这里讲解几个比较常见的：

![image-20250811165501110](./assets/22_typeScript(2).assets/image-20250811165501110.png)



## tsconfig.json文件

![image-20250811165508001](./assets/22_typeScript(2).assets/image-20250811165508001.png)