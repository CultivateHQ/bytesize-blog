---
layout: post
title: Putting Phoenix in an iframe
date: 2019-01-22 16:26:21 +0000
author: Alan Gardner and Valerie Dryden
categories: elixir phoenix
---

We recently had cause to run a Phoenix application within an iframe on another application. We'd managed to have an application working just fine inside the iframe for a standalone HTML page and a simple Vue app. However when we came to try the Phoenix app we were getting blocked by an error: `Refused to display <blah> in a frame because it set 'X-Frame-Options' to 'sameorigin'`.

In order to get our application to display we needed to remove the 'X-Frame-Options' header from the response we were sending. In Phoenix this header (among others) is added by default through a plug in the router, [put_secure_browser_headers/2](https://hexdocs.pm/phoenix/Phoenix.Controller.html#put_secure_browser_headers/2).

We need to be careful though. This header is there for [our protection](https://responsivedesign.is/articles/xframe-options/) to prevent [clickjacking](https://www.owasp.org/index.php/Clickjacking), so we should only remove the header after authenticating the source of the request.

To solve this we created a custom plug that would authenticate the request and then remove the header from the response on success. Phoenix has a handy function [delete_resp_header/2](https://hexdocs.pm/plug/Plug.Conn.html#delete_resp_header/2) that lets us do just that.

```elixir
# controllers/allow_cross_origin_iframe.ex
defmodule CultivateWeb.AllowCrossOriginIframe do
  import Plug.Conn

  def init(opts), do: opts

  def call(conn, _options) do
    conn
    |> authenticate_source
    |> delete_resp_header("x-frame-options")
  end

  defp authenticate_source(conn) do
    # check that source is allowed to delete header
  end
end
```

This plug can then be added to the router pipeline.

```elixir
defmodule CultivateWeb.Router do
  use CultivateWeb, :router

  pipeline :browser do
    plug :accepts, ["html"]
    plug :fetch_session
    plug :fetch_flash
    plug :protect_from_forgery
    plug :put_secure_browser_headers
    plug CultivateWeb.AllowCrossOriginIframe
  end

  scope "/", CultivateWeb do
    pipe_through :browser

    get "/", PageController, :index
  end
end

```

Is there a better way to solve this issue? Let us [know](https://twitter.com/cultivatehq)!
