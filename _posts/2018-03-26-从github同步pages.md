---
layout: post
comments: true
---

总结一下我是怎么把github pages同步到自己的服务器上的。我也不知道我为什么要把github pages同步到自己的服务器上，或许只是单纯的想再熟悉一下jenkins和jekyll吧！  

先说一下思路，使用jenkins监听github提交，拉取代码到服务器，执行jekyll build，来编译jekyll项目。  

前提条件:
服务器上装了jenkins,jekyll。同时因为github pages上用了模板，因此可能还需要在自己的服务器上安装对应的模板，我用的是jekyll-theme-architect
{% highlight ruby%}
gem install jekyll-theme-architect
{% endhighlight %}

/var/lib/gems/2.3.0/gems/jekyll-theme-architect-0.1.0，这是路径，如果想改模板可以到这里面去修改。

看来还可以写几篇关于安装jenkins和jekyll的文章！  

github上的每个库在设置里面都有一个webhooks的选项，只有给这个库配置了webhook，当代码提交时github才能通知到我们的jenkins服务。这里可以看出来，用的是推模式，而不是让jenkins轮巡的拉模式。还要配置jenkins的监听服务，就是图中的payload。  

![](/images/2018-03-27_webhooks01.png)  
![](/images/2018-03-27_webhooks02.png)

然后就是在jenkins中创建一个新的项目了！我这里选择了自由风格的类型。  
![](/images/2018-03-27_jenkins01.png)  
![](/images/2018-03-27_jenkins02.png)


至此，就弄完了！

可以编辑一下文件，提交上去测试一下了！(2018-03-26)

这两天重新熟悉Jenkins，发现上面的过程会有一个问题，当然不影响使用，问题是，会从GitHub拉两次代码，这不是浪费人家GitHub的资源吗？好吧，我承认，主要是国内网络太差，我不想等那么久。可以把【Execute Shell】中的git pull干掉，最终版本是:
{%highlight shell%}
jekyll build -d /home/username/projectname/_site
{%endhighlight%}
我们是借助jekyll build的d选项，设置了目的文件夹。(2018-03-29)

