---
title: yilia主题搭配Gitment评论系统
date: 2017-08-26 12:40:14
tags: [hexo,主题配置,gitment]
categories: 技术分享
---
## gitment介绍
一款由国内大神imsun基于 GitHub Issues 的评论系统<big>☞</big>[Go](https://imsun.net/posts/gitment-introduction/)
### 一.注册OAuth Application
[注册页](https://github.com/settings/applications/new)
* Application name, Homepage URL, Application description 随意填写
* Authorization callback URL 写自己Github Pages的URL(例如我的就是https://dcison.github.io)
* 注册后获得Client ID和Client Secret
<!-- more -->
###  二.引入gitment
1 在我们的主题目录下,即\themes\yilia\_config.yml评论系统位置里加入代码,
    ``` YAML
        #4、Disqus 在hexo根目录的config里也有disqus_shortname字段，优先使用yilia的
        disqus: false
        gitment: true #就加这一行,上面的是原来有的评论系统。
    ```
2 在\themes\yilia\layout\_partial目录下找到article.ejs,在里面找到
    ``` javascript
        <% if (!index && post.comments){ %>
    ```
在它下一行添加代码
    ``` javascript
        <% if (!index && post.comments){ %>
            //就是下面这一段代码
            <% if (theme.gitment){ %>
                <%- partial('post/gitment', {
                    key: post.slug,
                    title: post.title,
                    url: config.url+url_for(post.path)
                }) %>
            <% } %>
            //下面的都是已经有的,列出来给个参考位置而已
            <% if (theme.duoshuo){ %>
            <%- partial('post/duoshuo', {
    ```
3 在\themes\yilia\layout\_partial\post下新建一个文件:gitment.ejs,里面代码放
    ``` javascript
            <div id="box"></div>
            <link rel="stylesheet" href="https://imsun.github.io/gitment/style/default.css">
            <script src="https://imsun.github.io/gitment/dist/gitment.browser.js"></script>
            <script>
            var gitment = new Gitment({
            owner: '你的github名字',
            repo: '你的仓库名称(我们应该是用户名.github.io)',
            oauth: {
                client_id: '复制你注册的id',
                client_secret: '复制你注册的secret',
            },
            })
            gitment.render('box')
            </script>
    ```
注意一开始的id="box"原代码为container会与我们主题冲突,所以得换个id名字,不换你可以看看什么效果(滑稽),不喜欢box这个名字你自己起一个,然后最下方一行的render记得也要改就好了。
### 为每篇文章初始化评论系统
注意是每篇文章
1 上面几步做完后,部署到github上,点开一篇文章,在文章最下方就能看到评论框,并且写着Error: Comments Not Initialized,提示该篇博文的评论系统还没初始化。
2 点击Login with GitHub后，使用自己的github账号登录后,点Initialize Comments的按钮
3 完事,享受大神给我们的福利吧
感谢[imsun](https://imsun.net/posts/gitment-introduction/)
