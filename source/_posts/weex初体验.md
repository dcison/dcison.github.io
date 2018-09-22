---
title: weex初体验
date: 2018-05-02 20:30:47
tags: [WEEX,Android]
categories: 技术分享
---
# Weex开发Android体验（基于Vue)

## Weex环境搭建

1. 首先搭配的是Weex环境全家桶
```
npm install weex-toolkit -g
```
2. 启动项目，并安装好依赖后，运行web端体验
```
npm start
```
这里遇到第一个坑是：显示应用运行成功，但是访问对应URL无对应文件，导致404错误，采取的谜之解决方法是：在搭建weex create 时版本选择lts，之后npm start正常显示。

3. 搭建Android环境
Android SDK 搭建3天，其中痛苦大家自己去体会吧，常言说的好，环境搭配三小时，代码不足5分钟，外加吹逼半小时。
4. Weex 搭建Android
```
weex playweex platform add android
```
5. Weex AS 模拟器运行
开启AS模拟器（SDK基于26）
```
weex run android
```
上述环境将近一周（在有梯子的情况），配置完后终于可以好好写代码了（后来发现是我想多了）
<!-- more -->
## 使用过程

6. Weex UI 配合使用
[官方手册]([https://alibaba.github.io/weex-ui/#/cn/)
不得不说，这手册写的都比Weex开发手册好，Weex手册新旧内容掺杂，一个字：坑。
7. 扩展插件：
[Nat](http://natjs.com/#/zh-cn/)
目前使用的是image-picker与upload，做的功能是获取手机里图片并上传到服务器，通过该插件就不需去写Android了（好评，不然我还不如直接去写Android）
8. 路由
	1. router.js配置
	2. entry.js (入口文件引入)
	3. index.vue中就一个路由展示页面
	4. 在子页面使用子路由，否则在子页面中虽然显示URL跳转成功但是无页面显示
	5. Android按返回键的问题至今没解决（暂时只知道通过Android解决）
9. 自己采的巨坑(待更)
	1. Android端只支持Flex布局，且flex-direction:column,web端为row。。。
	2. Android端无display属性，因此不能用display:none 属性导致的v-show也无法使用，只能使用v-if/opacity/通过定位，把各种暂时不要显示的内容丢到视图以外（只能是通过定死过长的宽度实现，因为无法改变层叠样式）
	3. Weex 中的class 只能是单属性，必须逐条写。。。
	4. 只有class，没有id
	5. weex 不支持简写，如margin: 0 auto;
	6. 加载本地图片，这里我采用的是本地搭建的静态资源服务器（一共两个，一个专用于返回图片/视频，一个用于反应用户逻辑操作，如登陆，注册等。）
	7. 显示文本必须用<text></text>套着,且字体大小只对<text></text>有用，即你给<text></text>的父元素写font-size也没用。
	8. 单位只支持px
	9. 调试工具只支持Mac。。。。。（这个我真的想骂娘）只能是在web端console.log调试，然后又跑到Android测试，浪费一堆时间。
	10. 没有单向框，要么用WeexUI，要么用Weex自带的PICKER，这玩意又不支持Web，手册写的Web搭配写的不知道懵逼，不知道如何配置（是我太菜了)
	11. storage获取中是异步的
```
var flag  = null;
storage.getItem("token", e  => {
if (e.result  ==  "success"  &&  e.data  &&  e.data  !==  "no"){
	flag = true;
} 
});
if (flag){
	//do something
}
```
如果你这么写，有可能会炸，因为异步原因，你执行到if(flag)的时候，flag还没有被成功赋值true
所以得这么改
```
storage.getItem("token", e  => {
if (e.result  ==  "success"  &&  e.data  &&  e.data  !==  "no"){
	flag = true;//do something
});
```
	12.  android 默认超出的内容隐藏（还没找到解决方法）
	13. 貌似移动端无cookie概念？采用token验证
	14. weex build android 后导出的未加证书的app,需要手动加证书
```
// 生成签名文件
keytool -genkey -alias runan.keystore -keyalg RSA -validity 1000 -keystore runan.keystore
// 开始签名
jarsigner -verbose -keystore runan.keystore -signedjar runan.apk app-release-unsigned.apk runan.keystore
```
	15. image必须设置宽高，否则不显示。
	16. stream.fetch 不支持delete....支持GET，POST，PUT

---

## 总结

Weex口号喊得很响，当初也是在D2听到它的口号：一次编写，处处运行。听到之后，心想666，卧槽，以后前端又多了个利器了。写了接近一个月的weex 之后。。。心中万千草泥马，官方手册不完善，默认你都会，学习整个weex的过程我感觉都能去学Android了，用Android写功能起码稳定很多。现在Weex还有些原生功能不支持（虽然我也不用）。总的来说，Weex现在虽然是1.，但是感觉它还是'玩具'，真要写项目的话，感觉还需等成熟点，或者阿里那边有Weex的框架还未开源，比如今天才知道的BindingX。
		



