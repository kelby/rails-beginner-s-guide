## Forbidden Attributes Protection

Strong Parameters 默认是黑名单，params 在(controller层面) permit 后状态会变成 permitted (进入白名单)，只有 permit 的属性才能进入我们系统。为了防止程序员遗漏或图省事，直接传递白名单，我们可以在(model层面)里做校验，如果发现有属性不在白名单内，则报错：

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

此模块用于遗留项目，它的使用，还需要安装 gem 'strong_parameters'

```ruby
# 原理
if attributes.respond_to?(:permitted?) && !attributes.permitted?
  raise ActiveModel::ForbiddenAttributesError
end
```

#### 其它

Rails 的 where 查询也有类似 Mass Assignment 保护。
