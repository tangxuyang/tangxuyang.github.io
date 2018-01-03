---
layout: post
title: 读CSS - the Definitive Guide
tags: ['reading note','pendding','web']
---

### 重读
这本书我读过大部分，因为有的内容跳过去了，所以说是读了大部分。这次重读估计也还是大部分，总有写内容是我不感兴趣的！  

### 概览
CSS能够把文档的结构跟展现分离，这样便于维护。CSS提供了比HTML更丰富的文档外观，也更省时间，你可以只修改一个地方能能改变整个文档的外观。紧凑的文件大小使得加载更快！  


### CSS和文档
#### 单词时间
- obstensible, obstensibly 表面上的
- catch on 变得流行
- discrepancy 矛盾，不符合
- derail （使）出轨
- monolithic 整体的，庞大的

自从CSS2以后，CSS的标准就不在是整体的了，它被分成了好多模块，因此实际上并没有人们口中所说的CSS3，因为从CSS3（原谅我说CSS3）开始CSS的各个组成部分就分模块独立版本发布了。  

#### 替换元素和非替换元素
img和input都是替换元素  
大部分元素还都是非替换元素
#### 元素显示角色
块级和内联级  
还有别的显示类型，这两个是最基本的。  

块级元素会创建一个元素盒子，这个盒子会填充它父元素的内容区域，并且别的元素不能在它旁边。列表项是一个特殊的块级元素，除了跟块级元素的行为一致，它们还生成一个标注。  

我们用display来指定元素的显示角色  

#### 把CSS和HTML放在一起
{% highlight html %}
<link rel="stylesheet" href="" type="text/css"></link>
<style type="text/css">
@import url();
</style>
{% endhighlight %}
