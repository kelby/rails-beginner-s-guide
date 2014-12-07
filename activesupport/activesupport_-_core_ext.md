# Active Support - Core Ext

简单的按近义、反义归类。

## Array

```
deep_dup
extract_options!

from
to

in_groups
in_groups_of

second
third
fourth
fifth
forty_two

split

to_default_s
to_formatted_s
to_param
to_query
to_s
to_sentence
to_xml

wrap
```

## Benchmark

```
ms
```

## BigDecimal

```
to_s
_original_to_s
to_formatted_s

duplicable?
encode_with
```

## Class

```
class_attribute
subclasses
superclass_delegating_accessor
```

## Date

```
<=>
compare_without_coercion
compare_with_coercion

acts_like_date?
advance
ago

at_beginning_of_day
beginning_of_day
midnight
at_midnight

at_end_of_day
end_of_day

at_midday
at_noon
middle_of_day

at_middle_of_day

beginning_of_week
beginning_of_week=
change
current

find_beginning_of_week!

in
since

inspect
default_inspect
readable_inspect

midday
noon

to_default_s
to_formatted_s
to_s

to_time
tomorrow
xmlschema
yesterday
```

## Date Time

```
<=>

acts_like_date?
acts_like_time?

at_beginning_of_day
beginning_of_day

at_beginning_of_hour
beginning_of_hour

at_beginning_of_minute
beginning_of_minute

at_end_of_day
end_of_day

at_end_of_hour
end_of_hour

at_end_of_minute
end_of_minute

at_midday
middle_of_day
noon
at_middle_of_day

advance, ago,
at_midnight, at_noon
change, civil_from_format, current
formatted_offset

in
since

inspect
default_inspect

midday, midnight

readable_inspect
seconds_since_midnight, seconds_until_end_of_day

to_f
to_i

to_s
to_default_s
to_formatted_s

nsec
usec

utc
getutc

utc?
utc_offset
```

## Enumerable

```
exclude?
index_by
many?
sum
```

## File

```
atomic_write
```

## Hash

```
assert_valid_keys
compact, compact!
deep_dup, deep_merge, deep_merge!, deep_stringify_keys, deep_stringify_keys!, deep_symbolize_keys, deep_symbolize_keys!

deep_transform_keys
deep_transform_keys!

except
except!

extract!, extractable_options?

from_trusted_xml, from_xml

nested_under_indifferent_access
with_indifferent_access

reverse_merge
reverse_update
reverse_merge!

slice
slice!

stringify_keys
stringify_keys!

symbolize_keys
symbolize_keys!
to_options
to_options!

to_param
to_query

to_xml

transform_keys
transform_keys!
```

## Integer

```
month, months, multiple_of?
ordinal, ordinalize
year, years
```

## Kernel

```
breakpoint
capture, class_eval, concern
debugger
enable_warnings
quietly
silence, silence_stream, silence_warnings, suppress
with_warnings
```

## LoadError

```
is_missing?
path
```

## Marshal

```
load_with_autoloading
```

## Module

```
alias_attribute
alias_method_chain

anonymous?,

attr_internal
attr_internal_accessor

attr_internal_reader
attr_internal_writer

cattr_accessor
mattr_accessor

cattr_reader
mattr_reader

cattr_writer
mattr_writer

delegate

deprecate

foo

parent
parents
parent_name

qualified_const_defined?
qualified_const_get
qualified_const_set

redefine_method

remove_possible_method
```

## Name Error

```
missing_name, missing_name?
```

## Numeric

```
ago
byte, bytes
day, days, duplicable?
exabyte, exabytes
fortnight, fortnights, from_now
gigabyte, gigabytes
hour, hours, html_safe?
in_milliseconds
kilobyte, kilobytes
megabyte, megabytes, minute, minutes
petabyte, petabytes
second, seconds, since
terabyte, terabytes, to_formatted_s
until
week, weeks
```

## Object

```
acts_like?
blank?
create_fixtures
deep_dup, duplicable?
html_safe?
in?, instance_values, instance_variable_names
presence, presence_in, present?
to_json_with_active_support_encoder, to_param, to_query, try, try!
unescape
with_options
```

### Has hWith Indifferent Access

```
[],

[]=
regular_writer
store

convert_key
convert_value

deep_stringify_keys, deep_stringify_keys!

deep_symbolize_keys, default, delete, dup
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

nested_under_indifferent_access, new, new_from_hash_copying_default
reject, replace, reverse_merge, reverse_merge!
select, , stringify_keys, stringify_keys!, symbolize_keys
to_hash, to_options!

values_at
with_indifferent_access
```

## Range

```
include_with_range?
overlaps?
to_default_s, to_formatted_s, to_s
```

## Secure Random

```
uuid_from_hash, uuid_v3, uuid_v5
```

## String

```
acts_like_string?, at
blank?
camelcase, camelize, classify, constantize
dasherize, deconstantize, demodulize
exclude?
first, foreign_key, from
humanize
in_time_zone, indent, indent!, inquiry, is_utf8?
last
mb_chars
parameterize, pluralize
remove, remove!
safe_constantize, singularize, squish, squish!, strip_heredoc
tableize, titlecase, titleize, to, to_date, to_datetime, to_time, truncate
underscore
```

## Thread

```
freeze
thread_variable?, thread_variable_get, thread_variable_set, thread_variables
```

## Time

```
-
minus_with_coercion
minus_without_coercion
minus_without_duration

<=>
compare_with_coercion
compare_without_coercion

===,

_dump
_dump_without_zone

_load
acts_like_time?, advance, ago, all_day,

at_beginning_of_day
beginning_of_day
at_midnight
midnight

at_beginning_of_hour
beginning_of_hour

at_beginning_of_minute
beginning_of_minute

at_end_of_day
end_of_day

at_end_of_hour
end_of_hour

at_end_of_minute
end_of_minute

at_midday
midday

at_middle_of_day
middle_of_day

at_noon
noon

at_with_coercion
change, , current
days_in_month

eql?
eql_with_coercion
eql_without_coercion

find_zone, find_zone!, formatted_offset

in
since

seconds_since_midnight, seconds_until_end_of_day

to_default_s
to_formatted_s
to_s

use_zone
zone
zone=
```
