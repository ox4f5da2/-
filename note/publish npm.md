# npm发包流程

### 1. 注册npm官网账号

npm官网：https://www.npmjs.com/

输入用户名、密码、邮箱

### 2.建立文件

- 输入`npm init`，建立**package.json**文件，配置如下：

```json
{
  "name": "@karl_fang/mylibs",
  "version": "1.0.0",
  "description": "This is a JS library that packages the common functions of mathematics, data structure, function and array.",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [
    "javascript",
    "tools"
  ],
  "author": "karl_fang",
  "license": "MIT",
  "type": "module"
}
```

> - name：一般与文件夹的名字相同
> - version：版本号
> - description：对该NPM包的描述
> - main：入口文件
> - scripts：设置快捷命令
> - keywords：该NPM包的关键词，类似网站的keywords
> - author：作者名
> - license：我也不太清楚各种license的区别，就写了MIT

### 3.写JS库

把JS按module导出，然后在入口文件**index.js**中import导入，最后把所有的导出：

```js
import ArrayTools from './src/array/array.js';
import BinaryTree from './src/dataStructure/binaryTree.js';
import LinkedList from './src/dataStructure/linkedList.js';
import MinStack from './src/dataStructure/minStack.js';
import Sort from './src/dataStructure/sort.js';
import MyFunctions from './src/function/general.js';
import MyRegExp from './src/function/regRep.js';
import MyURL from './src/function/URL.js';
import MathTools from './src/math/math.js';

export {
  ArrayTools,
  BinaryTree,
  LinkedList,
  MinStack,
  Sort,
  MyFunctions,
  MyRegExp,
  MyURL,
  MathTools
}
```

### 4.写README文档

### 5.本地登陆NPM

- 输入命令

```shell
npm adduser
```

- 输入NPM账号的用户名
- 输入密码
- 输入绑定NPM账号的邮箱

> 如果遇到镜像源问题：
>
> ```shell
> npm ERR! code E403
> npm ERR! 403 403 Forbidden - PUT https://registry.npmjs.org/flexing - Forbidden
> npm ERR! 403 In most cases, you or one of your dependencies are requesting
> npm ERR! 403 a package version that is forbidden by your security policy
> ```
>
> 可以输入命令更改到官方的源(E403)才行：
>
> ```shell
> npm config set registry https://registry.npmjs.org/
> ```

### 6.发布NPM包

- 输入`npm publish`

### 7.更新版本

- 输入命令更新版本号

  - 修订号，问题修正

  ```shell
  npm version patch 
  ```

  - 次版本号，功能性新增

  ```shell
  npm version minor
  ```

  - 主版本号，不兼容的API 修改

  ```shell
  npm version major
  ```

- 发布输入

```shell
npm publish
```

> 还会收到邮件，输入8位的OTPcode
>
> 如果NPM包的名字是@开头的那可能会报错，
>
> ```
> npm ERR! publish Failed PUT 402
> npm ERR! code E402
> npm ERR! You must sign up for private packages :
> ```
>
> 这个当你的包名为`@your-name/your-package`时才会出现，原因是当包名以`@your-name`开头时，`npm publish`会默认发布为私有包，但是 npm 的私有包需要付费，所以需要添加如下参数进行发布：
>
> ```shell
> npm publish --access public
> ```

### 8.补充

一般的NPM包应该至少包括入口文件**index.js**，**README**文档，**package.json**文件

