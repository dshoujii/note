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

## express

### 创建一个基本的express服务器

```js
// 1. 导入 express
const express = require('express')
// 2. 创建 web 服务器
const app = express()

// 4. 监听客户端的 GET 和 POST 请求，并向客户端响应具体的内容
app.get('/user', (req, res) => {
  // 调用 express 提供的 res.send() 方法，向客户端响应一个 JSON 对象
  res.send({ name: 'zs', age: 20, gender: '男' })
})
app.post('/user', (req, res) => {
  // 调用 express 提供的 res.send() 方法，向客户端响应一个 文本字符串
  res.send('请求成功')
})
app.get('/', (req, res) => {
  // 通过 req.query 可以获取到客户端发送过来的 查询参数
  // 注意：默认情况下，req.query 是一个空对象
  console.log(req.query)
  res.send(req.query)
})
// 注意：这里的 :id 是一个动态的参数
app.get('/user/:ids/:username', (req, res) => {
  // req.params 是动态匹配到的 URL 参数，默认也是一个空对象
  console.log(req.params)
  res.send(req.params)
})

// 3. 启动 web 服务器
app.listen(80, () => {
  console.log('express server running at http://127.0.0.1')
})

```

### 向外托管静态资源

```JS

const express = require('express')
const app = express()

// 在这里，调用 express.static() 方法，快速的对外提供静态资源,根据目录的添加顺序查找文件
app.use('/files', express.static('./files'))
app.use(express.static('./clock'))

app.listen(80, () => {
  console.log('express server running at http://127.0.0.1')
})
```

## express路由

路由就是映射关系  
express中的路由指的是客户端的请求和服务器处理函数之间的映射关系，由三部分组成，分别是请求的类型，请求的URL地址，处理函数（app.METHOD(PATH,HANDLER)）

1. express路由按照定义的先后顺序进行匹配
2. 请求类型和请求的URL同时匹配成功才会调用对应的处理函数

### 简单的挂载路由

```js

const express = require('express')
const app = express()

// 挂载路由
app.get('/', (req, res) => {
  res.send('hello world.')
})
app.post('/', (req, res) => {
  res.send('Post Request.')
})

app.listen(80, () => {
  console.log('http://127.0.0.1')
})
```

### 模块化路由

1. 创建路由模块对应的.js文件
2. 调用epress.router()函数创建路由对象
3. 在路由对象上挂载具体的路由
4. 使用 module.exports向外共享路由对象
5. 使用app.use()函数注册路由模块

```JS

router.js

// 这是路由模块
// 1. 导入 express
const express = require('express')
// 2. 创建路由对象
const router = express.Router()

// 3. 挂载具体的路由
router.get('/user/list', (req, res) => {
  res.send('Get user list.')
})
router.post('/user/add', (req, res) => {
  res.send('Add new user.')
})

// 4. 向外导出路由对象
module.exports = router


server.js

const express = require('express')
const app = express()

// app.use('/files', express.static('./files'))

// 1. 导入路由模块
const router = require('./03.router')
// 2. 注册路由模块
app.use('/api', router)

// 注意： app.use() 函数的作用，就是来注册全局中间件

app.listen(80, () => {
  console.log('http://127.0.0.1')
})

```

## express中间件

express的中间件，本质上就是一个处理函数，中间件函数的形参列表中，必须包含next函数，而路由处理函数中只包含req和res  
中间件的作用:多个中间件之间，共享同一份req和res，基于这样的特性我们可以在上游的中间件中，统一为req和res对象添加自定义的属性和方法，供下游的中间件或路由使用  
可以用app.use()连续定义多个全局中间件，客户端请求到达服务器之后，会按照中间件定义的先后顺序依次进行调用  
注意事项：

1. 在路由之前注册中间件
2. 可以连续调用多个中间件
3. 不要忘记调用next()函数
4. next函数后不要写额外代码（为了防止代码逻辑混乱）
5. 多个中间件之间，共享同一份req和res

### 全局生效的中间件

客户端发起的任何请求，到达服务器之后都会触发的中间件叫做全局生效的中间件，通过调用app.use(中间件函数)即可定义一个全局生效的中间件

