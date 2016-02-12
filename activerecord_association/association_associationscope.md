### ~~Association Scope~~

**作用**：为 Collection Association、Singular Associations 和 Association 服务。

实现对 scope 的相关操作。

场景举例：

```ruby
module MyApplication
  module Business
    class Supplier < ActiveRecord::Base
       has_one :account
    end
  end
 
  module Billing
    class Account < ActiveRecord::Base
       belongs_to :supplier
    end
  end
end
```

Supplier 和 Account 不在一个命名空间(namespace)里，导致查询不便，Association Scope 可以解决此问题。

