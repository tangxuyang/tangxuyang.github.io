---
layout:post
permalink: /jquery-basic-build.html
---

jQuery使用grunt作为构建工具，针对jQuery的构建做了深度的定制。在基本构建中我们只是把build/tasks/build.js引入进来，它的用处是把模块化开发的jQuery源码编译成一整个文件。

因为只引入了build/tasks/build.js所以gruntfile中的构建过程也要相应的修改一下，只保留build相关的任务，别的都先注释掉。

下面是所有的操作：
- 创建一个目录myjquery
- 创建子目录build、dist、src
- npm init -y 创建package.json文件
- 拷贝build/tasks/build.js过来
- 创建拷贝gruntfile.js过来，注释掉除了build任务以外的代码
- 拷贝入口文件src/jquery.js，jquery.js只是一个入口文件，并没有实质性的代码，它把jQuery的各个模块代码引入了，由于我们当前还没有引入任何模块代码，只是先把jQuery的基本构建功能迁移过来。因此，先把jquery.js中的require依赖数组给注释掉！
- 拷贝src/wrapper.js过来

我们还需要安装一些依赖的npm包，首先是要全局安装grunt-cli，然后是局部的grunt、load-grunt-tasks、requirejs、insight

{% highlight shell %
npm install -g grunt-cli
npm install --save-dev grunt load-grunt-tasks requirejs insight strip-json-comments
{% endhighlight %}}

至此基本构建已经完成了，我把它放到了[basic-build](https://github.com/tangxuyang/myjquery)中的basic-build分支了！

