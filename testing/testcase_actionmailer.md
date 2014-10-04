# TestCase ActionMailer

除通常的测试方法外，还有

`deliveries` Provides a list of emails that have been delivered. 

使用举例:

```ruby
last_email = ActionMailer::Base.deliveries.last
expect(last_email.to).to eq ['test@example.com']
expect(last_email.subject).to have_content 'Welcome'

email = UserMailer.confirmation(user.id).deliver
assert ActionMailer::Base.deliveries.any?
assert_equal [user.email], email.to
```
