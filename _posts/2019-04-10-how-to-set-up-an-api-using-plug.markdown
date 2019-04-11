---
layout: post
title: 'How To Set Up An API Using Plug'
tags: ['API', 'Plug', 'Elixir', 'fast', 'concurrent', 'concurrency']
---
This is a quick and easy tutorial to get you started with setting up a basic API in Elixir using Plug.

Start a new Elixir project:
```bash
$ mix new api --sup
```

Add the `plug_cowboy` dependency to your `mix.exs`:
```elixir
def deps do
  [
    {:plug_cowboy, "~> 2.0"}
  ]
end
```

And install it using the command: `mix deps.get`. Update `lib/my_app/application.ex` as follows:
```elixir
defmodule Api do
  # See https://hexdocs.pm/elixir/Application.html
  # for more information on OTP Applications
  @moduledoc false

  use Application
  require Logger

  def start(_type, _args) do
    # List all child processes to be supervised
    children = [
      Plug.Cowboy.child_spec(scheme: :http, plug: ApiRouter, options: [port: 4001])
    ]

    # See https://hexdocs.pm/elixir/Supervisor.html
    # for other strategies and supported options
    opts = [strategy: :one_for_one, name: Api.Supervisor]
    Logger.info("Starting application...")
    Supervisor.start_link(children, opts)
  end
end
```

To write a "router" plug that dispatches based on the path and method of incoming requests, Plug provides `Plug.Router` (`lib/api_router.ex`):
```elixir
defmodule ApiRouter do
  use Plug.Router

  plug Plug.Logger
  plug :match
  plug :dispatch

  get "/" do
    send_resp(conn, 200, "Hello, api!")
  end

  get "/hello/:name" do
    send_resp(conn, 200, "Hello, #{name}")
  end

  match _ do
    send_resp(conn, 404, "oops")
  end
end
```

And then start the application with the command:
```bash
$ iex --erl "+S 1" -S mix
```

The option `--erl "+S 1"` specifies the number of scheduler threads available to the application.