```JS

const express = require('express')
const app = express()

// // 定义一个最简单的中间件函数
// const mw = function (req, res, next) {
//   console.log('这是最简单的中间件函数')
//   // 把流转关系，转交给下一个中间件或路由
//   next()
// }

// // 将 mw 注册为全局生效的中间件
// app.use(mw)

// // 这是定义全局中间件的简化形式
// app.use((req, res, next) => {
//   console.log('这是最简单的中间件函数')
//   next()
// })

// 这是定义全局中间件的简化形式
app.use((req, res, next) => {
  // 获取到请求到达服务器的时间
  const time = Date.now()
  // 为 req 对象，挂载自定义属性，从而把时间共享给后面的所有路由
  req.startTime = time
  next()
})

app.get('/', (req, res) => {
  console.log('调用了 / 这个路由')
  res.send('Home page.')
})
app.get('/user', (req, res) => {
  console.log('调用了 /user 这个路由')
  res.send('User page.')
})

app.listen(80, () => {
  console.log('http://127.0.0.1')
})

```

### 局部生效的中间件

不使用app.use()定义的中间件，叫做局部生效的中间件

```JS

// 导入 express 模块
const express = require('express')
// 创建 express 的服务器实例
const app = express()

// 1. 定义中间件函数
const mw1 = (req, res, next) => {
  console.log('调用了局部生效的中间件')
  next()
}

const mw2 = (req, res, next) => {
  console.log('调用了第二个局部生效的中间件')
  next()
}

// 2. 创建路由
app.get('/', mw1, (rAeq, res) => {
  res.send('Home page.')
})

// // 同时使用多个局部中间件
// app.get('/', [mw1, mw2], (req, res) => {
//   res.send('Home page.')
// })

app.get('/user', (req, res) => {
  res.send('User page.')
})

// 调用 app.listen 方法，指定端口号并启动web服务器
app.listen(80, function () {
  console.log('Express server running at http://127.0.0.1')
})

```

### 中间件的分类

1. 应用级别的中间件:通过app.use,app.get,app.post绑定到app实例上的中间件，叫做应用级别的中间件
2. 路由级别的中间件：绑定到express.router（）实例上的中间件
3. 错误级别的中间件：专门用来捕获项目中发生的错误，错误处理级别的中间件的functio处理函数中必须有四个形参（err,req,res,next）
4. express内置的中间件：
   1. express.static快速托管静态资源的内置中间件
   2. expr.json解析JSON格式的请求体数据
   3. express.urlencoded解析 URL-encoded格式的请求体数据
5. 第三方的中间件
   1. 使用npm下载第三方的中间件
   2. 使用require导入中间件
   3. 使用app.use注册并使用中间件

#### 错误级别中间件实例

```JS

// 导入 express 模块
const express = require('express')
// 创建 express 的服务器实例
const app = express()

// 1. 定义路由
app.get('/', (req, res) => {
  // 1.1 人为的制造错误
  throw new Error('服务器内部发生了错误！')
  res.send('Home page.')
})

// 2. 定义错误级别的中间件，捕获整个项目的异常错误，从而防止程序的崩溃，必须注册在路由之后
app.use((err, req, res, next) => {
  console.log('发生了错误！' + err.message)
  res.send('Error：' + err.message)
})

// 调用 app.listen 方法，指定端口号并启动web服务器
app.listen(80, function () {
  console.log('Express server running at http://127.0.0.1')
})

```

#### 内置中间件的实例

```JS

// 导入 express 模块
const express = require('express')
// 创建 express 的服务器实例
const app = express()

// 注意：除了错误级别的中间件，其他的中间件，必须在路由之前进行配置
// 通过 express.json() 这个中间件，解析表单中的 JSON 格式的数据
app.use(express.json())
// 通过 express.urlencoded() 这个中间件，来解析 表单中的 url-encoded 格式的数据
app.use(express.urlencoded({ extended: false }))

app.post('/user', (req, res) => {
  // 在服务器，可以使用 req.body 这个属性，来接收客户端发送过来的请求体数据
  // 默认情况下，如果不配置解析表单数据的中间件，则 req.body 默认等于 undefined
  console.log(req.body)
  res.send('ok')
})

app.post('/book', (req, res) => {
  // 在服务器端，可以通过 req,body 来获取 JSON 格式的表单数据和 url-encoded 格式的数据
  console.log(req.body)
  res.send('ok')
})

// 调用 app.listen 方法，指定端口号并启动web服务器
app.listen(80, function () {
  console.log('Express server running at http://127.0.0.1')
})

```

