---
title: Using Env vars in Elixir
date: 2018-01-20 09:45:28
categories: code
tags: code functional-programming elixir
img: /assets/env-vars-why-you-no-work.jpg
excerpt: It's being a while I don't work with compiled languages and I forgot Elixir is one of them.
---

**TL;DR;** Elixir is a compiled language and as such, every information needed for the code to compile needs to be available at, well..., _compilation_ time. If your application depends on the configuration to run, you'll need to make it available before releasing it.

## The way to env vars

I'm writing a small, barebones, web services using [elixir-plug][08801380] and here are the main mistakes I made trying to specify the Router's HTTP port using environment variables

### Mistake \#1: Using the wrong key in the configuration file
My application is called `Closeconnection` and that was my configuration file.

```elixir
use Mix.Config

config Closeconnection, port: 3000
```

Whenever I tried to access the value of port using, `Application.get_env(:closeconnection, :port)` I'd get back `nil` as return. Once I changed to `config :closeconnection, port: 3000` it worked 🎉!

### Mistake \#2: Env vars are always String

That's the code to start the server:

```elixir
Plug.Adapters.Cowboy.child_spec(
  :http,
  Router,
  [],
  port: Application.get_env(:closeconnection, :port)
)
```

The `Application.get_env` would get the information from the config and the config would get the information from the system with `System.get_env`, so far so good.

```none
$ PORT=4444 mix run
💥
```

The problem here is that the function `Plug.Adapters.Cowboy.child_spec/4` expects port to be an integer but `System.get_env` (and in turn `Application.get_env`) return `String`. After casting it to integer with `String.to_integer(Application.get_env(:closeconnection, :port))` it worked 🎉!


### Mistake \#3: Declaring the Env variable only at Run time
I wanted to get the value of port from the command line and not "hard coded" in a configuration file. Luckily, that's not so hard to do, I just changed my configuration file to the following:

```elixir
use Mix.Config

config :closeconnection, port: System.get_env("PORT")
```

I tested the app, ran it locally with `PORT=4444 mix run` and all seemed to work. Life was good again. I, then, went ahead and bundled the app in a binary using [distillery][6d3aab3c] which allows the app to run in any machine with erlang installed. Cool!

```none
$ mix release
...
$ ./build/dev/rel/closeconnection start
💥
```

The app wouldn't start. After some debugging I found the `PORT` was nil. But why!? That's when remembering that Elixir is a compiled language comes in handy. My app relied in that piece of information (the port) to run, but not to compile, so I needed to pass it during compilation time.

```none
$ PORT=4444 mix release
...
$ ./build/dev/rel/closeconnection start
🎉
```


### To recap

1. Check if config you specified has the same key in the configuration file and in the `Application.get_env`
2. Values coming from `System.get_env` are always String (or `nil` if not found)
3. Env variables must be available at compilation time

Did you have a similar experience with Elixir? How do you manage your env variables? Let me know in the comments below!

  [08801380]: https://github.com/elixir-plug/plug "elixir-plug github"
  [6d3aab3c]: https://github.com/bitwalker/distillery "distillery github"
