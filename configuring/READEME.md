# Rails 里所有的配置项

链接 [Configuring Rails Applications](http://edgeguides.rubyonrails.org/configuring.html)

附录：

```ruby
railties = ['action_mailer', 'active_record', 'action_controller',
'action_dispatch', 'action_view', 'active_support',
'i18n', 'assets']

railties.each do |railtie|
  p railtie.camelize

  klasses = Rails.configuration.action_dispatch.values.map(&:class).uniq

  klasses.each do |klass|
    p klass
    Rails.configuration.action_dispatch.each_pair do |k,v|
      p k if v.class == klass
    end
  end
end
```
