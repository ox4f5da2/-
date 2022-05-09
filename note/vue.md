[TOC]

## 1. ES6模块化

#### 1.1. 前端模块化规范的分类

在**ES6模块化规范**诞生之前， JavaScript社区已经尝试并提出了**AMD、CMD、CommonJS**等模块化规范。但是，这些由社区提出的模块化标准，还是存在一定的**差异性**与**局限性**、**并不是**浏览器与服务器**通用的模块化标准**，例如：

- AMD和CMD适用于**浏览器端**的Javascript模块化
- CommonJS 适用于**服务器端**的Javascript模块化

太多的模块化规范给开发者增加了**学习的难度**与**开发的成本**。因此，**大一统的ES6**模块化规范诞生了!



#### 1.2. 什么是ES6模块化规范

**ES6模块化规范是浏览器端与服务器端**通用的模块化开发规范。它的出现极大的降低了前端开发者的模块化学习成本，开发者不需再额外学习AMD、CMD或CommonJS等模块化规范。

ES6模块化规范中定义:

- 每个js 文件都是一个独立的模块

- 导入其它模块成员使用**import**关键字

- 向外共享模块成员使用**export**关键字



#### 1.3. 在node.js中体验ES6模块化

node.js中**默认仅支持CommonJS模块化规范**，若想基于node.js体验与学习ES6的模块化语法，可以按照如下两个步骤进行配置: 

1. 确保安装了`v14.15.1`或更高版本的node.js，

2. 在package.json的根节点中添加`"type": "module"`节点

> 快速新建package.json文件: 在终端中输入`npm init -y`



#### 1.4. ES6模块化的基本语法

ES6的模块化主要包含如下3种用法: 

##### 1.4.1 默认导出与默认导入

###### 1.4.1.1 默认导出

语法: **export default**默认导出的成员

```js
//定义模块私有成员n1
let n1=10
//定义模块私有成员n2 (外界访问不到n2，因为它没有被共享出去)
let n2 = 20 
//定义模块私有方法show
function show() {} 
//使用export default 默认导出语法，向外共享n1和show 两个成员
export default{ 
	n1,
	show
}
```

###### 1.4.1.2 默认导入

语法: **import 接收名称from '模块标识符'**

```js
//从01_ m1.js模块中导入export default 向外共享的成员
//并使用m1成员进行接收
import m1 from './01 m1.js'
//打印输出的结果为:
// { n1: 10，show: [Function: show] }
console.log(m1)
```



###### 1.4.1.3 按需导出与按需导入的注意事项

每个模块中，**只允许使用唯一的一次**export default，否则会报错!



##### 1.4.2 按需导出与按需导入

###### 1.4.2.1 按需导出

语法: **export按需导出的成员**

```js
//当前模块为03_ m2.js
//向外按需导出变量s1
export let s1 = 'aaa' 
//向外按需导出变量s2
export let s2 = 'ccc'
//向外按需导出方法say
export function say() {}

export default {
  a: 20
}
```



###### 1.4.2.2 按需导出

语法: **import{ s1 } from '模块标识符'**

```js
//导入模块成员
import info, {s1，s2 as str2，say} from './03 m2.js'

console.log(s1) // 打印输出aaa
console.log(s2) // 打印输出ccc
console.log(say) //打印输出[Function: say]
```



###### 1.4.2.3 按需导出与按需导入的注意事项

1. 每个模块中可以使用**多次**按需导出

2. 按需**导入的成员名称**必须和**按需导出的名称**保持一致

3. 按需导入时，可以使用**as关键字**进行重命名

4. 按需导入可以和默认导入一起使用



##### 1.4.3 直接导入并执行模块中的代码

如果只想单纯地执行某个模块中的代码，并不需要得到模块中向外共享的成员。此

时，可以直接导入并执行模块代码，示例代码如下:

```js
//当前文件模块为05_ m3.js
//在当前模块中执行一个for 循环操作
for(leti=0;i<3;i++){
	console.log(i)
}

// -------------------分割线---------------------

//直接导入并执行模块代码，不需要得到模块向外共享的成员
import './05_ m3.js'
```



## 2. Promise

#### 2.1 回调地狱

**多层回调函数的相互嵌套**，就形成了**回调地狱**。

```js
setTimeout(() => { //第1层回调函数
	console.log( '延时1秒后输出')
	setTimeout(() => { //第2层回调函数
		console.log( '再延时2秒后输出')
		setTimeout(() => { //第3层回调函数
			console.log('再延时3秒后输出')
		}，3000)
	}，2000)
}，1000)
```

> 回调地狱的缺点:
>
> - 代码耦合性太强，牵一发而动全身，**难以维护**
>
> - 大量冗余的代码相互嵌套，代码的**可读性变差**



#### 2.2 Promise的基本概念

1. **Promise 是一个构造函数**

   - 我们可 以创建Promise的实例`const p = new Promise()`

   - new 出来的Promise实例对象，**代表一个异步操作**

2. **Promise.prototype上包含一个.then()方法**

   - 每一次 new Promise()构造函数得到的实例对象，

   - 都可以通过**原型链**的方式访问到.then() 方法，例如`p.then()`

3. **.then() 方法用来预先指定成功和失败的回调函数**

   - p.then(成功的回调函数，失败的回调函数)

   - p.then(result=>{}, error=>{})

   - 调用 .then()方法时，**成功**的回调函数是**必选的**、失败的回调函数是可选的



#### 2.3 then-fs的基本使用

调用then-fs提供的**readFile()**方法，可以异步地读取文件的内容，它的**返回值是**

**Promise的实例对象**。因此**可以调用.then() 方法**为每个Promise异步操作指定**成**

**功和失败**之后的回调函数。示例代码如下:

```js
// 基于Promise 的方式读取文件
import thenFs from 'then-fs'
//注意: .then()中的失败回调是可选的，可以被省略
thenFs.readFile('./files/1.txt'，'utf8').then(r1 => { console.log(r1) }，err1 => { console.log(err1.message) })
7 thenFs.readFile('./files/2.txt'，'utf8').then(r2 => { console.log(r2) }，err2 => { console.log(err2.message) })
8 thenFs.readFile('./files/3.txt'，'utf8'). then(r3 => { console.log(r3) }，err3 => { console.log(err3.message) })
```



#### 2.4 基于Promise按顺序读取文件的内容

Promise支持链式调用从而来解决回调地狱的问题。示例代码如下:

```js
// 1.返回值是Promise的实例对象
thenFs.readFile('./files/1.txt'，'utf8') 
	// 2.通过.then 为第一个Promise 实例指定成功之后的回调函数
	.then((r1) => { 
		console.log(r1)
		// 3.在第一个.then 中返回一个新的Promise 实例对象
		return thenFs. readFile('./files/2.txt'，'utf8') 
})
	// 4.继续调用.then，为上一个.then的返回值(新的Promise 实例)指定成功之后的回调函数
	.then((r2) => {
		console.1og(r2)
  	// 5.在第二个.then 中再返回一个新的Promise 实例对象
		return thenFs. readFile(' ./files/3.txt'，'utf8') 
})
	// 6.继续调用.then，为上一个.then 的返回值(新的Promise 实例)指定成功之后的回调函数
	.then((r3) => { 
	console.log(r3)
})
```



#### 2.5 通过.catch捕获错误

在Promise的链式操作中如果发生了错误，可以使用Promise.prototype**.catch**方法进

行捕获和处理:

```js
thenFs.readFile('./files/11.txt'，'utf8') 
.then(r1 => {...})
.catch(err => {
  console.log(err.message)
})
```



#### 2.6 Promise.all()方法

Promise.all()方法会发起并行的Promise异步操作，等所有的异步操作全部结束后才会

执行下一步的.then操作( 等待机制)。示例代码如下:

