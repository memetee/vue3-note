# Composition API（一）

## 认识Mixin

目前我们是使用组件化的方式在开发整个Vue的应用程序，但是组件和组件之间有时候会存在相同的代码逻辑，我 们希望对相同的代码逻辑进行抽取。

在Vue2和Vue3中都支持的一种方式就是使用Mixin来完成： 

- Mixin提供了一种非常灵活的方式，来分发Vue组件中的可复用功能； 
- 一个Mixin对象可以包含任何组件选项； 
- 当组件使用Mixin对象时，所有Mixin对象的选项将被 混合 进入该组件本身的选项中；





## Mixin的基本使用

![image-20250724150445999](./15_composition API(1).assets/image-20250724150445999.png)





## Mixin的合并规则

如果Mixin对象中的选项和组件对象中的选项（重名）发生了冲突，那么Vue会如何操作呢？ 

- 这里分成不同的情况来进行处理；

情况一：如果是data函数的返回值对象 

- 返回值对象默认情况下会进行合并； 
- 如果data返回值对象的属性发生了冲突，那么会保留组件自身的数据；

情况二：如果是生命周期钩子函数 

- 生命周期的钩子函数会被合并到数组中，都会被调用；

情况三：如果值为对象的选项，例如 methods、components 和 directives，将被合并为同一个对象。 

- 比如都有methods选项，并且都定义了方法，那么它们都会生效； 
- 但是如果对象的key相同，那么会取组件对象的键值对；





## 全局混入Mixin

如果组件中的某些选项，是所有的组件都需要拥有的，那么这个时候我们可以使用全局的mixin： 

- 全局的Mixin可以使用 应用app的方法 mixin 来完成注册； 
- 一旦注册，那么全局混入的选项将会影响每一个组件；

![image-20250724150510107](./15_composition API(1).assets/image-20250724150510107.png)



## extends

另外一个类似于Mixin的方式是通过extends属性： 

- 允许声明扩展另外一个组件，类似于Mixins，也和mixins一样可以使用传入的data，methods等内容；

![image-20250724150519094](./15_composition API(1).assets/image-20250724150519094.png)

在开发中extends用的非常少，在Vue2中比较推荐大家使用Mixin，而在Vue3中推荐使用Composition API。

mixin和extends只会继承export default的对象，不会继承style和template





## Options API的弊端

在Vue2中，我们编写组件的方式是Options API： 

- Options API的一大特点就是在对应的属性中编写对应的功能模块； 
- 比如data定义数据、methods中定义方法、computed中定义计算属性、watch中监听属性改变，也包括生命 周期钩子；

但是这种代码有一个很大的弊端： 

- 当我们实现某一个功能时，这个功能对应的代码逻辑会被拆分到各个属性中； 
- 当我们组件变得更大、更复杂时，逻辑关注点的列表就会增长，那么同一个功能的逻辑就会被拆分的很分散； 
- 尤其对于那些一开始没有编写这些组件的人来说，这个组件的代码是难以阅读和理解的（阅读组件的其他人）；

下面我们来看一个非常大的组件，其中的逻辑功能按照颜色进行了划分： 

- 这种碎片化的代码使用理解和维护这个复杂的组件变得异常困难，并且隐藏了潜在的逻辑问题； 
- 并且当我们处理单个逻辑关注点时，需要不断的跳到相应的代码块中；



## 大组件的逻辑分散

![image-20250724150553386](./15_composition API(1).assets/image-20250724150553386.png)

![image-20250724150600176](./15_composition API(1).assets/image-20250724150600176.png)



如果我们能将同一个逻辑关注点相关的代码收集在一起会更好。 

这就是Composition API想要做的事情，以及可以帮助我们完成的事情。 

也有人把Vue Composition API简称为VCA。





## 认识Composition API

那么既然知道Composition API想要帮助我们做什么事情，接下来看一下到底是怎么做呢？ 

