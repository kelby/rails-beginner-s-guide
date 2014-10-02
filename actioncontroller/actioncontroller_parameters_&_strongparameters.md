# StrongParameters & Parameters

## StrongParameters

提供 `params` 这个对象，我们可以对它的属性进行读、写操作。

```
params, params=
```

另外，要说明：

```ruby
params == request.parameters
=> true
```

这个对象的值是什么？- 表单数据或传递过来的，加上 Controller 和 action

```ruby
Processing by PostsController#create as HTML
  Parameters: {"utf8"=>"✓", "authenticity_token"=>"kJttlgy9ptyuFS5TXrE95HFwKdhf7p74yuFZl73Lvxg=", "post"=>{"title"=>"hello world"}, "commit"=>"Create Post"}

params
  => {"utf8"=>"✓",
   "authenticity_token"=>"kJttlgy9ptyuFS5TXrE95HFwKdhf7p74yuFZl73Lvxg=",
   "post"=>{"title"=>"hello world"},
   "commit"=>"Create Post",
   "action"=>"create",
   "controller"=>"posts"}
```

## Parameters

提供 `params` 这个对象可以使用的方法：

```
[]
converted_arrays
delete, dup
each & each_pair
fetch
permit
require & required
slice
to_h, transform_values

extract!
permit!
select!

permitted?

permitted=
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

```
config.always_permitted_parameters = %w( controller action format )
```
