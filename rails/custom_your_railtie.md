## 定制自己的 Railtie

### 一，继承于 Rails::Railtie

继承于 Rails::Railtie，即可创建自己的 Railtie. 在 Rails 启动的时候，它也会被执行。

举例：

```ruby
# lib/my_gem/railtie.rb
module MyGem
  class Railtie < Rails::Railtie
  end
end

# lib/my_gem.rb
require 'my_gem/railtie' if defined?(Rails)
```

创建 Railtie，除了 lib/my_gem/railtie.rb 里继承于 Rails::Railtie 外，你不需要更改其它地方的代码。并不影响 my_gem 原来要做的事，也正如此，my_gem 也可以在 Rails 之外使用。

### 二，编写自己的 my_railtie/railtie.rb 文件内容

TODO

### 三，编写 Railtie 内容

TODO

### 四，在应用启动时调用定制的 Railtie

TODO
