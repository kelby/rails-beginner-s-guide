## Nested Attributes

方法 `accepts_nested_attributes_for(*attr_names)`

attr_names 由：一个或多个属性(association_name) 和 一个或多个可选参数(option)组成。

只接受options：

```
:allow_destroy
:reject_if
:limit
:update_only
```

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

---

update_only 选项的使用

[accepts_nested_attributes_for is Creating New Records; Gotcha!](http://robots.thoughtbot.com/accepts-nested-attributes-for-with-has-many-through)

invert_of 的另一个作用
[accepts_nested_attributes_for with Has-Many-Through Relations](http://robots.thoughtbot.com/accepts-nested-attributes-for-with-has-many-through)

allow_destroy 选项的使用
[【Rails】fields_for と accepts_nested_attributes_for](http://kzy52.com/entry/2013/07/10/200144)
和此方法配套使用的是 `fields_for` 方法。

[Rails nested form with has_many :through, how to edit attributes of join model?](http://stackoverflow.com/questions/2182428/rails-nested-form-with-has-many-through-how-to-edit-attributes-of-join-model)

[Complex Rails Forms with Nested Attributes](http://www.sitepoint.com/complex-rails-forms-with-nested-attributes/)
