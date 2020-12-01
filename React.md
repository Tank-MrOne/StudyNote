# React

## React 基础

### React基本使用

- 引入必要的依赖包

```html
<body>
    <div id="root"></div>
    <script src="./js/react.development.js"></script>
    <script src="./js/react-dom.development.js"></script>
    <script src="./js/babel.min.js"></script>
</body>
```

- 创建虚拟dom对象,将虚拟dom渲染到页面

```js
<script type="text/babel">
	// 创建虚拟dom对象
	const vd = <h1>你好</h1>
	
	// 将虚拟dom渲染到页面
	ReactDOM.render(vd,document.getElementById('root'))
</script>
```

- 创建虚拟Dom的2种方法

  1. 使用js语法 ： React.createElement(tagName,props,content)

     ```js
     const vd = React.createElement('h1',{id:'app'},"你好")
     ```

  2. 使用jsx语法

     ```js
     let content = "你好"
     const vd = <h1 id={id:'app'}>{content}</h1>
     ```

### 定义组件

- 工厂函数组件

  ```react
  function FunComponents() {
  	return <h1>你好</h1>
  }
  
  ReactDOM.render(<FunComponents />,document.getElementById('root'))
  ```

  

- ES6组件

  ```react
  class ClassComponents extends React.Component {
      render() {
          return <h1>你好</h1>
      }
  }
  
  ReactDOM.render(<ClassComponents />,document.getElementById('root'))
  ```

### 组件三大属性

#### props 

- 在组件外部，向组件标签传入标签属性
- 在组件内部，读取标签属性

```react
class ClassComponents extends React.Component {
    render() {
        //在组件内部，读取标签属性
        const {name , age , sex} = this.props
        return (
            <ul>
                <li>{name}</li>    
                <li>{age}</li>    
                <li>{sex}</li>    
            </ul>
        )
    }

}

let obj = {
    name : "tank",
    age : 28 ,
    sex : "男"
}

//在组件外部，向组件标签传入标签属性
ReactDOM.render(<ClassComponents name={obj.name} age={obj.age} sex={obj.sex}/> ,document.getElementById('root'))
```

- 限定属性的数据类型

  引入依赖包

  ```html
  <script src="./js/prop-types.js"></script>
  ```

  限定传入的属性类型

  ```js
  ClassComponent.propTypes = {
      name : PropTypes.string.isRequired ,
      age : PropTypes.number,
      sex : PropTypes.string
  }
  ```

  限定属性的默认值

  ```js
  ClassComponent.defaultProp = {
      age : 20 ,
      sex : "男"
  }
  ```

  一次性传入多个属性

  ```react
  let obj = {
      name : "tank",
      age : 28 ,
      sex : "男"
  }
  
  //在组件外部，向组件标签传入标签属性
  ReactDOM.render(<ClassComponents {...obj}/> ,document.getElementById('root'))
  ```

  

  **最终写法**

  ```react
  class ClassComponents extends React.Component {
      static propTypes = {
          name : PropTypes.string.isRequired ,
          age : PropTypes.number,
          sex : PropTypes.string
      }
      static defaultProp = {
          age : 20 ,
          sex : "男"
      }
      render() {
          //在组件内部，读取标签属性
          const {name , age , sex} = this.props
          return (
              <ul>
                  <li>{name}</li>    
                  <li>{age}</li>    
                  <li>{sex}</li>    
              </ul>
          )
      }
  }
  let obj = {
      name : "tank",
      age : 28 ,
      sex : "男"
  }
  ReactDOM.render(<ClassComponents {...obj}/> ,document.getElementById('root'))
  ```

  

#### refs 标识组件

```react
// （不推荐使用）
class ClassComponents extends React.Component {
    changeStr = () => {
        // 获取到input标签
        let str = this.refs.content
        console.log(str.value)
    }
    render() {
        return (
            <div>
                {/*标识input标签 */}
                <input type="text" ref="content"/>
                <button onClick={this.changeStr}> 点我获取数据 </button>    
            </div>
        )
    }
}
```



**使用ref容器代替ref**

1. 创建ref容器对象
2. 将ref容器传递给要标识的标签
3. 通过ref容器读取得到ref对象

```react
class ClassComponents extends React.Component {
    // 创建ref容器
    myRef = React.createRef()

    changeStr = () => {
        // 通过ref容器读取得到ref对象
        let str = this.myRef.current
        console.log(str.value)
    }
    render() {
        return (
            <div>
                {/* 将ref容器传递给要标识的标签 */}
                <input type="text" ref={this.myRef}/>
                <button onClick={this.changeStr}> 点我获取数据 </button>    
            </div>
        )
    }
}
```

**使用回调函数代替**

```react
class ClassComponents extends React.Component {
    changeStr = () => {
        // 通过回调函数获取标签对象
        let str = this.getInput
        console.log(str.value)
    }
    render() {
        return (
            <div>
                {/* 将ref传递一个回调函数*/}
                <input type="text" ref={ input => this.getInput = input }/>
                <button onClick={this.changeStr}> 点我获取数据 </button>    
            </div>
        )
    }
}
```



#### state 状态数据

- 初始状态数据
- 读取状态数据
- 更新状态数据

```react
class ClassComponents extends React.Component {    
    // 初始状态数据（简写）
    state = {
        str : "你好"
    }

    // 更新状态数据
    changeStr = () =>{
        this.setState({
            str : "hello"
        })
    }
    
    render() {
        // 读取状态数据
        return <h1 onClick={this.changeStr}>{this.state.str}</h1>
    }
}

ReactDOM.render(<ClassComponents />,document.getElementById('root'))
```

```react
// 初始状态数据(详细写法 ,不常用)
constructor(){
    super()
    this.state={
        str : "你好"
    }
}
```



### 组件中处理事件

```react
class ClassComponents extends React.Component {
    getInput = (e) => {
        // 通过event对象可以直接获取到事件对象
        let value = e.target.value
        console.log(value);
    }
    
    render() {
        return (
            <div>
                {/* 失去焦点事件，在组件中不需要调用，react会自动调用 */}
                <input type="text" onBlur={this.getInput}/>
            </div>
        )
    }
}
```

### 受控组件与非受控组件

- 非受控组件

  > 输入过程中不收集数据，点击提交才读取输入框的数据（用的少）

- 受控组件

  > 输入过程收集数据（state + onChange）,点击提交只需要读取state数据

  ```react
  class ClassComponents extends React.Component {
      state = {
          value : "hello"
      }
      getInput = (e) => {
          // 触发后修改state状态
          this.setState({
              value : e.target.value
          })
      }
      render() {
          let {value} = this.state
          return (
              <div>
                  {/* 通过onChange事件对state进行状态改变 */}
                  <input type="text" value={value} onChange={this.getInput}/>
              </div>
          )
      }
  }
  
  ```

### 组件的生命周期

1. 初始阶段

   - 【构造器 ： 创建实例对象】

     > constructor (props) {
     >       super(props)
     > }

   - 【挂载前】

     > componentWillMount(){}

   - 【★渲染 ：必须有，返回要渲染显示的虚拟DOM】

     > render(){
     >     return 
     >
     > }

   - 【★挂载后 ：执行异步任务，ajax请求,定时器，绑定监听，订阅消息】

     > componentDidMount(){}

2. 更新阶段

   - 【★ 更新阶段  ：是否要更新下面的三个方法 ， 组件优化时使用】

     > shouldComponentUpdate(){
     >     return true
     > }

   - 【数据准备更新前】

     > componentWillUpdate(){}

   - 【★渲染】

     > render(){
     >     return 
     >
     > }

   - 【数据更新后】

     > componentDidUpdate(){}

3. 卸载阶段

   - 【★销毁前,卸载前 ： 清除定时器，解绑监听，取消订阅】

     > componentWillUnmount(){}

4. 父组件有传入数据

   - 【是否会获取到新的数据】

     > componentWillReceiveProps(){}



### 事件传参（高阶函数）

- 高阶函数

  1. 返回函数的函数
  2. 接收的函数参数的函数

  ```react
  class ClassComponents extends React.Component {
      change = (text) => {
          // 返回一个函数
          return (e) =>{
              console.log("传入的参数是"+text);
              console.log("事件对象是"+e);
          }
      }
      render() {
          return (
              <div>
                  {/* 事件中定义参数*/}
                  <button onClick={this.change("你好")}></button>
              </div>
          )
      }
  }
  ```

  

- 函数柯里化

  - fn()()()



## React 环境配置

