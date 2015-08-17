## 其它

在外面，只要直接或间接 add_dependency "railties"，即可使用，因为它属于这个(而不是 rails)

**- 继承于 NamedBase 或 Base**

这是最常见的用法。

**- 直接使用某个模块**

当然你也可以更进一步，继承于其它模块，但因为没有 API 和文档，使用门槛提高不少。

- 目的很明确
- 非常确定这个模块干的是什么

举例：

Erb::Generators::ControllerGenerator

```ruby
require 'rails/generators/erb/controller/controller_generator'

module Haml
  module Generators
    class ControllerGenerator < Erb::Generators::ControllerGenerator
      source_root File.expand_path("../templates", __FILE__)

      protected

      def handler
        :haml
      end
    end
  end
end
```

#### ~~Generated Attribute~~

生成过程中的一些属性判断。

#### ~~Erb Generators Base~~

继承于 Rails::Generators::NamedBase

```
formats
format
handler

filename_with_extensions
```
