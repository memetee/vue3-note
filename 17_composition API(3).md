## 生命周期钩子

我们前面说过 setup 可以用来替代 data 、 methods 、 computed 、watch 等等这些选项，也可以替代生命周 期钩子。

那么setup中如何使用生命周期函数呢？ 

- 可以使用直接导入的 onX 函数注册生命周期钩子；

![image-20250725142051167](./assets/17_composition API(3).assets/image-20250725142051167.png)

![image-20250725142055868](./assets/17_composition API(3).assets/image-20250725142055868.png)

![image-20250725142102846](./assets/17_composition API(3).assets/image-20250725142102846.png)

这段话的意思就是beforeCreate和created中执行代码的话，直接写在setup中就行了，setup的执行实际上比beforeCreate和created还要早

不推荐在beforeCreate和created中在做一些操作了

生命周期可以写多个，都会执行，比如说可以在setup中写两个onMounted，他们两个都会执行。





## Provide函数

事实上我们之前还学习过Provide和Inject，Composition API也可以替代之前的 Provide 和 Inject 的选项。

我们可以通过 provide来提供数据： 

- 可以通过 provide 方法来定义每个 Property； 
- provide可以传入两个参数：
  - name：提供的属性名称； 
  - value：提供的属性值；

![image-20250725142113811](./assets/17_composition API(3).assets/image-20250725142113811.png)



## Inject函数

在 后代组件 中可以通过 inject 来注入需要的属性和对应的值： 

- 可以通过 inject 来注入需要的内容； 
- inject可以传入两个参数： 
  - 要 inject 的 property 的 name； 
  - 默认值；

![image-20250725142120370](./assets/17_composition API(3).assets/image-20250725142120370.png)

provide和inject都是在vue中解构出来的。

但是上面这样写，他们都不是响应式的，如果希望他们是响应式的那么怎么办呢？



> 尤大推荐，能用ref，尽量用ref，原因是reactive不方便做代码的抽离





## 数据的响应式

为了增加 provide 值和 inject 值之间的响应性，我们可以在 provide 值时使用 ref 和 reactive。

![image-20250725142133025](./assets/17_composition API(3).assets/image-20250725142133025.png)

这个时候就可以在这里修改counter和info了，子组件用的时候也会是响应式的

但是这样的做法不是特别好，因为子组件拿到info实际上是可以修改的，但是我们更希望数据流是自上而下的。

所以我们可以使用readonly api

```js
      const name = ref("coderwhy");
      let counter = ref(100);

      provide("name", readonly(name));
      provide("counter", readonly(counter));
```

当然也有其他的方式





## 修改响应式Property

如果我们需要修改可响应的数据，那么最好是在数据提供的位置来修改： 

- 我们可以将修改方法进行共享，在后代组件中进行调用；

![image-20250725142140165](./assets/17_composition API(3).assets/image-20250725142140165.png)





## useCounter

我们知道，如果想要实现一个计数器，我们需要有很多逻辑，比如methods中有增加、减少，data中要定义数据，如果有*2的需求，还需要computed中进行乘2的操作，代码最后写下来很长，而且特别分散。

我们先来对之前的counter逻辑进行抽取：

./App.vue

![image-20250725145606878](./assets/17_composition API(3).assets/image-20250725145606878.png)

这些代码可能看着还是很多

所以我们可以给封装到hook中

./hooks/useCounter.js

```js
import { ref, computed } from 'vue';

export default function() {
  const counter = ref(0);
  const doubleCounter = computed(() => counter.value * 2);

  const increment = () => counter.value++;
  const decrement = () => counter.value--;

  return {
    counter, 
    doubleCounter, 
    increment, 
    decrement
  }
}
```

接着使用这个hook

```vue
<template>
  <div>
    <h2>当前计数: {{counter}}</h2>
    <h2>计数*2: {{doubleCounter}}</h2>
    <button @click="increment">+1</button>
    <button @click="decrement">-1</button>
  </div>
</template>

<script>
  import { useCounter } from './hooks/useCounter';
  export default {
    setup() {
      // counter
      const { counter, doubleCounter, increment, decrement } = useCounter();
      return {
        counter,
        doubleCounter,
        increment,
        decrement,
      }
    }
  }
</script>

<style scoped>
</style>
```

这样可以发现，代码就简洁了很多





## useTitle

![image-20250725150311568](./assets/17_composition API(3).assets/image-20250725150311568.png)

