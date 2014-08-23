# ActiveSupport 概览

## Cache

`expand_cache_key(key, namespace = nil)`

Expands out the key argument into a key that can be used for the cache store. Optionally accepts a namespace, and all keys will be scoped within that namespace.

If the key argument provided is an array, or responds to to_a, then each of elements in the array will be turned into parameters/keys and concatenated into a single key. For example:

```ruby
expand_cache_key([:foo, :bar])               # => "foo/bar"
expand_cache_key([:foo, :bar], "namespace")  # => "namespace/foo/bar"
```

The key argument can also respond to cache_key or to_param.

`lookup_store(*store_option)`

Creates a new CacheStore object according to the given options.

If no arguments are passed to this method, then a new `ActiveSupport::Cache::MemoryStore` object will be returned.

If you pass a Symbol as the first argument, then a corresponding cache store class under the ActiveSupport::Cache namespace will be created. For example:

```ruby
ActiveSupport::Cache.lookup_store(:memory_store)
# => returns a new ActiveSupport::Cache::MemoryStore object

ActiveSupport::Cache.lookup_store(:mem_cache_store)
# => returns a new ActiveSupport::Cache::MemCacheStore object
```

Any additional arguments will be passed to the corresponding cache store class's constructor:

```ruby
ActiveSupport::Cache.lookup_store(:file_store, '/tmp/cache')
# => same as: ActiveSupport::Cache::FileStore.new('/tmp/cache')
```

If the first argument is not a Symbol, then it will simply be returned:

```ruby
ActiveSupport::Cache.lookup_store(MyOwnCacheStore.new)
# => returns MyOwnCacheStore.new
```

**MemoryStore**

A cache store implementation which stores everything into memory in the same process. If you're running multiple Ruby on Rails server processes (which is the case if you're using mongrel_cluster or Phusion Passenger), then this means that Rails server process instances won't be able to share cache data with each other and this may not be the most appropriate cache in that scenario.

This cache has a bounded size specified by the :size options to the initializer (default is 32Mb). When the cache exceeds the allotted size, a cleanup will occur which tries to prune the cache down to three quarters of the maximum size by removing the least recently used entries.

MemoryStore is thread-safe.

```
cleanup(options = nil)
Preemptively iterates through all stored keys and removes the ones which have expired.

clear(options = nil)

decrement(name, amount = 1, options = nil)
Decrement an integer value in the cache.

delete_matched(matcher, options = nil)

increment(name, amount = 1, options = nil)
Increment an integer value in the cache.

prune(target_size, max_time = nil)
To ensure entries fit within the specified memory prune the cache by removing the least recently accessed entries.

pruning?()
Returns true if the cache is currently being pruned.
```

## Callbacks

`define_callbacks(*names)`
Define sets of events in the object life cycle that support callbacks.
定义多个 callback，但不能传递类型，只能使用默认的，也就是 before 类型。基于 `set_callback`

会有 "_#{name}_callbacks"

```ruby
define_callbacks :validate
define_callbacks :initialize, :save, :destroy
```

`set_callback(name, *filter_list, &block)` 是调用，不是设置！
Install a callback for the given event.
第二个参数 filter_list 只能是 :before, :after, 或 :around 里的一个或多个，如果不设置，默认是 :before

```ruby
# before save 执行 before_meth
set_callback :save, :before, :before_meth

# 如果满足条件，在 after save 执行 after_meth
set_callback :save, :after,  :after_meth, if: :condition

# around save 始终执行后面的 lambda
set_callback :save, :around, ->(r, &block) { stuff; result = block.call; stuff }
```

除了上述方法外，还有：

```ruby
reset_callbacks(name)
skip_callback(name, *filter_list, &block)
```

上面给的方法，无论是定义还是调用都是通过名字的，如果你不想要名字，直接调用 `run_callbacks(kind, &block)`

> 使用举例：AbstractController::Callbacks 和 ActiveModel::Callbacks，在它们的章节会有介绍。

## Concern

以前：

```ruby
module M
  def self.included(base)
    base.extend ClassMethods
    base.class_eval do
      # 执行某些方法
      scope :disabled, -> { where(disabled: true) }

      # 执行某些方法
      include InstanceMethods
    end
  end

  # 定义类方法
  module ClassMethods
    ...
  end

  # 定义实例方法
  def a_instance_method
    ...
  end

  # 如果实例方法比较多，可以单独成 module，对应上面的 include InstanceMethods
  module InstanceMethods
    ...
  end
end
```

现在：

```ruby
require 'active_support/concern'

module M
  extend ActiveSupport::Concern

  included do
    # 执行某些方法
    scope :disabled, -> { where(disabled: true) }

    # 执行某些方法
    include InstanceMethods
  end

  # 定义类方法
  module ClassMethods
    ...
  end

  # 定义实例方法
  def a_instance_method
    ...
  end

  # 如果实例方法比较多，可以单独成 module，对应上面的 include InstanceMethods
  module InstanceMethods
    ...
  end
end
```

只有很小的区别，对于定义类方法，不需要手动 base.extend ClassMethods

## Inflector

## Rescuable

`rescue_from(*klasses, &block)`

klasses 表示一个或多个异常类。如果有 :with 选项，则用其 value(一般是个方法) 处理；否则，需要传递 block 来处理。

```ruby
class ApplicationController < ActionController::Base
  # 一个或多个异常类，有 :with 选项
  rescue_from User::NotAuthorized, with: :deny_access # self defined exception
  rescue_from ActiveRecord::RecordInvalid, with: :show_errors

  # 一个或多个异常类，传递 block
  rescue_from 'MyAppError::Base' do |exception|
    render xml: exception, status: 500
  end

  protected
    def deny_access
      ...
    end

    def show_errors(exception)
      exception.record.new_record? ? ...
    end
end
```

实现：类似'实例变量的运用'，`rescue_from` 把希望捕捉的异常类放到一个变量(rescue_handlers)里。抛出异常时，会对异常进行检查(rescue_with_handler)，如果抛出的异常恰好被包含在这个变量(rescue_handlers)，则用我们的方式进行处理。

-----------

## Autoload
Autoload and eager load conveniences for your library.

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

Autoload and Thread-Safety

Many libraries and frameworks (including Ruby on Rails) use a feature of Ruby known as autoload, which allows components of a library to be lazily loaded only when the constant is resolved in the code path of an executing program. The problem with this feature is that, historically, the implementation has not been thread-safe. In other words, if two threads tried to resolve an autoloaded constant at the same time, weird things would happen. This problem was finally tackled in Ruby 1.9.1 but then regressed in 1.9.2 and re-resolved in 1.9.3 (but only in a later patchlevel), causing a bit of confusion around whether `autoload` is actually safe to use in a threaded Ruby program.

Thread-Safe in 2.0

For all intents and purposes, autoloading of modules should be considered thread-safe in Ruby 2.0.0p0, as the patch was officially merged into the 2.0 branch prior to release. Any thread-safety issues in Ruby 2.0 should be considered regressions, according to that ticket.

Enter Eager Loading

Of course, guaranteeing support for Ruby 2.0 is not entirely sufficient for most programs still running on 1.9.x, and in some cases, 1.8.x, so you may need to use a more backward-compatible strategy. In Ruby on Rails, this was solved with an `eager_autoload` method that forcibly loads all modules marked to be lazily loaded. If you are running threaded code, it is recommended that you call this prior to launching threads. Note that in Rails 4.0, the framework will eager load all modules by default, which should help you avoid having to think about these threading issues.