1. 最下方提供的package.json和webpack.config.js配置好后运行npm install

2. 安装react和react-dom

   ```shell
   npm install react react-dom
   ```

3. 下载react相关包

   ```shell
   npm install @babel/preset-react -D
   ```

4. index.js入口文件中引入

   ```react
   import React from 'react'
   import ReactDOM from 'react-dom'
   import App from './App.jsx'
   
   ReactDOM.render(<App/>,document.getElementById('root'))
   ```

## React脚手架

1. 下载脚手架（如果已下载可直接跳过）

   ```shell
   npm install -g create-react-app
   ```

2. 通过脚手架创建react项目

   ```shell
   create-react-app 项目名称
   ```

3. 运行

   ```shell
   yarn start
   ```

4. 打包

   ```shell
   npm run build
   ```

   

## 组件间通信

### props

> 作用：父子组件间相互通信
>
> > **父向子：非函数props ，**
> >
> > *父组件将数据交给子组件是让子组件读取显示，不能直接更新*
> >
> > ```react
> > let obj = {
> >     name : "tank",
> >     age : 28 ,
> >     sex : "男"
> > }
> > 
> > //父组件向子组件标签传入标签属性
> > ReactDOM.render(<ClassComponents name={obj.name} age={obj.age} sex={obj.sex}/> 
> > ```
> >
> > **子向父：函数props**
> >
> > *子组件调用父组件传入的函数*，将数据以参数的形式传递给父组件，从而更新父组件数据
> >
> > ```react
> > class ClassComponents extends React.Component {
> >     render() {
> >         //子组件内部，读取标签属性
> >         const {name , age , sex} = this.props
> >         return (
> >             <ul>
> >                 <li>{name}</li>    
> >                 <li>{age}</li>    
> >                 <li>{sex}</li>    
> >             </ul>
> >         )
> >     }
> > 
> > }
> > ```



### refs

> 作用：父组件得到子组件对象
>
> *父组件可以获取子组件的state状态数据和子组件定义的方法*
>
> ```react
> // （不推荐使用）
> class ClassComponents extends React.Component {
>     changeStr = () => {
>         // 获取到input标签
>         let str = this.refs.content
>         console.log(str.value)
>     }
>     render() {
>         return (
>             <div>
>                 {/*标识input标签 */}
>                 <input type="text" ref="content"/>
>                 <button onClick={this.changeStr}> 点我获取数据 </button>    
>             </div>
>         )
>     }
> }
> ```



### 兄弟间通信PubSub

> Pubsub （任意组件间都可通信）
>
> > 引入Pubsub
> >
> > ```js
> > import PubSub from 'pubsub-js'
> > ```
> >
> > 订阅消息（接收数据）
> >
> > ```js
> > PubSub.subscribe('msgName' , (msgName , data ) => { } )
> > 
> > // 一般在加载完成订阅
> > componentDidMount() {
> > 	const token = PubSub.subscribe('msgName' , (msgName , data ) => { } )
> > }
> > ```
> >
> > 发布消息（发送数据）
> >
> > ```js
> > PubSub.publish('msgName' ,data)
> > ```
> >
> > 取消订阅
> >
> > ```js
> > PubSub.unsubscribe( token/msgName )
> > 	// token	只能取消单个订阅
> > 	// msgName 可以取消同名的多个订阅
> > 
> > // 一般在组件卸载前取消订阅
> > componentWillUnmount(){
> >     PubSub.unsubscribe( token/msgName )
> > }
> > ```



### Redux

> 任意组件间通信，功能比pubsub更强大，用来管理多个组件共享状态
>
> 详细记录在下面大纲中



### Context

> 组件间通信方式，常用用于祖孙组件间相互通信
>
> 利用Provide来向后代组件提供数据
>
> 后代组件通过Cunsumer来读取数据，也可以调用传递过来的函数

1. 创建Context容器对象

   ```react
   const xxxContext = React.createContext()
   ```

   

2. 在祖组件最外层包上Provider，传递给后代组件的数据与接收后代组件的函数

   ```react
   setAge = (num) =>{
       console.log("接收后代组件num值为"+num)
   }
   
   render(){
       // 通过context属性可以获取后代组件传递过来value数据对象
       const {name} = this.state
   	return (
           {/* 通过value属性传递给后代组件数据 */}
   		<xxxContext value={{name , this.setAge }}>
           	<Children></Children>
           </xxxContext>
   	)
   }
   ```

   

3. 后代组件读取数据

   - 类组件接收

     ```react
     class Children extends Component{
         // 声明接收context数据，会自动将context容器对象中的value数据保存到当前组件的context属性中
     	static contextType = xxxContext
     
     	render(){
             const {name , setAge} = this.context
     		return(
         		<div onClick={()=>setAge(123)} >{name}</div>
         	)
         }
     	
     }
     ```

   - 函数组件接收

     ```
     
     ```

   



### state数据定义的位置

- prop方案

  > 状态提升 ：如果是某个组件用就放在这个组件内，如果是某些组件用就放在共同的父组件中

- pubsub方案

  > 数据放在需要显示的组件内，在通过pubsub分发需要这个数据的其组件

- redux方案

  > 多个组件共享（用）的数据交给redux管理：有的组件要读取，有的组件要更新，都需要找redux的store对象



## React路由

### 下载安装

```shell
npm install react-router-dom
```

### 组件

<BrowserRouter>   history模式路由组件

<HashRouter>	hash模式路由组件

<Route>	注册路由组件

<Redirect>  重定向路由组件，一般放在最后，前面的路由都不匹配则默认匹配

<Link>  跳转路由组件

<NavLink>  带样式的跳转路由组件，当前路由连接就会有特定类名

<Switch>  选择唯一路由组件

### 对象

- history 对象:包含控制路由跳转的一些方法，如：push()  /  replace()  / goBack()
  - push
  - replace
  - goBack
- match 对象 : 包含路由相关信息的对象
  - params
- location 对象 ：包含路由相关信息的对象
  - pathname 
  - search
  - state
- withRouter 函数

### 路由的基本使用

1. 引入需要的路由组件

2. 在外层用上<HashRouter> 或 <BrowserRouter>

3. 注册路由 ： 在需要显示路由组件界面的区域使用<Route>注册

4. 通过<Switch>来对路由进行选择

5. 通过<Redirect>来设置默认显示路由

   ```react
   // 引入需要的路由组件
   import {BrowserRouter, Route , Switch, Redirect } from 'react-router-dom'
   
   // 引入相关组件
   import Home from './components/Home'
   import About from './components/About'
   
   export default class App extends Component {
       render() {
           return (
           {/* 在外层用上<HashRouter> 或 <BrowserRouter> */}
           <BrowserRouter>
            	{/* 通过<Switch>来对路由进行选择 */}  
   			<Switch>
                   {/* 注册路由 ： 在需要显示路由组件界面的区域使用<Route>注册 */}
   				<Route path="/about" component={About} />
   				<Route path="/home" component={Home} />
                   {/* 通过<Redirect>来设置默认显示路由 */}
                   <Redirect to="/about" />
               </Switch>
           </BrowserRouter>
           )
       }
   }
   ```

### 嵌套路由

> 用法和一级路由方式一样，
>
> ```react
> import React, { Component } from 'react'
> import {Switch,Route,Redirect,NavLink} from 'react-router-dom'
> 
> import News from '../News'
> import Message from '../Message'
> 
> export default class Home extends Component {
>     render() {
>         return (
>             <div>
>                 <h2>Home 组件内容</h2>
>                 <ul className="nav nav-tabs">
>                     <li><NavLink to="/home/news">News</NavLink></li>
>                     <li><NavLink to="/home/message" >Message</NavLink></li>
>                 </ul>
>                 <div>
>                     {/* 注册二级路由 */}
>                     <Switch>
>                         <Route path="/home/news" component={News} />
>                         <Route path="/home/message" component={Message} />
>                         <Redirect to="/home/news" />
>                     </Switch>
>                 </div>
>             </div>
>         )
>     }
> }
> ```



### 封装自定义NavLink

```react
import React, { Component } from 'react'
import {NavLink} from 'react-router-dom'

// 对NavLink进行二次封装
// 将接收的所有props都传递给我的子组件NavLink ==> 透传
// 组件一个特别的props : children  标识组件标签体内容（字符串，标签对象，标签对象的数组）
export default  function MyNavLink(props) {
    return <NavLink activeClassName="myActive" {...props} />
}
```



### 路由跳转的两种方式