这里是决定页面窗口上方的title名称的，但是有时候我们想要改掉这个title

随便用document.title= 不是很好的习惯

././hooks/useTitle.js

```js
import { ref, watch } from 'vue';

export default function(title = "默认的title") {
  const titleRef = ref(title);

  watch(titleRef, (newValue) => {
    document.title = newValue
  }, {
    immediate: true
  })

  return titleRef
}
```

./App.vue

```vue
<script>
  import { ref, computed } from 'vue';
  import { useTitle } from './hooks/useTitle';

  export default {
    setup() {
      // title
      const titleRef = useTitle("coderwhy");
      setTimeout(() => {
        titleRef.value = "kobe"
      }, 3000);
      return {}
    }
  }
</script>
```





## useScrollPosition

我们来完成一个监听界面滚动位置的Hook：



./App.vue

```vue
<template>
  <div>
    <div class="scroll">
      <div class="scroll-x">scrollX: {{scrollX}}</div>
      <div class="scroll-y">scrollY: {{scrollY}}</div>
    </div>
    <div class="mouse">
      <div class="mouse-x">mouseX: {{mouseX}}</div>
      <div class="mouse-y">mouseY: {{mouseY}}</div>
    </div>
  </div>
</template>

<script>
  import { ref, computed } from 'vue';

  import {
    useMousePosition,
    useScrollPosition,
  } from './hooks';

  export default {
    setup() {

      // 滚动位置
      const { scrollX, scrollY } = useScrollPosition();
      // 鼠标位置
      const { mouseX, mouseY } = useMousePosition();

      return {
        scrollX,
        scrollY,

        mouseX,
        mouseY,
      }
    }
  }
</script>
<style scoped>
  .content {
    width: 3000px;
    height: 5000px;
  }

  .scroll {
    position: fixed;
    right: 30px;
    bottom: 30px;
  }
  .mouse {
    position: fixed;
    right: 30px;
    bottom: 80px;
  }
</style>
```

./hook/useMousePosition.js

```js
import { ref } from 'vue';

export default function() {
  const mouseX = ref(0);
  const mouseY = ref(0);

  window.addEventListener("mousemove", (event) => {
    mouseX.value = event.pageX;
    mouseY.value = event.pageY;
  });

  return {
    mouseX,
    mouseY
  }
}

```

./hook/useScrollPosition.js

```js
import { ref } from 'vue';

export default function() {
  const scrollX = ref(0);
  const scrollY = ref(0);

  document.addEventListener("scroll", () => {
    scrollX.value = window.scrollX;
    scrollY.value = window.scrollY;
  });

  return {
    scrollX,
    scrollY
  }
}
```





## useLocalStorage

我们来完成一个使用 localStorage 存储和获取数据的Hook：

./hook/useLocalStorage.js

```js
import { ref, watch } from 'vue';

export default function(key, value) {
  const data = ref(value);

  if (value) {
    window.localStorage.setItem(key, JSON.stringify(value));
  } else {
    data.value = JSON.parse(window.localStorage.getItem(key));
  }

  watch(data, (newValue) => {
    window.localStorage.setItem(key, JSON.stringify(newValue));
  })

  return data;
}

// 一个参数: 取值
// const data = useLocalStorage("name");

// // 二个参数: 保存值
// const data = useLocalStorage("name", "coderwhy");

// data.value = "kobe";

```

./App.vue

```vue
<template>
  <div>
    <h2>{{data}}</h2>
    <button @click="changeData">修改data</button>
  </div>
</template>

<script>
  import {useLocalStorage} from './hooks';

  export default {
    setup() {
      // localStorage
      const data = useLocalStorage("info");
      const changeData = () => data.value = "哈哈哈哈"

      return {
        data,
        changeData
      }
    }
  }
</script>
```





## vue实验性写法

./App.vue

```vue
<template>
  <div>
    <h2>当前计数: {{counter}}</h2>
    <button @click="increment">+1</button>

    <hello-world message="呵呵呵" @increment="getCounter"></hello-world>
  </div>
</template>

<script setup>
  import { ref } from 'vue';
  import HelloWorld from './HelloWorld.vue';

  const counter = ref(0);
  const increment = () => counter.value++;

  const getCounter = (payload) => {
    console.log(payload);
  }
</script>

<style scoped>

</style>
```



./HelloWorld.vue

