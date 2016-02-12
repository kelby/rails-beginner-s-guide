### ~~Association Scope~~

场景"关联的时候，关联表带有 scope"，不使用它的话，生成的 SQL 里能看到有的条件是重复的。Association Scope 解决了这个问题。

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

Collection Association、Singular Associations 和 Association 调用了它。
