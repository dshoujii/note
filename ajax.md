# ajax

## http协议

超文本传输协议

### 请求报文

``` txt
行  GET /s?wd=尚硅谷 rsv_spt=1 HTTP/1.1
头  :authority: pss.bdstatic.com
    :method: GET
    accept-encoding: gzip, deflate, br
空行
体  (GET请求请求体为空)
```

### 响应报文

```txt
行  HTTP/1.1  200(状态码)
头  Content-Type：
    Content-length：
    Content-ecoding：
空行
体  <html>
        <head>
        </head>
    </html>
```

## express

``` js
//打开服务
node 文件名
```

## 原生ajax操作

``` js
//创建对象
const xhr = new XMLHttpRequest();

//初始化 设置请求方法和url
xhr.open('GET/POST', 'url');

//设置2s超时
xhr.timeout = 2000;
xhr.ontimeout = () => {
     alert('超时');
}

//网络异常回调
xhr.onerror = () => {
     alert('网络出现问题');
}

//发送
xhr.send();

//时间绑定
xhr.onreadystatechange = function () {

//判断(服务器返回的结果)
if (xhr.readyState === 4) {

//判断响应状态码 200 404 403 401 500
if (xhr.status >= 200 && xhr.status < 300) {

//处理结果 行 头 空行 体

//响应行
console.log(xhr.status);//状态码
console.log(xhr.statusText)// 状态字符串
console.log(xhr.getAllResponseHeaders());//所有响应头
console.log(xhr.response)//响应体
 } else {
 }
}

//取消请求
xhr.abort(); xhr为let不为const


//取消重复发送
//1.在全局中添加标识变量

let isSending = false;

//2.在事件触发时
if(isSending) xhr.abort();
isSending = true;

//3.在xhr.readyState === 4  时

isSending = false;
```

## axios

安装与引用

```js

npm install axios
<script  crossorigin="anonymous" src="https://cdn.bootcdn.net/ajax/libs/axios/0.27.2/axios.js"></script>
```

get与post

```js
//post请求
axios({
    //请求方法
    method: 'post',
    //url
    url: 'http://127.0.0.1:8000/post',
    //url参数
    params: {
        vip: 10,
        level: 30
    },
    //头信息
    headers: {
        a: 100,
        b: 200
    },
    //请求体信息
    data: {
        username: 'admin',
        password: 'admin'
    }
}).then(response => {
    //响应
    console.log(response);
    //响应状态码
    console.log(response.status);
    //响应状态字符串
    console.log(response.statusText);
    //响应头信息
    console.log(response.headers);
    //响应体
    console.log(response.data);
})

axios({
    //请求方法
    method: 'get',
    //url
    url: 'http://127.0.0.1:8000/get',
    //url参数
    params: {
        vip: 10,
        level: 30
    },
    //头信息
    headers: {
        a: 100,
        b: 200
    }
}).then(response => {
    //响应
    console.log(response);
    //响应状态码
    console.log(response.status);
    //响应状态字符串
    console.log(response.statusText);
    //响应头信息
    console.log(response.headers);
    //响应体
    console.log(response.data);
})
