## Serialization

有 ActiveRecord::Serializers::JSON 提供：

`serializable_hash(options = nil)` 方法。
<br>
效果和 as_json 差不多。实际起作用的是 ActiveModel::Serialization 里的同名方法。

有 ActiveRecord::Serializers::XmlSerializer 提供：

`to_xml` 方法。实现方式：include and extend ActiveModel::Serializers::Xml，并且里面有同名方法。

> Note: 包括了 XmlSerializer.

> Rails 5 里 ActiveRecord::Serialization::XmlSerializer 已废除。
