# ActiveModel

我们知道 MVC 结构里，Model(模型)层与数据库关系最紧密。现在我们是把 Model(模型层)再拆分一下，把对数据库真正有操作的这部分拆分成 ActiveRecord，把没有对数据库操作的这部分拆分成 ActiveModel。但它们都属于 Model(模型层)。

做了这样的折分之后，如果我们觉得 ActiveRecord 不合味口(使用 NoSQL 等)，但又不想完全抛弃它。解决方案来了，那就是 ActionModel，ActiveModel 以接口的形式提供一系列方法给 model 类使用。

ActiveRecord 对于需要持久化的数据进行校验或处理，是很强大的。但如果我们不需要持久化数据，或者我们只需要很少的功能，如：提交表单数据在 Web 开发中，比较常见(例如填表单，给我们发送反馈意见) - 用户提交信息，无论校验是有效还是无效，都应该得到反馈。ActiveRecord 太重了，使用 ActiveModel 可以帮助我们减少复杂度。

> Note: 说明一下，ActiveModel 脱胎于 ActiveRecord，而不是依赖，它可以单独使用；反过来，ActiveRecord 依赖于 ActiveModel。

