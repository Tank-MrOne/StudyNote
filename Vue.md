# Vue



## Vue 的简介

> 渐进式，只是一些核心代码，可以让我们快速搭建基本页面，如果页面功能相对比较丰富，只需要相关的一些插件去完成
>
> 动态构建显示用户界面
>
> 遵循MVVM模式



## Vue的基本使用

1. 在官网讲vue.js文件引入HTML中

2. vue的写法

   ```html
   <body>
       <div id="root">n'n'n'n'n'n'n'n'n'n'n'n'n'n'n'n'n'n'n
           {{msg}}
       </div>
   </body>
   <script src="vue.js"></script>
   <script>
   	new Vue({
           el : "#root" ,
           data : {
               msg : 'hello'
           }
       })
   </script>
   ```

>  vue当中是以数据驱动页面，以数据为主要，vue不必要去关系dom

- **<font color="orange">el</font>**  :  是代表Vue实例化对象要去服务的根元素，值本质是一个css选择器字符串，把对应的元素节点称作为Vue的挂载点

  ```vue
  <!-- 挂载点的另一种写法 -->
  <body>
      <div id="root">
          {{msg}}
      </div>
  </body>
  <script src="vue.js"></script>
  <script>
  	let vm = new Vue({
          data : {
              msg : 'hello'
          }
      })
      vm.$mount('#root')
  </script>
  ```

- **<font color="orange">data</font>**  :  可以是一个对象，这个对象就是我们的数据对象，里面的数据都是为了给模板提供的，模板就是挂载点内部的所有（不包括挂载点），data内部的数据，都会被Vue的实例获取到（数据代理）

  ```js
  // 对象写法
  data : {
      msg : 'hello'
  }
  
  // 函数写法
  data(){
      return {
          msg : 'hello'
      }
  }
  ```


### 模板语法-插值语法

> 插值： {{}}  代表插值语法，目的是为了去显示标签文本内容用的，{{}}内部是js表达式

```vue
<div id="root"> {{msg}} </div>
<div id="root"> {{msg.toUpperCase()}} </div>
```

### 模板语法-指令语法

> 指令： 大部分都是v-开头，本质是元素的自定义属性，目的是为了操作元素使用的

```vue
<!-- v-bind是为了给一个属性绑定动态获取的数据 -->
<a v-bind:href="url"></a>

<!-- v-bind简写方式 -->
<a :href="url"></a>
```

#### vue的内置指令

- `v-text`    文本输出，类似于js原生的innerText
- `v-html`    元素输出，类似于js原生的innerHTML
- `v-bind`   单向数据绑定  
- `v-on`    绑定事件
- `v-for`    列表渲染
- `v-if`     条件渲染
- `v-else-if`    条件渲染
- `v-else`    条件渲染
- `v-show`    条件渲染
- `ref`    为特定的元素添加引用标识，实例的$refs内部可以获取到

#### 自定义指令

- 全局指令：所有的模板都能使用

  注意：指令名字不带v-，指令名字不能大写

  ```vue
  <div v-upper="msg"></div>
  
  <script src="vue.js"></script>
  <script>
      /*
      * 通过Vue.directive来自定义全局指令
      * 第一个参数就是自定义名称，第二个参数是一个回调函数
      * @node 代表给元素使用的时候会传入元素的节点
      * @binding  是一个对象，里面可以获取到被绑定的数据，name,rawName,value,expression
      */
      Vue.directive('upper',function(node,binding){
          node.textContext = binding.value.toUpperCase()
      })
  	let vm = new Vue({
          data : {
              msg : 'hello'
          }
      })
      vm.$mount('#root')
  </script>
  ```

  