- 声明式跳转：通过路由链接点击跳转，用于跳转前没有逻辑处理的场景

- 编程式跳转：通过history.push()/replace() ,用于跳转前有逻辑处理的场景

  ```react
  pushTo = (id) =>{
  	return ()=>{
  		// 编程式路由跳转
  		this.props.history.push(`/home/${id}`)
  	}
  }
  
  render(){
  	return(
  		<button onClick={this.pushTo("123")}>跳转</button>
  		<button onClick={() => this.props.history.goBack()}>返回</button>
  	)
  }
  ```

  

### 路由组件传参

1. params

   > 注册路由时，path最后指定一个param参数
   >
   > ```react
   > <Route path="/home/:id" component={Home} />
   > ```
   >
   > 跳转路由时传入参数
   >
   > ```react
   > <Link to="/home/123" >跳转</Link>
   > ```
   >
   > 获取路由参数
   >
   > ```react
   > export default class Message extends Component {
   >     render() {
   >         const id = this.props.match.params.id
   >         return <h2>子路由</h2>
   >     }
   > }
   > //注意：必须在注册路由时指定标识名称
   > ```

2. query

   > 注册路由时，指定一个query参数
   >
   > ```react
   > <Route path="/home?id=123" component={Home} />
   > ```
   >
   > 获取路由query参数
   >
   > ```react
   > // 首先引入qs工具包
   > import qs from 'qs'
   > export default class Message extends Component {
   >     render() {
   >         // 读取query(search)参数，需要手动解析后才能使用
   >         const search = this.props.loaction.search
   >         // 使用qs工具包进行解析,并将？替换成空串
   >         const obj = qs.parse(search.replace('?',''))
   >         let id = obj.id
   >         return <h2>子路由</h2>
   >     }
   > }
   > ```

3. state

   > 携带state数据
   >
   > ```react
   > pushTo = () =>{
   > 	return ()=>{
   >         // 通过编程式跳转可在第二个参数传入任意数据类型
   > 		this.props.history.push(`/home`,{ name:"tank" , age : 28 })
   > 	}
   > }
   > ```
   >
   > 读取state数据
   >
   > ```react
   > export default class Message extends Component {
   >     render() {
   >         // 通过loaction可以获取state数据
   >         const state = this.props.loaction.state
   >         return <h2>子路由</h2>
   >     }
   > }
   > // 只有在history模式才可使用
   > ```

4. props

   > 传入标签属性props参数
   >
   > ```react
   > <Route path="/home" render={()=>(<Home id="xxx"/>)} />
   > ```
   >
   > 读取props数据
   >
   > ```js
   > export default class Message extends Component {
   >  render() {
   >      // 通过props获取
   >      const id = this.props.id
   >      return <h2>子路由</h2>
   >  }
   > }
   > ```



### 包装非路由组件

> 使用withRouter函数， 接收一个组件（函数），返回一个新的组件（函数），是一个高阶组件
>
> 作用：向非路由组件传递三个属性（history,location,match）
>
> ```react
> import React, { Component } from 'react'
> import {NavLink, withRouter} from 'react-router-dom'
> 
> function MyNavLink(props) {
>  return <NavLink activeClassName="myActive" {...props}></NavLink>
> }
> 
> // 向外暴露新的路由组件函数
> export default withRouter(MyNavLink)
> 
> ```
>
> NavLink自定义路由组件中当被激活后会生成一个className "active"，当然也可以修改这个类名，我们可以通过这个类名来对被激活的按钮或链接进行样式的修饰



## React路由表

### 路由匹配的2种模式

1. 模糊匹配（默认）：请求的路径只是前面部分与路由的path匹配 ，对于多层嵌套的路由时必要的

2. 完全匹配：只请求路径与路由的path完全相同才匹配 

   ```react
   // 设置完全匹配
   <Route exact/>
   ```

### 集中式

1. 集中式配置项目中所有路由相关的信息（路由/组件/子路由的数组/链接名称）

   ```react
   // 在src目录下创建一个config文件夹，在routes.js文件中配置以下信息
   
   import Welcome from '@/pages/Welcome'
   import Frontend from '@/pages/Welcome/Frontend'
   import Backend from '@/pages/Welcome/Backend'
   
   export default [
       {
           path : '/welcome', // 路由
           component : Welcome, // 组件 
           name : '首页', // 链接名称
           children :[ // 子路由的数组r
               {
                   path:'/welcome/frontend',
                   component:Frontend,
                   name : "前端"
               },
               {
                   path:'/welcome/backend',
                   component:Backend,
                   name : "后端"
               }
           ]
       }
   ]
   ```

   > 示范：
   >
   > 创建两个组件分别区分路由链接<Header>和路由组件<Content>
   >
   > ```react
   > import React, { Component } from 'react'
   > import { BrowserRouter} from 'react-router-dom'
   > 
   > import Content from './layout/Content'
   > import Header from './layout/Header'
   > 
   > export default class App extends Component {
   >     render() {
   >         return (
   >            <BrowserRouter>
   >                 <Header />
   >                 <Content />
   >            </BrowserRouter>
   >         )
   >     }
   > }
   > ```

2. 根据配置动态生成路由链接 <NavLink/>

   ```react
   import React, { Component } from 'react'
   import { NavLink } from 'react-router-dom'
   import routes from '../../config/routes'
   
   export default class Header extends Component {
   
       // 根据routes生成一级链接li的数组
       renderLis = () => {
           const li = routes.map(item => (
               <li key={item.path}>
                   <NavLink to={item.path}>{item.name}</NavLink>
               </li>
           ))
           return li
       }
       
       // 根据当前的路径来生成下级的路由链接，得到当前请求的路径，
       // 找到对应的一级路由的配置对象的children ,
       // 根据children数组来生成链接li的数组
       renderChildLis = () =>{
           const path = this.props.location.pathname
           const routeData = routes.find(item => path.startsWith(item.path))
           if(routeData && routeData.children){
               const li = routeData.children.map(item => (
                   <li key={item.path}>
                       <NavLink to={item.path}>{item.name}</NavLink>
                   </li>
               ))
               return li
           }
           return null
       }
       
       render() {
           return (
               <div>
                   <ul>
                       {
                           this.renderLis
                       }
                   </ul>
                   <ul>
                       {
                           this.renderChildLis
                       }
                   </ul>
               </div>
           )
       }
   }
   ```

   

3. 根据配置动态生成路由<Route/>

   ```react
   import React, { Component } from 'react'
   import {Switch , Route, Redirect} from 'react-router-dom'
   import routes from '../../config/routes'
    
   export default class Content extends Component {
   
       // 根据routes配置动态生成路由<Route>数组
       renderRouteTags = () =>{
           const routeTages = []
           // 添加一级链接对应的路由
           routes.forEach(item =>{
               routeTages.push(<Route key={item.path} path={item.path} component={item.component} exact/>)
               // 遍历内层children
               if( item.children && item.children.length > 0 ){
                   children.forEach(cItem =>{
                       // 添加二级链接对应的路由
                       routeTages.push(<Route key={cItem.path} path={cItem.path} component={cItem.component} exact/>)
                   })
               }
           })
           routeTages.push(<Redirect key={routeTages[0].path+"abc"} to={routeTages[0].path}/>)
           return routeTages
       }
       render() {
           return (
               <Switch>
                   {this.renderRouteTags}
               </Switch>
           )
       }
   }
   ```

## Redux

### 下载安装

```shell
yarn add redux
```

### redux模块化

> 创建redux文件夹
>
> > 创建store.js文件 ，redux最核心的管理对象
> >
> > 创建reducer.js文件，包含n个用于当前的state和指定的action计算产生新的state的函数
> >
> > 创建actions.js文件
> >
> > 创建actions-types.js文件

### redux核心API

> createStore()
>
> store对象
>
> > 组件与redux之间沟通的桥梁
> >
> > 内部维护着 state 和 reducer
> >
> > 核心方法
> >
> > - getState() 读取数据
> > - dispatch(action) 分发action
> > - subscribe(listener) 订阅监视
>
> applyMiddleware()
>
> combineReducers() 合并多个reducer

### redux的三个核心概念

