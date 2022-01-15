# play around

First, start

```shell
iex -S mix
```

### practice

- `12312425215447546 * 898247982786479328786`
- `:whatsthis`
- `"Hello, Elixir"`
- `"Hello, Elixir" |> String.graphemes() |> Enum.frequencies()`
- `%{a: 1}`
- `%{a: a} = %{a: 1}`, `a` = ?

### Invoke Erlang module

For example, `crypto` module from Erlang

```elixir
:crypto.hash(:md5, "crypto")
```

### define module

You can add module in `lib/`, there have no strict rules for module name(unlike Haskell), but making a convention in project is important.

```elixir
defmodule A.B.C.D do
end
```

### define constant in module

An identifier start with `@` in module scope is constant.

```elixir
defmodule A.B.C.D do
    @a_constant "hi"
end
```

### define function

- `def` define a function
- `defp` define a private one
- `defmacro` considered harmful

Here is an example

```elixir
defmodule A.B.C.D do
    def do_something(a, b, c, d) do
    end
end
```

One can do pattern matching on parameter

```elixir
def do_something(a, b, c, d = %{name: name}) do
end
```
