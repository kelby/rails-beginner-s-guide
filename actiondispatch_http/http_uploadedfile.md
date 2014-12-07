## Uploaded File

```
close
eof?
open
path
read
rewind
size
```

使用举例

```ruby
Factory.define :image do |o|  
  file = File.new(Rails.root + 'spec/fixtures/images/rails.png')  
  file.rewind  
  o.file ActionDispatch::Http::UploadedFile.new(:tempfile => file, :filename => File.basename(file))  
end  
  
@image = Factory(:image)  
```

参考

[RSpec & Rails: Simulating File Uploads](http://blog.hulihanapplications.com/browse/view/64-rspec-rails-simulating-file-uploads)
