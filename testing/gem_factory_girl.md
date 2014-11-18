## factory_girl

### Syntax Methods

构建单个对象

```
build
create
attributes_for
build_stubbed
```

构建多个对象

```
build_list
create_list
attributes_for_list
build_stubbed_list
```

链接 [FactoryGirl Syntax Methods](http://www.rubydoc.info/github/thoughtbot/factory_girl/FactoryGirl/Syntax/Methods)

### Getting Started

**Linting Factories**

类方法

```
lint
```

**Defining factories**

类方法

```
define
```

实例方法

```
factory
```

**Lazy Attributes**

大括号 `{}` 的使用

**Aliases**

可选参数 `:aliases` 的使用

**Dependent Attributes**

`{}` 大括号里面再求值

**Transient Attributes**

```ruby
transient
```

**Associations**

```
association
```

以及它的可选参数 `:factory` 和 `:strategy`

**Inheritance**

嵌套使用 `factory`

以及 factory 的可选参数 `:parent`

**Sequences**

```
sequence

generate
```

**Traits**

```
trait
```

及 factory 的可选参数 `:traits`

**Callbacks**

```
after(:build)
before(:create)
after(:create)
after(:stub)
```

**Modifying factories**

类方法

```
modify
```

**Building or Creating Multiple Records**

```
build_list
create_list

build_pair
create_pair
```

**Custom Construction**

```
initialize_with

attributes
```

需要自定义相关类及方法。

**Custom Strategies**

```
register_strategy
```

需要自定义相关类及方法。

**Custom Callbacks**

```
callback
```

使用"Custom Strategies"会自动添加上述默认的 Callbacks.

**Custom Methods to Persist Objects**

```
to_create

skip_create
```

链接 [Getting Started](http://www.rubydoc.info/gems/factory_girl/file/GETTING_STARTED.md)
