# ReactNative



## 简介

> react ,专门用于构建页面的js库，只关注界面（View）
>
> native，原生

react-native ： **开发手机应用**，简称（RN）

```gfm
如何开发一款手机应用？
	1、确定开发什么平台 ？ （IOS Android）
	2、招募IOS程序员、Android程序员、招募前端人员写网页
	
IOS程序员：
	1、语言：swift、OC
	2、环境：macOS + xcode ...

Android程序员：
	1、语言：java
	2、环境：windows / macOS + eclipse / AndroidStudio ...
	
【react-native可以将编写的js代码翻译成OC或java语言】
```

```gfm
主流的跨平台开发方案：
	1、ReactNative （FaceBook研发）
	2、Vue + Weex (Weex 阿里研发)
	3、AppCan
	4、Flutter（谷歌研发，dart语言）
```



## 原生App 与 WebApp

- 原生App ：通过原生 java 或 swiff 、OC 编写的 app
- WebApp : 
  1. 一个针对移动端设计的网页，称之为一个简单的webApp
  2. 加壳：将编写的网页通过IOS或Android进行包装，通过安装打开的依然是网页，（弊端：当手机断网的情况下导致白屏）

> RN 产生的不是网页应用，不是HTML5应用，也不是混合应用，是一个真正的原生移动应用



**功能**：

- 原生APP可以调用手机终端的硬件设备（语音、摄像头、短信、GPS、蓝牙、重力感应等）
- Web APP则可能会受到一些限制

**加载速度**

- 原生APP所有的UI元素、数据内容、逻辑框架均安装在手机终端上，访问的时候，不需要重新下载加载
- Web APP每打开一个页面，都需重新加载，访问速度受手机终端上网的限制，每次使用均会消耗一定的手机上网流量

**稳定性**

- APP技术更加成熟，有专业的开发环境、专门的开发语言、专门的编译环境
- WebbApp多为套用加壳模板，随着市场上浏览器、技术的进步，会逐步出现各种瓶颈问题，功能拓展性相对较差



## 优势

1. 跨平台开发
2. 实现热更新和热部署
3. 一次学习，随处编写

注意：尽量少调用有兼容性的API





# ReactNative环境搭建

## 配置java环境

1. 安装jdk1.8版本

2. 配置jdk环境变量

   - 环境变量path中追加jdk的bin目录

   ```apl
   C:\Program Files\Java\jdk1.8.0_181\bin
   ```

   - 新建环境变量JAVA_HOME，值为java目录下的jdk目录

   ```apl
   变量名：JAVA_HOME
   变量值：C:\Program Files\Java\jdk-18.0.2
   ```

   - 新建环境变量CLASSPATH，值为jdk的安装路

   ```apl
   变量名：CLASSPATH
   变量值：.;%JAVA_HOME%\lib;%JAVA_HOME%\lib\tools.jar
   ```

3. 检查环境变量是否安装完成

   ```apl
   打开cmd
   输入javac -version
   
   -- 出现版本号则表示成功
   ```

   

## 配置Python环境

1. 找到python安装包 python-2.7.17.msi
2. 步骤都是下一步

注意：

- 在Customize Python 2.7.17界面中，找到最后一个选项，选择Add Python.exe to Path ,将python配置到系统环境变量当中
- 尽量选择在默认系统盘安装



3. 检查Python是否配置成功

   ```apl
   打开cmd
   输入python
   
   -- 出现版本号则表示成功
   ```

   

## 配置 Android环境

1. 安装Android安装包，以管理员身份运行android-studio-windows.exe
2. 尽量安装到默认盘
3. 完成后注意事项
   - Import Android Studio Settings From... 弹窗，选择 Do not import settings（不用导入配置）
   - Data Sharing 弹窗 ，选择 Don't Send（不发生任何数据）
   - Android Studio First Run 弹窗 ， 选择 Cancel ( 不需要配置代理 )
4. 下载组件并解压，执行Finish即可完成



## 配置Android SDK

> 安卓集成开发包工具

1. 在右下角，Configure 选项，选择 SDK Manager
2. 勾选左侧导航栏的Android SDK选项，勾选右下角的Hide Obsolete 和 Show Package Details选项
3. 在主栏上选择SDK Platforms选项卡，勾选Android 10.0中的
   - Android SDK Platform 29
   - Sources for Android 29
   - Google APls Intel x86 Atom System Image
   - 注意保存Android SDK Location 的路径
4. 同意协议后下载
5. 在主栏上选择SDK Tools选项卡，勾选Android SDK Build-Tools中的
   - 30.0.1
   - 29.0.3
   - 28.0.3
