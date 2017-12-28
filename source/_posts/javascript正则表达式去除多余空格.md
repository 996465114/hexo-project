---
title: javascript正则表达式去除多余空格
date: 2017-12-25 19:59:18
tags:
---

现有一段字符串，字符串穿插着空格，空格可能在字符串的头部、尾部或者在中间，要求去除字符串中间的空格，首尾空格保留，例如：
```
'  a b  a ' 处理后变为 '  aba '

```

<br />
__按照以前的做法，看到这种需求，第一反应就是使用javascript原生方法处理字符串__。
```javascript
let arr = str.split(/\w/),
    len = arr.length,

    tempStr = str.replace(/\s*/g, '');

console.log(`${arr[0]}${tempStr}${arr[len - 1]}`);
```

先将字符串基于字母分隔成不同长度的字符串数组，这样就可以取得字符串首尾的空格，然后保存在`arr`里，再将原本的字符串所有空格去掉，最后拼接首尾空格就得到想要的效果了。

<!-- more -->
<br />
__上面的做法，太多中间变量，占用存储容量也大。如果使用正则表达式处理会更加简洁__。
```javascript
str.replace(/([^\s])\s*(?=[^\s])/g, '$1');
```

一行代码就够了，哈哈，果然更加简洁了。
如果要保留首尾空格，那要捕捉的字符串模式无外乎 （字母 + 空格 + 字母）或者 （字母 + 多个空格 + 字母）, 捕捉到这个的模式之后就可以轻松的去除中间的空格。用`不捕获匹配`匹配可以轻松的匹配出这样的模式，然后将`([^\s])\s`替换成[^\s]， 这里[^\s]表示非空格。

正则表达式写起来简洁，并且性能更好。至于正则比原生方法更快捷的说法，空口无凭，我做了一个demo测试了两者的性能。
```javascript
var str = ' ',
    arr = [' ', 'a', 'b', 'ab'];
// 随机生成N个字符串
for (let i = 0; i <= 10; i++) {
    str += arr[~~(Math.random() * 4)];
}

str = str + ' '; // 生成的字符串首尾至少都有一个空格

function method1 (str) {
    let arr = str.split(/\w/),
        len = arr.length,

        tempStr = str.replace(/\s*/g, '');

    return `${arr[0]}${tempStr}${arr[len - 1]}`;
}

function method2 (str) {
    return str.replace(/([^\s])\s*(?=[^\s])/g, '$1');
}

console.time('method1');
console.log(method1(str));
console.timeEnd('method1');

console.time('method2');
console.log(method2(str));
console.timeEnd('method2');
```

`(基于chrome)`，上面for循环的i分别使用了10，100，1000，10000打印后时间如下

* i = 10
method1: 1.42724609375ms
method2: 0.115966796875ms

* i = 100
method1: 1.708251953125ms
method2: 0.171142578125ms

* i = 1000
method1: 1.602783203125ms
method2: 0.216064453125ms

* i = 10000
method1: 5.887939453125ms
method2: 0.8408203125ms

__对比后，发现正则耗费的时间更少，性能更优__。