```js
// 1.定义一个数组，存放3个读文件的异步操作
const promiseArr = [
	thenFs.readFile('./files/11.txt'，'utf8'),
	thenFs.readFile('./files/2.txt'，'utf8'),
	thenFs.readFile('./files/3.txt'，'utf8'),
]
// 2.将Promise 的数组，作为Promise.all() 的参数
Promise . all(promiseArr )
	//2.1所有文件读取成功(等待机制)
	.then(([r1,r2,r3])=>{
	console.log(r1，r2，r3)
}
// 2.2捕获Promise 异步操作中的错误
.catch(err => { 
	console.log(err.message)
})
```

> 注意:数组中Promise实例的顺序，就是最终结果的顺序!



## 3. async / await

#### 3.1 async/await的基本使用

使用async/await简化Promise异步操作的示例代码如下: 

```js
import thenFs from 'then-fs'
// 按照顺序读取文件1，2，3的内容
async function getAllFile() {
	const r1 = await thenFs.readFile('./files/1.txt'，'utf8')
	console.log(r1)
	const r2 = await thenFs.readFile('./files/2.txt'，'utf8')
	console.log(r2)
	const r3 = await thenFs.readFile('./files/3.txt'，'utf8')
	console. log(r3)
}

getAllFile()
```



#### 3.2 async/await的使用注意事项

1. 如果在function中使用了await，则function必须被async修饰

2. 在async方法中，第一个await之前的代码会同步执行，await 之后的代码会异步



## 4. EventLoop

#### 4.1 同步任务和异步任务

为了防止某个**耗时任务**导致**程序假死**的问题， JavaScript把待执行的任务分为了两类:

1. **同步任务**(synchronous)

   - 又叫做**非耗时任务**，指的是在主线程.上排队执行的那些任务

   - 只有前一个任务执行完毕,才能执行后一个任务

2. **异步任务**( asynchronous )

   - 又叫做**耗时任务**，异步任务由JavaScript**委托给**宿主环境进行执行

   - 当异步任务执行完成后，会**通知JavaScript主线程**执行异步任务的**回调函数**



#### 4.2 同步任务和异步任务的执行过程

1. 同步任务由JavaScript主线程次序执行
2. 异步任务**委托给**宿主环境执行
3. 已完成的异步任务**对应的回调函数**，会被加入到任务队列中等待执行
4. JavaScript 主线程的**执行栈**被清空后，会读取任务队列中的回调函数，次序执行
5. **JavaScript 主线程不断重复上面的第4步**



#### 4.3 EventLoop的基本概念

**JavaScript主线程从“任务队列”中读取异步任务的回调函数,放到执行栈中依次执行。**

这个过程是循环不断的，所以整个的这种运行机制又称为**EventLoop** ( 事件循环)。



#### 4.4 宏任务与微任务

##### 4.4.1 什么是宏任务和微任务

JavaScript把异步任务又做了进一步的划分，异步任务又分为两类,分别是:

1. 宏任务(macrotask)
   - 异步 Ajax请求
   - setTimeout、 setInterval
   - 文件操作
   - 其它宏任务
2. 微任务(microtask)
   - Promise.then、 .catch 和.finally
   - process.nextTick
   - 其它微任务



##### 4.4.2 宏任务和微任务的执行顺序

每一个宏任务执行完之后，都会检查**是否存在待执行的微任务**，如果有，则执行完所有微

任务之后，再继续执行下一个宏任务。



## 5. webpack

#### 5.1 什么是Webpack ?

- 什么是webpack ?官方的解释:

  - At its core，webpack is a static module bundler for modern JavaScript applications.

  > 从本质上来讲，webpack是一个现代的JavaScript应用的静态模快打包工具。即模块和打包



#### 5.2 前端模块化

- 目前使用前端模块化的一些方案: AMD、CMD、CommonJS、ES6。

- 在ES6之前,我们要想进行模块化开发，就必须借助于其他的工具，让我们可以进行模块化开发。
- 并且在通过模块化开发完成了项目后，还需要处理模块间的各种依赖，并且将其进行整合打包。

- 而webpack其中一个核心就是让我们可能进行模块化开发，并且会帮助我们处理模块间的依赖关系。
- 而且不仅仅是JavaScript文件，我们的CSS、图片、json文件等等在webpack中都可以被当做模块来使用。
- 这就是webpack中模块化的概念。



#### 5.3 打包

- 就是将webpack中的各种资源模块进行打包合并成一个或多个包(Bundle)。
- 并且在打包的过程中，还可以对资源进行处理，比如压缩图片，将scss转成css ，将ES6语法转成ES5语法，将TypeScript转成JavaScript等等操作。



#### 5.4 webpack和gulp有什么不同

- grunt/gulp更加强调的是前端流程的自动化，模块化不是它的核心。
- webpack更加强调模块化开发管理，而文件压缩合并、预处理等功能是他附带的功能。



#### 5.5 安装webpack

1. 安装node
2. 输入`npm install webpack -g`  全局安装
3. 然后在项目文件中安装webpack: `npm install webpack@5.42.1-D`
4. 安装脚手架`npm install webpack-cli@4.9.0 -D`
5. 新建package.json: `npm init -y `
6. 新建src源代码目录，并新建index.html和index.js文件
7. 新建web pack.config.js: 

```js
const path = require('path');

module.exports = {
  mode: 'development',
  // 入口文件的路径
  entry: path.join(__dirname，'./src/index.js'),
  // 导出的文件配置
  output: {
    // 路径
    path: path.join(__dirname，'./dist'),
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



#### 5.6 webpack-loader

##### 5.6.1 css

1. 新建css文件夹,并新建index.css
2. 在index.js里导入 --> import '../css/index.css'
3. 接着在终端里运行`npm run dev`，得到报错

> - ERROR in ./css/index.css 1:3
> - Module parse failed: Unexpected token (1:3)
> - You may need an appropriate loader to handle this file type,currently no loaders are configured to process this file.See https://webpack.js.org/concepts#loaders

4. 执行`npm install style-loader@3.0.0 css-loader@5.2.6 -D`，安装处理css文件的loader
5. 在webpack.config.js的module -> rules数组中，添加loader规则:

```json
// test表示匹配的文件类型,use表示对应要调用的loader
module: {
  rules: [
    {test: /\.css$/，use: ['style-loader'，'css-loader']}
  ]
}
```



##### 5.6.2 图片

1. 新建图片文件夹并放入图片
2. 在index.html文件中写入`<img src="" alt="" class="box">`
3. 在index.js文件中导入并使用

```
import logo from '../images/WechatIMG2.jpg'

$('.box').attr('src', logo)
```

4. 运行`npm install url-loader@4.1.1 file-loader@6.2.0 -D`
5. 在webpack.config.js的module -> rules数组中，添加loader规则:

```json
// limit用来限定图片的大小，单位字节(byte)，如果小于这个阈值，图片会转成base64，否则还是保留图片的链接
module: {
  rules: [
    {test: /\.jpg|png|gif$/，use: ['url-loader?limit=22229']}
  ]
}
```



#### 5.7 打包

1. 在package.json文件的scripts节点下，新增build命令:

```json
"scripts": {
    "dev": "webpack serve",
    "build": "webpack --mode production"
  },
```

2. 运行`npm run build`



#### 5.8 优化图片和js文件

##### 5.8.1 图片

webpack.config.js中修改rules的use参数

```json
module: {
    rules: [
      ......
      {...，use: ['url-loader?limit=22229&outputPath=images']}
    ]
  }
```



##### 5.8.2 js文件

webpack.config.js中修改output的filename参数

```json
output: {
    path: path.join(__dirname，'./dist'),
    filename: 'js/bundle.js'
  },