6. 安装完成即可



## 配置环境变量

- 新建一个用户变量，取名：**ANDROID_HOME**
- 值为之前保存的Android SDK Location 的路径



## 配置Android AVD 

> 安卓模拟器

1. 打开Android Studio ,选择右下角Configure 选项，选择 AVD Manager
2. 创建一个模拟器，推荐选择如下
   - Pixel 3a XL 型号
   - Q 系统
   - 建议选择竖屏
3. 点击action选项中的运行按钮即可打开模拟器
4. 友好界面可在模拟器中的设置里将语言换成”简体中文“



##  创建RN项目

1. 全局下载react-native
2. 使用react-native init 项目名 来创建项目
3. 运行项目
   - 开启模拟手机
   - 项目目录下 ， 运行npm run android





# ReactNative规则

1. RN中依然使用jsx语法，但不允许使用HTML标签，必须使用Component组件
2. RN中所有的数值不允许有单位
3. RN中根标签有且只有一个
4. RN默认是flex纵向布局



# ReactNative 快捷

 ctrl + m  + Reload 重新加载 ( 或者在“桥” 窗口按 “r” )

 ctrl + m + Debug  调式器（在浏览器控制台上显示，不需要的时候记得关闭debug）



# ReactNative内置组件

## Text 文本组件

```react
import { Text } from 'react-native'

class MyStatusBar extends Component {
    render(){
        return (
            {/* （RN中文本必须写在Text组件中） */}
            <Text> hello , I am Text </Text>
        )
    }
}
```

常用属性：

- numberOfLines   限制内容的行数  

- ellipsizeMode   省略号位置（numberOfLines   必须值为1）

  - head  最前
  - middle  中间
  - tail   最后

- onPress    手指按压事件

  ```react
  <Text onPress={()=>{alert('hello')}}> hello , I am Text </Text>
  ```

- slelectable   手指长按选择（默认是true）

- onLongPress   手指长按事件

  ```react
  <Text onLongPress={()=>{alert('hello')}}> hello , I am Text </Text>
  ```

- disable   让文字失去所有事件监听





## StatusBar 状态栏

```react
import { StatusBar } from 'react-native'

class MyStatusBar extends Component {
    render(){
        return (
            {/* （自结束标签） */}
             <StatusBar />
        )
    }
}
```

常用属性

- backgroundColor 	背景颜色
- barStyle    状态栏文字的颜色（颜色非黑即白）
- hidden    状态栏隐藏
- animated     状态栏的效果切换动画效果（值为true  / false）
- translucent    沉浸式状态栏（让内容区扩展到状态栏下）



## StyleSheet.create 创建样式

```react
import { Text,StyleSheet } from 'react-native'

class MyStatusBar extends Component {
    render(){
        return (
            <>
             	<Text style={[ styles.aaa , style.bbb ]}> 123 </Text>
             	<Text style={ style.bbb }> 123 </Text>
            </>
        )
    }
}

// 一般写在类外面
const styles = StyleSheet.create({
  aaa: {
    flex: 1,  // 最外层容器设置则铺满全屏
    justifyContent: 'center',
    alignItems: 'center',
    backgroundColor: '#F5FCFF',
  }，
  bbb: {
    color: '#000',
  }
})
```



## SafeAreaView 安全视图组件

`SafeAreaView`会自动根据系统的各种导航栏、工具栏等预留出空间来渲染内部内容。更重要的是，它还会考虑到设备屏幕的局限

```react
import { Text,SafeAreaView,StatusBar } from 'react-native'

class MyStatusBar extends Component {
    render(){
        return (
            <>
            	{/* 状态栏一般不建议放在SafeAreaView组件中*/}
            	<StatusBar/>
           		<SafeAreaView>
             		<Text> 123 </Text>
            	</SafeAreaView>
            </>
        )
    }
}
```

## View组件

是一个支持Flexbox布局的组件，并已经开启了flex布局

```react
import { Text,View } from 'react-native'

class MyStatusBar extends Component {
    render(){
        return (
            <>
            	<StatusBar/>
           		<View>
             		<Text> 123 </Text>
            	</View>
            </>
        )
    }
}
```



## Dimensions和Platform获取设备信息

- Dimensions.get('window').width  获取设备的宽度
- Dimensions.get('window').height  获取设备的高度
- Platform.OS  获取当前设备的平台
- Platform.Version  获取当前设备的平台的版本
- Platform.isPad  获取当前设备的是否是Pad，返回布尔值，如果

