---
layout: post
title:  "Elixir: Map Puts Shortand"
date:   2018-12-17 07:37:29 +0000
categories: elixir
author: Valerie Dryden
---

In Elixir, we can use the `Map.puts` function to take an existing map and return a new map with either an updated value for a key, or to add a new key / value pair.

```elixir
cats = %{barnaby: 'red', boba: 'white', max: 'black'}
```

If we want to update Max to be a grey cat, we can do:

```elixir
cats = Map.put(cats, :max, 'grey')
```

Or we can use the nice shorthand syntax to do the same thing:
```elixir
cats = %{ cats | max: 'grey' }
```

Note that the shorthand syntax only works when updating an existing value in a map, not to insert a new key / value. To insert a new key / value we still need to use Map.put.