```vue
<template>
  <div>
    <h2>Hello World</h2>
    <h2>{{message}}</h2>
    <button @click="emitEvent">发射事件</button>
  </div>
</template>

<script setup>
  import { defineProps, defineEmit } from 'vue';

  const props = defineProps({
    message: {
      type: String,
      default: "哈哈哈"
    }
  })

  const emit = defineEmit(["increment", "decrement"]);

  const emitEvent = () => {
    emit('increment', "100000")
  }
  
</script>

<style scoped>

</style>
```

尤大实验性的,但是现在已经不是实验性了，而是推荐写法





## 认识h函数

Vue推荐在绝大数情况下使用模板来创建你的HTML，然后一些特殊的场景，你真的需要JavaScript的完全编程的 能力，这个时候你可以使用 渲染函数 ，它比模板更接近编译器； 

- 前面我们讲解过VNode和VDOM的改变： 
- Vue在生成真实的DOM之前，会将我们的节点转换成VNode，而VNode组合在一起形成一颗树结构，就是虚 拟DOM（VDOM）； 
- 事实上，我们之前编写的 template 中的HTML 最终也是使用渲染函数生成对应的VNode； 
- 那么，如果你想充分的利用JavaScript的编程能力，我们可以自己来编写 createVNode 函数，生成对应的 VNode；

那么我们应该怎么来做呢？使用 h()函数： 

- h函数是在render函数中使用的，render函数要求返回一个VNode，利用h函数生成VNode

  ```js
  export default {
    render() {
      return h(参数)
    }
  }
  ```

  

- h() 函数是一个用于创建 vnode 的一个函数； 
- 其实更准确的命名是 createVNode() 函数，但是为了简便在Vue将之简化为 h() 函数；

![image-20250725154644060](./assets/17_composition API(3).assets/image-20250725154644060.png)





## h()函数 如何使用呢？

h()函数 如何使用呢？它接受三个参数：

元素类型

![image-20250725142238372](./assets/17_composition API(3).assets/image-20250725142238372.png)

属性

![image-20250725142243112](./assets/17_composition API(3).assets/image-20250725142243112.png)

子元素

![image-20250725142249189](./assets/17_composition API(3).assets/image-20250725142249189.png)



注意事项： 

- 如果没有props，那么通常可以将children作为第二个参数传入； 
- 如果会产生歧义，可以将null作为第二个参数传入，将children作为第三个参数传入；





## h函数的基本使用

h函数可以在两个地方使用： 

- render函数选项中； 
- setup函数选项中（setup本身需要是一个函数类型，函数再返回h函数创建的VNode）；

./App.vue

```vue
<script>
  import { h } from 'vue';

  export default {
    // vue2中是通过render中接收一个h函数来使用h函数，但是vue3中h函数直接在vue中引入
    render() {
      return h("h2", {class: "title"}, "Hello Render")
    }
  }
</script>

<style scoped>

</style>
```

![image-20250725155441989](./assets/17_composition API(3).assets/image-20250725155441989.png)

注意：因为我们不需要编译template，我们自己生成VNode，所以template就可以不要了





## h函数计数器案例

./App.vue

```vue
<script>
  import { h } from 'vue';

  export default {
    data() {
      return {
        counter: 0
      }
    },
    render() {
      return h("div", {class: "app"}, [
        h("h2", null, `当前计数: ${this.counter}`),
        h("button", {
          onClick: () => this.counter++
        }, "+1"),
        h("button", {
          onClick: () => this.counter--
        }, "-1"),
      ])
    }
  }
</script>

<style scoped>

</style>
```

![image-20250725160228474](./assets/17_composition API(3).assets/image-20250725160228474.png)

render中的this指向的是vue实例，和methods中的this一样

![image-20250725160551538](./assets/17_composition API(3).assets/image-20250725160551538.png)



setup替代render

```vue
<script>
  import { ref, h } from 'vue';

  export default {
    setup() {
      const counter = ref(0);
      // render可以这样写了
      return () => {
        return h("div", {class: "app"}, [
          h("h2", null, `当前计数: ${counter.value}`),
          h("button", {
            onClick: () => counter.value++
          }, "+1"),
          h("button", {
            onClick: () => counter.value--
          }, "-1"),
        ])
      }
    }
  }
</script>

<style scoped>

</style>
```





## 函数组件和插槽的使用

./App.vue

