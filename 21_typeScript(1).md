# TypeScript语法精讲（一）

我始终相信：任何新技术的出现都是为了解决原有技术的某个痛点。 

JavaScript是一门优秀的编程语言吗？

- 每个人可能观点并不完全一致，但是从很多角度来看，JavaScript是一门非常优秀的编程语言； 
- 而且，可以说在很长一段时间内这个语言不会被代替，并且会在更多的领域被大家广泛使用；

著名的Atwood定律： 

- Stack Overflow的创立者之一的 Jeff Atwood 在2007年提出了著名的 Atwood定律。 
- any application that can be written in JavaScript, will eventually be written in JavaScript. 
- 任何可以使用JavaScript来实现的应用都最终都会使用JavaScript实现。

其实我们已经看到了，这句话正在一步步被应验： 

- Web端的开发我们一直都是使用JavaScript； 
- 移动端开发可以借助于ReactNative、Weex、Uniapp等框架实现跨平台开发； 
- 小程序端的开发也是离不开JavaScript； 
- 桌面端应用程序我们可以借助于Electron来开发； 
- 服务器端开发可以借助于Node环境使用JavaScript来开发。



## JavaScript的痛点

并且随着近几年前端领域的快速发展，让JavaScript迅速被普及和受广大开发者的喜爱，借助于JavaScript本身的 强大，也让使用JavaScript开发的人员越来越多。 

优秀的JavaScript没有缺点吗？

- 其实上由于各种历史因素，JavaScript语言本身存在很多的缺点； 
- 比如ES5以及之前的使用的var关键字关于作用域的问题； 
- 比如最初JavaScript设计的数组类型并不是连续的内存空间； 
- 比如直到今天JavaScript也没有加入类型检测这一机制；

JavaScript正在慢慢变好 

- 不可否认的是，JavaScript正在慢慢变得越来越好，无论是从底层设计还是应用层面。 
- ES6、7、8等的推出，每次都会让这门语言更加现代、更加安全、更加方便。 
- 但是知道今天，JavaScript在类型检测上依然是毫无进展（为什么类型检测如此重要，我后面会聊到）。







## 类型带来的问题

首先你需要知道，编程开发中我们有一个共识：错误出现的越早越好 

- 能在写代码的时候发现错误，就不要在代码编译时再发现（IDE的优势就是在代码编写过程中帮助我们发现错 误）。
- 能在代码编译期间发现错误，就不要在代码运行期间再发现（类型检测就可以很好的帮助我们做到这一点）。 
- 能在开发阶段发现错误，就不要在测试期间发现错误，能在测试期间发现错误，就不要在上线后发现错误。

现在我们想探究的就是如何在 代码编译期间 发现代码的错误： 

- JavaScript可以做到吗？不可以，我们来看下面这段经常可能出现的代码问题。

<img src="./assets/21_typeScript(1).assets/image-20230917202548554.png" alt="image-20230917202548554" style="zoom:33%;" />

<img src="./assets/21_typeScript(1).assets/image-20230917202607823.png" alt="image-20230917202607823" style="zoom:33%;" />



## 类型错误

这是我们一个非常常见的错误： 

- 这个错误很大的原因就是因为JavaScript没有对我们传入的参数进行任何的限制，只能等到运行期间才发现这个错误； 
- 并且当这个错误产生时，会影响后续代码的继续执行，也就是整个项目都因为一个小小的错误而深入崩溃；

当然，你可能会想：我怎么可能犯这样低级的错误呢？ 

- 当我们写像我们上面这样的简单的demo时，这样的错误很容易避免，并且当出现错误时，也很容易检查出来； 
- 但是当我们开发一个大型项目时呢？你能保证自己一定不会出现这样的问题吗？而且如果我们是调用别人的类 库，又如何知道让我们传入的到底是什么样的参数呢？

但是，如果我们可以给JavaScript加上很多限制，在开发中就可以很好的避免这样的问题了： 

- 比如我们的getLength函数中str是一个必传的类型，没有调用者没有传编译期间就会报错； 
- 比如我们要求它的必须是一个String类型，传入其他类型就直接报错； 
- 那么就可以知道很多的错误问题在编译期间就被发现，而不是等到运行时再去发现和修改；





## 类型思维的缺失

我们已经简单体会到没有类型检查带来的一些问题，JavaScript因为从设计之初就没有考虑类型的约束问题，所以 造成了前端开发人员关于类型思维的缺失： 

- 前端开发人员通常不关心变量或者参数是什么类型的，如果在必须确定类型时，我们往往需要使用各种判断验 证； 
- 从其他方向转到前端的人员，也会因为没有类型约束，而总是担心自己的代码不安全，不够健壮；

所以我们经常会说JavaScript不适合开发大型项目，因为当项目一旦庞大起来，这种宽松的类型约束会带来非常多 的安全隐患，多人员开发它们之间也没有良好的类型契约。 

- 比如当我们去实现一个核心类库时，如果没有类型约束，那么需要对别人传入的参数进行各种验证来保证我们代码的健壮性； 
- 比如我们去调用别人的函数，对方没有对函数进行任何的注释，我们只能去看里面的逻辑来理解这个函数需要传入什么参数，返回值是什么类型；



## JavaScript添加类型约束

为了弥补JavaScript类型约束上的缺陷，增加类型约束，很多公司推出了自己的方案： 

- 2014年，Facebook推出了flow来对JavaScript进行类型检查； 
- 同年，Microsoft微软也推出了TypeScript1.0版本； 
- 他们都致力于为JavaScript提供类型检查；

