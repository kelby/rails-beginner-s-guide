## 其它

#### ~~Forbidden Attributes Error~~

```ruby
class Person < ActiveRecord::Base
end

params = ActionController::Parameters.new(name: 'Bob')
Person.new(params)
# => ActiveModel::ForbiddenAttributesError

params.permit!
Person.new(params)
# => #<Person id: nil, name: "Bob">
```