- 局部指令：只有当前模板能使用

  ```vue
  <div v-upper="msg"></div>
  
  <script src="vue.js"></script>
  <script>
      /*
      * 通过实例中的directives属性来自定义局部指令
      * 属性中定义一个函数，函数没就是自定义名称
      * @node 代表给元素使用的时候会传入元素的节点
      * @binding  是一个对象，里面可以获取到被绑定的数据，name,rawName,value,expression
      */
  	let vm = new Vue({
          data : {
              msg : 'hello'
          },
          directives:{
          	upper(node,binding){
          		node.textContext = binding.value.toUpperCase()
      		}
          }
      })
      vm.$mount('#root')
  </script>
  ```

  

### 单项数据绑定

> 类似于v-bind这样的数据绑定，我们称为单项数据绑定，过程只是数据从data到挂载点
>
> 单项数据绑定各种属性都是可以使用的

### 双向数据绑定

> 类似v-model表单属性数据绑定，就是双向数据绑定，原因是因为表单类元素可以页面输出内容

```vue
<input v-model="msg" />
```

### 事件绑定

> 通过v-on可以对标签进行事件的绑定，在Vue函数中，通过method声明函数

```vue
<!-- 绑定点击事件 -->
<button v-on:click="handleClick">
    点击
</button>
<script src="vue.js"></script>
<script>
	new Vue({
        el : "#root" ,
        method:{
            handleClick(){
                console.log("点击了")
            }
        }
    })
</script>
```

#### 事件绑定简写

```vue
<!-- 绑定点击事件 -->
<button @click="handleClick">
    点击
</button>
<script src="vue.js"></script>
<script>
	new Vue({
        el : "#root" ,
        method:{
            handleClick(){
                console.log("点击了")
            }
        }
    })
</script>
```

#### 事件操作符

- `@click.stop`  阻止事件冒泡

- `@click.prevent`   阻止浏览器默认行为

- `@click.once`   事件只会触发一次

- `@keyup.enter`    按下回车键事件

- `@keyup.tab`    按下tab键事件

- `@keyup.delete`    按下删除键事件

- `@keyup.esc`    按下esc键事件

- `@keyup.space `    按下空格键事件

- `@keyup.up `   按下方向键 上键事件

- `@keyup.down`  按下方向键 下键事件

- `@keyup.left`  按下方向键 左键事件

- `@keyup.right `  按下方向键 右键事件

  



#### 注意

1. vue控制的所有回调函数的this都是vm

2. 所有data/computed/metods中的属性/方法都会成为vm的属性/方法

3. 模板中表达式读取数据，是读取vm 的属性，模板中调用函数，调用的是vm 的方法

4. 调用事件不传参，默认传的就是事件对象event

5. 调用事件如果传参，则接收的参数就是传入的参数，调用event会获取window.event数据

6. 如果事件的参数传递中想传入事件对象，则可以通过$event传入

   ```vue
   <button @click="handleClick( item , $event )">
   ```

### 计算属性

> 通过`computed`可以设置计算属性

```vue
<input v-model="fullName"/>

<script src="vue.js"></script>
<script>
	new Vue({
        el : "#root" ,
		date(){
            return {
                firstName : "wan",
                lastName : "junqing"
            }
        },
        /* 详细计算属性写法 
        computed:{
            fullName:{
                get(){
                    return this.firstName + "-" + this.lastName
                },
                set(value){
                    let arr = value.split('-')
                    this.firstName = arr[0]
                    this.lastName = arr[1]
                }
            }
        } */
        /*如果计算属性只有一个get方法,则可以简写 */
        computed:{
            fullName(){
                return this.firstName + "-" + this.lastName
            }
        }
    })
</script>
```

### 监视属性

> 通过`watch`可以设置vue的监视属性,被监视的属性初始化必须存在

```vue
<input v-model="fullName"/>

<script src="vue.js"></script>
<script>
	new Vue({
        el : "#root" ,
		date(){
            return {
                firstName : "wan",
                lastName : "junqing",
                fullName : 'wanjunqing'
            }
        },
        watch:{
            firstName(newValue , oldValue){
                this.fullName = newValue + this.lastName
            },
            lastName(newValue , oldValue){
                this.fullName =  this.firstName + newValue
            }
        }
    }) 
</script>
```

