# node.js

## FS模块

```js
const fs = require('fs')
//导入 fs 模块，来操作文件

fs.readFile('存放路径'[,'编码格式'],function(err,dataStr){})
//调用 fs.readFile() 方法读取文件
//参数1：读取文件的存放路径
//参数2：读取文件时候采用的编码格式，一般默认指定 utf8
//参数3：回调函数，拿到读取失败和成功的结果  err  dataStr
//如果读取成功，则 err 的值为 null
//如果读取失败，则 err 的值为 错误对象，dataStr 的值为undefined


fs.writefile('存放路径'，写入内容[，'编码格式'],function(err){})
//调用 fs.writeFile() 方法，写入文件的内容
//参数1：表示文件的存放路径
//参数2：表示要写入的内容
//参数3：回调函数
//如果文件写入成功，则 err 的值等于 null
//如果文件写入失败，则 err 的值等于一个 错误对象

// __dirname 表示当前文件所处的目录
//url = __dirname + '路径（如/file/1.txt）'
```

## path路径模块

```JS

const path = require('path')；
//导入path模块

path.join( )
//可以进行路径拼接
// 注意：  ../ 会抵消前面的路径
// const pathStr = path.join('/a', '/b/c', '../../', './d', 'e')
// console.log(pathStr)  // \a\b\d\e

path.basename('文件路径名'，'文件后缀名')
//path.pathname返回文件名，第二个参数可以去掉文件后缀名

path.extname(path);
//获选文件拓展名
```

## http模块

### 服务器相关概念

ip地址：每台计算机的唯一地址，点分十进制（a.b.c.d）如（192.168.1.1）

```JS
//搜索(终端中输入)
ping URL//如ping baidu.com
```

域名地址：与IP地址一一对应，对应关系存放在域名服务器(DNS服务器)  

端口号：不同端口号标识不同web服务

1. 一个端口号对应一个web服务
2. 在实际应用中，80端口可以被省略

### 创建web服务器

 ```js

// 1. 导入 http 模块
const http = require('http')

// 2. 创建 web 服务器实例
const server = http.createServer()

// 3. 为服务器实例绑定 request 事件，监听客户端的请求
//req是请求对象，包含了与客户端相关的数据和属性
server.on('request', function (req, res) {

  // req.url 是客户端请求的 URL 地址
  const url = req.url

  // req.method 是客户端请求的 method 类型
  const method = req.method
  
  // 调用 res.setHeader() 方法，设置 Content-Type 响应头，解决中文乱码的问题
  res.setHeader('Content-Type', 'text/html; charset=utf-8')

  // 调用 res.end() 方法，向客户端响应一些内容
  const str = 'str'
  x
})
// 4. 启动服务器
server.listen(8080, function () {  
  console.log('server running at http://127.0.0.1:8080')
})

 ```

## 模块化

将复杂问题中的系统划分成若干模块，模块是可组合，分解和更换的  

1. 内置模块：node官方提供，如fs，path，http
2. 自定义模块：用户创建的js文件
3. 第三方模块，第三方开发出的模块，使用前需要先下载

### 引入模块

```js

const fs = require('fs')//加载内置模块

const custom = require（'./custom.js'）//加载自定义模块（可以省略.js后缀名）

const moment = require('moment')//加载外置的moment模块

//使用require是会执行被加载对象
```

### 模块作用域

在模块中被定义的定义的变量和方法只能在当前模块中被访问

module对象，存储了和当前模块有关的信息

module.exports对象（默认为空）,可以将模块内的成员共享出去，供外界使用,require方法导入自定义模块时，得到的就是module.export所指向的对象

exports对象时对module.exports对象的简化，默认情况下指向同一个对象，最终结果以module.exports对象为准

```JS

// 1. 定义格式化时间的方法(dataForm.js)
function dateFormat(dtStr) {
  const dt = new Date(dtStr)

  const y = dt.getFullYear()
  const m = padZero(dt.getMonth() + 1)
  const d = padZero(dt.getDate())

  const hh = padZero(dt.getHours())
  const mm = padZero(dt.getMinutes())
  const ss = padZero(dt.getSeconds())

  return `${y}-${m}-${d} ${hh}:${mm}:${ss}`
}

// 定义补零的函数
function padZero(n) {
  return n > 9 ? n : '0' + n
}

module.exports = {
  dateFormat
}

//test.js
const time = require('./15.dateFormat')

const dt = new Date();

const newDt = time.dateFormat(dt);
console.log(newDt);
console.log(dt);

```

```JS

exports.name = 'zs';
module.exports = {
    id: 1,
    sayhello: function () {
        console.log('hello')
    }
}

console.log(module.exports)//{ id: 1, sayhello: [Function: sayhello] }
```

```JS

exports.name = 'zs'
console.log(module.exports)//{ name: 'zs' }
```

### Common.js中的模块化

1. 每个模块内部，module代表当前模块
2. module变量时隔对象，他的exports属性是对外的接口
3. 加载某个模块，实际是加载该模块的module.exprots属性。require()方法用于加载模块

## npm与包

Node.js中的第三方模块又叫做包

