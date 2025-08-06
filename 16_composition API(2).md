

## Reactive判断的API

isProxy 

- 检查对象是否是由 reactive 或 readonly创建的 proxy。

 isReactive 

- 检查对象是否是由 reactive创建的响应式代理： 

- 如果该代理是 readonly 建的，但包裹了由 reactive 创建的另一个代理，它也会返回 true；

isReadonly 

- 检查对象是否是由 readonly 创建的只读代理。

toRaw 

- 返回 reactive 或 readonly 代理的原始对象（不建议保留对原始对象的持久引用。请谨慎使用）。

shallowReactive 

- 创建一个响应式代理，它跟踪其自身 property 的响应性，但不执行嵌套对象的深层响应式转换 (深层还是原生对象)。

shallowReadonly 

- 创建一个 proxy，使其自身的 property 为只读，但不执行嵌套对象的深度只读转换（深层还是可读、可写的）。





## toRefs

如果我们使用ES6的解构语法，对reactive返回的对象进行解构获取值，那么之后无论是修改结构后的变量，还是修改reactive 返回的state对象，数据都不再是响应式的，相当于他们已经断开了响应式链接：

![image-20250725085400388](./16_composition API(2).assets/image-20250725085400388.png)

相当于:

```vue
let name = 'why'
let age = 18
```



那么有没有办法让我们解构出来的属性是响应式的呢？ 

- Vue为我们提供了一个toRefs的函数，可以将reactive返回的对象中的属性都转成ref； 
- 那么我们再次进行结构出来的 name 和 age 本身都是 ref的；

![image-20250725085407451](./16_composition API(2).assets/image-20250725085407451.png)

这样就相当于创建了两个ref

这种做法相当于已经在state.name和ref.value之间建立了链接，任何一个修改都会引起另外一个变化；

如果我们只希望转换一个reactive对象中的属性为ref, 那么可以使用toRef的方法：

![image-20250725085416483](./16_composition API(2).assets/image-20250725085416483.png)

注意：toRef不需要再解构了

toRefs：是将reactive对象的所有属性转成ref， 建立链接

toRef：是将其中一个属性转成ref，建立链接





## ref其他的API

unref

如果我们想要获取一个ref引用中的value，那么也可以通过unref方法： 

- 如果参数是一个 ref，则返回内部值，否则返回参数本身； 
- 这是 val = isRef(val) ? val.value : val 的语法糖函数；

isRef 

- 判断值是否是一个ref对象。

shallowRef 

- 创建一个浅层的ref对象；

```vue
<script>
  import { ref, shallowRef, triggerRef } from 'vue';

  export default {
    setup() {
      // 这里是一个对象
      const info = shallowRef({name: "why"})

      const changeInfo = () => {
        info.value.name = "james";	// 这样修改的话是不能修改这个name的，因为他是shallowRef,除非这样设置info.value = {name: 'james'}
      }

      return {
        info,
        changeInfo
      }
    }
  }
</script>
```

但是我就想通过info.value.name去修改，并且达到响应式怎么办？



triggerRef 

- 手动触发和 shallowRef 相关联的副作用：

![image-20250725085439364](./16_composition API(2).assets/image-20250725085439364.png)



## customRef

创建一个自定义的ref，并对其依赖项跟踪和更新触发进行显示控制： 

- 它需要一个工厂函数，该函数接受 track 和 trigger 函数作为参数； 
- 并且应该返回一个带有 get 和 set 的对象；

这里我们使用一个的案例： 

- 对双向绑定的属性进行debounce(节流)的操作；

./hook/useDebounce.js

```js
import { customRef } from 'vue';

// 自定义ref
export default function(value, delay = 300) {
  let timer = null;
  return customRef((track, trigger) => {
    return {
      get() {
        track(); 	// 触发依赖
        return value;
      },
      set(newValue) {
        clearTimeout(timer);
        timer = setTimeout(() => {
          value = newValue;
          trigger();	// 进行更新
        }, delay);
      }
    }
  })
}
```

./App.vue

```vue
<template>
  <div>
    <input v-model="message"/>
    <h2>{{message}}</h2>
  </div>
</template>

<script>
  import debounceRef from './hook/useDebounceRef';

  export default {
    setup() {
      const message = debounceRef("Hello World");

      return {
        message
      }
    }
  }
</script>

<style scoped>

</style>
```

他这里做的主要是拿到我们要进行响应式的值，当页面显示使用它的时候，就调用get方法，收集到这个依赖，返回我们的值，当我们设置它的时候我们赋值，然后触发页面更新





