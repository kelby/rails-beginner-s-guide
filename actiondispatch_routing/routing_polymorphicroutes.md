## PolymorphicRoutes

If you want to generate different urls according to different objects(例如，多态：`belongs_to :commentable, :polymorphic => true`), you should use the polymorphic_path/polymorphic_url to simplify the url generation.

`polymorphic_url(record_or_hash_or_array, options = {})`

Constructs a call to a named RESTful route for the given record and returns the resulting URL string. For example:

```ruby
# calls post_url(post)
polymorphic_url(post) # => "http://example.com/posts/1"
polymorphic_url([blog, post]) # => "http://example.com/blogs/1/posts/1"
polymorphic_url([:admin, blog, post]) # => "http://example.com/admin/blogs/1/posts/1"
polymorphic_url([user, :blog, post]) # => "http://example.com/users/1/blog/posts/1"
polymorphic_url(Comment) # => "http://example.com/comments"
```
