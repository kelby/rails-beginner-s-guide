## 定制自己的 Railtie

#### 一，继承于 Rails::Railtie

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

#### 二，编写自己的 my_railtie/railtie.rb 文件内容

可用 Rails::Railtie 提供的方法。

#### 三，编写 Railtie 内容

做了上述两步后，就是编写内容。该做什么事，做什么事；该完成什么功能，完成什么功能。

#### 四，在应用启动时自动调用定制的 Railtie

自动运行 my_railtie/railtie.rb 里面的相关启动代码。
