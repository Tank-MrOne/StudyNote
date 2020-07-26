##  SRC：开发目录编写 

* api 定义接口

  * ajax.js \   mockAjax.js 都是定义接口配置（axios修改默认属性）

    ```js
    import axios from 'axios'
    const instance = axios.create({//修改默认属性
        baseURL :'/api',//默认是"/"，修改为当前路径以API开头
        timeout : 15000//修改超时时间为15秒
    })
    //用以上修改后的axios去注册一个请求拦截
    instance.interceptors.request.use(config =>{
        return config
    })
    //注册响应拦截
    instance.interceptors.response.use(
        response =>{
            return response.data
        },
        error=> {
            return  Promise.reject(error)
        }
    )
    export default instance
    
    
    
    
    
    
    ```

  * index.js 定义API接口

    ```js
    //定义登录接口（参数data、路径url、方法mehtod)
    export function reqlogin (mobile,password){
        return instance({
            url : '/user/passport/login',
            method : 'POST',
            data :{
                mobile,//电话
                password//密码
            }
        })
    }
    ```

* store   vue数据模块化管理（管理每个组件的数据）

  ```js
  import Vuex from 'vuex'//导入vuex插件
  import vue from 'vue'//导入vue插件
  
  vue.use(Vuex)//安装vuex插件
  
  export default {
          state:{
                  productList : {}
          },
          mutations :{
              	//这里的state参数是内置参数，
                  receive_product_list(state,productList){
                          state.productList = productList
                  }
          },
          actions:{
                  async getProductList({commit},options){
                      //调用reqProductList接口
                          const result = await reqProductList(options)
                          if(result.code === 200){
                                  const productList = result.data
                                  // commit提交给 mutations里面的 receive_product_list这个函数，并把								productList这个参数给mutations
                                  commit('receive_product_list',productList)
                          }
                  }
          },
      	getters:{
                trademarkList(state){
                      return state.productList.trademarkList  || []
                }  ,
                attrsList(state){
                        return state.productList.attrsList || []
                }
          }
  }
  
  进入对应的组件中通过this.$store.dispach去触发actions里面的函数，通过this.$store.state. productList从而获取到数据，最后渲染到页面
  ```

* pages __静态组件拆分生成多个路由组件，（路由组件）

* components__拆分复用多次的路由组件，（非路由组件）

* router__定义路由规则

  ```js
  import Vue from "vue"
  import VueRouter from "vue-router"
  
  Vue.use(VueRouter)
  
  export default new VueRouter({
      //定义一个路由规则，当路径访问到/detail/:id，就会自动跳转到Detail组件
      {
          path:'/detail/:id',
          component:Detail
  	},
      
      mode:'history',
      scrollBehavior(to,from,savadPosition){
          return {x : 0 ,y : 0}
      }
  })
  ```

  

* APP.vue__拼接pages 和 components

  * <router-view></router-view> 显示切换所有路由组件

* main.js__打包整合入口文件，

  * 导入全局组件（typenav)，

  * 定义全局组件（Vue.component('Typenav',Typenav)，）

  * 实例化一个vm ,

    ```js
    new Vue({
      render: h => h(App),
    }).$mount('#app')
    
    ```

  * render属性将APP组件显示到页面

##  dist  打包后的项目文件（生产文件）

	* css  、img 、js
	* index.html

## node_modules  存放所有第三方依赖包（不需要上传仓库）



## public 静态代码

 * 里面文件都会打包到dist文件里面去，

## package.json第三方依赖包的配置文件（npm的配置文件）

* 多人协同开发，下载依赖包为同一版本，

## vue.config.js 为了修改webpack里面的配置选择

```jsx
 module.exports ={
    lintOnSave:false,  //关闭eslint语法检测，（比如定义了                        没有使用的变量） 
 devServer: {
        proxy: {
            '/api': {
                //生成一个可以跨域的服务代理
                target: "http://182.92.128.115",
                //允许跨域
                changeOrigin: true,
            };
        };
    };
}
```













