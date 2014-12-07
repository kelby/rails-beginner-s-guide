### Callbacks

```ruby
klazz.define_callbacks :restore

klazz.define_singleton_method("before_restore") do |*args, &block|
  set_callback(:restore, :before, *args, &block)
end

klazz.define_singleton_method("around_restore") do |*args, &block|
  set_callback(:restore, :around, *args, &block)
end

klazz.define_singleton_method("after_restore") do |*args, &block|
  set_callback(:restore, :after, *args, &block)
end
```

### a 方法

```ruby
def restore!(opts = {})
  ActiveRecord::Base.transaction do
    run_callbacks(:restore) do
      update_column paranoia_column, nil
      restore_associated_records if opts[:recursive]
    end
  end
end

alias :restore :restore!
```

至少要用到 3 个方法 define_callbacks、set_callback、run_callbacks 不能再少了，使用例子很多，例如 [paranoia 的实现](https://github.com/radar/paranoia/blob/rails4/lib/paranoia.rb)
