**after\_create**

after\_create 从名字上看，record 对象应该已经创建并保存到数据库了。但实际情况却并非总是如此，因为 after\_create 和整个创建、保存是处于同一个生命周期的（共用一个 commit），所以还是有可能保存失败。

坑在哪？如果我们在此回调需要调用了相关 record 对象，并且认为它已经保存到数据库了。例如用户创建后，即刻发送邮件给Ta，即使是异步的，也有可能会报找不到此对象。因为有一点点时间差，record 对象还未真正保存到数据库，但已经被当做保存数据库进而调用了。

**after\_commit on: :create**

使用它，则保证了 record 对象已经真正被保存了，保存到数据库。似乎解决了上述问题。

坑在哪？commit 里不能写和 commit 自己的代码。比如你要 save 一个 record 对象，那么不能再在 after\_commit on: :create 执行和    save 有关的操作，如果违反了，就会陷入死循环里。可以使用 update\_columns 等不会触发 commit 的方法更新自己，或者你 save 其它对象。



