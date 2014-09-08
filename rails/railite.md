# Railite

Railtie 是 Rails 的核心部分之一。通过它，可以扩展和/或修改 Rails 的初始化程序。

每一个 Rails 组件(如：Action Mailer, Action Controller,
Action View 和 Active Record等)都属于 Railtie. 因为它们都需要自己的初始化程序。

什么时候需要使用 Railtie? 当你的扩展符合下列情况时，可以考虑：

* creating initializers
* configuring a Rails framework for the application, like setting a generator
* adding <tt>config.*</tt> keys to the environment
* setting up a subscriber with ActiveSupport::Notifications
* adding rake tasks

## Engine 和 Application

Engine 是 Railtie 的子类，所以这里的方法，由于继承关系，它也可以使用。  
Application 是 Engine 的子类，所以这里的方法，由于继承关系，它也可以使用。
