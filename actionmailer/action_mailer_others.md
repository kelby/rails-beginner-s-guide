## 其它

### 更多关于 Action Mailer

Action Mailer 使用模板来创建邮件与 Action Controller 使用模板渲染视图，原理类似。并且，渲染过程都会运用到 Action View 的 Rendering 模块。

Action Mailer 提供我们 mailer 类和视图，mailer 类和 controller 非常相似。它们继承于 ActionMailer::Base 并放在 app/mailers 目录下，它们有自己关联的视图文件在 app/views 目录下。

ActionMailer::Base 继承于类 AbstractController::Base,
又包含但不限于以下'外部'模块，根据 Ruby 规则，它们也是可调用的。包括：

| 模块 | 作用 |
| -- | -- |
| ActionMailer::DeliveryMethods | 邮件发送 |
| ActionMailer::Previews | 邮件预览 |
| AbstractController::Rendering | 渲染 |
| AbstractController::Helpers | 引进、输出辅助方法 |
| AbstractController::Translation | I18n 相关 |
| AbstractController::Callbacks | 支持回调 |
| ActionView::Layouts | 视图布局等 |

`C < B < A` 

有时候 B 要 extend 前人 A，并不是为了 B 自己使用，而是为了方便后人 C. ActionMailer::Base 部分代码充当的角色和 B 一样，它自己却没有使用，而是为了方便我们自定义的 Mailer 类调用。

如：

Mailer 里：

```ruby
before_action :add_inline_attachment!

layout "mailer"

helper :application
```

View 里：

```ruby
<%= url_for(host: "example.com", controller: "welcome", action: "greeting") %>

<%= users_url(host: "example.com") %>
```

### 快速生成 Mailer 和模板

通过以下命令创建 mailer 类和视图：

```
$ rails g mailer UserMailer welcome

create  app/mailers/user_mailer.rb
invoke  erb
create    app/views/user_mailer
create    app/views/user_mailer/welcome.text.erb
invoke  test_unit
create    test/mailers/user_mailer_test.rb
```

### ~~Collector~~

mail 方法可以接收一个代码块，你可以在这里指定渲染模板的格式及内容等：

```ruby
mail(to: user.email) do |format|
  format.text
  format.html
end

# 或

mail(to: user.email) do |format|
  format.text { render plain: "Hello World!" }
  format.html { render html: "<h1>Hello World!</h1>".html_safe }
end
```

也只有在这个时候，这里的 Collector 才有用到。它和 AbstractController::Collector 相关，也就是和 Mime 相关。

> Note: 默认会发送和 mail 所在方法名同名的所有模板，不区分 Mime 格式。这也是我们常用的。

### ~~Delivery Job~~

使用 Active Job，配置以便延迟发送邮件。