> action
>
> - type : 标识属性，值为字符串，唯一，必要属性
> - xxx : 数据属性，值为任意类型，可选属性
>
> > 创建action对象的工厂函数action creator
> >
> > ```react
> > export const sum = (number) =>({type : 'SUM' ,data : number })
> > ```
> >
> > 一般通过action-types.js来定义action type名称
> >
> > ```react
> > // 在action-types.js中定义type名称
> > export const SUM = 'sum'
> > 
> > // 在actions.js中引入
> > import {SUM} from './action-types'
> > export const sum = (number) =>({type : SUM ,data : number })
> > ```
>
> 
>
> reducer
>
> - state ： 当前原本的状态值，第一次调用没有
>
> - action：对象，结构：{type：'标识名称'，data：数据值}
>
>   ```js
>   // 例：管理count数据的reducer函数
>   // 一般将reducer函数的名称指定为要管理数据的名称
>   export default function count(state = 0 , action){
>   	switch(action.type){
>   		case 'SUM' :
>   			return state + action.data
>   		default : 
>   			return state
>   	}
>   }
>   ```
>
>   
>
> store
>
> 1. 引入redux 的createStore创建store函数
>
> 2. 将reducer引入
>
> 3. 创建store对象
>
> 4. 暴露store
>
>    ```js
>    import {createStore} from 'redux'
>    import reducer from './reducer'
>    const store = createStore(reducer)
>    // store 内部管理着2个东西
>    // reducer 函数（接收的）
>    // state 数据： store对象产生时，就会调用一次reducer函数得到初始的state数据
>    export default store
>    ```
>
> 5. 将store传给组件
>
>    ```react
>    import store from './redux/store'
>    ReactDOM.render(<App store={store}/>,document.getElementById('root'))
>    ```
>
> 6. 组件中读取操作store数据
>
>    ```react
>    import React , {Component} from 'react'
>    import PropTypes from 'prop-types'
>    // 引入工厂函数
>    import {sum} from './redux/actions'
>    
>    export default class App extends Component {
>    	static propTypes = {
>    		store : PropTypes.object.isRequired
>    	}
>    }
>    
>    componentDidMount(){
>        // 通过store.subscribe对状态数据变化进行监听，并更新当前组件
>        // 返回的是解绑监听的函数
>        this.unSubscribe = this.props.store.subscribe(()=>{
>            this.setState({})
>        })
>    }
>    
>    componentWillUnmount(){
>        // 组件卸载前解绑监听
>        this.unSubscribe()
>    }
>    
>    add = () =>{
>        // 通过store.dispatch来对数据进行修改
>        // 参数是工厂函数定义的action函数
>        this.props.store.dispatch(sum(123))
>    }
>    
>    render(){
>    	// 通过store.getState()获取count
>    	const count = this.props.store.getState()
>    }
>    
>    ```

### react-redux

下载依赖包

```npm
yarn add react-redux
```

> 作用：专门用来简化react应用中使用redux
>
> - UI组件
>   - 只负责UI的呈现，不带有任何业务逻辑
>   - 通过props接收数据（一般数据和函数）
>   - 不适用任何Redux的API
>   - 一般保存在components文件夹下
> - 容器组件
>   - 负责管理数据和业务逻辑，不负责UI的呈现
>   - 使用Redux的API
>   - 一般保存在containers文件夹下
> - 相关API
>   - Provider 让所有组件都可以得到state数据
>   - connect() 用于包装UI 组件生成容器组件
>   - mapStateToprops() 将外部的数据（即state对象)转换为UI组件的标签属性
>   - mapDispatchToPropos() 将分发action的函数转换为UI组件的标签属性，简洁语法可以直接指定为actions对象或包含多个action方法的对象

操作步骤

1. 在入口index.js中引入react-redux的Provider 组件

   ```react
   import {Provider} from 'react-redux'
   ```

2. 用Provider 包裹需要store对象的容器组件

   ```react
   import store from './redux/store'
   ReactDOM.render(
       <Provider store={store}>
       	<App/>
       </Provider>
       ,document.getElementById('root')
   )
   ```

3. 定义UI组件，组件内部只接收一般属性和更新函数，不涉及任何redux函数

   ```react
   import React , {Component} from 'react'
   import PropTypes from 'prop-types'
   
   export default class App extends Component {
   	static propTypes = {
   		count : PropTypes.number.isRequired,
           sum : PropTypes.func.isRequired,
   	}
   }
   
   add = () =>{
       // 直接调用更新函数
       this.props.sum(123)
   }
   
   render(){
       // 获取count
   	const count = this.props.count
   }
   ```

4. 在组件中创建容器组件

   ```react
   import {connect} from 'react-redux'
   
   class App extends Component{...}
   
   // 定义容器组件
   const ContainerComp = connect(
   	state => ({count}),
       {sum}
   )(App)
   export default ContainerComp
   ```

### redux异步编程

下载redux异步中间件

```shell
yarn add redux-thunk
```

> 在store.js中引用redux异步中间件
>
> ```react
> import {createStore , applyMiddleware} from 'redux'
> import thunk from 'redux-thunk'
> 
> import reducer from './reducer'
> const store = createStore(
>  reducer,applyMiddleware(thunk)
> )
> 
> export default store
> ```
>
> 在action.js文件中处理异步action，将对象action变成函数action
>
> ```react
> export const syncAsync = (number) => {
> 	return dispatch =>{
>         // 执行异步操作
> 		setTimeout(()=>{
>             // 只有分发同步action才会触发reducer调用
> 			dispatch(sum(number))
> 		},2000)
> 	}
> }
> ```



### 合并多个reducer函数

> 管理多个状态数据，创建多个reducer函数
>
> 引入redux 的 combineReducers函数
>
> ```react
> import {combineReducers} from 'redux'
> ```
>
> 合并多个reducer生成一个总的reducer
>
> ```react
> function count(state = 0 , action){
> 	...
> }
> 
> function user(state = 0 , action){
> 	...
> }
> 
> export default combineReducers({
>     count , user
> })
> ```
>
> 组件中引入对应的reducer状态数据
>
> ```react
> export default connect(state => ({
> 	count : state.count
> }))(App)
> ```



### redux 配置多个代理

> 在src目录下创建一个文件setupProxy.js
>
> ```js
> const {createProxyMiddleware} = require('http-proxy-middleware')
> 
> module.exports = function(app){
> 	app.use(
> 		createProxyMiddleware(
> 			'/api',
> 			{
> 				"target" : "https://m.you.163.com" ,
> 				"changeOrigin" : true ,
> 				"pathRewrite" : {
> 					"^/api" : ""
> 				}
> 			}
>         )
>        )
> }
> ```
> 
> 在入口文件index.js文件中引入setupProxy文件
> 
> ```js
> require('./setupProxy.js')
> ```
> 
> 组件发送axios请求
> 
> ```js
>axios.get('/api/home').then(...).catch(...)
> ```



### 装饰器

> 作用：简化高阶组件的使用
>
> 下载装饰器包
>
> ``` shell
> yarn add @babel/plugin-proposal-decorators -D
> ```
>
> 在config-overrides.js中引入
>
> ```js
> const {addDecoratorsLegacy} = require('customize-cra')
> 
> module.exports = override (
>     //添加装饰器配置
> 	addDecoratorsLegacy()
> )
> ```
>
> 在webpack.config.js配置文件中修改plugins配置
>
> ```
> plugins:[
> 	["@babel/plugin-proposal-decorators",{"legacy":true}],
> 	["@babel/plugin-proposal-class-properties",{"loose":true}]
> ]
> ```
>
> 将：
>
> ```react
> class App extends Component { ... }
> 
> const ContainerComp = connect(
> 	state => ({count}),
>     {sum}
> )(App)
> 
> export default ContainerComp
> ```
>
> 转变为：
>
> ```react
> @connect(
> 	state => ({count}),
>     {sum}
> )
> class App extends Component { ... }
> 
> export default App
> ```
>
> 装饰器会自动执行@符号后的高阶组件函数，并把下面的class类进行赋值



## Fragments

文档碎片，不会进入页面

>  语法 ：<></>

```react
render(){
	return 
	<>
    	<p>你好</p>
    	<p>你好</p>
    	<p>你好</p>
    </>
}
```



# React 高级

## props能不能直接修改

- 在react中，props属性是属于readonly属性，官方不建议通过props属性修改属性值
- 如果直接修改props属性的基本数据类型，那程序直接报错
- 如果修改的是props属性的引用数据类型中的属性，那么不会报错
- 直接修改props属性并不会更新视图

> 在Vue中
>
> - 修改props属性的基本数据类型，控制台会发出警告提示，但不会触发视图的更新
> - 修改props属性的引用数据类型，程序不会报错，但会触发视图的更新
> - 如果需要在子组件中通过props修改数据，可以将原数据进行深度拷贝，但一般我们不会去这样的操作

