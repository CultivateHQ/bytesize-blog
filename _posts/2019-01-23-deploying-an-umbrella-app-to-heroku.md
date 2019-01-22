---
layout: post
title: Deploying an umbrella app to Heroku
date: 2019-01-22 17:51:44 +0000
author: Valerie Dryden and Alan Gardner
categories: elixir phoenix heroku
---

When deploying a Phoenix umbrella app to Heroku and using the [Phoenix static buildpack](https://github.com/gjaldon/heroku-buildpack-phoenix-static) you need to remember to add the relative path to your **phoenix_static_buildpack.config** file.

```bash
phoenix_relative_path="apps/my_app_web"
```

We've also found it useful when deploying to Heroku to specify the Erlang and Elixir versions in the **elixir_buildpack.config** file. This works for non-umbrella apps too.

```bash
always_rebuild=true

# Erlang version
erlang_version=20.1

# Elixir version
elixir_version=1.7.4
```
