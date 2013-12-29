---
layout: post
title: Good Bye Jacaranda and welcome ActiveRecord::Enum
meta_description: How I created my first gem and now I deprecated that gem since rails added the gem feature
meta_keywords: rails, jacaranda, active record, AR, enum
category: Rails
tags: [Rails, Open Source]
---

A time ago I want to create a Gem to learn how this works. But in the Ruby/Rails world we live in a mature community when we have a lot of libs to do our job, so  I give up for a moment, until I have a problem that more people have experiencied too.

So in a moment I find a pattern, that I and my friends do. When I have a model, with validation of inclusion like this:

{% highlight ruby %}
class Post < ActiveRecord::Base
  validates :status, inclusion: { in: %w{published unpublished draft} }
end
{% endhighlight %}

I normally need predicate methods like `@post.published?` and scopes like `Post.published`. So by this idea and some work born [Jacaranda](https://github.com/maurogeorge/jacaranda) my first gem.

But now in the rails 4.1.0, I testing using 4.1.0.beta1 since no release yet. We have the [ActiveRecord::Enum ](http://edgeapi.rubyonrails.org/classes/ActiveRecord/Enum.html) that can do the same thing we do with Jacaranda.

## Meeting the ActiveRecord::Enum

Lets create the same behavior of Jacaranda now using the `ActiveRecord::Enum`.

### Migration

We started with a migration like this.

{% highlight ruby %}
class CreatePosts < ActiveRecord::Migration
  def change
    create_table :posts do |t|
      t.string :title
      t.integer :status, default: 0

      t.timestamps
    end
  end
end
{% endhighlight %}

The important part is the column is a integer and we set the default value as 0.

### Model

In our model we simply use the macro `enum`.

{% highlight ruby %}
class Post < ActiveRecord::Base

  enum status: %w(published unpublished draft)
end
{% endhighlight %}

### In the Wild

#### Predicate methods

We start with a post with status is `published`. Remember you need to pass to the model the string and not integer, a `ArgumentError` is raised if you try pass the integer value.

{% highlight ruby %}
Post.create(title: 'Post title', status: 'published')
post = Post.first
post.published? # true
post.draft? # false
{% endhighlight %}

#### Scopes

We can use the scopes with the same name as declareted on our model.

{% highlight ruby %}
Post.published # SELECT "posts".* FROM "posts"  WHERE "posts"."status" = 0
{% endhighlight %}

Search is made using the integer value on the database.

#### The bang!

The bang are not in Jacaranda, but are cool =)

You can use the bang method to change the status of post.

{% highlight ruby %}
post.draft!
post.status # "draft"
{% endhighlight %}

Now our post have the status as a draft.

One cool thing about the `ActiveRecord::Enum` is that all values are stored like a integer and accessed as a string.

#### Using explicit mapping

Add status without key can be annoying since if one new status is added in a wrong position all objects have values changed.

{% highlight ruby %}
class Post < ActiveRecord::Base

  enum status: %w(reviewed published unpublished draft)
end
{% endhighlight %}

We add reviewed before the published, so now all published posts have the status as reviewed.

To avoid this problem, we can use the explicit mapping like.

{% highlight ruby %}
class Post < ActiveRecord::Base

  enum status: { published: 0, unpublished: 1, draft: 2, reviwed: 3 }
end
{% endhighlight %}

This way we dont care about the order but with the value.

Of course good specs ensure we are adding a thing that dont break others.


## Conclusion

Jacaranda is now deprecated, since we can use the `ActiveRecord::Enum`.

I learned a lot in the way using services like:

- [RubyGems](http://rubygems.org)
- [Travis CI](http://travis-ci.org)
- [Coveralls](http://coveralls.io)
- [Code Climate](https://codeclimate.com)
- [Gemnasium](https://gemnasium.com)

I used the [Semantic Versioning](http://semver.org). I learned how hard is the work to maintain a Gem. I started [work in others open source projects](https://github.com/maurogeorge). And is so enjoyable know that I was in the right direction =)