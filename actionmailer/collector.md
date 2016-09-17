## Collector

支持响应不同格式的邮件模板，就和 Controller#action 里我们编写的类似：

```ruby
mail(to: 'mikel@test.lindsaar.net') do |format|
  format.text
  format.html
end
```
