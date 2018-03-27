---
layout: post
---
### 简单介绍
我是在读《test-driven javascript development》时知道jstestdriver的。感觉还是挺好的。同时我一直以来都想把tdd的概念运用到自己的实际工作中，从而结束那种不敢修改的局面，也让自己的代码更健壮，更大胆的进行重构，毕竟重构的前提就是要有solid的测试作保证。  

jstestdriver主要是做js的单元测试的，以我的理解它应该只能做in-browser的js的单元测试。node环境的可能就用不上了，至少暂时我是这么认为的！它的思路跟别的js单元测试框架不一样，可谓另辟蹊径。它分为了服务器端和客户端，当然相对要复杂一点，对于运行环境也有要求，必须有java运行时，这个应该不难。

它的优点有很多，让我最喜欢的是，可以和CI工具集成（好吧，其实这个不是我最喜欢的）。我最喜欢的是，它在任何联网设备上的浏览器中运行测试，这样我们就能验证各种平台浏览器的JS兼容性问题，吊炸天！！

接着刚才的服务器和客户端话题说下去。
- 服务器，是一直开启的，作为一个center。当客户端进行测试时，会把测试的东西发送给服务器，服务器再把这些玩意转给所有跟服务器勾搭（capture，嘿嘿）上的浏览器，让这些浏览器去运行，浏览器运行完测试再把结果反馈给服务器，服务器再告诉客户端
- 客户端，运行测试

### 下载
地址：https://code.google.com/archive/p/js-test-driver/  
国内网络估计访问不到google的东西，可以在[这里](/attachments/jstestdriver-1.3.5.jar)下载  
这是最新版的了，而且已经好久没有更新过了，不过满足我当前需求是没问题的呢。

其实我总感觉google是不是太低调了，这么好的思路的测试框架为什么不能给个单独的官网，非得放在archive下，天呀，这是暴殄天物吗？

### 使用
#### 启动服务器端
服务器端作为中转站，要一直开启的。
{% highlight shell%}
java -jar %JSTESTDRIVER% --port 8899
{% endhighlight %}

我这个是在windows下运行的，请保证有java环境。同时我把jstestdriver的路径配成了环境变量JSTESTDRIVER，这样用起来比较方便，端口随便你！

以后就不用动服务器端了，就让他自己玩耍吧。

#### 勾搭（capture）浏览器
服务器端需要有浏览器跟它连接，才能把客户端提交的测试分发给浏览器去进行实地运行的。因此要开启你想在其中测试的浏览器http://localhost:8899，这个是本机浏览器访问的地址，如果你想在非本机环境下测试别的浏览器，比如手机上的浏览器，请保证手机能访问到jstestdriver服务器端运行的机器，常见情况是它们在一个局域网里面，然后在该浏览器中访问http://xx.xx.xx.xx:8899。进入一个页面，点击capture链接，就跟服务器勾搭上了。这步算是注册浏览器到服务器端，听候派遣！

#### 编写测试
{% highlight js %}
TestCase("test", {
	"test array length": function(){
		var arr = [1, 2, 3, 4];
		assertEquals(4, arr.length);
	}
});
{% endhighlight %}

TestCase和assertEquals都是jstestdriver暴露给我们用的。  
我好像忘记说jstestdriver的配置了。每个测试项目都需要有一个jsTestDriver.conf的配置文件在根目录里。

{% highlight yml %}
server: http://localhost:8899
load:
  - src/*.js
  - test/*.js
{% endhighlight %}

请把上面写的测试代码放到test目录下。

然后命令行切换到根目录，运行:java -jar %JSTESTDRIVER% --test all。这样就能测试了

我把jstestdriver的文档拷贝了一份放到了pdf中，可以[参考](/attachments/jstestdriver-doc.pdf)

### 跟kenjins集成
这个先放放，还没有实践呢
TODO:::