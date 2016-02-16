## rails-dom-testing

从 ActionView 分离出来的 gem，主要负责 DomAssertions 和 SelectorAssertions.

Dom Assertions

```ruby
assert_dom_equal '<h1>Lingua França</h1>', '<h1>Lingua França</h1>'

assert_dom_not_equal '<h1>Portuguese</h1>', '<h1>Danish</h1>'
```

```
```