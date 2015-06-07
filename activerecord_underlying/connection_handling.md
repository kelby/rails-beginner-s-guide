## Connection Handling 连接数据库

```
# 根据配置信息进行连接
establish_connection

# 是否已连接
connected?

# 连接信息
connection

# 连接的配置信息
connection_config

connection_id
connection_id=

connection_pool

remove_connection
retrieve_connection
```

通过 `establish_connection` 连接数据库，我们不必加载整个 Rails 环境，仅使用数据库操作这部分。

举例一：

```ruby
require 'yaml'
require 'active_record'

dbconfig = YAML::load(File.open('config/database.yml'))
# 这里以 development 环境为例
ActiveRecord::Base.establish_connection(dbconfig["development"])
# ActiveRecord::Base.logger = Logger.new(STDERR)

class User < ActiveRecord::Base
  # your code here ...
end
```

举例二(立即加载基准测试脚本)：

```ruby
require 'rubygems'
require 'faker'
require 'active_record'
require 'benchmark'

# This call creates a connection to our database.

ActiveRecord::Base.establish_connection(
  :adapter => "mysql2",
  :host => "127.0.0.1",
  :username => "root", # Note that while this is the default setting for MySQL,
  :password => "", # a properly secured system will have a different MySQL
  # username and password, and if so, you'll need to
  # change these settings.
  :database => "test")

# First, set up our database...
class Category < ActiveRecord::Base
end

unless Category.table_exists?
  ActiveRecord::Schema.define do
    create_table :categories do |t|
      t.column :name, :string
    end
  end
end

Category.create(:name => 'Sara Campbell\'s Stuff')
Category.create(:name => 'Jake Moran\'s Possessions')
Category.create(:name => 'Josh\'s Items')
number_of_categories = Category.count

class Item < ActiveRecord::Base
  belongs_to :category
end

# If the table doesn't exist, we'll create it.

unless Item.table_exists?
  ActiveRecord::Schema.define do
    create_table :items do |t|
      t.column :name, :string
      t.column :category_id, :integer
    end
  end
end

puts "Loading data..."

item_count = Item.count
item_table_size = 10000

if item_count < item_table_size
  (item_table_size - item_count).times do
    Item.create!(:name => Faker.name,
                 :category_id => (1+rand(number_of_categories.to_i)))
  end
end

puts "Running tests..."

Benchmark.bm do |x|
  [100, 1000, 10000].each do |size|
    x.report "size:#{size}, with n+1 problem" do
      @items=Item.find(:all, :limit => size)
      @items.each do |i|
        i.category
      end
    end
    x.report "size:#{size}, with :include" do
      @items=Item.find(:all, :include => :category, :limit => size)
      @items.each do |i|
        i.category
      end
    end
  end
end
```

举例三：

```ruby
require 'active_record'

ActiveRecord::Base.logger = Logger.new(STDERR)
# ActiveRecord::Base.colorize_logging = false

ActiveRecord::Base.establish_connection(
  :adapter => "sqlite3",
  :database => ":memory:"
)

ActiveRecord::Schema.define do
  create_table :albums do |table|
    table.column :title, :string
    table.column :performer, :string
  end

  create_table :tracks do |table|
    table.column :album_id, :integer
    table.column :track_number, :integer
    table.column :title, :string
  end
end

class Album < ActiveRecord::Base
  has_many :tracks
end

class Track < ActiveRecord::Base
  belongs_to :album
end
```

`connection` 使用举例：

```ruby
ActiveRecord::Base.connection.table_exists? 'some_table'
```

另外一个使用 `execute` 方法举例：

```ruby
ActiveRecord::Base.connection.execute "ALTER TABLE some_table CHANGE COLUMN..."
```
