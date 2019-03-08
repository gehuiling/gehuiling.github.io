---
layout: post
title:  "Windows环境下 Jekyll + Github 搭建个人博客"
date:   2019-03-01 04:09:00
categories: [博客]
tags: [Jekyll,Github]
comments: true
---
&#160;&#160;&#160;&#160;作为开篇博客的话，那就来说说如何利用Jekyll搭建个人博客的。
  
#### Jekyll介绍
&#160;&#160;&#160;&#160;Jekyll 是一个静态站点生成器。它提供了内容管理系统（CMS）的一些优点，同时避免了数据库驱动站点引入的性能和安全问题。[Jekyll中文文档](http://jekyll.bootcss.com/)、[Jekyll英文文档](https://jekyllrb.com/)。
*   &nbsp;通过Markdown（或者 Textile）以及 Liquid 转化成一个完整的可发布的静态网站
*   &nbsp;可以运行在 GitHub Page 上，使用 GitHub 的服务来搭建项目页面、博客或者网站，而且完全免费
*   &nbsp;无需数据库
*   &nbsp;扩展评论、分享等功能

#### 环境配置
&#160;&#160;&#160;&#160;&#160;&#160;**1.&nbsp;&nbsp;注册Github账号并创建仓库**  **[^1]**<br />
&#160;&#160;&#160;&#160;&#160;&#160;**2.&nbsp;&nbsp;下载最新版Git，安装Git环境**  **[^2]**<br />
&#160;&#160;&#160;&#160;&#160;&#160;**3.&nbsp;&nbsp;下载rubyinstaller、DevKit，安装Ruby和Devkit**  **[^3]**<br />
&#160;&#160;&#160;&#160;&#160;&#160;**4.&nbsp;&nbsp;配置Jekyll环境**

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;安装 jekyll，在命令行窗口输入命令 `gem install jekyll`

```     
$ gem install jekyll     
```  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;新建博客，在命令窗口输入 `jekyll new geblog`，会在当前目录下创建 geblog 站点的文件目录。

```     
$ jekyll new geBlog
```   

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;进入博客目录,即 geblog 文件夹目录下

```     
$ cd geBlog
```

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;启动本地服务，执行 `jekyll serve`