## Refs转发

> 语法： React.forwardRef( FunctionComp )
>
> 作用：在函数组件外部通过ref得到函数组件内部的标签，从而操作其内部标签
>
> 应用：Link/NavLink就使用了此技术来封装，来在组件外部获取操作内部包含的a标签

1. 利用forwardRef()包装函数组件生产一个新组件
2. 将容器交给子组件标签

```react


class App extends Component {
    // 创建一个ref容器
    funRef = React.createRef()
	
	getEle = () =>{
       // 通过容器的current方法可以获取到被标识的标签（获取到了下面的div）
       console.lof(this.funRef.current.innerHTML) 
    }

	render(){
		return(
			<>
            	<button onClick={this.getEle}>点击获取子组件</button>
            	{/* 将容器交给子组件标签 */}
            	<childrenComponent ref={this.funRef}></childrenComponent>
            </>
		)
	}
}

// 利用forwardRef()包装函数组件生产一个新组件
const childrenComponent = React.forwardRef(
    // 高阶组件会将接收的ref容器传递给函数组件的第二个参数
	function Children(props,ref){
   		return (
    		<>
            	<div ref={ref}> 被标识的标签内容 </div>
            </>
    	)
	}
)
```



## 组件优化

Component 组件的2个问题

1. 只要执行setState(),即使状态数据state没有改变，组件也会更新执行render()
2. 组件重新执行render()，就会让子组件也重新执行render(), 导致效率低

原因：

> 声明周期中的shouldComponenetUpdate()方法总是返回true，只要当前组件或父组件setState(),都会重新render()

解决办法

1. 重写shouldComponenetUpdate()，比较新旧state和props数据，如果有变化才返回true，否则返回false

   ```react
   shouldComponenetUpdate(nextProps,nextState){
   	// nextProps表示新的props对象，nextState表示新的state对象
       if(nextState.xx === this.state.xx && nextProps.xxx === nextProps.xxx ){
           return false
       }
       return true 
       
       //这种办法只是对数据进行浅比较，只会比较引用地址值，而不比较引用对象内部的数据
   }
   ```

2. 使用PureComponent

   > 原理：重写shouldComponenetUpdate，函数内部比较新旧state和props数据，如果有变化才返回true，否则返回false

   ```react
   // 1.首先引入PureComponent
   import React,{Component , PureComponent} from 'react'
   
   // 2.创建组件继承PureComponent
   class Children extends PureComponent { ... }
   ```



## render props渲染属性

<u>如何向组件内部动态传入带内容的结构（标签）？</u>

> Vue中 ： 使用slot插槽技术
>
> React中：使用children props :通过组件标签体传入结构，和使用render props:通过组件标签属性传入结构，一般用render函数属性

- children props使用方式（这种方式父组件无法向子组件传递状态数据）

  ```react
  export default class App extends Component {
  	render(){
  		return (
  			<>
  				<A>
              		<B></B>
              	</A>
  			</>
  		)
  	}
  }
  
  class A extends Component {
      render(){
          return (
          	<>
              	<h1>标题</h1>
              	{/* 通过children属性可以把B组件传入 */}
              	{this.props.children}
              </>
          )
      }
  }
  
  class B extends Component {
      render(){
          return (
          	<>
              	<p>段落</p>
              </>
          )
      }
  }
  ```

- 使用render props方式

  ```react
  import ProTypes from 'prop-types'
  export default class App extends Component {
  	render(){
  		return (
  			<>
              	{/* 使用render props，传递接收的状态数据 */}
  				<A render={(name) = > <B name={name}/>}></A>
  			</>
  		)
  	}
  }
  
  class A extends Component {
      // 接收render函数
      static propTypes = {
          render : PropTypes.func
      }
  	state = { name : 'tank' }
  
      render(){
          return (
          	<>
              	<h1>标题</h1>
              	{/* 通过()属性可以把B组件传入,并将状态数据传入 */}
              	{this.props.render(this.state.name)}
              </>
          )
      }
  }
  
  class B extends Component {
      // 获取A组件的state状态数据
      static propTypes = {
          name : PropTypes.String
      }
      render(){
          return (
          	<>
              	<p>段落</p>
              </>
          )
      }
  }
  ```
  - 在一些简单的路由界面，我们可以在定义路由时直接用render属性

    ```react
    <Router path="/..." render={() => <div>路由组件</div> } />
    ```



## 路由懒加载

> 作用：被打包的路由组件会被code split（代码分割）单独打包，只有当第一次请求路由时才会去后台加载其对应的打包文件

1. 引入react中的lazy函数

   ```react
   import {lazy} from 'react'
   ```

2. 在导入路由之前用lazy函数进行处理

   ```react
   const Home = lazy(() => import('@/pages/Home'))
   ```

3. 在路由组件外设置loading的组件

   ```react
   // 首先引入Suspense组件
   import React ,{Component , Suspense} from 'react'
   import {Switch,Route} from 'react-router-dom'
   
   render(){
   	return (
   		<>
           	{/* 设置加载时的loading组件 */}
           	<Suspense fallback={<div>Loading...</div>}>
           		<Switch>
               		<Route path="/..." component={Home} />
               	</Switch>
           	</Suspense>
           </>
   	)
   }
   ```

   

## HOOK

> 作用： 让函数组件也可以有自己状态一级生命周期的处理

### useState()

> useState( initValue ) 在内部生成一个状态数据，初始值是initValue，返回状态数据和用于更新此状态数据的函数
>
> 第一个元素是状态数据值
>
> 第二个元素是用于更新此状态数据的函数
>
> 注意：第一次调用回将initValue作为内部状态数据的初始值，后面再调用就会得到内部保存的状态数据值
>
> 首先引入useState()
>
> ```js
> import React,{ Component , useState } from 'react'
> ```
>
> 示例
>
> ```react
> function Demo(props){
> 	const [count , setCount] = useState(1000)
>     
>     return (
>     	<div onClick={()=> setCount(count + 1)}>点击coun值t加1</div>
>     )
> }
> ```



### useEffect()

> 作用：可以在函数组件中执行副作用操作
>
> - 异步操作：启动定时器、发送ajax请求、绑定监听、消息订阅
> - 直接操作DOM
> - 操作浏览器的数据
>
> useEffect( callback() , [] )相当于componentDidMount(),初始显示后执行一次
>
> > 第二个参数指定需要观察的可变数据，一旦数据发送变化回调函数就会重新执行，如果是空数组，则只会执行一次
> >
> > 返回的函数相当于componentWillUnmount()
>
> 引入
>
> ```js
> import React,{ Component , useState,  useEffect } from 'react'
> ```
>
> 示例
>
> ```react
> function Demo(props){
> 	const [count , setCount] = useState(1000)
>     useEffect(()=>{
>     	let clear = setTimeout(()=>{ 
>             // 注意： 直接调用setCount( count )
>             // 如果中间有更新，也不能得到最新的count值，依旧是计时器后的状态
>             // 所以应该指定回调，通过参数获取count最新的值
>         	setCount( count => count ++)
>         },5000)
>         
>         return ()=>{
>             clearTimeout(clear)
>         }
>     })
>     return (
>     	<div></div>
>     )
> }
> ```



### useRef()

> 作用：
>
> - 标识组件中的标签
> - 保存可变属性数据的容器
>
> 引入
>
> ```js
> import React,{ Component ,  useRef } from 'react'
> ```
>
> 示例1
>
> ```react
> function Demo(props){
> 	
>     const inputRef = useRef(null)
>     
>     return (
>     	<input type="text" ref={inputRef}/>
>         <button onClick={()=> inputRef.current.focus()}>点击自动获取焦点</button>
>     )
> }
> ```
>
> 示例2
>
> ```react
> function Demo(props){
> 	// 初始current的值为10
>     const numRef = useRef(10)
>     const addRef = ()=> {
>         numRef.current++
>         console.log(numRef.current)
>     }
>     return (
>         <button onClick={ addRef() }>点击current的值加1</button>
>     )
> }
> ```
>
> 

### useContext()

> 作用：在函数组件中得到Context容器对象中的value数据
>
> 引入
>
> ```js
> import React,{ Component ,  useContext } from 'react'
> ```
>
> 示例
>
> ```react
> // 父组件创建容器对象
> const HookContext = React.createContext()
> 
> exprot default class Demo extends Component{
> 	render(){
>        return (
>        	<HookContext.Provider value={"test"}>
>         	<Demo1></Demo1>  
>         </HookContext.Provide>
>        ) 
>     }
> }
> ```
>
> ```react
> function Demo1(props){
>     // 子组件/后代组件读取Demo父组对象中的value数据
>     const value = useContext(HookContext)
>     return(
>     	<div>{value}</div>
>     )
> }
> ```
>



