ActionDispatch 和 ActionController 都有 Flash 相关的代码，统一把它们放到这里来讲。

-----------

## 基本使用
类似 Hash，设置 flash

```ruby
class PostsController < ActionController::Base
  def create
    # save post
    flash[:notice] = "Post successfully created"
    redirect_to @post
  end

  def show
    # doesn't need to assign the flash notice to the template, that's done automatically
  end
end
```

类似 Hash, 读取 flash

```
# show.html.erb
<% if flash[:notice] %>
  <div class="notice"><%= flash[:notice] %></div>
<% end %>
```

## alert 和 notice
因为 alert 和 notice 类型的 flash 太常见，所以提供了语法糖，你还可以这么写：

```ruby
# 设置
flash.alert = "You must be logged in"
flash.notice = "Post successfully created"

# 读取
flash.alert
flash.notice
```

> **Note:** 其它 flash_type 默认不能这么写

上面是由 ActionDispatch 提供，下面由 ActionController 提供 (基于上面)。

## add_flash_types

觉得上面的写法还是不够简短，觉得 notice 和 alert 类型不够用？使用 **add_flash_types**

```
# in application_controller.rb
class ApplicationController < ActionController::Base
  add_flash_types :warning
end

# in your controller
redirect_to user_path(@user), warning: "Incomplete profile"

# in your view
<%= warning %>
```

两种效果：视图里可以直接有同名 warning 辅助方法，redirect_to 里可直接使用 warning。

它们和 flash[:warning] 或 flash.warning 和 flash: { warning: "Incomplete profile" } 效果一样。

## 还是 alert 和 notice

alert 和 notice 默认已经使用 add_flash_types

## 最后

也许，你还看过一种写法 flash.now[:flash_type]

它和 flash[:flash_type] 看起来有一点点不同，但效果是一样的，在此不再对它做讲解，统一使用后者。
