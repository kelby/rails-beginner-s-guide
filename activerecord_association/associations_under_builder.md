## builder 目录下的 associations

### Association

```
extensions
extensions=

valid_options
valid_options=

build
create_builder
define_callbacks
define_accessors
define_readers
define_writers
define_validations
valid_dependent_options

check_dependent_options
add_destroy_callbacks
```

```
name
scope
options

build
macro
valid_options
validate_options
define_extensions
```

### Singular Association

```
define_accessors
define_constructors
define_validations
```

```
valid_options
```

### Collection Association

```
define_callbacks
define_callback

define_readers
define_writers
```

```
valid_options
block_extension
define_extensions
```

### Has One

```
valid_dependent_options
add_destroy_callbacks
```

```
macro
valid_options
```

### Belongs To

```
valid_dependent_options
define_callbacks
define_accessors

add_counter_cache_methods
add_counter_cache_callbacks
touch_record
add_touch_callbacks
add_destroy_callbacks
```

```
macro
valid_options
```

### Has Many

```
valid_dependent_options
```

```
macro
valid_options
```

### Has And Belongs To Many

```
lhs_model
association_name
options

through_model
middle_reflection
```
