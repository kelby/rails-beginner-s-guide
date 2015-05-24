## Force SSL

**实例方法：**

```
force_ssl_redirect
```

重定向到 https 链接, url 会改变。  
重定向这部分用到了 redirect_to 方法，提供可选参数 :status, :flash, :alert, :notice

如果没有(用参数)指定链接，则用当前链接，但是 https 协议访问。可用于注册、登录等页面。
链接这部分用到了 url_for 方法，提供可选参数 :protocol, :host, :domain, :subdomain, :port, :path

上面是实例方法，对单个 action 有用；  
下面这是类方法，它封装了 force_ssl_redirect，对整个 Controller 有用。

**类方法：**

```
force_ssl
```

可指定 action 跳转，可选参数 :only, :except；或根据条件跳转，可选参数 :if, :unless

使用举例：

```ruby
class AccountsController < ApplicationController
  force_ssl if: :ssl_configured?

  def ssl_configured?
    !Rails.env.development?
  end
end
```
