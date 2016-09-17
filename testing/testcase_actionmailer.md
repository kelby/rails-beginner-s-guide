## Action Mailer

#### Test Helper

默认 Rails 提供两个 helper 方法用于测试：

|方法|解释|
|----|----|
|assert_emails | 断言已经发送的邮件数|
|assert_no_emails | 断言没有邮件发送出去(可用 assert_emails 0 代替)|
| assert_enqueued_emails | 断言邮件已进队列 |
| assert_no_enqueued_emails | 断言邮件不在队列里 |

assert_emails 和 assert_no_emails 两者本质都是封装 assert_equal.

#### ~~Behavior~~

类方法：

```
determine_default_mailer

mailer_class

tests
```

~~实例方法：~~

```
initialize_test_deliveries

restore_delivery_method
restore_test_deliveries

set_delivery_method
set_expected_mail
```

除通常的测试方法外，还有 `deliveries` 方法获取已经发送的邮件实例。

使用举例:

```ruby
last_email = ActionMailer::Base.deliveries.last

expect(last_email.to).to eq ['test@example.com']
expect(last_email.subject).to have_content 'Welcome'

email = UserMailer.confirmation(user.id).deliver_now

assert ActionMailer::Base.deliveries.any?
assert_equal [user.email], email.to
```
