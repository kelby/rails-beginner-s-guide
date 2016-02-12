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