> 监视的实例对象属性写法

```vue
<input v-model="fullName"/>

<script src="vue.js"></script>
<script>
	let vm = new Vue({
        el : "#root" ,
		date(){
            return {
                firstName : "wan",
                lastName : "junqing",
                fullName : 'wanjunqing'
            }
        }
    }) 
    vm.$watch('firstName',function(newValue , oldValue){
                this.fullName = newValue + this.lastName
    })
</script>
```

### 条件渲染

- `v-if` ，通过删除节点来决定元素是否显示

```vue
<div v-if="flag"> box1 </div>
<div v-else-if="flag"> box2 </div>
<div v-else> box3 </div>

<script src="vue.js"></script>
<script>
	new Vue({
        el : "#root" ,
		date(){
            return {
				flag : true
            }
        }
    }) 
</script>
```

- `v-show`，通过样式display来决定元素是否显示

```vue
<div v-show="flag"> box1 </div>
<div v-show="!flag"> box2 </div>

<script src="vue.js"></script>
<script>
	new Vue({
        el : "#root" ,
		date(){
            return {
				flag : true
            }
        }
    }) 
</script>
```

> 如果元素需要频繁的切换，建议使用v-show
>
> 如果元素切换的频率不高，建议使用v-if

### 样式的强制绑定

- 方式1 ---- 绑定类名

```vue
<style>
	.classA{
		color : 'red'
	}
</style>

<div :class="myClass"> box1 </div>

<script src="vue.js"></script>
<script>
	new Vue({
        el : "#root" ,
		date(){
            return {
				myClass : 'classA'
            }
        }
    }) 
</script>
```

- 方式2 --- 多个类名选择

```vue
<style>
	.classA{
		color : 'red'
	}
	.classA{
		color : 'blue'
	}
</style>

<div :class="{classA:isA , classB : isB}"> box1 </div>

<script src="vue.js"></script>
<script>
	new Vue({
        el : "#root" ,
		date(){
            return {
				isA : true,
				isB : false
            }
        }
    }) 
</script>
```

- 方式3   ----  多个类名共存

```vue
<style>
	.classA{
		color : 'red'
	}
	.classA{
		font-size : '20px'
	}
</style>

<div :class="['classA' , 'classB']"> box1 </div>

<script src="vue.js"></script>
<script>
	new Vue({
        el : "#root" 
    }) 
</script>
```

- 方式4  --- 动态样式值

```vue
<div :style="{color : myColor , fontSize : mySize}"> box1 </div>

<script src="vue.js"></script>
<script>
	new Vue({
        el : "#root" ,
        data(){
            return{
                myColor : 'red',
                mySize : '20px'
            }
        }
    }) 
</script>
```

### v-for循环渲染

```vue
<div v-for="(item,index) in arr" :key="item.id">
    {{ item.name }}
</div>

<script src="vue.js"></script>
<script>
	new Vue({
        el : "#root" ,
        data(){
            return{
                arr : [
                    {
                        id : 1,
                        name : 'Tom'
                    },
                    {
                        id : 2,
                        name : 'Lily'
                    },
                    {
                        id : 3,
                        name : 'Lucy'
                    }
                ]
            }
        }
    }) 
</script>
```

### 获取元素节点

> 为元素添加ref 标识后，可以通过this.$refs来获取元素

```vue
<div ref="abc"> box </div>
<script src="vue.js"></script>
<script>
	new Vue({
        el : "#root" ,
        mounted(){
            console.log(this.$refs.abc)
        }
    }) 
</script>
```



### 响应式数据

1. data当中的对象内部数据都会变为响应式数据，每个对象内部的属性都会添加get和set，添加了get和set，那么响应的数据就是响应式数据，对于data内部的不是对象属性则不会添加set和get方法（比如数组）
2. vue将观察数组的变化数组方法包裹起来，以便在调用这些方法时，也能够触发视图更新，这些包裹的方法：
   - push()
   - pop()
   - shift()
   - unshift()
   - splice()
   - sort()
   - reverse()

