# Generators

把常见的手动操作，用命令来实现。

执行 generator 时，会按先后顺序执行每一个公共方法。

**文件、目录、参数、操作！**

如果，你记不住这么多命令，不要紧，按按照手动操作，然后自己实现也是可以的。

```
fallbacks
help, hidden_namespaces
invoke
no_color!
print_generators, public_namespaces
sorted_groups, subclasses

levenshtein_distance

hide_namespaces
```

该目录下，还有 Rails 自带的 generator 的源代码，它们可供参考。

## 其它

在外面，只要直接或间接 add_dependency "railties"，即可使用，因为它属于这个(而不是 rails)

最常见的用法是：继承于 NamedBase 或 Base. 

当然你也可以更进一步，继承于其它模块，但因为没有 API 和文档，使用门槛提高不少。

- 目的很明确
- 非常确定这个模块干的是什么

直接使用某个模块，举例：

Erb::Generators::ControllerGenerator

ActiveRecord::Generators::Base

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