#### 第三方中间件的实例

```JS

// 导入 express 模块
const express = require('express')
// 创建 express 的服务器实例
const app = express()

// 1. 导入自己封装的中间件模块
const customBodyParser = require('./custom-body-parser')
// 2. 将自定义的中间件函数，注册为全局可用的中间件
app.use(customBodyParser)

app.post('/user', (req, res) => {
  res.send(req.body)
})

// 调用 app.listen 方法，指定端口号并启动web服务器
app.listen(80, function () {
  console.log('Express server running at http://127.0.0.1')
})

custom-body-parser文件

// 导入 Node.js 内置的 querystring 模块
const qs = require('querystring')

const bodyParser = (req, res, next) => {
  // 定义中间件具体的业务逻辑
  // 1. 定义一个 str 字符串，专门用来存储客户端发送过来的请求体数据
  let str = ''
  // 2. 监听 req 的 data 事件
  req.on('data', (chunk) => {
    str += chunk
  })
  // 3. 监听 req 的 end 事件
  req.on('end', () => {
    // 在 str 中存放的是完整的请求体数据
    // console.log(str)
    // TODO: 把字符串格式的请求体数据，解析成对象格式
    const body = qs.parse(str)
    req.body = body
    next()
  })
}

module.exports = bodyParser

```

## 编写接口

```JS

// 导入 express
const express = require('express')
// 创建服务器实例
const app = express()

// 配置解析表单数据的中间件
app.use(express.urlencoded({ extended: false }))

// 必须在配置 cors 中间件之前，配置 JSONP 的接口
app.get('/api/jsonp', (req, res) => {
  // TODO: 定义 JSONP 接口具体的实现过程
  // 1. 得到函数的名称
  const funcName = req.query.callback
  // 2. 定义要发送到客户端的数据对象
  const data = { name: 'zs', age: 22 }
  // 3. 拼接出一个函数的调用
  const scriptStr = `${funcName}(${JSON.stringify(data)})`
  // 4. 把拼接的字符串，响应给客户端
  res.send(scriptStr)
})

// 一定要在路由之前，配置 cors 这个中间件，从而解决接口跨域的问题
const cors = require('cors')
app.use(cors())

// 导入路由模块
const router = require('./16.apiRouter')
// 把路由模块，注册到 app 上
app.use('/api', router)

// 启动服务器
app.listen(80, () => {
  console.log('express server running at http://127.0.0.1')
})


const express = require('express')
const router = express.Router()

// 在这里挂载对应的路由
router.get('/get', (req, res) => {
  // 通过 req.query 获取客户端通过查询字符串，发送到服务器的数据
  const query = req.query
  // 调用 res.send() 方法，向客户端响应处理的结果
  res.send({
    status: 0, // 0 表示处理成功，1 表示处理失败
    msg: 'GET 请求成功！', // 状态的描述
    data: query, // 需要响应给客户端的数据
  })
})

// 定义 POST 接口
router.post('/post', (req, res) => {
  // 通过 req.body 获取请求体中包含的 url-encoded 格式的数据
  const body = req.body
  // 调用 res.send() 方法，向客户端响应结果
  res.send({
    status: 0,
    msg: 'POST 请求成功！',
    data: body,
  })
})

// 定义 DELETE 接口
router.delete('/delete', (req, res) => {
  res.send({
    status: 0,
    msg: 'DELETE请求成功',
  })
})

module.exports = router

```

## 跨域

CORS(跨域资源共享)由一系列响应头组成，决定浏览器是否阻止前端JS代码跨域获取资源，主要在服务端进行配置

### cors中间件

1. npm install cors
2. const cors = requi re('cors')
3. app.use(cors)

### 请求头

