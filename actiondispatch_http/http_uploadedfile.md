## Uploaded File

**上传文件时用到**，默认文件放到 :tempfile 里，相关会用到它来处理。

```
close
eof?
open
path
read
rewind
size
```

在 Http Parameters 和 Metal Parameters 里有用到它，对应 params 里有 :tempfile 和 params.permit(:x) 里包含文件对象时处理。

使用举例：

```ruby
Factory.define :image do |o|  
  file = File.new(Rails.root + 'spec/fixtures/images/rails.png')  
  file.rewind  
  o.file ActionDispatch::Http::UploadedFile.new(:tempfile => file,
                                                :filename => File.basename(file))  
end  
  
@image = Factory(:image)  
```

参考

[RSpec & Rails: Simulating File Uploads](http://blog.hulihanapplications.com/browse/view/64-rspec-rails-simulating-file-uploads)
