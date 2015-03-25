# turbolink 就是一陀屎

turbolink 要改变编程习惯。

有几个链接不会使用 turblinks.
如：# 开头、非 HTML 链接、target = _blank 以及下面要介绍的。
<br>
并且，正常的链接可以关闭 turblinks. 关闭某个链接，或 div 之类的 turbolink 可以给它加上属性：
data-no-turbolink

turbolinks 原理是监听页面的 click 事件，然后异步的去后台取得数据，然后替换掉 body 中的内容，并更改链接地址信息，这样就可以不用重新加载 js 和 css 资源，从而达到加快请求速度的目的

原目的是想在不同的页面加载不同的css和js资源，而那些与当前页面不相关的css和js，我都不加载。
遇到了问题，下面是回答。
turbolinks 设计本来就是为了减少加载 css 和 js 如果有公共大量部分 css 和 js 只有少量需要动态加载的，可以在 pageinit 里面写 js 动态加载

```
1.gem 'turbolinks'
2.//= require turbolinks
3.1
$(document).on 'click', '.edit_task input[type=checkbox]', ->
  $(this).parent('form').submit()

# 3.2
ready = ->
  $(document).on 'click', '.edit_task input[type=checkbox]', ->
    $(this).parent('form').submit()

$(document).ready(ready)
$(document).on('page:load', ready)

# 3.3
$(document).on 'ready page:load', ->
  $(document).on 'click', '.edit_task input[type=checkbox]', ->
    $(this).parent('form').submit()
```

Turbolinks 开启后，网站将成为一个单页应用，页面切换不释放 js 逻辑。这意味着编写前端逻辑的时候需要留意内存泄露或者其他持久页面导致的问题。
流程：

点击链接
XHR 请求页面
返回页面
将返回页面的 title 和 body 部分替换到当前页面，css 和 js 丢弃

```
尤其是

$(document).ready ->
  alert "page has loaded!"
要变成

$(document).on "page:change", ->
  alert "page has loaded!"

————

$(document).ready ->
的地方都直接替换成

$(document).on "page:change", ->

或者压根不用这么写法，而是：

$(document).on 'click', 'body', ->
  console.log('hit’)
```

———


地址栏会变
head 不变
ga script 如果放在 body 部分，那么跟正常用没什么分别

———

唱个反调: Turbolinks 并不好.

Turbolinks 的出现是为了解决什么问题?

- 减少 HTTP 连接数.
- 减少 Javascript 与 CSS 的编译时间
- 解决的同时送给你了什么新的礼物?

潜在的内存泄露 ( Chrome 的 DOM 与 JS Object 之间很容易泄露内存, 变相提高了程序员素质.

额外的适配原有后端逻辑的代码. ( 例如 适配 redirect_to 301, 在 turbolinks 代码内部. cache 以及 cookie

前端代码的 ready 事件改写. ( page:load 可以用 jquery.turbolinks 继续监听 $.ready

前端监听事件的复杂化, 本来就一个 DOM ready, 现在变成了 5 个事件. 你可以小心翼翼的来处理这 5 个事件, 来保证程序的正常运行.

$(document).on 以及老版本 jQuery 的 $(".class").live 要特别小心. 尽量使用 BackBone 之类的框架, 不要面向 DOM 编程. 因为事件绑定之类的会随着 View 对象的消失而被清理, 不然需要处理很多 on, off, 以及潜在的多次事件绑定.


————

加了 turbolinks 之后，究竟可不可以用 `$ ->` 呢？

我升级 Rails 4 的第一坑就是这个了，当时简单的搜索了一下，发现有三个选择：

- 删除 turbolinks
- $(document).on 'page:load' ->
- gem 'jquery-turbolinks’

当时花了两个小时试了试后两个选择，都是坑坑不止，唯有 dd 一下最爽，半秒钟解决所有问题，世界一下就清静了……

———



引入 turbolinks 后的几个事件：

```
page:before-change a Turbolinks-enabled link has been clicked (see below for more details)
page:fetch starting to fetch a new target page
page:receive the page has been fetched from the server, but not yet parsed
page:before-unload the page has been parsed and is about to be changed
page:change the page has been changed to the new version (and on DOMContentLoaded)
page:update is triggered alongside both page:change and jQuery's ajaxSuccess (if jQuery is available - otherwise you can manually trigger it when calling XMLHttpRequest in your own code)
page:load is fired at the end of the loading process.

page:restore is fired at the end of restore process.
```

By default, Turbolinks caches 10 of these page loads.

参考：

jquery.turbolinks https://github.com/kossnocorp/jquery.turbolinks

Turbolinks https://github.com/rails/turbolinks

pjax https://github.com/defunkt/jquery-pjax




