# Others

## Available Callbacks
Here is a list with all the available Active Record callbacks, listed in the same order in which they will get called during the respective operations:

### Creating an Object

before_validation
after_validation
before_save
around_save
before_create
around_create
after_create
after_save
after_commit/after_rollback

### Updating an Object

before_validation
after_validation
before_save
around_save
before_update
around_update
after_update
after_save
after_commit/after_rollback

### Destroying an Object

before_destroy
around_destroy
after_destroy
after_commit/after_rollback

> **Note:** after_save runs both on create and update, but always after the more specific callbacks after_create and after_update, no matter the order in which the macro calls were executed.

### after_initialize and after_find

The after_initialize callback will be called whenever an Active Record object is instantiated, either by directly using new or when a record is loaded from the database. It can be useful to avoid the need to directly override your Active Record initialize method.

The after_find callback will be called whenever Active Record loads a record from the database. after_find is called before after_initialize if both are defined.

The after_initialize and after_find callbacks have no before_* counterparts, but they can be registered just like the other Active Record callbacks.

```ruby
class User < ActiveRecord::Base
  after_initialize do |user|
    puts "You have initialized an object!"
  end

  after_find do |user|
    puts "You have found an object!"
  end
end

>> User.new
You have initialized an object!
=> #<User id: nil>

>> User.first
You have found an object!
You have initialized an object!
=> #<User id: 1>
```

### after_touch

The after_touch callback will be called whenever an Active Record object is touched.

```ruby
class User < ActiveRecord::Base
  after_touch do |user|
    puts "You have touched an object"
  end
end

>> u = User.create(name: 'Kuldeep')
=> #<User id: 1, name: "Kuldeep", created_at: "2013-11-25 12:17:49", updated_at: "2013-11-25 12:17:49">

>> u.touch
You have touched an object
=> true
```

It can be used along with belongs_to:

```ruby
class Employee < ActiveRecord::Base
  belongs_to :company, touch: true
  after_touch do
    puts 'An Employee was touched'
  end
end

class Company < ActiveRecord::Base
  has_many :employees
  after_touch :log_when_employees_or_company_touched

  private
  def log_when_employees_or_company_touched
    puts 'Employee/Company was touched'
  end
end

>> @employee = Employee.last
=> #<Employee id: 1, company_id: 1, created_at: "2013-11-25 17:04:22", updated_at: "2013-11-25 17:05:05">

# triggers @employee.company.touch
>> @employee.touch
Employee/Company was touched
An Employee was touched
=> true
```