> vue 如何去监视数据变化而更新页面
>
> 1. 修改数组当中对象的属性，发现可以改变
> 2. 调用数组的方法直接修改数组当中的整个对象，发现也可以改变
> 3. 通过数组下标直接修改数组当中的整个对象，发现改变不了
>
> 原因：
>
> - 修改数组当中对象的属性，都是可以修改的，因为对象的所有属性都添加了get和set方法
> - 通过数组的方法去修改整个对象也是可以修改的，因为vue当中重写了数组的部分原生方法，添加了展示到页面的功能
> - 通过下标直接修改数组整个对象，不可以展示到页面，但是数据确实改了，因为数组的数据整体并没有添加get和set方法

### 自定义过滤器

> 在展示数据的时候，对数据进行处理（格式化之后再显示，不会影响原数据）

- 全局过滤器

  ```vue
  <div>
  	{{ msg | msgFormat }}
  </div>
  <script src="https://cdn.bootcdn.net/ajax/libs/moment.js/2.27.0/moment.js"></script>
  <script src="vue.js"></script>
  <script>
      /*
      * 通过Vue.filter来自定义全局过滤器
      * 第一个参数是过滤器名称，第二个参数是一个回调函数
      * @value 代表需要被过滤的数据
      */
      Vue.filter('msgFormat',function(value){
          moment(value).format('YYYY-MM-DD hh:mm:ss')
      })
  	let vm = new Vue({
          data : {
              msg : Date.now()
          }
      })
      vm.$mount('#root')
  </script>
  ```

- 局部过滤器

  ```vue
  <div>
  	{{ msg | msgFormat }}
  </div>
  <script src="https://cdn.bootcdn.net/ajax/libs/moment.js/2.27.0/moment.js"></script>
  <script src="vue.js"></script>
  <script>
      /*
      * 通过filters属性来自定义局部过滤器
      * @value 代表需要被过滤的数据
      */
  	let vm = new Vue({
          data : {
              msg : Date.now()
          },
          filters:{
          	msgFormat(value){
          		moment(value).format('YYYY-MM-DD hh:mm:ss')
      		}
          }
      })
      vm.$mount('#root')
  </script>
  ```

  

### 自定义插件

> 创建一个js文件，作为自定义插件

```js
(function(w){
    // 插件必须是一个对象，而且这个对象必须有一个方法叫install
    const MyPlugin = {}
    
    MyPlugin.install = function(Vue , options){
        // 添加全局方法或属性
        Vue.myGlobalMethod = function (){
            
        }
        
        // 添加一个全局资源(asset),自定义一个全局指令
        Vue.directive('upper',function(el , binding){
                // 例：自定义一个将内容自动变成大写的插件
                el.textContent = bingding.value.toUpperCase()
        })
        
        // 添加一个实例方法
        Vue.prototype.$myMethod = function(methodOptions){
            
        }
    }
    
    // 将对象暴露出去
    w.MyPlugin = MyPlugin
})(window)
```

> 在定义Vue语法前，通过use属性来使用自定义的插件

```vue
<script src="vue.js"></script>
<script src="自定义插件的路径"></script>
<script>
	// 例：自定义插件对象叫MyPlugin，则调用use方法来使用自定义插件
    // Vue如果想要用插件，都要去安装才能使用
	Vue.use(MyPlugin)
    
	new Vue({
        el:"#root"
    })
    vm.$mount('#root')
</script>
```



### 过渡和动画

- 过渡的类名

在进入/离开的过渡中，会有 6 个 class 切换。

