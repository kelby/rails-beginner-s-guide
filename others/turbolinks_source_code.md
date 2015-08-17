## Turbolinks 补充

简单理解概念：把原来所有 GET 请求(包括链接、重定向、浏览器的后退)都通过 AJAX 来实现，并且 url 是变化的。

使用 turblinks 的话，注意：一，尽量从 AJAX 的角度出发写监听事件；二，尽量使用 turblinks 提供的监听事件。

#### Ruby 部分

**重写了 ActionController::Base 里的 redirect_to 和 render 方法**

```ruby
ActionController::Base.ancestors.select{|a| a.to_s =~ /^Turbolinks/}
 => [Turbolinks::XHRHeaders, Turbolinks::Cookies,
     Turbolinks::XDomainBlocker, Turbolinks::Redirection] 
```

render 使用的是 Turbolinks.replace
<br>
redirect_to 使用的是 Turbolinks.visit

**处理 :back 这种情况，使用 X-XHR-Referer 标识**

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

**覆盖了 Redirect 的 call 方法，修复 redirect_to 里的 bug**

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

**其它几点 **

一，3 个钩子

```ruby
before_action :set_xhr_redirected_to, :set_request_method_cookie
after_action :abort_xdomain_redirect
```

1) 因为 trublinks 改变了原来 redirect_to 的行为，带来了新的问题，然后它又提供了解决办法：

```ruby
  def set_xhr_redirected_to
    if session && session[:_turbolinks_redirect_to]
      response.headers['X-XHR-Redirected-To'] = session.delete :_turbolinks_redirect_to
    end
  end
```

对应 redirect_to :back 这种情况，使用 X-XHR-Referer 标识；用 X-XHR-Redirected-To 记录重定向的目标地址。

2) 它怎么记录是非 GET 请求的呢？用 cookies 记录。

```ruby
  def set_request_method_cookie
    if request.get?
      cookies.delete(:request_method)
    else
      cookies[:request_method] = request.request_method
    end
  end
```

对于非 GET 请求，turblinks 是不起作用的。

3) 跨域重定向：

```ruby
  def abort_xdomain_redirect
    to_uri = response.headers['Location']
    current = request.headers['X-XHR-Referer']
    unless to_uri.blank? || current.blank? || same_origin?(current, to_uri)
      self.status = 403
    end
  rescue URI::InvalidURIError
  end
```

某种情况下，禁止访问。

#### JS 部分

**所有对外 API**

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
  ProgressBar: ProgressBarAPI (包括：enable, disable, start, addvanceTo, done),
  allowLinkExtensions: Link.allowExtensions,
  supported: browserSupportsTurbolinks,
  EVENTS: clone(EVENTS)
```

**配置项：**

cacheSize 默认缓存 10 个页面

transitionCacheEnabled 事务缓存？默认为 false

requestCachingEnabled 请求缓存？默认为 true

progressBar 进度条

currentState 当前状态
<br>
loadedAssets 加载资源

referer 后退链接

xhr 异步请求

**所有事件：**

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

具体做了什么事，如何实现，可以查看对应源代码，在此不做讨论。

#### 亮点

**Transition Cache**

前面不是说过 turblinks 默认可以缓存 10 个页面吗？
<br>
现在问题来了：如果我要访问一个已经存在缓存里的页面，它是"直接从缓存里取"，还是"改为了 AJAX 实现重新从服务端取"呢？

默认还是从"改为了 AJAX 实现重新从服务端取"。不过，你可以通过以下配置，让它"直接从缓存里取"：

```
Turbolinks.enableTransitionCache();
```

**Partial replacements**

你可以使用

```
Turbolinks.visit(path, options)
```

或

```
Turbolinks.replace(html, options)
```

实现局部替换 HTML 文档内容。

#### 坑及策略

**事件的改变**

1) 点击链接：

```ruby
page:before-change # 链接刚刚被点击，turblinks 刚准备起作用的状态
page:fetch         # 从服务端获取内容
page:receive       # 已经收到服务器返回内容，但还没处理(去掉 header 等)的状态
page:before-unload # 已经处理完成，但还没替换的状态
page:change        # 已切换到新页面的状态
page:load          # 整个点击彻底的完成
```

2) 而使用浏览器历史返回上一页：

```ruby
page:before-unload # 页面已经从缓存里取出，准备切换
page:change        # 已切换到新页面的状态
page:restore       # 整个回退彻底的完成
```

3) 其它：

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

#### JS 要如何更改使用？

**jquery.turbolinks 实践，请按照 AJAX 的方式写 JS 代码。**

```javascript
/* 不要这么写 */
$(function() {
  $(document).on('click', 'button', function() { ... })
});

/* 而是这么写 */
$(document).on('click', 'button', function() { ... })
```

和

```javascript
// 不要这么写
$(document).on('ready', function () { /* ... */ });

// 而是这么写
$(document).ready(function () { /* ... */ });
$(function () { /* ... */ });
```

**怎样防止浏览器缓存 Turbolinks 请求:**

```ruby
# 全局
Turbolinks.disableRequestCaching()

# 每个请求
Turbolinks.visit url, cacheRequest: false
```

像 jQuery.ajax(url, cache: false), 会被追加 "_#{timestamp}" 到 GET 请求参数里。

**如何移除**

```ruby
# Gemfile

# 注释这一行：
gem 'turbolinks'
```

```ruby
# app/assets/javascripts/application.js

# 移除这一行：
//= require turbolinks
```

**如果有 js 逻辑绑定了DOMContentLoaded 事件，就需要为 Turbolinks 做兼容**

语法或历史遗留问题，可以使用以下方法：

```ruby
# Gemfile:
gem 'jquery-turbolinks'
```

```coffee
# Add it to your JavaScript manifest file, in this order:
//= require jquery
//= require jquery.turbolinks
//= require jquery_ujs
//
// ... your other scripts here ...
//
//= require turbolinks
```

**几个 data-* 属性**

data-no-transition-cache

data-turbolinks-permanent

data-no-turbolink

data-method, data-remote, or data-confirm

data-turbolinks-track

data-turbolinks-eval

data-turbolinks-temporary

**原来的 DOM 操作怎么办？并且它们是非幂等的**

DOM 操作最好是幂等。如果不是幂等的，你可以使用使用 page:load 而不是 page:change

```javascript
// using jQuery for simplicity

$(document).on("ready page:load", nonIdempotentFunction);
```

#### 其它类和对象

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
done

advanceTo
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

如何替换链接？

由 JS 实现。

如何防止丢失 Referer ？

压根就没有丢失，只是原来的不能用，用新的 X-XHR-Referer

参考：

[Turbolinks](https://github.com/rails/turbolinks)
<br>
[jquery.turbolinks](https://github.com/kossnocorp/jquery.turbolinks)