[全球最大包共享平台：npmjs](https://www.npmjs.com/)

```JS

//使用moment包
npm i moment//默认安装最新版，若要安装指定版本在包后面加 @版本号 a(大版本).b（功能版本）.c（bug修复版本）
// 导入自定义的格式化时间的模块
const TIME = require('./15.dateFormat')

// 调用方法，进行时间的格式化
const dt = new Date()
// console.log(dt)
const newDT = TIME.dateFormat(dt)
console.log(newDT)
```

装包成功后，项目文件夹会多一个叫做node_modules的文件夹（存放已安装的包）和package-lock.json（记录每一个包的下载信息）的配置文件

### 包管理配置文件

在项目根目录中，创建一个叫做package.json的配置文件，可用来记录项目中安装了哪个包，方便删除node_modules文件后，在团队协作中共享源代码

### npm命令

```js
npm install 包名//安装包
npm init -y//命令只能在英文的目录下运行成功，项目文件夹一定要用英文，不要有空格
npm install (npm i )//一次性安装所有依赖包（读取package.json中的dependencies节点
npm uninstall  包名 //删除指定的包  
npm i 包名 -D//安装包并记录到package.json中的devdependencies节点中 
npm i 包名 -g//安装为全局包（默认被安装在C:\Users\用户目录\AppData\Roaming\npm\node_modules） 
npm uninstall  包名// 卸载安装的全局包
```

### 包的分类

包分为项目包和全局包

项目包

1. 开发依赖包（devdependencies节点中的包，开发时候会用到）
2. 核心依赖包（dependencies节点，开发和项目上线都会用到）

全局包

1. 只有工具性质的包，才有全局安装的必要，因为它们提供了好用的终端命令
2. 判断某个包是否要全局安装后使用，可以参考官方提供的申明文档

```js

npm i i5ting_toc g //安装为全局包
i5ting_toc -f md文件路径 -o//调用i5ting_toc，转md为html
```

### 规范的包结构

1. 包必须以单独的目录存在
2. 包的顶级目录下要必须包括package.json这个包管理文件
3. package.json中必须包含name（包的名字），version（版本号），main（包的入口）这三个属性

### 开发包

1. 新建文件夹作为根目录
2. 在文件夹中新建三个文件
   1. package.json(包管理配置文件)
   2. index.js(包入口文件)
   3. README.md(包的说明文档)
      1. 安装方式
      2. 导入方式
      3. 格式化时间
      4. 转义HTML中的特殊字符
      5. 还原HTML中的特殊字符
      6. 开源协议

itzch文件夹

1. package.json
2. index.js
3. src文件夹
   1. dateFormate.js
   2. htmlescape.js

```json
package.json

{
    "name": "itzch",
    "version": "1.0.0",
    "main": "index.js",
    "description": "格式化时间,HTMLEscape相关的功能",
    "keywords": [
        "itzch",
        "dataFormat",
        "escape"
    ],
    "license": "ISC"
}

```

```js
index.js

const date = require('./src/dataFormate')

const escape = require('./src/htmlEscape')

module.exports = {
    ...date,
    ...escape
}

```

```JS
dateFormate.js

// 1. 定义格式化时间的方法(dataForm.js)
function dateFormat(dtStr) {
    const dt = new Date(dtStr)

    const y = dt.getFullYear()
    const m = padZero(dt.getMonth() + 1)
    const d = padZero(dt.getDate())

    const hh = padZero(dt.getHours())
    const mm = padZero(dt.getMinutes())
    const ss = padZero(dt.getSeconds())

    return `${y}-${m}-${d} ${hh}:${mm}:${ss}`
}

// 定义补零的函数
function padZero(n) {
    return n > 9 ? n : '0' + n
}

module.exports = {
    dateFormat
}
```

```JS
htmlescape.js

// 定义转义 HTML 字符的函数
function htmlEscape(htmlstr) {
    return htmlstr.replace(/<|>|"|&/g, match => {
        switch (match) {
            case '<':
                return '&lt;'
            case '>':
                return '&gt;'
            case '"':
                return '&quot;'
            case '&':
                return '&amp;'
        }
    })
}

// 定义还原 HTML 字符串的函数
function htmlUnEscape(str) {
    return str.replace(/&lt;|&gt;|&quot;|&amp;/g, match => {
        switch (match) {
            case '&lt;':
                return '<'
            case '&gt;':
                return '>'
            case '&quot;':
                return '"'
            case '&amp;':
                return '&'
        }
    })
}

module.exports = {
    htmlEscape,
    htmlUnEscape
}
```

### 发布包到npm

1. 注册账号
2. 登录mpm账号
   1. 将服务器切换为官方服务器
      1. nrm ls
      2. nrm use npm
   2. 在npm终端中执行npm login
   3. 输入用户名，密码，邮箱
3. 将终端切换到包的根目录，运行npm publish(包名不能雷同)
4. 删除已发布的包，运行 npm unpublish  包名 --force（只能删除72小时内发布的包，被删除的包24小时内不能重复发布）

## 模块的加载机制

1. 在第一次加载后被缓存，多次调用不会导致模块代码被执行多次
2. 内置模块的加载优先级最高
3. 加载自定义模块时，必须指定以./或../开头的路径标识符，否则会被当做内置模块或第三方模块。
4. 加载文件的顺序
   1. 按照确切的文件名进行加载
   2. 补全.js进行加载
   3. 补全.json进行加载
   4. 补全.node进行加载
   5. 报错
5. require()的模块标识符不是内置模块，也没有以./或../开头，则会从当前模块的父目录开始，尝试从/node_modules文件夹中加载第三方模块，如果还没有找到，则移动到上一层父目录，直到文件系统的根目录
6. 将目录作为模块标识符
   1. 查找package.json文件的main属性
   2. 如果第一步查找失败，试图加载目录下的index.js文件
   3. 报错
