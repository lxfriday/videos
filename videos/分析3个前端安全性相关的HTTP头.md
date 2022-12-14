# 分析3个前端安全性相关的HTTP头

- [前往视频](https://www.bilibili.com/video/BV12e4y1p7cp/)

今天来给大家分析分析
这3个和安全性相关的HTTP头
这3个HTTP头在面试的时候
出现的概率还是很大的
而对它们的应用也是随处可见
HSTS的全称叫做HTTP strict transport security
它是一个用来告诉浏览器
强制使用HTTPS的头
当浏览器在HTTPS页面中接收到这个指令之后
在指令的有效期内
如果使用HTTP协议访问这个页面的话
浏览器会直接给出一个307 Internal Redirect重定向
可以看到下面的location
重定向地址是https://a.com
307状态码呢我们平常是很难见到的
这里就是它的一个应用啊
这个http://a.com的请求
是直接被浏览器给拦截了
这个请求并不会发送到服务端啊
这个头里面常用的指令是max-age
和includeSubDomains
max-age的单位是秒
表示的是指令在浏览器中的有效期
而includeSubDomains
则表示是否把这个指令
应用于当前域名的子域名
这里服务端下发的指令中max-age=86400
也就是是一天
所以在一天的时间之内
使用http协议访问a.com的时候
都会被强制跳转到https://a.com中
而浏览器中是会存有两条记录的
当服务端下发了includeSubDomains指令之后
我们使用HTTP协议访问子域名b.a.com
也会被强制跳转到HTTPS协议
只要是a.com的子域名啊
它都会被强制跳转到以HTTPS协议访问
所以这个HSTS的作用
就是用来禁止客户端使用HTTP协议访问服务器
如果客户端首次访问时是以HTTP协议访问服务器
服务器中包含HSTS指令
这种情况下HSTS指令是不会生效的
也就是说当你刷新页面的时候
浏览器访问的还是http://b.com
并不会强制跳转
因为HTTP协议传输的时候并不可靠
而当你在HTTPS页面
访问HTTP协议地址的时候
这个请求会被浏览器直接挂掉
CSP在前端面试的时候
出现的概率非常高啊
它的全称叫做content security policy
也就是内容安全策略
它可以严格限制浏览器加载资源的来源
使用CSP可以有效地减少XSS攻击
因为CSP
只允许从指定的源来加载资源
所以即使页面中被注入了恶意的代码
恶意代码也没办法请求不可信的资源
CSP可以说是一个非常强力的安全工具
它的配置呢
说难也不算难
但就是指令比较多
因为它可以精细化到对不同类型的资源做不同限制
呃它的指令有十来个
兄弟们不要慌
这里呢我选取了常用的9个指令
指令虽然比较多啊
但是它的用法还是比较相似的
我们看指令的前缀
大体上就可以猜到这个指令
是用来做什么的
这些指令
基本上能够覆盖
网页上加载的各类资源
default-src是一个备选指令
也就是说
当你没有指定某个类型资源的加载来源的时候
浏览器就会从default-src中
查找是否有匹配的来源
这些指令如果没有指定的话
就会从default-source中查找
default-src能够传递的配置有这么几种
从第二条就可以看出
CSP可以严格地限定协议域名和端口
self表示匹配当前的源
只有协议域名和端口与当前页面都相同
self这条规则才能够通过
当前页面是一个HTTPS页面
用的是443端口
如果请求5678端口的话请求就会失败
第二条规则
是限定了资源的协议域名和端口
只有协议域名和端口都匹配上
才能够通过
这个就比较常见了
通常情况下
你也可以选择不加协议和端口
不加协议的话
就是HTTP、HTTPS两种协议都可以匹配
不加端口的话默认就是80端口
也可以使用一个*
来代表匹配全部的子域名
data: 和 blob:
表示是否允许使用这两种协议来加载资源
默认情况下这两种协议是不被允许的
当在default-src中添加了data:协议之后
data:image/png;base64, 加载的
图片就可以正常显示出来了
使用了CSP之后
默认是不允许加载内联资源的
你必须要添加一个unsafe-inline
这个html页面里面
插入了一段非常简单的内联js脚本
如果CSP中没有指定unsafe-inline的话
这个js脚本是无法执行的
unsafe-inline它可以限定是否加载内联css或者是内联js
而unsafe-eval
则是用来限定是否允许使用eval的
这里要注意
不要把英文的引号给漏掉了
兄弟们
CSP看起来是不是非常的全面啊
它可以精细化到
什么类型的资源都可以限定
那用了CSP之后
网页的安全性肯定是高了不少啊
下面的这几个指令
全部是用来控制对应类型资源的
配置方法和default-src差不多啊
unsafe-inline可以应用于script-src和style-src
而unsafe-eval则只应用于script-src
像style-src或者是img-src
可能有时候来源非常的复杂
这个时候你可以给它们添加一个*
表示可以从所有的来源加载资源
connect-src可以用来限定XHR或者是fetch请求
通常是使用self
或者是指定域名来限定
而frame-src
则是用来限定frame中的资源地址
media-src则用来限定audio或者是video标签中的资源
通常是音视频
font-src则是用来限定字体的加载来源
兄弟们
这些带-src的指令总算是讲完了
简直是全面的离谱啊
最后咱们再来看一看这个report-uri
它是一个用来打小报告的指令
在CSP中设置了report-uri之后
页面中加载的一个图片
违背了CSP的规则
那么浏览器就会向我们指定的report-uri地址
发送一个POST请求
请求里面会携带非常详细的信息
包括了资源的地址
资源违背的规则以及
CSP原始的规则
好了兄弟们CSP就讲完了
它的整体配置还是很复杂的呀
需要多看一看
这是本期视频的最后一个内容
X-Frame-Options
可以用来防止你的网页
被套在别人的网页上面
如果你的网页在不知情的情况下
被应用在别人的网页里面
那很有可能会出现安全性问题
或者是信任问题
比如说我想在自己的网页上面
套一个某度的主页
因为X-Frame-Options规则的限制
我自己的页面中
根本就没有办法显示这个页面
在浏览器的控制栏中
也给出了X-Frame-Options的提示
X-Frame-Options的用法也非常简单啊
通常就是deny或者是sameorigin
使用deny的话
当前页面是不允许在frame中嵌套的
而使用sameorigin的话
是只允许在同源的页面中嵌套
那如果要允许当前的页面
被某个页面嵌套呢
这就涉及到了CSP的frame-ancestors
使用frame-ancestors
就可以指定
当前的页面只允许被
某些指定的页面进行嵌套
比如说在a.com 中设置的是frame-ancestors b.com
表示只允许a.com被b.com嵌套
那在b.com中就可以正常显示a.com 
而在c.com中是无法正常显示a.com的
浏览器中也会给出CSP规则的提示
CSP中还有一个frame-src的规则
它用来限定
当前页面中frame的加载来源
好了本期视频到这里就结束了
大家如果觉得有用的话给个三连吧
拜拜
