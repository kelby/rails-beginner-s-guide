## Mail Helper

| 方法 | 解释 |
| -- | -- |
| message() | 表示邮件对象，即 Mail::Message 实例对象 |
| attachments() | 表示邮件里面的附件 |
|format_paragraph(text, len = 72, indent = 2)|处理一段文本消息，行首空两格，每行长度不超过 72 个字符|
|block_format(text)|使用 format_paragraph 处理大段的文本|
| mailer() | 表示邮件所在的 Mailer 对象，类似 Controller 对象 |
