## Rails 源代码里一些常用方法

### Active Support

`eager_autoload` 和 `autoload`

`class_attribute`

```
定义一个类属性，子类继承于父类。
子类可以更改自己的属性，但不影响到父类的。
```

`attr_internal`

```
alias_method :attr_internal, :attr_internal_accessor

声明一个读、写属性，功能类似 attr_accessor, 但内部实现有一点点不同。
```

`mattr_accessor`

```
alias :cattr_accessor :mattr_accessor

定义一个类属性，同时具备类和实例级别的读、写。(这里类似全局变量了)
```

`delegate`

```
委托，将它人的方法做为已用。
```

后面是个对象即可，而 Ruby 又号称"一切皆对象"。

最终通过 module_eval 并重新定义了方法。

委托类方法给实例对象使用。

```ruby
class Foo
  def self.hello
    "world"
  end

  delegate :hello, to: :class
end

Foo.new.hello # => "world"
```

`config_accessor`

```
定义一个属性，同时具备类和实例级别的读、写。
实例可以更改自己的属性，但不影响到类的。
```

`define_callbacks`

```
定义回调。
```

`run_callbacks`

```
运行回调。
```

`attr_internal_writer`

```
功能类似 attr_writer，但内部实现有一点点不同。
```

`info` `debug` `warn` `error` `fatal` `unknown`
也就是 Rails.logger 的各个级别(或者说类别)。

`included`

```
指的是 ActiveSupport::Concern 所提供的实例方法。
当此模块被 include 时，执行什么代码。
```

`extract_options!`

```
把可选参数里的 Hash 部分，萃取出来。
```

### Abstract Controller

`abstract!`

```
声明此 Controller 是抽象的。

ActionController::Metal 和 ActionController::Base 都声明为抽象的。(作用参考下面解释)
```

我们自定义的 Controller 里的 public instance methods(公开实例方法) 都会被当做 action 来对待。
因此，继承的时候要做一些处理，
以避免父类的实例方法被当做 action. 目前，解决方法是把父类声明为：abstract = true

`helper`

```
引入外部模块(专指 helper 模块)，功能类似 include.
```

`helper_method`

```
将 Controller 里的方法转化为 helper 方法。
```

### Railtie

delegate `:config`, to: :instance

### Initializable

`initializer`
