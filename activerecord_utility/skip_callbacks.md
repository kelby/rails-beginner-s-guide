### 跳过回调

主要有 3 种方式：

#### update_columns

类似 update_columns，直接使用不会触发回调的方法进行操作。

#### skip_callback

跳过某个回调。

#### x_without_callbacks

以 object.send(:x_without_callbacks) 跳过某个系列的回调。

参考

[How can I avoid running ActiveRecord callbacks?](http://stackoverflow.com/questions/632742/how-can-i-avoid-running-activerecord-callbacks)<br>
[How to skip ActiveRecord callbacks?](http://stackoverflow.com/questions/1342761/how-to-skip-activerecord-callbacks)
