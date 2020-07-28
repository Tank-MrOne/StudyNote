## webpack
### 1、webpack 介绍
* 什么是webpack<https://www.webpackjs.com/>
  * Webpack是一个模块打包器(bundler)。（构建工具、项目构建）
    * 其他的构建工具，比如：grunt、gulp
  * 在Webpack看来, 前端的所有资源文件(js/json/css/img/less/...)都会作为模块处理
  * 它将根据模块的依赖关系进行静态分析，生成对应的静态资源
  * 作用：
    * 
* 五个核心概念
  * Entry：入口起点(entry point)指示 webpack 应该使用哪个模块，来作为构建其内部依赖图的开始。
  * Output：output 属性告诉 webpack 在哪里输出它所创建的 bundles，以及如何命名这些文件
  * Loader：loader 让 webpack 能够去处理那些非 JavaScript 文件
  * Plugins：插件则可以用于执行范围更广的任务。例如：打包优化、压缩，
  * Mode：模式，有生产模式 production 和开发模式 development 
* 理解 Loader
  * Webpack 本身只能加载 JS/JSON 模块，如果要加载其他类型的文件(模块)，就需要使用对应的loader 进行转换/加载
  * Loader 本身也是运行在 node.js 环境中的 JavaScript 模块
  * 本身是一个函数，接受源文件作为参数，返回转换的结果
  * loader 一般以 xxx-loader 的方式命名，xxx 代表了这个 loader 要做的转换功能，比如 less-loader。
* 理解Plugins
  * 插件可以完成一些loader不能完成的功能。
  * 插件的使用一般是在 webpack 的配置信息 plugins 选项中指定。
* 配置文件(默认)
  * webpack.config.js : 是一个node模块，返回一个 json 格式的配置信息对象
	
### 2、开启项目
* 初始化项目：
  * 生成package.json文件
      ```   json
      {
        "name": "webpack-learn",
        "version": "1.0.0"
      }
      ```
* 安装webpack
  * npm install webpack webpack-cli -g  //全局安装,作为指令使用
  * npm install webpack webpack-cli -D //本地安装,作为本地依赖使用
    
### 3、编译打包应用
* 创建js文件
  * src/js/app.js
  * src/js/module1.js
  * src/js/module2.js
  * src/js/module3.js
* 创建json文件
  * src/json/data.json  
* 创建主页面: 
  * src/index.html
* 运行指令
  * 开发配置指令：webpack src/js/app.js -o build/js/app.js --mode=development
    * 功能: webpack能够编译打包js和json文件，并且能将es6的模块化语法转换成浏览器能识别的语法
  * 生产配置指令：webpack src/js/app.js -o build/js/app.js --mode=production
    * 功能: 在开发配置功能上加上一个压缩代码
* 结论：
  * webpack能够编译打包 js 和 json 文件
  * 能将 es6 的模块化语法转换成浏览器能识别的语法
  * 能压缩代码
* 缺点：
  * 不能编译打包 css、img 等文件
  * 不能将 js 的 es6 基本语法转化为 es5 以下语法
* 改善：使用 webpack 配置文件解决，自定义功能

### 4、使用 webpack 配置文件
* 目的：在项目根目录定义配置文件，通过自定义配置文件，还原以上功能
* 文件名称：webpack.config.js
* 文件内容：
    ```js
    //node内置核心模块，用来设置路径。
    const { resolve } = require('path');
    //只能使用 CommonJS 规范暴露
	module.exports = {
	  // 入口文件配置
	  entry: './src/js/app.js',   			
	  // 输出配置
	  output: {         
        // 输出文件名
        filename: './js/built.js',    
        //输出文件路径配置
        path: resolve(__dirname, 'build')   
      },
      // development 与 production 开发环境(二选一)
      mode: 'development'   				
    };
    ```
* 运行指令： webpack

### 5、打包 less 资源
less 文件 webpack 不能解析，需要借助 loader 编译解析，使用步骤如下：

1. 创建less文件
  * src/less/test1.less
  * src/less/test2.less

2. 入口app.js文件

  ```js
  //引入两个 less 文件
  import '../css/test1.less';
  import '../css/test2.less';
  ```

3. 安装 loader

  ```shell
  npm install css-loader style-loader less-loader less --save-dev 
  ```

4. 配置 loader
    ```js
	module.exports = {
	    .
	    .
	    .
	    module:{
	        rules:[
	            {
	                test:/\.less$/,  		// 检查文件是否以.less结尾（检查是否是less文件）
                    use:[					// 数组中loader执行是从下到上，从右到左顺序执行
                        'style-loader', 	// 创建style标签，添加上js中的css代码
                        'css-loader', 		// 将css以commonjs方式整合到js文件中
                        'less-loader' 		// 将less文件解析成css文件
                    ]
                }
            ]
        },
    }
    ```
    
5. 运行指令

    ```shell
    > webpack
    ```

