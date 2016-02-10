## Engine

Engine 下有 Railtie，上有 Application.

```
Your Application
     |
     V
 Application
     |
     V
   Engine
     |
     V
  Railtie
```

在 Engine 里，可以直接使用 Railtie 提供的方法；
<br>
在 Application 里，可以直接使用 Engine 提供的方法。

**Engine = Ruby Gem + Rails MVC stack elements.**

使用 Engine，可以把一个小型的 Rails 项目当成组件，插入到另一个 Rails 项目里。

它可以有自己的 MVC、路由、Helper、Assets、Rake、Generator 与配置，甚至是 lib、Migrate 和测试。

**和一般 gem 对比，Engine 至少有以下特点：**

- 按照约定，遵循 Rails 应用的文件、目录结构
- 从 Railtie 继承来的那一套方法，可用于配置、初始化
- 与 Rails 无缝集成

参考

[Rails Engine Patterns](http://www.slideshare.net/AndyMaleh/rails-engine-patterns)
