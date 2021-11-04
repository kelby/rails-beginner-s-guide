# Rails 6 开发进阶

本书主要包括两部分：Rails 源码剖析和 Rails 使用指南。Rails 是一个 Web 开发框架，也是一个工具。"工欲善其事必先利其器"，想要更好的使用 Rails 这个工具，清楚其背后的魔法，阅读源代码是必备功课。

本书尽量做到系统、全面，从源码出发，会讲解到原理。本书大部分内容为原创，少数内容为整理网上资料，鉴于参考资料太多，恕我不能一一列举来源。

#### 本书适合什么样的读者？

* 想要更好的使用 Rails
* 准备阅读 Rails 的源代码
* 想知道 Rails 的整体架构
* 想清楚 Rails 背后的魔法

#### 本书包含了哪些内容？

* 一些文档里找不到的方法
* 每个模块的关键所在
* 源码阅读路线图

#### 本书不讲哪些内容？

* 安装、部署
* 奇技淫巧
* Ruby 语法、Rails 入门
* 和具体业务相关的问题或功能
* 非常具体、底层的代码实现

#### 本书目的

* 让使用其它语言、框架的技术人员，能看懂是什么
* 让已经入门的 Rails 使用人员，知道更多原理，能看懂怎么用

#### 联系我

邮箱：leekelby@gmail.com

#### Rails 源代码划分

* actioncable

* actionmailer

* action\_pack

  * abstract\_controller
  * action\_controller
  * action\_dispatch

* actionview

* activejob

* activemodel

* activerecord

* activestorage

* activesupport
* railties

**基础库：**

activesupport

railties

**MVC：**

activemodel

activerecord

actionview

action\_pack

**特殊业务：**

actioncable

actionmailer

activejob

activestorage

Rails 优势：

* Ruby 一切皆对象，给程序员很大的自由

* Ruby 的快乐、高效编程，以及黑魔法

* 代码和命令行工具

* 最佳实践

* 理念，约定优于配置、不要重复你自己



