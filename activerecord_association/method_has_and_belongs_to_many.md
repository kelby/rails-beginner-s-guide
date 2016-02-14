## has_and_belongs_to_many

#### 方法本身表示什么意思。

指定多对多关系。用中间表来连接前者与后者。如果没有明确的指定中间表的名字，那么默认按前者、后者的字母顺序排序构成中间表。

#### 引进了哪些方法，表示什么意思。

...

#### 有什么参数，表示什么意思，使用后有什么效果。

普通参数 **Scope**

设置一个 scope，通过前者查询后者的时候(其它时候不影响)，自动加到查询语句里。(类似在后者 model 里定义了一个 default_scope，但只有通过前者查询才起作用)

举例:

```ruby
has_and_belongs_to_many :projects, -> { includes :milestones, :manager }
has_and_belongs_to_many :categories, ->(category) {
  where("default_category = ?", category.name)
}
```

普通参数 **Extension**

前面提到过，使用这几个关联方法时，"它们会引入一些额外的方法"，并且上文已经介绍了它们。但如果我们想要引入自己的方法，并且用途基本一样，要怎么做，可以用 Extension.

举例:

```ruby
has_and_belongs_to_many :contractors do
  def find_or_create_by_name(name)
    first_name, last_name = name.split(" ", 2)
    find_or_create_by(first_name: first_name, last_name: last_name)
  end
end
```

现在，可以用 `organization.people.find_active` 对后者进行操作。这样定义的方法，和 CollectionProxy 定义的方法类似。

> Note: 这些 Extension 方法，类似在后者 model 里定义的 scope, 但只能通过前者.后者这种方法调用！