而现在，无疑TypeScript已经完全胜出： 

- Vue2.x的时候采用的就是flow来做类型检查； 
- Vue3.x已经全线转向TypeScript，98.3%使用TypeScript进行了重构； 
- 而Angular在很早期就使用TypeScript进行了项目重构并且需要使用TypeScript来进行开发； 
- 而甚至Facebook公司一些自己的产品也在使用TypeScript；

学习TypeScript不仅仅可以为我们的代码增加类型约束，而且可以培养我们前端程序员具备类型思维。





## 认识TypeScript

虽然我们已经知道TypeScript是干什么的了，也知道它解决了什么样的问题，但是我们还是需要全面的来认识一下TypeScript到底是什么？

我们来看一下TypeScript在GitHub和官方上对自己的定义：

- GitHub说法：TypeScript is a superset of JavaScript that compiles to clean JavaScript output. 
- TypeScript官网：TypeScript is a typed superset of JavaScript that compiles to plain JavaScript. 
- 翻译一下：TypeScript是拥有类型的JavaScript超集，它可以编译成普通、干净、完整的JavaScript代码。

怎么理解上面的话呢？ 

- 我们可以将TypeScript理解成加强版的JavaScript。 
- JavaScript所拥有的特性，TypeScript全部都是支持的，并且它紧随ECMAScript的标准，所以ES6、ES7、ES8等新语法标准，它都是 支持的； 
- 并且在语言层面上，不仅仅增加了类型约束，而且包括一些语法的扩展，比如枚举类型（Enum）、元组类型（Tuple）等； 
- TypeScript在实现新特性的同时，总是保持和ES标准的同步甚至是领先； 
- 并且TypeScript最终会被编译成JavaScript代码，所以你并不需要担心它的兼容性问题，在编译时也不需要借助于Babel这样的工具； 
- 所以，我们可以把TypeScript理解成更加强大的JavaScript，不仅让JavaScript更加安全，而且给它带来了诸多好用的好用特性；



## TypeScript的特点

官方对TypeScript有几段特点的描述，我觉得非常到位（虽然有些官方，了解一下），我们一起来分享一下：

始于JavaScript，归于JavaScript 

- TypeScript从今天数以百万计的JavaScript开发者所熟悉的语法和语义开始。使用现有的JavaScript代码，包括流行的JavaScript库， 并从JavaScript代码中调用TypeScript代码； 
- TypeScript可以编译出纯净、 简洁的JavaScript代码，并且可以运行在任何浏览器上、Node.js环境中和任何支持ECMAScript 3（或 更高版本）的JavaScript引擎中；

TypeScript是一个强大的工具，用于构建大型项目 

- 类型允许JavaScript开发者在开发JavaScript应用程序时使用高效的开发工具和常用操作比如静态检查和代码重构； 
- 类型是可选的，类型推断让一些类型的注释使你的代码的静态验证有很大的不同。类型让你定义软件组件之间的接口和洞察现有 JavaScript库的行为；

拥有先进的 JavaScript

- TypeScript提供最新的和不断发展的JavaScript特性，包括那些来自2015年的ECMAScript和未来的提案中的特性，比如异步功能和 Decorators，以帮助建立健壮的组件； 
- 这些特性为高可信应用程序开发时是可用的，但是会被编译成简洁的ECMAScript3（或更新版本）的JavaScript；



## 众多项目采用TypeScript

正是因为有这些特性，TypeScript目前已经在很多地方被应用： 

- Angular源码在很早就使用TypeScript来进行了重写，并且开发Angular也需要掌握TypeScript； 
- Vue3源码也采用了TypeScript进行重写，在前面阅读源码时我们看到大量TypeScript的语法； 
- 包括目前已经变成最流行的编辑器VSCode也是使用TypeScript来完成的； 
- 包括在React中已经使用的ant-design的UI库，也大量使用TypeScript来编写； 
- 目前公司非常流行Vue3+TypeScript、React+TypeScript的开发模式； 
- 包括小程序开发，也是支持TypeScript的；



## 前端学不懂系列

在之前deno的issue里面出现了一个问题

![image-20230917202646259](./assets/21_typeScript(1).assets/image-20230917202646259.png)





## 大前端的发展趋势

大前端是一群最能或者说最需要折腾的开发者： 

- 客户端开发者：从Android到iOS，或者从iOS到Android，到RN，甚至现在越来越多的客户端开发者接触前端 相关知识（Vue、React、Angular、小程序）； 
- 前端开发者：从jQuery到AngularJS，到三大框架并行：Vue、React、Angular，还有小程序，甚至现在也要 接触客户端开发（比如RN、Flutter）； 
- 目前又面临着不仅仅学习ES的特性，还要学习TypeScript； 
- 新框架的出现，我们又需要学习新框架的特性，比如vue3.x、react18等等；

但是每一样技术的出现都会让惊喜，因为他必然是解决了之前技术的某一个痛点的，而TypeScript真是解决了 JavaScript存在的很多设计缺陷，尤其是关于类型检测的。 

并且从开发者长远的角度来看，学习TypeScript有助于我们前端程序员培养 类型思维，这种思维方式对于完成大型项目尤为重要。



## TypeScript的编译环境

在前面我们提到过，TypeScript最终会被编译成JavaScript来运行，所以我们需要搭建对应的环境： 

- 我们需要在电脑上安装TypeScript，这样就可以通过TypeScript的Compiler将其编译成JavaScript；

