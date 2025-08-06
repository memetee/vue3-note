



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

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210723224811842.png" alt="image-20210723224811842" style="zoom:50%;" />

![image-20210725102508161](C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725102508161.png)





## 平等缩小

我们可以使用Switch或者相等的一些运算符来表达相等性（比如===, !==, ==, and != ）：

![image-20210723224852618](C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210723224852618.png)

![image-20210725102521520](C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725102521520.png)



## instanceof

JavaScript 有一个运算符来检查一个值是否是另一个值的“实例”：

![image-20210723224938324](C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210723224938324.png)

![image-20210725102535397](C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725102535397.png)

## in

Javascript 有一个运算符，用于确定对象是否具有带名称的属性：in运算符

- 如果指定的属性在指定的对象或其原型链中，则in 运算符返回true；

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210723225029348.png" alt="image-20210723225029348" style="zoom:50%;" />

![image-20210725102546036](C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725102546036.png)

## TypeScript函数类型

在JavaScript开发中，函数是重要的组成部分，并且函数可以作为一等公民（可以作为参数，也可以作为返回值进 行传递）。 

那么在使用函数的过程中，函数是否也可以有自己的类型呢？ 

- 我们可以编写函数类型的表达式（Function Type Expressions），来表示函数类型；

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210723225113908.png" alt="image-20210723225113908" style="zoom:50%;" />

![image-20210725104113767](C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725104113767.png)

![image-20210725104810714](C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725104810714.png)

## TypeScript函数类型解析

在上面的语法中 (num1: number, num2: number) => void，代表的就是一个函数类型： 

- 接收两个参数的函数：num1和num2，并且都是number类型； 
- 并且这个函数是没有返回值的，所以是void；

在某些语言中，可能参数名称num1和num2是可以省略，但是TypeScript是不可以的：

![image-20210723225157312](C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210723225157312.png)



## 参数的可选类型

我们可以指定某个参数是可选的：

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210723225230487.png" alt="image-20210723225230487" style="zoom:50%;" />

这个时候这个参数x依然是有类型的，它是什么类型呢？ number | undefined

![image-20210723225308092](C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210723225308092.png)

另外可选类型需要在必传参数的后面：

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210723225336827.png" alt="image-20210723225336827" style="zoom:50%;" />

![image-20210725111111039](C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725111111039.png)

## 默认参数

从ES6开始，JavaScript是支持默认参数的，TypeScript也是支持默认参数的：

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210723225415553.png" alt="image-20210723225415553" style="zoom:50%;" />

这个时候y的类型其实是 undefined 和 number 类型的联合。

![image-20210725111602337](C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725111602337.png)

## 剩余参数

从ES6开始，JavaScript也支持剩余参数，剩余参数语法允许我们将一个不定数量的参数放到一个数组中。

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210723225509746.png" alt="image-20210723225509746" style="zoom:50%;" />



![image-20210725112724098](C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725112724098.png)



函数接收多个参数

![image-20210725113758636](C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725113758636.png)



![image-20210725114328574](C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725114328574.png)



## 可推导的this类型

this是JavaScript中一个比较难以理解和把握的知识点： 

- 我在公众号也有一篇文章专门讲解this：https://mp.weixin.qq.com/s/hYm0JgBI25grNG_2sCRlTA； p因为this在不同的情况下会绑定不同的值，所以对于它的类型就更难把握了；

那么，TypeScript是如何处理this呢？我们先来看一个例子：

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210723225703644.png" alt="image-20210723225703644" style="zoom:50%;" />

上面的代码是可以正常运行的，也就是TypeScript在编译时，认为我们的this是可以正确去使用的： 

- TypeScript认为函数 sayHello 有一个对应的this的外部对象 info，所以在使用时，就会把this当做该对象。



![image-20210725121037863](C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725121037863.png)



## 不确定的this类型

但是对于某些情况来说，我们并不知道this到底是什么？

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210723225751177.png" alt="image-20210723225751177" style="zoom:50%;" />

这段代码运行会报错的： 

