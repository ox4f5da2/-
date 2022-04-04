[TOC]

# Node.js

## 1 初识Node.js

### 1.1 什么是node.js

Node.js是一个Javascript运行环境(runtime)。发布于2009年5月，由RyanDahl开发，实质是对Chrorne V8引擎进行了封装。Nodejs是基于V8引擎，V8 是Google发布的开源JavaScript引擎，本身就是用于Chrome浏览器的JS解释部分，但是V8是一个可以独立的模块，完全可以嵌入你自己的应用，所以被Ryan Dahl这哥们，鬼才般的，把这个V8搬到了服务器上，用于做服务器的软件。

### 1.2 NodeJs的优势

1. NodeJs语法完全是js语法，只要你懂JS基础就可以学会Nodejs后端开发。
2. NodeJs超强的高并发能力。
3. 实现高性能服务器
4. 开发周期短、开发成本低、学习成本低

### 1.3 node需要安装什么

安装内容:

- 官网直接下载: https://nodejs.org/zh-cn/

- `npm`: (node package manager)

- `cnpm`: **(淘宝镜像)**--https://npmmirror.com/

  > - 安装**cnpm**: `npm install -g cnpm --registrry=https://registry.npmmirror.com`
  > - 更改镜像源: `npm config set registry https://registry.npmmirror.com`
  > - #查看npm配置信息
  >   `npm config list`

  结果如下所示: 

  ```text
  >>xxx@xxxdeMacBook-Pro ~ % npm config set registry https://registry.npmmirror.com
  
  >>xxx@xxxdeMacBook-Pro ~ % npm config list
  ; "user" config from /Users/karl/.npmrc
  
  registry = "https://registry.npmmirror.com/" 
  
  ; node bin location = /usr/local/bin/node
  ; cwd = /Users/karl
  ; HOME = /Users/karl
  ; Run `npm config ls -l` to show all defaults.
  ```

- `nodemon`: `npm install nodemon -g`(运行内容自动更新,相当于vs code的live Server)

> 如果是macBook电脑需要输入`sudo npm install nodemon -g`,然后输入开机密码,如果不加**sudo**就会出现错误日志为: **Error: EACCES: permission denied**

查看版本: 

```
[xxx@xxxdeMacBook-Pro ~ % nodemon -v            
2.0.15
┌─────────────────────────────────────────────────────────┐
│               nodemon update check failed               │
│           Try running with sudo or get access           │
│          to the local update config store via           │
│ sudo chown -R $USER:$(id -gn $USER) /Users/karl/.config │
└─────────────────────────────────────────────────────────┘
```

可以按照提示输入: `sudo chown -R $USER:$(id -gn $USER) /Users/karl/.config`,也可不输

### 1.4 常用命令

- 退回上一目录 `cd ..\`

- 进入指定目录 `cd名字`

- 进入D盘 `D:` (windows)

- 清空 `cls`

- 生成package.json说明书文件 `npm init`

  > 可以跳过向导，快速生成 `npm init -y`

- 下载包 `npm install 包名`

- 删除包 `npm uninstall 包名`

### 1.5 node模块

#### 1.5.1 模块概述

- 核心模块(系统自带的模块)

  - 文件操作的**fs**
  - http服务操作的**http**
  - **url**路径操作模块
  - **path**路径处理模块

  - **os**操作系统信息

- 第三方模块

  - **art-template jquery vuex express**等
  - 必须通过npm来下载才可以使用

- 自定义模块

  - 自己创建的文件

#### 1.5.2 http模块创建

1. 引入http模块

 `let http = require('http');`

2. createServer搭建服务

```js
http.createServer((req, res) => {
  console.log(1); // 在终端打印1
  res.write('111') // 在页面上显示111,写成数字111就会报错
  res.end() // 结束,必写,否则会一直转动或者直接写成res.end('111后台传入数据')
}).listen(8000) // 链式调用
// req: request
// res: response
```

> 如果传入的是汉子则需要设置响应头,否则会出现乱码
>
> `res.setHeader("Content-Type", "text/plain;charset=utf-8")`
>
> 常见Content-Type见**5.3 媒体格式类型**内容

3. 在终端内输入命令

`nodemon 文件名`

4. 运行结果如下

```
xxx@xxxdeMacBook-Pro node % nodemon xxx.js 
[nodemon] 2.0.15
[nodemon] to restart at any time, enter `rs`
[nodemon] watching path(s): *.*
[nodemon] watching extensions: js,mjs,json
[nodemon] starting `node httpModule.js`
```

5. 打开浏览器地址栏输入127.0.0.1:8000
6. 得到执行结果

#### 1.5.3 媒体格式类型

##### 1.5.3.1 常见的媒体格式类型

- text/html: HTML格式
- text/plain: 纯文本格式
- text/xml: XML格式
- image/gif: gif图片格式
- image/jpeg : jpg图片格式
- image/png: png图片格式

##### 1.5.3.2 以application开头的媒体格式类型

- application/xhtml+xml: XHTML格式

- application/xml:  XML数据格式

- application/atom+xml: Atom XML聚合格式

- application/json:  JSON数据格式

- application/pdf: pdf格式

- application/msword: Word文档格式

- application/octet-stream: 二进制流数据（如常见的文件下载）

- application/x-www-form-urlencoded: 

  > form表单数据被编码为key/value格式发送到服务器(表单默认的提交数据的格式)

### 1.6 CommonJS规范

node应用由模块组成,采用的commonjs模块规范.每一个文件就是一个模块,拥有自己独立的作用域,变量,以及方法等,对其他的模块都不可见.CommonJS规范规定,每个模块内部,module变量代表当前模块.这个变量是一个对象,它的**exports**属性(即module.exports)是对外的接口.加载某个模块,其实是加载该模块的module.exports属性.**require**方法用于加载模块.

## 2 url、path、fs模块

### 2.1模块缓存

这样做的目的是避免重复加载,提高模块加载效率

示例:

```js
let {obj} = require('./storage.js')
console.log(obj)//{a:1,b:2,c:3}
obj.d = 4
console.log(obj)//{a: 1, b: 2, c: 3, d: 4}
let {obj:obj1} = require('./storage.js' )
console.log(obj1)//{a: 1, b: 2, c: 3, d: 4}
```

### 2.2 path模块

#### 2.2.1 path模块引入

`const path = require('path')`

打印结果:

```
<ref *2> {
  resolve: [Function: resolve],
  normalize: [Function: normalize],
  isAbsolute: [Function: isAbsolute],
  join: [Function: join],
  relative: [Function: relative],
  toNamespacedPath: [Function: toNamespacedPath],
  dirname: [Function: dirname],
  basename: [Function: basename],
  extname: [Function: extname],
  format: [Function: bound _format],
  parse: [Function: parse],
  sep: '/',
  delimiter: ':',
  win32: <ref *1> {
    resolve: [Function: resolve],
    normalize: [Function: normalize],
    isAbsolute: [Function: isAbsolute],
    join: [Function: join],
    relative: [Function: relative],
    toNamespacedPath: [Function: toNamespacedPath],
    dirname: [Function: dirname],
    basename: [Function: basename],
    extname: [Function: extname],
    format: [Function: bound _format],
    parse: [Function: parse],
    sep: '\\',
    delimiter: ';',
    win32: [Circular *1],
    posix: [Circular *2],
    _makeLong: [Function: toNamespacedPath]
  },
  posix: [Circular *2],
  _makeLong: [Function: toNamespacedPath]
}
```

> join方法会把/转换成\

#### 2.2.2 path模块中文件路径的常用规定字

- `__dirname`: 指向当前js文件所在**文件夹**的绝对路径
- `__filename`: 指向当前**js文件**所的绝对路径

> 其中`__filename`其实相当于`path.join(__dirname, '文件夹名')`

#### 2.2.3 path模块中的resolve

**resolve函数作用: 将参数装换成绝对路径**

1. 不带参数时
   `path.resolve()`: 返回的是当前文件所在文件夹的绝对路径
2. 带参数(带不是**/**开头的参数)
   `path. resolve('a.js')`: 返回的是当前文件所在文件夹的绝对路径与参数拼接的结果
3. **./**开头的参数同情况2
4. 带**/**参数: 返回的是最后一个前面加**/**的参数拼接其右边的所有参数

```js
path.resolve('/a', 'b') // C:\a\b
path.resolve('/a', '/b')// C:\b
```

#### 2.2.4 path模块中的parse

### 2.3 url模块

#### 2.3.1 url模块引入

`const url = require('url')`

打印结果:

```
{
  Url: [Function: Url],
  parse: [Function: urlParse],
  resolve: [Function: urlResolve],
  resolveObject: [Function: urlResolveObject],
  format: [Function: urlFormat],
  URL: [class URL],
  URLSearchParams: [class URLSearchParams],
  domainToASCII: [Function: domainToASCII],
  domainToUnicode: [Function: domainToUnicode],
  pathToFileURL: [Function: pathToFileURL],
  fileURLToPath: [Function: fileURLToPath],
  urlToHttpOptions: [Function: urlToHttpOptions]
}
```

#### 2.3.2 url模块中的URL类

URL使用:

```js
const { URL } = require('url')

