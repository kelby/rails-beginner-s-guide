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
