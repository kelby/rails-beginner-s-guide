# ActiveSupport 概览






-----------


## ~~Dependencies~~

包括：ClassCache 和 WatchStack

## ~~DescendantsTracker~~

## ~~FileUpdateChecker~~

## ~~ProxyObject~~

## JSON

## Multibyte

    autoload :Chars
    autoload :Unicode

## OptionMerger

## OrderedHash

```ruby
oh = ActiveSupport::OrderedHash.new
oh[:a] = 1
oh[:b] = 2
oh.keys # => [:a, :b], this order is guaranteed
```