let url = new URL('http://127.0.0.1:8000/login?username=zx&&password=123456')
console.log(url)
```

结果打印:

```
URL {
  href: 'http://127.0.0.1:8000/login?username=zx&&password=123456',
  origin: 'http://127.0.0.1:8000',
  protocol: 'http:',
  username: '',
  password: '',
  host: '127.0.0.1:8000',
  hostname: '127.0.0.1',
  port: '8000',
  pathname: '/login',
  search: '?username=zx&&password=123456',
  searchParams: URLSearchParams { 'username' => 'zx', 'password' => '123456' },
  hash: ''
}
```

> origin: 同源策略,即协议、域名、端口号

### 2.4 fs模块

#### 2.4.1 fs模块引入

`const fs = require('fs')`

打印结果:

```
{
  appendFile: [Function: appendFile],
  appendFileSync: [Function: appendFileSync],
  access: [Function: access],
  accessSync: [Function: accessSync],
  chown: [Function: chown],
  chownSync: [Function: chownSync],
  chmod: [Function: chmod],
  chmodSync: [Function: chmodSync],
  close: [Function: close],
  closeSync: [Function: closeSync],
  copyFile: [Function: copyFile],
  copyFileSync: [Function: copyFileSync],
  cp: [Function: cp],
  cpSync: [Function: cpSync],
  createReadStream: [Function: createReadStream],
  createWriteStream: [Function: createWriteStream],
  exists: [Function: exists],
  existsSync: [Function: existsSync],
  fchown: [Function: fchown],
  fchownSync: [Function: fchownSync],
  fchmod: [Function: fchmod],
  fchmodSync: [Function: fchmodSync],
  fdatasync: [Function: fdatasync],
  fdatasyncSync: [Function: fdatasyncSync],
  fstat: [Function: fstat],
  fstatSync: [Function: fstatSync],
  fsync: [Function: fsync],
  fsyncSync: [Function: fsyncSync],
  ftruncate: [Function: ftruncate],
  ftruncateSync: [Function: ftruncateSync],
  futimes: [Function: futimes],
  futimesSync: [Function: futimesSync],
  lchown: [Function: lchown],
  lchownSync: [Function: lchownSync],
  lchmod: [Function: lchmod],
  lchmodSync: [Function: lchmodSync],
  link: [Function: link],
  linkSync: [Function: linkSync],
  lstat: [Function: lstat],
  lstatSync: [Function: lstatSync],
  lutimes: [Function: lutimes],
  lutimesSync: [Function: lutimesSync],
  mkdir: [Function: mkdir],
  mkdirSync: [Function: mkdirSync],
  mkdtemp: [Function: mkdtemp],
  mkdtempSync: [Function: mkdtempSync],
  open: [Function: open],
  openSync: [Function: openSync],
  opendir: [Function: opendir],
  opendirSync: [Function: opendirSync],
  readdir: [Function: readdir],
  readdirSync: [Function: readdirSync],
  read: [Function: read],
  readSync: [Function: readSync],
  readv: [Function: readv],
  readvSync: [Function: readvSync],
  readFile: [Function: readFile],
  readFileSync: [Function: readFileSync],
  readlink: [Function: readlink],
  readlinkSync: [Function: readlinkSync],
  realpath: [Function: realpath] { native: [Function (anonymous)] },
  realpathSync: [Function: realpathSync] { native: [Function (anonymous)] },
  rename: [Function: rename],
  renameSync: [Function: renameSync],
  rm: [Function: rm],
  rmSync: [Function: rmSync],
  rmdir: [Function: rmdir],
  rmdirSync: [Function: rmdirSync],
  stat: [Function: stat],
  statSync: [Function: statSync],
  symlink: [Function: symlink],
  symlinkSync: [Function: symlinkSync],
  truncate: [Function: truncate],
  truncateSync: [Function: truncateSync],
  unwatchFile: [Function: unwatchFile],
  unlink: [Function: unlink],
  unlinkSync: [Function: unlinkSync],
  utimes: [Function: utimes],
  utimesSync: [Function: utimesSync],
  watch: [Function: watch],
  watchFile: [Function: watchFile],
  writeFile: [Function: writeFile],
  writeFileSync: [Function: writeFileSync],
  write: [Function: write],
  writeSync: [Function: writeSync],
  writev: [Function: writev],
  writevSync: [Function: writevSync],
  Dir: [class Dir],
  Dirent: [class Dirent],
  Stats: [Function: Stats],
  ReadStream: [Getter/Setter],
  WriteStream: [Getter/Setter],
  FileReadStream: [Getter/Setter],
  FileWriteStream: [Getter/Setter],
  _toUnixTimestamp: [Function: toUnixTimestamp],
  F_OK: 0,
  R_OK: 4,
  W_OK: 2,
  X_OK: 1,
  constants: [Object: null prototype] {
    UV_FS_SYMLINK_DIR: 1,
    UV_FS_SYMLINK_JUNCTION: 2,
    O_RDONLY: 0,
    O_WRONLY: 1,
    O_RDWR: 2,
    UV_DIRENT_UNKNOWN: 0,
    UV_DIRENT_FILE: 1,
    UV_DIRENT_DIR: 2,
    UV_DIRENT_LINK: 3,
    UV_DIRENT_FIFO: 4,
    UV_DIRENT_SOCKET: 5,
    UV_DIRENT_CHAR: 6,
    UV_DIRENT_BLOCK: 7,
    S_IFMT: 61440,
    S_IFREG: 32768,
    S_IFDIR: 16384,
    S_IFCHR: 8192,
    S_IFBLK: 24576,
    S_IFIFO: 4096,
    S_IFLNK: 40960,
    S_IFSOCK: 49152,
    O_CREAT: 512,
    O_EXCL: 2048,
    UV_FS_O_FILEMAP: 0,
    O_NOCTTY: 131072,
    O_TRUNC: 1024,
    O_APPEND: 8,
    O_DIRECTORY: 1048576,
    O_NOFOLLOW: 256,
    O_SYNC: 128,
    O_DSYNC: 4194304,
    O_SYMLINK: 2097152,
    O_NONBLOCK: 4,
    S_IRWXU: 448,
    S_IRUSR: 256,
    S_IWUSR: 128,
    S_IXUSR: 64,
    S_IRWXG: 56,
    S_IRGRP: 32,
    S_IWGRP: 16,
    S_IXGRP: 8,
    S_IRWXO: 7,
    S_IROTH: 4,
    S_IWOTH: 2,
    S_IXOTH: 1,
    F_OK: 0,
    R_OK: 4,
    W_OK: 2,
    X_OK: 1,
    UV_FS_COPYFILE_EXCL: 1,
    COPYFILE_EXCL: 1,
    UV_FS_COPYFILE_FICLONE: 2,
    COPYFILE_FICLONE: 2,
    UV_FS_COPYFILE_FICLONE_FORCE: 4,
    COPYFILE_FICLONE_FORCE: 4
  },
  promises: [Getter]
}
```

#### 2.4.2 fs常用功能

##### 2.4.2.1 判断文件还是文件夹

```js
const fs = require('fs')
fs.stat('aa', (err, stats) => {
  // 如果有错误
  if (err) {
    console.log(err);
    return false;
  }
  else if (stats.isFile()) {
    console.log('文件');
  }
  else if (stats.isDirectory()) {
    console.log('文件夹');
  }
})
```

报错结果

```
[Error: ENOENT: no such file or directory, stat 'aa'] {
  errno: -2,
  code: 'ENOENT',
  syscall: 'stat',
  path: 'aa'
}
```

##### 2.4.2.2 创建文件夹

```js
fs.mkdir('js', err => {
  if (err) {
    console.log(err);
    return false;
  }
  console.log('创建文件夹成功');
})
```

##### 2.4.2.3 写入文件

```js
fs.writeFile('./a.txt', '这是写入的内容',err => {
  if (err) {
    console.log(err);
    return false;
  }
  console.log('写入成功');
})
```

> 会把原有文件全覆盖

##### 2.4.2.4 追加文件

```js
fs.appendFile('./a.txt', 'hahah\n',err => {
  if (err) {
    console.log(err);
    return false;
  }
  console.log('追加成功');
})
```

> 不会覆盖原有文件,只会把内容追加到文件最后方

##### 2.4.2.5 读取文件的内容

```js
fs.readFile('./a.txt',(err, data) => {
  if (err) {
    console.log(err);
    return false;
  }
  console.log(data);// 十六进制: <Buffer e8 bf 99 e6 98 af e5 86 99 e5 85 a5 e7 9a 84 e5 86 85 e5 ae b9>
  console.log(data.toString());// 这是写入的内容
})
```

##### 2.4.2.6 读取文件夹的内容

```js
fs.readdir('./',(err, data) => {
  if (err) {
    console.log(err);
    return false;
  }
  console.log(data);
  // 结果: [ 'a.txt', 'httpModule.js', 'js', 'mod.js' ]
})
```

##### 2.4.2.7 剪切文件并修改文件名字

```js
fs.rename('a.txt', './js/b.txt',err => {
  if (err) {
    console.log(err);
    return false;
  }
  console.log('修改成功');
})
```

##### 2.4.2.8 删除目录(空文件夹)

```js
fs.rmdir('./css', err => {
  if (err) {
    console.log(err);
    return false;
  }
  console.log('删除成功');
})
```

##### 2.4.2.9 递归删除文件夹

```js
let fs = require('fs')
let Path  = require('path')

