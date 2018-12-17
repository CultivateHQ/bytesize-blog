---
layout: post
title:  "Ruby and yield_self (next)"
date:   2018-12-17 07:37:29 +0000
categories: ruby
author: Paul Wilson
---

Welcome to Cultivate's _Bytesize Blog_. Here you will find tasty morsels of useful and cool stuff from our Cultivars.

One of the things that can make a series of operations on data more readable in code is using pipelines. In Elixir we will often use the [pipe operator](https://elixir-lang.org/getting-started/enumerables-and-streams.html#the-pipe-operator). In Ruby we will chain operations on collections together. For example:

```ruby
(1..100_000)
  .map {|i| i * 3}
  .select {|i| i.odd?}
  .sum() 
```

Such pipelines could only be consructed using methods existing on the input of each stage. Ruby 2.5 introduced [`Object#yield_self`](http://ruby-doc.org/core-2.5.3/Object.html#method-i-yield_self), which makes the following possible.

```ruby
"\ntmp/test.txt\n"
  .strip()
  .yield_self {|f| File.read(f)}
```

As `yield_self` is not the most intuitive of names, Ruby 2.6 will alias it to [`Object#then`](https://ruby-doc.org/core-2.6.0.preview2/Object.html#method-i-then).

## Acknowledgements

James Bell mentioned `Object#yield_self` and `Object#then` during the [December 2018 ScotRUG](https://scotrug.org/2018/12/09/edinburgh-the-2018-state-of-ruby.html) meetup.