- 这里我们再次强调一下，TypeScript进行类型检测的目的是让我们的代码更加的安全； 
- 所以这里对于 sayHello 的调用来说，我们虽然将其放到了info中，通过info去调用，this依然是指向info对象的； 
- 但是对于TypeScript编译器来说，这个代码是非常不安全的，因为我们也有可能直接调用函数，或者通过别的对象来 调用函数；



## 指定this的类型

这个时候，通常TypeScript会要求我们明确的指定this的类型：

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210723225932560.png" alt="image-20210723225932560" style="zoom:50%;" />

![image-20210725121453645](C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725121453645.png)

## 函数的重载

在TypeScript中，如果我们编写了一个add函数，希望可以对字符串和数字类型进行相加，应该如何编写呢？ 

我们可能会这样来编写，但是其实是错误的：

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210723230016348.png" alt="image-20210723230016348" style="zoom:50%;" />

那么这个代码应该如何去编写呢？ 

- 在TypeScript中，我们可以去编写不同的重载签名（overload signatures）来表示函数可以以不同的方式进行 调用； 
- 一般是编写两个或者以上的重载签名，再去编写一个通用的函数以及实现；



## sum函数的重载

比如我们对sum函数进行重构： 

- 在我们调用sum的时候，它会根据我们传入的参数类型来决定执行函数体时，到底执行哪一个函数的重载签名；

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210723230127933.png" alt="image-20210723230127933" style="zoom:50%;" />

但是注意，有实现提的函数，是不能直接被调用的：

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210723230151669.png" alt="image-20210723230151669" style="zoom:50%;" />



## 联合类型和重载

我们现在有一个需求：定义一个函数，可以传入字符串或者数组，获取它们的长度。 

这里有两种实现方案： 

- 方案一：使用联合类型来实现； 
- 方案二：实现函数重载来实现；

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210723230235353.png" alt="image-20210723230235353" style="zoom:50%;" />

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210723230257888.png" alt="image-20210723230257888" style="zoom:50%;" />

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

![image-20210725180016724](C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725180016724.png)

## 类的继承

面向对象的其中一大特性就是继承，继承不仅仅可以减少我们的代码量，也是多态的使用前提。 

我们使用extends关键字来实现继承，子类中使用super来访问父类。 

我们来看一下Student类继承自Person：

- Student类可以有自己的属性和方法，并且会继承Person的属性和方法； 
- 在构造函数中，我们可以通过super来调用父类的构造方法，对父类中的属性进行初始化；

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725133013485.png" alt="image-20210725133013485" style="zoom:50%;" />

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725133028113.png" alt="image-20210725133028113" style="zoom:50%;" />



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

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725133124492.png" alt="image-20210725133124492" style="zoom:50%;" />

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725133135493.png" alt="image-20210725133135493" style="zoom:50%;" />

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725210553103.png" alt="image-20210725210553103" style="zoom:50%;" />

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725210739883.png" alt="image-20210725210739883" style="zoom:50%;" />

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725211257753.png" alt="image-20210725211257753" style="zoom:50%;" />

![image-20210725212502755](C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725212502755.png)



## 只读属性readonly

如果有一个属性我们不希望外界可以任意的修改，只希望确定值后直接使用，那么可以使用readonly：

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725133202694.png" alt="image-20210725133202694" style="zoom:50%;" />

![image-20210725213505626](C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725213505626.png)

## getters/setters

在前面一些私有属性我们是不能直接访问的，或者某些属性我们想要监听它的获取(getter)和设置(setter)的过程， 这个时候我们可以使用存取器。

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725133234501.png" alt="image-20210725133234501" style="zoom:50%;" />

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725133248130.png" alt="image-20210725133248130" style="zoom:50%;" />



<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725215018727.png" alt="image-20210725215018727" style="zoom: 50%;" />

## 静态成员

前面我们在类中定义的成员和方法都属于对象级别的, 在开发中, 我们有时候也需要定义类级别的成员和方法。 

在TypeScript中通过关键字static来定义：

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725133315940.png" alt="image-20210725133315940" style="zoom:50%;" />

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

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725133715763.png" alt="image-20210725133715763" style="zoom:50%;" />

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725133738156.png" alt="image-20210725133738156" style="zoom:50%;" />