function del(path) {
    //先获取到这个地址所对应的对象
    let stats = fs.statSync(path)
    //判断是否为文件夹
    if (stats.isDirectory()) {
        //是一个文件夹

        //判断是否为空的文件夹

        //读取文件夹当中的所有内容
        let fileList = fs.readdirSync(path)

        if(fileList){
            //该文件夹不为空
            
            // C:/Users/Guai/Desktop/教学/03-暴力删除,爬虫/javascript/ + filepath
            fileList.forEach(filepath=>{
                //递归的思路
                del( Path.join(path, filepath))
            })
        }
        //删除空的文件夹
        fs.rmdirSync(path)
    }
    //这是文件
    if (stats.isFile()) {
        fs.unlinkSync(path)
    }

}

del('C:/Users/Guai/Desktop/教学/03-暴力删除,爬虫/javascript')
```

##### 2.4.2.10 删除文件

```js
fs.unlink('./mod.js', err => {
  if (err) {
    console.log(err);
    return false;
  }
  console.log('删除成功');
})
```

##### 2.4.2.11 文件流形式读取文件数据

```js
const fs = require('fs')
let readStream = fs.createReadStream('./a.txt'), str = '';
readStream.on('data', chunk => str += chunk);
// 读取完成
readStream.on('end', () => console.log(str))
//读取失败
readStream.on('error', err => console.log(err))
```

> 如果文件比较小使用readFile,如果文件比较大使用文件流

##### 2.4.2.12 文件流形式写入文件

```js
const fs = require('fs')
let str = '这是写入的内容\n';
let writeStream = fs.createWriteStream('output.txt');
for (let index = 0; index < 100; index++) writeStream.write(str, 'utf-8');
writeStream.on('finish', () => console.log('写入完成'));
writeStream.on('error', () => console.log('写入失败'));
```

##### 2.4.2.13 管道流传输

```js
let fs = require('fs')
//创建一个可读流
let readStream = fs.createReadStream('output.txt')
//创建一个可写流
let writeStream = fs.createWriteStream('copy.txt')
readStream.pipe(writeStream)
console.log('程序执行完毕')
```

## 3 爬虫

### 3.1 request模块的安装

`npm install request`

### 3.2 request模块引入

`let request = require('request');`

结果打印:

```
[Function: request] {
  get: [Function (anonymous)],
  head: [Function (anonymous)],
  options: [Function (anonymous)],
  post: [Function (anonymous)],
  put: [Function (anonymous)],
  patch: [Function (anonymous)],
  del: [Function (anonymous)],
  delete: [Function (anonymous)],
  jar: [Function (anonymous)],
  cookie: [Function (anonymous)],
  defaults: [Function (anonymous)],
  forever: [Function (anonymous)],
  Request: [Function: Request] {
    debug: undefined,
    defaultProxyHeaderWhiteList: [
      'accept',           'accept-charset',
      'accept-encoding',  'accept-language',
      'accept-ranges',    'cache-control',
      'content-encoding', 'content-language',
      'content-location', 'content-md5',
      'content-range',    'content-type',
      'connection',       'date',
      'expect',           'max-forwards',
      'pragma',           'referer',
      'te',               'user-agent',
      'via'
    ],
    defaultProxyHeaderExclusiveList: [ 'proxy-authorization' ]
  },
  initParams: [Function: initParams],
  debug: [Getter/Setter]
}
```

### 3.3 request模块中的get方法

#### 3.3.1 获取百度首页源代码

```js
let request = require('request');
request.get({
  url: 'https://www.baidu.com' // 网站地址
}, (err, res, body) => {
  if (err) {
    console.log(err);
    return false;
  }
  console.log(body);
})
```

打印结果:

```
<!DOCTYPE html><html><head><meta http-equiv="Content-Type" content="text/html; charset=UTF-8"><meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1"><meta content="always" name="referrer"><meta name="description" content="全球领先的中文搜索引擎、致力于让网民更便捷地获取信息，找到所求。百度超过千亿的中文网页数据库，可以瞬间找到相关的搜索结果。"><link rel="shortcut icon" href="//www.baidu.com/favicon.ico" type="image/x-icon"><link rel="search" type="application/opensearchdescription+xml" href="//www.baidu.com/content-search.xml" title="百度搜索"><title>百度一下，你就知道</title><style type="text/css">body{margin:0;padding:0;text-align:center;background:#fff;height:100%}html{overflow-y:auto;color:#000;overflow:-moz-scrollbars;height:100%}body,input{font-size:12px;font-family:"PingFang SC",Arial,"Microsoft YaHei",sans-serif}a{text-decoration:none}a:hover{text-decoration:underline}img{border:0;-ms-interpolation-mode:bicubic}input{font-size:100%;border:0}body,form{position:relative;z-index:0}#wrapper{height:100%}#head_wrapper.s-ps-islite{padding-bottom:370px}#head_wrapper.s-ps-islite .s_form{position:relative;z-index:1}#head_wrapper.s-ps-islite .fm{position:absolute;bottom:0}#head_wrapper.s-ps-islite .s-p-top{position:absolute;bottom:40px;width:100%;height:181px}#head_wrapper.s-ps-islite #s_lg_img{position:static;margin:33px auto 0 auto;left:50%}#form{z-index:1}.s_form_wrapper{height:100%}#lh{margin:16px 0 5px;word-spacing:3px}.c-font-normal{font:13px/23px Arial,sans-serif}.c-color-t{color:#222}.c-btn,.c-btn:visited{color:#333!important}.c-btn{display:inline-block;overflow:hidden;font-family:inherit;font-weight:400;text-align:center;vertical-align:middle;outline:0;border:0;height:30px;width:80px;line-height:30px;font-size:13px;border-radius:6px;padding:0;background-color:#f5f5f6;cursor:pointer}.c-btn:hover{background-color:#315efb;color:#fff!important}a.c-btn{text-decoration:none}.c-btn-mini{height:24px;width:48px;line-height:24px}.c-btn-primary,.c-btn-primary:visited{color:#fff!important}.c-btn-primary{background-color:#4e6ef2}.c-btn-primary:hover{background-color:#315efb}a:active{color:#f60}#wrapper{position:relative;min-height:100%}#head{padding-bottom:100px;text-align:center}#wrapper{min-width:1250px;height:100%;min-height:600px}#head{position:relative;padding-bottom:0;height:100%;min-height:600px}.s_form_wrapper{height:100%}.quickdelete-wrap{position:relative}.tools{position:absolute;right:-75px}.s-isindex-wrap{position:relative}#head_wrapper.head_wrapper{width:auto}#head_wrapper{position:relative;height:40%;min-height:314px;max-height:510px;width:1000px;margin:0 auto}#head_wrapper .s-p-top{height:60%;min-height:185px;max-height:310px;position:relative;z-index:0;text-align:center}#head_wrapper input{outline:0;-webkit-appearance:none}#head_wrapper .s_btn_wr,#head_wrapper .s_ipt_wr{display:inline-block;zoom:1;background:0 0;vertical-align:top}#head_wrapper .s_ipt_wr{position:relative;width:546px}#head_wrapper .s_btn_wr{width:108px;height:44px;position:relative;z-index:2}#head_wrapper .s_ipt_wr:hover #kw{border-color:#a7aab5}#head_wrapper #kw{width:512px;height:16px;padding:12px 16px;font-size:16px;margin:0;vertical-align:top;outline:0;box-shadow:none;border-radius:10px 0 0 10px;border:2px solid #c4c7ce;background:#fff;color:#222;overflow:hidden;box-sizing:content-box}#head_wrapper #kw:focus{border-color:#4e6ef2!important;opacity:1}#head_wrapper .s_form{width:654px;height:100%;margin:0 auto;text-align:left;z-index:100}#head_wrapper .s_btn{cursor:pointer;width:108px;height:44px;line-height:45px;padding:0;background:0 0;background-color:#4e6ef2;border-radius:0 10px 10px 0;font-size:17px;color:#fff;box-shadow:none;font-weight:400;border:none;outline:0}#head_wrapper .s_btn:hover{background-color:#4662d9}#head_wrapper .s_btn:active{background-color:#4662d9}#head_wrapper .quickdelete-wrap{position:relative}#s_top_wrap{position:absolute;z-index:99;min-width:1000px;width:100%}.s-top-left{position:absolute;left:0;top:0;z-index:100;height:60px;padding-left:24px}.s-top-left .mnav{margin-right:31px;margin-top:19px;display:inline-block;position:relative}.s-top-left .mnav:hover .s-bri,.s-top-left a:hover{color:#315efb;text-decoration:none}.s-top-left .s-top-more-btn{padding-bottom:19px}.s-top-left .s-top-more-btn:hover .s-top-more{display:block}.s-top-right{position:absolute;right:0;top:0;z-index:100;height:60px;padding-right:24px}.s-top-right .s-top-right-text{margin-left:32px;margin-top:19px;display:inline-block;position:relative;vertical-align:top;cursor:pointer}.s-top-right .s-top-right-text:hover{color:#315efb}.s-top-right .s-top-login-btn{display:inline-block;margin-top:18px;margin-left:32px;font-size:13px}.s-top-right a:hover{text-decoration:none}#bottom_layer{width:100%;position:fixed;z-index:302;bottom:0;left:0;height:39px;padding-top:1px;overflow:hidden;zoom:1;margin:0;line-height:39px;background:#fff}#bottom_layer .lh{display:inline;margin-right:20px}#bottom_layer .lh:last-child{margin-left:-2px;margin-right:0}#bottom_layer .lh.activity{font-weight:700;text-decoration:underline}#bottom_layer a{font-size:12px;text-decoration:none}#bottom_layer .text-color{color:#bbb}#bottom_layer a:hover{color:#222}#bottom_layer .s-bottom-layer-content{text-align:center}</style></head><body><div id="wrapper" class="wrapper_new"><div id="head"><div id="s-top-left" class="s-top-left s-isindex-wrap"><a href="//news.baidu.com/" target="_blank" class="mnav c-font-normal c-color-t">新闻</a><a href="//www.hao123.com/" target="_blank" class="mnav c-font-normal c-color-t">hao123</a><a href="//map.baidu.com/" target="_blank" class="mnav c-font-normal c-color-t">地图</a><a href="//live.baidu.com/" target="_blank" class="mnav c-font-normal c-color-t">直播</a><a href="//haokan.baidu.com/?sfrom=baidu-top" target="_blank" class="mnav c-font-normal c-color-t">视频</a><a href="//tieba.baidu.com/" target="_blank" class="mnav c-font-normal c-color-t">贴吧</a><a href="//xueshu.baidu.com/" target="_blank" class="mnav c-font-normal c-color-t">学术</a><div class="mnav s-top-more-btn"><a href="//www.baidu.com/more/" name="tj_briicon" class="s-bri c-font-normal c-color-t" target="_blank">更多</a></div></div><div id="u1" class="s-top-right s-isindex-wrap"><a class="s-top-login-btn c-btn c-btn-primary c-btn-mini lb" style="position:relative;overflow:visible" name="tj_login" href="//www.baidu.com/bdorz/login.gif?login&amp;tpl=mn&amp;u=http%3A%2F%2Fwww.baidu.com%2f%3fbdorz_come%3d1">登录</a></div><div id="head_wrapper" class="head_wrapper s-isindex-wrap s-ps-islite"><div class="s_form"><div class="s_form_wrapper"><div id="lg" class="s-p-top"><img hidefocus="true" id="s_lg_img" class="index-logo-src" src="//www.baidu.com/img/flexible/logo/pc/index.png" width="270" height="129" usemap="#mp"><map name="mp"><area style="outline:0" hidefocus="true" shape="rect" coords="0,0,270,129" href="//www.baidu.com/s?wd=%E7%99%BE%E5%BA%A6%E7%83%AD%E6%90%9C&amp;sa=ire_dl_gh_logo_texing&amp;rsv_dl=igh_logo_pcs" target="_blank" title="点击一下，了解更多"></map></div><a href="//www.baidu.com/" id="result_logo"></a><form id="form" name="f" action="//www.baidu.com/s" class="fm"><input type="hidden" name="ie" value="utf-8"> <input type="hidden" name="f" value="8"> <input type="hidden" name="rsv_bp" value="1"> <input type="hidden" name="rsv_idx" value="1"> <input type="hidden" name="ch" value=""> <input type="hidden" name="tn" value="baidu"> <input type="hidden" name="bar" value=""> <span class="s_ipt_wr quickdelete-wrap"><input id="kw" name="wd" class="s_ipt" value="" maxlength="255" autocomplete="off"> </span><span class="s_btn_wr"><input type="submit" id="su" value="百度一下" class="bg s_btn"> </span><input type="hidden" name="rn" value=""> <input type="hidden" name="fenlei" value="256"> <input type="hidden" name="oq" value=""> <input type="hidden" name="rsv_pq" value="b9ff093e0000e419"> <input type="hidden" name="rsv_t" value="3635FYbdbC8tlWmudZmYaUnaucNe+RzTzNEGqg/JuniQU10WL5mtMQehIrU"> <input type="hidden" name="rqlang" value="cn"> <input type="hidden" name="rsv_enter" value="1"> <input type="hidden" name="rsv_dl" value="ib"></form></div></div></div><div id="bottom_layer" class="s-bottom-layer s-isindex-wrap"><div class="s-bottom-layer-content"><p class="lh"><a class="text-color" href="//home.baidu.com/" target="_blank">关于百度</a></p><p class="lh"><a class="text-color" href="//ir.baidu.com/" target="_blank">About Baidu</a></p><p class="lh"><a class="text-color" href="//www.baidu.com/duty" target="_blank">使用百度前必读</a></p><p class="lh"><a class="text-color" href="//help.baidu.com/" target="_blank">帮助中心</a></p><p class="lh"><a class="text-color" href="//www.beian.gov.cn/portal/registerSystemInfo?recordcode=11000002000001" target="_blank">京公网安备11000002000001号</a></p><p class="lh"><a class="text-color" href="//beian.miit.gov.cn/" target="_blank">京ICP证030173号</a></p><p class="lh"><span id="year" class="text-color"></span></p><p class="lh"><span class="text-color">互联网药品信息服务资格证书 (京)-经营性-2017-0020</span></p><p class="lh"><a class="text-color" href="//www.baidu.com/licence/" target="_blank">信息网络传播视听节目许可证 0110516</a></p></div></div></div></div><script type="text/javascript">var date=new Date,year=date.getFullYear();document.getElementById("year").innerText="©"+year+" Baidu "</script></body></html>
```

#### 3.3.2 爬取网站图片

##### 3.3.2.1 获取网站源码

```js
let request = require('request');
request.get({
  url: 'http://photo.china.com.cn' // 网址
}, (err, res, body) => {
  if (err) {
    console.log(err);
    return false;
  }
  console.log(body);
})
```

##### 3.3.2.2 分析图片URL

- http://images.china.cn/site1000/2022-02/09/81e4dee8-a4f1-44f6-98e3-226339cc78ba.jpg
- http://images.china.cn/site1000/2022-02/11/c37a911c-80dc-4190-a47a-6e277c94fac0.jpg
- http://images.china.cn/site1000/2022-02/16/7db250f1-255d-478a-9652-d20c4bb2af06.jpg
- http://images.china.cn/site1000/2022-01/28/0ea9d856-5c64-4985-ba5d-155436c3b358.jpg
- http://images.china.cn/site1000/2019-10/23/a01dd522-0fce-430f-928c-003330f5faaa.jpg

##### 3.3.2.3 正则匹配图片URL

```
let reg = /http:\/\/images\.china\.cn\/site1000\/\d+-\d+\/\d+\/[\da-z-]+\.jpg/g;
let imgSrc = body.match(reg);
console.log(imgSrc);
```

##### 3.3.2.4 获取得到图片URL

```
[
  'http://images.china.cn/site1000/2021-08/25/99c99115-ba07-446b-b4c9-0ed25cce48e7.jpg',
  'http://images.china.cn/site1000/2022-02/19/2df8cfa2-dd08-403f-ab3b-b5338994e17d.jpg',
  'http://images.china.cn/site1000/2022-02/12/ea9f52cc-5ff2-4a67-b282-0a428853abde.jpg',
  'http://images.china.cn/site1000/2022-02/11/c37a911c-80dc-4190-a47a-6e277c94fac0.jpg',
  'http://images.china.cn/site1000/2022-02/11/caf38f90-977c-46a3-b693-2639906d1947.jpg',
  'http://images.china.cn/site1000/2018-01/02/943b307d-5914-43e9-aaf1-90538f186350.jpg',
  'http://images.china.cn/site1000/2017-05/10/6c0b840a23ae1a7dfaa401.jpg',
  'http://images.china.cn/site1000/2017-05/10/6c0b840a23ae1a7dfd3f03.jpg',
  'http://images.china.cn/site1000/2021-08/25/b3c96d57-bcc9-4e67-9e58-6d8eb7340678.jpg',
  'http://images.china.cn/site1000/2022-02/19/30a586e1-6fa6-4876-973e-cb287e9724ae.jpg',
  'http://images.china.cn/site1000/2022-02/19/ae2569ab-ee18-45fc-aa9b-69bef2c6a8dc.jpg',
  'http://images.china.cn/site1000/2022-02/19/76bbd161-f8ac-48eb-8744-f87b214589dd.jpg',
  'http://images.china.cn/site1000/2022-02/19/0ca32007-034a-4b04-a7a6-f5f27c35012c.jpg',
  'http://images.china.cn/site1000/2022-02/19/2e6fe4c9-a79e-4700-b181-08195de2e5db.jpg',
  'http://images.china.cn/site1000/2022-02/19/a3008316-d221-4305-bd1d-4391b151ea79.jpg',
  'http://images.china.cn/site1000/2022-01/05/7e1a82e3-e5f4-43c5-8a28-3e8f1a5b16d1.jpg',
  'http://images.china.cn/site1000/2021-12/10/f7626024-035d-4527-bc1a-a7ad901337ca.jpg',
  'http://images.china.cn/site1000/2021-12/09/30274ab1-d8a9-4a26-83ed-2e3d9fc33c64.jpg',
  'http://images.china.cn/site1000/2022-02/11/adad171b-8fcc-4090-b8aa-28053456174c.jpg',
  'http://images.china.cn/site1000/2021-08/25/c5536d47-f465-46fd-af74-d1327178dd3c.jpg',
  'http://images.china.cn/site1000/2021-08/20/42027d8b-891f-4302-a473-c5213e54bbea.jpg',
  'http://images.china.cn/site1000/2021-07/30/d1b380c7-ef21-42e3-8128-268d12c54d0d.jpg',
  'http://images.china.cn/site1000/2021-02/03/c71f0803-c997-4c0d-be9d-7a646bf9524f.jpg',
  'http://images.china.cn/site1000/2021-11/18/eae9a435-76c2-426d-bf26-4b8d52ffc832.jpg',
  'http://images.china.cn/site1000/2020-08/18/aa0c66a1-37d0-40ba-ae5c-18f187de0121.jpg',
  'http://images.china.cn/site1000/2019-11/06/4cfac031-fb93-4e79-9066-d7efcf67d850.jpg',
  'http://images.china.cn/site1000/2019-10/23/a01dd522-0fce-430f-928c-003330f5faaa.jpg',
  'http://images.china.cn/site1000/2019-01/29/099c7ee5-5dd6-4f0a-b48a-d8f5fffecfd2.jpg',
  'http://images.china.cn/site1000/2019-01/21/5a006720-9f74-4e6c-b34a-173711e9fcd3.jpg',
  'http://images.china.cn/site1000/2021-11/18/e808ade7-519a-46d3-9eeb-daca70178929.jpg',
  'http://images.china.cn/site1000/2018-03/03/120b0f5c-15e1-44f6-8f8d-feff7250f3ff.jpg',
  'http://images.china.cn/site1000/2018-02/13/a6e0027c-f946-4f81-a196-b3a5086028f3.jpg',
  'http://images.china.cn/site1000/2017-11/20/b8aeed99057d1b7ce14e5a.jpg',
  'http://images.china.cn/site1000/2017-09/01/6c0b840a25301b1304f534.jpg',
  'http://images.china.cn/site1000/2017-08/23/6c0b840a23ae1b072a9854.jpg',
  'http://images.china.cn/site1000/2022-02/11/c37a911c-80dc-4190-a47a-6e277c94fac0.jpg',
  'http://images.china.cn/site1000/2020-12/31/43b33d6d-bf01-4c1b-b446-c9396d5ffbf9.jpg',
  'http://images.china.cn/site1000/2020-10/10/b3db6a83-d6c4-459b-8589-62b3a02cdd7d.jpg',
  'http://images.china.cn/site1000/2019-11/13/57abbc6a-8122-46e1-8cca-d47021a3f3a6.jpg',
  'http://images.china.cn/site1000/2019-11/06/4cfac031-fb93-4e79-9066-d7efcf67d850.jpg',
  'http://images.china.cn/site1000/2019-10/23/a01dd522-0fce-430f-928c-003330f5faaa.jpg',
  'http://images.china.cn/site1000/2020-08/18/aa0c66a1-37d0-40ba-ae5c-18f187de0121.jpg',
  'http://images.china.cn/site1000/2019-11/04/779562e6-5da4-41ff-ab75-50ccccac125f.jpg',
  'http://images.china.cn/site1000/2019-03/27/7b29eeb6-adb5-4e37-8d22-c9a5d1cdf08c.jpg',
  'http://images.china.cn/site1000/2019-03/27/d6f2c206-f0bb-4c4e-ae1d-e04c62fd2b4a.jpg',
  'http://images.china.cn/site1000/2017-11/20/b8aeed99057d1b7cd0f53d.jpg',
  'http://images.china.cn/site1000/2019-01/21/5a006720-9f74-4e6c-b34a-173711e9fcd3.jpg',
  'http://images.china.cn/site1000/2018-04/27/9ea32424-8a68-437b-a946-0db79c9d2aac.jpg',
  'http://images.china.cn/site1000/2018-02/07/65addd13-9156-4008-9189-dd72113389f1.jpg',
  'http://images.china.cn/site1000/2017-12/21/6c0b840a23ae1b80a3282c.jpg',
  'http://images.china.cn/site1000/2016-09/24/b8aeed99057d1950b4d32b.jpg',
  'http://images.china.cn/site1000/2016-07/18/6c0b840a23ae18f6d28d2b.jpg',
  'http://images.china.cn/site1000/2022-01/28/0ea9d856-5c64-4985-ba5d-155436c3b358.jpg',
  'http://images.china.cn/site1000/2018-02/12/cb05bea2-039b-40e6-a77f-49eee9d529c1.jpg'
]
```

##### 3.3.2.5 将URL写入到txt文件

```js
let writeStream = fs.createWriteStream('imgSrc.txt');
writeStream.write(body.match(reg).join('\n'), 'utf-8');
```

##### 3.3.2.6 完整代码

```js
let request = require('request');
let fs = require('fs')
request.get({
  url: 'http://photo.china.com.cn'
}, (err, res, body) => {
  if (err) {
    console.log(err);
    return false;
  }
  let reg = /http:\/\/images\.china\.cn\/site1000\/\d+-\d+\/\d+\/[\da-z-]+\.jpg/g;
  let writeStream = fs.createWriteStream('imgSrc.txt');
  writeStream.write(body.match(reg).join('\n'), 'utf-8');
})
```

#### 3.3.3 爬取小说文件

##### 3.3.3.1 安装cheerio模块

`npm install cheerio`

> cheerio和jquery类似,因为已经把jquery库中的一些内容进行了删减保存了真正需要的API

##### 3.3.3.2 获取a标签的href属性值

```js
let $ = cheerio.load(body);
$('.col-4 a').each(function() {
  console.log($(this).prop('href'));
})
```

##### 3.3.3.3 封装获取小说内容函数

```js
function getContent(url, title) {
  request(url, (err, res, body) => {
    let $ = cheerio.load(body);
    let txt = $('.content').text();
    fs.writeFileSync(`./fiction/${title}.txt`, txt, err => {
      if (err) {
        console.log(err);
        return false;
      }
      console.log('写入成功');
    });
  })
}
```

##### 3.3.3.4 将小说内容写入txt文件

```js
$('.col-4 a').each(function(){
  getContent($(this).prop('href'), $(this).text());
});
```

##### 3.3.3.5 完整代码

```js
let request = require('request');
let cheerio = require('cheerio');
let fs = require('fs');
request.get({
  url: 'http://huayu.zongheng.com/showchapter/1092749.html' // 网址
}, (err, res, body) => {
  if (err) {
    console.log(err);
    return false;
  }
  let $ = cheerio.load(body);
  $('.col-4 a').each(function(){
    getContent($(this).prop('href'), $(this).text());
  });
})
function getContent(url, title) {
  request(url, (err, res, body) => {
    let $ = cheerio.load(body);
    let txt = $('.content').text();
    fs.writeFileSync(`./fiction/${title}.txt`, txt, err => {
      if (err) {
        console.log(err);
        return false;
      }
      console.log('写入成功');
    });
  })
}
```

## 4 express框架

### 4.1 art-template

#### 4.1.1 下载并导入art-template模块

- 下载: `npm install art-template`

- 导入: `let template = require('art-template');`

#### 4.1.2 创建HTML模板内容

```html
<!-- 文件名是template.html -->
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>模板</title>
</head>
<body>
  我的名字是{{name}}
  我的年龄是{{age}}
  我的课程是{{course}}
