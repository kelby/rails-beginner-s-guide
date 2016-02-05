## builder 目录下的 associations 文件

关联方法带来的基本方法。

**主要做 5 件事：**

```ruby
define_extensions model, name, &block
create_reflection model, name, scope, options, extension
define_accessors model, reflection
define_callbacks model, reflection
define_validations model, reflection
```

**对内实现。**

```
  HasOne   &   BelongsTo        HasMany   &   HasAndBelongsToMany
           |                              |
           V                              V
    SingularAssociation      &     CollectionAssociation
                             |
                             V   
                         Association
```

#### 1) Association

类方法：

```ruby
class << self
  attr_accessor :extensions
  attr_accessor :valid_options
end

build

create_builder

define_callbacks
define_accessors
define_readers
define_writers
define_validations

valid_dependent_options
```

实例方法：

```
attr_reader :name, :scope, :options

build

macro

valid_options

validate_options

define_extensions
```

其它类方法：

```
check_dependent_options

add_destroy_callbacks
```

#### 2) Singular Association

类方法：

```
define_accessors
define_constructors
define_validations
```

实例方法：

```
valid_options
```

#### 3) Collection Association

类方法：

```
define_callbacks
define_callback

define_readers
define_writers
```

实例方法：

```
valid_options

attr_reader :block_extension

define_extensions
```

其它实例方法：

```
wrap_scope
```

#### 4) Has One

类方法：

```
valid_dependent_options
```

其它类方法：

```
add_destroy_callbacks
```

实例方法：

```
macro
valid_options
```

#### 5) Belongs To

类方法：

```
valid_dependent_options

define_callbacks
define_accessors
```

实例方法：

```
macro
valid_options
```

其它类方法：

```
add_counter_cache_methods
add_counter_cache_callbacks

touch_record

add_touch_callbacks
add_destroy_callbacks
```

#### 6) Has Many

类方法：

```
valid_dependent_options
```

实例方法：

```
macro
valid_options
```

#### 7) Has And Belongs To Many

实例方法：

```
attr_reader :lhs_model, :association_name, :options

through_model
middle_reflection
```

其它实例方法：

```
middle_options

belongs_to_options
```

#### 主要内容

- 创建 reflection

- 读/写方法，如：build_x, create_x, create_x!；x_ids, x_ids=；x, x=  

- 参数相关处理：
  - 参数是否合法  
  - 参数的参数(如：dependence)是否合法
  - 回调（删除，增减 counter_cache，touch)  
  - 中间表(join_table, class_name, source 及多对多关系的自动生成...等)  
  - 检验(如：required)  
  - 定义 extension
  - autosave 相关实现

> Note: 对关联表的处理时，大量使用了 reflection 里面的实例方法。
