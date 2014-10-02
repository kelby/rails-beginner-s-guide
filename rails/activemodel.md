# ActiveModel

included by ResourceHelpers & ModelHelpers

**放到这里到底有什么用？**

ActiveModel is a class to be implemented by each ORM to allow Rails to generate customized controller code.

The API has the same methods as ActiveRecord, but each method returns a string that matches the ORM API.

For example:

```ruby
ActiveRecord::Generators::ActiveModel.find(Foo, "params[:id]")
# => "Foo.find(params[:id])"

DataMapper::Generators::ActiveModel.find(Foo, "params[:id]")
# => "Foo.get(params[:id])"
```

On initialization, the ActiveModel accepts the instance name that will receive the calls:

```ruby
builder = ActiveRecord::Generators::ActiveModel.new "@foo"
builder.save # => "@foo.save"
```

The only exception in ActiveModel for ActiveRecord is the use of self.build instead of self.new.

## Class Public methods

```
all
build
find
new
```

## Instance Public methods

```
destroy
errors
save
update
```
