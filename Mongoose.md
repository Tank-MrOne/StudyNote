# Mongoose

## 简介

> Mongoose中为我们提供了几个新的对象

- Schema(模式对象）
  - Schema对象定义约束了数据库中的文档结构
- Model
  - Model对象作为集合中的所有文档的表示，相当于MongoDB数据库中的集合collection
- Document
  - Document表示集合中的具体文档，相当于集合中的一个具体的文档



## 下载安装Mongoose

> npm安装

```node
npm i mongoose --save
```

> 项目中引入mongoose

```js
let mongoose = require("mongoose")
```

> 连接数据库

```js
//如果端口号是27017，则可以省略
mongoose.connect('mongodb://localhost/tank',(useMongoClient : true))
```

>监听MongoDB数据库的连接状态

```js
// 连接成功调用回调函数
mongoose.connection.once("open",function(){...})
// 连接中断调用回调函数
mongoose.connection.once("close",function(){...})
```

> 断开数据库连接

```js
// 一般不调用
mongoose.disconnect()
```



## 定义Schema对象

```javascript
let Schema = mongoose.Schema ;
let userSchema = new Schema({
	name : String ,
	age : Number , 
	gender : Boolean ,
	like : [
		{
			type : String
		}
	],
	date : {
		type : Date ,
		default : Date.now
	}
})

// mongoose.model("集合名称",Schema模型对象)
let User = mongoose.model("User",userSchema)
```



## Model 增删改查

> **向数据库插入文档**
>
> > Model.create( 文档/文档数组 , 回调函数（err） )

```js
User.create( {
	name : "Tank",
	age : 28,
	gender : true ,
	like : [
		{
			type : "水果"
		}
	]
} , function(err){
	if(!err){
		console.log("数据插入成功")
	}
})
```



> **查询数据库的文档**
>
> > <font color="red">查询所有符合条件的文档</font>，返回的结果是数组
> >
> > Model.find( 查询的条件 ，[投影] ，[查询选项(skip跳过,limit显示)] , 回调函数) 
>
> ><font color="red">根据id查询文档</font>，返回的结果是对象
> >
> >Model.findById( id ，[投影] ，[查询选项(skip,limit)] , 回调函数) 
>
> ><font color="red">查询符合条件的第一个文档</font>，返回的结果是对象
> >
> >Model.findOne( 查询的条件 ，[投影] ，[查询选项(skip,limit)] , 回调函数) 

```js
User.find({
	name : "Tank"
} , "name age -_id" , {
    skip : 1 , limit : 10
} ,function(err,data){
	if(!err){
		console.log("查询的结果是 ：" + data)
	}
})
```



> **修改数据库的文档**
>
> > <font color="red">修改符合条件的文档</font>
> >
> > Model.update( 查询条件 ， 修改后的文档 ， [选项] ， [回调函数])
>
> ><font color="red">修改多个文档</font>
> >
> >Model.updateMany( 查询条件 ， 修改后的文档 ， [选项] ， [回调函数])
>
> > <font color="red">修改一个文档</font>
> >
> > Model.updateOne( 查询条件 ， 修改后的文档  ， [选项] ， [回调函数])

```js
User.update({
	name : 'tank'
} , 
{
	$set : { age : 20 }
} ,
function(err){
	if(!err){
		console.log("修改成功")
	}
}
)
```



> **删除数据库文档**
>
> > <font color="red">删除符合条件的文档</font>
> >
> > Model.remove( 条件 , 回调函数 )
>
> > <font color="red">删除一个文档</font>
> >
> > Model.deleteOne( 条件 , 回调函数 )
>
> > <font color="red">删除多个文档</font>
> >
> > Model.deleteMany( 条件 , 回调函数 )

```js
User.remove({name : 'tank'} , function(err){
	if(!err){
		console.log("删除成功")
	}
})
```



> **统计文档的数量**
>
> Model.count( 条件 , 回调函数 )

```js
User.count({} , function(err,count){
	if(!err){
		console.log("一共查询到文档有"+count+"条数据")
	}
})
```



## Document对象

> **创建一个document对象**

```js
let user = new User({
	name : "MrOne",
	age : 28,
	gender : true ,
	like : [
		{ type : '运动' }
	]
})
```



> **document.save将对象插入数据库**

```js
user.save(fuction(err){
	if(!err){
		console.log("保存成功")
	}
})
```



>**document.update修改对象**

```js
user.update({$set : {age : 18}},function(err){
	if(err){
		console.log("修改成功")
	}
})

//或者
user.age = 18 
user.save()
```



> **document.remove删除对象**

```js
user.remove(function(err){
	if(err){
		console.log("删除成功")
	}
})
```



> **document.get获取文档中的指定属性值**

```js
user.get("name")
// 或者
user.name
```



>**document.set设置文档中的指定属性值**

```js
user.set("name","Wan")
```



## 模块化

```js
// 向外暴露
let UserModel = mongoose.model('User',UserSchema)
module.exports = UserModel

// 外部接收
let User = require('./mongodb/User.js')
```

