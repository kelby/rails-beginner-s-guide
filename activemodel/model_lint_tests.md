## Lint Tests

前面提到 Active Model 最重要的是 Model. 想要去掉 Active Record，使用其它 ORM，基本的兼容性是要做的。如何检测基本的兼容性做到了？通过 Lint::Tests 可以检测即可。

它主要测试一些"必不可少"的基本方法：

```
test_errors_aref
test_model_naming

test_to_key
test_to_param

test_to_partial_path

test_persisted?
```
