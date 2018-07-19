---
layout: blog
title:  "How to better organize your graphql queries on Ruby on Rails"
date:   2018-07-18
categories: development rails ruby graphql
---

# How to better organize your graphql queries on Ruby on Rails

## Intro

I'm getting started learning about GraphQL and it's concepts, because we decided at work to migrate some REST endpoints we have. It seems really straightforward and simple, but I'm still just getting started studying it and thought about sharing a few ideias for those who might end up having the same problems that I had.

I was migrating some REST endpoints to GraphQL at work, using [graphql-ruby](https://github.com/rmosolgo/graphql-ruby) gem.

Although the documentation is from an old version, everything was going really well. The gem makes it easy to create the Types and Queries.
After you setup the gem and run the installer command, `app/graphql/query_type.rb` is generated so you can create your queries.

## My first steps

I'm not going to get into the concepts of GraphQL and also how to define Types. Later I will also write about how to better organize your Types, [let me know](https://twitter.com/brunossilveira) if you're interested.

My first query looked like the following:

```ruby
class Types::QueryType < Types::BaseObject
  field :products, [Types::ProductType], null: false, description: "Retrieves list of products"

  def products
    Product.all
  end
end
```

Simple right? Just add the `field` and the method with the database query.

Then I added the finder query:

```ruby
class Types::QueryType < Types::BaseObject
  field :products, [Types::ProductType], null: false, description: "Retrieves list of products"
  field :product, Types::ProductType, null: false, description: "Retrieves one product based on ID" do
    argument :id, ID, required: true
  end

  def products
    Product.all
  end

  def product(id:)
    Product.find(id)
  end
end
```

## The problem

I quickly realized that by the end of it, after I finished adding all queries needed, this file would become giant.
So I thought for a while how I could make it better organized. I searched the documentation but could not find anything helpful for this problem. I even found [this article](https://m.alphasights.com/graphql-ruby-clean-up-your-query-type-d7ab05a47084) that solved my problem but when it was written the gem was in a different version. I tried to read the code and port the solution to the new version without success.

And then I realized... It's just Ruby 🤦.

The inspiration came from the [this documentation section](http://graphql-ruby.org/type_definitions/resolvers.html). I could just create a module, use `self.included` to add the `field`, and the method and voila.
And now behold my solution:

```ruby
module Products
  module Queries
    def self.included(child_class)
      field :products, [Types::ProductType], null: false, description: "Retrieves list of products"
      field :product, Types::ProductType, null: false, description: "Retrieves one product based on ID" do
        argument :id, ID, required: true
      end
    end

    def products
      Product.all
    end

    def product(id:)
      Product.find(id)
    end
  end
end
```

And the new `QueryType` file:

```ruby
class Types::QueryType < Types::BaseObject
  include ProductsQueries
end
```

The file tree looks like this:

```
.
├── graphql
│   ├── products
│   │   └── queries.rb
│   ├── types
│   │   └── ...
│   └── mutations
│       └── ...
└── schema.rb
```

That's all! Simple right?

Of course that it doesn't look that much better with just a few queries but think about the application you are currently working on and how many endpoints it has. Think about how `QueryType` would look like if you added all queries into a single file. Probably not the best.

## A few considerations

I noticed that this messes up a bit with the namespaces of the gem, but apart from that and as long as you use the full name of it's classes, everything will work as expected.

## That's all

I would love to hear if you have suggestions or a better solution. You can always find me on [twitter](https://twitter.com/brunossilveira) if you have any ideias/suggestions you want to share.
