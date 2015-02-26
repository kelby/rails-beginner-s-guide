## 其它

### ForbiddenAttributesError

```
class Person < ActiveRecord::Base
end

params = ActionController::Parameters.new(name: 'Bob')
Person.new(params)
# => ActiveModel::ForbiddenAttributesError

params.permit!
Person.new(params)
# => #<Person id: nil, name: "Bob">
```
