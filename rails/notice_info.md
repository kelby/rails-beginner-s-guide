## 提示信息页面

**ApplicationController**
<br>
这里指的是 Rails 自带的 Rails::ApplicationController, 不是我们 Controller 里继承的 Application.
<br>
它和 Application 没有对应关系，只是下面几个 Controller 的父类。

**WelcomeController**
<br>
新建 Rails 项目，没有任何路由及内容时，默认显示的首页 index.

**MailersController**
<br>
邮件预览的列表 index 和详情 preview 页。

**InfoController & Info**
<br>
项目信息页面，包括 Rails、Ruby、Rack 版本等。
<br>
有页面：index(同 routes) 和 properties.
<br>
默认首页里点击"
About your application’s environment" 可查看其内容。
