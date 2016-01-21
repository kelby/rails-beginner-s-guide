### Load Interlock

主要解决 eager_load 和 cache_classes 都 false 的情况下，仍然能很好的处理并发并且不怎么影响性能。

> 这和移除 Rack::Lock 及 config.threadsafe! 有一定关联。
>
简单说：Rack::Lock 多进程的话，它没能解决什么实质问题，还影响性能。
多线程的话，它也不能很好的解决问题，并且还暴露出自身存在的缺陷。 