<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725133753422.png" alt="image-20210725133753422" style="zoom:50%;" />



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

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725133823450.png" alt="image-20210725133823450" style="zoom:50%;" />



<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725224621306.png" alt="image-20210725224621306" style="zoom:50%;" />



## 接口的声明

在前面我们通过type可以用来声明一个对象类型：

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725133907676.png" alt="image-20210725133907676" style="zoom:50%;" />

对象的另外一种声明方式就是通过接口来声明：

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725133926401.png" alt="image-20210725133926401" style="zoom:50%;" />

他们在使用上的区别，我们后续再来说明。 

接下来我们继续学习一下接口的其他特性。

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725225714213.png" alt="image-20210725225714213" style="zoom:50%;" />

## 可选属性

接口中我们也可以定义可选属性：

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725134449769.png" alt="image-20210725134449769" style="zoom:50%;" />



## 只读属性

接口中也可以定义只读属性： 

- 这样就意味着我们再初始化之后，这个值是不可以被修改的；

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725134545299.png" alt="image-20210725134545299" style="zoom:50%;" />

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725134638923.png" alt="image-20210725134638923" style="zoom:50%;" />



## 索引类型

前面我们使用interface来定义对象类型，这个时候其中的属性名、类型、方法都是确定的，但是有时候我们会遇 到类似下面的对象：

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725134706225.png" alt="image-20210725134706225" style="zoom:50%;" />

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725134718105.png" alt="image-20210725134718105" style="zoom:50%;" />

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725230509636.png" alt="image-20210725230509636" style="zoom:50%;" />





## 函数类型

前面我们都是通过interface来定义对象中普通的属性和方法的，实际上它也可以用来定义函数类型：

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725134747897.png" alt="image-20210725134747897" style="zoom:50%;" />

当然，除非特别的情况，还是推荐大家使用类型别名来定义函数：

![image-20210725134807400](C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725134807400.png)

![image-20210725231404101](C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725231404101.png)

## 接口继承

接口和类一样是可以进行继承的，也是使用extends关键字： 

- 并且我们会发现，接口是支持多继承的（类不支持多继承）

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725134836079.png" alt="image-20210725134836079" style="zoom:50%;" />

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725134847643.png" alt="image-20210725134847643" style="zoom:50%;" />

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725232400420.png" alt="image-20210725232400420" style="zoom:50%;" />



## 接口的实现

接口定义后，也是可以被类实现的： 

- 如果被一个类实现，那么在之后需要传入接口的地方，都可以将这个类传入； 
- 这就是面向接口开发；

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725134926604.png" alt="image-20210725134926604" style="zoom: 50%;" />

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725134945761.png" alt="image-20210725134945761" style="zoom:50%;" />

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210726205556865.png" alt="image-20210726205556865" style="zoom:50%;" />

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210726205629325.png" alt="image-20210726205629325" style="zoom:50%;" />

## **交叉类型**

前面我们学习了联合类型： 

- 联合类型表示多个类型中一个即可

![image-20210725135020139](C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725135020139.png)

还有另外一种类型合并，就是交叉类型（Intersection Types）： 

- 交叉类似表示需要满足多个类型的条件； 
- 交叉类型使用 & 符号；

我们来看下面的交叉类型： 

- 表达的含义是number和string要同时满足； 
- 但是有同时满足是一个number又是一个string的值吗？其实是没有的，所以MyType其实是一个never类型；

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725135059987.png" alt="image-20210725135059987" style="zoom:50%;" />





## 交叉类型的应用

所以，在开发中，我们进行交叉时，通常是对对象类型进行交叉的：

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725135128689.png" alt="image-20210725135128689" style="zoom:50%;" />

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725233110935.png" alt="image-20210725233110935" style="zoom:50%;" />



## interface和type区别

我们会发现interface和type都可以用来定义对象类型，那么在开发中定义对象类型时，到底选择哪一个呢？ 

- 如果是定义非对象类型，通常推荐使用type，比如Direction、Alignment、一些Function；

如果是定义对象类型，那么他们是有区别的： 

