# 正则表达式

```js

var 变量名 = RegExp(/表达式/);
var 变量名 = /表达式/;
//创建利用RegExp对象或字面量


var rg = /123/;//只要包含123为true
console.log(rg.test(1233));//true
console.log(rg.test(123));//true
console.log(rg.test('abc'));//false
//test(str)检测是否符合规则，符合返回true，不符合返回false

var rg = /^123$/;//只能与'123'匹配
//边界符 ^（开头边界符：以后面的字符开头） $(结尾边界符：以前面的字符结尾) 


var rg = /[^a-zA-Z0-9_-]/ //26个字母(无论大小写),数字,-，_ ,都不符合，[]中的^表示取反
// 字符类[]只要匹配其中一个就行



//量词符
var reg = /^a*$/
// * >=0

var reg = /^a+$/;
// + >=1

var reg = /^a?$/;
// ? 0||1

var reg = /^a{3}$/;
//{n} 就是重复n次   

var reg = /^a{3,}$/;
//{n,}大于等于n次

var reg = /^a{3,6}$/;
//{n,m}大于等于n次，并且小于等于m次

var reg = /^abc{3}$/; //让c重复三次
var reg = /^(abc){3}$/; // 让abc重复三次
// 小括号 表示优先级

//预定义类
// \d = [0-9]
// \D = [^0-9]
// \w = [A-Za-z0-9_]
// \W = [^A-Za-z0-9_]
// \s = [\t\r\n\v\f] (匹配空格)
// \S = [^\t\r\n\v\f] (匹配空格)    

var reg = /^\d{3}-\d{8}|\d{4}-\d{7  }$/ 
//三数字-八数字 或 四数字-七数字  | 或者

var str = 'andy和red和andy';
var newStr = str.replace('andy', 'baby')//'baby和red和andy'
//正则替换：str.replace(被替换的字符串或者正则表达式，替换问的字符串)

var newStr = str.replace('andy'g, 'baby')//'baby和red和baby'
//  正则表达式参数 / /[switch] 
//g：全局匹配 i：忽略大小写 gi：全局匹配和忽略大小写 

newStr.exec(str)
//将结果以数组返回，否则返回null

```
