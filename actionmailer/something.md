# 源码导读

**Base**

我们 mailer 类继承的 ActionMailer::Base 在这里定义的，它继承于 AbstractController::Base，这也是 ActionMailer 依赖 AbstractController 的证据之一。

包含了一些自身及 AbstractController 的模块，尽管有的模块它并没有使用到，而是为了让它的子类(我们的 mailer 类)能够"更好用、更实用"。

包含了一些默认配置 default_params.

注册拦截器(interceptor)或观察者(observer)。

请求到邮件处理 process.

定义了 attachments、default、headers、mail、receive 等常用方法。

**MailHelper**

几个邮件相关的 Helper 方法。如：attachments、mailer、message，还有不太实用的 block_format、format_paragraph.

**Collector**

和 AbstractController::Collector 相关，也就是和 Mime 相关。

mail 方法可以接收一个代码块，你可以在这里指定渲染模板的格式及内容等：

```ruby
mail(to: 'mikel@test.lindsaar.net') do |format|
  format.text
  format.html
end

# 或

mail(to: 'mikel@test.lindsaar.net') do |format|
  format.text { render plain: "Hello Mikel!" }
  format.html { render html: "<h1>Hello Mikel!</h1>".html_safe }
end
```

也只有在这个时候，这里的 Collector 才有用到。

> Note: 默认会发送和 mail 所在方法名同名的所有模板，不区分 Mime 格式。

**DeliveryMethods**

邮件发送方式 add_delivery_method.

**LogSubscriber**

日志记录，继承于 ActiveSupport::LogSubscriber，执行哪个方法时想要记录日志，只需要创建和它同名方法，然后打印日志即可。LogSubscriber 章节会讲到。

**Previews & Preview**

邮件预览相关。

Previews，对于普通开发者来说主要是配置：

Set the location of mailer previews through app configuration:

    config.action_mailer.preview_path = "#{Rails.root}/lib/mailer_previews"

Enable or disable mailer previews through app configuration:

    config.action_mailer.show_previews = true

Defaults to true for development environment

Preview，是我们自定义 YourPreview 的父类，提供一些普通Web开发者察觉不到的方法，如：

`preview_name` Returns the underscored name of the mailer preview without the suffix

`emails` Returns all of the available email previews

这里的预览，和 [MailCatcher](https://github.com/sj26/mailcatcher)、[Letter Opener](https://github.com/ryanb/letter_opener) 等提供的预览不同，它属于规范的测试，而后者更类似于人肉测试。

> Note: 邮件预览，在 Rails 里也遵守 MVC. M 是 ActionMailer::Preview，V 是 rails/mailers/，C 是 Rails::MailersController

**Railtie**

ActionMailer 的 Railtie 配置及初始化。Railtie 章节会讲到。

**DeliveryJob**

使用 ActiveJob，配置以便延迟发送邮件。

**TestCase**

测试样例。

**TestHelper**

测试方法 assert_emails 和 assert_no_emails，本质是封装 assert_equal.
