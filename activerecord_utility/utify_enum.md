## Enum

```ruby
definitions.each do |name, values|
  klass.singleton_class.send(:define_method, name.to_s.pluralize)

  define_method("#{name}=")
  define_method(name)

  pairs.each do |value, i| # pairs 约等于 values

    define_method("#{value}?")
    define_method("#{value}!")
```

假设，我们有 `status` 字段，那么使用 `enum` 方法会引入什么魔法：

```ruby
class Post < ActiveRecord::Base
  enum status: [ :active, :archived ]
end
```

```ruby
# 1 同名类方法(复数形式)
Post.statuses
=> {"active"=>0, "archived"=>1}

# 2 值(scope)
Post.active
  Post Load (1.9ms)  SELECT "posts".* FROM "posts"  WHERE "posts"."status" = 0

post = Post.first

# 3 值?
post.active?
=> false

# 4 值!
post.active!
   (0.1ms)  begin transaction
  SQL (0.4ms)  UPDATE "posts" SET "status" = ?, "updated_at" = ? WHERE "posts"."id" = 2  [["status", 0], ["updated_at", "2014-04-20 09:06:53.722202"]]
   (8.7ms)  commit transaction
=> true

# 5 同名实例方法(get)
post.status
=> "active"

# 6 同名实例方法=(set)
post.status = "archived"
=> "archived"

# 7 和同名实例方法(get)作用一样
post.status_before_type_cast
=> "archived"
```

它们对应的源代码：

```ruby
def enum(definitions)
  klass = self
  definitions.each do |name, values|
    # statuses = { }
    enum_values = ActiveSupport::HashWithIndifferentAccess.new
    name        = name.to_sym

    # def self.statuses statuses end
    detect_enum_conflict!(name, name.to_s.pluralize, true)
    # 1 同名类方法(复数形式)
    klass.singleton_class.send(:define_method, name.to_s.pluralize) { enum_values }

    _enum_methods_module.module_eval do
      # def status=(value) self[:status] = statuses[value] end
      klass.send(:detect_enum_conflict!, name, "#{name}=")
      # 6 同名实例方法=(set)
      define_method("#{name}=") { |value|
        if enum_values.has_key?(value) || value.blank?
          self[name] = enum_values[value]
        elsif enum_values.has_value?(value)
          # Assigning a value directly is not a end-user feature, hence it's not documented.
          # This is used internally to make building objects from the generated scopes work
          # as expected, i.e. +Conversation.archived.build.archived?+ should be true.
          self[name] = value
        else
          raise ArgumentError, "'#{value}' is not a valid #{name}"
        end
      }

      # def status() statuses.key self[:status] end
      klass.send(:detect_enum_conflict!, name, name)
      # 5 同名实例方法(get)
      define_method(name) { enum_values.key self[name] }

      # def status_before_type_cast() statuses.key self[:status] end
      klass.send(:detect_enum_conflict!, name, "#{name}_before_type_cast")
      # 7 和同名实例方法(get)作用一样
      define_method("#{name}_before_type_cast") { enum_values.key self[name] }

      pairs = values.respond_to?(:each_pair) ? values.each_pair : values.each_with_index
      pairs.each do |value, i|
        enum_values[value] = i

        # def active?() status == 0 end
        klass.send(:detect_enum_conflict!, name, "#{value}?")
        # 3 值?
        define_method("#{value}?") { self[name] == i }

        # def active!() update! status: :active end
        klass.send(:detect_enum_conflict!, name, "#{value}!")
        # 4 值！
        define_method("#{value}!") { update! name => value }

        # scope :active, -> { where status: 0 }
        klass.send(:detect_enum_conflict!, name, value, true)
        # 2 值(scope)
        klass.scope value, -> { klass.where name => i }
      end
    end
    defined_enums[name.to_s] = enum_values
  end
end
```
