---
layout: post
title: "Getting started with Elixir"
date: 2016-01-07 22:06
comments: true
published: false
categories:
- Elixir
---

Some notes on getting started with Elixir:

Mix
Hex

    $ mix new trackline

add dep :erlsom
mix deps.get
mix

compile, compile

add mix-test.watch (https://github.com/lpil/mix-test.watch)

how to debug? `IEx.pry`

Do I need to know about a "type"? How do I find out?

```elixir
require IEx

IEx.pry
```

$ iex -S mix test


### Apex as a pretty formatter

```elixir
IO.inspect("foo")
Apex.ap "Foo"

defmodule Foo do
  adef do_something do
  end
end
```

Pattern matching is fun. (More on this...?)

Arranging files in Elixir project: modulizing files.

```elixir
defmodule Base do
end

defmodule Base.SomeFoo do
  alias Base.SomeBar
  SomeBar.some_method
end

defmodule Base.SomeBar do
end
```

### Typespecs

Type must be aliased as `t` - idomatic way.

http://learningelixir.joekain.com/elixir-type-specs/

```elixir
defmodule Trackline.Trackpoint do
  @type t :: map
end
```