- 为了开始使用Composition API，我们需要有一个可以实际使用它（编写代码）的地方； 
- 在Vue组件中，这个位置就是 setup 函数

setup其实就是组件的另外一个选项： 

- 只不过这个选项强大到我们可以用它来替代之前所编写的大部分其他选项； 
- 比如methods、computed、watch、data、生命周期等等；

接下来我们一起学习这个函数的使用： 

- 函数的参数 
- 函数的返回值





## setup函数的参数

我们先来研究一个setup函数的参数，它主要有两个参数： 

- 第一个参数：props 
- 第二个参数：context

props非常好理解，它其实就是父组件传递过来的属性会被放到props对象中，我们在setup中如果需要使用，那么就可 以直接通过props参数获取： 

- 对于定义props的类型，我们还是和之前的规则是一样的，在props选项中定义； 
- 并且在template中依然是可以正常去使用props中的属性，比如message； 
- 如果我们在setup函数中想要使用props，那么不可以通过 this 去获取（后面我会讲到为什么）； 
- 因为props有直接作为参数传递到setup函数中，所以我们可以直接通过参数来使用即可；

```vue
<script>
    export default {
     //注意，props依然是要写的，并且里面的type，required等都还是生效的
    props: {
      message: {
        type: String,
        required: true
      }
    },
    /**
     * 参数一: props, 父组件传递过来属性
     */
    // setup函数有哪些参数?
    // setup函数有什么样的返回值
    // setup(props, context) {
    setup(props) {
      console.log(props.message);
    },
    methods: {
      btnClick() {
        this.$emit("")
      }
    }
  }
</script>
```

另外一个参数是context，我们也称之为是一个SetupContext，它里面包含三个属性： 

- attrs：所有的非prop的attribute(id, class, name等)； 
- slots：父组件传递过来的插槽（这个在以渲染函数返回时会有作用，后面会讲到）； 
- emit：当我们组件内部需要发出事件时会用到emit（因为我们不能访问this，所以不可以通过 this.$emit发出事件）；

```js
    setup(props, {attrs, slots, emit}) {
      console.log(props.message);	// 拿到props的值
      console.log(attrs.id, attrs.class); // 拿到attrs
      console.log(slots);	// 拿到插槽，比如父组件给我们传入了插槽，我们如果需要拿到的话这样拿
      console.log(emit);	// setup中没有this，所以emit一个事件要这样写

      return {
        title: "Hello Home",
        counter: 100
      }
    },
```





## setup函数的返回值

setup既然是一个函数，那么它也可以有返回值，它的返回值用来做什么呢？ 

- setup的返回值可以在模板template中被使用； 
- 也就是说我们可以通过setup的返回值来替代data选项；

甚至是我们可以返回一个执行函数来代替在methods中定义的方法：

![image-20250724154920070](./15_composition API(1).assets/image-20250724154920070.png)

![image-20250724155351356](./15_composition API(1).assets/image-20250724155351356.png)

如果能在setup中拿到数据，那么就不会再data中拿了

setup中返回的都可以在template中使用，包括data中的数据，或者methods中的方法等等。

但是，如果我们将 counter 在 increment 进行操作时，是否可以实现界面的响应式呢？ 

- 答案是不可以； 
- 这是因为对于一个定义的变量来说，默认情况下，Vue并不会跟踪它的变化，来引起界面的响应式操作；





## setup不可以使用this

官方关于this有这样一段描述（这段描述是我给官方提交了PR之后的一段描述）： 

- 表达的含义是this并没有指向当前组件实例； 
- 并且在setup被调用之前，data、computed、methods等都没有被解析； 
- 所以无法在setup中获取this；

![image-20250724150630363](./15_composition API(1).assets/image-20250724150630363.png)

其实在之前的这段描述是和源码有出入的（我向官方提交了PR，做出了描述的修改）： 

