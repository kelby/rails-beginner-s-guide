## Engine

Engine 下有 Railtie，上有 Application.  
在 Engine 里，可以直接使用 Railtie 提供的方法；在 Application 里，可以直接使用 Engine 提供的方法。

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

可以把一个 Rails 项目当做一个组件，插入到另一个项目里。

可以把 Engine 当做一个不完整的应用，它依赖于 main_app (main_app 表示我们的项目本身，在 Application::Finisher 里定义)。

**Engine = Ruby Gem + Rails MVC stack elements.**

和一般 gem 对比，Engine 至少有以下特点：

- 遵循 Rails 的目录结构
- 从 Railtie 继承来的那一套方法
- 与 Rails 无缝集成

前两章节主要从源代码角度讲解，其余章节主要从使用角度讲解。

参考

[Rails Engine Patterns](http://www.slideshare.net/AndyMaleh/rails-engine-patterns)