## computed

在前面我们讲解过计算属性computed：当我们的某些属性是依赖其他状态时，我们可以使用计算属性来处理 

- 在前面的Options API中，我们是使用computed选项来完成的； 
- 在Composition API中，我们可以在 setup 函数中使用 computed 方法来编写一个计算属性；

如何使用computed呢？ 

- 方式一：接收一个getter函数，并为 getter 函数返回的值，返回一个不变的 ref 对象； 

- 方式二：接收一个具有 get 和 set 的对象，返回一个可变的（可读写）ref 对象；

  computed默认的getter

![image-20250725085548393](./16_composition API(2).assets/image-20250725085548393.png)

computed也可以有自己的getter和setter

![image-20250725085556252](./16_composition API(2).assets/image-20250725085556252.png)

computed这个函数的返回值是一个ref对象，所以在设置值的时候就要

```js
fullName.value = 'coder wts'
```







## 侦听数据的变化

在前面的Options API中，我们可以通过watch选项来侦听data或者props的数据变化，当数据变化时执行某一些 操作。

在Composition API中，我们可以使用watchEffect和watch来完成响应式数据的侦听； 

- watchEffect用于自动收集响应式数据的依赖； 
- watch需要手动指定侦听的数据源；





## watchEffect

会自动收集响应式依赖

当侦听到某些响应式数据变化时，我们希望执行某些操作，这个时候可以使用 watchEffect。

我们来看一个案例： 

- 首先，watchEffect传入的函数会被立即执行一次，并且在执行的过程中会收集依赖； 
- 其次，只有收集的依赖发生变化时，watchEffect传入的函数才会再次执行；

![image-20250725085607802](./16_composition API(2).assets/image-20250725085607802.png)

注意：这里没有指定监听某一个属性，而是直接传入了一个函数，在这个函数中任何一个响应式对象发生变化，这个函数就会重新执行





## watchEffect的停止侦听

如果在发生某些情况下，我们希望停止侦听，这个时候我们可以获取watchEffect的返回值函数，调用该函数即可。 

比如在上面的案例中，我们age达到20的时候就停止侦听：

![image-20250725085615922](./16_composition API(2).assets/image-20250725085615922.png)





## watchEffect清除副作用

什么是清除副作用呢？ 

- 比如在开发中我们需要在侦听函数中执行网络请求，但是在网络请求还没有达到的时候，我们停止了侦听器， 或者侦听器侦听函数被再次执行了。 
- 那么上一次的网络请求应该被取消掉，这个时候我们就可以清除上一次的副作用；

在我们给watchEffect传入的函数被回调时，其实可以获取到一个参数：onInvalidate 

- 当副作用即将重新执行 或者 侦听器被停止 时会执行该函数传入的回调函数； 
- 我们可以在传入的回调函数中，执行一些清除工作；

![image-20250725085630009](./16_composition API(2).assets/image-20250725085630009.png)

这里是组件销毁的时候，那么watchEffect也会销毁，那么在销毁的时候调用onInvalidate传入一个函数，函数中就可以做一些清除副作用（取消请求，清除计时器等）的操作了





## setup中使用ref

在讲解 watchEffect执行时机之前，我们先补充一个知识：在setup中如何使用ref或者元素或者组件？ 

- 其实非常简单，我们只需要定义一个ref对象，绑定到元素或者组件的ref属性上即可；

![image-20250725085640111](./16_composition API(2).assets/image-20250725085640111.png)

上面如果打印titleRef的话是null，因为titleRef需要等到挂载以后才会有值，那么我们可以这样做

```vue
<script>
  import { ref, watchEffect } from 'vue';

  export default {
    setup() {
      const titleRef = ref(null);

      // 这里会执行两次，第一次是null，第二次会拿到dom
      watchEffect(() => {
        console.log(titleRef.value);
      })

      return {
        titleRef
      }
    }
  }
</script>
```





## watchEffect的执行时机

默认情况下，组件的更新会在副作用函数执行之前： 

- 如果我们希望在副作用函数中获取到元素，是否可行呢？

![image-20250725085649569](./16_composition API(2).assets/image-20250725085649569.png)

![image-20250725085655932](./16_composition API(2).assets/image-20250725085655932.png)

我们会发现打印结果打印了两次： 

- 这是因为setup函数在执行时就会立即执行传入的副作用函数，这个时候DOM并没有挂载，所以打印为null； 
- 而当DOM挂载时，会给title的ref对象赋值新的值，副作用函数会再次执行，打印出来对应的元素；





## 调整watchEffect的执行时机

