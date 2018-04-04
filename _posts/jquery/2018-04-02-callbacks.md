---
permalink: /jquery-callbacks.html
layout: post
---

真的很惊讶于jQuery的强大，短短几百行代码就能创造出一个callback模块！  

callbacks是一个相对独立的模块，即使你想把它拿出来单独编译也是很容易的。  

正如它的名字一样，它就是提供回调功能的模块，让我们可以很容易管理回调列表！

我们都知道jQuery参见一般有两种，一种是jQuery.fn上的扩展，这些方法因为放到了jQuery的原型上，因此是相当于jQuery对象的实例方法。另一种是jQuery上的扩展，就相当于类方法。callback就是jQuery上扩展的，它本身是一个构造函数，可以用它来构造一个callback对象，用来管理回调列表。


- jquery.js中放开对callbacks的依赖
- 拷贝src/callbacks.js
- 拷贝callbacks.js的依赖src/var/rnothtmlwhite.js

### jQuery.Callbacks
构造函数，创建一个回调列表对象。

构造器接收一个可选的字符串，传入空格分割的flags，用来指定回调列表的行为。支持的flag有：
- once 回调列表只执行一次，多次fire也无效的
- memory 有记忆功能，当add一个回调时会根据上次fire的值调用这个回调
- unique 保证一个回调函数只能add一次
- stopOnFalse 当某个回调返回了false，停止接下来的回调的调用

### add()
添加回调函数，参数可以是一个函数，也可以是一个函数数组

### fire()
触发回调，参数会被传递给回调列表中所有回调

### fired()
判断是否被调用过

### fireWith()
指定执行上下文

### remove()
移除指定的函数从回调列表中

### disable()

### disabled()

### empty()
清空回调列表

### has
判断回调列表中是否有指定的函数，如果参数为空就判断回调列表是否有函数。

### lock()

### locked()


### 源码
#### 私有函数createOptions
这个函数是为了解析jQuery.Callbacks构造传入的字符串参数而存在的。它把字符串参数用空格分开，创建一个对象，是一个键值对，键就是字符串中的各个单词，值就是true。

比如"memory unique"会被转换成{memory: true, unique: true}。

思路还是很巧妙的，值得学习一下。

并且这里用到的不是我们常用的"".split(' ')，而是用的正则。支持单词之间任意多个空白！