</body>
</html>
```

#### 4.1.3 获取HTML源码

```js
let template = require('art-template');
let fs = require('fs');

fs.readFile('./template.html', (err, data) => {
  if (err) {
    console.log(err);
    return err;
  }
  console.log(data.toString()); // html源码
})
```

#### 4.1.4 渲染赋值

> 类似Vue的模板

渲染:

```js
let temp = template.render(data.toString(), {
  name: 'Karl',
  age: 22,
  course: 'node'
})
console.log(temp);
```

渲染结果:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>模板</title>
</head>
<body>
  我的名字是Karl
  我的年龄是22
  我的课程是node
</body>
</html>
```

#### 4.1.5 完整代码

html代码:

```html
<!-- 文件名是template.html -->
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>模板</title>
</head>
<body>
  我的名字是{{name}}
  我的年龄是{{age}}
  我的课程是{{course}}
</body>
</html>
```

js代码:

```js
let template = require('art-template');
let fs = require('fs');

fs.readFile('./template.html', (err, data) => {
  if (err) {
    console.log(err);
    return err;
  }
  let temp = template.render(data.toString(), {
    name: 'Karl',
    age: 22,
    course: 'node'
  })
  console.log(temp);
})
```

### 4.2 express的get请求

#### 4.2.1 下载并导入express模块