![image-20230917202706103](./assets/21_typeScript(1).assets/image-20230917202706103.png)



所以，我们需要先可以先进行全局的安装：

```
# 安装命令
npm install typescript -g

# 查看版本
tsc --version
```



typescript代码

./index.ts

```ts
const message: string = 'message'
function foo(payload: string) {
  console.log(payload.length)
} 

foo('aaa')
```

转换成js代码 ```tsc index.ts```

```js
var message = 'message';
function foo(payload) {
    console.log(payload.length);
}
foo('aaa');
```

生成以后会我们的ts会报错，这是因为我们ts文件里面有message和foo，但是我们编译后的js代码中也有这样的变量名，他会冲突，解决这个冲突，需要把每一个文件变成一个模块，那么需要给每个文件加上 `export`

```ts
const message: string = 'message'
function foo(payload: string) {
  console.log(payload.length)
} 

foo('aaa')
export {}
```





## TypeScript的运行环境

如果我们每次为了查看TypeScript代码的运行效果，都通过经过两个步骤的话就太繁琐了： 

- 第一步：通过tsc编译TypeScript到JavaScript代码； 
- 第二步：在浏览器或者Node环境下运行JavaScript代码；

是否可以简化这样的步骤呢？ 

- 比如编写了TypeScript之后可以直接运行在浏览器上？ 
- 比如编写了TypeScript之后，直接通过node的命令来执行？

上面我提到的两种方式，可以通过两个解决方案来完成： 

- 方式一：通过webpack，配置本地的TypeScript编译环境和开启一个本地服务，可以直接运行在浏览器上； 
- 方式二：通过ts-node库，为TypeScript的运行提供执行环境；

方式一：webpack配置 

- 方式一在之前的TypeScript文章中我已经有写过，如果需要可以自行查看对应的文章； 
- https://mp.weixin.qq.com/s/wnL1l-ERjTDykWM76l4Ajw；





## 使用ts-node

方式二：安装ts-node

```
npm install ts-node -g
```

另外ts-node需要依赖 tslib 和 @types/node 两个包：

```
npm install tslib @types/node -g
```

现在，我们可以直接通过 ts-node 来运行TypeScript的代码：

```
ts-node math.ts
```

注意：ts-node 必须是9版本以下，否则会出现问题，就是export的问题

如果是10版本的话，先执行`npm init`初始化一个项目，然后执行`npx tsc --init`，然后修改配置

./tsconfig.json

```json
{
  // Visit https://aka.ms/tsconfig to read more about this file
  "compilerOptions": {
    // File Layout
    // "rootDir": "./src",
    // "outDir": "./dist",

    // Environment Settings
    // See also https://aka.ms/tsconfig/module
    "module": "CommonJS",
    "target": "esnext",
    "types": [],
    // For nodejs:
    // "lib": ["esnext"],
    // "types": ["node"],
    // and npm install -D @types/node

    // Other Outputs
    "sourceMap": true,
    "declaration": true,
    "declarationMap": true,

    // Stricter Typechecking Options
    "noUncheckedIndexedAccess": true,
    "exactOptionalPropertyTypes": true,

    // Style Options
    // "noImplicitReturns": true,
    // "noImplicitOverride": true,
    // "noUnusedLocals": true,
    // "noUnusedParameters": true,
    // "noFallthroughCasesInSwitch": true,
    // "noPropertyAccessFromIndexSignature": true,

    // Recommended Options
    "strict": true,
    "jsx": "react-jsx",
    "verbatimModuleSyntax": true,
    "isolatedModules": true,
    "noUncheckedSideEffectImports": true,
    "moduleDetection": "force",
    "skipLibCheck": true,
  }
}

```

这里把module设置了CommonJS

接着执行`ts-node index.ts`就可以了





## 使用webpack

先执行`npm init`初始化一个项目，然后执行`npm install webpack webpack-cli -D`下载webpack

然后配置webpack，创建webpack.config.js并配置

./webpack.config.js

```js
const path = require('path')
module.exports = {
  entry: './src/index.ts',
  output: {
    path: path.resolve(__dirname, './dist'),
    filename: 'bundle.js'
  }
}
```

命令配置

./package.json

```json
{
  "name": "demo",
  "version": "1.0.0",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "webpack"
  },
  "author": "",
  "license": "ISC",
  "description": "",
  "devDependencies": {
    "webpack": "^5.101.0",
    "webpack-cli": "^6.0.1"
  }
}

```

我们写两个ts文件

入口文件

./src/index.ts

```ts
import { sum } from './math'
const message: string= '我是message'
console.log(message)
console.log(sum(10, 20))
```

./src/math.ts

```ts
function sum(a: number, b: number): number {
  return a + b
}

export {
  sum
}
```

这个时候执行`npm run build`会告诉我们需要一个loader对ts文件做一个处理

首先需要安装ts-loader,`npm install ts-loader`在安装一个局部的ts`npm install typescript`

./webpack.config.js

```js
const path = require('path')
const HtmlWebpackPlugin = require('html-webpack-plugin')
module.exports = {
  mode: 'development',
  entry: './src/index.ts',
  output: {
    path: path.resolve(__dirname, './dist'),
    filename: 'bundle.js'
  },
  resolve: {
    extensions: ['.ts', '.js']
  },
  module: {
    rules: [
      {
        test: /\.ts$/,
        loader: 'ts-loader'
      }
    ]
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: './index.html'
    })
  ]
}
```

接着配置tsconfig.json,使用`npx tsc --init`

