## Mail Helper

**几个邮件相关的 Helper 方法。**

如：attachments、mailer、message，及不太常用的 block_format、format_paragraph.

| 方法 | 解释 |
| -- | -- |
| message() | 表示邮件对象，即 Mail::Message 实例对象 |
| attachments() | 表示邮件里面的附件 |
| format_paragraph(text, len = 72, indent = 2) | 格式化文字内容。默认行首空两格，每行长度不超过 72 个字符 |
| block_format(text) | 使用上面的 format_paragraph 来处理大段的文字内容 |
| mailer() | YourMailer 的实例对象，类似 YourController 的实例对象 |

另，Mailer 渲染视图和 Controller 渲染视图，它们原理和过程基本上是一致的。所以除了上述 Helper 方法外，Action View 提供的辅助方法在这里也可用。
