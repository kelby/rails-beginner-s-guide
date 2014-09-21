# ActiveSupport - Class - 类属性 - 运用 define_singleton_method

## 作用

defines one or more class-level attributes whose value is inheritable and overwritable by subclasses and instances

## 场景

一个父类 通常对应 多个子类

子类默认继承于父类，允许子类改变自己，但不允许它改变父类。(邮件配置就很明显)

## 来源

[The Rails 3 Way Highlights: ActiveSupport's Class.class_attribute](http://blog.obiefernandez.com/content/2010/04/tr3w-highlights-activesupport-class-class-attribute.html)

## 与再有方法有何异同

[`cattr_accessor` vs `class_attribute`](https://coderwall.com/p/xhcmbg)

```ruby
cattr_reader(*syms)
Alias for: mattr_reader

cattr_writer(*syms)
Alias for: mattr_writer

cattr_accessor(*syms, &blk)
Alias for: mattr_accessor

def mattr_accessor(*syms, &blk)
  mattr_reader(*syms, &blk)
  mattr_writer(*syms, &blk)
end


      class_eval("        @@#{sym} = nil unless defined? @@#{sym}

        def self.#{sym}
          @@#{sym}
        end

      class_eval("          def #{sym}
            @@#{sym}
          end

------

      class_eval("        @@#{sym} = nil unless defined? @@#{sym}

        def self.#{sym}=(obj)
          @@#{sym} = obj
        end

        class_eval("          def #{sym}=(obj)
            @@#{sym} = obj
          end



```

```ruby
    define_singleton_method(name) { nil }
    define_singleton_method("#{name}?") { !!public_send(name) } if instance_predicate

    define_singleton_method("#{name}=") do |val|
      singleton_class.class_eval do
        define_method(name) { val }
      end

      if singleton_class?
        class_eval do
          define_method(name) do
            if instance_variable_defined? ivar
              instance_variable_get ivar
            else
              singleton_class.send name
            end
          end
        end
      end
      val
    end



      define_method(name) do
        if instance_variable_defined?(ivar)
          instance_variable_get ivar
        else
          self.class.public_send name
        end
      end
      define_method("#{name}?") { !!public_send(name) } if instance_predicate
```

