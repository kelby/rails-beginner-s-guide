# 从源代码出发，理解和使用 Turbolinks

简单理解：把原来所有 GET 请求(包括链接、重定向、浏览器的后退)都通过 AJAX 来实现，并且 url 是变化的。

Turbolinks 会接管所有站内链接，用 ajax 请求获得目标页面，然后替换 body 内容。Turbolinks 接管下的网站不用重复解析 js 和 css 文件，这可以达到加速的目的，同时也让整站成为一个单页应用，页面切换不释放之前的页面内容。

涉及：大概原理、带来的问题、解决办法。

使用 turblinks 的话，注意：一，尽量从 AJAX 的角度出发写监听事件；二，尽量使用 turblinks 提供的监听事件。

语法或历史遗留问题，可以使用以下方法：

```
gem 'jquery-turbolinks'
```

解决。

## Ruby 部分

### 覆盖了 url_for 和 referer 方法，修复普通链接里的 :back

更改了 url_for 方法(会影响到 link_to 等调用到它的方法)

当 url_for 使用到 :back 参数时有用，它会把原来 header 里的 HTTP_REFERER 改为 HTTP_X_XHR_REFERER

原因：Turbolinks 获取内容的时候使用 XMLHttpRequest 发起请求(简称 XHR)

带来的问题：XHR 请求的 HTTP 头 Referer 并不正确

解决办法：添加了 X-XHR-Referer 头，后端程序获取 Referer 的时候，需要先取 X-XHR-Referer。

```ruby
module XHRUrlFor
  def self.included(base)
    base.alias_method_chain :url_for, :xhr_referer
  end

  def url_for_with_xhr_referer(options = {})
    options = (controller.request.headers["X-XHR-Referer"] || options) if options == :back
    url_for_without_xhr_referer options
  end
end

# 辅助方法
def referer
  self.headers['X-XHR-Referer'] || super # super 就是 self.headers['Referer']
end
alias referrer referer
```

### 覆盖了 Redirect 的 call 方法，修复 redirect_to 里的 bug

```ruby
def call_with_turbolinks(env)
  status, headers, body = call_without_turbolinks(env)

  if env['rack.session'] && env['HTTP_X_XHR_REFERER']
    env['rack.session'][:_turbolinks_redirect_to] = headers['Location']
  end

  [status, headers, body]
end
alias_method_chain :call, :turbolinks
```

注意：上面设置了头 X-XHR-Redirected-To

### 其它几点 

**3 个钩子**

```
before_action :set_xhr_redirected_to, :set_request_method_cookie
after_action :abort_xdomain_redirect
```

**因为 trublinks 改变了原来 redirect_to 的行为，带来了新的问题，然后它又提供了解决办法：**

```
  def set_xhr_redirected_to
    if session && session[:_turbolinks_redirect_to]
      response.headers['X-XHR-Redirected-To'] = session.delete :_turbolinks_redirect_to
    end
  end
```

**对于非 GET 请求，turblinks 是不起作用的(压根就不会初始化)。它怎么记录是非 GET 请求的呢？用 cookies 啊。**

```
  def set_request_method_cookie
    if request.get?
      cookies.delete(:request_method)
    else
      cookies[:request_method] = request.request_method
    end
  end
```

**跨域重定向：**

```
  def abort_xdomain_redirect
    to_uri = response.headers['Location']
    current = request.headers['X-XHR-Referer']
    unless to_uri.blank? || current.blank? || same_origin?(current, to_uri)
      self.status = 403
    end
  rescue URI::InvalidURIError
  end
```

**重写了 ActionController::Base 的 render, redirect_to 方法：**

```ruby
ActionController::Base.ancestors.select{|a| a.to_s =~ /^Turbolinks/}
 => [Turbolinks::XHRHeaders, Turbolinks::Cookies, Turbolinks::XDomainBlocker, Turbolinks::Redirection] 
```

render 使用的是 Turbolinks.replace
<br>
redirect_to 使用的是 Turbolinks.visit

```ruby
def redirect_to(url = {}, response_status = {})
  turbolinks, options = _extract_turbolinks_options!(response_status)

  value = super(url, response_status)

  if turbolinks || (turbolinks != false && request.xhr? && !request.get?)
    _perform_turbolinks_response "Turbolinks.visit('#{location}'#{_turbolinks_js_options(options)});"
  end

  value
end

def render(*args, &block)
  render_options = args.extract_options!
  turbolinks, options = _extract_turbolinks_options!(render_options)

  super(*args, render_options, &block)

  if turbolinks || (turbolinks != false && options.size > 0 && request.xhr? && !request.get?)
    _perform_turbolinks_response "Turbolinks.replace('#{view_context.j(response.body)}'#{_turbolinks_js_options(options)});"
  end

  self.response_body
end
```

