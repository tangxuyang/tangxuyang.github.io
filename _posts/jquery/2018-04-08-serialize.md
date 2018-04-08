---
permalink: /jquery-serialize.html
layout: post
---

serialize用来把form表单、对象转换成name/value的数组，进而转换成query string。  

- 放开jquery.js中serialize的依赖
- 拷贝serialize.js
- 拷贝serialize依赖


### jQuery.fn.serializeArray()
根据jQuery对象中包含的form control生成name/value的数组。  当前jQuery对象可以是form或者form control的。

form control需要有name属性，不能是disabled的，必须是input、select、textarea、keygen这些元素，并且type不能是submmit、buttom、image、reset、file，如果是checkbox或radio必须是checked的。

这段代码里，jQuery用elements来获取form元素中的所有表单元素，可以记录下来，以后参考。

### jQuery.fn.serialize()
调用jQuery.fn.serializeArray获取表单元素对应的name/value数组，然后调用jQuery.param来把这个name/value数组转换成query string，即name=value&name=value....

### jQuery.param()

1. 把name/value数组转换成query string，只要参数是数组，就认为数组元素是包含有name和value字段的对象
2. 把jQuery对象转换成query string,认为jQuery对象中包含的元素都是有name和value的
3. 把普通对象，转换成query string。第二参数用来控制是否递归，默认是false，表示递归。其实第二个参数是traditional，传统方式就是不递归，因此false表示非传统，即递归！