- 下载: `npm install express`

- 导入: `let express = require('express');` 

#### 4.2.2 监听端口

创建app: `let app = express();`

监听端口: 

```js
const port = 8000;
app.listen(port, () => {
	console.log(`监听${port}端口成功~`);
})
```

#### 4.2.3 get请求

##### 4.2.3.1 固定路由请求

```js
app.get('/', (req, res) => {
  res.send('这是根路径');
})
app.get('/login', (req, res) => {
    res.send('login页面')
})
app.get('/index', (req, res) => {
    res.send('index页面')
})
```

##### 4.2.3.2 动态路由请求

```js
// :name相当于形参
app.get('/course/:name', (req, res) => {
  let { name } = req.params; // 路由名字
  let { pageNum } = req.query;
  res.send(`这是${name}课程${pageNum==undefined?``:`的第${pageNum}页`}.`);
})
```

- test1:
  - input: `http://localhost:8000/course/C++`
  - output: `这是C++课程.`

- test2:
  - input: `http://localhost:8000/course/python?pageNum=10`
  - output: `这是python课程的第10页.`

##### 4.2.3.3 错误路由中间件

```js
app.use((req, res) => {
	res.send('这是404页面');
})
```

##### 4.2.3.4 路由错误后重定向

###### 4.2.3.4.1 开放指定目录中的资源