1. `v-enter`：定义进入过渡的开始状态。在元素被插入之前生效，在元素被插入之后的下一帧移除。
2. `v-enter-active`：定义进入过渡生效时的状态。在整个进入过渡的阶段中应用，在元素被插入之前生效，在过渡/动画完成之后移除。这个类可以被用来定义进入过渡的过程时间，延迟和曲线函数。
3. `v-enter-to`：**2.1.8 版及以上**定义进入过渡的结束状态。在元素被插入之后下一帧生效 (与此同时 `v-enter` 被移除)，在过渡/动画完成之后移除。
4. `v-leave`：定义离开过渡的开始状态。在离开过渡被触发时立刻生效，下一帧被移除。
5. `v-leave-active`：定义离开过渡生效时的状态。在整个离开过渡的阶段中应用，在离开过渡被触发时立刻生效，在过渡/动画完成之后移除。这个类可以被用来定义离开过渡的过程时间，延迟和曲线函数。
6. `v-leave-to`：**2.1.8 版及以上**定义离开过渡的结束状态。在离开过渡被触发之后下一帧生效 (与此同时 `v-leave` 被删除)，在过渡/动画完成之后移除。

```vue
<style>
	.fade-enter-active, .fade-leave-active /* 过度的过程 */ {
 	 	transition: opacity .5s;
	}
	.fade-enter, .fade-leave-to /* 过度的初始和结果在 */ {
  		opacity: 0;
	}
</style>

<div id="demo">
  <button v-on:click="show = !show">
    Toggle
  </button>
  <!-- transition是Vue的内置组件，专门用来实现过度效果，name对应vue过度的类名 -->  
  <transition name="fade">
    <p v-if="show">hello</p>
  </transition>
</div>

<script src="vue.js"></script>
<script>
	new Vue({
  		el: '#root',
  		data: {
    		show: true
  		}
	})
</script>
```

- 动画

```vue
<style>
	.bounce-enter-active {
  		animation: bounce-in .5s;
	}
	.bounce-leave-active {
  		animation: bounce-in .5s reverse;
	}
	@keyframes bounce-in {
  		0% {
    		transform: scale(0);
  		}
  		50% {
    		transform: scale(1.5);
  		}
  		100% {
    		transform: scale(1);
  		}
	}
</style>

<div id="example-2">
  <button @click="show = !show">Toggle show</button>
  <transition name="bounce">
    <p v-if="show">Lorem ipsum dolor sit amet</p>
  </transition>
</div>

<script src="vue.js"></script>
<script>
	new Vue({
  		el: '#root',
  		data: {
    		show: true
  		}
	})
</script>
```





## Vue的生命周期

> 每个 Vue 实例在被创建时都要经过一系列的初始化过程——例如，需要设置数据监听、编译模板、将实例挂载到 DOM 并在数据变化时更新 DOM 等。同时在这个过程中也会运行一些叫做**生命周期钩子**的函数，这给了用户在不同阶段添加自己的代码的机会

