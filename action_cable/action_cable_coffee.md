## Coffee 脚本

我们用脚本生成 Channel 时，一般地都有对应的 Coffee 脚本。

Consumer 开动机器，Subscription 接受命令，Connection 干活。

Rails 默认使用 `App.cable` 表示其 Consumer 实例。

**4 种调用**

- 约定调用的 coffee 方法
- 外部直接调用 coffee 方法
- 内部调用自己的方法
- 调用 Channel 里的 action