```JS
//允许资源访问
res.setHeader('Access-Control-Allow-Origin','*或具体域名')
//默认情况下，cors仅支持客户端向服务器发送默认的9个请求头，若要发送额外请求头则需要在服务器端通过Access-Control-Allow-Headers进行设置
res.setHeader('Access-Control-Allow-Headers','请求头1,请求头2，请求头3')
//默认情况CORS仅仅支持发送GET,POST,HEAD请求。如果希望通过PUT,DELETE等方式，则需要通过Access-Control-Allow-Origin进行设置
res.setHeader('Access-Control-Allow-Headers','*/方法名1，方法名2，方法名3')
```

### 分类

1. 简单请求(同时满足以下条件)
   1. 请求方式：GET,POST,HEAD三者之一
   2. HTTP头部信息：无自定义头部字段，Accept，Accept Language，Content-Language,DPR,Downlink,Save-Data,Viewpoint-Width,Content-Type(application/x-www-form-urlencoded,multipart/form-data,text)
2. 预检请求，在正式通信之前会发送option请求进行预检(包含一种情况)：
   1. GET,POST,HEAD三者之外
   2. 请求头中包含自定义头部字符串
   3. 向服务器发送application/json的数据

## mysql模块

### 增删改查

```js
 npm i mysql
 // 1. 导入 mysql 模块
const mysql = require('mysql')
// 2. 建立与 MySQL 数据库的连接关系
const db = mysql.createPool({
  host: '127.0.0.1', // 数据库的 IP 地址
  user: 'root', // 登录数据库的账号
  password: 'admin123', // 登录数据库的密码
  database: 'my_db_01', // 指定要操作哪个数据库
})

测试 mysql 模块能否正常工作
db.query('select 1', (err, results) => {
  // mysql 模块工作期间报错了
  if(err) return console.log(err.message)
  // 能够成功的执行 SQL 语句
  console.log(results)
})

// 查询 users 表中所有的数据
const sqlStr = 'select * from users'
db.query(sqlStr, (err, results) => {
  // 查询数据失败
  if (err) return console.log(err.message)
  // 查询数据成功
  // 注意：如果执行的是 select 查询语句，则执行的结果是数组
  console.log(results)
}) 

// 向 users 表中，新增一条数据，其中 username 的值为 Spider-Man，password 的值为 pcc123
 const user = { username: 'Spider-Man', password: 'pcc123' }
// 定义待执行的 SQL 语句
const sqlStr = 'insert into users (username, password) values (?, ?)'
// 执行 SQL 语句
db.query(sqlStr, [user.username, user.password], (err, results) => {
  // 执行 SQL 语句失败了
  if (err) return console.log(err.message)
  // 成功了
  // 注意：如果执行的是 insert into 插入语句，则 results 是一个对象
  // 可以通过 affectedRows 属性，来判断是否插入数据成功
  if (results.affectedRows === 1) {
    console.log('插入数据成功!')
  }
}) 

// 演示插入数据的便捷方式
const user = { username: 'Spider-Man2', password: 'pcc4321' }
// 定义待执行的 SQL 语句
const sqlStr = 'insert into users set ?'
// 执行 SQL 语句
db.query(sqlStr, user, (err, results) => {
  if (err) return console.log(err.message)
  if (results.affectedRows === 1) {
    console.log('插入数据成功')
  }
})

//演示如何更新用户的信息
const user = { id: 6, username: 'aaa', password: '000' }
// 定义 SQL 语句
const sqlStr = 'update users set username=?, password=? where id=?'
// 执行 SQL 语句
db.query(sqlStr, [user.username, user.password, user.id], (err, results) => {
  if (err) return console.log(err.message)
  // 注意：执行了 update 语句之后，执行的结果，也是一个对象，可以通过 affectedRows 判断是否更新成功
  if (results.affectedRows === 1) {
    console.log('更新成功')
  }
})

 演示更新数据的便捷方式
 const user = { id: 6, username: 'aaaa', password: '0000' }
// 定义 SQL 语句
const sqlStr = 'update users set ? where id=?'
// 执行 SQL 语句
db.query(sqlStr, [user, user.id], (err, results) => {
  if (err) return console.log(err.message)
  if (results.affectedRows === 1) {
    console.log('更新数据成功')
  }
})

//删除 id 为 5 的用户
const sqlStr = 'delete from users where id=?'
db.query(sqlStr, 5, (err, results) => {
  if (err) return console.log(err.message)
  // 注意：执行 delete 语句之后，结果也是一个对象，也会包含 affectedRows 属性
  if (results.affectedRows === 1) {
    console.log('删除数据成功')
  }
})

// 标记删除
const sqlStr = 'update users set status=? where id=?'
db.query(sqlStr, [1, 6], (err, results) => {
  if (err) return console.log(err.message)
  if (results.affectedRows === 1) {
    console.log('标记删除成功')
  }
})
  
 ```

