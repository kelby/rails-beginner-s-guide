## Layouts

`layout`

布局，影响渲染的效果，但和渲染没有直接关联。

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
# nil
class CommentsController < ApplicationController
  # 始终使用默认的 layout (首先是 comments，然后是 application 的)
  layout nil
end
```

```ruby
# false
class TillController < BankController
  layout false
end
```

此外，还有可选参数 `:only` 和 `:except`

> Note: 没有 layout true 这种写法，会报错的。

layout 有继承关系：

```ruby
class ApplicationController < ActionController::Base
  layout "application"
end

class PostsController < ApplicationController
  # 继承使用 "application" layout
end
```