### 6、JS 语法检查
webpack 使用 ESLint（<https://eslint.bootcss.com/>） 能对 JS 基本语法错误/隐患进行提前检查，==但是不能检测运行时错误==，使用步骤

1. 安装loader

   ```shell
   npm install eslint-loader eslint --save-dev
   ```

   > eslint 是语法检查的包
   >
   > eslint-loader 是 eslint 在 webpack 中的 loader 包

2. webpack.config.js 配置 loader

   ```js
   module.exports = {
       .
       .
       .
       module: {
           rules: [
       		.
       		.
       		.
               {
                   test: /\.js$/,                  //只检测js文件
                   exclude: /node_modules/,        //排除node_modules文件夹
                   enforce: "pre",                 //提前加载使用
                   use: {                          
                       loader: "eslint-loader"		//使用eslint-loader解析
                   }
               }
           ]
       }
   }
   ```

   

3. 创建 `.eslintrc.js` 文件

    ```js
    module.exports = {
        "parserOptions": {
            "ecmaVersion": 6, 				// 支持es6
            "sourceType": "module"			// 使用es6模块化
        },
        "env": { 							// 设置环境
            "browser": true,   				// 支持浏览器环境： 能够使用window上的全局变量
            "node": true       				// 支持服务器环境:  能够使用node上global的全局变量
        },
        "globals": {						// 声明使用的全局变量, 这样即使没有定义也不会报错了
            "$": "readonly"					// $ 不允许重写变量
        },
        "rules": {  						// eslint检查的规则  0 忽略 1 警告 2 错误
            "no-console": 0, 				// 不检查console
            "eqeqeq": 0,					// 用 == 而不用 === 就报错
            "no-alert": 0 					// 不能使用alert
        },
        "extends": "eslint:recommended" 	// 使用eslint推荐的默认规则
    }
    ```

4. 运行指令

   ```shell
   > webpack
   ```


### 7、JS 语法转换
借助 Babel 可以将浏览器不能识别的新语法（ES6, ES7）转换成原来识别的旧语法（ES5），浏览器兼容性处理

1. 安装loader

   ```shell
   > npm install babel-loader @babel/core @babel/preset-env --save-dev
   ```

   > @babel/core  是 babel 的核心库
   >
   > @babel/preset-env  是 babel 的预设的工具包，默认可以将所有最新的语法转为为 ES5
   >
   > babel-loader   是 babel 在 webpack 中的 loader 包

2. 配置loader

    ```js
    module: {
      rules: [
        .
        .
        .
        {
          test: /\.js$/,
          exclude: /node_modules/,
          use: {
            loader: "babel-loader",
            options: {
              presets: ['@babel/preset-env']
            }
          }
        }
    ]
    }
    ```

3. 运行指令
  ```
  > webpack	
  ```

  

### 8、JS 兼容性处理

Polyfill 是一块代码（通常是 Web 上的 JavaScript），用来为旧浏览器提供它没有原生支持的较新的功能

1. 安装 polyfill

   ```shell
   > npm install @babel/polyfill
   ```

2. app.js（入口文件）引入

	```js
	import '@babel/polyfill';
	```

> 解决 babel 只能转换语法的问题(如：let/const/解构赋值...)，引入polyfill可以转换高级语法(如:Promise...)



### 9、打包样式文件中的图片资源

图片文件 webpack 不能解析，需要借助 url-loader编译解析

1.  两张资源图片:
   * 小图, 小于8kb: src/images/vue.png
   * 大图, 大于8kb: src/images/react.jpg
   
2. 在 less 文件中通过背景图的方式引入图片

    ```css
    .react {
      width: 200px;
      height: 200px;
      background: url('../images/react.png') no-repeat;
      background-size: cover;
    }
    
    .vue {
      width: 200px;
      height: 200px;
      background: url('../images/vue.png') no-repeat;
      background-size: cover;
    }
    ```

3. 安装 loader
  ```shell
  > npm install file-loader url-loader --save-dev 
  ```

  > 补充：url-loader是对象file-loader的上层封装，使用时需配合file-loader使用。

4. webpack.config.js 配置 loader
    ```js
    module.exports = {
        .
        .
        .
        module: {
            rules: [
                .
                .
        		.
                {
                    test: /\.(png|jpg|gif)$/,
                    use: {
                        loader: 'url-loader',
                        options: {
                            limit: 8192,               		// 8kb以下的图片会base64处理
                            outputPath: 'images',           // 文件本地输出路径
                            publicPath: '../build/images',   // 图片的url路径
                            name: '[hash:8].[ext]',         // 修改文件名称和后缀 
                        }
                    }
                },
            ]
        }
    
    }
    ```
    
5. 运行指令

    ```shell
    > webpack
    ```

    

### 10、打包 HTML 文件
 HTML 文件不能直接被 webpack 解析，需要借助 `HtmlWebpackPlugin` 插件编译解析

