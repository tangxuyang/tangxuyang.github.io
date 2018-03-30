---
layout: post
---

其一为了练习Jenkins使用，其二因为国内访问国外网络有问题，这其中当然包括了bootstrap的官网，因此把bootstrap的文档搞一份在自己服务器上也是有用的，速度应该会快点。

### 创建自由风格的任务

在【源码管理】填写bootstrap的github地址：https://github.com/twbs/bootstrap.git 

然后在构建中选择"Execute shell"
{%highlight shell%}
jekyll build -d 目的目录
{%endhighlight%}

这样就行了，我只是用来构建，并不是想监听bootstrap的提交之后构建，这个构建是我手动触发的。

理想是丰满的，现实是骨干的。我最终还是放弃了，因为网络真的太慢了，此时要全部Git clone下来全部的bootstrap项目要300M，根本就是不可能的事情，即使使用了spare checkout也还要80多M，算啦，有些无力！！我还是拷一份bootstrap源码，然后自己手动吧，不用jenkins。


注：这个要求机器上装了jekyll