- 之前的描述大概含义是不可以使用this是因为组件实例还没有被创建出来； 
- 后来我的PR也有被合并到官方文档中；



## 之前关于this的描述问题

![image-20250724150638867](./15_composition API(1).assets/image-20250724150638867.png)



## 我是如何发现官方文档的错误的呢？

在阅读源码的过程中，代码是按照如下顺序执行的： 

- 调用 createComponentInstance 创建组件实例； 
- 调用 setupComponent 初始化component内部的操作； 
- 调用 setupStatefulComponent 初始化有状态的组件； 
- 在 setupStatefulComponent 取出了 setup 函数； 
- 通过callWithErrorHandling 的函数执行 setup；

从上面的代码我们可以看出， 组件的instance肯定是在执行 setup 函数之前就创建出来的。

![image-20250724150655987](./15_composition API(1).assets/image-20250724150655987.png)

![image-20250724171437616](./15_composition API(1).assets/image-20250724171437616.png)

组件实例已经创建出来后，调用的setup

![image-20250724172259657](./15_composition API(1).assets/image-20250724172259657.png)

![image-20250724173116684](./15_composition API(1).assets/image-20250724173116684.png)

![image-20250724173304934](./15_composition API(1).assets/image-20250724173304934.png)

![image-20250724173334034](./15_composition API(1).assets/image-20250724173334034.png)

一般用_来做占位



## Reactive API

如果想为在setup中定义的数据提供响应式的特性，那么我们可以使用reactive的函数：

![image-20250724150705306](./15_composition API(1).assets/image-20250724150705306.png)

那么这是什么原因呢？为什么就可以变成响应式的呢？ 

- 这是因为当我们使用reactive函数处理我们的数据之后，数据再次被使用时就会进行依赖收集； 
- 当数据发生改变时，所有收集到的依赖都是进行对应的响应式操作（比如更新界面）； 
- 事实上，我们编写的data选项，也是在内部交给了reactive函数将其编程响应式对象的；

前面说之所以不能做大响应式，是因为counter仅仅使我们定义的一个变量，没有经过vue内部的一个响应式的处理，所以，你点击的话页面识别不到依赖发生变化，也就不会更新，如果想要让这个状态被vue处理，变成响应式，应该怎么办？那就需要一个api，就是reactive,在之前的数据之所以能做到响应式，实际上也是reactive在内部对data中的状态做了处理，所以，如果想要setup中的数据也做到响应式，就需要用reactive对它做一些处理

```vue
<template>
  <div>
    Home Page
    <h2>{{message}}</h2>
    <h2>当前计数: {{state.counter}}</h2>
    <button @click="increment">+1</button>
  </div>
</template>

<script>
  import { reactive } from 'vue';

  export default {
    props: {
      message: {
        type: String,
        required: true
      }
    },
    setup() {
      const state = reactive({
        counter: 100
      })

      // 局部函数
      const increment = () => {
        state.counter++;
        console.log(state.counter);
      }

      return {
        state,
        increment
      }
    }
  }
</script>

<style scoped>

</style>
```







## Ref API

reactive API对传入的类型是有限制的，它要求我们必须传入的是一个对象或者数组类型： 

- 如果我们传入一个基本数据类型（String、Number、Boolean）会报一个警告；

![image-20250724150714943](./15_composition API(1).assets/image-20250724150714943.png)



这个时候Vue3给我们提供了另外一个API：ref API pref 会返回一个可变的响应式对象，该对象作为一个 响应式的引用 维护着它内部的值，这就是ref名称的来源； 

- 它内部的值是在ref的 value 属性中被维护的；

![image-20250724150721508](./15_composition API(1).assets/image-20250724150721508.png)

这里有两个注意事项： 

- 在模板中引入ref的值时，Vue会自动帮助我们进行解包操作，所以我们并不需要在模板中通过 ref.value 的方式 来使用； 
- 但是在 setup 函数内部，它依然是一个 ref引用， 所以对其进行操作时，我们依然需要使用 ref.value的方式；

