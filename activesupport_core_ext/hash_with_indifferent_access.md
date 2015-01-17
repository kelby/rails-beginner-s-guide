### Hash With Indifferent Access

处理 Hash 时，使得 :foo 和 "foo" 所代表的意思是一样的。

```ruby
rgb = ActiveSupport::HashWithIndifferentAccess.new

rgb[:black] = '#000000'
rgb[:black]  # => '#000000'
rgb['black'] # => '#000000'

rgb['white'] = '#FFFFFF'
rgb[:white]  # => '#FFFFFF'
rgb['white'] # => '#FFFFFF'
```

```
[],

[]=
regular_writer
store

convert_key
convert_value

deep_stringify_keys
deep_stringify_keys!

deep_symbolize_keys

default

delete

dup

extractable_options?

fetch

has_key?
key?
include?
member?

merge
update
regular_update
merge!

nested_under_indifferent_access
new
new_from_hash_copying_default
reject
replace

reverse_merge
reverse_merge!

select

stringify_keys
stringify_keys!

symbolize_keys

to_hash
to_options!

values_at
with_indifferent_access
```
