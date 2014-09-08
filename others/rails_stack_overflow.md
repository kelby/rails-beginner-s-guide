# Rails Stack OverFlow

## How can I rename a database column in a Rails migration?

I wrongly named a column hased_password instead of hashed_password.

答：

```ruby
def change
  rename_column :table_name, :old_column, :new_column
end
  
# 或
  
def self.up
  rename_column :table_name, :old_column, :new_column
end

def self.down
  # rename back if you need or do something else or do nothing
end
```

## How do I get the current absolute URL in Ruby on Rails?

How can I get the current absolute URL in my Ruby on Rails view?

The `request.request_uri` only returns the relative URL.

答：

**For Rails 3.2 or Rails 4** You should use `request.original_url` to get the current URL. More detail.

## A concise explanation of nil v. empty v. blank in Ruby on Rails

I find myself repeatedly looking for a clear definition of the differences of nil?, blank?, and empty? in Ruby on Rails. Here's the closest I've come:

blank? objects are false, empty, or a whitespace string. For example, "", " ", nil, [], and {} are blank.

nil? objects are instances of NilClass.

empty? objects are class-specific, and the definition varies from class to class. A string is empty if it has no characters, and an array is empty if it contains no items.

Is there anything missing, or a tighter comparison that can be made?

答：

Here I made this useful table with all the cases

|  | nil? | true( if condition) | empty?(string, array or hash) | blank? | present?(!blank?) |
| -- | -- | -- | -- | -- | -- |
| nil | √ | X |   | √ | X |
| false | X | X |   | √ | X |
| true | X | √ |   | X | √ |
| "" | X | √ | √ | √ | X |
| " " | X | √ | X | √ | X |
| [] | X | √ | √ | √ | X |
| [nil] | X | √ | X | X | √ |
| {}| X | √ | √ | √ | X |
| {temp: nil} | X | √ | X | X | √ |
| 0 | X | √ |   | X | √ |
| 5 | X | √ |   | X | √ |

It should be noted that blank? and present? are Rails-only.





