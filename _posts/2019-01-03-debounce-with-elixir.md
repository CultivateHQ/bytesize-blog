---
layout: post
title: Debounce with Elixir and GenServer timeout
date: 2019-01-03 15:54:33 +0000
author: Paul Wilson
categories: elixir otp
---

Over the New Year holiday I worked on a little [Nerves](https://nerves-project.org) project to help my daughter with her morse skills. It enables text to be sent to Slack, via a morse [telegraph key](https://en.wikipedia.org/wiki/Telegraph_key) and a [Raspberry Pi Zero W](https://www.raspberrypi.org/products/raspberry-pi-zero-w/). (The project is [here](https://github.com/paulanthonywilson/morsey).)

One of the issues to overcome was [contact bounce](https://en.wikipedia.org/wiki/Switch#Contact_bounce) on a key down. Several events showing the circuit opening and closing in quick succession would be sent by the [Elixir Circuits](https://github.com/elixir-circuits/circuits_gpio) library each time the key was depressed.

Rather than _debounce_ with an electrical circuit I decided to do it with an OTP [GenServer](https://hexdocs.pm/elixir/GenServer.html), which initially looked like this:

```elixir
defmodule Telegraph.Debounce do
  @moduledoc """
  Remove the bounce caused by closing the telegraph key. Doing it in
  software so we don't have to mess with capacitors. Ignores events that occur
  within 40ms of each other. Sends events that are not followed by another after 50 milliseconds.


  See https://en.wikipedia.org/wiki/Switch#Contact_bounce
  """

  use GenServer

  @debounce_time 40

  defstruct receiver: nil, last_message: nil
  @type t :: %__MODULE__{receiver: pid, last_message: any()}

  @doc """
  Pass in the pid that receives events
  """
  @spec start_link(pid()) :: :ignore | {:error, any()} | {:ok, pid()}
  def start_link(events_receiver) do
    GenServer.start_link(__MODULE__, events_receiver)
  end

  def init(receiver) do
    {:ok, %__MODULE__{receiver: receiver}}
  end

  def handle_info(:timeout, s) do
    %{receiver: receiver, last_message: message} = s
    send(receiver, message)
    {:noreply, s}
  end

  def handle_info(message, s) do
    {:noreply, %{s | last_message: message}, @debounce_time}
  end
end
```

The third element in the default `handle_info/2` return tuple is a 40ms timeout. This means that _if no more messages_ are received within 40ms, a `:timeout` message is sent to the GenServer. Only on receiving a timout is the _last message_ sent on to the receiving process. That is, a "key down" or "key up" message is only passed on once the circuit has settled down.

([Later on](https://github.com/paulanthonywilson/morsey/blob/cc9f75b8b9cce67f3683f6ba0a97b10ba5aea06d/apps/telegraph/lib/telegraph/debounce.ex), it got a bit more sophisticated: the last message is passed on, but with the timestamp from the first message of the sequence.)

