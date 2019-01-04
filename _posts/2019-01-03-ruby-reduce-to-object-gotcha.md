---
layout: post
title: Ruby reduce to object gotcha
date: 2019-01-03 16:55:12 +0000
author: Valerie Dryden
categories: ruby
---

Often with `reduce` we'd be taking in an array of items and condensing them to a given single value.

```ruby
[1,4,6].reduce(:+)
# returns 11
```

Sometimes though we want to be building up a hash or array to return as the output of the reduce.

Say we have an array of numbers and we want to turn them into a hash where the key is the number and the value is the number multiplied by 2, then we might do something like this:

```ruby
[1,4,6].reduce({}) { |out, n| out[n] = n * 2; }
```

We would expect to see the hash `{1=>2, 4=>8, 6=>12}` returned but instead we get:
```ruby
NoMethodError (undefined method '[]=' for 2:Integer).
```
This is because we haven't **explicitly returned** the object we are building up in the reduce. The last expression we evaluated, in this case `n*2`, is passed on to the next iteration of the reduce instead of the object. We then try to set the key `4` on the integer `2` instead of on the object we are trying to build, causing the error that we see.

This behaviour of passing on the last expression evaluated can be very helpful in other situations, like in the ['find the longest word' example](https://ruby-doc.org/core-2.1.0/Enumerable.html#method-i-reduce) from the Ruby docs but is problematic for us when building up an object.

### Solution

We might be tempted to return the object explicitly at the end of the evaluation to fix this:
```ruby
[1,4,6].reduce({}) { |out, n| out[n] = n * 2; out; }
# returns {1=>2, 4=>8, 6=>12}
```
This looks a bit nasty though and a better fix is to use [use each_with_object instead](https://www.rubydoc.info/gems/rubocop/RuboCop/Cop/Style/EachWithObject) which will always implicitly return the object on each iteration.

```ruby
 [1,4,6].each_with_object({}) { |n, out| out[n] = n * 2; }
```
Be mindful that the arguments to the block for `each_with_object` are the reverse of the arguments to the block for `reduce`.
