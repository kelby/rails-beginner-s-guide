# ActiveRecord 底层

## Serialization

`serializable_hash(options = nil)` 和 `as_json` 结果一样

## ConnectionHandling

```
# 对外用得比较多的
establish_connection

# 其它
connected?, connection, connection_config, connection_id, connection_id=, connection_pool
remove_connection, retrieve_connection
```

通过它连接数据库，我们不必加载整个 Rails 环境，仅使用数据库操作这部分。举例：

```ruby
require 'yaml'
require 'active_record'

dbconfig = YAML::load(File.open('config/database.yml'))
# 这里以 development 环境为例
ActiveRecord::Base.establish_connection(dbconfig["development"])

class User < ActiveRecord::Base
  # your code here ...
end
```