## 自定义Redux

### createStore

> 创建store对象，管理state数据的函数

```js
export function createStore(reducer){
	// 内部保存当前state数据 ,初始状态数据
    let currentState = reducer(undefined , {type : "@@redux/INIT a . b . c"})
    // 内部保存listener的容器数组
    let listeners = []
    
    // 得到内部保存的当前state数据
    function getState(){
        return currentState
    }
    
    // 分发action，调用reducer产生新的state数据，并保存为当前的state
    function dispatch(action){
        currentState = reducer(currentState,action)
        // 通知所有保存的listener
        listeners.forEach(listener => listener())
    }
    
    // 订阅监听，监视内部state数据的改变
    function subscribe(listener){
        listeners.push(listener)
        // 返回用于解绑监听的函数
        return () => listeners.splice(listeners.indexOf(listener),1)
    }
    
    // 返回store对象
    return {
        getState,
        dispatch,
        subscribe
    }
}
```



### combineReducers

> 合并多个reducer返回一个新的总reducer

```js
export function combineReducers(reducers){
    // 简洁写法
	return (totalState={} , action) =>{
        return Object.keys(reducers).reduce((pre,ket)=>{
            pre[key] = reducers[key](totalState[key],action)
            return pre
        },{})
    }
    
    // 详细写法
    return (totalState={} , action)=>{
        const newTotalState = {}
        Object.keys(reducers).forEach(key =>{
        	// 得到对应的reducer
        	const reducer = reducers[key]
        	// 得到对应的子state
        	const state = totalState[key]
        	// 调用reducer得到新的子state
        	const newState = reducer(state,action)
        	// 保存到新的总state对象中
        	newTotalState[key] = newState
    	})
    	return newTotalState
    }
}
```



## 自定义react-redux

### Provider组件

> 作用：将接收的store全局共享给所有的容器组件（Provider的后代组件）
>
> 使用：<Provider store={store}><App/></Provider> 

```react
// 创建Context容器对象StoreContext，用于传递store对象
const StoreContext = React.createContext()

export class Provider extends Component{
    static propTypes = {
        store : Protypes.object.isRequired
    }

	render(){
		const {store , children} = this.props 
        return (
        	//通过<StoreContext.Provider>向后代组件提供store
            <StoreContext.Provider value={store}>
                {/* 这个children相当于Provider的子组件*/}
            	{children}
            </StoreContext.Provider>
        )
	}
}
```



### connect函数

```js
export function connect( mapStateToProps , mapDispatchToProps ){
	// 返回一个高阶组件函数
    return (UIComponent) => {
        // 返回一个容器组件
        return class extends Component {
            // 声明一个在调式工具中显示的友好名称
            static displayName = `connect(${UIComponent.displayName || UIComponent.name || 'Component'})`
            
            // 通过StoreContext得到store，从而向UI组件传入特定的一些操作store中的state数据的props 
            // 声明读取context容器对象中value,组件对象就多了context属性，值为store对象
            static contextType = StoreContext
        	
        	// 监听state更新组件
        	conponentDidMount(){
                const store = this.context
                this.unsubscribe = store.subscribe(()=>{
                    this.setState({})
                })
            }
        	componentWillUnmount(){
                this.unsubscribe()
            }
        	
            render () {
                const store = this.context
               	// 确定UI组件传递的所有一般属性
                const stateProps = mapStateToProps(store.getState())
                let dispatchProps
                if(typeof mapDispatchToProps === 'function'){
                    // 如果mapDispatchToProps是函数
                    // 确定向UI组件传递的所有函数属性
                	dispatchProps = mapDispatchToProps(store.dispatch)
                }else{
                    // 如果mapDispatchToProps是对象
                    // 根据mapDispatchToProps中的每个actionCreator函数包装定义一个包含dispatch语句的函数
                    dispatchProps = {}
                    Object.keys(mapDispatchToProps).forEach(key => {
                        dispatchProps[key] = (...args)=> 
                        	store.dispatch(mapDispatchToProps[key](...args))
                    })
                }       
                return <UIComponent {...stateProps} {...dispatchProps}/>
            }
        }
    }
}
```

##  区别真实DOM和虚拟DOM

### 真实DOM元素

- 是一个包含300以上的属性的对象
- 界面的显示就是根据真实DOM树绘制显示的
- 要更新界面，就必须得最终更新真实DOM
- 更新真实DOM就很可能引起界面更新

### 虚拟DOM对象

- 只包含少量属性数据的一般js对象
- 不是真实DOM，但又对应关系，存储了真实DOM 的部分重要信息

- 结构：

  ```js
  {
  	type : 'div' , // 元素类型
      props: {
          id:'app',	// 元素的id属性
          className : 'abc' , // 元素的class属性
          children:[] , // 元素的子节点
          onClick : aaa  // 事件回调函数
      }
  }
  ```

### 为什么要用虚拟DOM

1. 真实DOM操作相对JS对象操作要慢很多
2. 当我们需要对页面上的多个节点进行更新显示时，
   1. 直接多次更新真实DOM来更新呢界面，效率低，反应慢
   2. 用上虚拟DOM并借助DOM diff 算法就可以实现对DOM 更少的更新
3. 注意： 页面初始显示，用上虚拟DOM会更慢些，主要时在更新显示时有优势

### 自定义React虚拟DOM 的创建和渲染

- React.createElement函数

  ```js
  export function createElement( type , props , ...children ){
      if(props === undefined || props === null ){
          props = {}
      }
      if( children.length > 0 ){
          if(children,length === 1){
              props.children = children[0]
          }else{
              props.children = children
          }
      }
      return {
          type ,
          props ,
      }
  }
  ```

  

- React.Component类

  ```js
  export class Component {
      // 标识是类组件
      static isClassComponent = true
  
  	constructor(props){
         	this.props = props        
      }
  }
  ```

  

- ReactDOM.render函数

  ```js
  // 渲染虚拟DOM
  export function render( element , container ){
  	const {type} = element
      if(typeof type === 'string'){
          renderHtmlDom(element , container)
      }else if(typeof type === 'function'){
          if( type.isClassComponent ){
              renderClassDom(element , container)
          }else{
              renderFunctionDom(element , container)
          }
      }else{
          renderText(element , container)
      }
  }
  // renderText渲染文本虚拟DOM
  function renderText(text,container){
      const textNode = document.createTextNode(text)
      container.appendChild(textNode)
  }
  
  // renderHtmlDom渲染标签虚拟DOM
  function renderHtmlDom(htmlDom,container){
      const {type,props} = htmlDom
      const {children,...restProps} = props
      
      const elementNode = document.createElement(type)
      Object.keys(restProps).forEach( key => {
          if(key === 'className'){
              elementNode.setAttribute('class',restProps[key])
          }else if(key.startWith('on')){
              const eventName = key.substring(2).toLowerCase()
              elementNode.addEventListener(eventName,restProps[key])
          }else{
              elementNode.setAttribute(key,restProps[key])
          }
      })
    
      if(children){
          if(Array.isArray(children)){
              children.forEach(child => render(child,elementNode))
          }else{
              render(children,elementNode)
          }
      }
      
      container.appendChild(elementNode)
  }
  
  // renderClassDom 渲染类组件虚拟DOM
  function renderClassDom(classDom,container){
      const {type , props} = classDom
      // 创建类组件实例对象，调用其render方法，得到需要渲染的虚拟DOM
      const vdom = new type(props).render()
      // 渲染得到的虚拟DOM，需要递归调用render
      render(vdom,container)
  }
  
  // renderFunctionDom 渲染函数组件虚拟DOM
  function renderFunctionDom(functionDom,container){
      const {type , props} = functionDom
      // 直接调用组件函数得到需要渲染的虚拟DOM
      const vdom = type(props)
      render(vdom,container)
  }
  
  // 向外暴露ReactDOM对象
  export defalut {
      render
  }
  ```
  



## DOM diff算法

- 虚拟DOM ： 包含了真实DOM相关重要信息数据的一个很小的js对象，也称为DOM树
- DOM Diff ： 比较得到新的虚拟DOM树与旧的虚拟DOM树的差异部分，从而决定只对哪些部分真实DOM进行更新
- DOM Diff算法 ： 而进行DOM Diff操作所使用的技巧或规则，就称之为DOM Diff算法，也就是使用DOM Diff算法进行DOM Diff操作