./tsconfig.json

```json
{
  // Visit https://aka.ms/tsconfig to read more about this file
  "compilerOptions": {
    // File Layout
    // "rootDir": "./src",
    // "outDir": "./dist",

    // Environment Settings
    // See also https://aka.ms/tsconfig/module
    "module": "nodenext",
    "target": "esnext",
    "types": [],
    // For nodejs:
    // "lib": ["esnext"],
    // "types": ["node"],
    // and npm install -D @types/node

    // Other Outputs
    "sourceMap": true,
    "declaration": true,
    "declarationMap": true,

    // Stricter Typechecking Options
    "noUncheckedIndexedAccess": true,
    "exactOptionalPropertyTypes": true,

    // Style Options
    // "noImplicitReturns": true,
    // "noImplicitOverride": true,
    // "noUnusedLocals": true,
    // "noUnusedParameters": true,
    // "noFallthroughCasesInSwitch": true,
    // "noPropertyAccessFromIndexSignature": true,

    // Recommended Options
    "strict": true,
    "jsx": "react-jsx",
    "verbatimModuleSyntax": false,
    "isolatedModules": true,
    "noUncheckedSideEffectImports": true,
    "moduleDetection": "force",
    "skipLibCheck": true,
  }
}

```

这个时候就会生成dist文件了

./dist/bundle.js

```js
(()=>{"use strict";console.log("abc")})();
```

上面就已经对ts代码做一个编译了，我们以后写了ts代码，就可以直接通过`npm run build`就能生成js代码了

但是现在有一个问题，每次编写完代码都要重新打包

安装`npm install webpack-dev-serve`,来搭建一个本地服务

给webpack指定一个模板

./index.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<body>
  
</body>
</html>
```

配置plugins

./webpack.config.js

```js
const path = require('path')
const HtmlWebpackPlugin = require('html-webpack-plugin')
module.exports = {
  mode: 'development',
  entry: './src/index.ts',
  output: {
    path: path.resolve(__dirname, './dist'),
    filename: 'bundle.js'
  },
  module: {
    rules: [
      {
        test: /\.ts$/,
        loader: 'ts-loader'
      }
    ]
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: './index.html'
    })
  ]
}
```

这样我们写的代码就能做到一个热更新了

这样我们就通过webpack搭建了ts的转换过程





## 变量的声明

我们已经强调过很多次，在TypeScript中定义变量需要指定标识符的类型。 

所以完整的声明格式如下：

- 声明了类型后TypeScript就会进行类型检测，声明的类型可以称之为类型注解；

```
var/let/const 标识符: 数据类型 = 赋值;
```



比如我们声明一个message，完整的写法如下： 

- 注意：这里的string是小写的，和String是有区别的 
- string是TypeScript中定义的字符串类型，String是ECMAScript中定义的包装类的类型

```typescript
let message: string = 'Hello World';
```

如果我们给message赋值其他类型的值，那么就会报错：

![image-20230917203435609](./assets/21_typeScript(1).assets/image-20230917203435609.png)



## tslint规范

js代码规范有esLint，ts代码也有tslint

全局安装`npm install tslint -g`

生成tslint配置`tslint --init`

到时候如果我们的代码写的有不规范的话，会给我们报一些警告的





## 声明变量的关键字

在TypeScript定义变量（标识符）和ES6之后一致，可以使用var、let、const来定义。

```ts
var myname: string = 'wts';
let myage: number = 20;
const myheight: number = 1.88;
```

当然，在tslint中并不推荐使用var来声明变量： 

- 可见，在TypeScript中并不建议再使用var关键字了，主要原因和ES6升级后let和var的区别是一样的，var是没有块级作用域的，会引起很多的问题，这里不再展开探讨。

![image-20250811163649143](./assets/21_typeScript(1).assets/image-20250811163649143.png)





## 变量的类型推导（推断）

在开发中，有时候为了方便起见我们并不会在声明每一个变量时都写上对应的数据类型，我们更希望可以通过TypeScript本身的特性帮助我们推断出对应的变量类型:

![image-20250811163716719](./assets/21_typeScript(1).assets/image-20250811163716719.png)



如果我们给message赋值123：

![image-20250811163722868](./assets/21_typeScript(1).assets/image-20250811163722868.png)



这是因为在一个变量第一次赋值时，会根据后面的赋值内容的类型，来推断出变量的类型： 

- 上面的message就是因为后面赋值的是一个string类型，所以message虽然没有明确的说明，但是依然是一个string类型；

![image-20250811163728131](./assets/21_typeScript(1).assets/image-20250811163728131.png)





## JavaScript和TypeScript的数据类型

我们经常说TypeScript是JavaScript的一个超集：

![image-20250811163753974](./assets/21_typeScript(1).assets/image-20250811163753974.png)



## JavaScript类型 – number类型

数字类型是我们开发中经常使用的类型，TypeScript和JavaScript一样，不区分整数类型（int）和浮点型 （double），统一为number类型。

```js
let num = 100;
num = 200;
num = 6.66
```

如果你学习过ES6应该知道，ES6新增了二进制和八进制的表示方法，而TypeScript也是支持二进制、八进制、十 六进制的表示：

```ts
let num = 100; // 十进制
num = 0b110; // 二进制
num = 0o555; // 八进制
num = 0xf23; // 十六进制
```





## JavaScript类型 – boolean类型

boolean类型只有两个取值：true和false，非常简单

```ts
let flag: boolean = true;
flag = false;
flag = 20 > 30;
```





## JavaScript类型 – string**类型**

string类型是字符串类型，可以使用单引号或者双引号表示：

```ts
let message: string = 'Hello World';
message = 'Hello TypeScript';
```



同时也支持ES6的模板字符串来拼接变量和字符串：

```ts
const name = 'wts';
const age = 18;
const height = 1.88;

