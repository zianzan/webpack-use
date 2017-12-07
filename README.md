# webpack 模块化打包工具

## Webpack

![webpack](./images/webpack-1.png)

- [webpack 官网](http://webpack.github.io/)
- bundle `[ˈbʌndl]` 捆绑，收集，归拢，把…塞入

### 概述

> webpack 是一个现代 JavaScript 应用程序的模块打包器(module bundler)  
> webpack 是一个**模块化方案（预编译）**  
> webpack获取具有依赖关系的模块，并生成表示这些模块的静态资源

- 四个核心概念：**入口(entry)**、**输出(output)**、**加载器loader**、**插件(plugins)**

```html
模块化方案: webpack 和 requirejs（通过编写代码的方式将前端的功能，划分成独立的模块）

browserify 是与 webpack 相似的模块化打包工具

webpack 预编译 (在开发阶段通过webpack进行模块化处理, 最终项目上线, 就不在依赖于 webpack)
requirejs 线上的编译( 代码运行是需要依赖与 requirejs 的 )
```

### webpack起源

- webpack解决了现存模块打包器的两个痛点：
  - 1 **Code Spliting** - 代码分离
  - 2 **静态资源的模块化处理方案**

### webpack 的目标

- 1 分离依赖树到chunks（块，文件），实现按需加载
- 2 保证初始化加载时间更短
- 3 每一个静态资源能够被当作一个模块处理
- 4 能够整合第三方包使其成为模块
- 5 能够定制模块打包的每一部分（环节）
- 6 适合大型项目

### webpack与模块

- [前端模块系统的演进](http://zhaoda.net/webpack-handbook/module-system.html)
- 在webpack看来：**所有的静态资源都是模块**
- webpack 模块能够识别以下等形式的模块之间的依赖：

- JS的模块化规范：
  - ES2015 `import` `export`
  - CommonJS `require()` `module.exports`
  - AMD `define` 和 `require`

- 非JS等静态资源：
  - css/sass/less 文件中的 `@import`
  - 图片连接，比如：样式 `url(...)` 或 HTML `<img src=...>`
  - 字体 等

### webpack能做什么

- 1 模块化
- 2 将`ES6`、`TypeScript`、`CoffeeScript`等浏览器无法识别的语言转化为`ES5`
- 3 将`SCSS`、`LESS`等预编译器创建的CSS转化为浏览器识别的CSS
- 4 进行文件压缩、合并、拷贝
- 5 启动服务器，实现页面实时刷新，热加载
- 6 项目上线，通过配置生成项目目录，优化代码提升性能

```html
1 webpack 是一个模块化打包工具
2 两个核心功能：1 模块化 2 打包
3 webpack能够对前端中使用的所有的静态资源进行模块化分析处理
```

### webpack文档和资源

- [webpack 中文网](https://doc.webpack-china.org/)
- [webpack 1.0](http://webpack.github.io/docs/what-is-webpack.html)
- [webpack 2.x+](https://webpack.js.org/)
- [入门Webpack，看这篇就够了](http://www.jianshu.com/p/42e11515c10f#)

---

## 安装webpack

- 全局安装：`npm i -g webpack`
  - 目的：在任何目录中通过CLI使用 `webpack` 这个命令
- 项目安装：`npm i -D webpack`
  - 目的：执行当前项目的构建

## webpack的基本使用

- 安装：`npm i -D webpack`
- webpack的两种使用方式：1 命令行 2 配置文件（`webpack.config.js`）

### 命令行方式演示 - 案例：隔行变色

- 1 使用`npm init -y` 初始package.json，使用npm来管理项目中的包
- 2 新建`index.html`和`index.js`，实现隔行变色功能
- 3 运行`webpack src/js/index.js dist/bundle.js`进行打包构建，语法是：`webpack 入口文件 输出文件`
- 4 注意：需要在页面中引入 输出文件 的路径（此步骤可通过配置webpack去掉）

```js
/*
  src/js/index.js
*/

// 1 导入 jQuery
import $ from 'jquery'
// 2 获取页面中的li元素
const $lis = $('#ulList').find('li')
// 3 隔行变色
// jQuery中的 filter() 方法用来过滤jquery对象
$lis.filter(':odd').css('background-color', '#def')
$lis.filter(':even').css('background-color', 'skyblue')
```

### 配置文件方式（推荐）

```js
/*
  webpack.config.js

  运行命令：webpack

  entry 入口的配置说明：
  https://doc.webpack-china.org/concepts/entry-points
*/

var path = require('path')
module.exports = {
  // 入口文件
  entry: path.join(__dirname, 'src/js/main.js'),

  // 输出文件
  output: {
    path: path.join(__dirname, './dist'),   // 输出文件的路径
    filename: 'bundle.js'                   // 输出文件的名称
  }
}
```

## webpack-dev-server

- 安装：`npm i -D webpack-dev-server`
- 作用：配合webpack，创建开发环境（启动服务器、监视文件变化、自动编译、刷新浏览器等），提高开发效率
- 注意：无法直接在终端中执行 `webpack-dev-server`，需要通过 `package.json` 的 `scripts` 实现
- 使用方式：`npm run dev`

```json
"scripts": {
  "dev": "webpack-dev-server"
}
```

### 使用说明

- 注意：`webpack-dev-server`将打包好的文件存储在内存中，提高编译和加载速度，效率更高
- 注意：输出的文件被放到项目根目录中
  - 命令行中的提示：`webpack output is served from /`
  - 在`index.html`页面中直接通过 `/bundle.js` 来引入内存中的文件

### 配置说明 - CLI配置

- `--contentBase` ：主页面目录
  - `--contentBase ./`：当前工作目录
  - `--contentBase ./src`：webpack-dev-server 启动的服务器，我们在浏览器中打开的时候会自动展示 ./src 中的 index.html 文件
- `--open` ：自动打开浏览器
- `--port` ：端口号
- `--hot` ：热更新，只加载修改的文件(按需加载修改的内容)，而非全部加载

```js
/* package.json */
/* 运行命令：npm run dev */

{
  "scripts": {
    "dev": "webpack-dev-server --contentBase ./src --open --port 8888 --hot"
  }
}
```

### 配置说明 - webpack.config.js

```js
const webpack = require('webpack')

devServer: {
  // 设置服务器内容的根目录
  // Tell the server where to serve content from
  // https://webpack.js.org/configuration/dev-server/#devserver-contentbase
  contentBase: path.join(__dirname, './'),
  // 自动打开浏览器
  open: true,
  // 端口号
  port: 8888,

  // --------------- 1 热更新 -----------------
  hot: true
},

plugins: [
  // ---------------- 2 启用热更新插件 ----------------
  new webpack.HotModuleReplacementPlugin()
]
```

## html-webpack-plugin 插件

- 安装：`npm i -D html-webpack-plugin`
- 作用：1 根据模板，自动生成html页面 2 自动引入js、css等文件
- 优势：页面存储在内存中，自动引入`bundle.js`、`css`等文件
- 如果使用该插件，那么`webpack-dev-server`中的contentBase配置项就可以省略了

```js
/* webpack.config.js */
const htmlWebpackPlugin = require('html-webpack-plugin')

// ...
plugins: [
  new htmlWebpackPlugin({
    // 模板页面路径
    template: path.join(__dirname, './index.html'),
  })
]
```

## Loaders（加载器）

- [webpack - Loaders](https://webpack.js.org/loaders/)
- [webpack - 管理资源示例](https://doc.webpack-china.org/guides/asset-management)

> webpack enables use of loaders to preprocess files. This allows you to bundle any static resource way beyond JavaScript. 

- webpack只能处理JavaScript资源
- webpack通过loaders处理非JavaScript静态资源

## CSS打包

- 1 CSS打包文件（加载）
- 2 SASS打包文件（编译为CSS）

### 使用webpack打包CSS

- 安装：`npm i -D style-loader css-loader`
- 注意：use中模块的顺序不能颠倒，加载顺序：从右向左加载

```js
/* index.js */

// 导入 css 文件
import './css/app.css'


/* webpack.config.js */

// 配置各种资源文件的loader加载器
module: {
  // 配置匹配规则
  rules: [
    // test 用来配置匹配文件规则（正则）
    // use  是一个数组，按照从后往前的顺序执行加载
    {test: /\.css$/, use: ['style-loader', 'css-loader']},
  ]
}
```

### 使用webpack打包sass文件

- 安装：`npm i -D sass-loader node-sass`
- 注意：`sass-loader` 依赖于 `node-sass` 模块

```js
/* webpack.config.js */

// 参考：https://webpack.js.org/loaders/sass-loader/#examples
// "style-loader"  ：creates style nodes from JS strings 创建style标签
// "css-loader"    ：translates CSS into CommonJS 将css转化为CommonJS代码
// "sass-loader"   ：compiles Sass to CSS 将Sass编译为css

module:{
  rules:[
    {test: /\.scss$/, use: ['style-loader', 'css-loader', 'sass-loader']},
  ]
}
```

## 图片和字体打包

- 安装：`npm i -D url-loader file-loader`
- `file-loader`：加载并重命名文件（图片、字体 等）
- `url-loader`：将图片或字体转化为base64编码格式的字符串，嵌入到样式文件中

```js
/* webpack.config.js */

module: {
  rules:[
    // 打包 图片文件
    { test: /\.(jpg|png|gif|jpeg)$/, use: 'url-loader' },

    // 打包 字体文件
    { test: /\.(woff|woff2|eot|ttf|otf|svg)$/, use: 'file-loader' }
  ]
}
```

### 图片打包注意点

- 1 默认将图片转为base64编码格式
- 2 `limit`参数的作用：
  - 限制图片的文件大小，单位为：字节(byte)
  - 文件重命名为哈希值，保证文件不会重复。例如：一张图片拷贝一个副本，这两个图片实际是同一个
- 3 规则：
  - 当图片文件大小（字节）`小于`指定的limit时，图片被转化为base64编码格式
  - 当图片文件大小（字节）`大于等于`指定的limit时，图片被重命名，不使用base64编码，此时，需要`file-loader`来加载处理图片

- 如果图片是以文件路径的形式来引用的，那么，`file-loader`会计算图片的 MD5值，来作为图片的的名称
- MD5加密：每一个文件都有一个唯一的MD5值，只要是同一个文件，那么这个文件的MD5值就是相同的

```js
/* webpack.config.js */

module: {
  rules: [
    // {test: /\.(jpg|png|gif|jpeg)$/, use: 'url-loader?limit=100'},
    {
      test: /\.(jpg|png|gif|jpeg)$/,
      use: [
        {
          loader: 'url-loader',
          options: {
            limit: 8192
          }
        }
      ]
    }
  ]
}
```

### 字体文件打包说明

- 1 可以使用 `url-loader` 或者 `file-loader`
- 2 `url-loader` 会将字体文件解析为 base64编码格式的字符串，嵌入到CSS样式中
- 3 `file-loader` 以文件形式加载字体文件

## ES6语法 - class关键字

- ES6以前，JS是没有class概念的，而是通过构造函数+原型的方式来实现的
- 注意：ES6中的class仅仅是一个语法糖，并不是真正的类，与Java等服务端语言中的类是有区别的
- [ES6 - 文档](http://es6.ruanyifeng.com/#docs/class)

```js
class Person {
  // 静态属性
  static testName = '静态属性'
  static age = 20

  constructor() {
    // 实例属性
    this.isLoading = true
  }
}

console.log(Person.testName)
```

### 静态属性和实例属性

- 静态属性：直接通过类名就可以访问到，不需要创建类的实例就能访问
- 实例属性：必须先创建类的实例对象，然后，通过实例对象访问
- 说明：由于webpack不识别`static`关键字，需要借助于`babel-loader`来处理ES6语法

## 在webpack中配置babel-loader

- [babel全家桶](https://github.com/brunoyang/blog/issues/20)
- 安装：`npm i -D babel-core babel-loader babel-plugin-transform-runtime`
- 安装：`npm i -D babel-preset-es2015 babel-preset-stage-0`
- 安装：`npm i -S babel-runtime`
  - 注意：**stage-0 依赖于 es2015**
  - ES6 -> ES7 过程中要经历：4个阶段（0-3），并且 stage-* 的语法比ES6更新

- `babel-preset-env`：相当于是 es2015/es2016/es2017 的集合

```html
babel-preset-env 相当与：babel-preset-es2015, babel-preset-es2016, and babel-preset-es2017 together

但是要注意：babel-preset-env 不包含 stage-*，所以，env是无法处理静态属性的语法
```

### 基本使用（两步）

- 第一步：

```js
/* webpack.config.js */

module: {
  rules: [
    // exclude 排除，不需要编译的目录，提高编译速度
    // exclude的正则匹配：'/node_modules/vue/dist/vue.js'
    {test: /\.js$/, use: 'babel-loader', exclude: /node_modules/}
  ]
}
```

- 第二步：在项目根目录中新建`.babelrc`配置文件

```json
/* .babelrc */

// 将来babel-loader运行的时候，会检查这个配置文件，并读取相关的语法和插件配置
{
  "presets": ["es2015", "stage-0"],
  "plugins": ["transform-runtime"]
}
```

## babel的说明

### babel-preset-*

> Babel通过语法转换器，能够支持最新版本的JavaScript语法  
> babel-preset-* 用来指定我们书写的是什么版本的JS代码

- 作用：将浏览器无法识别的新语法转化为ES5代码
- [ES6语法提案的批准流程](http://es6.ruanyifeng.com/#docs/intro#语法提案的批准流程)
  - ES2015 也就是 ES6, 下一个要发布的ES7, 从 ES6 到 ES7之间经历了 5 个阶段
  - 一般进入到 stage 4 就可以认为是下一个版本的语法了!
  - babel-preset-es2015 能够转换es6的语法
  - babel-preset-stage-0 能够转换比es6更新的语法

```html
Stage 0 - Strawman（展示阶段）
Stage 1 - Proposal（征求意见阶段）
Stage 2 - Draft（草案阶段）
Stage 3 - Candidate（候选人阶段）
Stage 4 - Finished（定案阶段）

Stage 0 is "i've got a crazy idea", 
stage 1 is "this idea might not be stupid", 
stage 2 is "let's use polyfills and transpilers to play with it", 
stage 3 is "let's let browsers implement it and see how it goes", 
stage 4 is "now it's javascript".
```

### babel-polyfill 和 transform-runtime

- 作用：实现浏览器对不支持API的兼容（兼容旧环境、填补）
- [polyfill](https://babeljs.io/docs/usage/polyfill/#usage-in-node-browserify-webpack)
- [transform-runtime](https://babeljs.io/docs/plugins/transform-runtime/)
- 命令：`npm i -S babel-polyfill`
- 命令：`npm i -D babel-plugin-transform-runtime` 和 `npm i -S babel-runtime`
- 为什么要使用 babel-polyfill 和 transform-runtime 这两个包：
  - 为了在低版本浏览器中使用高级的ES6或ES7的方法或函数
  - 'abc'.split('')
  - 'abc'.padStart(10)

```html
区别：
polyfill 污染全局环境、支持实例方法
runtime  不污染全局环境、不支持实例方法

polyfill：如果想要支持全局对象（比如：`Promise`）、静态方法（比如：`Object.assign`）或者**实例方法**（比如：`String.prototype.padStart`）等，那么就需要使用`babel-polyfill`

babel-runtime ：提供了兼容旧环境的函数，使用的时候，需要我们自己手动引入
  比如： const Promise = require('babel-runtime/core-js/promise')
  问题：
    1 手动引入太繁琐
    2 多个文件引入同一个helper（定义），造成代码重复，增加代码体积
    3 安装该插件即可
babel-plugin-transform-runtime：
    1 自动引入helper（比如，上面引入的 Promise）
    2 babel-runtime提供helper定义，引入这个helper即可使用，避免重复
    3 babel-runtime包中的代码会被打包到你的代码中（-S）
    4 依赖于 babel-runtime 插件

transform-runtime插件的使用：直接在 .bablerc 文件中，添加一个 plugins 的配置项即可！！！
  "plugins": [
    "transform-runtime"
  ]
```

```js
/* 1 main.js */
// 第一行引入
require("babel-polyfill")

var s = 'abc'.padStart(4)
console.log(s)


// 2 webpack.config.js 配置
module.exports = {
  entry: ['babel-polyfill', './js/main.js']
}
```

### 总结

```html
babel-core babel核心包

babel-loader 用来解析js文件

babel-preset-* 能够将 抽象语法树（AST） 转化为浏览器能够识别的语法
  就是：将 浏览器不能识别的高版本的JS代码，转化为ES5这一类浏览器能够识别的代码
  注意: 只能处理新的语法( static / 箭头函数 ) , 但是对于一些新的JS API 无法处理

transform-runtime / babel-polyfill 提供浏览器不识别的全局对象或者新的API的兼容实现，以达到兼容浏览器的目的

// 判断浏览器是否兼容 padStart 这个 API
if (!String.prototype.padStart) {
  // 如果不兼容, 就自己模拟 padStart的功能实现一份
  String.prototype.padStart = function padStart(targetLength,padString) {
  }
}
```

---

## Webpack 发布项目

- [webpack 打包的各种坑](https://dailc.github.io/2017/03/13/webpackfreshmanualAndBug.html)

### 项目发布

- 开发期间，为了提高工作效率和编译速度，我们使用`webpack-dev-server`插件来辅助开发
- 项目发布：剔除开发期间用到的开发工具，模块化分析和打包项目代码

### 创建项目发布配置文件

- 开发期间配置文件：`webpack.config.js`
- 项目发布配置文件：`webpack.prod.js` （文件名称非固定）
- 命令：`webpack --config webpack.prod.js` 指定配置文件名称运行webpack
- 参数：`--display-error-details` 用于显示webpack打包的错误信息

```json
/* package.json */

"scripts": {
  "build": "webpack --config webpack.prod.js"
}
```

```html
1 在项目根目录中创建 webpack.prod.js 文件
2 在 package.json 中, 配置一个 scripts
3 在 终端中 通过 npm run build 对项目进行打包
```

### 初次打包的问题

- 1 js、html 没有压缩
- 2 所有的文件都放到了同一个目录中
- 3 没有css文件（实际包含在了：bundle.js 中）

```html
打包处理的过程：
1 删除掉 devServer 相关的配置项
2 将图片和字体文件输出到指定的文件夹中
3 自动删除dist目录
4 分离第三方包（将使用的vue等第三方包抽离到 vender.js 中）
5 压缩混淆JS 以及 指定生成环境
6 抽取和压缩CSS文件
7 压缩HTML页面
```

### 处理图片路径

- 注意：如果`limit`小于比图片大，那么图片将被转化为`base64`编码格式
- [name参数介绍](https://github.com/webpack-contrib/file-loader)

```js
/* webpack.prod.js */
// 处理URL路径的loader

{
  test: /\.(jpg|png|gif|bmp|jpeg)$/,
  use: {
    loader: 'url-loader',
    options: {
      limit: 8192,
      // name参数：重命名文件以及修改文件路径
      // [hash:7]：表示使用7位哈希值代表文件名称
      // [ext]：表示保持文件原有后缀名
      // images 表示图片生成到哪个文件夹中
      name: 'images/[hash:7].[ext]'
      // name: 'imgs/img-[hash:7].[ext]'
    }
  }
},
```

### 自动删除dist目录

- 安装：`npm i -D clean-webpack-plugin`
- 作用: 每次打包之前, 删除上一次打包的dist目录

```js
/* webpack.prod.js */
const cleanWebpackPlugin = require('clean-webpack-plugin')

plugins: [
  // 创建一个删除文件夹的插件，删除dist目录
  new cleanWebpackPlugin(['./dist'])
]
```

### 分离第三方包

- 目的：将公共的第三方包，抽离为一个单独的包文件，这样防止重复打包！
  - 例如：3个页面中都引入了jQuery，不分离的话，会被打包3次
  - 抽离后, jQuery文件只会被打包一次, 用到的地方仅仅引用

```js
/* webpack.prod.js */

// 1 入口 -- 打包文件的入口
entry: {
  // 项目代码入口
  app: path.join(__dirname, './src/js/main.js'),
  // 第三方包入口
  vendor: ['vue', 'vue-router', 'axios']
},

output: {
  // 修改输出文件的名称
  filename: 'js/[name].[chunkhash].js',
},

plugins: [
  // 分离第三方包（公共包文件）
  new webpack.optimize.CommonsChunkPlugin({
    // 将 entry 中指定的 ['vue', 'vue-router', 'axios'] 打包到名为 vendor 的js文件中
    // 第三方包入口名称，对应 entry 中的 vendor 属性
    name: 'vendor',
  }),
]
```

### 压缩混淆JS

- 注意：**uglifyjs 无法压缩ES6的代码**

```js
plugins: [
  // 优化代码
  // https://github.com/webpack-contrib/uglifyjs-webpack-plugin/tree/v0.4.6
  new webpack.optimize.UglifyJsPlugin({
    // 压缩
    compress: {
      // 移除警告
      warnings: false
    }
  }),

  // 指定环境为生产环境：vue会根据这一项启用压缩后的vue文件
  new webpack.DefinePlugin({
    'process.env': {
      'NODE_ENV': JSON.stringify('production')
    }
  })
]
```

### 抽取和压缩CSS文件

- 安装：抽离 `npm i -D extract-text-webpack-plugin`
- 安装：压缩 `npm i -D optimize-css-assets-webpack-plugin`
- [webpack 抽离CSS文档](https://doc.webpack-china.org/plugins/extract-text-webpack-plugin/)
- [压缩抽离后的CSS](https://www.npmjs.com/package/optimize-css-assets-webpack-plugin)

```js
/* webpack.prod.js */

// 分离 css 到独立的文件中
const ExtractTextPlugin = require("extract-text-webpack-plugin");
// 压缩 css 资源文件
const OptimizeCssAssetsPlugin = require('optimize-css-assets-webpack-plugin')

// bug描述: 生成后面的css文件中图片路径错误，打开页面找不到图片
// 解决：google搜索 webpack css loader 样式图片路径
output: {
  // ...

  // https://doc.webpack-china.org/configuration/output/#output-publicpath
  // 设置公共路径
  publicPath: '/',
},

module: {
  rules: [
    {
      test: /\.css$/,
      use: ExtractTextPlugin.extract({
        fallback: "style-loader",
        use: "css-loader"
      })
    },
    {
      test: /\.scss$/,
      use: ExtractTextPlugin.extract({
        fallback: "style-loader",
        use: ['css-loader', 'sass-loader']
      })
    },
  ]
},
plugins: [
  // 通过插件抽离 css (参数)
  new ExtractTextPlugin("css/style.css"),
  // 抽离css 的辅助压缩插件
  new OptimizeCssAssetsPlugin()
]
```

### 压缩HTML页面

- 详细的配置可以参考[html-minifier](https://github.com/kangax/html-minifier#options-quick-reference)

```js
new htmlWebpackPlugin({
  // 模板页面
  template: path.join(__dirname, './index.html'),

  // 压缩HTML
  minify: {
    // 移除空白
    collapseWhitespace: true,
    // 移除注释
    removeComments: true,
    // 移除属性中的双引号
    removeAttributeQuotes: true
  }
}),
```

## vue配合webpack实现路由按需加载

- [Vue- 路由懒加载](https://router.vuejs.org/zh-cn/advanced/lazy-loading.html)
- [Vue 异步组件](https://cn.vuejs.org/v2/guide/components.html#异步组件)
- [VUE2 组件懒加载浅析](http://www.cnblogs.com/zhanyishu/p/6587571.html)
- [Vue.js路由懒加载[译]](http://www.jianshu.com/p/abb02075b56b)

### 使用步骤

- 1 修改组件的引用方式

```js
// 方式一: require.ensure()
const NewsList = r => require.ensure([], () => r(require('../components/news/newslist.vue')), 'news')
// const NewsInfo = r => require.ensure([], () => r(require('../components/news/newsinfo.vue')), 'news')

// 方式二: import() -- 推荐
// 注意：/* webpackChunkName: "newsinfo" */ 是一个特殊的语法，表示生成js文件的名称
const NewsInfo = () => import(/* webpackChunkName: "newsinfo" */ '../components/news/newsinfo.vue')
```

- 2 修改 webpack 配置文件的output

```js
output: {
  // ------添加 chunkFilename, 指定输出js文件的名称------
  chunkFilename: 'js/[name].[chunkhash].js',
},
```
