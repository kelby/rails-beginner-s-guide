# Active Model 功能模块

我们知道 MVC 结构里，Model\(模型\)层与数据库关系最紧密。现在我们是把 Model\(模型层\)再拆分一下，把对数据库真正有操作的这部分拆分成 Active Record，把没有对数据库操作的这部分拆分成 Active Model.

做了这样的折分之后，如果我们觉得 Active Record 不合口味\(例如：想使用 NoSQL\)，但又不想完全抛弃它。解决方案来了，使用 Action Model，Active Model 可以以接口的形式提供一系列方法给 model 使用。

Active Record 对于需要持久化的数据进行校验或处理，是很强大的。但如果我们不需要持久化数据，或者我们只需要很少的功能，如：提交表单数据在 Web 开发中是比较常见\(联系我们，给我们发送反馈意见\) - 用户提交信息，无论校验是有效还是无效，都应该得到反馈。Active Record 太重了，使用 Active Model 可以帮助我们减少复杂度。

> Note:   
> 说明一下，Active Model 脱胎于 Active Record，它可以单独使用；  
> 反过来，Active Record 依赖于 Active Model，不可以单独使用。



