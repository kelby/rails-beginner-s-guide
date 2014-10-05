
## Helpers

不是指辅助方法！

`helper(*args, &block)` 把 Helper 方法变成 Controller 方法。

参数类型，可分为 3 类：String、Symbol，Module，block。这些参数还可以混合使用。

- 当参数是模块名时，直接 include 此模块 (没有 requires )

```
helper FooHelper # => includes FooHelper
```

- 而当参数是字符串或符号时，会根据约定 require 相关文件，并 include 相关模块。

```
helper :foo             # => requires 'foo_helper' and includes FooHelper
helper 'resources/foo'  # => requires 'resources/foo_helper' and includes Resources::FooHelper
```

此外，helper 可以接受并处理一个代码块。(用得不多，并且不推荐)

最后要说的提，上述说的参数类型可以混合使用，你可以同时传递符号、字符串、模块和代码块给 helper 方法。

```ruby
helper(:three, BlindHelper) { def mice() 'mice' end }
```

`helper_method(*meths)` 把 Controller 方法变成 Helper 方法。

如下文举例，将 current_user 方法，可以在视图里使用：

```ruby
class ApplicationController < ActionController::Base
  helper_method :current_user, :logged_in?

  def current_user
    @current_user ||= User.find_by(id: session[:user])
  end

  def logged_in?
    current_user != nil
  end
end
```

在视图里:

```ruby
<% if logged_in? %>Welcome, <%= current_user.name %><% end %>
```

具体实现(以元编程的形式定义定义同名方法，然后 send 调用原 helper 方法)

```ruby
meths.each do |meth|
  _helpers.class_eval <<-ruby_eval, __FILE__, __LINE__ + 1
    def #{meth}(*args, &blk)                               # def current_user(*args, &blk)
      controller.send(%(#{meth}), *args, &blk)             #   controller.send(:current_user, *args, &blk)
    end                                                    # end
  ruby_eval
end
```

helper 和 helper_method 可以简单理解为一对作用相反的操作。
