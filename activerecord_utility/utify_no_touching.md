## No Touching

常用方法：

```
no_touching
```

使用举例：

```ruby
ActiveRecord::Base.no_touching do
  Project.first.touch # does nothing
  Message.first.touch # does nothing
end

Project.no_touching do
  Project.first.touch # does nothing
  Message.first.touch # works, but does not touch the associated project
end
```

除上述外，还有方法：

```
no_touching?
touch
```
