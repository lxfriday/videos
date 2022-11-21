# 开源网络请求库 AXIOS 手把手教学，附带关键源码分析（上）

- [前往视频](https://www.bilibili.com/video/BV1nv4y1D7t3)


兄弟们前面两期关于XHR和fetch的视频
感觉怎么样啊
今天这期视频呢
咱们来了解了解前端领域里面
z强大
z丰富
z受欢迎的网络请求库
AXIOS
AXIOS是一个能够同时运行于
NODEJS和浏览器环境中的网络请求库
它内部设计了一个适配器
代码运行在NODEJS环境中的时候
使用的是HTTP模块
而运行在浏览器中的时候使用的是XHR接口 
所以能够做到兼容两种环境
而它本身也是基于Promise 的网络请求库
这也就意味着
可以像使用fetch一样使用AXIOS
如此强大的一个开源网络请求库
它的star数也来到了将近10万颗左右
在GITHUB全球开源 项目中
它的排名是38名
这么热门的开源 项目
要是面试的时候不会讲
那真有可能掉大分
除了能够无缝衔接两种运行环境
和支持Promise之外
它的另一个强大之处在于
它能够支持拦截请求和响应
此外还有创建AXIOS实例
支持通用配置等等等等
如果你的项目使用到了包管理器
比如npm或者是yarn
那就使用  npm install axios
或者是yarn add axios
如果没有使用包管理器呢
那就直接使用CDN上面的axios
当然了
要注意在axios引入之后才能够使用axios
而对于axios的引入呢
则直接import或者是require就可以了
对于比较简单的请求
fetch和axios的配置方法一模一样
可以给axios传递一个URL
来发起一个比较简单的get请求
也可以在axios的第二个参数上
添加对请求的配置
这个配置项非常之丰富
咱们后面再讲
或者把URL和其他配置放在一个对象中
作为一个参数
或者使用axios提供的别名方法
来发起这个方法对应的HTTP请求
像这种情况
给太多用法反而会让人觉得有点迷糊
我的个人见解哈
对于axios的配置
它是极其灵活而且极其丰富的
在全球开源软件榜上面能够排38名
那肯定得有几把刷子了
假定我们当前的页面是https://x.com
url和baseURL这两个都是对请求地址
进行配置的属性
baseURL通常用来配置比较通用的前缀
而url用来配置后面的路径等等
注意哈
上面这两种写法效果是一样的
只不过
baseURL作为一个比较通用的配置
放在axios.defaults里面设置会比较好一点
axios.defaults是对axios实例进行通用配置的地方
也就是说
你对axios.defaults的配置
会在每个axios实例的请求中都会应用到
通常我们会把请求的路径放在url里面
把域名放在baseURL中
这种写法中的url它是一个相对路径
如果在url中填的是一个绝对路径
axios会自动忽略baseURL的配置
这一段是axios的源码
咱们可以看到
如果requestURL是一个绝对路径的话
它会直接忽略baseURL
另外axios对url还有自动纠正功能
如果你一不小心在baseURL的最后
以及url的开头都添加了斜杠
axios都可以给你正常地纠正过来
这是url纠正的源码
真的是非常的贴心
method是用来配置HTTP请求方法的属性
请求方法字符串大小写都可以
在axios内部会给你转换成大写
headers是用来配置HTTP请求头的属性
如果headers中的key出现了重复
则用后者覆盖前者
另外axios可以依据data的数据类型
而自动推断出 content-type
这是部分源码
date是用来设置HTTP请求体的属性
你可以给它传递各种数据
前提是你的后端能够接得住
前面说到的这些属性
axios和fetch基本上差不多
接下来就涉及到axios独有的属性了
params是一个用来拼装URL的属性
或者说是一个用来处理URL query 部分的属性
axios 能把params中的键值对序列化成字符串
然后拼装到URL的尾部
如果你的URL中本来就有部分query
axios 照样也可以处理
如果已有的 query 和 params 中的 key
存在同名情况
那 axios 形成的请求地址会有两个相同的 key
不过这个问题一般不会出现啊
写的时候稍微检查一下就可以了
它是用来配置请求超时时间的属性
单位是毫秒
超时事件触发之后
会抛出一个 promise reject
需要 catch 捕捉
正常情况下捕捉到的 code 是 ECONNABORTED
如果你想区分超时的错误码呢
需要在配置里面 transitional.clarifyTimeoutError 属性设置为 true
这样
超时事件触发之后的错误码就变成了
ETIMEDOUT
然后
你就可以依据 ETIMEDOUT 这个错误码
来做对应的超时处理
关于超时呢
需要注意一下
XHR的超时属性也是 timeout
然后它通过 ontimeout 来处理超时事件
然而 fetch 中是没有直接处理超时的属性的
兄弟们一定要注意避免被坑哈
这个属性
用来决定请求的时候是否携带 cookie
因为 axios 在浏览器端底层使用的是 XHR
所以 withCredentials
就只有 true 和 false 两个属性
默认是 false
这个属性对于同域的请求是无效的
也就是说对于同域的请求
即使设置 withCredentials 为 false 
也照样会发送 cookie
而对于不同域的请求
设置 withCredentials 为 true  的时候
要注意好在后端配置好跨域策略
另外 access-control-allow-origin
必须指定为当前的域名
不能够为  * 
axios 中特意给出了
onUploadProgress 和 onDownloadProgress
来处理上传和下载事件
通常这两个事件用来做进度控制
这里给出了各个属性的意义
signal 是用来处理取消请求的属性
axios 中 signal 的用法 和 fetch 中 signal 的用法一模一样
不过由于 axios 底层的 XHR
并不直接支持 signal
所以 axios 使用事件机制
加 xhr.abort() 来取消请求
这是 axios 中一个比较灵活的功能
它可以依据 HTTP 状态码
来决定是 resolve 还是 reject
当前的 Promise 流程
换句话说
如果状态码在200-300之间
那就走 then 中的逻辑
通常情况下
一个请求发送成功之后
我们会在 then 中做后续的处理
否则就要走 catch
也可以把状态码的范围扩大到200-400
或者直接 return true
这样的话
就不会因为状态码而抛出异常
axios 内部默认2开头的状态码会被 resolve
其他状态码会被直接 reject
responseType 返回的数据做自动格式化的属性
因为底层是基于XHR的
所以 xhr.responseType 能设置什么值
这里就能设置什么值
另外 axios 针对 json  类型的返回数据
内部有额外的处理流程
如果你发起了一个请求
但是并没有设置 responseType
那么 axios 会尝试使用 JSON.parse() 来解析返回的数据
即使我设置了返回头中
content-type 为 text/plain
axios  照样会尝试把返回的数据解析成 JSON
呃这个地方出现了一个乱码
在返回的 content-type 类型中
添加 utf8  编码之后
显示就正常了
如果服务端返回的是一个错误的 json 字符串
那 axios 是肯定没有办法把它给解析成 json 的
看原码就知道
如果解析失败的话会走 catch 的逻辑
但是因为 strictJSONParsing 为 false
所以说直接后面 return data 了
另外有一点需要明确啊
对于普通的 XHR 请求
如果不设置 responseType
那么得到的 response 是一个纯字符串
不是一个 json 对象
而设置了 responseType 为 json  之后
就能从 xhr.response 获取到  json 对象
但是 xhr.responseText 是不能够直接获取的
兄弟们
上面讲的这些属性是比较常用的属性
实际上还有十来个属性我没有讲
没有涉及到的那些呢
都是日常使用比较少的属性
大家感兴趣的话可以自己去看一下
那么
本期视频一不小心又来到了7分钟
这时间不等人啊
关于 axios 呢实际上还有一部分没有讲
那么剩下的呢就留到明天喽
