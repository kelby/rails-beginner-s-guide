## 获取 record 对象

归结起来，有两种方式可以获得 record 对象。

从数据库里获取或新创建。

前者可以通过各种方法，查询得到，但都是封装了 init_with

后者也可以通过各种方法，创建得到，但都是封装了 initialize

```ruby
# 查询得到。从 SQL 层面转向到 Ruby 层面
def find_by_sql(sql, binds = [])
  result_set.map { |record| instantiate(record, column_types) }
end
```

`instantiate` 查询得到，等价于：`self.allocate` + `init_with`

`initialize` 创建得到，比如：`new` 或 `build`

前者，也就是查询得到，一定有 persisted? # => true

后者，也就是创建得到，一定有 persisted? # => false

相关类/模板及其方法：

```
ActiveRecord::Querying

initialize
init_with
init_internals
== 和 eql?
<=>

ActiveRecord::Persistence

persisted?
instantiate
delete
destroy
create_record

ActiveRecord::Querying

find_by_sql
```

其它类似：

```
clone
dup

initialize_dup
```
参考

[ActiveRecord 对象的拼装](http://thekaiway.com/2013/07/26/assemble-ar-object/)
<br>
[Read and Write Activerecord Attribute](http://thekaiway.com/2013/09/08/read-write-activerecord-attribute/)
<br>
[ActiveRecord Object Instantiate](https://ruby-china.org/topics/23523)
<br>
[Using Allocate to Create ActiveRecord Objects When Stubbing find_by_sql](http://neovintage.blogspot.jp/2010/10/using-allocate-to-create-activerecord.html)
