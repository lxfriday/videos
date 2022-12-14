# 前端jsonp跨域手把手教程

- [前往视频](https://www.bilibili.com/video/BV1Av4y1Q7UZ/)

在上期视频中呢
我们已经把前端CORS跨域
给完整分析了一遍
那么本期视频呢
咱们接着了解前端领域里面
常用的其他跨域方式
在上期关于CORS的视频里面
就有说到
发起请求的时候
只要本次请求的协议
或者域名或者端口与当前页面不同
那就会受到浏览器安全策略的限制
而CORS策略呢
是浏览器层面对
跨域请求的一种比较正式的规范
那如果不使用CORS策略
或者说没有办法使用CORS策略的情况下
还有哪些偏技巧的解决办法呢
jsonp是对一种跨域方式的称呼
它现在的使用和json没有必然联系
在正常情况下
咱们使用XHR或者是fetch
发起一个跨域请求
如果目标地址的服务端没有配置CORS策略的话
这个请求肯定会失败
既然这样的话那该怎么办呢
你们应该都碰到过这种情况
就是一个页面的url
与这个页面中的script标签的url
并不是同源的
但是这些script标签所加载的内容
却能够正常执行
咱不正是可以利用script标签的这一特性
来发起跨域请求吗
我们可以用js创建一个script标签
然后在全局的window对象上
挂在一个名为jsonp的函数
再把这个标签添加到body标签的尾部
在服务端呢先从请求url中解析出query
得到的是cb和uid
接着服务端做一些业务处理
组织一下返回的数据
在返回数据之前呢
先设置HTTP的返回头中
content-type为text/javascript
最后服务端返回一串字符串
返回的内容对于服务端来讲
是一串字符串
对于前端来讲是一段可执行的js代码
所以整个流程
呈现到页面上的效果是这样的
在浏览器控制台中呢
也打印出了后端的数据
这就是jsonp的实现原理
在前端只需要指定一个全局的函数
然后在后端把需要传输的数据组织好
再把数据序列化成字母串
最终由服务端返回一段js可执行代码
而这段代码内容就是一个函数要执行
兄弟们这个过程可能也有点绕啊
jsonp也是需要前后端一起协同配合的
前端需要做两部分工作
其中一步呢
是要插一个全局函数到window上
这样后端传过来的js代码
就可以直接执行这个函数
前端的另一个工作呢
是需要传输一串query
这样后端才能知道
需要执行的全局函数名字
除了这个cb呢
query中可能是需要传递其他信息
用于后端的业务处理
这里有一点需要注意
因为jsonp请求依赖于script标签
而script标签发送的请求
请求方法是GET
所以就不能像POST一样
在body里面传输数据
只能使用url query或是cookie传输数据
query的写法是这样的啊
那如果是用cookie传输数据的话
script标签需要添加一个属性叫做crossOrigin
它的值得是use-credentials
不过仅仅是前端这样改的话
发起请求的时候它就会
还是会被CORS策略所限制的啊
如果非要发送cookie的话
服务端就得配置一些access-control-* 相关的字段
这个内容在上期视频有详细的讲述
这个视频就不讲了哈
这是一个成功发送cookie的例子
这里如果已经涉及到后端需要配置CORS策略
那jsonp就没有必要用了
咱们要是老在业务里面写这样的代码
那肯定是不好的哈
query中的字段如果比较多的话
那肯定得有一个自动拼接query的处理过程
每次挂载的全局函数都是一样的
那肯定也会有问题
script标签只新增不删除
那也是有问题
所以一个比较通用的
优化过后的jsonp封装就是这样的
使用的时候呢
直接promise化调用就可以了
每次请求的时候
全局函数的名字都是变化的
新增的script标签
在请求完成之后也会自动删除
缺点还是有的
没有错误捕捉
这个问题就比较麻烦了
所以暂时不在本期讨论之内
严格来讲这可以说不算是一种跨域
这种跨域形式
是浏览器完全感知不到的
因为浏览器不能够直接
请求不同的源嘛
所以就把请求真实目标地址这个任务
交给了同源的服务器
在xx.com的网页
请求xx.com的服务器地址是一点问题都没有的
然后呢代理服务器
再去请求yy.com的真实服务器地址
拿到数据之后呢
再返回到浏览器上面来
简直没一点毛病
就是整个过程看起来比较繁琐
同样呢也是需要后端做一些处理
这个中间层代理跨域的思想就是这样
那具体实现了咱就不实现了
比较麻烦
用的也不多
了解就好
然后呢关于其他的跨域方法
也还有好几种
但是用的也不多
这里就不做深入讨论了
最后咱说一点
前端虽然经常可能会碰到跨域的问题
但是
z好的解决办法就是把它抛给后端
咱什么都不做是最好的
本期视频到这里就结束了