1. 在 src 目录下创建 index.html 文件，==注意不要在 HTML 中引入任何 CSS 和  JS  文件==

2. 安装插件 

   ```shell
   > npm install html-webpack-plugin --save-dev 
   ```

3. webpack.config.js 修改配置

   ```js
   // 插件都需要手动引入
   const HtmlWebpackPlugin = require('html-webpack-plugin');
   .
   .
   module.exports = {
       .
       .
       plugins: [
           new HtmlWebpackPlugin({
               template: './src/index.html', // 设置要编译的 HTML 源文件路径
           })
       ]
   }
   ```

4. 运行指令

    ```
    > webpack
    ```

> src 目录就是源文件目录，所有的代码和资源都保存在该目录，index.html 也是如此

### 11、打包 HTML 中图片资源
url-loader 只能处理 JS 和 CSS 中引入的图片，无法处理 HTML 中的 img 图片，需要 html-loader 处理。

1. src/index.html 添加 img 标签

  ```html
  <img src="./images/sun.jpg" alt="">
  ```

2. 安装loader
	
	```shell
	> npm install html-loader --save-dev 
	```
	
3. 配置loader
    ```js
    module.exports = {
        .
        .
        .
        module: {
            rules: [
                .
                .
                .
                {
                    test: /\.(html)$/,
                    use: {
                        loader: 'html-loader'
                    }
                }
            ]
        }
    }
    ```
    
4. 运行指令
	```
	> webpack
	```

### 12、打包字体资源
字体文件需要借助 file-loader 编译解析，以 iconfont 为例，下载一个项目

1. 将字体文件保存在 `src/fonts` 目录下
  * src/fonts/iconfont.eot
  * src/fonts/iconfont.svg
  * src/fonts/iconfont.ttf
  * src/fonts/iconfont.woff
  * src/fonts/iconfont.woff2

2. 创建 src/css/iconfont.less 并将 iconfont 的 css 样式粘到 less 文件中，并修改字体路径
    ```css
    @font-face {
      font-family: 'iconfont';
      src: url('../fonts/iconfont.eot');
      src: url('../fonts/iconfont.eot?#iefix') format('embedded-opentype'),
          url('../fonts/iconfont.woff2') format('woff2'),
          url('../fonts/iconfont.woff') format('woff'),
          url('../fonts/iconfont.ttf') format('truetype'),
          url('../fonts/iconfont.svg#iconfont') format('svg');
    }
    
    .iconfont {
      font-family: "iconfont" !important;
      font-size: 16px;
      font-style: normal;
      -webkit-font-smoothing: antialiased;
      -moz-osx-font-smoothing: grayscale;
    }
    ```
    
3. 修改 `src/index.html`

    ```html
    <span class="iconfont">&#xe8ab;</span>
    ```

4. 配置 loader
    ```js
	module.exports = {
	    .
	    .
	    .
	    module: {
	        rules: [
	            .
	            .
                .
                {
                    test: /\.(eot|svg|woff|woff2|ttf|mp3|mp4|avi)$/,  // 处理字体文件
                    loader: 'file-loader',
                    options: {
                      outputPath: 'fonts',
                      name: '[hash:8].[ext]'
                    }
                }
            ]
        }
    }
    ```
    
5. 运行指令

    ```shell
    > webpack
    ```

    


### 13、自动编译打包运行

之前的操作，每次修改代码都需要重新执行 webpack 命令，可以使用 webpack-dev-server 自动打包运行

1. 安装 loader
	
	```shell
	> npm install webpack-dev-server --save-dev
	```
	
2. 详细配置见官网 <https://www.webpackjs.com/configuration/dev-server/>

3. 修改 webpack.config.js
    ```js
    .
	.
	.
	module.exports = {
        .
        output: {
            path: resolve(__dirname, 'build'),
            filename: 'js/app.js',
            //1. 添加 devServer 服务后需要调整输出的路径
            publicPath: '/'
        },
        module: {
            rules: [
                .
                .
                .
                {
                    test: /\.(png|jpg|gif)$/,
                    use: {
                        loader: 'url-loader',
                        options: {
                            limit: 8192,               		
                            outputPath: 'images',           
                            name: '[hash:8].[ext]',       
                			//2. 删除 publicPath 配置
                        }
                    }
                },
                
    
            ]
        },
        .
        .
        //3. 增加 devServer 配置
        devServer: {
            open: true, 	// 自动打开浏览器
            compress: true, // 启动gzip压缩
            port: 3000, 	// 端口号
        },
        mode: 'development'
    }
    ```
