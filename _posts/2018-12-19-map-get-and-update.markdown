---
layout: post
title:  "Elixir: Map 'get and update'"
date:   2018-12-17 07:37:29 +0000
categories: elixir
author: Valerie Dryden
---

The other day I found myself trying to get the count of how many times an item appeared in a list in Elixir. There's no inbuilt way to do this that I've seen so far (please do comment if you know of one!) and there's various ways to achieve it but I chose to do it like so:

```elixir
defmodule Scratch do
  # Given a list of items, create a map that shows how many times that
  # item appears in the list e.g. [1, 2, 2, 3] => %{1 => 1, 2 => 2, 3 => 1}
  def item_occurences(output, [item | t]) do
    # if we have seen the value before, get the current count otherwise set to nil
    count = case Map.fetch(output, item) do
      {:ok, value} -> value
      :error -> nil
    end

    output
    |> update_map(count, item) # if count is nil, set to 1 otherwise add 1 to it
    |> item_occurences(t)
    end

  def item_occurences(output, []), do: output

  # we have never seen the item, make it 1
  defp update_map(output, nil, item) do
    Map.put(output, item, 1)
  end

  # we have seen the item, add one to the count
  defp update_map(output, count, item) do
    Map.put(output, item, count + 1)
  end
end
```

This worked but needed a lot of code just to do the part where we try to get an item out of the map, then depending on whether it is present or not follow a given path. 

I then discovered the `Map.get_and_update` function which tidied things up significantly. This function allows us to do the retrieval of a value and returning a new map with an updated value based on it's current value in one go.

```elixir
defmodule Scratch do
  def item_occurences(output, [item | t]) do
    {_, updated_map} = Map.get_and_update(output, item, fn current_value ->
      new_value = case current_value do
        nil -> 1
        _ -> current_value + 1
      end
      # this functions requires that we return a tuple of the old and new value
      {current_value, new_value}
    end)

    updated_map
    |> item_occurences(t)
  end

  def item_occurences(output, []), do: output
end
```
