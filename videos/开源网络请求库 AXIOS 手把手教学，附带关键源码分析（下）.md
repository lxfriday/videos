# 开源网络请求库 AXIOS 手把手教学，附带关键源码分析（下）

- [前往视频](https://www.bilibili.com/video/BV1jG4y1b7KA)

在上期视频中
我们已经把 axios 的基本用法
和常用配置都给过了一遍
那么本期视频呢
咱们接着讲 axios 请求成功之后的处理
以及 axios 的封装 通用配置和实例
axios 请求成功之后的 response 中
总共会有6个属性
在一个真实的请求中打印response
response中会携带本次请求的配置信息
以及请求返回的相关信息
response.config中的数据是
关于本次请求的一些配置信息
而 response.data
则是本次请求
从服务端接收到的数据
response.headers
是本次请求服务端返回的HTTP头信息
response.request
是本次发起请求的XHR实例
最后
response.status则是本次请求返回的HTTP状态码
axios 实例是 axios 特有的一个功能
使用 axios.create()
就可以创建出一个新的 axios 实例
create 能够传递的参数和 axios config 的参数是一样的
如果你的项目比较简单的话
实际上可能用不到 axios 实例这个功能
如果有一些请求使用的是相同的地址
或者是有一些共同的属性
则可以使用实例
这个地方我们创建了两个 axios 实例
发往 x.com 的请求就用左边的实例
而发往 cat.com 的请求 
则用右边的实例
如果存在好几个请求
都需要配置相同的头信息的时候
也可以用实例
总之呢用实例可以节省代码量
同时呢用起来也比较方便
从原码中可以看到
我们通过 import 导入的 axios 
它就是一个实例
而通过 axios.create() 创建的
也是一个实例 
不过这一段代码比较绕啊
兄弟们可以暂停下来
分析分析这段源码
axios.defaults 的作用
实际上和创建一个 axios 实例差不多
通过 axios.defaults 
可以变更 axios 实例的默认配置项
通过前面的分析我们知道
导入的 axios 就是一个实例
当给 axios.defaults 配置了一些属性之后
使用实例发起请求的时候
对 defaults 的配置会应用到本次请求上
因为 axios.defaults axios.create 和 axiosInstance
发起请求的时候
本身都可以添加配置信息
所以就自然而然涉及到配置的优先级问题
对于不太了解 axios的兄弟们而言
这个地方优先级可能会比较难理解
看不太明白也没太大关系啊
因为你大多数时候都不会配错
当调用  axiosInstance  的时候
就会发起一个请求
所以 config2 的优先级是最高的
而 axios.defaults 的配置信息
因为导入的时候就会存在
所以优先级是最低的
所以实际上配置的优先级是这样的
大家可以看看这个例子
在 axios.defaults.baseURL 中设置的是 aaa.com
而在 axios.create 中设置的 baseURL 是 bbb.com 
最终用实例发起请求的时候 baseURL 是 ccc.com
在网页上发起请求之后
最终显示出来的地址也是 ccc.com
当把最高优先级的 baseURL 删除之后
则会应用 axios.create 里面的 baseURL
呃在这里插一句啊
axiosInstance.defaults 也是可以设置属性的
在这个 defaults 中配置的属性
它只比 axiosInstance 中配置的属性优先级低一点
在 axios  中存在请求拦截和响应拦截
请求拦截函数会在请求发起之前执行
它的参数是合并之后的配置信息
当拦截函数执行完成之后
请求就会被发送出去
而响应拦截函数会在下面的 then 之前执行
如果我在响应拦截函数的参数中添加了一个 key
那在后续的 then  处理流程中
也可以获取到这个 key
兄弟们说真的
这一段讲起来属实是过于枯燥了
关于拦截器呢也没太多好说的
实际上看一遍就知道了
提一个需要注意的点
拦截器中的 return  一定不要忘记了
否则会导致请求出现问题
一旦忘记的话要么是请求发送不出去
要么是 then  中获取不到数据
好了兄弟们
关于 axios 的方方面面都已经讲完了
从这两期视频的分析中
你们应该都可以看出来
axios 的功能非常之全面
所以这也是为什么它能够
如此受欢迎的原因吧
基本上一个前端项目
只要对包体积没有太大要求
那 axios  大概率就是必选了
本期视频到这里就结束了
希望对你们有所帮助
