# Autoload - 实例变量的运用

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

## Autoload

自动加载和急加载。

This module allows you to define autoloads based on Rails conventions (i.e. no need to define the path it is automatically guessed based on the filename) and also define a set of constants that needs to be eager loaded:

```ruby
module MyLib
  extend ActiveSupport::Autoload

  autoload :Model

  eager_autoload do
    autoload :Cache
  end
end
```

Then your library can be eager loaded by simply calling:

```ruby
MyLib.eager_load!
```

```ruby
autoload, autoload_at, autoload_under, autoloads
eager_autoload, eager_load!
```

1. 线程安全
2. 有没有必要(不使用但也要加载？)
3. 对性能的影响

自动加载 和 线程安全

Many libraries and frameworks (including Ruby on Rails) use a feature of Ruby known as autoload, which allows components of a library to be lazily loaded only when the constant is resolved in the code path of an executing program. The problem with this feature is that, historically, the implementation has not been thread-safe. In other words, if two threads tried to resolve an autoloaded constant at the same time, weird things would happen. This problem was finally tackled in Ruby 1.9.1 but then regressed in 1.9.2 and re-resolved in 1.9.3 (but only in a later patchlevel), causing a bit of confusion around whether `autoload` is actually safe to use in a threaded Ruby program.

2.0 里面的线程安全

For all intents and purposes, autoloading of modules should be considered thread-safe in Ruby 2.0.0p0, as the patch was officially merged into the 2.0 branch prior to release. Any thread-safety issues in Ruby 2.0 should be considered regressions, according to that ticket.

进入 Eager Loading

Of course, guaranteeing support for Ruby 2.0 is not entirely sufficient for most programs still running on 1.9.x, and in some cases, 1.8.x, so you may need to use a more backward-compatible strategy. In Ruby on Rails, this was solved with an `eager_autoload` method that forcibly loads all modules marked to be lazily loaded. If you are running threaded code, it is recommended that you call this prior to launching threads. Note that in Rails 4.0, the framework will eager load all modules by default, which should help you avoid having to think about these threading issues.

