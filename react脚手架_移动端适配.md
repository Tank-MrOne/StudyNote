# React脚手架适配

1. 第一步：暴露react配置

   ​	```yarn eject```

2. 第二步：安装依赖

   ​	```	yarn add lib-flexible postcss-px2rem```

3. 第三步：在webpack.config.js中引入px2rem 

   ​	```const px2rem = require('postcss-px2rem')```

4. 第四步：在index.js文件中引入lib-flexible

   ​	```import 'lib-flexible'```

5. 第五步：安装less相关依赖

   ​	```yarn add less less-loader ```

6. 第六步：webpack.config.js中配置根字体大小，值为设计稿宽度/10

   ```js
   options: {
             // Necessary for external CSS imports to work
             // https://github.com/facebook/create-react-app/issues/2677
             ident: 'postcss',
             plugins: () => [
               require('postcss-flexbugs-fixes'),
               require('postcss-preset-env')({
                 autoprefixer: {
                   flexbox: 'no-2009',
                 },
                 stage: 3,
               }),
               // Adds PostCSS Normalize as the reset css with default options,
               // so that it honors browserslist config in package.json
               // which in turn let's users customize the target behavior as per their needs.
               postcssNormalize(),
       
               // 根据设计稿，设置根字体大小
               px2rem({ remUnit: 37.5 })
             ],
             sourceMap: isEnvProduction && shouldUseS ourceMap,
           },
   ```

7. 第七步：在webpack.config.js中找到cssRegex位置，定义lessRegex、lessModuleRegex

   ```js
   const cssRegex = /\.css$/;
   const cssModuleRegex = /\.module\.css$/;
   
   // 引入less
   const lessRegex = /\.less$/;
   const lessModuleRegex = /\.module\.less$/;
   
   const sassRegex = /\.(scss|sass)$/;
   const sassModuleRegex = /\.module\.(scss|sass)$/;
   ```

8. 第八步：getStyleLoaders中，接收lessOptions，追加less-loader

   ```js
   const getStyleLoaders = (cssOptions, lessOptions, preProcessor) => {
       const loaders = [
         isEnvDevelopment && require.resolve('style-loader'),
         isEnvProduction && {
           loader: MiniCssExtractPlugin.loader,
           // css is located in `static/css`, use '../../' to locate index.html folder
           // in production `paths.publicUrlOrPath` can be a relative path
           options: paths.publicUrlOrPath.startsWith('.')
             ? { publicPath: '../../' }
             : {},
         },
         {
           loader: require.resolve('css-loader'),
           options: cssOptions,
         },
         {
           loader: require.resolve('less-loader'),
           options: lessOptions,
         },
         ... 
   }
   ```

9. 参第九步：考cssRegex和cssModuleRegex，追加less相关配置。

   ```js
               {
                 test: cssRegex,
                 exclude: cssModuleRegex,
                 use: getStyleLoaders({
                   importLoaders: 1,
                   sourceMap: isEnvProduction && shouldUseSourceMap,
                 }),
                 // Don't consider CSS imports dead code even if the
                 // containing package claims to have no side effects.
                 // Remove this when webpack adds a warning or an error for this.
                 // See https://github.com/webpack/webpack/issues/6571
                 sideEffects: true,
               },
               // Adds support for CSS Modules (https://github.com/css-modules/css-modules)
               // using the extension .module.css
               {
                 test: cssModuleRegex,
                 use: getStyleLoaders({
                   importLoaders: 1,
                   sourceMap: isEnvProduction && shouldUseSourceMap,
                   modules: {
                     getLocalIdent: getCSSModuleLocalIdent,
                   },
                 }),
               },
   
               //配置less规则
               {
                 test: lessRegex,
                 exclude: lessModuleRegex,
                 use: getStyleLoaders({
                   importLoaders: 1,
                   sourceMap: isEnvProduction && shouldUseSourceMap,
                 }),
                 sideEffects: true,
               },
               {
                 test: lessModuleRegex,
                 use: getStyleLoaders({
                   importLoaders: 1,
                   sourceMap: isEnvProduction && shouldUseSourceMap,
                   modules: {
                     getLocalIdent: getCSSModuleLocalIdent,
                   },
                 }),
               },
   ```

   

   

   

