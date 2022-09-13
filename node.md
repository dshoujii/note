# FS模块

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






