## 其它

#### 为什么有的 helper 我不推荐？

以下是我的一些经验之谈，仅供参考。

- 基于其它 helper，并且功能上十分类似；
- 使用它，写出来的代码反而很丑陋；
- 和样式关联密切，或者CSS、JS也能做的；
- 不实用、或者说不通用

不要迷信 Rails 提供的 helper，有的用起来难读、难懂，还不如自己写。别忘了，设计软件的重点：好读、易维护、以全局观思考。

#### ~~Dependency Tracker~~

供 Digestor 使用，digest 的时候可以以数组的形式传 dependencies 作为其可选参数。

#### ~~几个比较有意思的 helper 方法~~

**atom_feed**

不同于其它信息，Atom 信息不能用 ERB 或其它模板语言创建。但通过这个方法，却能构建 Atom 订阅信息。

**cache**

缓存。

**content_for**

存储代码块，yield 调用。

**fields_for**

构建表单。区别于 form_for，与 record 对象直接关联不大。

**form_for**

构建表单。和 record 对象关联比较大。

**render**

渲染。

**content_tag**

用于生成一个闭合的 HTML 标签。很多 helper 方法，都是封装于它。

**tag**

用于生成一个打开的 HTML 标签。很多 helper 方法，都是封装于它。