```vue
<script>
  import { h } from 'vue';
  import HelloWorld from './HelloWorld.vue';

  export default {
    render() {
      return h("div", null, [
        h(HelloWorld, null, {
          default: props => h("span", null, `app传入到HelloWorld中的内容: ${props.name}`)
        })
      ])
    }
  }
</script>

<style scoped>

</style>
```



./HelloWrold.vue

```vue
<script>
  import { h } from "vue";

  export default {
    render() {
      return h("div", null, [
        h("h2", null, "Hello World"),
        this.$slots.default ? this.$slots.default({name: "coderwhy"}): h("span", null, "我是HelloWorld的插槽默认值")
      ])
    }
  }
</script>

<style lang="scss" scoped>

</style>
```





## jsx的babel配置

项目中通过写jsx代码后，通过babel把js代码转成render函数，就不需要写上面那种代码了

如果我们希望在项目中使用jsx，那么我们需要添加对jsx的支持： 

- jsx我们通常会通过Babel来进行转换（React编写的jsx就是通过babel转换的）； 
- 对于Vue来说，我们只需要在Babel中配置对应的插件即可；

安装Babel支持Vue的jsx插件：

```
npm install @vue/babel-plugin-jsx -D
```

在babel.config.js配置文件中配置插件:

![image-20250725142407262](./assets/17_composition API(3).assets/image-20250725142407262.png)

![image-20250725142413137](./assets/17_composition API(3).assets/image-20250725142413137.png)

![image-20250725161928625](./assets/17_composition API(3).assets/image-20250725161928625.png)

现在不需要配，也就是不需要下载npm install @vue/babel-plugin-jsx -D，现在可以直接使用，脚手架应该是已经支持了





## jsx计数器案例

![image-20250725142420658](./assets/17_composition API(3).assets/image-20250725142420658.png)





## jsx组件的使用

插槽

./HelloWorld.vue

![image-20250725142429222](./assets/17_composition API(3).assets/image-20250725142429222.png)



./App.vue

![image-20250725142435113](./assets/17_composition API(3).assets/image-20250725142435113.png)

这种代码通过babel生成了h函数，然后还是render函数在调用h函数后，生成了VNode，然后VNode生成真实dom





## 认识自定义指令

在Vue的模板语法中我们学习过各种各样的指令：v-show、v-for、v-model等等，除了使用这些指令之外，Vue 也允许我们来自定义自己的指令。 

- 注意：在Vue中，代码的复用和抽象主要还是通过组件； 
- 通常在某些情况下，你需要对DOM元素进行底层操作，这个时候就会用到自定义指令；

自定义指令分为两种： 

- 自定义局部指令：组件中通过 directives 选项，只能在当前组件中使用； 
- 自定义全局指令：app的 directive 方法，可以在任意组件中被使用；

比如我们来做一个非常简单的案例：当某个元素挂载完成后可以自定获取焦点 

- 实现方式一：如果我们使用默认的实现方式； 
- 实现方式二：自定义一个 v-focus 的局部指令； 
- 实现方式三：自定义一个 v-focus 的全局指令；





## 实现方式一：聚焦的默认实现

![image-20250725142445280](./assets/17_composition API(3).assets/image-20250725142445280.png)





## 实现方式二：局部自定义指令

实现方式二：自定义一个 v-focus 的局部指令 

- 这个自定义指令实现非常简单，我们只需要在组件选项中使用 directives 即可； 
- 它是一个对象，在对象中编写我们自定义指令的名称（注意：这里不需要加v-）； 
- 自定义指令有一个生命周期，是在组件挂载后调用的 mounted，我们可以在其中完成操作；

```vue
<template>
  <div>
    <input type="text" v-focus>
  </div>
</template>

<script>
  export default {
    // 局部指令
    directives: {
      focus: {
        mounted(el, bindings, vnode, preVnode) {
          console.log("focus mounted");
          el.focus();
        }
      }
    }
  }
</script>

<style scoped>

</style>
```





## 方式三：自定义全局指令

自定义一个全局的v-focus指令可以让我们在任何地方直接使用

```js
import { createApp } from 'vue'
import App from './03_自定义指令/App.vue'

const app = createApp(App);

app.directive("focus", {
  mounted(el, bindings, vnode, preVnode) {
    console.log("focus mounted");
    el.focus();
  }
})

app.mount('#app');

```





## 指令的生命周期

