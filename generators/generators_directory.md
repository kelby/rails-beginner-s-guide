## generators 下的目录、文件

对于 Generator, Rails 是自产(定义)和自销(自销)，还对外提供接口，供我们使用。

#### 全部的文件及目录：

```
# 目录
css/
erb/
js/assets/
rails/
test_unit/

# 文件
actions.rb
active_model.rb
app_base.rb
base.rb
erb.rb
generated_attribute.rb
migration.rb
model_helpers.rb
named_base.rb
resouce_helpers.rb
test_case.rb
test_unit.rb

# 目录
actions/
testing/
```

#### 重要的类：

Base、Named Base 以及 App Base.

#### 自己调用自己：

其中，除 actions/ 和 testing/ 外的其它 5 个目录为调用。

```
css/
erb/
js/assets/
rails/
test_unit/
```

我们在写自己的脚本时，可以参考。

#### 方法

```

A
api_only!
F
fallbacks
H
help, hidden_namespaces, hide_namespace, hide_namespaces
I
invoke
L
levenshtein_distance
N
no_color!
P
print_generators, public_namespaces
S
sorted_groups, subclasses
```
