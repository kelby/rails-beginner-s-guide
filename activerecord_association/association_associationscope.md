### ~~Association Scope~~

**作用**：为 Collection Association、Singular Associations 和 Association 服务。

实现对 scope 的相关操作。

场景举例：

```ruby
module MyApplication
  module Business
    class Supplier < ActiveRecord::Base
      has_one :account,
        class_name: "MyApplication::Billing::Account"
    end
  end
 
  module Billing
    class Account < ActiveRecord::Base
      belongs_to :supplier,
        class_name: "MyApplication::Business::Supplier"
    end
  end
end
```

```ruby
class Review < ActiveRecord::Base
  belongs_to :restaurant
  belongs_to :user

  scope :positive, -> { where("rating > 3.0") }
end

class Restaurant < ActiveRecord::Base
  has_many :reviews
  has_many :postitive_reviews, -> { positive }, class_name: "Review"
end

class User < ActiveRecord::Base
  has_many :reviews
  has_many :postitive_reviews, -> { positive }, class_name: "Review"
end
```

```ruby
restaurants = Restaurant.includes(:positive_reviews).first(5)
```
