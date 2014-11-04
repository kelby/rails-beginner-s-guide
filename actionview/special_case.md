## 几个比较有意思的 helper

**atom_feed**

不同于其它信息，Atom 信息不能用 ERB 或其它模板语言创建。但通过这个方法，却能构建 Atom 订阅信息。

**cache**

缓存。

**content_for**

存储代码块，yield 调用。

**fields_for**

构建表单。区别于 form_for，与 model 对象直接关联不大。

**form_for**

构建表单。和 model 对象关联比较大。

**render**

渲染。

**content_tag**

用于生成一个闭合的 HTML 标签。很多 helper 方法，都是封装于它。

**tag**

用于生成一个打开的 HTML 标签。很多 helper 方法，都是封装于它。
