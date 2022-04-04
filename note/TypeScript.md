# TypeScript

[TOC]

## 1. TypeScript介绍

- 以JavaScript为基础构建的语言
- 可以在任何支持JavaScript的平台中执行
- 一个JavaScript的超集
  -  增加了类型
  - 支持ES的新特性
  - 添加ES不具备的新特性
  - 丰富的配置选项
  - 强大的开发工具
- TypeScript扩展了JavaScript并添加了类型

> TS不能被JS解析器直接执行



## 2. TypeScript开发环境搭建

1. 下载Node.js

   打开[Node官网](https://nodejs.org/en/)进行下载安装

   > macOS的话也可以直接打开终端输入`brew install node`

2. 安装Node.js
3. 检查是否安装成功
   - 在终端输入`node -v`显示版本则安装成功

4. 使用npm全局安装typescript

- 进入命令行
- 输入： `npm install -g typescript`

> mac的话记得输入`sudo npm install -g typescript`
>
> 安装后输入`tsc -v`显示版本则安装成功

5. 创建一个ts文件

```tsx
// hello.ts
console.log('Hello TS');
```

6. 使用tsc对ts文件进行编译

- 进入命令行
- 进入ts文件所在目录
- 执行命令: tsc xxx.ts

> 会在当前目录下创建出相应的js文件



## 3.基本类型

### 3.1 类型声明

- 类型声明是TS非常重要的一个特点

- 通过类型声明可以指定TS中变量（参数、形参）的类型

- 指定类型后，当为变量赋值时，TS编译器会自动检查值是否符合类型声明，符合则赋值，否则报错

- 简而言之，类型声明给变量设置了类型，使得变量只能存储某种类型的值

- 语法：

  - ```typescript
    let 变量: 类型;
    let 变量: 类型 = 值;
    function fn(参数: 类型, 参数: 类型): 类型{
        ...
    }
    ```

- 例子：

  ```tsx
  let a:number;
  let b:number = 10;
  //如果变量的声明和赋值是同时进行的，TS可以自动对变量进行类型检测
  let c = false
  //会提示错误，但是编译会成功，可以通过配置让其编译不成功
  a = 'hello' 
  
  function sum(a: number, b: number): number {
    return a + b;
  }
  ```

### 3.2 自动类型判断

- TS拥有自动的类型判断机制
- 当对变量的声明和赋值是同时进行的，TS编译器会自动判断变量的类型
- 所以如果你的变量的声明和赋值时同时进行的，可以省略掉类型声明

### 3.3 类型：

|  类型   |       例子        |              描述              |
| :-----: | :---------------: | :----------------------------: |
| number  |    1, -33, 2.5    |            任意数字            |
| string  | 'hi', "hi", `hi`  |           任意字符串           |
| boolean |    true、false    |       布尔值true或false        |
| 字面量  |      其本身       |  限制变量的值就是该字面量的值  |
|   any   |         *         |            任意类型            |
| unknown |         *         |         类型安全的any          |
|  void   | 空值（undefined） |     没有值（或undefined）      |
|  never  |      没有值       |          不能是任何值          |
| object  |  {name:'孙悟空'}  |          任意的JS对象          |
|  array  |      [1,2,3]      |           任意JS数组           |
|  tuple  |       [4,5]       | 元素，TS新增类型，固定长度数组 |
|  enum   |    enum{A, B}     |       枚举，TS中新增类型       |

#### 3.3.1 number

```tsx
let decimal: number = 6;
let hex: number = 0xf00d;
let binary: number = 0b1010;
let octal: number = 0o744;
let big: bigint = 100n;
```

#### 3.3.2 boolean

```tsx
let isDone: boolean = false;
```

#### 3.3.3 string

```tsx
let color: string = "blue";
color = 'red';

let fullName: string = `Bob Bobbington`;
let age: number = 37;
let sentence: string = `Hello, my name is ${fullName}.

I'll be ${age + 1} years old next month.`;
```

#### 3.3.4 字面量

也可以使用字面量去指定变量的类型，通过字面量可以确定变量的取值范围

```tsx
let color: 'red' | 'blue' | 'black';
let num: 1 | 2 | 3 | 4 | 5;

// 表明a只能被赋值为"male"或"female"，如果为其他则会出错
let a: "male" | "famale";
// 表明b只能被赋值为10
let b: 10;
// 表明c只能被赋值为布尔类型或字符串类型
let c: boolean | string
```

#### 3.3.5 any

> 相对于关闭了TS类型检测

```tsx
// 显式的any声明
let d: any;
// 声明变量如果不指定类型，则TS解析器会自动判断变量的类型为any (隐式的any)
let e;
e = 20;
// e的类型是any，它可以赋值给任意变量
b = e;
```

#### 3.3.6 unknown

```tsx
let f:unknown;
f = 10;
f = "hello";
f = true;

// f的类型是unknown，它不可以直接赋值给其他变量，会报错
b = f;
// 解决方法1：类型检测
if(typeof f === 'number'){
  b = f;
}
// 解决方法2：断言
b = f as string;
b = <string> f
```

#### 3.3.7 void

```tsx
let unusable: void = undefined;
// 没有返回值的函数
function fn():void {}
```

#### 3.3.8 never

```tsx
function error(message: string): never {
  throw new Error(message);
}
```

#### 3.3.9 object

```tsx
let obj: object = {};
// {}用来指定对象中可以包含哪些属性
// 语法: {属性名:属性值, 属性名:属性值}
// 在属性名后边加上?，表示属性是可选的
let b: {name: string, age?: number};
b = {name: 'xxx', age: 18};
// [propName: string]: any表示任意类型的属性
let c: {name :  string, [propName: string]: any};
c = {name: 'yyy', age: 19, gender: '男'};
// 定义函数参数个数和类型
let d: (a:number, b:number) => number;
d = function (n1 : number , n2 : number): number {
	return n1 + n2;
}
```

#### 3.3.10 array

```tsx
let list: number[] = [1, 2, 3];
let list: Array<number> = [1, 2, 3];
```

#### 3.3.11 tuple

```tsx
let x: [string, number];
x = ["hello", 10]; 
```

#### 3.3.12 enum

```tsx
enum Color {
  Red,
  Green,
  Blue,
}
let c: Color = Color.Green;

enum Color {
  Red = 1,
  Green,
  Blue,
}
let c: Color = Color.Green;

enum Color {
  Red = 1,
  Green = 2,
  Blue = 4,
}
let c: Color = Color.Green;

enum Gender {
	Male,
	Female
}
let i: {name: string, gender: Gender};
i = {
  name: 'xxx',
  gender: Gender.Male // 'male'
}
console.log(i.gender == Gender .Male);
```

#### 3.3.13 类型别名

```tsx
type myType = number | string | boolean;
let a: myType
```

#### 3.3.14 类型断言

- 有些情况下，变量的类型对于我们来说是很明确，但是TS编译器却并不清楚，此时，可以通过类型断言来告诉编译器变量的类型，断言有两种形式：

  - 第一种

    ```tsx
    let someValue: unknown = "this is a string";
    let strLength: number = (someValue as string).length;
    ```

  - 第二种

    ```tsx
    let someValue: unknown = "this is a string";
    let strLength: number = (<string>someValue).length;
    ```



## 4. 编译选项

### 4.1 自动编译文件

- 编译文件时，使用`-w`指令后，TS编译器会自动监视文件的变化，并在文件发生变化时对文件进行重新编译。
- 示例：`tsc xxx.ts -w`


### 4.2 自动编译整个项目

#### 4.2.1 简单配置

- 如果直接使用tsc指令，则可以自动将当前项目下的所有ts文件编译为js文件。

- 但是能直接使用tsc命令的前提时，要先在项目根目录下创建一个ts的配置文件 tsconfig.json

- tsconfig.json是一个JSON文件，添加配置文件后，只需只需 tsc 命令即可完成对整个项目的编译，输入：`tsc -w`，就可以监视整个文件夹的所有文件


#### 4.2.2 配置选项

##### 4.2.2.1 include

- 定义希望被编译文件所在的目录

- 默认值：["\*\*/\*"]

- 示例：

  - ```json
    "include":["src/**/*", "tests/**/*"]
    ```

  - 上述示例中，所有src目录和tests目录下的文件都会被编译

##### 4.2.2.2 exclude

- 定义需要排除在外的目录

- 默认值：["node_modules", "bower_components", "jspm_packages"]

- 示例：

  - ```json
    "exclude": ["./src/hello/**/*"]
    ```

  - 上述示例中，src下hello目录下的文件都不会被编译

##### 4.2.2.3 extends

- 定义被继承的配置文件

- 示例：

  - ```json
    "extends": "./configs/base"
    ```

  - 上述示例中，当前配置文件中会自动包含config目录下base.json中的所有配置信息

##### 4.2.2.4 files

- 指定被编译文件的列表，只有需要编译的文件少时才会用到

- 示例：

  - ```json
    "files": [
        "core.ts",
        "sys.ts",
        "types.ts",
        "scanner.ts",
        "parser.ts",
        "utilities.ts",
        "binder.ts",
        "checker.ts",
        "tsc.ts"
      ]
    ```

  - 列表中的文件都会被TS编译器所编译


##### 4.2.2.5 compilerOptions

- 编译选项是配置文件中非常重要也比较复杂的配置选项

- 在compilerOptions中包含多个子选项，用来完成对编译的配置


###### 4.2.2.5.1 target

- 设置ts代码编译的目标版本

- 可选值：

  - ```json
    'es3', 'es5', 'es6', 'es2015', 'es2016', 'es2017', 'es2018', 'es2019', 'es2020', 'es2021', 'es2022', 'esnext'
    ```

- 示例：

  - ```json
    "compilerOptions": {
        "target": "ES6"
    }
    ```

  - 如上设置，我们所编写的ts代码将会被编译为ES6版本的js代码

###### 4.2.2.5.2 lib

- 指定代码运行时所包含的库（宿主环境）

- 可选值：

  - ```json
    'es5', 'es6', 'es2015', 'es7', 'es2016', 'es2017', 'es2018', 'es2019', 'es2020', 'es2021', 'es2022', 'esnext', 'dom', 'dom.iterable', 'webworker', 'webworker.importscripts', 'webworker.iterable', 'scripthost', 'es2015.core', 'es2015.collection', 'es2015.generator', 'es2015.iterable', 'es2015.promise', 'es2015.proxy', 'es2015.reflect', 'es2015.symbol', 'es2015.symbol.wellknown', 'es2016.array.include', 'es2017.object', 'es2017.sharedmemory', 'es2017.string', 'es2017.intl', 'es2017.typedarrays', 'es2018.asyncgenerator', 'es2018.asynciterable', 'es2018.intl', 'es2018.promise', 'es2018.regexp', 'es2019.array', 'es2019.object', 'es2019.string', 'es2019.symbol', 'es2020.bigint', 'es2020.promise', 'es2020.sharedmemory', 'es2020.string', 'es2020.symbol.wellknown', 'es2020.intl', 'es2021.promise', 'es2021.string', 'es2021.weakref', 'es2021.intl', 'es2022.array', 'es2022.error', 'es2022.object', 'es2022.string', 'esnext.array', 'esnext.symbol', 'esnext.asynciterable', 'esnext.intl', 'esnext.bigint', 'esnext.string', 'esnext.promise', 'esnext.weakref'
    ```

- 示例：

  - ```json
    "compilerOptions": {
        "target": "ES6",
        "lib": ["ES6", "DOM"],
        "outDir": "dist",
        "outFile": "dist/aa.js"
    }
    ```

###### 4.2.2.5.3 module

- 设置编译后代码使用的模块化系统

- 可选值：

  - ```json
    'none', 'commonjs', 'amd', 'system', 'umd', 'es6', 'es2015', 'es2020', 'es2022', 'esnext', 'node12', 'nodenext'
    ```

- 示例：

  - ```typescript
    "compilerOptions": {
        "module": "CommonJS"
    }
    ```

###### 4.2.2.5.4 outDir

- 编译后文件的所在目录

- 默认情况下，编译后的js文件会和ts文件位于相同的目录，设置outDir后可以改变编译后文件的位置

- 示例：

  - ```json
    "compilerOptions": {
        "outDir": "./dist"
    }
    ```

  - 设置后编译后的js文件将会生成到dist目录

###### 4.2.2.5.5 outFile

- 将所有的文件编译为一个js文件

- 默认会将所有的编写在全局作用域中的代码合并为一个js文件，如果module制定了none、system或amd则会将模块一起合并到文件之中

- 示例：

  - ```json
    "compilerOptions": {
      	"module": "system"
        "outFile": "./dist/app.js"
    }
    ```

###### 4.2.2.5.6 rootDir

- 指定代码的根目录，默认情况下编译后文件的目录结构会以最长的公共目录为根目录，通过rootDir可以手动指定根目录

- 示例：

  - ```json
    "compilerOptions": {
        "rootDir": "./src"
    }
    ```

###### 4.2.2.5.7 allowJs

- 是否对js文件编译

###### 4.2.2.5.8 checkJs

- 是否对js文件进行检查

- 示例：

  - ```json
    "compilerOptions": {
        "allowJs": true,
        "checkJs": true
    }
    ```

###### 4.2.2.5.9 removeComments

- 是否删除注释
- 默认值：false

###### 4.2.2.5.10 noEmit

- 不对代码进行编译
- 默认值：false

###### 4.2.2.5.11 sourceMap

- 是否生成sourceMap
- 默认值：false

##### 4.2.2.6 严格检查

- strict
  - 启用所有的严格检查，默认值为true，设置后相当于开启了所有的严格检查
- alwaysStrict
  - 总是以严格模式对代码进行编译
- noImplicitAny
  - 禁止隐式的any类型
- noImplicitThis
  - 禁止类型不明确的this
- strictBindCallApply
  - 严格检查bind、call和apply的参数列表
- strictFunctionTypes
  - 严格检查函数的类型
- strictNullChecks
  - 严格的空值检查
- strictPropertyInitialization
  - 严格检查属性是否初始化

##### 4.2.2.7 额外检查

- noFallthroughCasesInSwitch
  - 检查switch语句包含正确的break
- noImplicitReturns
  - 检查函数没有隐式的返回值
- noUnusedLocals
  - 检查未使用的局部变量
- noUnusedParameters
  - 检查未使用的参数

##### 4.2.2.8 高级

- allowUnreachableCode
  - 检查不可达代码
  - 可选值：
    - true，忽略不可达代码
    - false，不可达代码将引起错误
- noEmitOnError
  - 有错误的情况下不进行编译
  - 默认值：false

## 5. webpack

- 通常情况下，实际开发中我们都需要使用构建工具对代码进行打包，TS同样也可以结合构建工具一起使用，下边以webpack为例介绍一下如何结合构建工具使用TS。

- 步骤：

  1. 初始化项目

     - 进入项目根目录，执行命令 ``` npm init -y```
       - 主要作用：创建package.json文件

  2. 下载构建工具

     - ```npm i -D webpack webpack-cli webpack-dev-server typescript ts-loader clean-webpack-plugin```
       - 共安装了7个包
         - webpack
           - 构建工具webpack
         - webpack-cli
           - webpack的命令行工具
         - webpack-dev-server
           - webpack的开发服务器
         - typescript
           - ts编译器
         - ts-loader
           - ts加载器，用于在webpack中编译ts文件
         - html-webpack-plugin
           - webpack中html插件，用来自动创建html文件
         - clean-webpack-plugin
           - webpack中的清除插件，每次构建都会先清除目录

  3. 根目录下创建webpack的配置文件webpack.config.js

     - ```javascript
       const path = require("path");
       const HtmlWebpackPlugin = require("html-webpack-plugin");
       const { CleanWebpackPlugin } = require("clean-webpack-plugin");
       
       module.exports = {
           optimization:{
               minimize: false // 关闭代码压缩，可选
           },
       
           entry: "./src/index.ts",
           
           devtool: "inline-source-map",
           
           devServer: {
               contentBase: './dist'
           },
       
           output: {
               path: path.resolve(__dirname, "dist"),
               filename: "bundle.js",
               environment: {
                   arrowFunction: false // 关闭webpack的箭头函数，可选
               }
           },
       
           resolve: {
               extensions: [".ts", ".js"]
           },
           
           module: {
               rules: [
                   {
                       test: /\.ts$/,
                       use: {
                          loader: "ts-loader"     
                       },
                       exclude: /node_modules/
                   }
               ]
           },
       
           plugins: [
               new CleanWebpackPlugin(),
               new HtmlWebpackPlugin({
                   title:'TS测试'
               }),
           ]
       
       }
       ```

  4. 根目录下创建tsconfig.json，配置可以根据自己需要

     - ```json
       {
           "compilerOptions": {
               "target": "ES2015",
               "module": "ES2015",
               "strict": true
           }
       }
       ```

  5. 修改package.json添加如下配置

     - ```json
       {
         ...略...
         "scripts": {
           "test": "echo \"Error: no test specified\" && exit 1",
           "build": "webpack",
           "start": "webpack serve --open chrome.exe"
         },
         ...略...
       }
       ```

  6. 在src下创建ts文件，并在并命令行执行```npm run build```对代码进行编译，或者执行```npm start```来启动开发服务器

     

## 6. Babel

- 经过一系列的配置，使得TS和webpack已经结合到了一起，除了webpack，开发中还经常需要结合babel来对代码进行转换以使其可以兼容到更多的浏览器，在上述步骤的基础上，通过以下步骤再将babel引入到项目中。

  1. 安装依赖包：

     - ```npm i -D @babel/core @babel/preset-env babel-loader core-js```
     - 共安装了4个包，分别是：
       - @babel/core
         - babel的核心工具
       - @babel/preset-env
         - babel的预定义环境
       - @babel-loader
         - babel在webpack中的加载器
       - core-js
         - core-js用来使老版本的浏览器支持新版ES语法

  2. 修改webpack.config.js配置文件

     - ```javascript
       ...略...
       module: {
           rules: [
               {
                   test: /\.ts$/,
                   use: [
                       {
                           loader: "babel-loader",
                           options:{
                               presets: [
                                   [
                                       "@babel/preset-env",
                                       {
                                           "targets":{
                                               "chrome": "58",
                                               "ie": "11"
                                           },
                                           "corejs":"3",
                                           "useBuiltIns": "usage"
                                       }
                                   ]
                               ]
                           }
                       },
                       {
                           loader: "ts-loader",
       
                       }
                   ],
                   exclude: /node_modules/
               }
           ]
       }
       ...略...
       ```

     - 如此一来，使用ts编译后的文件将会再次被babel处理，使得代码可以在大部分浏览器中直接使用，可以在配置选项的targets中指定要兼容的浏览器版本。



## 7. 面向对象

面向对象是程序中一个非常重要的思想，它被很多同学理解成了一个比较难，比较深奥的问题，其实不然。面向对象很简单，简而言之就是程序之中所有的操作都需要通过对象来完成。

- 举例来说：
  - 操作浏览器要使用window对象
  - 操作网页要使用document对象
  - 操作控制台要使用console对象

一切操作都要通过对象，也就是所谓的面向对象，那么对象到底是什么呢？这就要先说到程序是什么，计算机程序的本质就是对现实事物的抽象，抽象的反义词是具体，比如：照片是对一个具体的人的抽象，汽车模型是对具体汽车的抽象等等。程序也是对事物的抽象，在程序中我们可以表示一个人、一条狗、一把枪、一颗子弹等等所有的事物。一个事物到了程序中就变成了一个对象。

在程序中所有的对象都被分成了两个部分数据和功能，以人为例，人的姓名、性别、年龄、身高、体重等属于数据，人可以说话、走路、吃饭、睡觉这些属于人的功能。数据在对象中被成为属性，而功能就被称为方法。所以简而言之，在程序中一切皆是对象。

### 7.1 类（class）

- 定义类：

  - ```typescript
    class 类名 {
    	属性名: 类型;
    	
    	constructor(参数: 类型){
    		this.属性名 = 参数;
    	}
    	
    	方法名(){
    		....
    	}
    
    }
    ```

- 示例：

  - ```typescript
    class Person{
        name: string;
        age: number;
    
        constructor(name: string, age: number){
            this.name = name;
            this.age = age;
        }
    
        sayHello(){
            console.log(`大家好，我是${this.name}`);
        }
    }
    ```

- 使用类：

  - ```typescript
    const p = new Person('xxx', 18);
    p.sayHello();
    ```



### 7.2 面向对象的特点

#### 7.2.1 属性权限设置

- 对象实质上就是属性和方法的容器，它的主要作用就是存储属性和方法，这就是所谓的封装

- 默认情况下，对象的属性是可以任意的修改的，为了确保数据的安全性，在TS中可以对属性的权限进行设置

- 只读属性（readonly）：

  - 如果在声明属性时添加一个readonly，则属性便成了只读属性无法修改

- TS中属性具有三种修饰符：

  - public（默认值），可以在类、子类和对象中修改
  - protected ，可以在类、子类中修改
  - private ，可以在类中修改

- 示例：

  - public

    - ```typescript
      class Person{
        	// 写或什么都不写都是public
          public name: string; // 1
          public age: number; // 2
      		
          constructor(name: string, age: number){ // 3
              this.name = name; // 可以在类中修改 // 4
              this.age = age; // 5
          } // 6
        // constructor(public name: string, public age: number){...} 相当于上面1-6行代码的简写
      
          sayHello(){
              console.log(`大家好，我是${this.name}`);
          }
      }
      
      class Employee extends Person{
          constructor(name: string, age: number){
              super(name, age);
              this.name = name; //子类中可以修改
          }
      }
      
      const p = new Person('xxx', 18);
      p.name = 'yyy';// 可以通过对象修改
      ```

  - protected

    - ```typescript
      class Person{
          protected name: string;
          protected age: number;
      
          constructor(name: string, age: number){
              this.name = name; // 可以修改
              this.age = age;
          }
      
          sayHello(){
              console.log(`大家好，我是${this.name}`);
          }
      }
      
      class Employee extends Person{
      
          constructor(name: string, age: number){
              super(name, age);
              this.name = name; //子类中可以修改
          }
      }
      
      const p = new Person('xxx', 18);
      p.name = 'yyy';// 不能修改
      ```

  - private

    - ```typescript
      class Person{
          private name: string;
          private age: number;
      
          constructor(name: string, age: number){
              this.name = name; // 可以修改
              this.age = age;
          }
      
          sayHello(){
              console.log(`大家好，我是${this.name}`);
          }
      }
      
      class Employee extends Person{
      
          constructor(name: string, age: number){
              super(name, age);
              this.name = name; //子类中不能修改
          }
      }
      
      const p = new Person('xxx', 18);
      p.name = 'yyy';// 不能修改
      ```


#### 7.2.2 属性存取器

- 对于一些不希望被任意修改的属性，可以将其设置为private

- 直接将其设置为private将导致无法再通过对象修改其中的属性

- 我们可以在类中定义一组读取、设置属性的方法，这种对属性读取或设置的属性被称为属性的存取器

- 读取属性的方法叫做setter方法，设置属性的方法叫做getter方法

- 示例：

  - ```typescript
    class Person{
        private _name: string;
    
        constructor(name: string){
            this._name = name;
        }
    
        get name(){
            return this._name;
        }
    
        set name(name: string){
            this._name = name;
        }
    
    }
    
    const p1 = new Person('xxx');
    console.log(p1.name); // 通过getter读取name属性
    p1.name = 'yyy'; // 通过setter修改name属性
    ```

#### 7.2.3 静态属性

- 静态属性（方法），也称为类属性。使用静态属性无需创建实例，通过类即可直接使用

- 静态属性（方法）使用static开头

- 示例：

  - ```typescript
    class Tools{
        static PI = 3.1415926;
        
        static sum(num1: number, num2: number){
            return num1 + num2
        }
    }
    
    console.log(Tools.PI);
    console.log(Tools.sum(123, 456));
    ```

#### 7.2.4 this

- 在类中，使用this表示当前对象

#### 7.2.5 继承

- 继承时面向对象中的又一个特性

- 通过继承可以将其他类中的属性和方法引入到当前类中

  - 示例：

    - ```typescript
      // 父类
      class Animal{
          name: string;
          age: number;
      
          constructor(name: string, age: number){
              this.name = name;
              this.age = age;
          }
      }
      // 子类
      // 此时，Animal被称为父类，Dog被称为子类
      // 使用继承后，子类将会拥有父类所有的方法和属性
      // 通过继承可以将多个类中共有的代码写在一个父类中,
      // 这样只需要写一次即可让所有的子类都同时拥有父类中的局性和方法
      // 如果希望在子类中添加些父类中没有的属性或方法直接加就行
      class Dog extends Animal{
          bark(){
              console.log(`${this.name}在汪汪叫！`);
          }
      }
      
      const dog = new Dog('dog', 4);
      dog.bark();
      ```

- 通过继承可以在不修改类的情况下完成对类的扩展


#### 7.2.6 重写

- 发生继承时，如果子类中的方法会替换掉父类中的同名方法，这就称为方法的重写

- 示例：

  - ```typescript
    class Animal{
        name: string;
        age: number;
    
        constructor(name: string, age: number){
            this.name = name;
            this.age = age;
        }
    
        run(){
            console.log(`父类中的run方法！`);
        }
    }
    
    class Dog extends Animal{
    
        bark(){
            console.log(`${this.name}在汪汪叫！`);
        }
    
        run(){
            console.log(`子类中的run方法，会重写父类中的run方法！`);
        }
    }
    
    const dog = new Dog('dog', 4);
    dog.bark();
    ```

  - 在子类中可以使用super来完成对父类的引用

#### 7.2.7 抽象类（abstract class）

- 抽象类是专门用来被其他类所继承的类，它只能被其他类所继承不能用来创建实例

- ```typescript
  // 以abstract开头的类是抽象类,抽象类和其他类区别不大，只是不能用来创建对象，抽象类就是专门用来被继承的类
  abstract class Animal{
    	// 定义一个抽象方法，抽象方法使用abstract开头，没有方法体，抽象方法只能定义在抽象类中，子类必须对抽象方法进行重写
      abstract run(): void;
      bark(){
          console.log('动物在叫~');
      }
  }
  
  class Dog extends Animals{
      run(){
          console.log('狗在跑~');
      }
  }
  ```

- 使用abstract开头的方法叫做抽象方法，抽象方法没有方法体只能定义在抽象类中，继承抽象类时抽象方法必须要实现

## 8. 接口（Interface）

接口的作用类似于抽象类，不同点在于接口中的所有方法和属性都是没有实值的，换句话说接口中的所有方法都是抽象方法。接口主要负责定义一个类的结构，接口可以去限制一个对象的接口，对象只有包含接口中定义的所有属性和方法时才能匹配接口。同时，可以让一个类去实现接口，实现接口时类中要保护接口中的所有属性。

- 示例（检查对象类型）：

  - ```typescript
    // 接口用来定义一个类结构，用来定义一个类中应该包含哪些属性和方法，同时接口也可以当成类型声明去使用
    interface Person{
        name: string;
        sayHello():void;
    }
    
    function fn(per: Person){
        per.sayHello();
    }
    
    fn({name:'xxx', sayHello() {console.log(`Hello, 我是 ${this.name}`)}});
    
    ```

- 示例（实现）

  - ```typescript
    // 接口可以在定义类的时候去限制类的结构，接口中的所有的属性都不能有实际的值，接口只定义对象的结构，而不考虑实际值
    interface Person{
        name: string;
        sayHello():void;
    }
    
    class Student implements Person{
        constructor(public name: string) {
        }
    
        sayHello() {
            console.log('大家好，我是'+this.name);
        }
    }
    ```



## 9. 泛型（Generic）

定义一个函数或类时，有些情况下无法确定其中要使用的具体类型（返回值、参数、属性的类型不能确定），此时泛型便能够发挥作用。

- 举个例子：

  - ```typescript
    // 在定义函数或是类时，如果遇到类型不明确就可以使用泛型
    function test(arg: any): any{
    	return arg;
    }
    ```

  - 上例中，test函数有一个参数类型不确定，但是能确定的时其返回值的类型和参数的类型是相同的，由于类型不确定所以参数和返回值均使用了any，但是很明显这样做是不合适的，首先使用any会关闭TS的类型检查，其次这样设置也不能体现出参数和返回值是相同的类型

  - 使用泛型：

  - ```typescript
    function test<T>(arg: T): T{
    	return arg;
    }
    ```

  - 这里的```<T>```就是泛型，T是我们给这个类型起的名字（不一定非叫T），设置泛型后即可在函数中使用T来表示该类型。所以泛型其实很好理解，就表示某个类型。

  - 那么如何使用上边的函数呢？

    - 方式一（直接使用）：

      - ```typescript
        test(10)
        ```

      - 使用时可以直接传递参数使用，类型会由TS自动推断出来，但有时编译器无法自动推断时还需要使用下面的方式

    - 方式二（指定类型）：

      - ```typescript
        test<number>(10)
        ```

      - 也可以在函数后手动指定泛型

  - 可以同时指定多个泛型，泛型间使用逗号隔开：

    - ```typescript
      function test<T, K>(a: T, b: K): K{
          return b;
      }
      
      test<number, string>(10, "hello");
      ```

    - 使用泛型时，完全可以将泛型当成是一个普通的类去使用

  - 类中同样可以使用泛型：

    - ```typescript
      class MyClass<T>{
          prop: T;
      
          constructor(prop: T){
              this.prop = prop;
          }
      }
      ```

  - 除此之外，也可以对泛型的范围进行约束

    - ```typescript
      interface MyInter{
          length: number;
      }
      
      function test<T extends MyInter>(arg: T): number{
          return arg.length;
      }
      ```

    - 使用T extends MyInter表示泛型T必须是MyInter的子类，不一定非要使用接口类和抽象类同样适用。
