# Commonly used method

## ActiveSupport

`eager_autoload`<br>
`autoload`

`class_attribute`

```
alias :cattr_accessor :mattr_accessor

Declare a class-level attribute whose value is inheritable by subclasses.
Subclasses can change their own value and it will not impact parent class.
```

`attr_internal`

```
alias_method :attr_internal, :attr_internal_accessor

Declares an attribute reader and writer backed by an internally-named instance variable.
```

`cattr_accessor`

```
Allows you to add shortcut so that you don't have to refer to attribute
through config. Also look at the example for config to contrast.
```

`mattr_accessor`

```
alias :cattr_accessor :mattr_accessor

Defines both class and instance accessors for class attributes.
```

`delegate`

```
Provides a +delegate+ class method to easily expose contained objects'
public methods as your own.
```

`config_accessor`

```
Allows you to add shortcut so that you don't have to refer to attribute
through config.
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

## AbstractController

`abstract!`

```
Define a controller as abstract.
```

`helper`

```
The +helper+ class method can take a series of helper module names, a block, or both.
```

`helper_method`

```
Declare a controller method as a helper.
```


## Railtie

delegate :config, to: :instance

## Initializable

initializer