```

然后修改index.html的`<script src='/js/bundle.js'></script>`



#### 5.9 clean-webpack-plugin

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



#### 5.10 Source Map

**Source Map就是一个信息文件,里面储存着位置信息。**也就是说，Source Map文件中存储着压缩混淆后的代码，所对应的**转换前的位置**。出错的时候，除错工具将**直接显示原始代码，而不是转换后的代码**，能够极大的方便后期的调试。

开发式打开source map在webpack.config.js加入

```json
// eval-source-map仅限在开发模式中使用
module.exports = {
  ......
  devtool: 'source-map'
}
```



发布后让source map只定位报错的位置，但不暴露源码

```json
module.exports = {
  ......
  devtool: 'nosources-source-map'
}
```





## 6. 安装Vue Devtools

1. 访问[Vue插件——Vue Devtools](https://chrome.zzzmh.cn/info?token=nhdogjmejiglipccpnnnanhbledajbpd)或者[GitHub网页](https://github.com/vuejs/devtools#vue-devtools)下载
1. 点击Chrome浏览器的右上角**三个点**——**更多工具**——**扩展程序**
2. 将.crx文件拖入Chrome的扩展程序中
3. 点击插件的详情按钮，将“允许访问文件网址”的开关打开



## 7. 什么是vue

1. 构建用户界面
   - 用vue往html页面中填充数据，非常的方便
2. 框架
   - 框架是一套现成的解决方案,程序员只能遵守框架的规范，去编写自己的业务功能!
   - 要学习vue，就是在学习vue框架中规定的用法!
   - vue的指令、组件(是对UI结构的复用)、路由、Vuex、vue组件库
3. vue的版本
   当前，vue共有3个大版本，其中:
   - **2.x版本的vue是目前企业级项目开发中的主流版本**
   - 3.x版本的vue于2020-09-19发布，生态还不完善，尚未在企业级项目开发中普及和推广
   - 1.x版本的vue几乎被淘汰，不再建议学习与使用
4. 总结:
   - **3.x版本的vue是未来企业级项目开发的趋势;**
   - 2.x版本的vue在未来(1 ~ 2年内)会被逐渐淘汰;



## 8. vue的两个特性

1. 数据驱动视图:
   - 数据的变化**会驱动视图**自动更新
   - 好处: 程序员只管把数据维护好，那么页面结构会被vue自动渲染出来!
   - 注意：数据驱动视图是**单向的数据绑定**
   
2. 双向数据绑定:
   - 在网页中，form 表单负责**采集数据**，Ajax 负责**提交数据**。
   - js数据的变化，会被自动渲染到页面上
   - 页面上表单采集的数据发生变化的时候，会被vue自动获取到，并更新到js 数据中
   - 好处：开发者不再需要手动操作DOM元素来获取表单最新的值



## 9. MVVM

**MVVM**是vue实现**数据驱动视图**和**双向数据绑定**的核心原理。MVVM指的是**M**odel、 **V**iew和**V**iew**M**odel，它把每个HTML页面都拆分成了这三个部分。

> - **Model**表示当前页面渲染时所依赖的数据源。
> - **View**表示当前页面所渲染的DOM结构。
> - **ViewModel**表示vue的实例，它是MVVM的核心。

具体可参见下图: 

![MVVM](https://cn.vuejs.org/images/mvvm.png?_=5619070)

## 10. Vue2 / Vue3的使用

1. 导入vue.js的script脚本文件
2. 在页面中声明一个将要被vue所控制的DOM区域
3. 创建vm实例对象(vue 实例对象)

#### 10.1 导入方式一

- 访问[官方安装](https://cn.vuejs.org/v2/guide/installation.html)页面，自行下载安装
- 或者点击下载开发版本[vue.js](https://cn.vuejs.org/js/vue.js)或者生产版本[vue.min.js](https://cn.vuejs.org/js/vue.min.js)文件，并导入该文件

```html
<!-- Vue2 -->
<div id="app">{{msg}}</div>
<script src="./vue.js"></script>
<script>
  const vm = new Vue({
    el: '#app',
    data: {
      msg: 'hello world!'
    }
  })
</script>
```



#### 10.2 导入方式二

```html
<!-- Vue2 -->
<!-- 开发环境版本，包含了有帮助的命令行警告 -->
<script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>

<!-- 生产环境版本，优化了尺寸和速度 -->
<script src="https://cdn.jsdelivr.net/npm/vue@2"></script>

<!-- Vue3 -->
<script src="https://unpkg.com/vue@next"></script>
```



#### 10.3 导入方式三

在用 Vue 构建大型应用时推荐使用 NPM 安装。NPM 能很好地和诸如 [webpack](https://webpack.js.org/) 或 [Browserify](http://browserify.org/) 模块打包器配合使用。同时 Vue 也提供配套工具来开发单文件组件。

```
# vue2最新稳定版
$ npm install vue

# vue3最新稳定版
$ npm install vue@next
```





## 11. Vue的指令

#### 11.1 {{ }} 插值语法

`{{}}`:  数据绑定，既可以绑定基本数据类型，也可绑定对象、数组等类型; 同时也可以进行变量相加运算或者三目运算等

```html
<body>
   <div id="root">
    <!-- 视图层 v-->
    插值表达式：渲染数据，基本运算，三元运算
    {{msg}}{{numa+numb}}{{flag ? '真的':'假'}}
    <h1>
        {{newObj.name}}是一个{{newObj.age}}的{{newObj.job}}
    </h1>
    <h1>
        {{newArry[0]}}{{newArry[1]}}{{newArry[2]}}
    </h1>
    <div>
        <input type="text" v-model="value">
        {{value}}
    </div>
    <h1>{{msg}}</h1>
  </div>
  <!-- {{msg}} -->

  <script>
      var vm = new Vue({//创建了一个vue的实例 vm
          el:"#root",//定位
          data:{//数据源M
              msg:"hello world!",
              numa:1,
              numb:'2',
              flag:false,
              newObj:{
                  name:'张三',
                  age:58,
                  job:'程序员'
              },
              newArry:['hello'，'vue'，2],
              value:''
          }
      })
  </script>
</body>
```



#### 11.2 v-text

`v-text`: 会将原来的值覆盖，并设置文本值，相当于innerText

```html
<body>
    <div id="root">
        <!-- v-text：设置标签文本值 -->
        <h1>{{msg}}</h1>
        <!-- 可以将data的数据渲染在页面上 -->
        <h1 v-text="msg"></h1>
        <!-- 可以进行JavaScript逻辑运算 -->
        <h1 v-text="msg+'111'"></h1>
        <!-- 三目运算 -->
        <h1 v-text="1>2 ?'大于':'小于'"></h1>
        <!-- 字符串拼接 -->
        <h1 v-text="msg+msg"></h1>

        <h1>{{msg}}哈哈哈</h1>
        <h1 v-text="msg">哈哈哈</h1>
        <!-- <h1 v-text="ha">哈哈哈</h1> -->
    </div>

    <script>
        var vm = new Vue({//创建了一个vue的实例 vm
            el:"#root",//定位
            data:{//数据源M
                msg:"hello world!"
            }
        })
    </script>
</body>
```



#### 11.3 v-html

`v-html`: 会将原来的值覆盖，将字符串转为HTML

```html
<body>
    <div id="root">
        <!-- v-html 把字符串当作html渲染，类似与innerHTML -->
        <div v-html="msg"></div>
        <div v-cloak>{{msg}}</div>
        <div v-text="msg"></div>
        <!-- 字符串拼接 -->
        <div v-html="msg+'123456'"></div>
        <!-- 三目运算 -->
        <div v-html="3>2?msg:'1才不大于2'"></div>
        <div v-html="msg">哈哈哈</div>

        <!-- 
            3种方式可以去渲染数据：
            1：{{}}  加载的时候会出现回闪，解决方法使用v-cloak（会出现{{msg}}）
            2.v-text  会将中间的数据覆盖掉
            3.v-html   会将中间的数据覆盖掉
         -->
    </div>

    <script>
        var vm = new Vue({//创建了一个vue的实例 vm
            el:"#root",//定位
            data:{//数据源M
                msg:"<h1>hello world!</h1>"
            }
        })
    </script>
