## 4 个关联方法的使用

**对外提供接口。**

Rails 提供了 4 个类方法，用于声明对象与对象之间的关系。它们的意思，理解起来很简单，和字面意思一样，如"Project has one Project Manager" 或 "Project belongs to a Portfolio".   但实际使用过程，还是有很多要注意的。不同的场景，需要不同的参数；并且，它们会引入一些额外的方法，有的和 attr_* 类似，但有的不是。

```ruby
class Project < ActiveRecord::Base
  belongs_to              :portfolio
  has_one                 :project_manager
  has_many                :milestones
  has_and_belongs_to_many :categories
end
```

上面例子里的场景最简单，不需要带任何参数；下面是它们引入的额外的方法，包括但不限于：

```ruby
# 1
Project#portfolio
Project#portfolio=(portfolio)
Project#portfolio.nil?

# 2
Project#project_manager
Project#project_manager=(project_manager)
Project#project_manager.nil?,

# 3
Project#milestones.empty?
Project#milestones.size
Project#milestones
Project#milestones<<(milestone)
Project#milestones.delete(milestone)
Project#milestones.destroy(milestone)
Project#milestones.find(milestone_id)
Project#milestones.build
Project#milestones.create

# 4
Project#categories.empty?
Project#categories.size
Project#categories
Project#categories<<(category1)
Project#categories.delete(category1)
Project#categories.destroy(category1)
```

#### 其它

注意：可选参数 :dependent 在 has_one 和 belongs_to 里，删除关联用的是 delete；而在 has_many 里，删除关联用的是 delete_all.
这两者要做的事性质上是一样的，但请注意这点区别，并且它们都没有 destroy_all.

上述对方法或参数的解释，仅供参考，实际情况以运行结果为准。

当不确定参数表示什么意思，使用后有什么效果时，请慎用，可以选择其它确定/有把握的方法代替。