```react
import { Text,Dimensions, Platform } from 'react-native'

<Text>当前设备宽是：{Dimensions.get('window').width} px</Text>
<Text>当前设备宽是：{Dimensions.get('window').height} px</Text>
<Text>当前app运行平台是：{Platform.OS}</Text>
<Text>当前app运行平台版本是：{Platform.Version}</Text>
<Text>当前app是：{Platform.isPad}</Text>
```



## Image 和 ImageBackground

> 引入本地图片 Image  (自结束标签)

属性：

- source	引入图片

  ```react
  import { Text,Image } from 'react-native'
  
  // 引入本地图片
  <Image source={require('../img.jpg')} />
                 
  // 引入网络图片(必须给宽高)
  <Image source={{uri:"https://wwww......jpg"}} style={{width:100,height:100}}/>
      
  // 引入base64图片
  <Image source={{uri:"data:.....jpg"}} style={{width:100,height:100}}/>
  ```

- resizeMode    设置图片的尺寸
  - cover    图片不留空白，铺满容器，图片可能显示不完整
  - contain    图片完整显示，但可能容器不能铺满
  - stretch    图片拉伸，图片完整铺满整个容器
  - repeat    图片平铺，图片铺满容器，但会重复
  - center    图片居中显示，等比例缩放

> 图片背景标签

```react
import { Text,ImageBackground } from 'react-native'

class MyStatusBar extends Component {
    render(){
        return (
            <>
            	<StatusBar/>
           		<ImageBackground source={require('../img.jpg')}>
             		<Text> 123 </Text>
            	</ImageBackground>
            </>
        )
    }
}
```

- 将图片作为Android静态资源

1. 在app/src/main/res/drawable文件夹中存放静态资源

2. 重启项目

3. 引入静态资源图片

```react
<Image source={{uri:'静态资源名'}}>
```









## TextInput 输入框

```react
import { TextInput,View } from 'react-native'

class MyStatusBar extends Component {
    render(){
        return (
            <>
            	<StatusBar/>
           		<View>
             		<TextInput />
            	</View>
            </>
        )
    }
}
```

常用属性：

- placeholder   提示文字

- placeholderTextColor  提示文字颜色

- keyboardType   键盘类型

  - phone-pad  仅电话键盘
  - email-address   仅邮箱键盘

- multiline  是否可换行（布尔值）

- autoCapitalize  自动大写

  - words  首字母自动大写
  - characters  所有的字符大写
  - sentences 每句话的第一个字符大写

- onChangeText  表单内容发送变化事件( 推荐将值写入state状态 )

  ```react
  <TextInput onChangeText={(data)=>{ console.log(data) }}/>
  ```
  
  

## Button

```react
import { Button,View } from 'react-native'

class MyStatusBar extends Component {
    render(){
        return (
            <>
           		<View>
             		<Button title="click" />
            	</View>
            </>
        )
    }
}
```

常用属性：

- title    按钮的内容
- color    按钮的颜色
- onPress    点击事件
- touchSoundDisabled   取消声音（false）

注意：

- 按钮无法修改宽度高度，只有修改按钮外层容器即可

- 按钮无法修改字体大小

  

## TouchableOpacity自定义按钮

> 自定义按钮，不是自结束标签

```react
import { TouchableOpacity,View,Text } from 'react-native'

class MyStatusBar extends Component {
    render(){
        return (
            <>
           		<View>
                	<TouchableOpacity style={{...}}>
                		<Text>点我</Text>
                	</TouchableOpacity>
            	</View>
            </>
        )
    }
}
```

常用属性：

- activeOpactiy    点击后的背景透明效果，（0 - 1 ），默认0.2



## ScrollView 滚动视图

> ScrollView必须要有一个高度才能正常使用

```react
import { ScrollView ,View,Text } from 'react-native'

class MyStatusBar extends Component {
    render(){
        return (
            <>
           		<ScrollView >
                	<View style={{...}}>
                		<Text>点我</Text>
                	</View>
            	</ScrollView >
            </>
        )
    }
}
```

常用属性

- horizontal   水平滚动

- paginEnabled    整屏滚动

- showHorizontalScrollIndicator   是否显示滚动条

- scrollEnabled    是否可以滚动

- onScroll   滚动时的回调