4. 现在就可以启动服务
	```shell
	> npx webpack-dev-server
	```
  ```

5. 配置 package.json 中 scripts 指令。增加 server 配置
  
   ```json
   {
      .
      .
      .
      "scripts": {
           "server": "webpack-dev-server" 
       },
      .
      .
      .
    }
    
  ```

6. 运行指令

   ```shell
   > npm run server 
   ```

   


### 14、热模替换功能
模块热替换 (HMR - Hot Module Replacement) 功能会在应用程序运行过程中替换、添加或删除模块，而无需重新加载整个页面，详细配置地址（<https://www.webpackjs.com/guides/hot-module-replacement/>）

修改 webpack.config.js 的 devServer 配置
```js
.
.
.
module.exports = {
    //index.html 不能自动刷新的解决方法
    //新增一个入口，解决开启热模块替换后首页无法刷新的问题
    entry: {
        main:['./src/js/app.js','./src/index.html']
    },
    .
    .
    .
    devServer: {
        open: true, 	
        compress: true, 
        port: 3000, 	
        hot: true		// 开启热模块替换功能
    },
    mode: 'development'
}
```



### 15、devtool

devtool 是 webpack 中的一个配置， 可以将压缩/编译文件中的代码映射回源文件中的原始位置，便于调试代码

详细配置官网地址 <https://www.webpackjs.com/configuration/devtool/>

配置 webpack.config.js 

```js
.
.
.
module.exports = {
    .
    .
    .
    devtool:  'cheap-module-eval-source-map', //设置 devtool 策略
    mode: 'development'
}
```

推荐使用：
* 开发环境： cheap-module-eval-source-map
* 生产环境： source-map     none

### 16、准备生产环境
webpack 可以使用不同的配置文件，进行不同的编译。

1. 创建文件夹 config，将 webpack.config.js 复制两份

    * ./config/webpack.dev.js
    * ./config/webpack.prod.js

2. 修改 webpack.prod.js 配置，删除 webpack-dev-server 配置

    ```js
    .
    .
    .
    module.exports = {
        entry: {
            main:['./src/js/app.js','./src/index.html']
        },
        .
        .
        .
        //1. 设置 devtool
        devtool: 'source-map',
        //2. 设置 mode
        mode: 'production'
        //3. 删除 devServer 配置
    }
    ```
3. 修改 package.json 的指令

  ```json
  {
  	.
  	.
  	.
  	"scripts": {
          "dev": "webpack-dev-server --config ./config/webpack.dev.js",
          "build": "webpack --config ./config/webpack.prod.js"
      }
      .
      .
      .
  }
  ```

4. 开发环境指令
  * npm run dev   		用于开发环境   不打包文件
  * npm run build        用于生产环境    打包文件 （==打包后的index.html不能直接双击打开，需要启动服务==）
### 17、清除打包文件目录
每次打包生成了文件，都需要手动删除，引入插件 `clean-webpack-plugin` 帮助我们自动删除上一次生成的文件

1. 安装插件

   ```shell
   > npm install clean-webpack-plugin --save-dev
   ```

2. `webpack.prod.js` 引入插件

   ```js
   //1. 引入插件
   const { CleanWebpackPlugin } = require('clean-webpack-plugin'); 
   .
   .
   .
   module.exports = {
       .
       .
       .
       plugins: [
           new HtmlWebpackPlugin({
               template: './src/index.html', 
           }),
           //2. 配置插件
           new CleanWebpackPlugin() 
       ],
   }
   ```

3. 运行指令

   ```shell
   > npm run build
   ```

### 18、提取 CSS 成单独文件

前面的 CSS 样式代码都是放在 style 标签中，这里可以借助 mini-css-extract-plugin 抽离 CSS 文件

1. 安装插件
	
	```shell
	> npm install mini-css-extract-plugin --save-dev 
	```
	
2. 配置 webpack.prod.js

  ```js
  .
  .
  // 1. 引入插件
  const MiniCssExtractPlugin = require("mini-css-extract-plugin");
  
  module.exports = {
      .
      .
      .
      module: {
          rules: [
              {
                  test: /.less$/,
                  use: [
                      MiniCssExtractPlugin.loader,   	// 2. 修改配置 loader
                      'css-loader',
                      'less-loader'
                  ]
              }
          ]
      },
      plugins: [
          .
          .
          new MiniCssExtractPlugin({					// 3. 配置插件
              filename: "css/[hash:8].css",
          })
      ]
      .
      .
  }
  ```

3. 运行指令
	```
	> webpack
	```

### 19、添加 CSS 兼容
* 安装 loader
	
	```shell
	> npm install postcss-loader autoprefixer --save-dev 
	```
	
* 配置 loader
  ```js
  .
  .
  .
  module.exports = {
     	.
      .
      module: {
          rules: [
              {
                  test: /\.less$/, 
                  use: [
                      MiniCssExtractPlugin.loader,
                      'css-loader',
                      {
                          loader: 'postcss-loader',      		// 1. 设置 postcss-loader
                          options: {
                              plugins: [
                                  require("autoprefixer") 	// 2. 配置插件
                              ]
                          }
                      },
                      'less-loader',
                  ]
              },
              .
              .
          ]
      }
      
  }
  ```
  
* 在项目目录下创建 `.browserslistrc`  ==这里一要加目标浏览器设置==
  ```con
	chrome 50
	last 1 versions
	ie 10
	iOS 7
	```
  
* 运行指令：
  
  ```shell
  > npm run build
  ```
  
  

### 20、压缩 CSS
1. 安装插件

   ```shell
   > npm install optimize-css-assets-webpack-plugin --save-dev 
   ```

2. 引入插件，配置插件

    ```js
    //1. 引入插件
    const OptimizeCssAssetsPlugin = require('optimize-css-assets-webpack-plugin');

    module.exports = {

        plugins: [
            .
            .
            .
            //2. 配置插件
            new OptimizeCssAssetsPlugin({
                cssProcessorPluginOptions: {
            		//移除所有的注释
                    preset: ['default', {discardComments: {removeAll: true}}],
                },
                // 解决没有source map问题
                cssProcessorOptions: {                  
                    map: true
                }
            })
        ],
        mode: 'production'
    }
    ```
    
3. 运行指令

   ```shell
   > npm run build
   ```
   
   

## webpack配置流程

1. 创建项目，初始化webpack

   ```shell
    npm init
   ```

2. 创建src目录，src目录下创建js文件夹和public文件夹

3. 安装webpack(如果已经全局安装webpack，可以跳过此步)

   ```
   npm i webpack webpack-cli -D
   ```

4. 在项目根目录下创建webpack.config.js文件

5. 在webpack.config.js中配置入口entry、出口output、module、plugins（代码查看官网）

5. 在package.json文件中的scripts对象中设置"start":"webpack"

7. 运行webpack指令【npm run start 】

### webpack.config.js 文件基本模板

```js
const path = require('path');
module.exports = {
    //（入口） 
    entry: path.resolve(__dirname, 'src/index.js'), //当前文件所在目录位置
    //（出口）
    output: {
        path: path.resolve(__dirname, 'dist'),
        filename: 'main.js'
    },
    //(配置loader) 
    module: {
        rules: []
    },
    //配置插件
    plugins: []
}
```

### 设置开发模式或生产模式

```js
// webpack.config.js配置文件

