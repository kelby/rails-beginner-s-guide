# ActiveRecord 约定、配置

## 类映射表，属性映射列

使用 Rails 框架时，这个不由我们考些。

## Naming Conventions

By default, Active Record uses some naming conventions to find out how the mapping between models and database tables should be created. Rails will pluralize your class names to find the respective database table. So, for a class Book, you should have a database table called books. The Rails pluralization mechanisms are very powerful, being capable to pluralize (and singularize) both regular and irregular words. When using class names composed of two or more words, the model class name should follow the Ruby conventions, using the CamelCase form, while the table name must contain the words separated by underscores. Examples:

Database Table - Plural with underscores separating words (e.g., book_clubs).
Model Class - Singular with the first letter of each word capitalized (e.g., BookClub).

|Model / Class |	Table / Schema|
|:----:|:---:|
|Post|	posts|
|LineItem|	line_items|
|Deer|	deers|
|Mouse|	mice|
|Person	|people|

## Schema Conventions

Active Record uses naming conventions for the columns in database tables, depending on the purpose of these columns.

 - **Foreign keys** - These fields should be named following the pattern singularized_table_name_id (e.g., item_id, order_id). These are the fields that Active Record will look for when you create associations between your models.

- **Primary keys** - By default, Active Record will use an integer column named id as the table's primary key. When using Active Record Migrations to create your tables, this column will be automatically created.

There are also some optional column names that will add additional features to Active Record instances:

- created_at - Automatically gets set to the current date and time when the record is first created.
- updated_at - Automatically gets set to the current date and time whenever the record is updated.
- lock_version - Adds optimistic locking to a model.
- type - Specifies that the model uses Single Table Inheritance.
- (association_name)_type - Stores the type for polymorphic associations.
- (table_name)_count - Used to cache the number of belonging objects on associations. For example, a comments_count column in a Post class that has many instances of Comment will cache the number of existent comments for each post.

> While these column names are optional, they are in fact reserved by Active Record. Steer clear of reserved keywords unless you want the extra functionality. For example, type is a reserved keyword used to designate a table using Single Table Inheritance (STI). If you are not using STI, try an analogous keyword like "context", that may still accurately describe the data you are modeling.

## Timestamp

Active Record automatically timestamps create and update operations if the
table has fields named `created_at/created_on` or
`updated_at/updated_on`.
  
Timestamping can be turned off by setting:
  
    config.active_record.record_timestamps = false
  
Timestamps are in UTC by default but you can use the local timezone by setting:
  
    config.active_record.default_timezone = :local
  
### Time Zone aware attributes
  
By default, ActiveRecord::Base keeps all the datetime columns time zone aware by executing following code.
  
    config.active_record.time_zone_aware_attributes = true
  
This feature can easily be turned off by assigning value `false` .

If your attributes are time zone aware and you desire to skip time zone conversion to the current Time.zone
when reading certain attributes then you can do following:
  
```ruby
class Topic < ActiveRecord::Base
  self.skip_time_zone_conversion_for_attributes = [:written_on]
end
```