一个指令定义的对象，Vue提供了如下的几个钩子函数： 

- created：在绑定元素的 attribute 或事件监听器被应用之前调用； 
- beforeMount：当指令第一次绑定到元素并且在挂载父组件之前调用； 
- mounted：在绑定元素的父组件被挂载后调用； 
- beforeUpdate：在更新包含组件的 VNode 之前调用； 
- updated：在包含组件的 VNode 及其子组件的 VNode 更新后调用； 
- beforeUnmount：在卸载绑定元素的父组件之前调用； 
- unmounted：当指令与元素解除绑定且父组件已卸载时，只调用一次；

```vue
<template>
  <div>
    <button v-if="counter < 2" v-why.aaaa.bbbb="'coderwhy'" @click="increment">当前计数: {{counter}}</button>
  </div>
</template>

<script>
  import { ref } from "vue";

  export default {
    // 局部指令
    directives: {
      why: {
        created(el, bindings, vnode, preVnode) {
          console.log("why created", el, bindings, vnode, preVnode);
          console.log(bindings.value);
          console.log(bindings.modifiers);
        },
        beforeMount() {
          console.log("why beforeMount");
        },
        mounted() {
          console.log("why mounted");
        },
        beforeUpdate() {
          console.log("why beforeUpdate");
        },
        updated() {
          console.log("why updated");
        },
        beforeUnmount() {
          console.log("why beforeUnmount");
        },
        unmounted() {
          console.log("why unmounted");
        }
      }
    },
    setup() {
      const counter = ref(0);
      const increment = () => counter.value++;

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

![image-20250725203430524](./assets/17_composition API(3).assets/image-20250725203430524.png)





## 指令的参数和修饰符

如果我们指令需要接受一些参数或者修饰符应该如何操作呢？

- info是参数的名称；
- aaa-bbb是修饰符的名称；
- 后面是传入的具体的值；

在我们的生命周期中，我们可以通过 bindings 获取到对应的内容：

![image-20250725142516234](./assets/17_composition API(3).assets/image-20250725142516234.png)

![image-20250725142525244](./assets/17_composition API(3).assets/image-20250725142525244.png)

这里打印的是

```js
        created(el, bindings, vnode, preVnode) {
          console.log("why created", bindings);
        },
```





![image-20250725142601605](./assets/17_composition API(3).assets/image-20250725142601605.png)



案例:

把时间戳进行格式化

./directives/format-time.js

```js
import dayjs from 'dayjs';

export default function(app) {
  app.directive("format-time", {
    created(el, bindings) {
      bindings.formatString = "YYYY-MM-DD HH:mm:ss";
      
      // 获取用户传过来的值，在这里加一个属性，这个属性在mounted中就能拿到了
      if (bindings.value) {
        bindings.formatString = bindings.value;
      }
    },
    mounted(el, bindings) {
      const textContent = el.textContent;
      let timestamp = parseInt(textContent);
      if (textContent.length === 10) {
        timestamp = timestamp * 1000
      }
      el.textContent = dayjs(timestamp).format(bindings.formatString);
    }
  })
}
```

./directives/index.js

```js
import registerFormatTime from './format-time';

export default function registerDirectives(app) {
  registerFormatTime(app);
}
```



./main.js

```js
import { createApp } from 'vue'
import App from './03_自定义指令/App.vue'
import registerDirectives from './directives'
import pluginObject from './plugins/plugins_object'
import pluginFunction from './plugins/plugins_function'

const app = createApp(App);

registerDirectives(app);

app.use(pluginObject);
app.use(pluginFunction);

app.directive("focus", {
  mounted(el, bindings, vnode, preVnode) {
    console.log("focus mounted");
    el.focus();
  }
})

app.mount('#app');

```



./App.vue

```vue
<template>
  <h2 v-format-time="'YYYY/MM/DD'">{{timestamp}}</h2>

  <h2 v-format-time>{{timestamp}}</h2>
  <h2 v-format-time>{{timestamp}}</h2>
  <h2 v-format-time>{{timestamp}}</h2>
  <h2 v-format-time>{{timestamp}}</h2>
  <h2 v-format-time>{{timestamp}}</h2>

</template>

<script>
  export default {
    setup() {
      const timestamp = 1624452193;

      return {
        timestamp
      }
    },
    mounted() {
      console.log("app mounted");
    }
  }
</script>

<style scoped>

