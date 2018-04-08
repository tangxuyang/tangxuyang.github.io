---
permalink: /jquery-access.html
layout: post
---

这是一个帮助方法，源代码中好多地方都使用了，是一个私有函数，jQuery以外是访问不到的。

它可以用来设置/获取集合的值。

参数：
- elems 集合元素
- fn
- key 要设置/获取的键
- value 要设置/获取的值
- chainable 是否可连缀
- emptyGet 获取时的默认值
- raw 是否原始格式

### Get
当key不是对象，且value没有值时，认为是Get。  
- 当key为null时，没有指定键，就是批量获取，会以elems为上下文来调用fn作为返回值
- 当key有值，会以elems[0]和key作为参数调用fn，如果elems的长度为0则直接返回emptyGet

Get还是很简单的，分为批量Get和单个Get。

### Set
当key为对象或value有值，认为是Set
- key是对象，则遍历key，分别调用access  
- key有非对象值
1. value不是函数，遍历elems，以elems[i],key,value为参数调用fn
2. value是函数，遍历elems，以elems[i],key,value.call(elems[i],i, fn(elems[i],key))为参数调用fn
- key为null
1. value不是函数，以elems和value为参数调用fn
2. value是函数，....