module.exports = {
  mode: 'development'  // 开发模式development,生产模式production
};
```

### 打包生成HTML文件
安装HtmlWebpackPlugin

```shell
npm install --save-dev html-webpack-plugin
```

```js
// webpack.config.js配置文件
const HtmlWebpackPlugin = require('html-webpack-plugin');//第一步

var webpackConfig = {
  entry: '......',
  output: {
    ......
  },
  plugins: [
    	new HtmlWebpackPlugin({                     //第二步
            template: './src/public/index.html'  //设置html地址
        })    
  ] 
};
```

### 清理dist
安装插件

```shell
npm install --save-dev clean-webpack-plugin
```

```js
// webpack.config.js 配置文件
const { CleanWebpackPlugin } = require('clean-webpack-plugin');//第一步

var webpackConfig = {
  entry: '......',
  output: {
    ......
  },
  plugins: [
    new CleanWebpackPlugin()    //第二步
  ] 
};
```

### 将ES6文件解析成ES5文件
安装插件

```shell
npm install babel-loader@8.0.0-beta.0 @babel/core @babel/preset-env webpack
```

```js
// webpack.config.js配置文件
module: {
  rules: [
    {
      test: /\.js$/,
      exclude: /(node_modules|bower_components)/,
      use: {
        loader: 'babel-loader',
        options: {
          presets: ['@babel/preset-env']
        }
      }
    }
  ]

}
```

【babel补丁，解析es7、es8语法】
安装插件

```shell
npm install --save @babel/polyfill
```

```js
module.exports = {
    entry: ["@babel/polyfill", "入口地址"]
}
```

### 打包CSS文件
安装插件

```shell
npm install --save-dev css-loader style-loader
```

```js
// webpack.config.js配置文件
module: {
    rules: [
      {
        test: /\.css$/,
         use: [ 'style-loader', 'css-loader' ]
      }
    ]
}
```

安装less

```shell
npm install less less-loader --save-dev
```

```js
module.exports = {
    ...
    module: {
        rules: [{
            test: /\.less$/,
            use: ['style-loader', 'css-loader','less-loader']
        }]
    }
};
```



### 打包图片文件
安装插件

```shell
npm install --save-dev url-loader file-loader
```

```js
// webpack.config.js 配置文件
  module: {
    rules: [
      {
        test: /\.(png|jpg|gif)$/,
        use: [
          {
            loader: 'url-loader',
            options: {
              limit: 8192,//图片超过8k转base64图
              name:'[hash:8].[ext]'  //图片名字取8位哈希值
            }
          }
        ]
      }
    ]
  }
```

### 自动打包
安装插件

```shell
npm install webpack-dev-server --save-dev
```

```js
// webpack.config.js 配置文件
devServer: {
        port: 8000, //服务启动的端口
        open: true, //自动打开浏览器
        quiet:true //输入少量的提升信息
}

