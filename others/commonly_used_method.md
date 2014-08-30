# Commonly used method

## ActiveSupport

`eager_autoload`<br>
`autoload`

`class_attribute`

```
Declare a class-level attribute whose value is inheritable by subclasses.
Subclasses can change their own value and it will not impact parent class.
```

`attr_internal`

```
alias_method :attr_internal, :attr_internal_accessor

Declares an attribute reader and writer backed by an internally-named instance variable.
```

`mattr_accessor`

```
alias :cattr_accessor :mattr_accessor

Defines both class and instance accessors for class attributes.
```

`delegate`

```
Provides a +delegate+ class method to easily expose contained objects' public methods as your own.
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
Allows you to add shortcut so that you don't have to refer to attribute through config.
```

`define_callbacks`

```
Define sets of events in the object life cycle that support callbacks.
```

`run_callbacks`

```
Runs the callbacks for the given event.
```

`attr_internal_writer`

```
Declares an attribute writer backed by an internally-named instance variable.
```

`info` `debug` `warn` `error` `fatal` `unknown`


`included`

```
指的是 ActiveSupport::Concern 所提供的实例方法。
```

`extract_options!`

```
Extracts options from a set of arguments. Removes and returns the last element in the array if it's a hash, otherwise returns a blank hash.
```

## AbstractController

`abstract!`

```
Define a controller as abstract.

ActionController::Metal and ActionController::Base are defined as abstract
```

我们自定义的 Controller 里的 public instance methods(公开实例方法) 都会被当做 action 来对待。因此，继承的时候要做一些处理，
以避免父类的实例方法被当做 action. 目前，解决方法是把父类声明为：abstract = true

`helper`

```
The helper class method can take a series of helper module names, a block, or both.
```

`helper_method`

```
Declare a controller method as a helper.
```

## Railtie

delegate :config, to: :instance

## Initializable

initializer

