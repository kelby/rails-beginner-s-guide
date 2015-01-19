# Factory Girl

### 可用于构建 record 对象的方法

**构建单个对象：**

```
build
create
attributes_for
build_stubbed
```

使用举例：

```
build(:completed_order)

create(:post) do |post|
  create(:comment, post: post)
end

attributes_for(:post, title: "I love Ruby!")

build_stubbed(:user, :admin, :male, name: "John Doe")
```

**构建多个对象：**

```
build_list
create_list
attributes_for_list
build_stubbed_list
```

使用举例：

```
build_list(:completed_order, 2)
create_list(:completed_order, 2)

attributes_for_list(:post, 4, title: "I love Ruby!")

build_stubbed_list(:user, 15, :admin, :male, name: "John Doe")
```

链接 [FactoryGirl Syntax Methods](http://www.rubydoc.info/github/thoughtbot/factory_girl/FactoryGirl/Syntax/Methods)

### Getting Started

**Linting Factories**

类方法：

```
lint
```

**Defining factories**

类方法：

```
define
```

实例方法：

```
factory
```

(定义 factory.)

**Lazy Attributes**

定义属性时，大括号 `{}` 的使用。

**Aliases**

定义 factory 时，可选参数 `:aliases` 的使用。

(给 factory 起外号)

**Dependent Attributes**

`{}` 大括号里面再求值。

**Transient Attributes**

```ruby
transient
```

(在定义 factory 的代码内同时定义方法，之后使用到)

**Associations**

```
association
```

以及它的可选参数 `:factory` 和 `:strategy`

(关联对象的 factory.)

**Inheritance**

嵌套使用 `factory` 方法。

或 factory 的可选参数 `:parent`

(继承已有的 factory.)

**Sequences**

```
sequence

generate
```

(按顺序生成 factory 的属性内容)

**Traits**

定义：

```
trait
```

调用：factory 的可选参数 `:traits` 或直接调用。

(避免重复代码)

**Callbacks**

默认已经有：

```
after(:build)
before(:create)
after(:create)
after(:stub)
```

(创建 factory 时的回调)

自定义：

```
callback
```

**Modifying factories**

类方法：

```
modify
```

(更改已有 factory. 场景：在 rails c 里检验)

**Building or Creating Multiple Records**

```
build_list
create_list

build_pair
create_pair
```

(批量创建 factory 对象)

**Custom Construction**

```
initialize_with

attributes
```

需要自定义相关类及方法。

(Model 里使用了 initialize 并设置了实例变量)

**Custom Strategies**

类方法：

```
register_strategy
```

需要自定义相关类及方法。

(自定义 factory 对象的生成规则)

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

(重新定义并调用"保存方法"；创建 factory 对象的时候跳过"保存")

**重新加载 factory**

类方法：

```
find_definitions
```

链接 [Getting Started](http://www.rubydoc.info/gems/factory_girl/file/GETTING_STARTED.md)
