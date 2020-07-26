## 组件间通信

- v-model	一般用于封装带表单项的复用性组件，比如input , checkbox , radio , select

  ```vue
  // 方式1
  父组件: 
          <Children v-model="aaa"/>
  子组件: 
          props: ['value']
          <input type="text" :value="value" @input="$emit('input',$event.target.value)">
  
  // 方式2 
  父组件: 
          <Children :value="aaa" @input="aaa=$event"/>
  子组件: 
          props: ['value']
          <input type="text" :value="value" @input="$emit('input',$event.target.value)">
  ```



- sync  属性修饰符  用于父组件和子组件的数据进行双向绑定

  ```vue
  父组件：
  	<Children :abc.sync="123"/>
  子组件：
  	props: ['abc']
  	<div>{{abc}}</div>
  ```



- $attr 和 $listeners 属性数据的传递

  ```vue
  父组件
  	<Children  type="info" icon="el-icon-deit" @click="abc"  />
  子组件
  	<el-button v-bind="$attrs" v-on="$listeners">我是按钮</el-button>
  
  // 通过v-bind获取父组件的所有属性（type,icon）
  // 通过v-on获取父组件的所有监听事件（click）
  ```



- 作用域插槽 	

  ##### 用于父子组件属性数据传递

  ```vue
  父组件
  <!-- 写法1 -->
  	<Children >
  		<template slot-scope="abc">
  			<div>{{abc.row.xxx}}</div>
  			<div>{{abc.$index}}</div>
  		</template>
  	</Children>
  
  <!-- 写法2 -->
  	<Children >
  		<template slot-scope="{row,$index}">
  			<div>{{row.xxx}}</div>
  			<div>{{$index}}</div>
  		</template>
  	</Children>
  	
  子组件
      <div v-for="(item,index) in data" :key="index">
          <solt :row="item" :$index="index" ></solt>
  	</div>	
  	
  ```

  

## Mixin技术

​		mixin用于封装vue复用的js代码

```js
// 创建一个mixin.js ， 封装js代码
export const abc = {
	methods:{
		aaaa(){
			...执行代码
		}
	}
}
```

```js
//在vue组件中引入mixin.js
import {abc} from './mixin.js'

export default{
	mixins : [abc]
}
```



## .native

##### 		如果vue中没有的事件监听可以通过.native来绑定原生js 的事件监听

```vue
<!-- vue有的事件可以直接@绑定 -->
<div @click="abc"></div>

<!-- vue没有的事件可以通过.native绑定原生的事件 -->
<div @dbclick.native="abc"></div>
```



## 路由守卫

##### 路由导航(跳转)守卫是什么?
​	vue-router提供的能监控(监视和控制)路由跳转的相关语法功能

##### 分类:  (应用开发中基本都是用前置守卫)

- 全局守卫
  - 前置: 监视任意路由跳转, 在准备跳转到目标路由时回调
    			router.beforeEach((to, from, next) => {})
          			to: 目标路由对象
          			from: 当前路由对象  对应的就$route
          			next: 控制路由跳转的函数
          			  不执行: 不放行, 不会跳转到目标路由
          			  next(): 放行, 请求的路由组件才能显示
          			  next(path): 强制跳转到指定路由去
  - 后置: 监视任意路由跳转, 在已经跳转到目标路由时才调用
- 路由守卫
  - 前置: 监视是跳转到当前路由, 当准备跳转时回调
    			beforeEnter: (to, from, next) => { }
- 组件守卫
  - 前置: 与路由前置守卫功能类似
    			beforeRouteEnter (to, from, next) {},
          				next((component) => {}) // 指定回调函数在组件对象创建之后执行
  - 更新: beforeRouteUpdate (to, from, next) 
  - 离开:  beforeRouteLeave (to, from, next)



## 路由跳转指定显示位置

```js
const router = new VueRouter({
    mode:'history',
    routes,
    
    //跳转路由后显示页面最顶部
    scrollBehavior(){
        return {x : 0 ,y : 0}
    }
})
```



## $nextTick()

##### 	$nextTick()，是将回调函数延迟在下一次dom更新数据后调用,当数据更新了，在dom中渲染后，自动执行该函数，

```js
methods:{
    testClick:function(){
      this.abc="修改后的值";
      this.$nextTick(function(){
        console.log(this.$refs.xxx.innerText);  //输出：修改后的值
      });
    }
}
```



## $set 和 $delete

##### 	响应式数据对象，修改数据界面会自动刷新数据

 - 设置响应式数据

   ```js
   Vue.set( target, key, value )
   vm.$set( target, key, value )
   ```

- 删除属性响应式数据对象的属性

  ```js
  Vue.delete( target, key )
  vm.$delete( target, key )
  ```

  

## sessionStorage

##### 	sessionStorage将数据暂时存储到本地浏览器上，关闭浏览器则数据清空

- 设置sessionStorage.

  ```js
  window.sessionStorage.setItem(key, value)
  ```

