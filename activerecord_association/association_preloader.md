### ~~Preloader~~

实现 includes, preload, eager_load.

```ruby
class Author < ActiveRecord::Base
  # columns: name, age
  has_many :books
end

class Book < ActiveRecord::Base
  # columns: title, sales, author_id
end
```

针对上述关联，执行以下两种查询：

```ruby
Author.includes(:books).where(name: ['bell hooks', 'Homer']).to_a

Author.includes(:books).where(books: {title: 'Illiad'}).to_a
```

它们对应的 SQL 差别很大。究竟要执行哪种查询，可以让 Preloader 来判断。
