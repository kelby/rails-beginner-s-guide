## request, load, autoload 和线程安全

### require(name) → true or false

根据给定的**名字**进行加载。第一次加载，返回 `true`; 之后再加载，返回 `false`.

加载路径只能是**绝对路径或 $LOAD_PATH ($:)**

扩展名为 `.rb` 则尝试加载源文件；其它所有情况 `.so`, `.o`, `.dll` 或`没有扩展名`，都当做 Ruby 的扩展进行加载。上述两种情况，都找不到，则报 LoadError 错误。

```
require "my-library.rb"
require "./my-library"
require "db-driver"
```

链接 http://www.ruby-doc.org/core-2.1.5/Kernel.html#method-i-require

### load(filename, wrap=false) → true

根据给定的**文件名**进行加载**并执行**。

加载路径只能是**绝对路径或 $LOAD_PATH ($:)**

可以在第二个参数指定，执行的环境(命名空间)。

每一次调用，都是相同的"加载并执行"。

链接 http://www.ruby-doc.org/core-2.1.5/Kernel.html#method-i-load

### autoload(module, filename) → nil

自动加载(也叫：延迟加载)，只有在真正用到的时候才会 require.<br>
对原来的"加载"，进行了再次细分。

```
autoload(:MyModule, "/usr/local/lib/modules/my_module.rb")
```

```
module A
end
A.autoload(:B, "b")
A::B.doit            # autoloads "b"
```

链接 http://www.ruby-doc.org/core-2.1.5/Kernel.html#method-i-autoload
<br>
链接 http://www.ruby-doc.org/core-2.1.5/Module.html#method-i-autoload

---

为了性能等原因，Rails 大量使用了 `autoload`.
<br>
为了"线程安全"等原因，Rails 在需要全局变量的地方，又使用了 `eager_autoload`.

百度百科里对"线程安全"的描述：

> 如果你的代码所在的进程中有多个线程在同时运行，而这些线程可能会同时运行这段代码。如果每次运行结果和单线程运行的结果是一样的，而且其他的变量的值也和预期的是一样的，就是线程安全的。
>
或者说:一个类或者程序所提供的接口对于线程来说是原子操作或者多个线程之间的切换不会导致该接口的执行结果存在二义性,也就是说我们不用考虑同步的问题。
>
线程安全问题都是由全局变量及静态变量引起的。
>
若每个线程中对全局变量、静态变量只有读操作，而无写操作，一般来说，这个全局变量是线程安全的；若有多个线程同时执行写操作，一般都需要考虑线程同步，否则的话就可能影响线程安全。

解决"线程安全"问题，有一个很简单的办法，那就是：**只生一个！**

---

[Ruby 多线程](http://www.w3cschool.cc/ruby/ruby-multithreading.html)
<br>
[Thread safety for your Rails](http://m.onkey.org/thread-safety-for-your-rails)