const info = `my name is ${name}, age is ${age}, height is ${height}`;
console.log(info);
```





## JavaScript类型 – Array类型

数组类型的定义也非常简单，有两种方式：

```ts
// 类型注解：方式一
const names: string[] = ['abc', 'cba', 'cba'] // 不推荐 在jsx中有冲突<div></div> 都有尖括号不知道怎么解析了

// 类型注解：方式二
const names2: Array<string> = ['abc', 'cba', 'nba'] // 推荐写法

names.push('wts')
names2.push('wts')
```

在数组中存放不同的数据类型是非常不好的习惯，在数组中存放的数据一般都是相同的数据类型

如果添加其他类型到数组中，那么会报错：

![image-20250811163849208](./assets/21_typeScript(1).assets/image-20250811163849208.png)





## JavaScript类型 – Object类型

对象类型如果我们不写类型注解，默认会推导出来

```ts
const myInfo = {
    name: 'wts',
    age: 18,
    height: 1.88
}
```

推导出来的类型就是该对象

当然也可以给它指定一个类型

```ts
const myInfo: object = {
    name: 'wts',
    age: 18,
    height: 1.88
}
```

object对象类型可以用于描述一个对象，但是从myinfo中我们不能获取数据，也不能设置数据：

![image-20250813085712473](./assets/21_typeScript(1).assets/image-20250813085712473.png)

这也是因为name这个属性不存在object类型中，所以最好我们让他自己推导，当然，我们也可以使用其他方式，例如as，但是后面再说







## JavaScript类型 – Symbol类型

在ES5中，如果我们是不可以在对象中添加相同的属性名称的，比如下面的做法：

```ts
const person = {
    identity: '程序员',
    identity: '老师'
}
```

通常我们的做法是定义两个不同的属性名字：比如identity1和identity2。 

但是我们也可以通过symbol来定义相同的名称，因为Symbol函数返回的是不同的值：

```ts
const s1: symbol = Symbol('title')
const s2: symbol = Symbol('title')

const person = {
    [s1]: '程序员',
    [s2]: '老师'
}
```





## JavaScript类型 – null和undefined类型

在 JavaScript 中，undefined 和 null 是两个基本数据类型。 

在TypeScript中，它们各自的类型也是undefined和null，也就意味着它们既是实际的值，也是自己的类型：

```ts
let n: null = null
let u: undefined = undefined
```

早期如果写的是`let n = null`，这个n的类型会被推导为any，但是最新的ts已经不会这样了，最新的ts版本会推导为null





## TypeScript类型 - any类型

在某些情况下，我们确实无法确定一个变量的类型，并且可能它会发生一些变化，这个时候我们可以使用any类型（类似 于Dart语言中的dynamic类型）。

any类型有点像一种讨巧的TypeScript手段：

- 我们可以对any类型的变量进行任何的操作，包括获取不存在的属性、方法； 
- 我们给一个any类型的变量赋值任何的值，比如数字、字符串的值；

```ts
let a: any = 'wts';
a = 123;
a = true;
const aArray: any[] = ['wts', 18, 1.88];
```

但是上述情况下使用any，实际上是不安全的。

如果对于某些情况的处理过于繁琐不希望添加规定的类型注解，或者在引入一些第三方库时，缺失了类型注解，这个时候 我们可以使用any：

- 包括在Vue源码中，也会使用到any来进行某些类型的适配；







## TypeScript类型 - unknown类型

unknown是TypeScript中比较特殊的一种类型，它用于描述类型不确定的变量。 

什么意思呢？我们来看下面的场景：

```ts
function foo(): string {
    return 'foo'
}

function bar(): number {
    return 123
}
const flag = true;
let result
if (flag) {
    result = foo()
} else {
    result = bar()
}
console.log(result)
```

问？result在初始的时候我们给它什么类型？（除了any）

我们可能不知道foo和bar返回的数据是什么类型，所以我们可以使用unknown类型

```ts
const flag = true;
let result: unknown
if (flag) {
    result = foo()
} else {
    result = bar()
}
if (typeof result === 'string') {
    console.log(result.length)
}

export {}
```

最好不要随意使用any

之所以出现unknown是因为以前的any，容易拿到其他地方滥用

注意：

- unknown类型只能赋值给any类型和unknown类型
  - 否则报错：Type 'unknown' is not assignable to type 'string'.
- any类型可以赋值给任意类型





## TypeScript类型 - void类型



void通常用来指定一个函数是没有返回值的，那么它的返回值就是void类型： 

- 我们可以将null和undefined赋值给void类型，也就是函数可以返回null或者undefined

```ts
function sum(num1: number, num2: number) {
    console.log(num1 + num2)
}
```

![image-20250813094321803](./assets/21_typeScript(1).assets/image-20250813094321803.png)

这个函数我们没有写任何类型，那么它默认返回值的类型就是void的，我们也可以显示的来指定返回值是void：

```ts
function sum(num1: number, num2: number): void {
    console.log(num1 + num2)
}
```

一般在开发中是不写的





## TypeScript类型 - never类型

never 表示永远不会发生值的类型，比如一个函数： 

- 如果一个函数中是一个死循环或者抛出一个异常，那么这个函数会返回东西吗？ 
- 不会，那么写void类型或者其他类型作为返回值类型都不合适，我们就可以使用never类型；

```ts
// 这个函数永远都没有返回结果
function loopFun(): never {
    while(true) {
        console.log('123')
    }
}

