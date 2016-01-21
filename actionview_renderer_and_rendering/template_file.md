#### template 本文件下的内容

```
encode!

inspect

refresh

render

supports_streaming?

type
```

这里的 `render` 会调用方法，进而渲染对应的模板。

```
attr_accessor :locals, :formats, :variants, :virtual_path

attr_reader :source, :identifier, :handler, :original_encoding, :updated_at
```