- 目标
  - 更少次数的差异比较
  - 更少的虚拟DOM差异，更多的真实DOM复用（更少的真实DOM创建/更新）

- 策略

  1. react的diff只做同层级比较
  2. 相同类型的两个组件将会生成相似的树形结构，不同类的两个组件将会生成不同的树形结构
  3. 对于同一层级的一组同一类型子节点，通过分配唯一 id（key值）进行区分

  > 基于以上三个前提策略，React分别对 tree diff 、 component diff 以及 element diff 进行算法优化，事实也证明这三个前提策略是合理且准确的，它保证了整体界面构建的高性能

  - tree diff
    - 概念：讲新旧两颗虚拟DOM树，按照层级对应的关系，从头到尾的遍历一遍，就能找到哪些元素是需要更新的，这种方式：Tree Diff ，（如果出现了DOM节点跨层级的移动操作，性能不好）



## 深入理解SetState

### setState()更新状态的2种写法：

1. setState( stateChangeObj , [callback] ) 对象式的setState（常用）
   1. stateChange为状态改变对象（该对象可以体现出状态的更改）
   2. callback是可选的回调函数，它在状态更新完毕，界面也会更新后（render调用后）才被调用
2. setState( updater , [callback] ) 函数时的setState
   1. updater为返回stateChange对象的函数
   2. updater可以接收到state和props，（state，props） => stateChange
   3. updater接收到的state和props是最新的
   4. callback是可选的回调函数，他在状态更新呢、界面也更新后（render调用后）才被调用

> 总结：
>
> 1. 调用setState后，有俩个连锁反应：react会更新状态，render会被调用
> 2. 对象式的setState是函数式的setState的简写方式
> 3. 使用原则
>    - 如果新状态不依赖于原状态，使用对象方式
>    - 如果新状态依赖于原状态，使用函数式
>    - 如果需要在setState()执行后获取最新的状态数据，要在第二个callback函数中读取



### setState的同步还有异步

setState() 执行位子对其动作的影响

- setState若在由react所控制的回调函数中执行，那么更新状态的动作是【异步】的

  > 生命周期钩子 / react事件监听回调

- setState若在非react控制的异步回调函数中更新的动作是【同步】的

  > 定时器回调 / 原生事件监听回调 / promise回调



关于异步的setState() 多次调用的问题

- 多次调用，如何处理？

  - 若是对象式的setState

    > 多次更新状态的动作合为一次（且以最后一次为准），所以自然也就调用一次render（）去更新界面
    >
    > “更新的动作，和render的调用，均合并为一次，且以最后一次为准”

  - 若是函数式的setState

    > 每次更新的动作都生效（更新动作不合并），但要注意的是：只调用一次render（）更新界面
    >
    > “更新状态的动作没有合并，但render合并了”



# Ant-design

## 下载安装

```shell
yarn add antd --save
```

## 引入css

```js
// 在index.js入口文件种引入 （如果采取按需引入则不用）
import 'antd/dist/antd.css'
```

## 按需引入

1. 下载依赖包

   ```shell
   yarn add react-app-rewired customize-cra babel-plugin-import --dev
   ```

2. 配置package.json启动命令

   ```json
   "scripts": {
     "start": "react-app-rewired start",
     "build": "react-app-rewired build",
     "test": "react-app-rewired test"
   }
   ```

3. 在项目根目录下创建config-overrides.js

   ```js
   const { override, fixBabelImports } = require('customize-cra');
   
   module.exports = override(
     fixBabelImports('import', {
       libraryName: 'antd',
       libraryDirectory: 'es',
       style: 'css',
     }),
   );
   ```

4. 在组件中引入entd的组件

   ```js
   // 例： 引入button组件
   import button from 'antd/es/button'
   ```

## 自定义主题

1. 下载less

   ```shell
   yarn add less less-loader --dev
   ```

2. 修改config-overrides.js配置

   ```js
   //例：修改primary的主题颜色
   const { override, fixBabelImports, addLessLoader } = require('customize-cra');
   
   module.exports = override(
   	fixBabelImports('import', {
   		libraryName: 'antd',
   		libraryDirectory: 'es',
   		style: true,
   	}),
   	addLessLoader({
   		lessOptions:{
   			javascriptEnabled: true,
   			modifyVars: { '@primary-color': '#1DA57A' },
   		}
   	}),
   );
   ```

## 添加提供@别名的函数

1. 修改config-overrides.js配置

   ```js
   const { addWebpackAlias } = require('customize-cra');
   const path = require('path')
   
   module.exports = override(
   	addWebpackAlias({
   		["@"]:path.resolve(__dirname,"src")
   	})
   );
   ```




## 常用组件









# 不常用的方法

## 删除对象属性

```js
delete obj.xxx
```



## 阻止默认行为

```js
fun = (e) => {
	e.preventDefault()
}
```



## 卸载指定组件

> 将组件在页面中移除

```js
ReactDOM.unmountComponentAtNode(document.getElementById('xxx'))
```



## 区别开发环境和生产环境

> 调用方法 ： process.env.NODE_ENV  ，返回结果分别为 "production"和"development"

```js
// 示例 ：
const store = createStore(
 	reducer,
 	process.env.NODE_ENV === 'production' ? applyMiddleware(thunk) :
    composeWithDevTools(applyMiddleware(thunk))
)
```







## Fetch请求

- Fetch是浏览器内部函数
- Fetch文档
  - https://github.github.io/fetch/
  - https://segmentfault.com/a/1190000003810652
- 使用方式

```react
fetch(`/api/search/users?q=${data}`).then(function(res){
    // 首先返回包含响应体json数据的promise
    return res.json()
}).then(function(data){
    // 这里就能直接得到响应体数据，相当于response.data
    console.log(data);
}).catch(function(err){
    console.log(err.message);
})
```

## 离线访问项目

- 引入serviceWorker文件

  ```js
  import * as serviceWorker from './serviceWorker'
  ```

- 调用unregister方法

  ```js
  serviceWorker.unregister()
  ```

- <details>
      <summary>serviceWorker.js</summary>
  <pre>const isLocalhost = Boolean(
    window.location.hostname === 'localhost' ||
      window.location.hostname === '[::1]' ||
      window.location.hostname.match(
        /^127(?:\.(?:25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)){3}$/
      )
  );
  export function register(config) {
    if (process.env.NODE_ENV === 'production' && 'serviceWorker' in navigator) {
      const publicUrl = new URL(process.env.PUBLIC_URL, window.location.href);
      if (publicUrl.origin !== window.location.origin) {
        return;
      }
      window.addEventListener('load', () => {
        const swUrl = `${process.env.PUBLIC_URL}/service-worker.js`;
        if (isLocalhost) {
          checkValidServiceWorker(swUrl, config);
          navigator.serviceWorker.ready.then(() => {
            console.log(
              'This web app is being served cache-first by a service ' +
                'worker. To learn more, visit https://bit.ly/CRA-PWA'
            );
          });
        } else {
          registerValidSW(swUrl, config);
        }
      });
    }
  }
  function registerValidSW(swUrl, config) {
    navigator.serviceWorker
      .register(swUrl)
      .then(registration => {
        registration.onupdatefound = () => {
          const installingWorker = registration.installing;
          if (installingWorker == null) {
            return;
          }
          installingWorker.onstatechange = () => {
            if (installingWorker.state === 'installed') {
              if (navigator.serviceWorker.controller) {
                console.log(
                  'New content is available and will be used when all ' +
                    'tabs for this page are closed. See https://bit.ly/CRA-PWA.'
                );
                if (config && config.onUpdate) {
                  config.onUpdate(registration);
                }
              } else {
                console.log('Content is cached for offline use.');
                if (config && config.onSuccess) {
                  config.onSuccess(registration);
                }
              }
            }
          };
        };
      })
      .catch(error => {
        console.error('Error during service worker registration:', error);
      });
  }
  function checkValidServiceWorker(swUrl, config) {
    fetch(swUrl, {
      headers: { 'Service-Worker': 'script' },
    })
      .then(response => {
        const contentType = response.headers.get('content-type');
        if (
          response.status === 404 ||
          (contentType != null && contentType.indexOf('javascript') === -1)
        ) {
          navigator.serviceWorker.ready.then(registration => {
            registration.unregister().then(() => {
              window.location.reload();
            });
          });
        } else {
          registerValidSW(swUrl, config);
        }
      })
      .catch(() => {
        console.log(
          'No internet connection found. App is running in offline mode.'
        );
      });
  }
  export function unregister() {
    if ('serviceWorker' in navigator) {
      navigator.serviceWorker.ready
        .then(registration => {
          registration.unregister();
        })
        .catch(error => {
          console.error(error.message);
        });
    }
  }</pre>
  </details>

  ​    

