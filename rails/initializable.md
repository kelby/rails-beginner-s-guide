## Initializable

指的是 Rails::Initializable.

Railtie 及其子类里常用到的 `initializer` 方法，就是它定义的。

被 Railtie 所调用，又由于继承关系，Engine、Application、AppName 都可用。

**类方法：**

```
initializer
initializers_for

initializers
initializers_chain
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

可用 `initializers_for` 获取应用里某类 initializer 的名字：

```ruby
Rails.application.class.initializers_for("web_console").map &:name
```
