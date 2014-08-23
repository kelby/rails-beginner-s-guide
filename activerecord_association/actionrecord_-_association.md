```ruby
class Project < ActiveRecord::Base
  belongs_to              :portfolio
  has_one                 :project_manager
  has_many                :milestones
  has_and_belongs_to_many :categories
end
```

**Singular associations (one-to-one)**

|generated methods                 | belongs_to |  belongs_to :polymorphic | has_one|
|----------------------------------|------------|--------------|---------|
|other                             |     X      |      X       |    X|
|other=(other)                     |     X      |      X       |    X|
|build_other(attributes={})        |     X      |              |    X|
|create_other(attributes={})       |     X      |              |    X|
|create_other!(attributes={})      |     X      |              |    X|


**Collection associations (one-to-many / many-to-many)**

|generated methods                 | habtm | has_many | has_many :through|
|----------------------------------|-------|----------|----------|
|others                            |   X   |    X     |    X|
|others=(other,other,...)          |   X   |    X     |    X|
|other_ids                         |   X   |    X     |    X|
|other_ids=(id,id,...)             |   X   |    X     |    X|
|others<<                          |   X   |    X     |    X|
|others.push                       |   X   |    X     |    X|
|others.concat                     |   X   |    X     |    X|
|others.build(attributes={})       |   X   |    X     |    X|
|others.create(attributes={})      |   X   |    X     |    X|
|others.create!(attributes={})     |   X   |    X     |    X|
|others.size                       |   X   |    X     |    X|
|others.length                     |   X   |    X     |    X|
|others.count                      |   X   |    X     |    X|
|others.sum(*args)                 |   X   |    X     |    X|
|others.empty?                     |   X   |    X     |    X|
|others.clear                      |   X   |    X     |    X|
|others.delete(other,other,...)    |   X   |    X     |    X|
|others.delete_all                 |   X   |    X     |    X|
|others.destroy(other,other,...)   |   X   |    X     |    X|
|others.destroy_all                |   X   |    X     |    X|
|others.find(*args)                |   X   |    X     |    X|
|others.exists?                    |   X   |    X     |    X|
|others.distinct                   |   X   |    X     |    X|
|others.uniq                       |   X   |    X     |    X|
|others.reset                      |   X   |    X     |    X|

`belongs_to(name, scope = nil, options = {})`
---------

b belongs_to :a

Options

```
:class_name 指定 a 所对应的类名，默认是 A
:foreign_key 记录 a 的 id，默认是 a_id (不受 :class_name 影响)

:foreign_type 多态时用到，polymorphic: true，此时 a 不再是简单的关系名,默认是 a_type (不受 :class_name 影响)

:primary_key 默认是 id
:dependent 设置 :destroy 时，删除 b 会同时删除 a (！反人类)
:counter_cache 记录 b 的个数，默认是 bs_counter (受 b 的 table_name 影响)
:polymorphic 多态时用到
:validate 保存 a 时，校验 b (默认为 false)
:autosave 保存 a 时，自动保存 b (accepts_nested_attributes_for 时默认为 true)
:touch 保存 b 时，会更新 a 的 updated_at 属性
:inverse_of 中间表自动获取两个外键比较困难，有时候你需要告诉它
```

`has_and_belongs_to_many(name, scope = nil, options = {}, &extension)`
---------

a has_and_belongs_to_many :bs

```
:class_name 指定 b 所对应的类名，默认是 B

:join_table 指定 a 与 b 的关联表表名，默认是 as_bs
:foreign_key 记录中间表里 a 的 id，默认是 a_id
:association_foreign_key 记录中间表时 b 的 id, 默认是 b_id
:readonly 通过 b 获取的 a 只允许读操作
:validate 通过 b 保存的 a 校验与否？默认为 true
:autosave 保存 b 时会自动保存 a (accepts_nested_attributes_for 时默认为 true)
```

`has_many(name, scope = nil, options = {}, &extension)`
---------

a has_many :b

