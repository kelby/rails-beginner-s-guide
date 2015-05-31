## Flash

Action Controller 和 Action Dispatch 都有 Flash 相关的代码。

### 基本使用

类似 Hash，设置 flash

```ruby
class PostsController < ActionController::Base
  def create
    # save post
    flash[:notice] = "Post successfully created"
    redirect_to @post
  end

  def show
    # ...
  end
end
```

类似 Hash, 读取 flash

```ruby
# show.html.erb
<% if flash[:notice] %>
  <div class="notice"><%= flash[:notice] %></div>
<% end %>
```

### alert 和 notice

因为 alert 和 notice 类型的 flash 太常见，所以提供了语法糖，你还可以这么写：

```ruby
# 设置
flash.alert = "You must be logged in"
flash.notice = "Post successfully created"

# 读取
flash.alert
flash.notice
```

> Note: 其它 flash_type 默认不能这么写

上面是由 Action Dispatch 提供，下面由 Action Controller 提供。

### add_flash_types 方法

觉得上面的写法还是不够简短，觉得 notice 和 alert 类型不够用？使用 `add_flash_types`

```ruby
# app/cotrollars/pplication_controller.rb
class ApplicationController < ActionController::Base
  add_flash_types :warning, :success, :danger
end

# View 代码
<%= warning %>

# Controller 代码
redirect_to user_path(@user), warning: "Incomplete profile"
```

两种效果：视图里可以直接有同名 `warning` 辅助方法，`redirect_to` 里可直接使用 `:warning`.

它们和 flash[:warning] 或 flash.warning 和 flash: { warning: "Incomplete profile" } 效果一样。

### 还是 alert 和 notice

alert 和 notice 默认已经使用 add_flash_types

### flash.now[:flash_type]

也许，你还看过一种写法 `flash.now[:flash_type]`

flash[:flash_type] 消息的生命周期可到下一个 action. 所以，通常搭配 redirect_to 使用。  
flash.now[:flash_type] 消息的生命周期仅限于本 action. 所以，通常搭配 render 使用。

以 update 为例：如果成功则跳转到新页面，那么可用 flash[:flash_type]; 失败则停留在当前页面，那么可用 flash.now[:flash_type].

另，当你意外的发现提醒消息在一个页面出现，在下一个页面还是出现，不妨改为 flash.now[:flash_type] 试试。