```js
app.use(express.static('./public'));
```

> 只要新建文件夹并命名为public即可,文件夹的所以资源就可直接访问如:`http://localhost:8000/js/a.js`

###### 4.2.3.4.2 路由重定向

```js
app.use((req, res) => {
  // res.send('这是404页面');
  res.redirect('./html/404.html');
})
```

> - 只能输入1层文件夹名: `http://localhost:8000/htmls`,能正常显示404;
>
> - 如果输入2层文件夹名则报错: `http://localhost:8000/htmls/404.html`.

###### 4.2.3.4.3 404页面跳转到首页

代码如下:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>404</title>
</head>
<body>
  <h2>抱歉你访问的页面不存在,<span style="color: blue;">5</span>s后访问首页.</h2>
  <script>
    let span = document.querySelector('span');
    let timer = setInterval(function(){
      if(span.innerText !== '1') 
        span.innerText = span.innerText - 1;
      else {
        clearInterval(timer);
        location.href = './index.html';
      }
    }, 1000);
  </script>
</body>
</html>
```

##### 4.2.3.5 完整代码

```js
let express = require('express');
let app = express();
const port = 8000;

//通过中间件调用自身的API,去开放指定目录中的资源
app.use(express.static('./public'));

app.get('/', (req, res) => {
  res.send('这是根路径');
})

