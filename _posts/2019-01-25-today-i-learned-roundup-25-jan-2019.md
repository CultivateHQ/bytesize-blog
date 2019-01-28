---
layout: post
title: Today I learned roundup 25 Jan 2019
date: 2019-01-25 15:07:44 +0000
author: Paul Wilson
categories: today-i-learned
---

Here is a roundup of some interesting links and tiny things posted to Cultivate's `today_i_learned` Slack channel.

* In Ruby, you can use `bundler` [without a Gemfile](https://victorafanasev.info/tech/you-can-use-bundler-without-gemfile)
* Using Elixir `mix xref callers Module.function` [displays all the code calling that function](https://twitter.com/gmile/status/1083744666544156672?s=12)
* Openssl on the command line is good if you want some random stuff. eg `openssl rand -hex 16` outputs a 32 character random hex string. `openssl rand -base64 12` outputs a 12 character random base64 string.
* When using Express.js, if you want to pass some data from one middleware to a subsequent middleware or the handler, you can put it into [response local variables](http://expressjs.com/en/4x/api.html#res.locals)
* When previewing [Jekyll blogs](https://jekyllrb.com) (like this) using `jekyll -serve`, blogs dated in the future will not be displayed unless you use the the `--future` flag
* Some handy [ssh tips](https://twitter.com/b0rk/status/1087936439470444544), including using `ssh-copy-id`
* Voyagers 1 and 2 are still collecting data and [communicating](https://twitter.com/shannonmstirone/status/1088303552370331649) from 13 and 11 billion miles away

