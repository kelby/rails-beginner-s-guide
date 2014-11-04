### create_table

可选参数：

**:id**

```ruby
create_table(:categories_suppliers, id: false) do |t|
  t.column :category_id, :integer
  t.column :supplier_id, :integer
end
```

**:primary_key**

```ruby
# 改名字叫 guid
create_table(:products, primary_key: 'guid') do |t|
  # ...
end

# 主键仍然是 integer 类型
create_table :products, id: false do |t|
  t.primary_key :sku
  # ...
end
```

上述两种解决方法类似，但它们类型都是 integer ... 比如我们想要 string 呢，只能这样：

1 使用 SQL 创建主键

```ruby
execute "ALTER TABLE employees ADD PRIMARY KEY (emp_id);"
```

问题解决了，但另一问题又来了，这么做迁移不记录在案。如果想要记录，我们还要


```ruby
config.active_record.schema_format = :sql
```

显然，这并不是好的做法。

另一解法是：

2 类似主键，但不是主键

```ruby
create_table :employees, :primary_key => :emp_id do |t|
  t.string :first_name
  t.string :last_name
end
change_column :employees, :emp_id, :string
```

上述方法，并不是真正的 primary_key，其实是 string

另外，

更改主键，则 `id` 和 `id=` 也会做相应变化，如果你觉得别扭，可以重置它们(尽管不推荐)。

```ruby
class Product < ActiveRecord::Base
  ....
  # 很多方法都依赖于 id，不推荐这么做
  def id
    raise NoMethodError, "Please call #{self.class.primay_key} instead."
  end

  def id=(value)
    raise NoMethodError, "Please call #{self.class.primay_key}= instead."
  end
  ....
end
```

其它地方相应更改：

```ruby
# app/models/product.rb
class Product < ActiveRecord::Base
  # model 层面指定 primay_key
  self.primay_key = 'sku'
  
  # 重写 to_param，更友好的 params
  def to_param
    sku.parameterize
  end
end

# config/routes.rb
# 路由里使用自定义主键代替 :id
# 在这里是 prams[:sku] 代替 params[:id]
resources :products, param: :sku
```

> Note: 总结，使用 string 类型做主键，还没有好的解决方案。

**:options**

```ruby
create_table :products, options: "ENGINE=BLACKHOLE" do |t|
  t.string :name, null: false
end
```

**:temporary**

```ruby
create_table(:long_query, temporary: true,
  as: "SELECT * FROM orders INNER JOIN line_items ON order_id=orders.id")
```

**:force**

创建表之前先删除它(如果已经存在同名表的话)，确保创建过程是顺利的。

功能类似：

```ruby
drop_table :table_name if table_exists?("table_name")
create_table :table_name
```

**:as**

```ruby
create_table(:long_query, temporary: true,
  as: "SELECT * FROM orders INNER JOIN line_items ON order_id=orders.id")
```

## change_table

change_table 时的 :bulk 参数
并且，這對已有不少資料量的資料庫來說，會有不少執行速度上的差異，可以減少資料庫因為修改被 Lock 鎖定的時間。
