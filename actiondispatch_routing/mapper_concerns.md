### Concerns

为了防止 `config/routes.rb` 里的重复代码而来的。

`concern(name, callable = nil, &block)` 定义(一次只能定义一个)
`concerns(*args)` 调用(一次可调用多个)

```ruby
concern :commentable do
  resources :comments
end

concern :image_attachable do
  resources :images, only: :index
end

# 配合 resources
resources :messages, concerns: [:commentable, :image_attachable]

# 配合 scope 或 namespace
namespace :posts do
  concerns :commentable
end
```
