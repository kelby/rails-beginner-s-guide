# Migration say_with_time 方法

使用举例：

```ruby
def up
  ...
  say_with_time "Updating salaries..." do
    Person.all.each do |p|
      p.update_attribute :salary, SalaryCalculator.compute(p)
    end
  end
  ...
end
```