![](https://cn.vuejs.org/images/lifecycle.png)

### beforeCreact

> 实例创建之前钩子
>
> 初始化之前，数据还没准备好

### created

> 实例创建之后的钩子
>
> 初始化之后，数据可以获取到

### beforeMount

> 页面元素挂载之前的钩子
>
> 页面渲染之前，无法获取到元素节点

### mounted

> 页面渲染完成的钩子
>
> 可以获取到元素节点，
>
> 一般一些异步操作建议在这个钩子中执行

### beforeUpdate

> 数据修改之前的钩子

### updated

> 数据修改之后的钩子

### beforeDestory

> 实例在销毁之前的钩子
>
> 一般一些异步操作的销毁在这个钩子中执行
>
> 如果想要执行这个方法，必须调用Vue实例的一个方法，this.$destory()

### destoryed

> 实例在销毁之后的钩子
>
> 如果想要执行这个方法，必须调用Vue实例的一个方法，this.$destory()



## Vue 组件化、模块化

> 一个js文件就是一个模块，模块就是为了复用js，简化js编写，提高js运行效率

> 组件就是界面功能效果的代码集合，使用组件就是为了复用编码，简化项目编码，提高运行效率

> 模块化：但应用的js都以模块来编写的，这个应用就是模块化开发

> 组件化：当应该是以多个组件的方式实现，这个应用就是一个组件化的应用

#### 定义基本组件

##### 非单文件组件

- 组件的三大步

  - 全局组件
    1. 定义组件（本质上就是生产一个实例化组件对象的构造函数）

       ```js
       // 这个配置对象和Vue的配置对象很相似，但不能写el
       let MyComponent = Vue.extend({
           // 组件中的data属性不能使用对象，只能使用函数
       	data(){
       		return {
                   item : 0
               }
       	},
           // 模板（定义标签）
           template : '<button></button>'
       })
       ```

       

    2. 注册组件

       ```js
       // 给组件定义一个自定义标签名
       Vue.component('Mybutton',MyComponent)
       ```

       

    3. 使用组件

       ```vue
       <!-- 在Vue模板当中使用自定义的标签名，就会自动的去实例化一个组件对象 -->
       <div id="root">
       	<Mybutton> 自定义按钮组件 </Mybutton>
       </div>
       ```

  - 局部组件

    ```js
    new Vue({
    	el : "#root",
    	data(){
    		item : ""
    	},
    	components : {
    		Mybutton :{
                data(){
                    return {
                        item : 0
                    }
                },
                // 模板（定义标签）
                template : '<button></button>'
    		}
    	}
    })
    ```

    

- 定义组件的简写方式

  ```js
  // 当调用Vue的component方法，传入的第二个参数是一个配置对象，内部，会将这个对象自动取调用Vue.extend生成对象的构造函数
  Vue.component('Mybutton',{
  	data(){
  		return {
              item : 0
          }
  	},
      template : '<button></button>'
  })
  ```


##### 单文件组件

> 一般单文件组件都会创建在component文件中
>
> 在component文件夹外创建一个App.vue文件，用来组装定义好的组件，App本质也是一个组件
>
> vm实例最终只需要把App组件注册然后进行渲染

单文件组件模板：

```vue
<template>
	<!-- 在组件中，根标签只能有一个 -->
	<div></div>
</template>

<script type="text/ecmascript-6">
export default {
	data(){},
	component:{}
}
</script>

<style scoped>

</style>
```

##### 定义父子组件

```vue
<!-- 非单文件父子组件定义 -->
<script>
new Vue({
	el : "#root",
	data(){
		item : ""
	},
})
// 定义子组件
Vue.component('Mychildren',{
    template : '<button></button>'
})
// 定义父组件，父组件中包含子组件
Vue.component('Myparent',{
    template : '<div> <Mychildren></Mychildren></div>'
})
</script>
```

```vue
<!-- 单文件父子组件定义 -->
<!-- 在父组件中引入子组件 -->
<template>
	<div>
    	<Mychildren />
    </div>
</template>

<script type="text/ecmascript-6">
// 1.首先导入子组件
import Mychildren from './Mychildren.vue'

export default {
	component:{
		Mychildren
	}
}
</script>

<style scoped>

</style>
```

##### index.js入口文件

- 首先安装vue

  ```shell
  npm i vue
  ```

  ```js
  // 引入vue
  import Vue from 'vue'
  
  // 引入组装好的整体组件App
  import App from './App.vue'
  ```
  
  ```js
  // 第一种写法
  new Vue({
  	el : "#root",
  	render: v => v(App) 
      // 把导入过来的App组件配置对象，在vue模板当中注册解析为一个标签名<App/>并使用
    // 并把这个标签在模板当中进行渲染
  })
  ```
  
  ```js
  // 第二种写法(用的很少)
  import Vue from 'vue/dist/vue.esm.js'
  new Vue({
  	el : "#root",
  	components:{
  		App
  	},
  	template:"<App/>"  // 这种方式需要用解析器进行处理，需要导入vue/dist/vue.esm.js
  })
  ```
  
  

## Vue组件间通信

### 父子组件通过属性传递数据

> 父组件通过:xxx="value"的形式将数据传入子组件
>
> 在子组件模板中通过**props**属性接收数据

```vue
<Mychildren :xxx="value"></Mychildren>
Vue.component('Mychildren',{
	props:['xxx'],
    template : '<div>{{xxx}}</div>'
})
```

**props**，组件间通信，只适用父子组件传递

- 父给子传：可以传递函数数据和非函数数据
- 子给父传：通过调用父传递过来的函数数据，然后通过参数给父传数据



### 通过自定义事件传递数据

> vue原型对象和组件都可以通过$on ,$emit来进行数据的传递

```vue
<!-- 父组件 -->
<Mychildren ref="child"></Mychildren>

<script>
import Mychildren from './Mychildren.vue'
export default {
    components:{
        Mychildren
    },
    mounted(){
        // $on 绑定事件
        this.$refs.child.$on("xxx",function(){
            console.log("父组件定义的一个事件")
        })
    }  
}
</script>
<!-- 
$on 的第一个参数：“事件名”
	第二个参数：绑定事件的回调函数
-->
```

```vue
<!-- 子组件 -->
<script>
export default {
    mounted(){
        // $emit 触发事件
		this.$emit
    }  
}
</script>
```







# 额外工具、依赖包、方法

## 不常用方法

### 清除浏览器控制台vue启动时生产提示

```js
Vue.config.productionTip = false
```

### 获取当前事件

```js
Date.now()  // 获取1970年到现在的毫秒数
```

### 通过事件对象获取数据

- 获取元素内容 event.target.innerText
- 获取表单内容 event.target.value
- 获取元素类型 event.target

### 阻止事件冒泡

```vue
<div @click="outer">
    <button @click.stop="inner">
</div>
   
<!-- 原生写法 ： event.stopPropagation() -->
```

### 阻止浏览器默认行为

```vue
<a href="https:///...." @click.prevent="inner"></a>

<!-- 原生写法 ： event.preventDefault() -->
```



## 工具

### chrome 助手

>  用于能访问谷歌的商店，点击下载   [google-access-helper-2.3.0.zip](http://180.76.238.89:8111/google-access-helper-2.3.0.zip) 



### 谷歌工具

> vuedevtools  ，vue 的调试工具



### vscode插件

- Vetur
- vue
- Vue 2 Snippets
- Vue VS Code Extension Pack
- vue-beautify
- vue-helper
- VueHelper
- wpy-beautify



### 时间格式化工具

```html
<script src="https://cdn.bootcdn.net/ajax/libs/moment.js/2.27.0/moment.js"></script>
```



## 依赖包

### 安装vue

```shell
npm i vue
```

### 安装vue-loader

```shell
npm i -D vue-loader vue-template-complier
```

配置webpack,找到loader项，添加以下

```js
module.exports = {
    ...
    rules : [
        ...,
        {
            test:/\.vue$/,
            loader:'vue-loader'
        }
    ]
}
```

配置webpack,引入VueLoaderPligin,并使用

```js
const VueLoaderPligin = require('vue-loader/lib/plugin')

module.exports = {
    ...
    plugin:[
        ...,
        new VueLoaderPligin()
    ]
}
```

### 配置webpack，打包CSS修改css-loader

```js
module.exports = {
    ...
    rules : [
        ...,
        {
            test:/\.css$/,
            use:['vue-style-loader','css-loader']
        }
    ]
}
```

### 配置webpack，解析文件后缀名

```js
module.exports = {
    ...
    resolve:{
    	extensions:['.js','.json'.'.vue']
    }
}
```

### 配置webpack，给路径取别名

```
module.exports = {
    ...
    resolve:{
    	...,
    	alias:{
    		'^@':'...路径'
    	}
    }
}
```