```vue
<template>
  <div>
    Home Page
    <h2>{{message}}</h2>
    <!-- 当我们在template模板中使用ref对象, 它会自动进行解包 -->
    <h2>当前计数: {{counter}}</h2>
    <button @click="increment">+1</button>

    <show-message :message="counter"></show-message>
  </div>
</template>

<script>
  import { ref } from 'vue';

  export default {
    props: {
      message: {
        type: String,
        required: true
      }
    },
    setup() {
      // counter编程一个ref的可响应式的引用
      // counter = 100;
      let counter = ref(100);

      // 局部函数
      const increment = () => {
        counter.value++;
        console.log(counter.value);
      }

      return {
        counter,
        increment
      }
    }
  }
</script>

<style scoped>

</style>
```







## Ref自动解包

模板中的解包是浅层的解包，如果我们的代码是下面的方式：

如果我们将ref放到一个reactive的属性当中，那么在模板中使用时，它会自动解包：

![image-20250724150730959](./15_composition API(1).assets/image-20250724150730959.png)

上面这个不会进行解包，在template中必须加.value



这里不需要再template中使用.value，他会自动解包

![image-20250724150742396](./15_composition API(1).assets/image-20250724150742396.png)

注意：只有在template中会自动解包，在setup中并不会

只有reactive会自动解包，非reactive就不会解包



## 认识readonly

我们通过reactive或者ref可以获取到一个响应式的对象，但是某些情况下，我们传入给其他地方（组件）的这个 响应式对象希望在另外一个地方（组件）被使用，但是不能被修改，这个时候如何防止这种情况的出现呢？ 

- Vue3为我们提供了readonly的方法； 
- readonly会返回原生对象的只读代理（也就是它依然是一个Proxy，这是一个proxy的set方法被劫持，并且不 能对其进行修改）；

在开发中常见的readonly方法会传入三个类型的参数： 

- 类型一：普通对象； 
- 类型二：reactive返回的对象； 
- 类型三：ref的对象；





## readonly的使用

在readonly的使用过程中，有如下规则： 

- readonly返回的对象都是不允许修改的； 
- readonly接收proxy响应式的对象；
- 但是经过readonly处理的原来的对象是允许被修改的；
  - 比如 const info = readonly(obj)，info对象是不允许被修改的； 
  - 当obj被修改时，readonly返回的info对象也会被修改； 
  - 但是我们不能去修改readonly返回的对象info；

其实本质上就是readonly返回的对象的setter方法被劫持了而已

```vue
<script>
  import { reactive, ref, readonly } from 'vue';

  export default {
    setup() {
      // 1.普通对象
      const info1 = {name: "why"};
      const readonlyInfo1 = readonly(info1);

      // 2.响应式的对象reactive
      const info2 = reactive({
        name: "why"
      })
      const readonlyInfo2 = readonly(info2);

      // 3.响应式的对象ref
      const info3 = ref("why");
      const readonlyInfo3 = readonly(info3);

      const updateState = () => {
        // readonlyInfo3.value = "coderwhy"
        info3.value = "coderwhy";
      }

      return {
        updateState,
      }
    }
  }
</script>
```

一般情况下，如果传递给子组件的是一个readonly响应式对象，那么子组件不能修改，只有父组件能修改，这样会更好一些，当然，如果传给子组件的不是一个readonly响应式对象，而是一个普通对象，那么必然不能响应式。

注意：readonly返回的对象不需要解包





## readonly的应用

那么这个readonly有什么用呢？ 

- 在我们传递给其他组件数据时，往往希望其他组件使用我们传递的内容，但是不允许它们修改时，就可以使用 readonly了；

![image-20250724150807734](./15_composition API(1).assets/image-20250724150807734.png)

![image-20250724150814769](./15_composition API(1).assets/image-20250724150814769.png)

