# Callbacks
a方法 类别 b方法

a方法 类别 (语法糖)
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
# a方法
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

始终要用到3个方法。参考 [paranoia 的实现](https://github.com/radar/paranoia/blob/rails4/lib/paranoia.rb)