</style>
```





## 认识Teleport

在组件化开发中，我们封装一个组件A，在另外一个组件B中使用： 

- 那么组件A中template的元素，会被挂载到组件B中template的某个位置； 
- 最终我们的应用程序会形成一颗DOM树结构；

但是某些情况下，我们希望组件不是挂载在这个组件树上的，可能是移动到Vue app之外的其他位置： 

- 比如移动到body元素上，或者我们有其他的div#app之外的元素上； 
- 这个时候我们就可以通过teleport来完成；

Teleport是什么呢？ 

- 它是一个Vue提供的内置组件，类似于react的Portals； 

- teleport翻译过来是心灵传输、远距离运输的意思；

  - 它有两个属性： 
    - to：指定将其中的内容移动到的目标元素，可以使用选择器； 
    - disabled：是否禁用 teleport 的功能；

  ​	



## 我们来看下面代码的效果：

./App.vue

```vue
<template>
  <div class="app">
    <!-- 如果不加to，那么这里面的元素不显示 -->
    <teleport to="#why">
      <h2>当前计数</h2>
      <button>+1</button>
      
      <!-- 组件也是一样的 -->
      <hello-world></hello-world>
    </teleport>

    <teleport to="#why">
      <span>呵呵呵呵</span>
    </teleport>
  </div>
</template>

<script>
  import { getCurrentInstance } from "vue";

  import HelloWorld from './HelloWorld.vue';

  export default {
    components: {
      HelloWorld
    },
    setup() {
      const instance = getCurrentInstance();
      console.log(instance.appContext.config.globalProperties.$name);
    },
    mounted() {
      console.log(this.$name);
    },
    methods: {
      foo() {
        console.log(this.$name);
      }
    }
  }
</script>

<style scoped>

</style>
```

可以发现正常挂载到app下面的元素，挂载到了why元素下面

![image-20250725205546801](./assets/17_composition API(3).assets/image-20250725205546801.png)

有多个teleport的话，他们会合并到一起

其实像antd 或者element也会有一些弹窗功能用的是这种





## 和组件结合使用

当然，teleport也可以和组件结合一起来使用： 

- 我们可以在 teleport 中使用组件，并且也可以给他传入一些数据；

![image-20250725142630809](./assets/17_composition API(3).assets/image-20250725142630809.png)



![image-20250725142637703](./assets/17_composition API(3).assets/image-20250725142637703.png)





## 多个teleport

如果我们将多个teleport应用到同一个目标上（to的值相同），那么这些目标会进行合并：

![image-20250725142646079](./assets/17_composition API(3).assets/image-20250725142646079.png)



实现效果如下：

![image-20250725142652854](./assets/17_composition API(3).assets/image-20250725142652854.png)





## 认识Vue插件

通常我们向Vue全局添加一些功能时，会采用插件的模式，它有两种编写方式： 

- 对象类型：一个对象，但是必须包含一个 install 的函数，该函数会在安装插件时执行； 
- 函数类型：一个function，这个函数会在安装插件时自动执行；

插件可以完成的功能没有限制，比如下面的几种都是可以的： 

- 添加全局方法或者 property，通过把它们添加到 config.globalProperties 上实现； 
- 添加全局资源：指令/过滤器/过渡等； 
- 通过全局 mixin 来添加一些组件选项； 
- 一个库，提供自己的 API，同时提供上面提到的一个或多个功能；

## 插件的编写方式

![image-20250725142706469](./assets/17_composition API(3).assets/image-20250725142706469.png)



![image-20250725142713148](./assets/17_composition API(3).assets/image-20250725142713148.png)



使用：

./main.js

```js
import { createApp } from 'vue'
import pluginObject from './plugins/plugins_object'
import pluginFunction from './plugins/plugins_function'
const app = createApp(App);
app.use(pluginObject);
app.use(pluginFunction);

app.mount('#app');

```



## 给vue全局增加属性及使用

```js
export default {
  install(app) {
    app.config.globalProperties.$name = "coderwhy"
  }
}
```

使用这个属性是这样的

```vue
<script>
  import { getCurrentInstance } from 'vue'
  // 获取一：options api中获取
	mounted() {
    console.log(this.$name) // 使用全局的属性
  }
  
  // 获取二：composition api中获取
  setup() {
    const instance = getCurrentInstance()	// 获取组件实例
    console.log(instance.appContext.config.globalProperties.$name)
  }
</script>
```