app.get('/login', (req, res) => {
  res.send('login页面')
})

app.get('/index', (req, res) => {
  res.send('index页面')
})

app.get('/course/:name', (req, res) => {
  let { name } = req.params;
  let { pageNum } = req.query;
  res.send(`这是${name}课程${pageNum==undefined?``:`的第${pageNum}页`}.`);
})

app.use((req, res) => {
  // res.send('这是404页面');
  res.redirect('./html/404.html');
})

app.listen(port, () => {
  console.log(`监听${port}端口成功~`);
})
```

### 4.3 express结合art-template

#### 4.3.1 下载并导入使用模块

- 下载: 

  - **art-template**模块: `npm install art-template`

  - **express-art-template**模块: `npm install express-art-template`

- 导入:

```js
let express = require('express');
let app = express();
app.use(express.static('./public'));
//虽然这里没有用到art-template这个包,但是我们在使用express-art-template的时候,也必须要下载art-template这个包,这是因为express-art-template这个包里面依赖了很多art-template这个包中的内容
app.engine('html', require('express-art-template'));
```

#### 4.3.2 渲染数据

```js
//有一个默认的规则,它会自动从views目录中查找该模板文件
app.get('/', (req, res) => {
  res.render('./html/template.html', {
    name: 'Karl',
    age: 22,
    course: 'node.js'
  })
})
```

> **注意**: 一定要新建一个**views**文件夹
>
> /: 代表输入网址的路由可以访问到渲染后的页面,等价于输入`http://localhost:8000/`就会访问渲染后到页面;
>
> ./html/template.html: 代表views文件夹下的模板路径

