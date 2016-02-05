## Suppressor

某个类使用 `suppress` 方法后，会对后续 block 里的保存(save)操作进行处理。

这里影响的是同类型的实例对象，无论是新建、更新操作表面上都执行了，但实际上没有执行。

```ruby
user = User.create! token: 'asdf'

User.suppress do
  user.update token: 'ghjkl'
  assert_equal 'asdf', user.reload.token

  user.update! token: 'zxcvbnm'
  assert_equal 'asdf', user.reload.token

  user.token = 'qwerty'
  user.save
  assert_equal 'asdf', user.reload.token

  user.token = 'uiop'
  user.save!
  assert_equal 'asdf', user.reload.token
end
```