// 在package.json文件的scripts对象中添加"dev":"webpack-dev-server"
```

终端运行指令

```shell
npm run dev
```

### 定位出错的原始所在位子

```js
// webpack.config.js 配置文件
devtool:'cheap-module-eval-source-map'
```

### 安装 vue
安装vue

```shell
npm i vue
```

安装loader

```shell
npm install -D vue-loader vue-template-compiler
```

```js
// webpack.config.js 配置文件
const VueLoaderPlugin = require('vue-loader/lib/plugin')  

  module: {
    rules: [
      {
        test: /\.vue$/,
        loader: 'vue-loader'
      }
    ]
  },
  plugins: [
    new VueLoaderPlugin()
  ]


// 把style-loader改成 vue-style-loader
```

### 将其他文件拷贝到dist文件夹下
安装插件

```shell
npm install --save-dev copy-webpack-plugin@5.1.1
```

```js
// webpack.config.js配置文件
const CopyPlugin = require('copy-webpack-plugin');

《webpack.config.js》配置文件
plugins: [
       new CopyPlugin([
            {
                from:path.resolve(__dirname,"src/public"),
                to:path.resolve(__dirname,'dist'),
                ignore:['index.html']
            }
        ])
]
```

### 省略后缀名
```js
resolve:{
	extensions :[".js", ".json" ,".vue"]
}
```

### 以@为vue项目路径取别名
```js
resolve:{
    alias:{
            '@': path.resolve(__dirname, 'src')
    }
}
```

### 配置http代理转发
```js
devServer:{
    proxy: {
      "/api": {
        target: "http://localhost:3000",
        pathRewrite: {"^/api" : ""},
        changeOrigin : true
      }
    }
}
```

### 安装Element UI
```js
npm i element-ui -S
```

```js
// 完整引入
import Vue from 'vue';
import ElementUI from 'element-ui';
import 'element-ui/lib/theme-chalk/index.css';

Vue.use(ElementUI)
```

按需引入
安装插件

```shell
npm install babel-plugin-component -D
```

```js
// 在babel-loader中添加
{
  presets: [["es2015", { "modules": false }]],
  plugins: [
    [
      "component",
      {
        "libraryName": "element-ui",
        "styleLibraryName": "theme-chalk"
      }
    ]
  ]
}
```

### 图标字体处理

```js
rules:[
	{ test: /\.(eot|svg|ttf|woff|woff2)(\?\S*)?$/, loader:'file-loader'},
]
```

### 安装 Vuex
```shell
npm install vuex --save
```

```js
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)
```

### 安装vue-router
```shell
npm i vue-router -S
```

router 路由使用history配置

```js
devServer: {
        historyApiFallback: true
}

output: {
        publicPath: '/'
}
```



## webpack进阶

### HMR 模块热替换

> 应用程序运行过程中替换、添加或删除模块，而无需重新加载整个页面

修改 webpack.config.js 的 devServer 配置

```js
module.exports = {
    //index.html 不能自动刷新的解决方法
    //新增一个入口，解决开启热模块替换后首页无法刷新的问题
    entry: {
        main:['./src/js/app.js','./src/index.html']
    },
    .
    .
    .
    devServer: {
        open: true, 	
        compress: true, 
        port: 3000, 	
        hot: true		// 开启热模块替换功能
    },
    mode: 'development'
}
```

> js文件开启热模替换

- 编写监听完成js的HMR：

```js
import module from './xxx.js'