#### 4.3.3 完整代码

```js
let express = require('express');
let app = express();
const port = 8000;
app.use(express.static('./public'));
app.engine('html', require('express-art-template'));

app.get('/', (req, res) => {
  res.render('./html/template.html', {
    name: 'Karl',
    age: 22,
    course: 'node.js'
  })
})

app.listen(port, () => console.log(`监听${port}端口成功~`));
```

### 4.4 express的post请求

#### 4.4.1 引入获取post参数的方法

```js
//express框架的自带方法获取post参数
app.use(express.urlencoded({ extended: false }))
```

#### 4.4.2 表单页面

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>模板</title>
</head>
<body>
  <form action="/post" method="post">
    账号: <input name="name"/>
    密码: <input name="pwd"/>
    <button type="submit">提交</button>
  </form>
</body>
</html>
```

#### 4.4.3 监听登陆页面

```js
app.get('/login', (req, res) => {
  res.redirect('/html/login.html');
});

// 或者用模板渲染
// app.get('/login', (req, res) => {
//    res.render('./html/login.html', {
//        name: '账号',
//        psw: '密码'
//    })
// })
```

#### 4.4.4 获取表单参数

```js
app.post('/post', (req, res) => { 
  console.log(req.body); // { name: 'fzw', pwd: '123456' }
  if (req.body.name === 'fzw' && req.body.pwd === '123456')
    res.redirect('/html/index.html');
  else
    res.redirect('/html/404.html');
});
```

## 5 cookie和session

### 5.1 cookie

#### 5.1.1 cookie简介

cookie是存储于访问者的计算机中的变量.可以让我们用同一个浏览器访问同一个域名的时候共享数据.

HTTP是无状态协议.当你浏览了一个页面,然后转到同一个网站的另一个页面,服务器无法认识到这是同一个浏览器在访问同一个网站.每一次的访问,都是没有任何关系的.

- cookie保存在浏览器本地,正常设置的cookie是不加密的,用户可以自由看到
- 用户可以删除cookie
- cookie可以修改
- cookie可以用于攻击
- cookie存储量很小

#### 5.1.2 下载并导入cookie-parser模块

- 下载: `npm install cookie-parser`

- 导入: `let cookie = require('cookie-parser');` 

#### 5.1.3 设置cookie值

```js
app.get('/', (req, res) => {
  res.cookie('name', 'hahaha', {
    maxAge: 10000, // 过期时间,单位毫秒
    // domain: '.zixuan.com',//指定那个域名能共享cookie信息
    // path:'/index' //设置缓存的路径,只有/index才能访问得到cookie信息
    // secure:false, //设置是否只能通过https来传递此条cookie  true:https   false:http
    httpOnly: true //不能通过document.cookie来访问cookie信息,防止程序(JS)脚本来读取Cookie信息,防止XSS攻击
    //XSS(CSS)攻击:巧妙的输入一些恶意代码到网页,来获取一些不该获取到的数据
    //SQL注入
  });
  res.send('这是根页面'); // 写在cookie设置的下方
});
```

### 5.2 session

#### 5.2.1 下载并导入express-session模块

- 下载: `npm install express-session`

- 导入: `let session = require('express-session');` 

#### 5.2.2 设置session的中间件

```js
//设置seesion 的中间件
app.use(session({
    secret: 'keyboard cat',
    resave: false,//强制保存 session 即使它没有变化,建议改为false
    saveUninitialized: true,//无论有没有session cookie 每次请求都设置一个session cookie
    cookie: {
        secure: true
    },
    rolling: true//在每次请求时都会更新cookie信息,重置cookie过期时间
}))
```

#### 5.2.3 session与cookie的区别

1. cookie数据存放在客户的浏览器上,session数据放在服务器上;
2. cookie不是很安全,别人可以分析存放在本地的COOKIE并进行COOKIE欺骗,考虑到安全应当使用session;
3. session会在一定时间内保存在服务器上.当访问增多会比较占用你服务器的性能考虑到减轻服务器性能方面,应当使用COOKIE;
4. 单个cookie保存的数据不能超过4K,很多浏览器都限制一个站点最多保存20个cookie.
