## RSpec configure

**Transactional**

默认 RSpec 每个测试案例之间是没有关联的，在一个测试案例里创建的数据，在别一个测试案例运行之前会被销毁。我们可以通过配置 use_transactional_examples 改变这一行为：

默认(使用)：

```ruby
RSpec.describe Widget do
  it "has none to begin with" do
    expect(Widget.count).to eq 0
  end

  it "has one after adding one" do
    Widget.create
    expect(Widget.count).to eq 1
  end

  it "has none after one was created in a previous example" do
    expect(Widget.count).to eq 0
  end
end
```

明确使用：

```ruby
RSpec.configure do |c|
  c.use_transactional_examples = true
end

RSpec.describe Widget do
  it "has none to begin with" do
    expect(Widget.count).to eq 0
  end

  it "has one after adding one" do
    Widget.create
    expect(Widget.count).to eq 1
  end

  it "has none after one was created in a previous example" do
    expect(Widget.count).to eq 0
  end
end
```

明确不使用：

```ruby
RSpec.configure do |c|
  c.use_transactional_examples = false
  c.order = "defined"
end

RSpec.describe Widget do
  it "has none to begin with" do
    expect(Widget.count).to eq 0
  end

  it "has one after adding one" do
    Widget.create
    expect(Widget.count).to eq 1
  end

  it "has one after one was created in a previous example" do
    expect(Widget.count).to eq 1 # -> 注意这里
  end

  after(:all) { Widget.destroy_all }
end
```

**Render_views**

默认 Controller 里的测试案例是不渲染 View 的，你可以直接使用 render_views 方法进行渲染。也可以通过配置，改变这一默认行为：

```ruby
RSpec.configure do |c|
  c.render_views
end
```

链接 [Transactional examples](https://relishapp.com/rspec/rspec-rails/docs/model-specs/transactional-examples)

