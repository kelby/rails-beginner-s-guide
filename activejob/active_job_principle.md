# Active Job 哲学

语法糖 + 适配器 + 各种小工具

延迟任务用得最多的是：Resque、Sidekiq 和 Delayed job.

## 灵活、易用

Active Job 并不在意这些，它是上述后端任务的封装。仅为了易用

既然原理相同，为何 API 互相差异呢？

适配器统一接口。
<br>
选择，写业务相关代码。
<br>
你不用关心如何实现，不用担心被绑在一颗树上。
<br>
表面统一，内在多样化。

## 任务、队列、载体

Job

Queue

Object、Arguments


## 附：几个常见延迟任务实现

### Resque

Resque - Github 出品。

基于 Redis，多队列，延迟执行。

Background jobs can be any Ruby class or module that responds to `perform`.

运用到极限的时候，面临着修bug或受其设计限制。

SQS - 等待时间。

ActiveMessaging - 非 Ruby，我们希望是 Ruby 类或对象。

BackgroundJob - 看起来不错，可以传 Ruby 对象，数据可以自己结构化，还可以设置优先级。
<br>
每次需要加载整个 Rails 环境，花费内存和时间。
<br>
支持队列命名或打标签。

DelayedJob 像 BackgroundJob 有队列还能持久化，设置优先级。只加载一次 Rails 环境，然后一直循环。
<br>
我们采用，并扩展了一下下。
<br>
任务一多，它就抗不住了(备份机制)。新建及查询都很慢。
<br>
对待失败任务、长时间任务的态度。kill -9 了还怎么看？
<br>
但，它还是矮子里的高个。
<br>
不支持队列命名或打标签。

beanstalkd - 快、多队列、优先级、YAML 数据格式，即使大量 push/pop 操作时间几乎没有差别。
<br>
持久化还处理实验阶段。
<br>
不能查看失败的任务、不能查看等待中的任务、管理队列（移除某些任务等）
<br>
支持队列命名或打标签。

数据存在 Redis，也有 Web 窗口方便查看。

### Sidekiq

Sidekiq - 快。

多线程。
<br>
不依赖 Rails，却能轻松集成。
<br>
兼容 Resque (数据格式是一样的)，甚至你可以使用 Resque 的 Redis 进队列，使用 Sidekiq 出队列、执行。
<br>
快，不服看对比表。
<br>
也有 Web 窗口方便查看

### Delayed job

Delayed job - Shopify 出品。


跑起来稳定、跑起来快、用起来简单。
