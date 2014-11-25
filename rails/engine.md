# Engine

Engine 下有 Railtie，上有 Application.  
在 Engine 里，可以直接使用 Railtie 提供的方法；在 Application 里，可以直接使用 Engine 提供的方法。

可以把一个 Rails 项目当做一个组件，插入到另一个项目里。

Engine 可以当做一个不完整的应用，它依赖于 main_app (main_app 表示我们的项目本身，在 Application::Finisher 里定义).

## 对外提供的接口

```
app
config
eager_load!, endpoint, env_config
find
helpers, helpers_paths
isolate_namespace
load_console, load_generators, load_runner, load_seed, load_tasks
railties, routes

load_config_initializer
```

```
isolated? & isolated
engine_name & railtie_name
```

## initializer

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

## 其它

除了 initializer 外，rake_tasks 也会被复制到 main_app 里。所以，有时候你会看到提示：  
"Copy migrations from #{railtie_name} to application"