</body>
```



#### 11.4 v-pre

`v-pre`: 不编译，跳过绑定，直接显示{{XXX}}

```html
<body>
    <div id="root">
       <div>{{msg}}</div>
       <!-- 不编译， -->
       <div v-pre>{{msg}}</div>
    </div>

    <script>
        var vm = new Vue({
            el:"#root",
            data:{
                msg:"<h1>hello world!</h1>"
            }
        })
    </script>
</body>
```



#### 11.5 v-once

`v-once`: 不随变量值的改变而改变，即只在最开始的时候渲染一次

```html
<body>
    <div id="root">
       <div>{{msg}}</div>
       <div v-once>{{msg}}</div>
    </div>

    <script>
        var vm = new Vue({
            el:"#root",
            data:{
                msg:"hello world!"
            }
        })
    </script>
</body>
```



#### 11.6 v-cloak

`v-cloak`: {{}} 加载的时候可以出现回闪，通常用v-clock解决，首先在标签上加上v-cloak，然后在style样式里加上`[v-cloak] {display: none}`代码块。

```html
<style>
  [v-cloak] {
    display: none;
  }
</style>
<body>
	<h1 v-cloak>{{msg}}</h1>
  <script>
      var vm = new Vue({
          el:"#root",
          data:{
              msg:"hello world!"
          }
      })
  </script>
</body>
```



#### 11.7 v-model（双向绑定）

`v-model`: 

- 双向绑定 v-model=“XXX” {{XXX}}
- v-model="hobbies" / hobbies: []
- V-model.lazy: 在敲回车和失去焦点的时候才进行绑定
- v-model.number: 使字符串转为数字
- v-model.trim: 去除前后空格

> v-model指令一般用于**表单元素**结合使用才有意义

| 修饰符  | 描述                               |
| :------ | ---------------------------------- |
| .lazy   | 在敲回车和失去焦点的时候才进行绑定 |
| .number | 使字符串转为数字                   |
| .trim   | 去除前后空格                       |

```html
<body>
    <div id="root">
        <div>
            <input type="text" v-model="value">
            {{value}}
        </div>

        <input type="checkbox" v-model="hobby">爱好
        {{hobby}}
        <!-- 值为true 或者 false -->

        <!-- 做一个点单 -->
        <div>
            <input type="checkbox" v-model="diandan" value="炸鸡">炸鸡
            <input type="checkbox" v-model="diandan" value="烤鸭">烤鸭
            <input type="checkbox" v-model="diandan" value="纸包鱼">纸包鱼
            <input type="checkbox" v-model="diandan" value="蒜香花甲">蒜香花甲
            <input type="checkbox" v-model="diandan" value="烤鱼">烤鱼
            {{diandan}}
        </div>

        <!-- v-model修饰符 -->
        <div>
            <!-- 没有加修饰符 -->
            <input type="text" v-model="value">
            {{value}}
            <!-- 加修饰符 .lazy-->
            <input type="text" v-model.lazy="value">
            {{value}}

            <!-- 加修饰符 .number-->
            <input type="text" v-model.number="num">
            {{num}}

            <!-- 加修饰符 .trim-->
            <!-- 去头去尾空格号 -->
            <input type="text" v-model.trim="value">
            {{value}}

            <!-- 结合加修饰符 -->
            <input type="text" v-model.number.lazy="num">
            {{num}}
        </div>

    </div>

    <script>
        var vm = new Vue({
            el:"#root",
            data:{
                value:'',
                hobby:'',
                diandan:[],
                num:0
            }
        })
    </script>
</body>
```



#### 11.8 v-show / v-if / v-else

`v-if/v-else`: 会把节点删除，**只切换一次**，大部分情况用这个

`v-show`: 不会删除节点，只是多加了display:none，在隐藏与显示**频繁切换**

- 如果要频繁的切换元索的显示状态，用v-show性能会更好

- 如果刚进入页面的时候，某些元素默认不需要被展示，而且后期这个元素很可能也不需要被展示出来，此时v-if性能更好

```html
<body>
    <div id="root">
        <!-- js假值：false,0,'',nan,null,undefined -->
        <div v-show="false">{{msg}}</div>
        <div v-if="null">{{msg}}</div>
        <!-- 
            相同点：都能隐藏元素
            不同点：v-show是切换元素display   css属性，参加页面渲染
            v-if是节点重新渲染或者销毁
         -->

        <div>
            <div>
                <button @click="imgChange">切换图片</button>
            </div>
            <img v-show="change" src="#">
            <img v-show="!change" src="#">
        </div>

        <div>
            <h1 v-if="age<18">未成年</h1>
            <h1 v-else-if="age<30">青年</h1>
            <h1 v-else-if="age<60">正值壮年</h1>
            <h1 v-else>老年</h1>
        </div>

    </div>

    <script>
        var vm = new Vue({
            el: "#root",
            data: {
                msg: '我又出现了',
                change: true,
                age:203
            },
            methods: {
                imgChange() {
                    this.change = !this.change
                }
            },
        })
    </script>
</body>
```



#### 11.9 v-for

`v-for`: 

- 循环遍历数组 `"(item, index) in XXX"`
- 遍历对象`"(value, key) in XXX"`

```html
<body>
    <div id="root">
        <!-- 遍历数组 -->
        <ul>
            <li v-for="(item,index) in msg">{{index+1}}-{{item}}</li>
        </ul>
        <!-- 遍历对象 -->
        <ul>
            <li v-for="(item,index) in user">{{index}}-{{item}}</li>
        </ul>
        <li v-for="u in user.name">{{u}}</li>
    </div>

    <script>
        var vm = new Vue({
            el: "#root",
            data: {
                msg: ["炸鸡"，"烤鸭"，"小黄鱼"，"烧烤"],
                user: {
                    name: '张三',
                    age: 18,
                    hobby: "打官司"
                }
            }
        })
    </script>
</body>
```

##### 11.9.1 v-for使用时的建议

1. 官方建议：只要用到了v-for指令，那么**一定**要绑定一个`:key`属性

2. 而且，尽量把id作为key的值，不要用index作为key

3. 官方对key的值类型，是有要求的:字符串或数字类型

4. key的值是千万不能重复的，否则会终端报错: Duplicate keys detected*(重复的键被检测到)*

##### 11.9.2 key的注意事项

1. key 的值只能是**字符串**或**数字**类型
2. key 的值**必须具有唯一性**(即: key 的值不能重复)
3. 建议把**数据项id属性的值**作为**key**的值( 因为id属性的值具有唯一性 )
4. 使用**index的值**当作key的值**没有任何意义**( 因为index的值不具有唯一性 )
5. 建议使用v-for指令时**一定要指定key的值**(既提升性能、又防止列表状态紊乱)



#### 11.10 v-bind

`v-bind`: 

- 属性绑定，可以简写为`:src="XXX"`相当于`v-bind:src="XXX"`
- 动态绑定class—— :class{类名1: 布尔值,类名2: 布尔值}
- 然后v-on:click="fn"，在fn里改变布尔值
- :class='getClasses()',getClasses: function(){return {...}}
- 绑定样式——:style="{fontsize: size+'px'}"
- 在使用v-bind属性绑定期间，如果绑定内容需要进行动态拼接,则字符串的外面应该包裹单引号

```html
<head>
   ......
    <style>
        .activeA{
            color: rgb(231，68，68);
        }
        .activeB{
            font-size: 50px;
        }
        .activeC{
            color: rgb(231，68，68);
            font-size: 50px;
        }
    </style>
</head>

<body>
    <div id="root">
        <div class="ha" :class="{'activeA':isA,'activeB':isB}">{{msg}}</div>

        <div :style="{fontSize: fontSize, color: fontColor}">
            {{msg}}
        </div>
    </div>

    <script>
        var vm = new Vue({
            el: "#root",
            data: {
               msg:"真是知识点",
               isA:true,
               isB:false,
               isC:"activeC",
               fontSize: '50px',
               fontColor: 'red'
            }
        })
    </script>
