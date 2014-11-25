## Action Controller 概览

Controller 里的 public方法(也就是action) 会自动对应 Route 里的路由规则。当请求到来时，action 接受请求并处理，最后渲染相应视图模板(Get-and-show)或重定向到另一 action(do-and-redirect)。

默认，只有 ApplicationController 直接继承于 ActionController::Base，其它的控制器继承于 ApplicationController。所以，如果你想在所有 controller 处理之前做一些什么，你可以把它们写在 ApplicationController 里。

## Metal

**ActionController::Metal** 本身是一个功能极简的 Controller, 但它符合 Rack 接口规范，所以也可以把它称之为 Rack application. 相比于 **ActionController::Base** 它的功能真的很有限。

如你的 Rails 项目对性能要求比较高，或者说对实时性要求比较高，又或者你的项目做为 API 对外提供服务，那么你可以尝试直接继承使用 Metal.

> Rails Metal is a subset of Rack middleware

## Metal 之外

Metal 仅包含 metal.rb 这个文件，不包含其同名目录。下面对 metal/ 目录下面包含的东西，做一下简述：

## 其它

ActionController include 了这些模块，而我们自定义的 Controller 又继承于 ActionController.

自然的，它们的 ClassMethods 就会变成我们自定义 Controller 的类方法，而其它方法则类似实例方法，可运用于 action.

## 参考

[Introducing Rails Metal](http://weblog.rubyonrails.org/2008/12/17/introducing-rails-metal/)<br>
[Rails on Rack](http://edgeguides.rubyonrails.org/rails_on_rack.html)<br>
[Developing api with rails metal](http://www.slideshare.net/artellectual/developing-api-with-rails-metal)<br>
[Rails Metal](http://railscasts.com/episodes/150-rails-metal)<br>