```     
$ jekyll serve
```  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;浏览器里输入： [**http://localhost:4000**](http://localhost:4000) ，就可以看到默认的博客页面。

![](/img/posts/image1.png)

### 目录结构
　
Jekyll 的核心其实是一个文本转换引擎。它的概念其实就是： 你用你最喜欢的标记语言来写文章，可以是 Markdown，也可以是 Textile,或者就是简单的 HTML, 然后 Jekyll 就会帮你套入一个或一系列的布局中。在整个过程中你可以设置URL路径, 你的文本在布局中的显示样式等等。这些都可以通过纯文本编辑来实现，最终生成的静态页面就是你的成品了。

&#160;&#160;&#160;&#160;一个基本的 Jekyll 网站的目录结构一般是像这样的：

```
.
├── _config.yml
├── _includes
|   ├── footer.html
|   └── header.html
├── _layouts
|   ├── default.html
|   ├── post.html
|   └── page.html
├── _posts
|   └── 2019-03-01-welcome-to-jekyll.markdown
├── _sass
|   ├── _base.scss
|   ├── _layout.scss
|   └── _syntax-highlighting.scss
├── about.md
├── css
|   └── main.scss
├── feed.xml
└── index.html

```

&#160;&#160;&#160;&#160;这些目录结构以及具体的作用可以参考 [官网文档](http://jekyll.com.cn/docs/structure/) 

&#160;&#160;&#160;&#160;进入 _config.yml 里面，修改成你想看到的信息，重新 jekyll server ，刷新浏览器就可以看到你刚刚修改的信息了。

&#160;&#160;&#160;&#160;到此，博客初步搭建就完成啦。

### 建立Github Page 

&#160;&#160;&#160;&#160;有两种方法：一种是自己学习 Jekyll 和前端知识部署独一无二的博客，而对于我们大多数人来说套用模版搭建模版会容易的多，所以我以  **套用模板** 讲解：

*   &nbsp;**在[Jekyll主题列表](http://jekyllthemes.org/)里选择自己喜欢的模板，下载、解压到本地**

![](/img/posts/image2.png)

*   &nbsp;**新建一个github仓库**

&#160;&#160;&#160;&#160;仓库名称必须为：(github用户名).github.io，比如我的github用户名为 **[gehuiling](https://github.com/gehuiling)**，我的 github 仓库名就叫 **[gehuiling.github.io](https://github.com/gehuiling/gehuiling.github.io)**

*   &nbsp;**克隆github的远程仓库到本地文件仓库夹**

&#160;&#160;&#160;&#160;在本地新建一个文件夹blog，在文件夹内打开git bash，运行命令

```
$  git clone https://github.com/{username}/{username}.github.io.git     // 用你的Github用户名替换{username}
```
&#160;&#160;&#160;&#160;可以看到文件夹内多了文件夹(github的仓库名).io.

&#160;&#160;&#160;&#160;进入克隆到本地的仓库文件夹即文件夹(github的仓库名).io，把刚才下载好的模版文件夹的内容全部拷贝进来，执行命令：

```
$  git add .
$  git commit -a -m "first commit"
$  git remote add origin https://github.com/(github用户名)/(github仓库名).git
$  git push -u origin master
```
&#160;&#160;&#160;&#160;提交成功后再刷新github仓库就会和本地仓库一样的内容。

*   &nbsp;**在浏览器地址中输入你刚新建的github仓库名yourname.github.io,跳转到和刚选择模版同样的站点网页。OK，和模板一样的属于自己的博客搭建完成！**

### 自定义博客样式

*   &nbsp;**模板博客里面的信息并不是所要的，我们现在需要进行修改，使其真正称为自己的个性化博客**

&#160;&#160;&#160;&#160;使用编辑器打开本地仓库中的 _config.yml 文件，按照里面的注释修改为自己的内容。

![](/img/posts/image3.png)

*   &nbsp;**新建文章**

&#160;&#160;&#160;&#160;Jekyll 所有的文章都是 _posts 目录下面，文章格式为 mardown 格式，文章文件名可以是 .mardown 或者 .md。

&#160;&#160;&#160;&#160;编写一篇新文章很简单，你可以直接从 _posts/ 目录下复制一份出来 `2019-03-01-welcome-to-jekyll副本.md` ，修改名字为 2019-03-01-Jekyll_Blog.md ，注意：文章名的格式前面必须为 2019-03-01- ，日期可以修改，但必须为 年-月-日- 格式，后面的 Jekyll_Blog 是整个文章的连接 URL，建议文章名最好是英文的或者阿拉伯数字。 双击 2019-03-01-Jekyll_Blog.md 打开

```
---
layout: post
title:  "Windows环境下 Jekyll + Github 搭建个人博客"
date:   2019-03-01 04:09:00
categories: [博客]
tags: [windows,Jekyll,Github]
comments: true
---

正文...

```
&#160;&#160;&#160;&#160;title: 显示的文章名   

&#160;&#160;&#160;&#160;date:  显示的文章发布日期 

&#160;&#160;&#160;&#160;categories: 文章分类  

&#160;&#160;&#160;&#160;tags: 文章标签分类 

&#160;&#160;&#160;&#160;comments: 是否支持评论     

&#160;&#160;&#160;&#160;注意：文章头部格式必须为上面的，.... 就是文章的正文内容。我写文章使用的是 Visual Studio Code 编辑器，[Markdown语法简单易学](http://www.appinn.com/markdown/)。

&#160;&#160;&#160;&#160;然后使用 git push 到你自己的仓库里面去，检查你远端仓库，在浏览器输入 username.github.io 就可以看到修改的信息以及添加的文章。当然，除了上述基本的修改，你还可以继续添加自定义的功能与样式，那就看你的表演了。

&#160;&#160;&#160;&#160;OK，现在你拥有自己的博客啦！

&#160;&#160;&#160;&#160;但。。。。。。别急，我还没说完，你还要继续看嘛，嘻嘻。

### TIPS
*   &nbsp;**开启本地实时预览修改内容**

&#160;&#160;&#160;&#160;每次修改与更新博客的内容就push一次的话比较麻烦，而且有些修改在push到仓库之后然后显示可能不太理想，这时候还要继续修改。这时候，打开windows系统命令行，输入以下代码：
```
$ cd {local repository} // {local repository}替换成你的本地仓库的目录
$ jekyll serve
```

*   &nbsp;**运行 jekyll serve 可能出现的错误及处理方法**

&#160;&#160;&#160;&#160;1.addressable相关提示

```
C:\DevKit\geblog>jekyll serve
E:/Ruby233/Ruby23-x64/lib/ruby/gems/2.3.0/gems/bundler-2.0.1/lib/bundler/runtime.rb:319:in `check_for_activated_spec!': You have already activated addressable 2.6.0, but your Gemfile requires addressable 2.5.2. Prepending `bundle exec` to your command may solve this. (Gem::LoadError)
        from E:/Ruby233/Ruby23-x64/lib/ruby/gems/2.3.0/gems/bundler-2.0.1/lib/bundler/runtime.rb:31:in `block in setup'
        from E:/Ruby233/Ruby23-x64/lib/ruby/gems/2.3.0/gems/bundler-2.0.1/lib/bundler/spec_set.rb:148:in `each'
        from E:/Ruby233/Ruby23-x64/lib/ruby/gems/2.3.0/gems/bundler-2.0.1/lib/bundler/spec_set.rb:148:in `each'
        from E:/Ruby233/Ruby23-x64/lib/ruby/gems/2.3.0/gems/bundler-2.0.1/lib/bundler/runtime.rb:26:in `map'
        from E:/Ruby233/Ruby23-x64/lib/ruby/gems/2.3.0/gems/bundler-2.0.1/lib/bundler/runtime.rb:26:in `setup'
        from E:/Ruby233/Ruby23-x64/lib/ruby/gems/2.3.0/gems/bundler-2.0.1/lib/bundler.rb:107:in `setup'
        from E:/Ruby233/Ruby23-x64/lib/ruby/gems/2.3.0/gems/jekyll-3.8.5/lib/jekyll/plugin_manager.rb:50:in `require_from_bundler'
        from E:/Ruby233/Ruby23-x64/lib/ruby/gems/2.3.0/gems/jekyll-3.8.5/exe/jekyll:11:in `<top (required)>'
        from E:/Ruby233/Ruby23-x64/bin/jekyll:22:in `load'
        from E:/Ruby233/Ruby23-x64/bin/jekyll:22:in `<main>'
```

&#160;&#160;&#160;&#160;解决办法

```
C:\DevKit\geblog>gem uninstall addressable

$ Select gem to uninstall:
 1. addressable-2.5.2
 2. addressable-2.6.0
 3. All versions
> $ 2
Successfully uninstalled addressable-2.6.0
```

2.liquid相关提示

```
C:\DevKit\geblog>jekyll serve
E:/Ruby233/Ruby23-x64/lib/ruby/gems/2.3.0/gems/bundler-2.0.1/lib/bundler/runtime.rb:319:in `check_for_activated_spec!': You have already activated liquid 4.0.1, but your Gemfile requires liquid 4.0.0. Prepending `bundle exec` to your command may solve this. (Gem::LoadError)
        from E:/Ruby233/Ruby23-x64/lib/ruby/gems/2.3.0/gems/bundler-2.0.1/lib/bundler/runtime.rb:31:in `block in setup'
        from E:/Ruby233/Ruby23-x64/lib/ruby/gems/2.3.0/gems/bundler-2.0.1/lib/bundler/spec_set.rb:148:in `each'
        from E:/Ruby233/Ruby23-x64/lib/ruby/gems/2.3.0/gems/bundler-2.0.1/lib/bundler/spec_set.rb:148:in `each'
        from E:/Ruby233/Ruby23-x64/lib/ruby/gems/2.3.0/gems/bundler-2.0.1/lib/bundler/runtime.rb:26:in `map'
        from E:/Ruby233/Ruby23-x64/lib/ruby/gems/2.3.0/gems/bundler-2.0.1/lib/bundler/runtime.rb:26:in `setup'
        from E:/Ruby233/Ruby23-x64/lib/ruby/gems/2.3.0/gems/bundler-2.0.1/lib/bundler.rb:107:in `setup'
        from E:/Ruby233/Ruby23-x64/lib/ruby/gems/2.3.0/gems/jekyll-3.8.5/lib/jekyll/plugin_manager.rb:50:in `require_from_bundler'
        from E:/Ruby233/Ruby23-x64/lib/ruby/gems/2.3.0/gems/jekyll-3.8.5/exe/jekyll:11:in `<top (required)>'
        from E:/Ruby233/Ruby23-x64/bin/jekyll:22:in `load'
        from E:/Ruby233/Ruby23-x64/bin/jekyll:22:in `<main>'
```

&#160;&#160;&#160;&#160;解决办法

```
$ Select gem to uninstall:
 1. liquid-4.0.0
 2. liquid-4.0.1
 3. All versions
> $ 2
Successfully uninstalled liquid-4.0.1
```


[^1]: [Github使用教程](https://blog.csdn.net/p10010/article/details/51336332)

[^2]: [Git安装教程](https://jingyan.baidu.com/article/020278117cbe921bcc9ce51c.html)

[^3]: [ruby下载与安装教程](https://jingyan.baidu.com/article/48b558e33558ac7f38c09aee.html)

*** 