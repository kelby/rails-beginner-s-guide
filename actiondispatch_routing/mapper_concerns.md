### Concerns

重复使用已经定义的路由，避免 `config/routes.rb` 里出现重复代码。

| 方法 | 解释 |
| -- | -- |
| concern | 定义(一次只能定义一个) |
| concerns | 调用(一次可调用一个或多个) |

举例(使用 concern & concerns 之前)：

```ruby
AppName::Application.routes.draw do
  resources :messages do
    resources :comments
    resources :categories
    resources :tags
  end

  resources :posts do
    resources :comments
    resources :categories
    resources :tags
  end

  # ...
end
```

举例(使用 concern & concerns 之后)：

```ruby
AppName::Application.routes.draw do
  concern :sociable do
    resources :comments
    resources :categories
    resources :tags
  end

  resources :messages do
    concerns :sociable
  end

  resources :posts do
    concerns :sociable
  end

  # ...
end
```

再次举例：

```ruby
concern :commentable do
  resources :comments
end

concern :image_attachable do
  resources :images, only: :index
end

# concerns 可以跟多个 concern
resources :messages, concerns: [:commentable, :image_attachable]

namespace :posts do
  concerns :commentable
end
```

**一些疑问？**

即使是去除重复代码，也还有其它方法实现。如：

```ruby
AppName::Application.routes.draw do
  def add_posts
    resources :posts, :only => [:create, :destroy]
  end

  resources :events do
    add_posts
  end
end
```

有必要使用 DSL 吗？

在 [讨论](https://github.com/rails/rails/commit/0dd24728a088fcb4ae616bb5d62734aca5276b1b#commitcomment-1749011) 里 dhh 回答了此问题。
