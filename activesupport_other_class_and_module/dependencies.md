## ~~Dependencies~~

依赖。

**Class Cache**

```
[] & get

clear!

empty?

key?

safe_get

store
```

**Watch Stack**

```
each

new_constants

watch_namespaces

watching?
```

另：

自动加载会从 app/models, app/controllers, lib/ and other load paths.等加载，如果没有会：

```ruby
# in active_support/dependencies.rb
def const_missing(const_name)
  from_mod = anonymous? ? guess_for_anonymous(const_name) : self
  Dependencies.load_missing_constant(from_mod, const_name)
end
```

然后：

```ruby
#lib/active_support/dependencies.rb:477
expanded = File.expand_path(file_path)
expanded.sub!(/\.rb\z/, '')

if loading.include?(expanded)
  raise "Circular dependency detected while autoloading constant #{qualified_name}"
end
```

此处有魔法。
