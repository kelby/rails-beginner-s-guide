## Autoload

**延迟加载(自动加载)和预加载(立即加载)。**

常用方法：

```
autoload
eager_autoload
```

```
eager_load!
```

#### 使用 require 加载

```ruby
require "some/library"
```

使用 `require`, 如果此前没有加载过，则加载并返回 true; 如果此前已经加载过，则返回 false.<br>
(区别于 `load`，load 每次都会加载。没有返回 true/false 的概念)

#### 使用 require 面临的问题

require 意味着"立即加载所有"，一开始这很美好，但随着项目的成长，启动速度就会变得很慢。因为我们并没有真正使用到"所有"文件，但却需要加载。

并且，require 不是线程安全的。

#### 使用 autoload 自动加载(延迟加载)

```ruby
module Foo
  autoload :Bar, "path/to/bar"
end
```

使用 `autoload`，相当于先声明要加载的模块(纪录在 @_autoloads)，真正需要的时候才会加载。

autoload 本质还是 require, 只是把"加载"细分为"声明 + 加载"。

#### 使用 autoload 面临的问题

多线程时，这又会引出一个新问题。就是在一个线程里需要此模块，此时会加载(返回 true)。在这之后，另一个线程也需要此模块，此时会加载，但是因为前一个线程已经加载过了，所以这里会失败(返回 false).

也就是说，autoload 不是线程安全的。(准确点，应该是 Ruby 2.0 之前的版本不是线程安全的)

#### 使用 eager_autoload

为每个线程声明一下要加载的模块(纪录在各自的 @_autoloads)，真正需要的时候才会加载。因为每个线程都有声明，所以一个线程是否加载，并不会影响另一进程。

并且，很好的解决了 Ruby 旧版本不运行 autoload 的困境。

#### 使用举例

```ruby
module MyLib
  extend ActiveSupport::Autoload

  autoload :Model

  eager_autoload do
    autoload :Cache
  end
end
```

```ruby
MyLib.eager_load!
```

#### 其它方法

除上述常用方法外，还有：

```
autoload_at
autoload_under

autoloads
```
