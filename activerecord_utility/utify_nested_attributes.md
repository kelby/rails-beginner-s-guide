## Nested Attributes 嵌套属性

提供类方法 `accepts_nested_attributes_for(*attr_names)`

attr_names 由：一个或多个属性(association_name) 和 一个或多个可选参数(option)组成。

只接受 options：

```
:allow_destroy
:reject_if
:limit
:update_only
```

`:limit` 在单个 record 构建时不执行；指量构建时，在校验之前执行，并且超过限制的话直接抛错误。实际项目中，不推荐使用。

**当你声明嵌套属性时，Rails 会自动帮你定义属性的写方法。**

```ruby
# 摘录部分代码
def #{association_name}_attributes=(attributes)
  assign_nested_attributes_for_#{type}_association(:#{association_name}, attributes)
end
```

`association_name` 就是你声明的属性，例如：

```ruby
class Book < ActiveRecord::Base
  has_one :author
  has_many :pages

  accepts_nested_attributes_for :author, :pages
end
```

生成 `author_attributes=(attributes)` 和 `pages_attributes=(attributes)`

**对于关联对象，会自动设置 `:autosave`**

```ruby
# 摘录部分代码
reflection.autosave = true                     # 自动保存
add_autosave_association_callbacks(reflection) # 回调在自动保存时仍然有效
```

**对于嵌套的属性，默认你可以执行写操作，但不能删除它们。**

如果你真的要这么做，也可以通过 `:allow_destroy` 来设置。如：

```ruby
class Member < ActiveRecord::Base
  has_one :avatar
  accepts_nested_attributes_for :avatar, allow_destroy: true
end

# 然后

member.avatar_attributes = { id: '2', _destroy: '1' }
member.avatar.marked_for_destruction? # => true
member.save
member.reload.avatar # => nil
```

上面举例是一对一，下面的一对多关系类似：

```ruby
params = { member: {
  name: 'joe', posts_attributes: [
    { title: 'Kari, the awesome Ruby documentation browser!' },
    { title: 'The egalitarian assumption of the modern citizen' },
    { title: '', _destroy: '1' } # this will be ignored
  ]
}}

member = Member.create(params[:member])
member.posts.length # => 2
member.posts.first.title # => 'Kari, the awesome Ruby documentation browser!'
member.posts.second.title # => 'The egalitarian assumption of the modern citizen'
```

**自动保存多个嵌套属性，有的可能不符合校验。**

为了处理这种情况。你可以设置 `:reject_if`:

```ruby
class Member < ActiveRecord::Base
  has_many :posts

  accepts_nested_attributes_for :posts, reject_if: proc do |attributes|
    attributes['title'].blank?
  end
end

params = { member: {
  name: 'joe', posts_attributes: [
    { title: 'Kari, the awesome Ruby documentation browser!' },
    { title: 'The egalitarian assumption of the modern citizen' },
    { title: '' } # this will be ignored because of the :reject_if proc
  ]
}}

member = Member.create(params[:member])
member.posts.length # => 2
member.posts.first.title # => 'Kari, the awesome Ruby documentation browser!'
member.posts.second.title # => 'The egalitarian assumption of the modern citizen'
```

在这里，效果和上面使用 `_destroy: '1'` 有类似之处。

**重现 autosave 创建过程**

```ruby
Book has_many :pages

reflection = Book._reflect_on_association(:pages)
Book.send(:add_autosave_association_callbacks, reflection)

Book.reflect_on_all_autosave_associations
```

**update_only - 解决更新关联对象时的困扰**

之前的写法：

```ruby
# alias.rb
class Alias < ActiveRecord::Base  
  belongs_to :user
  accepts_nested_attributes_for :user
end  
```

举例：更新关联对象

```ruby
# 传递 id
Alias.first.user.name  
>> "Alice"
Alias.first.update_attributes(  
{
  :user_attributes => {
    :id => 1, # <- 这行
    :name => "Bob"
  }
})

Alias.first.user.name  
>> "Bob"

# 外键用的是传递过来的 id
Alias.first.user_id  
>> 1

# 不传递 id
Alias.first.update_attributes(  
{
  :user_attributes => {
    :name => "Bob"
  }
})

# 没有传递外键，则会先删除，然后重新创建关联对象
Alias.first.user_id  
>> 2
```

上述情况，虽然文档上已经写明了。但这违背了我们的直觉，建议加上 `:update_only` 参数。

之后的写法：

```ruby
# alias.rb
class Alias < ActiveRecord::Base  
  belongs_to :user
  accepts_nested_attributes_for :user, update_only: true
end  
```

update_only 仅作用于单一关系，对 collection 使用无效。

当然，除上述方法外，还有解决办法就是：
直接获取，然后操作被关联的对象作。或者，直接通过关联对象进行赋值，然后保存（利用 auto_save 进行自动更新）。

---

invert_of 的另一个作用
[accepts_nested_attributes_for with Has-Many-Through Relations](http://robots.thoughtbot.com/accepts-nested-attributes-for-with-has-many-through)

allow_destroy 选项的使用
[【Rails】fields_for と accepts_nested_attributes_for](http://kzy52.com/entry/2013/07/10/200144)
和此方法配套使用的是 `fields_for` 方法。