- refreshControl   滚动刷新

  ```react
  import { ScrollView ,RefreshControl } from 'react-native'
  <ScrollView 
     refreshControl={ // 下拉刷新控制器
          <RefreshControl  // 刷新组件
              resfreshing={true}   //用于指定列表是否处于加载中(布尔值)
              onRefresh={this.onRefresh}>  // 当用户在垂直滚动视图中最顶端位置，依然下拉时触发
              colors={['yellow','blue']}  // 加载圆圈的颜色
          </RefreshControl>
      } 
  ></ScrollView>
         
  onRefresh = () => {...}
  ```

  

## FlatList分组滚动视图

> 当数据过多时，ScrollView可能会产生卡顿，而FlatList不光可以解决这个问题，还可以实现多列下拉滚动

```react
import { FlatList ,View,Text } from 'react-native'

class MyStatusBar extends Component {
    state={
        arr : [  ...数据 ]
    }

	renderItem = ({item,index}) => {
        return <Text> hello </Text>
    }

    render(){
        return (
            <>
           		<FlatList 
                    data={this.state.arr}
                    renderItem={this.renderItem} 
                    keyExtractor={(item,index)=>{
                    	return index.toString()
                	}}
                    refreshing={true}  
                    onRefresh={()=>{}}
                />
            </>
        )
    }
}
```

常用属性

- data    数据源，data属性目前只支持普通数组（必填项）
- renderItem    用于渲染每一项，（必填项）
- keyExtractor   生成唯一的key（必须是字符串）
- onRefresh    下拉刷新
- refreshing    刷新小圆圈，必须配合onRefresh一同使用
- ListEmptyComponent  列表没有数据显示组件（值为组件）
- numColumns={1}   列数
- initialNumToRender={10}   初始化显示多少条数据
- onEndReachedThreshold={0.2}  决定当距离底部还有多少条数据时触发onEnReached回调
- onEnReached={()=>{...}}    请求下一批回调
- horizontal    横向滚动



## SectionList高性能分组列表组件

> 高性能的分组列表组件

```react
import { FlatList ,View,Text } from 'react-native'

class MyStatusBar extends Component {
    state={
        arr : [ 
        	{ ...obj},{ ...obj},{ ...obj},
        ]
    }

	renderItem = ({item,index}) => {
        return <Text> s所有项 </Text>
    }

    renderHeader = ({item,index}) => {
        return <Text> 标题 </Text>
    }
    
    render(){
        return (
            <>
           		<SectionList 
                    sections={this.state.arr}
                    renderItem={this.renderItem}
                    renderSectionHeader={this.renderHeader}
                />
            </>
        )
    }
}
```

常用数据

- sections    数据源属性
- renderItem    渲染项
- renderSectionHeader    渲染标题





# ReactNative路由导航

> 安装react-navigation 及周边库

```vb
yarn add react-navigation      核心库
yarn add react-native-reanimated     用于支持动画
yarn add react-native-gesture-handler   用于支持手势相关
yarn add react-native-screens    用于支持多屏切换的库
yarn add react-navigation-stack    用于创建栈式导航
yarn add react-native-safe-area-context    安全上下文
yarn add @react-native-community/masked-view     以上个别的库依赖这个库
```

### 顶部栈式导航

1. 将需要切换的内容，拆分成多个组件 并引入

   ```js
   import Home from './Home'
   import Hot from './Hot'
   ```

2. 创建导航器( 顶部栈式导航 ，底部tab导航)

   ```js
   // 引入createStackNavigator用于创建导航器，承载Home和Hot两自定义组件
   import {createStackNavigator } from 'react-navitation-stack'
   
   // 引入createAppContainer，用于创建一个App容器
   import {createAppContainer} from 'react-navigation'
   ```

3. 创建一个承接导航器的容器（navigator => AppContainer）

   ```js
   // 创建顶部栈式导航
   const topStackNavigator = creactStackNavigator({
   	"Home" : { 
           screen : Home ,
           navigationOptions:{
               title : '主页'  // 设置导航标题名称
           }
       },
   	"Hot" : { screen : Hot }
   },{
      initiaRouteName:'Home'  // 初始展示页面是Home
   })
   
   /* 简写
   const topStackNavigator = creactStackNavigator({
   	Home , Hot
   }) */
   
   // 创建App容器，并暴露
   export default creactAppContainer(topStackNavigator)
   ```

5. 路由组件中通过props进行跳转

   ```react
   import { View,Text } from 'react-native'
   
   class Home extends Component {
       static navigarionOptions={
           title : '导航栏标题名称',
           headerStyle : { ...头部样式 },
           headerTitleStyle :{ //头部标题样式
               fontWeight : 'bold' 
           }
       }
       
   	goHot = () => {
   		this.props.navigation.navigate('Hot')
   	}
       
       render(){
           return (
               <>
                   <View>
                   	<Text onPress={this.goHot}>点我</Text>
                   </View>
               </>
           )
       }
   }
   ```

