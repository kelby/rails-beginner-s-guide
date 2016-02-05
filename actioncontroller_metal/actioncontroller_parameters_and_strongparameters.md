由两部分组成：Parameters 和 Strong Parameters.

## Parameters

Parameters 继承于 Hash With Indifferent Access，而 Hash With Indifferent Access 又继承于 Hash. 所以它的实例对象类似于 Hash 的实例对象。

提供 `params` 这个对象可以使用的方法(部分与 Hash 实例方法类似)：

```
permit

require & required

extract!

permit!

select!

permitted?

permitted=

[]

delete, dup

each & each_pair

fetch

to_h

slice

transform_values

converted_arrays
```

我们可以在 Rails 之外创建自己的 params 对象：

```ruby
require 'action_controller/parameters'

params = ActionController::Parameters.new({
  person: {
    name: 'Francesco',
    age:  22,
    role: 'admin'
  }
})

# require(key) 和 permit(*filters) 方法
permitted = params.require(:person).permit(:name, :age)
permitted            # => {"name"=>"Francesco", "age"=>22}
permitted.class      # => ActionController::Parameters
# permitted? 方法
permitted.permitted? # => true

Person.first.update!(permitted)
# => #<Person id: 1, name: "Francesco", age: 22, role: "user">
```

配置默认的 permitted parameters

```ruby
config.always_permitted_parameters = %w( controller action format )
```

## Strong Parameters

我们在 Controller 里常用的 `params` 就是这里提供的。

`params` 实际上是 Parameters 实例对象，我们可以对它的属性进行读、写操作。

```
params

params=
```

另外，要说明：

```ruby
params == request.parameters
# => true
```

并不表明它和 `request.parameters` 是完全等价的，后者是 ActiveSupport::HashWithIndifferentAccess 实例对象。

这个对象的值是什么？- 表单数据或传递过来的，加上 :controller 和 :action

```ruby
Processing by PostsController#create as HTML
Parameters: {"utf8"=>"✓",
             "authenticity_token"=>"kJttlgy9ptyuFS5TXrE95HFwKdhf7p74yuFZl73Lvxg=",
             "post"=>{"title"=>"hello world"},
             "commit"=>"Create Post"}

params
=> {"utf8"=>"✓",
    "authenticity_token"=>"kJttlgy9ptyuFS5TXrE95HFwKdhf7p74yuFZl73Lvxg=",
    "post"=>{"title"=>"hello world"},
    "commit"=>"Create Post",
    "action"=>"create",
    "controller"=>"posts"}
```

**attributes.permitted? 与 ForbiddenAttributesProtection**

因 Base 与 API 都有 `include StrongParameters` 并且仅提供 `params` 和 `params=` 方法，所以有理由相信在 Controller 和 View 里通过 params 给 record 对象属性赋值(AttributeAssignment)的话，都会询问一遍是否 `permitted?` 如果包含未被允许更新的字段，会抛 ForbiddenAttributesError 错误。
