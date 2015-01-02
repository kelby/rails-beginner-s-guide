## 与表单不直接相关的辅助方法

Rails 提供了很多的 helper 供我们使用，处理 asset、date、form、number 和 model 对象等。默认它们可以在所有模板上使用。

基于 Tag 实现的 helper, 部分可以到 action_view/helpers/tags 目录下找到相应文件，里面的 render 方法里面有常用的可选参数。

对设置默认值等，特别有帮助。不用写 html，也不用额外添加变量。

当和 record 对象没有关联时，尽量使用 x_tag 的形式。