## JS 部分

### 所有对外 API

```ruby
Public API
   Turbolinks.visit(url)
   Turbolinks.pagesCached()
   Turbolinks.pagesCached(20)
   Turbolinks.cacheCurrentPage()
   Turbolinks.enableTransitionCache()
   Turbolinks.disableRequestCaching()
   Turbolinks.ProgressBar.enable()
   Turbolinks.ProgressBar.disable()
   Turbolinks.ProgressBar.start()
   Turbolinks.ProgressBar.advanceTo(80)
   Turbolinks.ProgressBar.done()
   Turbolinks.allowLinkExtensions('md')
   Turbolinks.supported
   Turbolinks.EVENTS
   
# 去除重复的有：
  visit,
  replace,
  pagesCached,
  cacheCurrentPage,
  enableTransitionCache,
  disableRequestCaching,
  ProgressBar: ProgressBarAPI,
  allowLinkExtensions: Link.allowExtensions,
  supported: browserSupportsTurbolinks,
  EVENTS: clone(EVENTS)
```

### 配置项：

cacheSize 默认缓存 10 个页面

transitionCacheEnabled 事务缓存？默认为 false

requestCachingEnabled 请求缓存？默认为 true

progressBar 进度条

currentState 当前状态
<br>
loadedAssets 加载资源

referer 后退链接

xhr 异步请求

### 所有事件：

```
EVENTS =
  BEFORE_CHANGE:  'page:before-change'
  FETCH:          'page:fetch'
  RECEIVE:        'page:receive'
  CHANGE:         'page:change'
  UPDATE:         'page:update'
  LOAD:           'page:load'
  RESTORE:        'page:restore'
  BEFORE_UNLOAD:  'page:before-unload'
  EXPIRE:         'page:expire'
```

## 亮点

### Transition Cache

前面不是说过 turblinks 默认可以缓存 10 个页面吗？
<br>
现在问题来了：如果我要访问一个已经存在缓存里的页面，它是"直接从缓存里取"，还是"改为了 AJAX 实现重新从服务端取"呢？

默认还是从"改为了 AJAX 实现重新从服务端取"。不过，你可以通过以下配置，让它"直接从缓存里取"：

```
Turbolinks.enableTransitionCache();
```

### Partial replacements

你可以使用

```
Turbolinks.visit(path, options)
```

或

```
Turbolinks.replace(html, options)
```

实现局部替换 HTML 文档内容。

## 坑及策略

### 事件的改变

**page:fetch 组成**

运用了 XMLHttpRequest 的实例对象。

```
rememberReferer    # 记住从哪来
cacheCurrentPage   # 缓存当前链接
progressBar?.start # 使用了进度条的话，就给我显示

如果没有开启 Transition Cache，则：
fetchReplacement

如果开启了 Transition Cache，则：
fetchHistory     # 注意，这样是为了进一步加快速度
fetchReplacement # 注意，这一步还是要做的
```

**点击链接：**

```
page:before-change # 链接刚刚被点击，turblinks 刚准备起作用的状态
page:fetch         # 从服务端获取内容
page:receive       # 已经收到服务器返回内容，但还没处理(去掉 header 等)的状态
page:before-unload # 已经处理完成，但还没替换的状态
page:change        # 已切换到新页面的状态
page:load          # 整个点击彻底的完成
```

**而使用浏览器历史返回上一页：**

```
page:before-unload # 页面已经从缓存里取出，准备切换
page:change        # 已切换到新页面的状态
page:restore       # 整个回退彻底的完成
```

**其它：**

```
# 到目前为止，我们就有两类 AJAX 请求了。
# 一是 turblink 转换过来的，二是我们编写的真正的。
# 使用下面这个状态，可以同时监听上述两种 AJAX 引起的页面变化。
page:update

# 手动缓存当前页面
Turbolinks.cacheCurrentPage();

# 页面缓存数量达到上限时，触发它：
page:expire


# 效果和 fetch 一样，只不过多了个判断。
# 如果浏览器支持 turblinks 那么使用 turblinks 的 fetch.
# 如果浏览器不支持 turblinks 那么使用普通的访问
Turbolinks.visit

# 局部替换 HTML 文档内容
Turbolinks.replace
```

## 优点汇总

