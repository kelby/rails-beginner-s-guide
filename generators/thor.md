## Thor

Thor 和 rake 类似，提供了功能强大的命令行接口。

因为 Rails 的 generator 实际上是封装了 Thor ，所以还有 [Thor Actions](http://rdoc.info/github/erikhuda/thor/master/Thor/Actions.html)

#### Actions 实例方法：

`action`<br>
封装一个实例对象，并调用它。

`append_to_file`<br>
向文件里追加文本内容。

```
append_to_file 'config/environments/test.rb', 'config.gem "rspec"'
```

`apply`<br>
加载并执行文件。

```
apply "recipes/jquery.rb"
```

`chmod`<br>
更改文件或目录的权限。

```
chmod "script/server", 0755
```

`comment_lines`<br>
注释指定文件里面符合条件的行。

```
comment_lines 'config/initializers/session_store.rb', /cookie_store/
```

`copy_file`<br>
复制文件。(默认源文件放在 source_root 下)

```
copy_file "README", "doc/README"
```

`create_file & add_file`<br>
创建新文件。

```
create_file "config/apache.conf", "your apache config"
```

`create_link(destination, *args, &block) (also: #add_link)`<br>
创建一个链接文件。

```
create_link "config/apache.conf", "/etc/apache.conf"
```

`destination_root`<br>
返回目标目录。

`destination_root=(root)`<br>
设置目标目录。

`directory`<br>
按规则复制整个目录。(并不是直接复制，注意转换规则)

```
# 源文件、目录如下：
doc/
  components/.empty_directory
  README
  rdoc.rb.tt
  %app_name%.rb

# 复制整个目录：
directory "doc"

# 得到目标文件、目录：
doc/
  components/
  README
  rdoc.rb
  blog.rb
```

`empty_directory`<br>
创建一个空目录。

```
empty_directory "doc"
```

`find_in_source_paths`<br>
在源目录里查找文件。

`get`<br>
获取内容并放到文件里。

```
get "http://gist.github.com/103208", "doc/README"
```

`gsub_file`<br>
按正则替换文件内容。

```
gsub_file 'README', /rake/, :green do |match|
  match << " no more. Use thor!"
end
```

`in_root`<br>
到根目录执行 block 里的代码。

`inject_into_class`<br>
将内容插入到指定的文件，指定的 class 后面。(比 insert_into_file 更精确)

```
inject_into_class "app/controllers/application_controller.rb", ApplicationController do
  "  filter_parameter :password\n"
end
```

`insert_into_file & inject_into_file`<br>
将内容插入到指定的文件内。(如：添加 js 文件后，一般会在 application.js 里插入 require 等代码)

```
insert_into_file "config/environment.rb",
                 "config.gem :thor",
                 :after => "Rails::Initializer.run do |config|\n"
```

`inside`<br>
在根目录或指定的目录里执行 block 里的代码。

`link_file`<br>
链接文件。

```ruby
link_file "README", "doc/README"
```

`prepend_to_file & prepend_file`<br>
在文件开头处追加文本内容。

```ruby
prepend_to_file 'config/environments/test.rb', 'config.gem "rspec"'
```

`relative_to_original_destination_root`<br>
以目标为根目录，获取相对路径。

`remove_file & remove_dir`<br>
移除文件。

```
remove_file 'README'
```

`run`<br>
执行某条命令，并返回执行结果。

```
inside('vendor') do
  run('ln -s ~/edge rails')
end
```

`run_ruby_script`<br>
运行 Ruby 脚本。(照顾 WIN32 的用户)

`source_paths`<br>
得到源路径。(方便后续操作)

`template`<br>
复制示例模板文件，生成新的 ERB 文件。

`thor`<br>
运行 thor 命令。

```
thor :list, :all => true, :substring => 'rails'
```

`uncomment_lines`<br>
去掉指定文件里符合条件的行的注释。

```
uncomment_lines 'config/initializers/session_store.rb', /active_record/
```

#### Actions 类方法：

`add_runtime_options!`<br>
添加了 force、pretend、quiet、skip 这几个 class_option.

`source_paths`<br>
存储并返回定义这个类所在的位置

`source_paths_for_search`<br>
获取 source_paths、source_root(有的话)、source_paths(父亲的)

`source_root`<br>
存储并返回定义这个类所在的位置。(Base 已经覆盖此方法)

#### 其它

`argument`<br>
给我们的"命令行"添加参数，并有 attr_accessor<br>
(注意：这里的参数区别于类或方法里的参数、可选参数)<br>
如命令 rails g mailer NAME [method method] [options] 这里的 [method method] 这部分。

`ClassName#public_instance_method`<br>
一般的，类名会被当做 namespace，而实例方法会被当做 task，也就是：

```
class_name:public_instace_method
```

(实例方法的参数仍被当做参数对待)

`desc`<br>
对 task 的描述。

`method_option 和 method_options`<br>
使用 task 时传递的可选参数。

`invoke`<br>
调用方法(为什么不直接调用？)

`Thor::Group`<br>
继承于它的话，下面的实例方法会被当成"组"，会按照定义顺序执行。

`class_option 和 class_options`<br>
使用 task 时传递的可选参数。(配合 Thor::Group 很好)

`namespace`<br>
定义 namespace (不使用默认以 ClassName 生成的)

`ClassName.start`<br>
可以把 task 封装成可执行命令，不必用 thor 命令。这是启动命令，通常放在最后。

`option 和 options`<br>
可选参数(如：thor my_cli:hello --from "Carl Lerche" Kelby 这里的 --from)

`public_task & public_command`<br>
定义一个实例方法，方法内容就是其父类的私有方法。

```ruby
public_command :foo
```

链接

[What Is Thor](http://whatisthor.com/)<br>
[Thor Wiki](https://github.com/erikhuda/thor/wiki)
