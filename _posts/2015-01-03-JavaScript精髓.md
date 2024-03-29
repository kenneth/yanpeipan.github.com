---
layout: post
title: "JavaScript精髓"
description: ""
category: 
tags: [JavaScript]
---
{% include JB/setup %}

# JavaScript基础

## JavaScript能做什么?

现在JavaScript能做的事情已经非常多了：

* 图形处理
* PDF生成
* 建立服务器
* 编译解释器
* 图形界面
* 数据库
* 各种测试工具
* 视频和音频播放和处理
* 通信
* 多人协作

* 做DNA分析：genome.js
* 在浏览器上模拟出一个Linux系统：JsLinux
* 在浏览器上模拟出一个Git版本控制系统：JS-Git
* 制作动画：Processing.js
* 物理效果模拟引擎：verlet-js
* 可编辑的在线发票生成器：Invoice
* 在线待办事宜列表：Todo List
* 为图片加效果(需要HTML5 canvas支持)：blend.js

当然JavaScript是作为脚本语言存在的，所以基本上它只是调用现有的底层API，也就是用其他低级语言所编写的模块，而JavaScript任务就是调用这些API去处理实际的问题。

## JavaScript由哪几部分组成？

尽管 ECMAScript 是一个重要的标准，但它并不是 JavaScript 唯一的部分，当然，也不是唯一被标准化的部分。实际上，一个完整的 JavaScript 实现是由以下 3 个不同部分组成的：

* 核心（ECMAScript）
* 文档对象模型（DOM）
* 浏览器对象模型（BOM）

## 什么是ECMAScript？

> ECMAScript是一种由Ecma国际（前身为欧洲计算机制造商协会）通过ECMA-262标准化的脚本程序设计语言。这种语言在万维网上应用广泛，它往往被称为JavaScript或JScript，但实际上后两者是ECMA-262标准的实现和扩展。

> ECMAScript 是标准化组织 ECMA（Ecma International - European association for standardizing information and communication systems）发布的脚本语言规范。现在大家常见的 JavaScript、微软的 JScript 以及 Adobe 的 ActionScript 等语言都是遵循这个规范的，属于 ECMAScript 语言的变体。每个 ECMAScript 规范的变体语言都可能增加自己额外的功能特性。

## 各浏览器执行的ECMAScript版本是什么情况？

许多程序，尤其是网页浏览器支持ECMAScript。浏览器中的ECMAScript实现添加了与文档对象模型的接口，可以通过脚本改变网页的内容、结构和样式。

## `parseInt('08')`在什么情况下等于0,什么情况下等于8？

`'8' is not an octal digit.`
ECMAScript 5 规范中 parseInt 函数部分不在允许实现环境把以 0 字符开始的字符串作为八进制数值了。

## setTimeout方法，如果第二个参数是0, 那么是立即执行吗？

    setTimeout( function(){ while(true){} } , 100);
    setTimeout( function(){ alert(’你好!’); } , 200);
    setInterval( callbackFunction , 200);

setTimeout is simply like calling the funcion after delay has finished. Whenever a function is called it is not executed immediately, but queued so that it is executed after all the executing and currently queued eventhandlers finish first. setTimeout(,0) essentially means execute after all current functions in the present queue get executed. No guruantees can be made about how long it could take.

setImmediate is similar in this regard except that it doesn't use queue of functions. It checks queue of I/O eventhandlers. If all I/O events in the current snapshot are processed, it executes the callback. It queues them immedieately after the last I/O handler somewhat like process.nextTick. So it is faster.

> JavaScript引擎是单线程运行的,浏览器无论在什么时候都只且只有一个线程在运行JavaScript程序.

> 如果队列非空,引擎就从队列头取出一个任务,直到该任务处理完,即返回后引擎接着运行下一个任务,在任务没返回前队列中的其它任务是没法被执行的.

## switch（变量），变量和case语句当中出现的值等于===还是==？

==用来判断两个值是否相等。当两个值类型不同时，会发生自动转换，得到的结果非常不符合直觉。

    "" == "0" // false
    0 == "" // true
    0 == "0" // true
    false == "false" // false
    false == "0" // true
    false == undefined // false
    false == null // false
    null == undefined // true
    " \t\r\n" == 0 // true

## Undefined和Null的区别是什么？

1. null表示"没有对象"，即该处不应该有值。典型用法是：
* 作为函数的参数，表示该函数的参数不是对象。
* 作为对象原型链的终点。
2. undefined表示"缺少值"，就是此处应该有一个值，但是还没有定义。典型用法是：
* 变量被声明了，但没有赋值时，就等于undefined。
* 调用函数时，应该提供的参数没有提供，该参数等于undefined。
* 对象没有赋值的属性，该属性的值为undefined。
* 函数没有返回值时，默认返回undefined。

