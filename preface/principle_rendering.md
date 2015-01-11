## 非 Rails 是如何渲染的

先看例子，然后看看非 Rails 是如何渲染的，需要哪几个重要元素。

### 标准库 ERB 举例

```ruby
require "erb"

# build data class
class Listings
  PRODUCT = { :name => "Chicken Fried Steak",
              :desc => "A well messages pattie, breaded and fried.",
              :cost => 9.95 }

  attr_reader :product, :price

  def initialize(product = "", price = "")
    @product = product
    @price = price
  end

  def build
    b = binding
    # create and run templates, filling member data variables
    ERB.new(<<-'END_PRODUCT'.gsub(/^\s+/, ""), 0, "", "@product").result b
      <%= PRODUCT[:name] %>
      <%= PRODUCT[:desc] %>
    END_PRODUCT
    ERB.new(<<-'END_PRICE'.gsub(/^\s+/, ""), 0, "", "@price").result b
      <%= PRODUCT[:name] %> -- <%= PRODUCT[:cost] %>
      <%= PRODUCT[:desc] %>
    END_PRICE
  end
end

# setup template data
listings = Listings.new
listings.build

puts listings.product + "\n" + listings.price
```

输出结果

```
Chicken Fried Steak
A well messages pattie, breaded and fried.

Chicken Fried Steak -- 9.95
A well messages pattie, breaded and fried.
```

上面的例子，也许没能体现环境的变化，再来一个：

```ruby
require 'erb'

# 准备条件
class User
  attr_accessor :name

  def initialize(name)
    @name = name
  end
end

# 设置实例变量
@user = User.new("Kelby")

# 打包环境
binder = self.send(:binding) # calling a private method

# 模板
template = "Hello, <%= @user.name %>"

# 渲染器(名词)
render = ERB.new(template)
# 渲染(动词)
render.result(binder)

# 输出结果
=> "Helo, Kelby"
```

在这里，重要元素有：渲染器(名词)对应着 render，上下文对应着 binder，渲染(动词)对应着 result.

Rails 里，对应的有：渲染器(名词)对应着 render，上下文对应着 view_context，渲染(动词)对应着 render.
<br>
Rails 里模板文件众多，而且可以嵌套使用，为了方便查找模板内容，引入了 lookup_context 的概念。

### Rails 使用的是 Erubis

Rails 用的是 gem 'erubis'，不是 Ruby 标准库里的 ERB，不过它们原理类似，不再深入。

### 相关

[Binding](http://ruby-doc.org/core-2.1.2/Binding.html)<br>
[ERB](http://www.ruby-doc.org/stdlib-2.1.2/libdoc/erb/rdoc/index.html)<br>
[Erubis](https://github.com/genki/erubis)