// 这个函数没有返回结果
function loopErr(): never {
    throw new Error()
}
```



never的应用场景：

有这样一个函数

```ts
function handleMessage(message: number | string) {
    switch(typeof message) {
        case 'string':
            console.log('foo')
            break;
        case 'number':
            console.log('bar')
            break;
    }
}
handleMessage('abc')
handleMessage(123)
```

但是如果有一天我们加入了布尔类型，我们可能会改成这样

```ts
function handleMessage(message: number | string | boolean) {
    switch(typeof message) {
        case 'string':
            console.log('foo')
            break;
        case 'number':
            console.log('bar')
            break;
    }
}
handleMessage('abc')
handleMessage(123)

handleMessage(true)
```

这样是不会报错的，但是我们发现我们没有对布尔类型的处理，never就可以很好的处理这种情况，如果我们没有对boolean处理的话，会给我们提示一个错误

```ts
function handleMessage(message: number | string | boolean) {
    switch(typeof message) {
        case 'string':
            console.log('foo')
            break;
        case 'number':
            console.log('bar')
            break;
        default: 
          const check: never = message
    }
}
handleMessage('abc')
handleMessage(123)

handleMessage(true)
```

![image-20250813095400641](./assets/21_typeScript(1).assets/image-20250813095400641.png)

给我们这样一个提示之后，这个提示是说因为boolean没有处理，那么会走到default，那么never类型的check就会被复制，就会报错，我们就知道了boolean没有处理，我们再加上对boolean的代码处理，来消除这个错误

```ts
function handleMessage(message: number | string | boolean) {
  switch (typeof message) {
    case 'string':
      console.log('foo')
      break;
    case 'number':
      console.log('bar')
      break;
    case 'boolean':
      console.log('bar')
      break;
    default:
      const check: never = message
  }
}
handleMessage('abc')
handleMessage(123)

handleMessage(true)
```





## TypeScript类型 - tuple类型

tuple是元组类型，很多语言中也有这种数据类型，比如Python、Swift等。

```ts
const tInfo: [string, number, number] = ['wts', 18, 1.88];
const item1 = tInfo[0]; // wts, 并且知道类型是string类型
const item2 = tInfo[1]; // 18, 并且知道类型是number类型
```

那么tuple和数组有什么区别呢？ 

- 首先，数组中通常建议存放相同类型的元素，不同类型的元素是不推荐放在数组中。（可以放在对象或者元组 中） 
- 其次，元组中每个元素都有自己特性的类型，根据索引值获取到的值可以确定对应的类型；

```ts
const info: (string | number)[] = ['wts', 18, 1.88]
const item1 = info[0] // 不能确定类型

const tInfo: [string, number, number] = ['wts', 18, 1.88]
const item2 = tInfo[0] // 一定是string类型
```





## Tuple的应用场景

那么tuple在什么地方使用的是最多的呢？ 

- tuple通常可以作为返回的值，在使用的时候会非常的方便；

```ts
function useState<T>(state: T): [T, (newState: T) => void] {
    let currentState = state;
    const changeState = (newState: T) => {
        currentState = newState
    }
    return [currentState, changeState]
}
const [counter, setCounter] = useState(10)
```

注意：`(newState: T) => void`是一个函数类型





## 函数的参数类型

函数是JavaScript非常重要的组成部分，TypeScript允许我们指定函数的参数和返回值的类型。 

参数的类型注解 

- 声明函数时，可以在每个参数后添加类型注解，以声明函数接受的参数类型：

```ts
// 给参数加上类型注解；通常在开发中，不写返回值的类型注解（自动推导）
function greet(name: string) {
    console.log('Hello ' + name.toUpperCase())
}

// 对参数类型有限制
// Argument of type 'number' is not assignable to parameter of type 'string'
greet(123)

// 对参数个数有限制
// Expected 1 arguments, but got 2.ts(2554)
greet('abc', 'cba')
```





## 函数的返回值类型

我们也可以添加返回值的类型注解，这个注解出现在函数列表的后面：

```ts
function sum(num1: number, num2: number): number {
    return num1 + num2
}
```

和变量的类型注解一样，我们通常情况下不需要返回类型注解，因为TypeScript会根据 return 返回值推断函数的 返回类型： 

- 某些第三方库处于方便理解，会明确指定返回类型，但是这个看个人喜好；





## 匿名函数的参数

匿名函数与函数声明会有一些不同： 

- 当一个函数出现在TypeScript可以确定该函数会被如何调用的地方时； 
- 该函数的参数会自动指定类型；

```ts
// 通常情况下，在定义一个函数时，都会给参数加上类型注解的
function foo(message: string) {
    
}

const names = ['abc', 'cba', 'nba']
// item根据上下文的环境推导出来的，这个时候可以不添加类型注解，这个时候称之为上下文中的函数
names.forEach(item => {
    console.log(item.toUpperCase())
})
```

我们并没有指定item的类型，但是item是一个string类型： 

- 这是因为TypeScript会根据forEach函数的类型以及数组的类型推断出item的类型； 
- 这个过程称之为上下文类型（contextual typing），因为函数执行的上下文可以帮助确定参数和返回值的类型；





## 对象类型

如果我们希望限定一个函数接受的参数是一个对象，这个时候要如何限定呢？ 

- 我们可以使用对象类型；

```ts
function printCoordinate(point: {x: number, y: number}) {
    console.log('x坐标:', point.x)
    console.log('y坐标:', point.y)
}

