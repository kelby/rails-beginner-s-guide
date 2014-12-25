## rspec-expectations/matchers 示例

**match array**

```ruby
RSpec.describe Widget do
  let!(:widgets) { Array.new(3) { Widget.create } }

  if ::Rails::VERSION::STRING >= '4'
    subject { Widget.all }
  else
    subject { Widget.scoped }
  end

  it "returns all widgets in any order" do
    expect(subject).to match_array(widgets)
  end
end
```
