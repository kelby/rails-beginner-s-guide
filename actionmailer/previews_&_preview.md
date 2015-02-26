## Previews & Preview

**邮件预览相关。**

Previews，对于普通开发者来说主要是配置：

```ruby
# 配置预览文件存放的位置，默认如下:
config.action_mailer.preview_path = "#{Rails.root}/lib/mailer_previews"

# 配置是否允许邮件预览。开发模式下，默认为 true
config.action_mailer.show_previews = true
```

默认可以到以下 url, 查看预览邮件：

```
http://localhost:3000/rails/mailers/
```

Preview，是我们自定义 YourPreview 的父类，提供一些普通 Web 开发者察觉不到的方法，如：

`preview_name` 返回自定义类名，但把 "Preview" 后缀去掉。如 YourPreview 返回 "Your"

`emails` 返回所有可预览的邮件

这里的预览，和 [Mail Catcher](https://github.com/sj26/mailcatcher)、[Letter Opener](https://github.com/ryanb/letter_opener) 等提供的预览不同，它属于规范的测试，而后者更类似于人肉测试。

> Note: 邮件预览，在 Rails 里也遵守 MVC. M 是 ActionMailer::Preview，V 是 rails/mailers/，C 是 Rails::MailersController

Preview 提供类方法：

```
all
emails

preview_name

call
email_exists?

find
exists?
```

Previews 提供类方法：

```
register_preview_interceptor
register_preview_interceptors
```