```
:class_name 指定 b 所对应的类名，默认是 B
:foreign_key b 表中记录 a 的 id，默认是 a_id
:primary_key 默认是 id
:dependent 设置 :destroy 时，删除 a 会自动删除 b
:counter_cache
:as 多态时用到
:through 中间表名称
:source
:source_type
:validate 通过 a 保存 b 时需要校验
:autosave 保存 a 时会自动保存 b
:inverse_of 配合 belongs_to 作用
```

`has_one(name, scope = nil, options = {})`
---------

a has_one :b

```
:class_name 指定 b 所对应的类名，默认是 B
:dependent 当为 :destroy 时，删除 a 会同时删除 b
:foreign_key 指定 b 里记录的 id，默认为 a_id
:primary_key 默认为 id
:as 多态时用到
:through 中间表名称
:source
:source_type
:validate 通过 a 保存 b 时需要校验
:autosave 保存 a 时会自动保存 b
:inverse_of 配合 belongs_to 作用
```

注意：可选参数里 scope 和 options 的区别！

规则：外键永远放在 belongs_to 那一方。
规则：inverse_of 节约性能(不用二次数据库查询)，一致性(object_id是一样的)。不过有 automatic_inverse_of，当你设置了 :class_name, :foreign_key, :polymorphic, 或表名单复数不规则时用到。

|                 | has_one | has_many | belongs_to | habtm |
|----------------------------------|-------|----------|----------|-------------|
|:class_name                           |   √   |    √     |    √  |  √|
|:foreign_key                          |   √   |    √     |    √  |  √|
|:validate                             |   √   |    √     |    √  |  √|
|:autosave                             |   √   |    √     |    √  |  √|
|:primary_key                          |   √   |    √     |    √  ||
|:dependent                            |   √   |    √     |    √  ||
|:inverse_of                           |   √   |    √     |    √  ||
|:as                                   |   √   |    √     |       ||
|:through                              |   √   |    √     |       ||
|:source                               |   √   |    √     |       ||
|:source_type                          |   √   |    √     |       ||
|:counter_cache                        |       |    √     |    √  ||
|:polymorphic                          |       |          |    √  ||
|:touch                                |       |          |    √  ||
|:foreign_type                         |       |          |    √  ||
|:join_table                           |       |          |       |  √|
|:association_foreign_key              |       |          |       |  √|
|:readonly                             |       |          |       |  √|




a association :b

`:class_name` 指定 b 所对应的类名，默认是 B

Singular associations (has_one, belongs_to) no longer have a proxy and simply returns the associated record or nil. This means that you should not use undocumented methods such as bob.mother.create - use bob.create_mother instead.

The behavior of association.destroy for has_and_belongs_to_many and has_many :through is changed. From now on, 'destroy' or 'delete' on an association will be taken to mean 'get rid of the link', not (necessarily) 'get rid of the associated records'.

## Builder

### Association

```ruby
def #{name}(*args)
  association(:#{name}).reader(*args)
end

def #{name}=(value)
  association(:#{name}).writer(value)
end

及其它一些比较重要的方法！
```

### SingularAssociation

```ruby
def build_#{name}(*args, &block)
  association(:#{name}).build(*args, &block)
end

def create_#{name}(*args, &block)
  association(:#{name}).create(*args, &block)
end

def create_#{name}!(*args, &block)
  association(:#{name}).create!(*args, &block)
end

及其它一些比较重要的方法！
```

**BelongsTo**

**HasOne**

### CollectionAssociation

```ruby
def #{name.to_s.singularize}_ids
  association(:#{name}).ids_reader
end

def #{name.to_s.singularize}_ids=(ids)
  association(:#{name}).ids_writer(ids)
end

及其它一些比较重要的方法！
```

**HasMany**

### HasAndBelongsToMany

> **Note:** Rails 4 里 Scope 必需要有一个可调用对象。

---------

You can use it one of three ways, as the following table summarizes::
￼￼￼￼:dependent value :destroy :delete
:nullify
Behavior
Calls destroy() on each child record, invoking callbacks Deletes each child record in a single database query
Sets the foreign key to null for each child record in a single database query

|    :dependent value             | Behavior  |
|----------------------------------| :-----------|
|:destroy                         |   Calls `destroy()` on each child record, invoking callbacks|
|:delete                        |  Deletes each child record in a single database query|
|:nullify | Sets the foreign key to null for each child record in a single database query|
