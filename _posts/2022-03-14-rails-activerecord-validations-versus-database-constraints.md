---
layout: blog
title:  "Should you use ActiveRecord validations or database constraints?"
date:   2022-03-14
categories: rails
slug: rails-activerecord-database-constraints
---

# Should you use ActiveRecord validations or database constraints?

When I was starting learning Ruby on Rails one of the things I enjoyed the most about the framework was the fact that it was super quick and easy to get a form nicely done, with good error messages and all. All I had to do is to use some methods on my ActiveRecord model and voilà, it would just work!
Because AR validations were so powerful I basically ignored database constraints. As I learned more about Ruby on Rails and web development in general I learned when and where to use the different tools available to me.

In this post I will try to explain more about what are the differences between both things to hopefully help you understand when to use one, the other or both.

## ActiveRecord Validations

ActiveRecord validations provide meaningful error messages rather than cryptic database errors. For example, how do you think a customer would feel if when they tried to submit a form and received an error message like this?

<img src="/assets/images/form-example.png" alt="Form Example" style="max-width: 720px;" />

Not very nice right? By adding validations to your model you can prevent wrong data from being written to your database and at the same time improve the experience of your customer.

Here you can also be more expressive, for example:

```ruby
class User < ApplicationRecord
  validates :last_name, presence: true, if: :first_name
end
```

### Validations on specific lifecycle parts

Sometimes you just want to make sure that a piece of information is required when an object is first created.

```ruby
class Foo < ApplicationRecord
  validates :email, uniqueness: true, on: :create
end
```

### Business logic validations

You can also do validations that are specific to your application. For example:

```ruby
class Invoice < ApplicationRecord
  validate :expiration_date_cannot_be_in_the_past,
    :discount_cannot_be_greater_than_total_value

  def expiration_date_cannot_be_in_the_past
    if expiration_date.present? && expiration_date < Date.today
      errors.add(:expiration_date, "can't be in the past")
    end
  end

  def discount_cannot_be_greater_than_total_value
    if discount > total_value
      errors.add(:discount, "can't be greater than total value")
    end
  end
end
```

## Database Constraints

So now you must be thinking: Why should I care about database constraints? ActiveRecord models will make sure the application saves only correct data plus I get good error messages and customizations.
Well... Not really. For a few reasons:

### Data consistency

Depending on which ActiveRecord methods you are using to write to your database, your model validations might be skipped entirely and cause unwanted data to be saved. The database on the other hand is _very_ good at making sure that any given database transaction must change affected data only in allowed ways.

### Race Conditions

Imagine the following scenario:

You have a table `users`, that has an `email` field. And you wrote the validation correctly as so:

```ruby
class User < ApplicationRecord
  validates :email, uniqueness: true
end
```

Now imagine that you have two different requests that are trying to save the same email to the database and those requests happened in two different processes at the _exact same time_. This is what will happen:

1 - Both requests will come through and try to write to the database concurrently;

2 - ActiveRecord will run the email validation for both requests, however because there isn't an email already on the database the validation will pass;

3 - The request will finish and save both records with the same email to the db.

That is, very simply put, what a race condition is. You can learn more about that [here](https://karolgalanciak.com/blog/2020/06/07/race-conditions-on-rails/).

As you could see, ActiveRecord alone won't prevent bad data from being written if there is a race condition.
If you add a database constraint, because transactions to the same table will happen one at a time, the second time you try to write the duplicate email to the database, it will throw an error message and prevent it from happening.

### External clients

This is not very common in most Ruby on Rails applications, but sometimes there are other clients writing to the database other than ActiveRecord models. This is yet another reason why you should always use database constraints to make sure the data is consistent.

## When to skip database constraints

If you are prototyping, sometimes restricting the database with constraints can get in your way. For the sake of speed I find it okay to skip db constraints in this case.

## Conclusion

It was never about one versus the other. Both options are there to serve different purposes and if you are making something people are going to use you should use both.
