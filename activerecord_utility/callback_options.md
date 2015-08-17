### 可选参数

**:on**

使用 :on 指定要关联的事件。

```ruby
class User < ActiveRecord::Base
  before_validation :normalize_name, on: :create
 
  # :on takes an array as well
  after_validation :set_location, on: [ :create, :update ]
 
  protected
    def normalize_name
      self.name = self.name.downcase.titleize
    end
 
    def set_location
      self.location = LocationService.query(self)
    end
end
```

**:prepend**

```ruby
class Topic < ActiveRecord::Base
  has_many :children, dependent: destroy

  before_destroy :log_children

  private
    def log_children
      # Child processing
    end
end
```

topic 已经被删除了，没办法 log_children，可以改为这样：

```ruby
class Topic < ActiveRecord::Base
  has_many :children, dependent: destroy

  before_destroy :log_children, prepend: true

  private
    def log_children
      # Child processing
    end
end
```

**:if** 和 **:unless**

```ruby
class Order < ActiveRecord::Base
  before_save :normalize_card_number, if: :paid_with_card?
end

class Order < ActiveRecord::Base
  before_save :normalize_card_number, if: "paid_with_card?"
end

class Order < ActiveRecord::Base
  before_save :normalize_card_number,
    if: Proc.new { |order| order.paid_with_card? }
end

class Comment < ActiveRecord::Base
  after_create :send_email_to_author, if: :author_wants_emails?,
    unless: Proc.new { |comment| comment.article.ignore_comments? }
end
```