</body>
```



#### 11.11 v-on

`v-on`: 

- 绑定事件v-on:click="" 
- 语法糖：`@click=""`
- @click.stop: 阻止冒泡e.stopPropagation()
- @click.prevent: 阻止默认事件 e.preventDefault()
- @keyup.enter: 输入回车
- @click.native: 组件监听
- @click.once: 点击回调只触发一次
- 在绑定事件处理函数的时候，可以使用() 传递参数，如果要传event则需要传递$event，如果只传入event时函数的参数可以不写

##### 11.11.1 v-on基础用法

```html
<body>
    <div id="root">
        你的颜值分数：{{count}}
        <button @click="add">加分</button>
        <button @click="minus">减分</button>
    </div>

    <script>
        var vm = new Vue({
            el: "#root",
            data: {
                count:100
            },
            methods: {
                add(){
                    this.count++;
                },
                minus(){
                    if(this.count <= 60){
                        alert('不能再减了，我只有内在美了')
                        return
                    }
                    this.count--;
                }
            },
        })
    </script>
</body>
```

```html
<!-- 传入event参数的实例 -->
<body>
  <div id="root">
    <div>值：{{ count }}</div>
    <button @click="add($event)">+1</button>
    <!-- <button @click="add">+1</button> -->
    <button @click="sub($event, 2)">-1</button>
  </div>

  <script>
    let vm = new new Vue({
      el: "#root",
      data: {
        count: 0
      },
      methods: {
        add() {
          console.log(e); // e可以省略，传入的函数参数也可以省略，也可以像注释部分那样写
          this.count++;
        },
        sub(e, step) {
          console.log(e); // e不可以省略
          this.count -= step;
        }
      }
    })
  </script>
</body>
```



##### 11.11.2 v-on事件修饰符

事件修饰符表格：

| 修饰符       | 描述                                          |
| ------------ | --------------------------------------------- |
| **.stop**    | 阻止事件冒泡（调用 event.stopPropagation()）  |
| **.prevent** | 阻止默认事件（调用 event.preventDefault()。） |
| .capture     | 添加事件捕获                                  |
| .self        | 只能由自身触发事件                            |
| .once        | 该事件只会触发一次                            |

```html
<body>
    <div id="root">
        <!-- .stop -->
        <div @click="stopWai">
            <div @click.stop="stopNei">哈哈</div>
        </div>

        <!-- .prevent -->
        <a href="http://www.baidu.com">去百度</a>
        <a href.prevent="http://www.baidu.com">你哪那也去不了</a>

        <!-- .capture -->
        <div @click="stopWai">
            <div @click.capture="stopNei">
                <div @click="stopChange">哈哈哈</div>
            </div>
        </div>

        <!-- .self -->
        <div @click="stopWai">
            外面
            <div @click.self="stopNei">
                中间
                <div @click="stopChange">里面</div>
            </div>
        </div>

        <!-- .once -->
        <button @click.once="stopChange">点击</button>
    </div>

        <!-- native   .passive -->

    <script>
        var vm = new Vue({
            el: "#root",
            data: {
                msg:'我出现了'
            },
            methods: {
                stopWai(){
                    console.log('我是外部出现的');
                },
                stopNei(){
                    console.log('我是内部出现的');
                },
                stopChange(){
                    console.log('中心出现的');
                }
            },
        })
    </script>
</body>
```



##### 11.11.3 v-on按键修饰符

```html
<body>
    <div id="root">
        <!-- 键盘按键 = 键值 -->
        <input v-on:keyup.13='submit'>
        <input v-on:keyup.enter='submit'>
    </div>

    <script>
        var vm = new Vue({
            el: "#root",
            data: {
                msg:'我出现了'
            },
            methods: {
                submit(){
                    console.log('我被回车了')
                }
            }
        })
    </script>
</body>
```

按键修饰符表格：

| 修饰符  | 描述         |
| ------- | ------------ |
| .enter  | 回车         |
| .tab    | tab键        |
| .delete | 删除”和“退格 |
| .esc    | 退出         |
| .space  | 空格         |
| .up     | ↑            |
| .down   | ↓            |
| .left   | ←            |
| .right  | →            |



#### 11.12 自定义指令

##### 11.12.1 基本用法

- 全局: Vue.**directive**(名字, function(el, binding, vnode){...})
- 局部: **detectives:** {'名字': function(el, binding, vnode){...}}

```html
<body>
    <div id="root">
        <!-- 小写转大  全局-->
        <!-- v-text  v-html   {{}} -->
        <h1 v-upper*text='msgMax'></h1>
        <!-- 大转小   局部 -->
        <h1 v-lower-text='msgMin'></h1>
    </div>

    <script>
        //定义全局指令
        Vue.directive('upper*text',function(el,binding){
            console.log(el,binding)
            el.innerHTML=binding.value.toUpperCase()
        })


        var vm = new Vue({
            el: "#root",
            data: {
                msgMax:'i love you',
                msgMin:'HELLO WORLD'
            },
            //局部定义
            directives:{
                'lower-text':function(el,binding){
                el.innerHTML=binding.value.toLowerCase()
            }}
        })
    </script>
</body>
```



##### 11.12.2 钩子函数

| 函数             | 描述                                                         |
| ---------------- | ------------------------------------------------------------ |
| bind             | 只调用一次，指令第一次绑定到元素时调用。在这里可以进行一次性的初始化设置。 |
| inserted         | 被绑定元素插入父节点时调用 (仅保证父节点存在，但不一定已被插入文档中)。 |
| update           | 所在组件的 VNode 更新时调用，但是可能发生在其子 VNode 更新之前。指令的值可能发生了改变，也可能没有。但是你可以通过比较更新前后的值来忽略不必要的模板更新 。 |
| componentUpdated | 指令所在组件的 VNode 及其子 VNode 全部更新后调用。           |
| unbind           | 只调用一次，指令与元素解绑时调用。                           |



##### 11.12.3 钩子函数的参数

| 函数    | 描述                                                         |
| ------- | ------------------------------------------------------------ |
| el      | 指令所绑定的元素，可以用来直接操作 DOM。                     |
| binding | 一个对象，包含一下这些属性(name、value、oldValue、expression、arg、modifiers) |
| vnode   | vue 编译生成的虚拟节点。                                     |
| oldnode | 上一个虚拟节点，仅在 update 和 componentUpdated 钩子中可用。 |

> - 当指令第一次被绑定到元素上的时候，会立即触发bind 函数
> - 形参中的el表示当前指令所绑定到的那个DOM对象
> - 页面DOM更新时会调用update函数



##### 11.12.4 指令简写

```vue
<template>
  <div id="app">
    <h1 v-color="color">这是根组件</h1>
    <button @click="color='green'">改变文字颜色</button>
  </div>
</template>

directives: {
  color: {
    bind(el，binding){
      el.style.color = binding.value;
    },
    update(el，binding){
      el.style.color = binding.value;
    }
  }
	// 函数简写,当bind和update函数相同时
	color(el，binding){
    el.style.color = binding.value;
  }
}
```



#### 11.13 filter过滤器

**过滤器(Filters)** 是vue为开发者提供的功能，常用于文本的格式化。过滤器可以用在两个地方：**插值表达式**和**v-bind属性绑定**。过滤器应该被添加在JavaScript表达式的尾部，由“ 管道符"`|`进行调用。

具体用法和**指令detective**一样，分为局部过滤器和全局过滤器。局部过滤器是使用`filters:{}`，全局过滤器是使用`Vue.filter(...)`，当局部过滤器与全局过滤器重名冲突时，局部优先。

> filter只在vue2提供，vue3不支持filter操作。

##### 11.13.1 局部过滤器

```html
<body>
    <div id="root">
        <!-- ￥200  |管道符 -->
      <h1>苹果的价格：{{price | addPriceIcon}}</h1>
      <h1>香蕉的价格：{{price1 | addPriceIcon}}</h1>
      <h1>西瓜的价格：{{price2 | addPriceIcon}}</h1>
      <h1>葡萄的价格：{{price3 | addPriceIcon}}</h1>
      <h1>玉米的价格：{{price4 | addPriceIcon}}</h1>
    </div>

    <script>
        var vm = new Vue({
            el: "#root",
            data: {
                price:200,
                price1:3000,
                price2:3050,
                price3:3400,
                price4:6000,
            },
            // 局部定义
            filters:{
                // 过滤器函数形参中的value, 永远都是”｜“前面的那个值
                addPriceIcon(value){
                    console.log(value);
                    return '￥' + value + '元'
                }
            }
        })
    </script>
