# Rails 里所有的配置项

链接 [Configuring Rails Applications](http://edgeguides.rubyonrails.org/configuring.html)

配置项，除了 Configuration 对象外，还可以由 `class_attribute` 进行设置。
<br>
前者相当于只有一个总开关，只能决定开或关。而后者除了总开关外，还可以有分开关，总开关设置状态后，可以不设置状态（继承），或者设置自己的状态。

附录：

```ruby
railties = ['action_mailer', 'active_record', 'action_controller',
  'action_dispatch', 'action_view', 'active_support', 'i18n', 'assets']

railties.each do |railtie|
  p railtie.camelize
  p "=============="

  klasses = Rails.configuration.send(railtie).values.map(&:class).uniq

  klasses.each do |klass|
    p klass

    Rails.configuration.send(railtie).each_pair do |k,v|
      p k if v.class == klass
    end
  end
end
```
