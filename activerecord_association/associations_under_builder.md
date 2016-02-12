## Builder - 功能的核心

实际指的是 Builder Associations，关联方法带来的基本方法。

#### 主要做 5 件事

```ruby
define_extensions model, name, &block
create_reflection model, name, scope, options, extension
define_accessors model, reflection
define_callbacks model, reflection
define_validations model, reflection
```

#### 关系图

```
  HasOne   &   BelongsTo        HasMany   &   HasAndBelongsToMany
           |                              |
           V                              V
    SingularAssociation      &     CollectionAssociation
                             |
                             V   
                         Association
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
