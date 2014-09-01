## Rendering empty responses

Sometimes (and especially once you start dealing with writing web services) you’ll find yourself wanting to return an empty response, with only a status code and (possibly) a few headers set.

You can do this easily enough using the render method:

```ruby
headers['Location'] = person_url(@person)
render :nothing => true, :status => "201 Created"
```

That, however, is unbearably verbose, especially when you find yourself needing to do it in multiple places.

Enter the head method:

```ruby
head :created, :location => person_url(@person)
```

There, isn’t that beautiful?
