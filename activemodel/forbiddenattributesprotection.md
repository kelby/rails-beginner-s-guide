## ForbiddenAttributesProtection

StrongParameters 默认是黑名单，params 在(controller层面) permit 后状态会变成 permitted (进入白名单)，只有 permit 的属性才能进入我们系统。为了防止程序员遗漏或图省事，直接传递白名单，我们可以在(model层面)里做校验，如果发现有属性不在白名单内，则报错：

```ruby
# app/controllers/people_controller.rb
class PeopleController < ActionController::Base
  def create
    # 正常情况
    Person.create(person_params)

    # 遗漏或图省事
    Person.create(params[:person])
  end

  private

    def person_params
      params.require(:person).permit(:name, :age)
    end
end
```

```ruby
# app/models/people.rb
class Person < ActiveRecord::Base
  include ActiveModel::ForbiddenAttributesProtection
end
```

此模块用于遗留项目，这里破例讲解一次。它的使用，还需要安装 gem 'strong_parameters'

```ruby
# 原理
raise ActiveModel::ForbiddenAttributesError if attributes.respond_to?(:permitted?) && !attributes.permitted?
```

## 其它(下列内容为引用)

Rails 4.1.5 开始 where 查询也有类似 Mass Assignment 保护了

前两天将项目中的 Rails 生到了 4.1.5，跑了测试发现有几个查询相关的用例抛出了 ActiveModel::ForbiddenAttributesError 的异常，这个异常大家很熟悉，是防止 Mass Assignment 而在 Rails 4 中引入的一种保护机制。但是常用在创建或更新对象的逻辑中，突然在查询相关逻辑中抛出这样的异常，还是有些莫名奇妙。

根据异常栈查看类 Rails 的源码，发现 4.1.5 where 查询确实进行了更新，新增了 opts = sanitize_forbidden_attributes(opts) 这步，而 sanitize_forbidden_attributes 本身就是 sanitize_for_mass_assignment 方法的别名。
