---
title: yillia主题的配置
date: 2017-08-22 16:49:19
tags: [hexo,主题配置]
---
## hexo常用命令

``` bash
$ npm install hexo -g #安装  
$ npm update hexo -g #升级
$ hexo init #初始化
$ hexo n "xxxx" # 新建文章
$ hexo p #publish
$ hexo clean && hexo g 
$ hexo s #启动服务
$ hexo d #部署
```

## 主题的一些注意点

* _config中的menu：

``` yml
menu:
  主页: /
  随笔: /categories/随笔
  XXX: /categories/XXX 会自动归档分类
```

只要用hexo n "x" #新建文章,在文章里如下写

``` markdown
---
title: XXX
date: 2017-08-22 20:46:19
tags: [tag1,tag2,...]
categories: 随笔 //对应归档的名字
---
文章正文XXX
```
就可以自动归档啦~