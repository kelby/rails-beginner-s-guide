## Initializable

指的是 Rails::Initializable.

本模块被 Railtie 所引用，又由于继承关系，Engine、Application、AppName 都可用里面的方法。
<br>
如：
自定义的 Railtie 及其子类里常用到的 `initializer` 方法，就是它定义的。

**类方法：**

```
initializer

initializers_for

initializers
initializers_chain
```

`initializer` 负责创建 initializer，并加入 initializers.

可用 `initializers_for` 获取应用里某类 initializer 的名字：

```ruby
Rails.app_class.initializers_for("web_console").map &:name
```

**实例方法：**

```
initializers

run_initializers
```

可用 `initializers` 获取应用里所有的 initializer 的名字：

```ruby
Rails.application.initializers.map &:name
```
