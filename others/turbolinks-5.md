Turbolinks 5

比 Turbolinks 3 好用，代码事整洁，文档也更友好。如果你之前直接关闭了 Turbolinks 的话，现在建议你尝试一下。

**两方面：**
一、Ruby 层面
二、JS 层面


**访问方式：**
GET 第一次访问页面
GET 第二次访问页面
刷新页面
重定向到页面
浏览器提供的“后退”
浏览器提供的“前进”


**原理：**
ajax 请求，改变 url，丢掉 head 部分、直接替换 body 部分。
window 和 document 保持不变。


**底层第三方技术：**
重点：
HTML5 History API 

**一般：**
Window.requestAnimationFrame
XMLHttpRequest

**其它：**
MutationObserver
Custom Elements


**工作方式：**
Turbolinks.xxx 直接调用
data-turbolinks-xxx 间接声明


**特殊情况，特殊处理：**
一般都是通过
turbolinks-xxx
data-turbolinks-xxx 
等方式声明当时是特殊情况，要求特殊处理。

data-turbolinks
data-turbolinks-action
data-turbolinks-eval
data-turbolinks-preview
data-turbolinks-permanent
data-turbolinks-track

turbolinks-cache-control
turbolinks-progress-bar
turbolinks-root


**生命周期：**
turbolinks:click
turbolinks:before-visit
turbolinks:visit
turbolinks:request-start
turbolinks:request-end
turbolinks:before-cache
turbolinks:before-render
turbolinks:render
turbolinks:load


**内部实现两种访问方式：**
直接从缓存里拿
从服务器响应里拿


**内部关键术语：**
Turbolinks-Location


**部分 API：**
Turbolinks.visit
Turbolinks.clearCache
Turbolinks.supported


**问题：**
重复执行
不执行
首次执行，后续不执行
document.read 等老的写法
页面引入 script
等。


**建议：**
先看官方文档（新、权威）
再看源代码
对比着看相关参考文章