- interface 可以重复的对某个接口来定义属性和方法； 
- 而type定义的是别名，别名是不能重复的；

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725135220533.png" alt="image-20210725135220533" style="zoom:50%;" />

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725135242421.png" alt="image-20210725135242421" style="zoom:50%;" />



![image-20210726210603492](C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210726210603492.png)

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210726211144468.png" alt="image-20210726211144468" style="zoom:50%;" />

![image-20210726211449038](C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210726211449038.png)



## 字面量赋值

我们来看下面的代码：

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725135320715.png" alt="image-20210725135320715" style="zoom:50%;" />

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725135332831.png" alt="image-20210725135332831" style="zoom:50%;" />

这是因为TypeScript在字面量直接赋值的过程中，为了进行类型推导会进行严格的类型限制。 

- 但是之后如果我们是将一个 变量标识符 赋值给其他的变量时，会进行freshness擦除操作。

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210726220337576.png" alt="image-20210726220337576" style="zoom:50%;" />

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210726220345735.png" alt="image-20210726220345735" style="zoom: 50%;" />

把这个对象赋值给一个变量，再传就可以



## TypeScript枚举类型

枚举类型是为数不多的TypeScript特性有的特性之一： 

- 枚举其实就是将一组可能出现的值，一个个列举出来，定义在一个类型中，这个类型就是枚举类型； 
- 枚举允许开发者定义一组命名常量，常量可以是数字、字符串类型；

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725140009393.png" alt="image-20210725140009393" style="zoom:50%;" />

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725140029949.png" alt="image-20210725140029949" style="zoom:50%;" />

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210726221737061.png" alt="image-20210726221737061" style="zoom:50%;" />

## 枚举类型的值

枚举类型默认是有值的，比如上面的枚举，默认值是这样的： 

当然，我们也可以给枚举其他值：

- 这个时候会从100进行递增；

我们也可以给他们赋值其他的类型：

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725140129925.png" alt="image-20210725140129925" style="zoom:50%;" />

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725140140044.png" alt="image-20210725140140044" style="zoom:50%;" />

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725140151768.png" alt="image-20210725140151768" style="zoom:50%;" />

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210726222016141.png" alt="image-20210726222016141" style="zoom:50%;" />

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210726222029773.png" alt="image-20210726222029773" style="zoom:50%;" />

## 认识泛型

软件工程的主要目的是构建不仅仅明确和一致的API，还要让你的代码具有很强的可重用性： 

- 比如我们可以通过函数来封装一些API，通过传入不同的函数参数，让函数帮助我们完成不同的操作； 
- 但是对于参数的类型是否也可以参数化呢？

什么是类型的参数化？ 

- 我们来提一个需求：封装一个函数，传入一个参数，并且返回这个参数；

如果我们是TypeScript的思维方式，要考虑这个参数和返回值的类型需要一致：

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725140258783.png" alt="image-20210725140258783" style="zoom:50%;" />

上面的代码虽然实现了，但是不适用于其他类型，比如string、boolean、Person等类型：

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725140321363.png" alt="image-20210725140321363" style="zoom:50%;" />



## 泛型实现类型参数化

虽然any是可以的，但是定义为any的时候，我们其实已经丢失了类型信息： 

- 比如我们传入的是一个number，那么我们希望返回的可不是any类型，而是number类型； 
- 所以，我们需要在函数中可以捕获到参数的类型是number，并且同时使用它来作为返回值的类型；

我们需要在这里使用一种特性的变量 - 类型变量（type variable），它作用于类型，而不是值：

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725140403523.png" alt="image-20210725140403523" style="zoom:50%;" />

这里我们可以使用两种方式来调用它： 

- 方式一：通过 <类型> 的方式将类型传递给函数； 
- 方式二：通过类型推到，自动推到出我们传入变量的类型：
  - 在这里会推导出它们是 字面量类型的，因为字面量类型对于我们的函数也是适用的

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725140439638.png" alt="image-20210725140439638" style="zoom:50%;" />

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725140453577.png" alt="image-20210725140453577" style="zoom:50%;" />

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210727221404339.png" alt="image-20210727221404339" style="zoom:50%;" />



