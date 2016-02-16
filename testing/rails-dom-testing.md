## rails-dom-testing

从 ActionView 分离出来的 gem，主要由以下两部分构成：

Dom Assertions

```ruby
assert_dom_equal '<h1>Lingua França</h1>', '<h1>Lingua França</h1>'

assert_dom_not_equal '<h1>Portuguese</h1>', '<h1>Danish</h1>'
```

Selector Assertions

```ruby
# implicitly selects from the document_root_element
css_select '.hello' # => Nokogiri::XML::NodeSet of elements with hello class

# select from a supplied node. assert_select asserts elements exist.
assert_select document_root_element.at('.hello'), '.goodbye'

# elements in CDATA encoded sections can also be selected
assert_select_encoded '#out-of-your-element'

# assert elements within an html email exists
assert_select_email '#you-got-mail'
```