- 获取sessionStorage

  ```js
  window.sessionStorage.getItem(key)
  ```

- 删除sessionStorage

  ```
  window.sessionStorage.removeItem(key)
  ```

  

## 图片懒加载

- 使用vue-lazyload实现

  ```js
  // 下载: npm install -S vue-lazyload
  import VueLazyload from 'vue-lazyload'
  // 配置vue的插件
  Vue.use(VueLazyload, { // 内部自定义一个指令: lazy
  	loading,  // 配置loading图片
  })
  <img v-lazy="img图片地址" />
  ```

  

## mock数据接口

##### 	mock的作用是生成随机数据，拦截 Ajax 请求，前后端分离，模拟后台获取数据

- 下载安装mock

  ```js
  npm install mockjs
  ```

- 创建一个用来存储json数据的json文件

- 创建一个mockserver.js

  ```js
  // 引入mock
  import Mock from 'mockjs'
  // 引入json文件
  import abc from './abc.json'
  
  // 定义mock服务器的路由
  Mock.mock('/mock/abc',{
          code : 200,
          data : abc
  })
  ```

- 在api中创建一个mockAjax.js

  ```js
  import axios from "axios"
  
  //创建axios新实例，修改两个属性
  const instance = axios.create({
      baseURL : '/',
      timeout : 15000
  })
  
  export default instance
  ```

- 然后在api文件中定义一个专门发生mock请求的方式

  ```js
  //引入mockAjax.js
  import mockAjax from './mockAjax'
  
  //发生mock请求
  export const reqAbc = () => mockAjax('/mock/abc')
  ```

  

## watch监听属性

##### 	监听当前路由是否发生变化

```js
watch: {
	//当路由发生变化后，比如params参数值修改，则会执行
    $route(to, from) {
    
      //下面是自定义的页面刷新的方法
      this.shuaxin();
    }
},
```

##### 	监听属性值发生变化

```js
//方式1：
watch: {
//当需要监听一个对象的改变时，普通的watch方法无法监听到对象内部属性的改变，此时就需要deep属性对对象进行深度监听。
    abc: {
    	handler(newVal) {
            //...执行命令
        },
        deep: true
    }
},
//这种方式会将所有属性都加上这个监听器，当对象属性较多时，每个属性值的变化都会执行handler
    
//方式2：（优化，监听某一个属性）
watch: {
    //监听abc对象的xxx属性
    'abc.xxx': {
        handler(newVal, oldVal) {
             ......
        },
    deep: true
    }    
}
```





## 数组方法

- map  修改数组中的每一个元素获取新数组

  ```js
  // 数组中所有的元素都加10
  let arr = [1,2,3,4,5]
  let abc = arr.map((item, index) => item + 10))
  
  //结果abc = [11,12,13,14,15]
  ```

- reduce  累积处理数组中的每一个元素获取累积后的数据

  ```js
  // 数组中所有的元素累加
  let arr = [1,2,3,4,5]
  let abc = arr.reduce(
      //第一个参数是一个回调函数，preTotal是每次累积的值
  	(preTotal, item, index) => {
        return preTotal + item
      }, 
      //第二个参数是初始值，表示从0开始累积
      0
  )
  
  //结果abc = 15
  ```

- filter   过滤数组中的每一个元素并获取新数组

  ```js
  // 找到是偶数的元素
  let arr = [1,2,3,4,5]
  let abc = arr.filter((item, index) => item % 2 === 0))
  
  //结果abc = [2,4]
  ```

- find  和filter类似，用于寻找数组中的元素

  ```js
  // 找到是偶数的元素
  let arr = [1,2,3,4,5]
  let abc = arr.find((item, index) => item % 2 === 0))
  
  //结果abc = [2,4]
  ```

- findIndex   数组中的元素的下标

  ```js
  // 找到是偶数的元素的下标
  let arr = [1,2,3,4,5]
  let abc = arr.findIndex((item, index) => item % 2 === 0))
  
  //结果abc = [1,3]
  ```

- every    用于判断数组中的所有元素，如果全都满足条件结果就是true

  ```js
  // 判断数组中的所有元素是否都大于3
  let arr = [1,2,3,4,5]
  let abc = arr.every((item, index) => item > 3 ))
  
  //结果abc = false
  ```

- some    用于判断数组中的所有元素，只要有一个元素满足条件结果就是true

  ```js
  // 判断数组中的元素是否有大于3的值
  let arr = [1,2,3,4,5]
  let abc = arr.some((item, index) => item > 3 ))
  
  //结果abc = true
  ```

  

## VUE深度选择器

##### 	通过深度选择器可以在scoped下修改子/孙组件的样式, 尤其是第三方UI组件库组件

```css
/*原生css*/
.abc >>> .xxx {
      color: red;
}

/* 预编译器 */
.abc { 
    /deep/ .xxx {
        color: red;
    }
}
```