1. 除了第一次 GET 请求，其它 GET 请求，页面明显速度
2. js 和 css 不需要重新加载、编译
3. 它更像持续进程，而不是垃圾回收

## 其它

### 请按照 AJAX 的方式写 JS 代码。

### To prevent browsers from caching Turbolinks requests:

```ruby
# Globally
Turbolinks.disableRequestCaching()

# Per Request
Turbolinks.visit url, cacheRequest: false
```
This works just like jQuery.ajax(url, cache: false), appending "_#{timestamp}" to the GET parameters.

### 如何移除

```ruby
# 对于基于 Rails 4.0 的新项目，在 Gemfile 中，移除这一行：
gem 'turbolinks' # 移除

# 在 app/assets/javascripts/application.js 中，移除这一行：
//= require turbolinks // 移除
```

### 如果有 js 逻辑绑定了DOMContentLoaded 事件，就需要为 Turbolinks 做兼容

```ruby
# Gemfile:
gem 'jquery-turbolinks'

# Add it to your JavaScript manifest file, in this order:
//= require jquery
//= require jquery.turbolinks
//= require jquery_ujs
//
// ... your other scripts here ...
//
//= require turbolinks
```

### jquery.turbolinks 实践

```
/* BAD: don't bind 'document' events while inside $()! */
$(function() {
  $(document).on('click', 'button', function() { ... })
});

/* Good: events are bound outside a $() wrapper. */
$(document).on('click', 'button', function() { ... })
```

和

```
// BAD: this will not work.
$(document).on('ready', function () { /* ... */ });

// OK: these two are guaranteed to work.
$(document).ready(function () { /* ... */ });
$(function () { /* ... */ });
```

## 三个 data-* 属性

**data-turbolinks-track**

默认情况下，Turbolinks 忽略返回的 head 部分变化，这个属性的作用就是让 Turbolinks 检查 head 内静态文件是否有改变，如有改变则重载页面。

例如返回的页面头部有以下内容：

```
<link href="/assets/application-9bd64a86adb3cd9ab3b16e9dca67a33a.css" rel="stylesheet" type="text/css" data-turbolinks-track>
```

那么 Turbolinks 会检查当前的页面中是否已经记录这个静态文件，如果已记录的静态文件数量和名字跟新页面的不完全相符，就刷新当前页面。

推荐给 head 里面的静态文件链接都加上这个属性，这是为了防止网站的部署更新之后，用户没有刷新页面而一直使用旧的静态文件出现行为或样式异常。

**data-no-turbolink**

如果点击链接的标签或者父级标签拥有 data-no-turbolink 属性，那么这个标签的点击事件不会触发 Turbolinks 访问。

例如以下标签：

```
<a href="/">Home (via Turbolinks)</a>
<div id="some-div" data-no-turbolink>
  <a href="/">Home (without Turbolinks)</a>
</div>
```

前一个链接会发起 Turbolinks 请求，后一个不会。

如果网站按功能区分了不同的区域，各自使用不同的静态文件，不适合全部打包成一个，那么就在切换这些功能区域的链接上加上这个属性。

**data-turbolinks-eval**

如果一个 script 标签拥有 data-turbolinks-eval 属性，那么这个 script 标签只有在这个页面是直接访问的页面时才会执行，通过 Turbolinks 访问则不会执行。

例如以下 script 标签存在于 body 之中：

```
<script type="text/javascript" data-turbolinks-eval="false">
  console.log("I'm only run once on the initial page load");
</script>
```

那么这段代码只有在这个页面是直接访问的时候才会执行。目前还没发现必须使用这个属性的情景。

### 与 Assets Pipeline 配合

**打包成一个，但可以根据不同的模块区分布局**

Turbolinks 忽略 head 内的变化，细化静态文件按需载入的做法会失效。如果前端逻辑不是特别复杂，那么最好打包成一个。

打包成一个也不一定全站只有一个静态 js 和 css，可以根据不同的模块区分布局，每个布局有自己的静态文件，布局页面间的链接加上 data-no-turbolink。

例如有两个布局 application 和 admin，那么 js 代码可以这样组织：

```
app/assets/javascripts/application/
├─ ...
app/assets/javascripts/admin/
├─ ...
app/assets/javascripts/application.js.coffee
app/assets/javascripts/admin.js.coffee
```

application.js 和 admin.js 是最终编译出来的 js 文件，内容类似：

```
# in application.js.coffee
#= require jquery
#= require jquery_ujs
#= require jquery.turbolinks
#= require_tree ./application
```