如果我们希望在第一次的时候就打印出来对应的元素呢？ 

- 这个时候我们需要改变副作用函数的执行时机； 
- 它的默认值是pre，它会在元素挂载或者更新之前执行； 
- 所以我们会先打印出来一个空的，当依赖的title发生改变时，就会再次执行一次，打印出元素；



我们可以设置副作用函数的执行时机：

![image-20250725085704662](./16_composition API(2).assets/image-20250725085704662.png)

watchEffect默认会执行一次，但是设置为post后就会在dom挂载完之后执行一次了，它的应用场景就是如果在侦听dom发生了改变，需要获取更新后的dom，那么就可以选择post

flush 选项还接受 sync，这将强制效果始终同步触发。然而，这是低效的，应该很少需要。





## Watch的使用

watch的API完全等同于组件watch选项的Property： 

- watch需要侦听特定的数据源，并在回调函数中执行副作用； 
- 默认情况下它是惰性的，只有当被侦听的源发生变化时才会执行回调；

与watchEffect的比较，watch允许我们： 

- 懒执行副作用（第一次不会直接执行）； 
- 更具体的说明当哪些状态发生变化时，触发侦听器的执行； 
- 访问侦听状态变化前后的值；





## 侦听单个数据源

watch侦听函数的数据源有两种类型： 

- 一个getter函数：但是该getter函数必须引用可响应式的对象（比如reactive或者ref）； 
- 直接写入一个可响应式的对象，ref（如果侦听的是一个reactive对象，需要进行某种转换）；

watch函数第一个参数传入一个getter函数

![image-20250725085715006](./16_composition API(2).assets/image-20250725085715006.png)

watch函数第一个参数传入一个可响应对象（ref）

这里newValue和oldValue拿到的是kobe和james，而不是响应式对象

![image-20250725085720456](./16_composition API(2).assets/image-20250725085720456.png)

watch函数第一个参数传入一个可响应对象（reactive对象）

这里的newValue和oldValue是响应式对象

```js
      const info = reactive({name: "why", age: 18});

      // 传入一个可响应式对象: reactive对象/ref对象
			// reactive对象获取到的newValue和oldValue本身都是reactive对象
      watch(info, (newValue, oldValue) => {
        console.log("newValue:", newValue, "oldValue:", oldValue);
      })
```

如果希望newValue和oldValue是一个普通的对象

```js
      watch(() => ({...info}), (newValue, oldValue) => {
        console.log("newValue:", newValue, "oldValue:", oldValue);
      })
```



watch的源码：

![image-20250725135304810](./16_composition API(2).assets/image-20250725135304810.png)

所以可以传入Ref,react,array,function，最终都会赋值给getter

> 注意：
>
> ​	watch没办法直接侦听一个reactive的对象





## 侦听多个数据源

侦听器还可以使用数组同时侦听多个源：

![image-20250725085730784](./16_composition API(2).assets/image-20250725085730784.png)

这里的newValue是name和age的新的值组成的数组，oldValue是name和age的旧的值组成的数组





## 侦听响应式对象

如果我们希望侦听一个数组或者对象，那么可以使用一个getter函数，并且对可响应对象进行解构：

![image-20250725085740368](./16_composition API(2).assets/image-20250725085740368.png)

甚至可以这样写

```js
      // 1.定义可响应式的对象
      const info = reactive({name: "why", age: 18});
      const name = ref("why");

      // 2.侦听器watch
      watch([() => ({...info}), name], ([newInfo, newName], [oldInfo, oldName]) => {
        console.log(newInfo, newName, oldInfo, oldName);
      })
```







## watch的选项

如果我们希望侦听一个深层的侦听，那么依然需要设置 deep 为true： 

- 也可以传入 immediate 立即执行；



默认情况下watch是支持深度侦听的，因为源码是这样写的

![image-20250725141034005](./16_composition API(2).assets/image-20250725141034005.png)

但是如果是这种写法的话

![image-20250725085749903](./16_composition API(2).assets/image-20250725085749903.png)

或者这样写的话

```js
      // 1.定义可响应式的对象
      const info = reactive({
        name: "why", 
        age: 18,
        friend: {
          name: "kobe"
        }
      });

      // 2.侦听器watch
      watch(() => ({...info}), (newInfo, oldInfo) => {
        console.log(newInfo, oldInfo);
      }, {
        deep: true,	// 深度侦听
        immediate: true // 默认执行
      })
```

他默认情况下就不会做深度侦听了，如果想做深度侦听，需要传deep: true，如果第一次需要执行就传入immediate

