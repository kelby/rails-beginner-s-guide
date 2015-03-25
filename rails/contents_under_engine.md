## Engine 文件下的内容

### 对外提供的接口

实例方法：

```
app

config

eager_load!

endpoint

env_config

helpers
helpers_paths

load_console
load_generators
load_runner
load_seed
load_tasks

railties

routes
```

类方法：

```
endpoint

find
find_root

isolate_namespace
```

```
# 并且

isolated? & isolated

engine_name & railtie_name
```

其它方法：

```
delegate :middleware, :root, :paths, to: :config
delegate :engine_name, :isolated?, to: :class
```

### 有哪些 initializer

设置 load path  
设置 autoload paths  
添加 routing paths  
添加 locales  
添加 view paths  
加载 environment config  
Append assets path  
Prepend helpers path  
加载 config initializers  
~~Engines blank point~~