```
# in admin.js.coffee
#= require jquery
#= require jquery.turbolinks
#= require_tree ./admin
```

**页面特定的逻辑**

在整站打包一个 js 文件的情况下，可以用以下办法区分页面特定逻辑。

在 layout 中设置页面 ID：

```
<body id="<%= controller_name%>-<%= action_name %>">
  ...
</body>

<!--
Example:

<body id="topics-show">
</body>
!--
```

这样页面特定逻辑就可以根据是否有页面 ID 进行判断（已加载 jquery.turbolinks）：

```
$ ->
  if($('#topics-show').length)
    # topics show logic
```

**区域特定的逻辑**

对于垮页面的逻辑，可以根据特定元素的 ID 触发。

```
<div id="sidebar">
  ...
</div>
```

```
$ ->
  if($('#sidebar')).length
    # sidebar logic
```

### 注意事项

Turbolinks 开启后，网站将成为一个单页应用，页面切换不释放 js 逻辑。这意味着编写前端逻辑的时候需要留意内存泄露或者其他持久页面导致的问题。

**避免全局变量**

在单页面应用中，js 全局变量将真的成为全局变量——除非刷新，否则变量会进入所有访问页面。

所有 js 逻辑建议放在匿名空间中执行：

```
// wrong
var foo = 'bar'; // 全局变量

// right
(function() {
  var foo = 'bar'; // 匿名空间内的局部变量
})();
```

如果使用 CoffeeScript，那么所有编译结果都已经包裹在匿名空间内：

```
# right in coffee
foo = 'bar'
```

所以推荐使用 CoffeeScript。

**清理定时任务**

如果某个页面设置了定时任务，记得加上退出逻辑。

```
$ ->
  if $('articles-index').length
    updateInterval = setInterval ->
      doSomething()
    , 10 * 1000

    $(document).one 'page:change', ->
      clearInterval updateInterval
```

**清理没有释放的绑定**

绑定在 document 和 window 上的事件是在页面切换后是保留的，记得清理。

```
$ ->
  if $('articles-index').length
    $(window).on 'scroll', ->
      doSomething()

    $(document).one 'page:change', ->
      $(window).off 'scroll'
```

### 原来的 DOM 操作怎么办？并且它们是非幂等的

DOM transformations that are idempotent are best. If you have transformations that are not, bind them to page:load (in addition to the initial page load) instead of page:change (as that would run them again on the cached pages):

```
// using jQuery for simplicity

$(document).on("ready page:load", nonIdempotentFunction);
```

## 其它类和对象


**Click**
管理着链接和点击事件，触发时会进行检查。如果符合 trublinks 则用 AJAX 执行请求，否则改用普通的请求。

**ProgressBar**
进度条。对应 class name 为 turbolinks-progress-bar

**ProgressBarAPI**
管理进度条可用的接口。有：

```
enable
disable
start
advanceTo
done
```

**CSRFToken**
管理 csrf token，提供接口：

```
get
update
```

**Link**
对于已经存在的链接，进行一些特殊处理。
比如，前面提到过的，有的链接不能使用 turblinks

**各种浏览器不同引起的问题**

**细节**

```
fetch
transitionCacheFor
enableTransitionCache
disableRequestCaching
fetchReplacement
fetchHistory
cacheCurrentPage
pagesCached
constrainPageCacheTo
replace
changePage
findNodes
findNodesMatchingKeys
swapNodes
executeScriptTags
removeNoscriptTags
setAutofocusElement
reflectNewUrl
reflectRedirectedUrl
crossOriginRedirect
rememberReferer
rememberCurrentUrl
rememberCurrentState
manuallyTriggerHashChangeForFirefox
recallScrollPosition
resetScrollPosition
clone
popCookie
uniqueId
triggerEvent
pageChangePrevented
processResponse
extractTitleAndBody
createDocument

bypassOnLoadPopstate
installDocumentReadyPageEventTriggers
installJqueryAjaxSuccessPageUpdateTrigger
installHistoryChangeHandler
initializeTurbolinks

browserSupportsTurbolinks
# 等价于下面 3 者
browserSupportsPushState
browserIsntBuggy
requestMethodIsSafe
```

参考：

[Turbolinks 向导](http://chloerei.com/2013/07/14/turbolinks-guide/)
[Turbolinks 后端逻辑分析](http://chloerei.com/2013/06/30/turbolinks-backend/)
[Turbolinks](https://github.com/rails/turbolinks)
[jquery.turbolinks](https://github.com/kossnocorp/jquery.turbolinks)