<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210727222049635.png" alt="image-20210727222049635" style="zoom:50%;" />

## 泛型的基本补充

当然我们也可以传入多个类型：

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725140522458.png" alt="image-20210725140522458" style="zoom:50%;" />

平时在开发中我们可能会看到一些常用的名称： 

- T：Type的缩写，类型 
- K、V：key和value的缩写，键值对 
- E：Element的缩写，元素 
- O：Object的缩写，对象



## 泛型接口

在定义接口的时候我们也可以使用泛型：

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725140617278.png" alt="image-20210725140617278" style="zoom:50%;" />

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725140634706.png" alt="image-20210725140634706" style="zoom:50%;" />

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210727222550453.png" alt="image-20210727222550453" style="zoom:50%;" />



## 泛型类

我们也可以编写一个泛型类：

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725140702253.png" alt="image-20210725140702253" style="zoom:50%;" />

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210727223135853.png" alt="image-20210727223135853" style="zoom:50%;" />

## 泛型约束

有时候我们希望传入的类型有某些共性，但是这些共性可能不是在同一种类型中： 

- 比如string和array都是有length的，或者某些对象也是会有length属性的； 
- 那么只要是拥有length的属性都可以作为我们的参数类型，那么应该如何操作呢？

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725140741456.png" alt="image-20210725140741456" style="zoom:50%;" />

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210727223642085.png" alt="image-20210727223642085" style="zoom:50%;" />

## 模块化开发

TypeScript支持两种方式来控制我们的作用域： 

- 模块化：每个文件可以是一个独立的模块，支持ES Module，也支持CommonJS； 
- 命名空间：通过namespace来声明一个命名空间

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725140816485.png" alt="image-20210725140816485" style="zoom:50%;" />





## 命名空间namespace

命名空间在TypeScript早期时，称之为内部模块，主要目的是将一个模块内部再进行作用域的划分，防止一些命名 冲突的问题。

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725140842788.png" alt="image-20210725140842788" style="zoom:50%;" />

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210727230226008.png" alt="image-20210727230226008" style="zoom:50%;" />

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210727230250007.png" alt="image-20210727230250007" style="zoom:50%;" />









## 类型的查找

之前我们所有的typescript中的类型，几乎都是我们自己编写的，但是我们也有用到一些其他的类型：

![image-20210725140923893](C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725140923893.png)

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

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725141208769.png" alt="image-20210725141208769" style="zoom:50%;" />

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725141221240.png" alt="image-20210725141221240" style="zoom:50%;" />



## 声明模块

我们也可以声明模块，比如lodash模块默认不能使用的情况，可以自己来声明这个模块：

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725141256386.png" alt="image-20210725141256386" style="zoom:50%;" />

声明模块的语法: declare module '模块名' {}。 

- 在声明模块的内部，我们可以通过 export 导出对应库的类、函数等；



## declare文件

在某些情况下，我们也可以声明文件： 

- 比如在开发vue的过程中，默认是不识别我们的.vue文件的，那么我们就需要对其进行文件的声明； 
- 比如在开发中我们使用了 jpg 这类图片文件，默认typescript也是不支持的，也需要对其进行声明；

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725141343547.png" alt="image-20210725141343547" style="zoom:50%;" />





## declare命名空间

比如我们在index.html中直接引入了jQuery： 

- CDN地址： https://cdn.bootcdn.net/ajax/libs/jquery/3.6.0/jquery.js

我们可以进行命名空间的声明：

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725141425275.png" alt="image-20210725141425275" style="zoom:50%;" />



在main.ts中就可以使用了：

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725141447400.png" alt="image-20210725141447400" style="zoom:50%;" />



## tsconfig.json文件

tsconfig.json是用于配置TypeScript编译时的配置选项： 

- https://www.typescriptlang.org/tsconfig

我们这里讲解几个比较常见的：

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725141532093.png" alt="image-20210725141532093" style="zoom:50%;" />



## tsconfig.json文件

<img src="C:\Users\小山\AppData\Roaming\Typora\typora-user-images\image-20210725141559379.png" alt="image-20210725141559379" style="zoom: 50%;" />