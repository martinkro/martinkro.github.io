---
layout: post
title: "一步一步创建Jekyll主题"
date: 2016-10-23
---
* 目录
{:toc}

## 搭建本地的Jekyll环境

因为图方便所以个人直接在Ubuntu下搭建了环境，在Mac下也几乎一样。

1. 安装ruby环境
  `sudo apt-get install ruby`
2. 安装Jekyll
  `sudo gem install jekyll`
3. 安装kramdown
  `sudo gem install kramdown`
4. 安装rouge
  `sudo gem install rouge`
5. 测试Jekyll是否安装完成
  `jekyll --version`

> 从上面可以看出，这是一套基于Ruby语言的工具集。
> **题外话：**gem install是ruby用来管理ruby工具集的工具，而npm install是nodejs用来管理js工具集的工具。

## 本地跑起来
```
jekyll new mytheme
cd mytheme
jekyll server
```
运行上面的命令，然后访问[127.0.0.1:4000](http://127.0.0.1:4000)，就能看到一个由Jekyll搭建的博客了。

## Github Pages环境本地化

上面搭建的只是Jekyll的本地环境，当push到Github Pages后环境会有所变化，为了本地看到的效果和托管在Github Pages看到的效果一致，我们最好搭建本地的Github Pages环境。

1. 升级ruby到2.0.0以上
  如果`ruby --version`查看版本低于2.0.0，那么需要升级ruby。
2. 安装ruby工具集管理工具[Bundler](http://bundler.io/)
  `sudo gem install bundler`
3. 创建Gemfile
  在上面的mytheme根目录下创建一个Gemfile文件，内容为：
  `source 'https://rubygems.org'`
  `gem 'github-pages', group::jekyll_plugins`
4. 安装Github Pages的工具集
  在Gemfile所在的目录，即Jekyll主题的根目录，执行下面的命令：
  `bundle install`
5. 跑起来
  `bundle exec jekyll serve`

如果出现`bundle exec jekyll serve`能启动，而`jekyll serve`不能启动，则删除Gemfile和Gemfile.lock重新运行`jekyll serve`即可。
更多Github Pages本地化环境搭建，可参考[github-helper-setting-up-your-github-pages-site-locally-with-jekyll](https://help.github.com/articles/setting-up-your-github-pages-site-locally-with-jekyll)

## Windows下搭建Jekyll环境
因为不少的时间在Windows平台下工作，所以后来还是搭建了Windows下的Jekyll环境。

1. 安装ruby环境
  下载[ruby for windows](http://rubyinstaller.org/downloads)，随便搜索即可，建议安装ruby2.0以上。
2. 安装完毕，设置Windows环境变量
  在我的电脑 - 属性 - 高级 - 环境变量 - 系统 - path字段，添加ruby的安装路径。比如`C:\Ruby22\bin;`，安装包有提供选项可以在安装时自动添加到path。
3. 安装Jekyll
    打开命令行，输入gem.bat (Ruby22/bin/gem.bat)，如果没有找到该命令，说明环境变量还没有生效。在命令行输入`set a = b`，然后重启命令行即可(运行set只是让命令行重新加载环境变量)。
    执行`gem install jekyll`
4. 安装bundler
    `gem install bundler`
5. 使用bundler安装github pages的依赖
    `bundle install`
6. 跑起来
    `bundle exec jekyll server`
7. 如果端口被占用
    `bundle exec jekyll server --port 5000`

### bundle install失败
bundle install出现了以下错误:

Please update your PATH to include build tools or download the DevKit
from 'http://rubyinstaller.org/downloads' and follow the instructions
at 'http://github.com/oneclick/rubyinstaller/wiki/Development-Kit'
大致意思是插件需要编译安装，而系统没有安装编译环境，只有运行环境，请按照wiki里面的步骤安装。

修复：
下载[ruby-devkit](http://rubyinstaller.org/downloads)，如果ruby是32位则下载32位的devkit，否则下载64位的。

> ruby --version可以看到是32位还是64位

安装：
详细说明在[github wiki](https://github.com/oneclick/rubyinstaller/wiki/Development-Kit)里

1. 解压devkit到目录A
2. 进入目录A
3. 命令行下运行`ruby dk.rb init`
4. 运行`ruby dk.rb review`
5. 运行`ruby dk.rb install`
6. 如果上述步骤只有info输出而没有warning输出，则应该安装成功了
7. 测试`gem install json --platform=ruby`如果安装成功，则表示devkit安装成功
8. 如果失败，可以重装ruby和ruby-devkit，或者选择更低的ruby版本

### ruby/bin下面的bundle与bundle.bat区别

bundle是ruby脚本而bundle.bat是windows批处理文件
在windows命令行下，bundle其实执行的是bundle.bat，所以不会报错。bundle文件不会被识别为可执行文件。
在mingw命令行下(mingw/msys.bat)，bundle可以成功执行，而bundle.bat则会因为使用了windows命令而报错。

## 环境配置总结
环境的配置，简而言之，只有以下的步骤：

1. 安装ruby
2. gem install jekyll
3. gem install bundle
4. git clone https://github.com/jokinkuang/stepbystep.git
5. cd stepbystep
6. bundle install
7. bundle exec jekyll server

## 需要一个网页原型

Github Pages和Jekyll本地环境已经搭建完成，访问[127.0.0.1:4000](http://127.0.0.1:4000)也能够看到一个简单的博客，接下来就是思考自己的博客应该长哪样。

一般来说，要定制自己的博客，最好先设计出博客的网页原型，所谓网页原型即是使用html、css甚至js来完成静态的网页效果。当前博客的原型只有三个页面：`index.html`、`article.html`和`post.html`。

最后，**Jekyll模板已经跑起来了，网页原型也有了，怎么将两者结合起来呢？**在整合之前，我们需要先了解Jekyll博客系统。