## 前后端身份认证

### web开发模式

1. 基于服务端渲染的传统web开发模式（客户端界面由服务器通过字符串动态生成）
   1. 前端耗时少
   2. 有利于SEO
   3. 占用服务器端资源
   4. 不利于前后端资源，开发效率低
2. 前后端分离的web开发模式(后端负责提供api接口，前端使用ajax接口）
   1. 开发体验好
   2. 用户体验好
   3. 减轻服务器端的渲染压力

### session身份认证

服务器渲染推荐

HTTP协议的无状态性：客户端的每次HTTP请求是独立的，服务器不会主动保留每次HTTP请求的状态

Cookie：存储在用户浏览器的大小不超过4kb的字符串，由一个名称，一个值，和其他几个用于控制cookie有效期，安全性，使用范围的可选属性。  
不同域名下的Cookie各自独立，当客户端发起请求时会自动把当前域名下的所有未过期的cookie一同发送到服务器  
cookie特性

1. 自动发送
2. 域名独立
3. 过期过时
4. 4kb限制
5. 不安全，容易被伪造
  
客户端第一次请求服务器的时候，服务器通过响应头的方式，向客户端发送一个身份认证的cookie，客户端会自动保存在浏览器中  
当客户端浏览器请求服务器时，浏览器会通过请求头的形式将身份认证相关的cookie发送给服务器，服务器可验明身份  

session的工作原理

1. 客户端登陆：提交账号密码
2. 服务器验证账号密码，将登录成功后的用户信息存储在服务器的内存中，同时生成对应的cookie字符串
3. 服务器响应：将生成的cookie响应给客户端
4. 浏览器自动将cookie保存在当前域名下
5. 客户端再次发送请求的时候，通过请求头自动把当前域名下所有可用的cookie发送给服务器
6. 服务器根据请求头中携带的信息从内存中找到对应的用户信息
7. 用户认证成功后，服务器针对当前用户生成特定的响应内容并响应

#### session中间件的使用

```JS

npm i express-session

// 导入 express 模块
const express = require('express')
// 创建 express 的服务器实例
const app = express()

// TODO_01：请配置 Session 中间件
const session = require('express-session')
app.use(
  session({
    secret: 'itheima',
    resave: false,
    saveUninitialized: true,
  })
)

// 托管静态页面
app.use(express.static('./pages'))
// 解析 POST 提交过来的表单数据
app.use(express.urlencoded({ extended: false }))

// 登录的 API 接口
app.post('/api/login', (req, res) => {
  // 判断用户提交的登录信息是否正确
  if (req.body.username !== 'admin' || req.body.password !== '000000') {
    return res.send({ status: 1, msg: '登录失败' })
  }

  // TODO_02：请将登录成功后的用户信息，保存到 Session 中
  // 注意：只有成功配置了 express-session 这个中间件之后，才能够通过 req 点出来 session 这个属性
  req.session.user = req.body // 用户的信息
  req.session.islogin = true // 用户的登录状态

  res.send({ status: 0, msg: '登录成功' })
})

// 获取用户姓名的接口
app.get('/api/username', (req, res) => {
  // TODO_03：请从 Session 中获取用户的名称，响应给客户端
  if (!req.session.islogin) {
    return res.send({ status: 1, msg: 'fail' })
  }
  res.send({
    status: 0,
    msg: 'success',
    username: req.session.user.username,
  })
})

// 退出登录的接口
app.post('/api/logout', (req, res) => {
  // TODO_04：清空 Session 信息
  req.session.destroy()
  res.send({
    status: 0,
    msg: '退出登录成功',
  })
})

// 调用 app.listen 方法，指定端口号并启动web服务器
app.listen(80, function () {
  console.log('Express server running at http://127.0.0.1:80')
})

```

session缺点  
需要配合cookie使用，默认不支持跨域访问，前后端跨域请求需要很多额外配置

### JWT身份认证

前后端分离推荐

工作原理

1. 客户端登陆：提交账号密码
2. 服务器验证账号密码，用户的信息对象经过加密后生成Token字符串
3. 服务器响应：将生成的token字符串响应给客户端
4. 将token存储到localstorage或sessionstorgae
5. 客户端再次发送请求的时候，通过请求头的Authorization把token发送给服务器
6. 服务器把token字符串还原成用户的信息对象
7. 用户认证成功后，服务器针对当前用户生成特定的响应内容并响应

组成部分（三者之间由.分割）

1. Header（头部）：安全性相关信息
2. Payload（有效荷载）:真正的用户信息
3. Signature（签名）：安全性相关信息

在express中使用JWT

```JS

//安装两个包（jsonwebtoken 生成JWT字符串 express-jwt将JWT字符串解析为JSON现象）
npm install jsonwebtoken express-jwt

// 导入 express 模块
const express = require('express')
// 创建 express 的服务器实例
const app = express()

// TODO_01：安装并导入 JWT 相关的两个包，分别是 jsonwebtoken 和 express-jwt
const jwt = require('jsonwebtoken')
const expressJWT = require('express-jwt')

// 允许跨域资源共享
const cors = require('cors')
app.use(cors())

// 解析 post 表单数据的中间件
const bodyParser = require('body-parser')
app.use(bodyParser.urlencoded({ extended: false }))

// TODO_02：定义 secret 密钥，建议将密钥命名为 secretKey
const secretKey = 'itheima No1 ^_^'

// TODO_04：注册将 JWT 字符串解析还原成 JSON 对象的中间件
// 注意：只要配置成功了 express-jwt 这个中间件，就可以把解析出来的用户信息，挂载到 req.user 属性上
app.use(expressJWT({ secret: secretKey }).unless({ path: [/^\/api\//] }))

// 登录接口
app.post('/api/login', function (req, res) {
  // 将 req.body 请求体中的数据，转存为 userinfo 常量
  const userinfo = req.body
  // 登录失败
  if (userinfo.username !== 'admin' || userinfo.password !== '000000') {
    return res.send({
      status: 400,
      message: '登录失败！',
    })
  }
  // 登录成功
  // TODO_03：在登录成功之后，调用 jwt.sign() 方法生成 JWT 字符串。并通过 token 属性发送给客户端
  // 参数1：用户的信息对象
  // 参数2：加密的秘钥
  // 参数3：配置对象，可以配置当前 token 的有效期
  // 记住：千万不要把密码加密到 token 字符中
  const tokenStr = jwt.sign({ username: userinfo.username }, secretKey, { expiresIn: '30s' })
  res.send({
    status: 200,
    message: '登录成功！',
    token: tokenStr, // 要发送给客户端的 token 字符串
  })
})

// 这是一个有权限的 API 接口
app.get('/admin/getinfo', function (req, res) {
  // TODO_05：使用 req.user 获取用户信息，并使用 data 属性将用户信息发送给客户端
  console.log(req.user)
  res.send({
    status: 200,
    message: '获取用户信息成功！',
    data: req.user, // 要发送给客户端的用户信息
  })
})

// TODO_06：使用全局错误处理中间件，捕获解析 JWT 失败后产生的错误
app.use((err, req, res, next) => {
  // 这次错误是由 token 解析失败导致的
  if (err.name === 'UnauthorizedError') {
    return res.send({
      status: 401,
      message: '无效的token',
    })
  }
  res.send({
    status: 500,
    message: '未知的错误',
  })
})

// 调用 app.listen 方法，指定端口号并启动web服务器
app.listen(8888, function () {
  console.log('Express server running at http://127.0.0.1:8888')
})

```