if (module.hot) {
	  // 若module.hot 为true，说明开启了HMR功能
	  module.hot.accept('./module1.js', () => {
	    //accept方法会监听 module1.js 文件的变化，一旦发生变化，会执行后面的回调函数,其他模块不会重新打包构建。
	    demo();
	  });
}
```

###  source-map

* 概述：当我们的代码经过了压缩以后，若源代码出了问题，控制台的错误提示很不友好，例如：报错永远是js的第一行有问题，我们需要将压缩过后代码的报错，映射回源文件的具体位置，就要使用source-map技术。
* 配置：
  1. 追加配置：```devtool: 'xxx'```
  2. 上述的xxx是映射模式，可选值可参考：https://www.webpackjs.com/configuration/devtool/
  3. 开发模式推荐值：```cheap-eval-source-map```
  4. 生产模式推荐值：```source-map```

```js
module.exports = {
    devtool:'source-map'
}
// source-map  生成外部的.map文件 + 源代码错误的位置 + 精准的错误信息 + 点击可定位
// inline-source-map  内联map文件 + 源代码错误的位置 + 精准的错误信息 + 点击可定位
// hidden-source-map  生成外部的.map文件 + 精准的错误信息
// eval-source-map  内联map文件(与代码混合) + 精准的错误信息 + 源代码错误的位置 + 点击可定位
// nosource-source-map  生成外部的.map文件 + 精准的错误信息 + 源代码错误的位置 + 点击可定位
// cheap-source-map  生成外部的.map文件 + 精准的错误信息
// cheap-eval-source-map  内联map文件 + 源代码错误的位置 + 精准的错误信息 + 点击可定位
```

> 使用原则：在不影响提示的前提下，追求速度

### one of

* 概述：正常来讲，一个文件只能被一个loader处理。 当一个文件要被多个loader处理，那么一定要指定loader执行的先后顺序，例如我们的loader中对js的处理：要先执行eslint 在执行babel

* 配置：

  ```javascript
  module:{
  		rules:[
  			{},//若有些文件不只是被一个loader处理，要把其他loader放在oneOf外侧
  			{
  				oneOf:[
  					{},//loader1
  					{},//loader2
  					{},//loader3
  				]
  			}
  		]
  	}
  ```

### 开启babel缓存

* 概述：在生产环境中我们是没有devServer的，所以我们有些时候只是改变了某几个文件，或一个文件后，重新build时，webpack也是会将所有的模块全都重新构建一遍，很浪费时间。

* 配置：在babel-loader配置中追加配置项：```cacheDirectory: true```

  ```js
  module: {
    	rules: [
          ...,
          {
            test: /\.js$/,
            exclude: /node_modules/,
            use: {
              loader: "babel-loader",
              options: {
                	presets: ['@babel/preset-env'],
               	cacheDirectory: true
              }
            }
          }
  	]
  }
  ```

### 服务器强缓存下的修复

* 概述：我们都知道，代码经过最终的打包之后是要部署在服务器上的，若服务器开启了，强缓存，就意味着：在强缓存期间，若某个文件要临时更改我们是办不到的。

* 配置方案一：

  * 文件名都混入hash值，每次wepack构建时会生成一个唯一的hash值，我们将这个值混入文件名。

  ```javascript
  output: {
      filename: 'js/built.[hash:10].js',
      path: resolve(__dirname, 'build')
    },
  
  new MiniCssExtractPlugin({
      filename: 'css/built.[hash:10].css'
    })
  ```

  * 弊端：因为每次打包都有一个唯一的hash值，所以每次打包之后没有修改的文件名字也发生了改变，就导致没有改变的文件也重新请求了服务器，效率不高。

* 配置方案二：

  * 使用contenthash值

  ```javascript
  output: {
      filename: 'js/built.[contenthash:10].js',
      path: resolve(__dirname, 'build')
    },
  
  new MiniCssExtractPlugin({
      filename: 'css/built.[contenthash:10].css'
  })
  ```

  * 优势：只有当文件的内容改变时，contenthash的值才会改变，内容不变就算多次打包，contenthash值也不变。

### tree-shaking

* 概述：有些时候，我们一个模块向外暴露了n个函数、对象、或其他一些数据，但是我们只是用到了其中的一个或几个，那在最终打包的时候，我们只希望把我们所用的打包进去，这时候就要tree-shaking，即：去除无用代码。
* 配置：满足两个条件webpack会自动开启tree-shaking：

  1. 必须使用ES6模块化  
  2. 开启production环境

* 备注：开启了tree-shaking后，有时候webpack会错误的把我们的样式代码全部干掉，解决办法是：在package.json中追加一个配置：```"sideEffects":["*.css","*.less"]```

### code split  将第三方库单独打包

1. 方案一：调整入口文件，写成多入口形式
   1. 存在问题：多个文件都用到第三方库会导致重复打包。
2. 方案二：借助optimization配置将node_modules中的第三方库单独打包
   具体配置：

```javascript
	 /*
	    1. 可以将第三方库单独打包
	    2. 会自动分析各文件中均用到的第三方包。
	  */
	  optimization: {
	    splitChunks: {
	      chunks: 'all'
	    }
	  }
```

3. 特殊情况(方案三)：a.js中要引入b.js暴露出来的内容，就不可避免在a.js中引入b.js，这样就会导致webpack打包时把a.js和b.js打包成一个js文件，但有时我们的需求是b.js要是一个单独的文件。

```javascript
	import(/* webpackChunkName: 'test' */'./module2').then(
		(result)=>{
			// eslint-disable-next-line
			console.log(result)
		}
	).catch(()=>{
		// eslint-disable-next-line
		console.log('error');
	})
```

4. 备注：如果采取方案三可能eslint-loader的语法检查不认识方案三的引入方式，会报错，我们需要安装：babel-eslint，```npm i babel-eslint -D```随后再package.json中的eslintConfig中追加：```"parser": "babel-eslint"```

###  lazy loading

* 概述：
  1. 懒加载：当文件需要使用时才加载
    2. 正常加载：可以认为是并行加载（同一时间加载多个文件） 
* 懒加载配置：

```javascript
	btn.onclick = ()=>{
		import(/* webpackChunkName: 'test'*/'./test').then(({ demo }) => {
		    console.log(demo(4, 5));
		  });
	} 
