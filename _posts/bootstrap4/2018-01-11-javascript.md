---
layout: post
---

bootstrap除了提供样式，也提供了一些常用的交互脚本。这些插件是标准的jQuery插件。可以全部引入，或者引入单个。bootstrap的脚本依赖jQuery和popper。 


所有的bootstrap插件都可以使用data属性来开启和配置，意思是说，不用写任何的脚本就能用bootstrap插件，只需要在元素的data属性中配置插件需要的内容，厉害了我的哥！不过也有不需要开启这个功能的时候，使用以下代码来关闭这个功能。
{% highlight javascript %}
$(document).off('.data-api')
{% endhighlight %}

或者也可以关闭某个组件的data attribute api，如下
{% highlight javascript %}
$(document).off('.alert.data-api');
{% endhighlight %}


### 事件
bootstrap插件都有很多事件钩子，让我们可以在适当的时候执行自定义的操作。这些事件一般都是成对出现的，使用不定式和过去分词的形式。不定时表示动作执行前，过去分词表示动作执行后。类似pre和post。所有的不定式事件都提供一个preventDefault来阻止接下来的事件的执行。

{% highlight javascript %}
$('#myModal').on('show.bs.modal', function(e){
    return e.preventDefault();
});
{% endhighlight %}

这就阻止了对话框的显示！！！  


除了可以使用data attribute api来定制插件，完全可以用编程的方式来使用插件。首先每个插件都有一个名字，比如按钮button，弹框modal。调用插件的API很简单，都是调用插件名方法，比如.button(),要调用的api作为第一个参数，如果这个api需要额外的参数，就作为第二个参数！！  

插件方法可以接受的参数有三种：1. 空 2. 对象 3. 字符串

每个插件都有一些默认的配置，比如$.fn.modal.Constructor.Default  