## 什么情况下会产生意外的全局变量？

1. 忽略了关键字`var` （误写）
2. 隐式全局变量 `var a = b = 3;` >> `var a = ( b = 3)`
3. 函数没被调用

    var obj = {
      doSomething:function() {
        var a = "bob";
        console.log(this);
        (function() {
          console.log(this);
          console.log(a);
        }());
      }
    }
    obj.doSomething()

## 如何遍历数组？

    var x
    var mycars = new Array()
    mycars[0] = "Saab"
    mycars[1] = "Volvo"
    mycars[2] = "BMW"
    for (x in mycars)
    {
    document.write(mycars[x] + "<br />")
    }

## 在进行比较false=={}时，类型的转换过程是怎样的？

比较运算x==y, 其中x和 y是值，产生true或者false。这样的比较按如下方式进行：

* 若Type(x)与Type(y)相同， 则
* 若Type(x)为Undefined， 返回true。
* 若Type(x)为Null， 返回true。
* 若Type(x)为Number， 则返回false。
* 若x为NaN， 返回false。
* 若y为NaN， 返回false。
* 若x与y为相等数值， 返回true。
* 若x 为 +0 且 y为−0， 返回true。
* 若x 为 −0 且 y为+0， 返回true。
* 
* 若Type(x)为String, 则当x和y为完全相同的字符序列（长度相等且相同字符在相同位置）时返回true。 否则， 返回false。
* 若Type(x)为Boolean, 当x和y为同为true或者同为false时返回true。 否则， 返回false。
* 当x和y为引用同一对象时返回true。否则，返回false。
* 若x为null且y为undefined， 返回true。
* 若x为undefined且y为null， 返回true。
* 若Type(x) 为 Number 且 Type(y)为String， 返回comparison x == ToNumber(y)的结果。
* 若Type(x) 为 String 且 Type(y)为Number，
* 返回比较ToNumber(x) == y的结果。
* 若Type(x)为Boolean， 返回比较ToNumber(x) == y的结果。
* 若Type(y)为Boolean， 返回比较x == ToNumber(y)的结果。
* 若Type(x)为String或Number，且Type(y)为Object，返回比较x == ToPrimitive(y)的结果。
* 若Type(x)为Object且Type(y)为String或Number， 返回比较ToPrimitive(x) == y的结果。
* 返回false。

## 如果某个外部引用的Js文件出现运行时错误，后面的脚本还可以执行吗？

## 全局变量有什么弊端？

> 一般创建全局变量都认为是比较糟糕的实践，尤其是在团队开发的大背景下更是问题多多。随着代码量的增长，全局变量会导致一些非常重要的可维护性你难题。全局变量越多，引入错误的概率就越高。

> 当脚本中的全局变量越来越多时，也容易发生命名冲突，即可能无意间就使用了一个已经声明的变量名字。所有的变量都被定义为局部变量，这样代码才是最好维护的。

## 什么是严格模式？

> ECMAScript 规范第五版的一个重要新特性是引入了代码执行时的严格模式。在严格模式下，对于 ECMAScript 代码执行时的限制更多。某些使用方式在严格模式下是不允许的。这有利于避免一些潜在的问题，提高代码的鲁棒性。一般来说，框架需要可以在严格模式下能正确运行，而一般的应用程序则可以选择是否使用严格模式。通过在 ECMAScript 代码的最开始使用 "use strict" 或 'use strict' 就可以声明这段代码需要运行在严格模式下。

## 哪个值不等于自己？

`NaN`

---

# 函数

## 如何实现函数参数的默认值？

## 如何实现函数的链式调用？

## 函数的参数是值传递还是引用传值？

## 什么是即时函数（立即调用的函数表达式）？

## 什么情况下new构造函数，得到的对象不是该构造函数的实例？

## 什么情况下对象的方法执行时this不能指向该对象本身？

---

# 参考

[深入探讨 ECMAScript 规范第五版](http://www.ibm.com/developerworks/cn/web/1305_chengfu_ecmascript5/)\\
[Javascript 严格模式详解](http://www.ruanyifeng.com/blog/2013/01/javascript_strict_mode.html)\\
[parseInt](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/parseInt)\\
[javascript线程解释（setTimeout,setInterval你不知道的事）](http://www.iamued.com/qianduan/1645.html)\\
[undefined与null的区别](http://www.ruanyifeng.com/blog/2014/03/undefined-vs-null.html)\\
[意外造成全局变量的误区](https://prezi.com/0nvpargkptwc/presentation/)\\
[Javascript的10个设计缺陷](http://www.ruanyifeng.com/blog/2011/06/10_design_defects_in_javascript.html)\\
