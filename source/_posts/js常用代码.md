---
title: js常用代码
date: 2017-09-12 21:46:22
tags: [javascript,笔记]
categories: 笔记
---
[笔记来源](https://segmentfault.com/a/1190000008632671)

### js删除节点
* IE下removeNode()
* 别的removeChild()

### sort方法排序
只能对数组使用

### 编码
* stringObject.charCodeAt(index),index是字符在字符串中的下标

### 刷新
* location.reload([bForceGet]) ,[bForceGet]默认值是false,表示从客户端缓存里取当前页。设置为true, 则以GET方式,从服务端取最新的页面, 相当于客户端按了F5键
* location.replace(URL),通过指定URL替换当前缓存在历史里（客户端）的项目,因此不能通过**前进**和**后退**来访问已经被替换的URL。
* history.go(0) 推荐

### 检查数值
isFinite(NUM) NUM放要检测的对象,是数值返回true

### parseInt(string/num,num)
* 如果字符串的第一个字符不能转化为数字(后面跟着数字的正负号除外),返回NaN,别的会把其中的数字转换
* 第二个参数默认是10,表示进制转换,范围是2-36
* 如果第二个参数不是数值,会被自动转为一个整数。这个整数只有在2到36之间,才能得到有意义的结果,超出这个范围,则返回NaN。如果第二个参数是0、undefined和null,则直接忽略。
* 如果字符串包含对于指定进制无意义的字符,则从最高位开始,只返回可以转换的数值。如果最高位无法转换,则直接返回NaN。

### parseFloat()
* parseFloat方法会自动过滤字符串前导的空格
* parseFloat会将空字符串转为NaN
<!-- more -->

### charAt()
string.charAt(index) 返回string里面index的字符

### join()
arrayObject.join(separator) 将数组里面值用separator作为分隔符连接成一串字符串,跟python一样

### split()
stringObject.split(separator,howmany)
* separator:必需。字符串或正则表达式,从该参数指定的地方分割 stringObject。
* howmany:可选。该参数可指定返回的数组的最大长度。如果设置了该参数,返回的子串不会多于这个参数指定的数组。如果没有设置该参数,整个字符串都会被分割,不考虑它的长度。

### indexOf()
stringObject.indexOf(searchvalue,fromindex)
* searchvalue:必需。规定需检索的字符串值。
* fromindex:可选的整数参数。规定在字符串中开始检索的位置。它的合法取值是 0 到 stringObject.length - 1。如省略该参数,则将从字符串的首字符开始检索。
* indexOf() 方法对大小写敏感！ 

### lastIndexOf()
stringObject.lastIndexOf(searchvalue,fromindex)
* searchvalue:必需。规定需检索的字符串值。
* fromindex:可选的整数参数。规定在字符串中开始检索的位置。它的合法取值是 0 到 stringObject.length - 1。如省略该参数,则将从字符串的最后一个字符处开始检索。

### call()
obj1.method1.call(obj2,argument1,argument2)

### substring()
stringObject.substring(start,stop)
* start:必需。一个非负的整数,规定要提取的子串的第一个字符在 stringObject中的位置。
* stop:可选。一个非负的整数,比要提取的子串的最后一个字符在 stringObject中的位置多1。如果省略该参数,那么返回的子串会一直到字符串的结尾。
* 返回值:一个新的字符串,该字符串值包含stringObject的一个子字符串,其内容是从start处到stop-1处的所有字符,其长度为 stop 减 start。

### toString()
* Array.toString():将数组转换成一个字符串,并且返回这个字符串。
* Boolean.toString():将布尔值转换为字符串。(true/false)
* Date.toString():将Date对象转换成一个字符串,采用本地时间。
* Error.toString():将Error对象转换成字符串
* Function.toString():把函数转换成字符串
* Number.toString(num):将数字转换为字符串。num指定的基数或底数(底数范围为2-36)。如果省略参数,则使用基数10。当参数值为2时,返回二进制数。

### fromCharCode()
String.fromCharCode(numX,numX,...,numX)
numX：必需。一个或多个 Unicode 值,即要创建的字符串中的字符的 Unicode 编码。

### substr() 
stringObject.substr(start,length)
* start:必需。要抽取的子串的起始下标。必须是数值。如果是负数,那么该参数声明从字符串的尾部开始算起的位置。也就是说,-1 指字符串中最后一个字符,-2 指倒数第二个字符,以此类推。
* length:可选。子串中的字符数。必须是数值。如果省略了该参数,那么返回从 stringObject 的开始位置到结尾的字串。
* substr() 的参数指定的是子串的开始位置和长度,因此它可以替代 substring() 和 slice() 来使用。
* ECMAscript 没有对该方法进行标准化,因此反对使用它。