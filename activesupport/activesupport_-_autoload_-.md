# ActiveSupport - Autoload - 实例变量的运用

## Autoload

### 使用及原理

```ruby
module MyLib
  extend ActiveSupport::Autoload

  autoload :Model

  eager_autoload do
    autoload :Cache
  end
end

# 本质是 require
MyLib.eager_load!
```

原理，设置和使用这几个变量：

```ruby
# 加载结果
@_autoloads = {}

# 被加载对象所在路径，默认是约定优于配置
@_under_path = nil

# 不约定优于配置，那你就指定加载路径
@_at_path = nil

# 是否 eager autoload ?
@_eager_autoload = false
```

加载，并不是真正的加载，而是先保存中间状态(路径保存在@_autoloads)，真正需要的时候才加载(会触发eager_load!)。这和 ActiveRecord 里的 Relation 原理差不多。

--------

### require 隐藏的问题 - 要解决的问题

```ruby
require "some/library"
```

Some libraries work great by simply using requires, but as they grow, some of them tend to rely on autoload techniques to **avoid loading all up-front**. autoload is particularly important on Rails applications because it helps boot time to stay low in development and test environments, since we will load modules as we need them.

### 引入 autoload 后又发现新问题

```ruby
module Foo
  autoload :Bar, "path/to/bar"
end
```

Now, the first time Foo::Bar is accessed, it will be automatically loaded. The issue with this approach is that it is not **thread-safe**, except for latest JRuby versions (since 1.7) and Ruby master (2.0).
This means that eager loading these modules on boot (i.e. loading it all up-front) is essential for **multi-threaded** production servers, otherwise it could lead to scenarios where your application tries to **access a module that does not exist yet when it just booted**.

第一次访问后，flag 就设置为已经加载(防止重复加载)，然后就不再加载。但那是在另一线程进行的，对本线程无效。

In **multi-process** servers, we may or may not want to eager load them. For instance, if your web server uses fork to handle each request (like Unicorn, Passenger), loading them up-front will become more important as we move towards Ruby 2.0 which will provide copy-on-write semantics. In such cases, if we load Foo::Bar on boot, we will have one copy of Foo::Bar shared between all processes otherwise each process will end up loading its own copy of Foo::Bar possibly leading to higher memory usage.
The downside of eager loading is that we may eventually load code that your application never uses, increasing memory usage. This not a problem if your server uses fork or threads, it is exactly what we want (share and load the most we can), but in case you server doesn’t (for example, Thin) nothing bad is going to happen.

```ruby
def autoload(const_name, path = @_at_path)
  unless path
    full = [name, @_under_path, const_name.to_s].compact.join("::")
    path = Inflector.underscore(full)
  end

  if @_eager_autoload
    @_autoloads[const_name] = path
  end

  super const_name, path
end

def eager_autoload
  old_eager, @_eager_autoload = @_eager_autoload, true
  yield
ensure
  @_eager_autoload = old_eager
end

# 本质是 require
def eager_load!
  @_autoloads.values.each { |file| require file }
end
```


[Eager loading for greater good](http://blog.plataformatec.com.br/2012/08/eager-loading-for-greater-good/)<br>
[Threading with the AWS SDK for Ruby](http://ruby.awsblog.com/blog/tag/autoload)<br>
[Ruby on Rails 线程安全代码](http://ruby-china.org/topics/10932)<br>
[线程安全 - 概述](http://baike.baidu.com/view/1298606.htm#1)