</body>
```



##### 11.13.2 全局过滤器

```html
<body>
    <div id="root">
       <h1>{{viewContent | addName}}</h1>
    </div>

    <script>
        // 定义全局
        Vue.filter("addName",(value)=>{
            return "今天的天气是：" + value
        })

        var vm = new Vue({
            el: "#root",
            data: {
                viewContent:'晴天'
            }
        })
    </script>
</body>
```



##### 11.13.3 过滤器实现首字母大写

```html
<body>
    <div id="root">
      <!-- 把输入的英文首字母大写  hello world -->
      <input type="text" v-model="content" @change="changeEvent">
      {{viewContent | changeContent}}
    </div>

    <script>
        var vm = new Vue({
            el: "#root",
            data: {
                content:'',
                viewContent:''
            },
            methods: {
                changeEvent(){
                    this.viewContent = this.content
                }
            },
            // 局部定义
            filters:{
                changeContent(value){
                    if(value){
                        let str = value.toString();
                        let newArr = str.split(" ").map(ele=>{
                            return ele.charAt(0).toUpperCase() + ele.slice(1)
                        });
                        return newArr.join(" ")
                    }
                }
            }
        })
    </script>
</body>
```

> 注意：vue3已经删除了filter但是Vue2还保留着。



#### 11.14 计算属性

`computed`: 计算属性

```json
data: {
  r: 0,
  g: 0,
  b: 0
}
computed: {
  rgb(){
    return `rgb(${this.r}，${this.g}，${this.b})`
  }
}
```

- 特点:
  1. 定义的时候，要被定义为**“方法"格式**
  2. 在使用计算属性的时候，当普通的属性使用即可，因为已经被挂载在Vue对象上了，可打印查看
- 好处:

1. 实现了代码的复用
2. 只要计算属性中依赖的数据源变化了，则计算属性会自动重新求值



#### 11.15 监听器

`watch`: 监听数据变化

##### 11.15.1 基本用法

```js
data: {
	username: 'admin'
}
watch: {
	username(newValue，oldValue){
		......
	}
}
```



##### 11.15.2 监听器immediate的用法

- immediate: 进入页面是否触发，true表示自动触发一次

```js
watch: {
  username: {
    handler(newValue，oldValue){
      ......
    },
    immediate: true
  }
}
```



##### 11.15.3 监听器deep深度监听

```json
data: {
	info: {
		username: 'admin'
	}
},
watch: {
	info: {
		handler(newValue，oldValue){
      ......
    },
    // 只要对象中任一个属性发生变化,都会触发对象的监听器
    deep: true
	}
}
```



##### 11.15.4 监听对象里的某一个属性的变化

```json
data: {
	info: {
		username: 'admin'
	}
},
watch: {
	'info.username'(newValue，oldValue){
    ......
  }
}
```



## 12. axios

> axios是一个专注于网络请求的库

```js
axios({
	method:'请求的类型',
	url:'请求的URL地址',
	params: {},
  data: {}
}).then((result) => { 
	// .then 用来指定请求成功之后的回调函数
	// 形参中的result 是请求成功之后的结果
}
```

> - GET用params
>
> - POST用data
> - await - async

```js
document.querySelector('#btnPost').addEventListener('click'，async function(){
//如果调用某个方法的返回值是Promise 实例，则前面可以添加await
// await只能用在被async “修饰”的方法中
	const { data } = await axios({
		method: 'POST',
		url: 'http://www.liulongbin.top:3006/api/post',
		data: {
			name :'zs',
			age: 20
		}
	})
})
```

```js
document.querySelector('#btnGet').addEventListener('click'，async function(){
//解构赋值的时候使用:进行重命名
const {data: res} = await axios({
	method: 'GET' ,
	url: 'http://www.liulongbin.top:3006/api/getbooks'
})
	console.log(res.data)
})
```



#### 12.1 axios的get方法

```js
document.querySelector('#btnGET').addEventListener('click'，async function(){
	const {data: res} = await axios.get('http://www.liulongbin.top:3006/api/getbooks'，{
		params:{ id: 1 }
  })
	console.log(res)
})
```



#### 12.2 axios的post方法

```js
document.querySelector('#btnGET').addEventListener('click'，async function(){
	const {data: res} = await axios.post('http://www.liulongbin.top:3006/api/getbooks'，{
		name: 'zs',
    age: 20
  })
	console.log(res)
})
```



#### 12.3 把axios挂载到Vue到原型上

1. 在main.js文件中写入，这样每个文件都能访问

```vue
import axios from 'axios'

// 配之后就不用每次都写完整的路径
axios.defaults.baseURL = 'XXXXXX'
Vue.prototype.$http = axios
```

2. 使用axios

```
this.$http('/api/post',{ name: 'zs'，age: 20 })
```



## 13. vue-cli

### 13.1 vue项目的安装

- 安装vue-cli: `npm install -g @vue-cli`
- 在指定的目录下创建项目: `vue create 项目名称(英文的，最好别带空格)`
- **方向键**控制选中的文字，建议选**第三个(Manually select features)**，确认敲**回车**
- 选择需要装的功能，按**空格**进行选择或取消，括号里有`*`表示安装此项

|              选项名               |             作用             |
| :-------------------------------: | :--------------------------: |
|        Choose Vue version         |    询问安装那个版本的Vue     |
|               Babel               | 解决JS兼容性**（一定要装）** |
|            TypeScript             |   TS，比JS更强大的脚本语言   |
| Progressive Web App (PWA) Support |       渐进式的Web框架        |
|              Router               |             路由             |
|               Vuex                |           状态管理           |
|        CSS Pre-processors         |         CSS预处理器          |
|        Linter 1 Formatter         |         代码规范检验         |
|           Unit Testing            |           单元测试           |
|            E2E Testing            |          端到端测试          |

>1. Where do you prefer placing config for Babel, ESLint, etc. ? 
>
>- **In dedicated config files(✔️)** 
>- In package. json
>
>2. Save this as a preset for future projects? (y/N)
>
>- 一般选`y`表示保存选择过的选项，下一次如果用相同的配置的话选择起来会快一点

- 最后进行安装,得到如下结果:

```
added 1241 packages in 40s

13 packages are looking for funding
  run `npm fund` for details
🚀  Invoking generators...
📦  Installing additional dependencies...


added 15 packages in 3s

13 packages are looking for funding
  run `npm fund` for details
⚓  Running completion hooks...

📄  Generating README.md...

🎉  Successfully created project demo1.
👉  Get started with the following commands:

 $ cd demo1
 $ npm run serve
```

- 打开目录的终端界面并运行指令`cd 项目名称`和 `npm run serve`

> 文件结构目录：
>
> - `node_modules`: 项目依赖，用npm安装的包都在这里放着
> - `src`: 源码
>   - `assets` : 存放项目中用到的静态资源文件，例如: css 样式表、图片资源
>   - `components`: 程序员封装的、可复用的组件，都要放到components 目录下
>   - main.js 是项目的入口文件。整个项目的运行，要先执行main.js
>   - `App.vue`: 项目的根组件 

### 13.2 vue项目的运行与使用

- 在工程化的项目中，vue 要做的事情很单纯——通过`main.js`把`App.vue`渲染到`index.html`的指定区域中。
  - App.vue用来编写待渲染的**模板结构**
  - index.html中需要预留一个**el区域**
  - main.js把App.vue渲染到了index.html所预留的区域中

main.js：

```js
//导入vue这个包，得到Vue构造函数
import Vue from 'vue'
//导入App.vue根组件，将来要把App.vue中的模板结构，渲染到HTML页面中
import App from './App.vue'
Vue.config.productionTip = false

//创建Vue的实例对象
new Vue({
	el: '#app',
	//把render函数指定的组件渲染到HTML页面中
	render: h => h(App)
})
// Vue实例的$mount() 方法，作用和el属性完全一样!
```



## 14. Vue组件

例如，你可能会有页头、侧边栏、内容区等组件，每个组件又包含了其它的像导航链接、博文之类的组件。

为了能在模板中使用，这些组件必须先注册以便 Vue 能够识别。这里有两种组件的注

册类型：**全局注册**和**局部注册**。

![组件](https://cn.vuejs.org/images/components.png)

- vue是一个支持组件化开发的前端框架。
- vue中规定组件的后缀名是`.vue`。之前接触到的App.vue文件本质上就是一个vue的组件

1. vue组件的三个组成部分

- `template` -> 组件的**模板结构**
- `script` -> 组件的**JavaScript行为**
- `style` -> 组件的**样式**

2. 在components目录下新建自定义组件

```vue
// test.vue
<template>
	<!-- template中只能包含一个根节点 -->
  <div class="test">
    <div class="a">
      {{ str }}
      <p>111111</p>
    </div>
    <button @click="changeText">改变文字</button>
  </div>
</template>

<script>
// 默认导出，这是固定写法
// 注意: .vue组件中的data不能像之前一样，不能指向对象
// 注意: 组件中的data必须是一个函数
export default {
  data() {
    // 这个return出去的{ }中，可以定义数据
    return {
      str: "hello vue.js",
    };
  },
  methods: {
    changeText() {
      this.str = "hello world";
    },
  },
};
</script>

<style lang="less">
.test {
  background-color: orange;
  .a {
    color: red;
    p {
      color: purple;
    }
  }
}
</style>
```

3. 在App.vue文件中引用并使用

```vue
<template>
  <div id="app">
    <h1>这是根组件</h1>
    <test></test>
  </div>
</template>

<script>
import test from '@/components/test.vue'

export default {
  components: {
    test
  }
}
</script>

<style lang="less">
#app {
  font-family: Avenir，Helvetica，Arial，sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  background-color: #ccc;
}
</style>
```



#### 14.1 vue 全局组件

在vue项目的main.js入口文件中，通过Vue.component()方法注册全局组件

```vue
import Count from '@/components/Count.vue'
Vue.component('MyCount'，Count)
```



#### 14.2 组件的复用性

```vue
export default {
  props: {
    init: {
			// 设置默认值
      default: 0,
			// 指定值的类型必须是Number数字
			type: Number,
			// 必填属性
			required: true
    }
  },
  data() {
    return {
      count: this.init
    }
  }
}