6. 路由传参

   ```js
   // 传递参数
   this.props.navigation.push('组件名'，{ 参数... })
   // 接收参数
   this.props.navigation.getParam('参数名')
   
   // 接收所有参数
   JSON.stringify(this.props.navigation.state.params)
   ```

   

### 底部Tab导航

1. 引入路由组件

2. 下载安装react-navigation-tabs

   ```shell
   yarn add react-navigation-tabs
   ```

3. 引入react-navigation-tabs，创建导航器，承装路由组件

   ```js
   import {createBottomTabNavigator } from "react-navigation-tabs"
   ```

4. 引入 ，用于创建一个App容器

   ```js
   import {createAppContainer} from "react-navigation"
   ```

5. 创建一个底部tab导航

   ```js
   const bottomTabNavigator = createBottomTabNavigator({
   	"Home" : {screen : Home},
   	"Hot" : {screen : Hot}
   })
   ```

   

- 查看react-navigation的版本

  ```shell
  npm view react-navigation version
  ```

  



# 生成apk安装包

1. 将java/jdk/bin/keytool.exe添加到环境变量中

2. 在项目根目录下，通过终端生成密钥（10000天内有效）

   ```shell
   keytool -genkeypair -v -keystore my-release-key.keystore -alias my-key-alias -keyalg RSA -keysize 2048 -validity 10000
   ```

3. 初次设置口令与信息后，会生成一个my-release-key.keystore文件

4. 在当前项目的android/gradle/gradle.properties文件中添加以下内容

   ```sh
   MYAPP_RELEASE_STORE_FILE=my-release-key.keystore
   MYAPP_RELEASE_KEY_ALIAS=my-key-alias
   MYAPP_RELEASE_STORE_PASSWORD=密码
   MYAPP_RELEASE_KEY_PASSWORD=密码
   ```

5. 在项目中androil/app/build.gradle中添加如下配置

   ```apl
   ...
   android {
       ...
       defaultConfig { ... }
       signingConfigs {
           release {
               if (project.hasProperty('MYAPP_RELEASE_STORE_FILE')) {
                   storeFile file(MYAPP_RELEASE_STORE_FILE)
                   storePassword MYAPP_RELEASE_STORE_PASSWORD
                   keyAlias MYAPP_RELEASE_KEY_ALIAS
                   keyPassword MYAPP_RELEASE_KEY_PASSWORD
               }
           }
       }
       buildTypes {
           release {
               ...
               signingConfig signingConfigs.release
           }
       }
   }
   ...
   ```

6. 在终端输入命令生成安装包

   ```shell
   gradlew assembleRelease
   ```

7. apk文件在android/app/outputs/release文件夹中








# 弹性盒解决最后一行对齐

```react
import React, { Component } from 'react'
import { Text, View,Image,StyleSheet,Dimensions } from 'react-native'
// 导入图片的json文件
import frontEndArr from '../../assets/json/front_end'

export default class PictureWall extends Component {

	state = {
		imgWidth:120,
		imgHeight:100
	}

	styles = StyleSheet.create({
		wraper:{
			marginTop:10,
			flexDirection:'row',
			flexWrap:'wrap',
			justifyContent:'space-around'
		},
		text:{
			textAlign:'center'
		},
		img:{
			width:this.state.imgWidth,
			height:this.state.imgHeight,
		}
	})

	renderItem = ()=>{
		//屏幕的宽度
		const screenWidth = Dimensions.get('window').width
		//每一张图片的宽高
		const {imgWidth} = this.state
		//图片总数
		const total = frontEndArr.length
		//计算每一行能展示几个图片
		const oneRowCount = Math.floor(screenWidth / imgWidth)
		//计算要用几行去展示
		const rows = Math.ceil(total / oneRowCount)
		//计算出要补充的数量
		const addNumber = oneRowCount * rows - total
		for (let i = 0; i <addNumber ; i++) {
			frontEndArr.push({name:'',uri:''})
		}
		const {text,img} = this.styles
		return frontEndArr.map((picObj,index)=>{
			return (
				<View key={index}>
					<Image resizeMode="contain" style={img} source={{uri:picObj.url}}/>
					<Text style={text}>{picObj.name}</Text>
				</View>
			)
		})
	}

	render() {
		const {wraper} = this.styles
		return (
			<View style={wraper}>
				{this.renderItem()}
			</View>
		)
	}
}
```





