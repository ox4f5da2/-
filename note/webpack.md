### 什么是Webpack ?

- 什么是webpack ?官方的解释:

  - At its core, webpack is a static module bundler for modern JavaScript applications.

  > 从本质上来讲, webpack是一个现代的JavaScript应用的静态模快打包工具。即模块和打包



### 前端模块化

- 目前使用前端模块化的一些方案: AMD、CMD、CommonJS、ES6。

- 在ES6之前,我们要想进行模块化开发,就必须借助于其他的工具,让我们可以进行模块化开发。
- 并且在通过模块化开发完成了项目后,还需要处理模块间的各种依赖,并且将其进行整合打包。

- 而webpack其中一个核心就是让我们可能进行模块化开发 ,并且会帮助我们处理模块间的依赖关系。
- 而且不仅仅是JavaScript文件,我们的CSS、图片、json文件等等在webpack中都可以被当做模块来使用。
- 这就是webpack中模块化的概念。



### 打包

- 就是将webpack中的各种资源模块进行打包合并成一个或多个包(Bundle)。
- 并且在打包的过程中,还可以对资源进行处理,比如压缩图片,将scss转成css ,将ES6语法转成ES5语法,将TypeScript转成JavaScript等等操作。



### webpack和gulp有什么不同

- grunt/gulp更加强调的是前端流程的自动化,模块化不是它的核心。
- webpack更加强调模块化开发管理,而文件压缩合并、预处理等功能是他附带的功能。



### 安装webpack

1. 安装node
2. 输入`npm install webpack -g`  全局安装
3. 然后在项目文件中安装webpack: `npm install webpack@5.42.1-D`
4. 安装脚手架`npm install webpack-cli@4.9.0 -D`
5. 新建package.json: `npm init -y `
6. 新建src源代码目录,并新建index.html和index.js文件
7. 新建web pack.config.js: 

```js
const path = require('path');

module.exports = {
  mode: 'development',
  // 入口文件的路径
  entry: path.join(__dirname, './src/index.js'),
  // 导出的文件配置
  output: {
    // 路径
    path: path.join(__dirname, './dist'),
    // 名字
    filename: 'bundle.js'
  },
  devServer: {
    open: true
  }
}
```

8. 如果想每次实时自动打包则安装,就安装webpack-dev-server

`npm install webpack-dev-server@3.11.2 -D`

9. 在package.json文件中加入

```json
"scripts": {
    "dev": "webpack serve"
  },
```

10. 在终端输入npm run dev即可,当改变js文件的代码保存后即可自动打包

11. 访问时使用http://localhost:8080/
12. index.html文件中引用`<script src='/bundle.js'></script>`



### webpack-loader

#### css

1. 新建css文件夹,并新建index.css
2. 在index.js里导入 --> import '../css/index.css'
3. 接着在终端里运行`npm run dev`,得到报错

> - ERROR in ./css/index.css 1:3
> - Module parse failed: Unexpected token (1:3)
> - You may need an appropriate loader to handle this file type,currently no loaders are configured to process this file.See https://webpack.js.org/concepts#loaders

4. 执行`npm install style-loader@3.0.0 css-loader@5.2.6 -D`,安装处理css文件的loader
5. 在webpack.config.js的module -> rules数组中,添加loader规则:

```json
// test表示匹配的文件类型,use表示对应要调用的loader
module: {
  rules: [
    {test: /\.css$/, use: ['style-loader', 'css-loader']}
  ]
}
```



#### 图片

1. 新建图片文件夹并放入图片
2. 在index.html文件中写入`<img src="" alt="" class="box">`
3. 在index.js文件中导入并使用

```
import logo from '../images/WechatIMG2.jpg'

$('.box').attr('src', logo)
```

4. 运行`npm install url-loader@4.1.1 file-loader@6.2.0 -D`
5. 在webpack.config.js的module -> rules数组中,添加loader规则:

```json
// limit用来限定图片的大小, 单位字节(byte),如果小于这个阈值,图片会转成base64,否则还是保留图片的链接
module: {
  rules: [
    {test: /\.jpg|png|gif$/, use: ['url-loader?limit=22229']}
  ]
}
```



#### 打包

1. 在package.json文件的scripts节点下,新增build命令:

```json
"scripts": {
    "dev": "webpack serve",
    "build": "webpack --mode production"
  },
```

2. 运行`npm run build`



#### 优化图片和js文件

##### 图片

webpack.config.js中修改rules的use参数

```json
module: {
    rules: [
      ......
      {..., use: ['url-loader?limit=22229&outputPath=images']}
    ]
  }
```



##### js文件

webpack.config.js中修改output的filename参数

```json
output: {
    path: path.join(__dirname, './dist'),
    filename: 'js/bundle.js'
  },
```

然后修改index.html的`<script src='/js/bundle.js'></script>`

#### clean-webpack-plugin

作用: 自动删除dist文件中的额外文件后再重新生成新的

1. 运行`npm install --save--dev clean-webpack-plugin`或`npm install  clean-webpack-plugin -D`

2. 在webpack.config.js加入`const { CleanWebpackPlugin } = require('clean-webpack-plugin')`
3. 在module.exports加入plugins

```json
module.exports = {
  ......
  plugins: [
    new CleanWebpackPlugin()
  ]
}
```



### Source Map

**Source Map就是一个信息文件,里面储存着位置信息。**也就是说，Source Map文件中存储着压缩混淆后的代码，所对应的**转换前的位置**。出错的时候，除错工具将**直接显示原始代码，而不是转换后的代码**，能够极大的方便后期的调试。

开发式打开source map在webpack.config.js加入

```json
// eval-source-map仅限在开发模式中使用
module.exports = {
  ......
  devtool: 'source-map'
}
```



发布后让source map只定位报错的位置,但不暴露源码

```json
module.exports = {
  ......
  devtool: 'nosources-source-map'
}
```



