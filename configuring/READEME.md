# Rails 里所有的配置项

链接 [Configuring Rails Applications](http://edgeguides.rubyonrails.org/configuring.html)

附录：

```ruby
klasses = Rails.configuration.action_dispatch.values.map(&:class).uniq

klasses.each do |klass|
  p klass
  Rails.configuration.action_dispatch.each_pair do |k,v|
    p k if v.class == klass
  end
end
```