<MyCount :init="6"></MyCount>
```



#### 14.3 组件样式的冲突

使用属性选择器,在组件内的每个标签都加上一个固定的属性或者在style加scoped属性`<style lang="less" scoped>...</style>`



#### 14.4修改子组件样式

/deep/



#### 14.5 生命周期函数

> - 组件创建阶段: beforeCreate、created、beforeMount、mounted
> - 组件运行阶段: beforeUpdate、updated
> - 组件销毁阶段: beforeDestroy、destroyed

![](https://cn.vuejs.org/images/lifecycle.png)

在created函数中发起ajax请求，在mounted函数里操作DOM



#### 14.6 父组件到子组件的传值

```vue
<MyCount :init="myCountNum"></MyCount>

export default {
  data() {
    return {
      myCountNum: 999
    };
  }
};
```



#### 14.7 子组件到父组件到传值

```vue
// 子组件MyCount
methods: {
   add(){
     this.count++;
     this.$emit('numchange'，this.count);
   }
 }

// 父组件
{{ nowCnt }}
<MyCount :init="myCountNum" @numchange="getCount"></MyCount>

export default {
  data() {
    return {
      nowCnt: 0
    }
  },
  methods: {
    getCount(val){
      this.nowCnt = val;
    }
  },
};
```



#### 14.8 兄弟组件之间的传值

EventBus



#### 14.9 使用Ref引用DOM

```vue
// 设置ref属性，名字别冲突
<p ref="count1">当前值为: {{ count }}</p>

// 使用
this.$refs.count1
```

> this.$nextTick(cb): 会把cb回调推迟到下一一个DOM更新周期之后执行。通俗的理解是: 等组件的DOM更新完成之后，再执行cb回调函数。从而能保证cb回调函数可以操作到最新的DOM元素。



#### 14.10 动态展示组件

##### 14.10.1 代码示例

```vue
// 通过is属性来动态绑定想要展示的组件的名字,切换后之前的组件会被销毁
<components :is="TagName"></components>
<button @click="changeTag">改变组件</button>

<script>
import Test from '@/components/test.vue'
import MyCount from '@/components/myCount.vue'

export default {
  data(){
    return {
      TagName: 'Test'
    }
  },
  components: {
    Test,
    MyCount
  },
  methods: {
    changeTag(){
      this.TagName = this.TagName === 'Test' ? 'MyCount' : 'Test';
    }
  }
}
</script>
```



##### 14.10.2 keep-alive的使用

```vue
<keep-alive>
  <components :is="TagName"></components>
</keep-alive>
```



> keep-alive对应的生命周期函数
>
> - 当组件被缓存时，会自动触发组件的deactivated生命周期函数。
> - 当组件被激活时，会自动触发组件的activated生命周期函数。

include属性指定要缓存的组件`include="MyCount，Test"`

exclude属性同理，但是include和exclude不能同时使用



#### 14.11 slot的使用

```vue
<template>
  <div>
    <p>这是Count组件</p>
    <p ref="count1">当前值为: {{ count }}</p>
    <button @click="add">+1</button>
    // 留一个插槽
    <slot></slot>
  </div>
</template>

<MyCount :init="myCountNum" @numchange="getCount">
  <p>这是插槽</p>
</MyCount>
```

> 每一个插槽slot必须有一个name名称，默认name的值为default



##### 14.11.1 将slot放到指定的插槽里

```vue
<template>
  <div>
    <p>这是Count组件</p>
    <p ref="count1">当前值为: {{ count }}</p>
    <button @click="add">+1</button>
    // 给插槽定义一个name名称
    <slot name="footer">默认内容</slot>
  </div>
</template>

<MyCount :init="myCountNum" @numchange="getCount">
  <template v-slot:footer>
    <p>这是插槽</p>
  </template>
</MyCount>
```

> v-slot的语法糖形式是#，即#footer



##### 14.11.2 具名插槽

`<slot name="footer" >默认内容</slot>`

```vue
<MyCount :init="myCountNum" @numchange="getCount">
  <template #footer="scope"> // 接受传过来的值msg并重命名为scope
    <p>这是插槽</p>
		// 使用值
    <p>{{scope.msg}}</p>
  </template>
</MyCount>

<template>
  <div>
    <p>这是Count组件</p>
    <p ref="count1">当前值为: {{ count }}</p>
    <button @click="add">+1</button>
    // msg是预留的,可能用得到的值
    <slot name="footer" msg="这是尾部"></slot>
  </div>