## React 严格模式

> 只在开发模式下有效

```react
ReactDOM.render(
	<React.StrictMode>
		<App />
	</React.StrictMode>
	,document.getElementById('root')
)
```





# 额外工具、依赖包、配置

## 插件

- chrome插件 ： React Developer Tools

- vscode插件：ES7 React/Redux/GraphQL/React-Native snippets
  - 快捷指令
    - rcc  快速生成应用组件
    - ptypes 快速生成propTypes静态代码
  
- chrome插件：Redux DevTools

  - 下载依赖包

    ```shell
    yarn add -D redux-devtools-extension
    ```

  - 在store.js文件中引入工具函数

    ```react
    import {composeWithDevTools} from 'redux-devtools-extension'
    
    const store = createStore(
     	reducer,
        composeWithDevTools(applyMiddleware(thunk))
    )
    ```

    

  

## 依赖包

- react相关包

  ```shell
  ## 安装 react和react-dom
  npm install react react-dom
  
  ## 安装 babel配置react的包
  npm install @babel/preset-react -D
  ```

- react路由

  ```shell
  npm install react-router-dom
  ```

- react脚手架全局依赖包

  ```shell
  npm install -g create-react-app
  ```

- 下载PubSub依赖包

  ```shell
  yarn add pubsub-js
  ```

- 下载ant-design依赖包

  ```shell
  yarn add antd --save
  ```

- 下载ant-design按需引入依赖包

  ```shell
  yarn add react-app-rewired customize-cra babel-plugin-import --dev
  ```

- 下载less依赖包

  ```shell
  yarn add less less-loader --dev
  ```

- 下载redux依赖包

  ```shell
  yarn add redux
  ```

- 下载react-redux依赖包

  ```shell
  yarn add react-redux
  ```

- 下载redux异步中间件

  ```shell
  yarn add redux-thunk
  ```

- 下载配置chrome插件 Redux DevTools 依赖包

  ```shell
  yarn add -D redux-devtools-extension
  ```

- 下载装饰器依赖包

  ```shell
  yarn add @babel/plugin-proposal-decorators -D
  ```

- 下载naoid

  ```js
  yarn add naoid
  
  //const key = naoid()  // 生成一个唯一key
  ```

  

## 配置

- 解析类属性 组件类中的state

  - 在webpack.config.js中配置plugins

    ```js
    options: {
    	presets: [
    		'@babel/preset-env', // 编译js语法
    		"@babel/preset-react" // 编译jsx语法
    	],
    	plugins: [
    		"@babel/plugin-proposal-class-properties"
    	]
    }
    ```

  - 下载依赖包

    ```shell
    npm install @babel/plugin-syntax-class-properties -D
    ```

    

- <details>
      <summary>package.json</summary>
   	<pre>
  {
    "name": "vuecomponents",
    "version": "1.0.0",
    "scripts": {
      "start": "webpack",
      "dev": "webpack-dev-server"
    },
    "dependencies": {
      "@babel/core": "^7.9.6",
      "@babel/polyfill": "^7.10.1",
      "@babel/preset-env": "^7.9.6",
      "axios": "^0.19.2",
      "babel-loader": "^8.0.0-beta.0",
      "copy-webpack-plugin": "^5.1.1",
      "pubsub-js": "^1.8.0",
      "react": "^16.13.1",
      "react-dom": "^16.13.1"
    },
    "devDependencies": {
      "@babel/preset-react": "^7.10.1",
      "clean-webpack-plugin": "^3.0.0",
      "css-loader": "^3.5.3",
      "file-loader": "^6.0.0",
      "html-webpack-plugin": "^4.3.0",
      "style-loader": "^1.2.1",
      "url-loader": "^4.1.0",
      "webpack": "^4.43.0",
      "webpack-cli": "^3.3.11",
      "webpack-dev-server": "^3.11.0"
    }
  }
  </pre></details>

- <details>
  <summary>webpack.config.js</summary>
  <pre>
  const path = require('path')
  const HtmlWebpackPlugin = require('html-webpack-plugin');//打包带着html文件
  const { CleanWebpackPlugin } = require('clean-webpack-plugin');//每次打包自动清理dist
  const CopyPlugin = require('copy-webpack-plugin');
  module.exports = {
      // entry: path.resolve(__dirname,'src/index.js'),//入口
      // entry:'./src/index.js',//入口
      entry: ["@babel/polyfill", "./src/index.js"],
      output: {
          path: path.resolve(__dirname, 'dist'),//打包完成后的文件放在哪，dist文件夹会自动创建好
          filename: 'main.js',
          publicPath: '/'// 引入打包的文件时路径以/开头
      },
      //配置各种loader
      module:{
          rules:[
              //解析ES6
              {
                  test: /\.jsx?$/,
                  exclude: /(node_modules|bower_components)/,
                  use: {
                      loader: 'babel-loader',
                      //本身并不能解析es6语法，它依赖的是babel/preset-env去解析的
                      //babel/preset-env包含了一系列的ES6语法解析的插件,每个插件对应一个ES6语法
                      //babel-loader它就是依靠这些插件去解析的
                      options: {
                          presets: [
                              '@babel/preset-env', // 编译js语法
                              "@babel/preset-react" // 编译jsx语法
                          ],
                          plugins: []
                      }
                  }
              },
              //打包CSS
              {
                  test: /\.css$/,
                  use: [ 'style-loader', 'css-loader' ]//使用loader的时候有顺序，从后往前
              },
              //打包图片，内部会使用到file-loader
              {
                  test: /\.(png|jpg|gif)$/,
                  use: [
                      {
                          loader: 'url-loader',//内部会使用到file-loader
                          options: {
                              limit: 8192*100, 
                              //如果图片小于这个值，会被base64编码为一个字符串，提高效率，减少请求
                              name:'[hash:8].[ext]' //打包后的图片名字，截取哈希值的前八位就ok
                          }
                      }
                  ]
              },
              //配置loader处理字体图标
              {
                  test: /\.(eot|svg|ttf|woff|woff2)(\?\S*)?$/,
                  loader: 'file-loader'
              }
          ]
      },
      //配置插件完成其它搞不定的功能
      plugins:[
          new HtmlWebpackPlugin({
              //得去找我要配置的html文件
              template:'./public/index.html'
          }),
          new CleanWebpackPlugin(),//自动清理dist文件夹插件,
          new CopyPlugin([ //为了把public下除了index.html文件外的其余所有，给dist目录下拷贝一份
              {
                  from:path.resolve(__dirname,'public'),
                  to:path.resolve(__dirname,'dist'),
                  ignore:['index.html']
              }
          ]),
      ],
      mode:'development',//配置启动模式，开发模式还是生产模式
      devServer: {
          port:8080,//服务启动的端口
          open:true,//是否自动打开浏览器
          quiet:true,//输出少量的提示信息
          proxy: {
              '/api': {//这个/api其实是为了告诉代理，以后什么样的请求，需要给我代理转发
                  target: 'http://localhost:4000',
                  //转发的目标地址，不需要路径，因为转发的时候会把发送请求的路径默认频道目标后面
                  //我们发http://localhost:8080/api/users/info
                  //最终转发的目标会变为http://localhost:4000/api/users/info
                  pathRewrite: {'^/api' : ''},
                  //真正的目标地址应该是http://localhost:4000/users/info
                  //这一行在干的活就是把/api去掉，不就是真正的目标地址？
                  changeOrigin: true, // 支持跨域, 如果协议/主机也不相同, 必须加上
          }
      },
      historyApiFallback: true,// 任意的 404 响应都被替代为 index.html 备胎
  },
  devtool:'cheap-module-eval-source-map',//定位出错所在的原始代码行
  resolve:{
      extensions: [".js", ".jsx", ".json"],//解决导入省略后缀名称
      alias:{
          //给路径取别名,以后导入vue的时候，默认是在找'vue/dist/vue.esm.js'
          // 'vue$':'vue/dist/vue.esm.js',
          '@': path.resolve(__dirname, 'src')//取别名，让@代替根路径下的src  '/src'
      }
  }
  };
  </pre>
  </details>
  





 