printCoordinate({x: 10, y: 30})
```

在这里我们使用了一个对象来作为类型： 

- 在对象我们可以添加属性，并且告知TypeScript该属性需要是什么类型； 
- 属性之间可以使用 , 或者 ; 来分割，最后一个分隔符是可选的； 
- 每个属性的类型部分也是可选的，如果不指定，那么就是any类型；





## 可选类型

对象类型也可以指定哪些属性是可选的，可以在属性的后面添加一个?：

```ts
function printCoordinate(point: {x: number, y: number, z?: number}) {
    console.log('x坐标:', point.x)
    console.log('y坐标:', point.y)
    
    // 如果是可选类型，最好判断是否存在
    if (point.z) {
        console.log('z坐标:', point.z)
    }
}

// 可选类型表示传入和没传入都可以
printCoordinate({x: 10, y: 30})
printCoordinate({x: 10, y: 30, z: 40})
```





## 联合类型

TypeScript的类型系统允许我们使用多种运算符，从现有类型中构建新类型。 

我们来使用第一种组合类型的方法：联合类型（Union Type）

- 联合类型是由两个或者多个其他类型组成的类型； 
- 表示可以是这些类型中的任何一个值； 
- 联合类型中的每一个类型被称之为联合成员（union's members）；

```ts
function printId(id: number | string) {
    console.log('你的id是：', id)
}
printId(10)
printId('abc')
```





## 使用联合类型

传入给一个联合类型的值是非常简单的：只要保证是联合类型中的某一个类型的值即可 

- 但是我们拿到这个值之后，我们应该如何使用它呢？因为它可能是任何一种类型。 
- 比如我们拿到的值可能是string或者number，我们就不能对其调用string上的一些方法；

那么我们怎么处理这样的问题呢？ 

- 我们需要使用缩小（narrow）联合（后续我们还会专门讲解缩小相关的功能）； 
- TypeScript可以根据我们缩小的代码结构，推断出更加具体的类型；

```ts
function printId(id: number | string) {
    // 类型缩小
    if (typeof id === 'string') {
        // 确定id是string类型
        console.log('你的id是：', id.toUpperCase())
    } else {
        // 确定id是number类型
        console.log('你的id是', id)
    }
}
```





## 可选类型补充

其实上，可选类型可以看做是类型和undefined的联合类型：

可选类型：

```ts
// 让参数本身是可选的
function print(message?: string) {
    console.log(message)
}
print()
print('CoderWts')
print(undefined)
```

联合类型:

```ts
function print(message: string | undefined) {
    console.log(message)
}
print()
print('CoderWts')
print(undefined) // 注意：联合类型不能这样写，这是他们的区别
```

他们都不能传入null

```ts
// error: Argument of type 'null' is not assignable to parameter of type 'string | undefined'
print(null)
```





## 类型别名

在前面，我们通过在类型注解中编写 对象类型和联合类型，但是当我们想要多次在其他地方使用时，就要编写多次。

比如我们可以给对象类型起一个别名：

```ts
// 对象的类型别名定义，可以在多个地方复用这个类型了
type Point = {
    x: number
    y: number
}

function printPoint(point: Point) {
    console.log(point.x, point.y)
}
function sumPoint(point: Point) {
    console.log(point.x + point.y)
}
printPoint({x: 20, y: 30})
sumPoint({x: 20, y: 30})

// 联合类型的类型别名定义
type ID = number | string
function printId(id: ID) {
    console.log('您的id:', id)
}
```





## 类型断言as

有时候TypeScript无法获取具体的类型信息，这个我们需要使用类型断言（Type Assertions）。 

- 比如我们通过 document.getElementById，TypeScript只知道该函数会返回 HTMLElement ，但并不知道它具体的类型：

案例一：

```ts
// document.getElementById('my-img')获取到的只是HTMLElement的类型，这是一个大的类型
// 但是HTMLElement类型没有src属性，如果确实有一个id为my-img的img标签的话，我们可以给他断言为HTMLImageElement类型
const myEl = document.getElementById('my-img') as HTMLImageElement

// HTMLImageElement类型是有src属性的
myEl.src = '图片地址'
```

案例二：

```ts
class Person {
    
}
class Student extends Person {
    studying() {}
}
function sayHello(p: Person) {
    // 把更宽泛的类型断言为更具体的类型
    (p as Student).studying()
}
const stu = new Student()
sayHello(stu)
```

TypeScript只允许类型断言转换为更具体（as 断言的更具体的） 或者不太具体（unknown  any） 的类型版本，此规则可防止不可能的强制转换：

如果强制将string转换成number会报错

![image-20250811164245613](./assets/21_typeScript(1).assets/image-20250811164245613.png)

通过将string转成unknown（any）在转成number是可以的

```ts
const name = ('coderwts' as unknown) as number;
```

不是特殊情况不要这样做。会让代码混乱





## 非空类型断言!

当我们编写下面的代码时，在执行ts的编译阶段会报错： 

- 这是因为传入的message有可能是为undefined的，这个时候是不能执行方法的；

```ts
function printMessage(message?: string){
    // error TS2532: Object is possibly 'undefined'
    console.log(message.toUpperCase())
}