```

### PWA

* 概述，PWA即：渐进式网络开发应用程序(离线可访问)，使网页应用在无网络连接状态不至于白屏。

* 使用到的技术：google推出的workbox，我们在webpack中使用的插件：workbox-webpack-plugin

* 配置：

  1. ```npm i workbox-webpack-plugin -D```
  2. ```const WorkboxWebpackPlugin = require('workbox-webpack-plugin');```
  3. 实例化插件

  ```javascript
  new WorkboxWebpackPlugin.GenerateSW({
        /*
          1. 帮助serviceworker快速启动
          2. 删除旧的 serviceworker生成一个 serviceworker 配置文件~
        */
        clientsClaim: true,
        skipWaiting: true
    })
  ```

  3. 注册serviceWorker

  ```javascript
  // 一般写在入口js文件的最下面 ，用来判断浏览器是否有serviceWorker属性
  if ('serviceWorker' in navigator) {
    window.addEventListener('load', () => {
      navigator.serviceWorker
        .register('/service-worker.js')
        .then(() => {
          console.log('sw注册成功了~');
        })
        .catch(() => {
          console.log('sw注册失败了~');
        });
    });
  }
  ```
  
  注意：PWA技术必须项目上线才能使用

### 多进程打包(优化webpack)

* 概述：多进程打包可以让构建速度更快(不是绝对的)

* 配置：```npm i thread-loader -D```

* 修改babel-loader的配置，追加一个loader ,

  ```js
  use:[
      {
          loader: 'babel-loader',
  		...
      },
      {
          loader: 'thread-loader',
          options: {
            workers: 2 // 进程2个( 并不是进程越多越好 )
          }
      }
  ]
  ```

### externals

* 概述：忽略指定的第三方库

  ```js
  externals:{
    jquery:'jQuery'
  }
  ```



## ES6暴露方式

- 默认暴露

  ```js
  export default {
      name : 'Tank'
  }
  ```

  ```js
  import obj from './xxx'
  ```

  

- 分别暴露

  ```js
  export function aaa(){
  	...
  }
  export function bbb(){
  	...
  }
  ```

  ```js
  import { aaa , bbb } from './xxx'
  ```

  

- 统一暴露

  ```js
  const aaa = '111'
  const bbb = '222'
  
  export { aaa , bbb } // 简写形式
  export { aaa as aaa , bbb as bbb } // 完整形式
  ```

  ```js
  import { aaa , bbb } from './xxx'
  ```

  




## 附录

### browserslist 

browserslist 目标浏览器配置表，可以针对目标浏览器进行编译处理，避免不必要的兼容代码

配置的方法有两种，一种是在 package.json 中，一种是创建 `.browserslistrc`

package.json 形式

```json
{
	.
	.
	.
	"browserslist": [
        "> 1%",
        "last 2 versions"
    ]
}
```

`.browserslistrc` 形式

```
> 1%
last 2 versions
```

配置规则介绍

| 规则                   | 介绍                                                  |
| ---------------------- | ----------------------------------------------------- |
| > 1%                   | 全球超过1%人使用的浏览器                              |
| > 5% in US             | 指定国家使用率覆盖                                    |
| last 2 versions        | 所有浏览器兼容到最后两个版本根据CanIUse.com追踪的版本 |
| Firefox > 20           | 指定浏览器的版本范围                                  |
| not ie <=8             | 排除 ie8 及以下                                       |
| Firefox 12.1           | 指定浏览器的兼容到指定版本                            |
| since 2013             | 2013年之后发布的所有版本                              |
| not dead with > 0.2%   | 仍然还在使用且使用率大于 0.2%                         |
| last 2 Chrome versions | 最新的两个 Chrome 配置                                |
| cover 99.5%            | 99.5% 的浏览器都是目标                                |

### 将CSS文件变成单独文件

安装ExtractTextWebpackPlugin

```shell
npm install --save-dev extract-text-webpack-plugin
```

引入

```js
const ExtractTextPlugin = require("extract-text-webpack-plugin");
```

```js
module.exports = {
  module: {
    rules: [
      {
        test: /\.less$/,
        use: ExtractTextPlugin.extract({ // 3，然后ExtractTextPlugin生成文件
          fallback: "style-loader",  // 4.如果失败将css写入js文件中
          use: ["css-loader","less-loader"]// 1,首先处理less，2，再处理css
        })
      }
    ]
  },
  plugins: [
    // 实例化，取名叫index.css
    new ExtractTextPlugin("css/index.css"),
  ]
}


// 注意：  css-loader 需要降为3.6.0版本才可用
```

## 快速搭建一个服务器的方法

1. Express

2. json-server

3. mock

4. server

   ```shell
   -- 全局安装
   npm server -g
   ```

   ```shell
   -- 在项目已目录下运行cmd
   server
   ```

   

