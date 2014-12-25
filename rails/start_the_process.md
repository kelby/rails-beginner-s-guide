## Rails 应用启动过程

1) 入口 config.ru

```
require ::File.expand_path('../config/environment',  __FILE__)
```

2) 转入 environment.rb

```
require File.expand_path('../application', __FILE__)
```

3) 转入 application.rb

```
require File.expand_path('../boot', __FILE__)
```

4) 转入 boot.rb

```
ENV['BUNDLE_GEMFILE'] ||= File.expand_path('../../Gemfile', __FILE__)
```

5) Gemfile

```
gem 'gem_name'
```

6) 回到 boot.rb

执行 bundle

7) application.rb

7.1) require 'something' # like require 'rails'

7.2) AppName::Application < Rails::Application

7.3) config your AppName! 在上一步继承之后就已经有配置了。

8) enviroment.rb

AppName::Application.initialize!

```ruby
# Initialize the application passing the given group. By default, the
# group is :default
def initialize!(group=:default) #:nodoc:
  raise "Application has been already initialized." if @initialized
  run_initializers(group, self)
  @initialized = true
  self
end
```

8.1) Bootstrap

  8.2) 默认的 Railtie, Engine, Application

  8.3) 定制的 Railtie, Engine

构建 middleware stack.<br>
(Rails.application.send :middleware 查看 middleware, 顺序从前到后)  
(Rails.application.send :default_middleware_stack 查看 middleware, 顺序是默认)

8.4) Finisher

上面是我自己归纳的。

---

启动过程基本上都在：

```
Rails::Application - config/application.rb

Rails::Railtie::Configuration - configuration.rb
```

config/application.rb
里先 require 再 config 最后 eager_load.

相关代码：

```
AppName.initialize!
```

```
run_initializers(group, self)


initializers.tsort_each do |initializer|
  initializer.run(*args) if initializer.belongs_to?(group)
end


@initializers ||= self.class.initializers_for(self)

Collection.new(initializers_chain.map { |i| i.bind(binding) })

def initializers_chain
  initializers = Collection.new
  ancestors.reverse_each do |klass|
    next unless klass.respond_to?(:initializers)
    initializers = initializers + klass.initializers
  end
  initializers
end


def run(*args)
  @context.instance_exec(*args, &block)
end
```

各样的钩子，如：

```
before_configuration

before_eager_load

before_initialize

after_initialize
```

它们在 Railtie::Configuration 里定义，使用范围很广。

最后才是 initializer!
