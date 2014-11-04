# ActionView 非渲染 & 非 helper 方法

下面这些方法，和渲染没有直接关联，并且严格意义上讲不属于 helper 方法

## Record Identifier

`dom_class` 和 `dom_id` 根据所传递的对象，生成能代表其身份的"字符串"，可配合其它 helper 一起使用。

使用举例：

```ruby
dom_class(post)   # => "post"
dom_class(Person) # => "person"

# 带前缀
dom_class(post, :edit)   # => "edit_post"
dom_class(Person, :edit) # => "edit_person"
```

```ruby
dom_id(Post.find(45))       # => "post_45"
dom_id(Post.new)            # => "new_post"

# 带前缀
dom_id(Post.find(45), :edit) # => "edit_post_45"
dom_id(Post.new, :custom)    # => "custom_post"
```

实现它们时用到了 ActiveModel::Model 里的方法。

## Routing Url For

`url_for`

有多个 url_for 方法，这里指的是 View 里用到的。极端情况下会用到 ActionDispatch 里的东西。

它生成的内容是 url，区别于 link_to 等生成的是链接。

## Layouts

`layout`

影响渲染的效果，但和渲染不直接相关。

```ruby
# String
class InformationController < BankController
  layout "information"
end
```

```ruby
# Symbol
class VaultController < BankController
  layout :access_level_layout
end
```

```ruby
# false
class TillController < BankController
  layout false
end
```

```ruby
# nil
class CommentsController < ApplicationController
  # Will search for "comments" layout and fallback "application" layout
  layout nil
end
```

此外，layout 有继承关系：

```ruby
class ApplicationController < ActionController::Base
  layout "application"
end

class PostsController < ApplicationController
  # Will use "application" layout
end
```

此外，只有 false 没有 true (要渲染可以直接指明 layout 或者不指明使用默认，但不能使用 true，会报错的):

```ruby
# false
class TillController < BankController
  layout false
end
```

此外，还有可选参数 `:only` 和 `:except`