</template>
```



## 15. ESLint

> **在创建Vue项目的时候要选中`Linter / Formatter`，并选中`ESLint + Standard config`，接着选择`Lint on save`**

[ESLint](https://eslint.bootcss.com/)是检查代码格式的一款插件,会让写出来的代码风格保持统一

> **VScode格式设置:** 
>
> 1. 打开右下角设置，输入`tabsize`
> 2. 将`Editor: Tab Size`和`Vetur › Format › Options: Tab Size`的值都改为`2`
> 3. 继续搜索`format`
> 4. 将`Editor: Format On Save`选项打勾☑️

报错可以访问[Rules规则](https://eslint.org/docs/rules/)进行查阅



## 16. router路由

#### 16.1 什么是路由

路由(router)就是**对应关系**



#### 16.2 前段路由的工作方式

1. 用户**点击**了页面上的**路由链接**
2. 导致了**URL地址栏**中的**Hash值**发生了变化
3. **前端路由监听了到Hash地址的变化**(window.onhashchange事件 => location.hash => #/home)
4. 前端路由把当前**Hash地址对应的组件**渲染都浏览器中



#### 16.3 Vue-router

1. 输入命令`npm install vue-router@3.5.2 -S`进行安装

2. 在src源代码目录下，新建router/index.js路由模块，并初始化如下的代码:

```js
// 1.导入Vue和VueRouter 的包
import Vue from 'vue'
import VueRouter from 'vue-router'

// 2.调用Vue.use()函数， 把VueRouter 安装为Vue的插件
Vue.use(VueRouter)

// 3.创建路由的实例对象

const router = new VueRouter()

//4.向外共享路由的实例对象
export default router
```

3. 在main.js文件里挂载router模块

```js
import Vue from 'vue'
import App from './App.vue'
import router from './router/index.js'

Vue.config.productionTip = false

new Vue({
  render: h => h(App),
  router
}).$mount('#app')
```

4. 使用vue-router

```js
// router/index.js
import hello from '@/components/HelloWorld.vue'
import count from '@/components/count.vue'

const router = new VueRouter({
  routers: [
    // 路由规则
    { path: '/home'，component: hello },
    { path: '/count'，components: count }
  ]
})
```

```vue
// 在.vue文件中使用<router-view></router-view>
<template>
  <div>
    <p>hello world</p>
    <a href="#/home">主页</a>
    <a href="#/count">计数器</a>
		<hr />
    <router-view></router-view>
  </div>
</template>
  
// 或者把a标签换成<router-link></router-link>
<router-link to="/home">主页</router-link>
<router-link to="/count">计数器</router-link>
```



#### 16.4 路由重定向

**路由重定向**指的是:用户在访问**地址A**的时候,**强制用户跳转**到地址C，从而展示特定的组件页面。通过路由规则的**redirect**属性，指定一个新的路由地址，可以很方便地设置路由的重定向:

```js
const router = new VueRouter({
  routes: [
    // 重定向地址
    { path: '/'，redirect: '/home'},
    ......
  ]
})
```



#### 16.5 嵌套路由

```vue
// App.vue
<template>
  <div>
    <p>hello world</p>
    <router-link to="/home">主页</router-link>
    <router-link to="/count">计数器</router-link>
    <router-link to="/about">关于</router-link>
    <hr />
    <router-view></router-view>
  </div>
</template>

// about.vue
<template>
  <div>
    <h3>About 组件</h3>
    
    <router-link to="/about/tab1">tab1</router-link>
    <router-link to="/about/tab2">tab2</router-link>
    
    <router-view></router-view>
  </div>
</template>
```

1. 在src/components/目录下先创建tab文件夹，并在文件夹里创建tab1.vue和tab2.vue文件

2. 使用**children**属性声明**子路由规则**

```js
import Tab1 from '@/components/tabs/Tab1.vue'
import Tab2 from ' @/components/tabs/Tab2.vue'

const router = new VueRouter({
  routes: [
    {//about页面的路由规则(父级路由规则)
      path: '/about',
      component: About,
      // 重定向路由
      redirect: '/about/tab1'
      children: [ // 1.通过children属性，嵌套声明子级路由规则
        // 2.访问/about/tab1 时，展示Tab1组件
        { path: 'tab1'，component: Tab1}，
        // 2.访问/about/tab2 时，展示Tab2组件
        { path: 'tab2'，component: Tab2 } 
      ]
    }
  ]
})
```



#### 16.6 动态路由

动态路由指的是: 把Hash地址中**可变的部分**定义为**参数项**，从而**提高路由规则的复用性**。在vue-router中使用**英文的冒号**(:) 来定义路由的参数项。示例代码如下:

`{ path: '/movie/:id?name=zs age=20'，component: Movie}`

拿到id值: `this.$route.param.id`或`$route.param.id`

> - 在hash地址中，/ 后面的参数项叫做“路径参数”，在路由“参数对象”中,需要使用this.$route.params来访问路径参数
> - 在hash地址中，? 后面的参数项叫做“查询参数”，在路由“参数对象”中,需要使用this.$route.query来访问查询参数
> - 在this.$route中，path只是路径部分，fullPath是完整的地址



##### 16.6.1 开启props传参

1. 在router/index.js中的routers数组中写入

`{ path: '/movie/:id'，component: Movie，props: true }`

2. 在movie.vue文件中写入，即可使用{{ id }}

```js
exports default {
	props: ['id']
}
```



#### 16.7 导航跳转

##### 16.7.1 声明式导航 & 编程式导航

- 在浏览器中，**点击链接**实现导航的方式，叫做**声明式导航**。例如: 普通网页中点击<a>链接、vue项目中点击<router-link>都属于声明式导航
- 在浏览器中，**调用API方法**实现导航的方式，叫做**编程式导航**。例如: 普通网页中调用`location.href`跳转到新页面的方式，属于编程式导航



##### 16.7.2 vue-router中的编程式导航API

**vue-router**(导航对象 )提供了许多编程式导航的API,其中最常用的导航API分别是:

1. this.$router.**push**('hash 地址')

- 跳转到指定hash地址，并**增加**一条历史记录

`<button @click="$router.push('/about')">跳转到关于</button>`

> 以上代码只能在写跳转页面的页面上,比如Helloworld.vue文件中,不能写在App.vue中

2. this.$router.**replace**('hash 地址')

- 跳转到指定hash地址，并**替换掉**当前的历史记录

3. this.$router.**go**(数值 n)

- 前进或后退相应的记录



##### 16.7.3 导航全局前置守卫

每次发生路由的**导航跳转**时，都会触发**全局前置守卫**

```js
//创建路由实例对象
const router = new VueRouter ({...})
                               
//调用路由实例对象的beforeEach方法，即可声明“全局前置守卫”
//每次发生路由导航跳转的时候，都会自动触发fn这个“回调函数”
router.beforeEach((to，from，next) => {
	//to是将要访问的路由的信息对象
	//from是将要离开的路由的信息对象
	//next是一个函数，调用next()表示放行，允许这次路由导航
  1. next()//当前用户拥有后台主页的访问权限，直接放行
  2. next('/login')//当前用户没有后台主页的访问权限，强制其跳转到登录页面
  3. next(false)//当前用户没有后台主页的访问权限，不允许跳转到后台主页
})
```

模拟登陆操作

```js
router.beforeEach((to，from，next) => {
if (to.path === '/main') {
	const token = localStorage . getItem( ' token' )
	if (token) {
		next() //访问的是后台主页，且有token的值
  } else {
		next('/login') //访问的是后台主页，没有token的值
} else {
	next() //访问的不是后台主页，直接放行
)
```



#### 16.8 Vant组件库

1. 通过npm安装

> - Vue2项目,安装Vant2: `npm i vant -S`
> - Vue3项目,安装Vant3: `npm i vant@next -S`

2. 在main.js文件中引入全部组件

```js
import Vue from 'vue';
import Vant from 'vant';
import 'vant/lib/index.css';

Vue.use(Vant);
```

> Tips: 配置按需引入后，将不允许直接导入所有组件。

3. 复制相应代码到.vue文件中使用组件