printMessage('hello')
```

![image-20250813115446668](./assets/21_typeScript(1).assets/image-20250813115446668.png)说

上面的代码编译会报错的，因为message是可选类型，就是可能有值，也可能没有值，所以表示这部分代码是不严谨的

解决方法：

方案一：类型缩小

```ts
// 让参数本身是可选的
function print(message?: string) {
    if (message) {
         console.log(message.length)
    }
}
console.log('aaa')
```



方案二：非空断言

- 我们确定传入的参数是有值的，这个时候我们可以使用非空类型断言： 
  - 非空断言使用的是 ! ，表示可以确定某个标识符是有值的，跳过ts在编译阶段对它的检测；
  
  ```ts
  function printMessage (message?: string) {
      console.log(message!.toUpperCase()) // 加上一个叹号
  }
  
  printMessage('hello')
  ```





## 可选链的使用

可选链事实上并不是TypeScript独有的特性，它是ES11（ES2020）中增加的特性： 

- 可选链使用可选链操作符 ?.； 
- 它的作用是当对象的属性不存在时，会短路，直接返回undefined，如果存在，那么才会继续执行； 
- 虽然可选链操作是ECMAScript提出的特性，但是和TypeScript一起使用更版本；

```ts
type Person = {
    name: string
    friend?: {
        name: string
        age?: number
        girlFriend?: {
            name: string
        }
    }
}

const info: Person = {
    name: 'wts',
    friend: {
        name: 'kobe',
        girlFriend: {
            name: 'lily'
        }
    }
}

console.log(info.friend.name) // 编译不通过，因为friend是可选类型可能为空
// 怎么解决呢？
// 方法一：
if (info.friend){
    console.log(info.friend.name)
}

// 方法二:
console.log(info.friend!.name)

// 方法三：
console.log(info.friend?.name) // lilys
console.log(info.friend?.age) // undefined
console.log(info.friend?.girlFriend?.name) // undefined
```





## ??和!!的作用

有时候我们还会看到 !! 和 ??操作符，这些都是做什么的呢？ 

!!操作符：

- 将一个其他类型转换成boolean类型； 
- 类似于Boolean(变量)的方式；

```ts
const message = 'Hello world'

// flag1/flag2都是布尔类型了,这是javascript的特性
let flag1 = Boolean(message)
let flag2 = !!message
```



??操作符：

- 它是ES2020增加的新特性； 
- 空值合并操作符（??）是一个逻辑操作符，当操作符的左侧是 null 或者 undefined 时，返回其右侧操作数， 否则返回左侧操作数；

```ts
const message = '321'
const result = message ?? '123'
console.log(result)
```

- 他和 `||` 是有区别的，在`||`中如果是`0`的话，他转成布尔值的话是`false`，但是在`??`中他表示是有值的





## 字面量类型

除了前面我们所讲过的类型之外，也可以使用字面量类型（literal types）：

```ts
// 这里的message1是一个string类型
let message1 = 'Hello World';

// 这里的message1是一个Hello World类型,这个就是字面量类型
const message2 = 'Hello World';

// 这里的message3是一个Hello World类型
let message3: 'Hello World' = 'Hello World'

// 这里的num就是123类型
let num: 123 = 123

// Type '"你好啊，李银河"' is not assignable to type '"Hello World"'.
message = "你好啊，李银河"
```



那么这样做有什么意义呢？ 

- 默认情况下这么做是没有太大的意义的，但是我们可以将多个类型联合在一起；

```ts
// 这里就是字面量类型组成的联合类型
type Alignment = 'left' | 'right' | 'center'
function changeAlign(align: Alignment) {
    console.log('修改方向:', align)
}

changeAlign('left')
changeAlign('right')
changeAlign('center')

// 赋值其他类型就会报错
```





## 字面量推理

我们来看下面的代码：

```ts
const info = {
    url: 'https://coderwts.org/abc',
    method: 'GET'
}
```

当前info的对象会被推理

![image-20250813161235858](./assets/21_typeScript(1).assets/image-20250813161235858.png)

我们也可以给url或者method进行赋值，但是这样会带来安全隐患

假如封装一个请求方法

```ts
const info = {
    url: 'https://coderwts.org/abc',
    method: 'GET'
}
function request(url: string, method: 'GET' | 'POST') {
    console.log(url, method)
}
request(info.url, info.method)
```

第二个参数会报错

![image-20250813161435777](./assets/21_typeScript(1).assets/image-20250813161435777.png)

这是因为我们的对象再进行字面量推理的时候，info其实是一个 {url: string, method: string}，所以我们没办法将 一个 string 赋值给一个 字面量类型。

解决方式一：

```ts
request(info.url, info.method as 'GET')
```

解决方式二：

```ts
const info: TRequest = {
    url: 'https://coderwts.org/abc',
    method: 'GET'
}

type TRequest = {
    url: string
    method: 'GET' | 'POST'
}

function request(url: string, method: 'GET' | 'POST') {
    console.log(url, method)
}
request(info.url, info.method)
```



解决方式三：

```ts
const info = {
    url: 'https://coderwts.org/abc',
    method: 'GET'
}
type TMethod = 'GET' | 'POST'

function request(url: string, method: 'GET' | 'POST') {
    console.log(url, method)
}
request(info.url, info.method as TMethod)
```



解决方法四：

```ts
const info = {
    url: 'https://coderwts.org/abc',
    method: 'GET'
} as const

function request(url: string, method: 'GET' | 'POST') {
    console.log(url, method)
}
request(info.url, info.method)
```

这种方式被称为字面量推理
