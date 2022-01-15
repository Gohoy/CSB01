# setup

- elixir official document: [Elixir Documentation - The Elixir programming language](https://elixir-lang.org/docs.html)
- phoenix site: [Phoenix Framework](https://www.phoenixframework.org/)
- package register: [hexdocs](https://hexdocs.pm)

#### start

```shell
mix archive.install hex phx_new
mix phx.new <project name> --no-html
cd <project name>
# dependencies update
mix deps.get
# Database
docker run --rm --name dev-postgre -e POSTGRES_PASSWORD=postgres -p 5432:5432 -d timescale/timescaledb-postgis:latest-pg12
mix ecto.create
```

#### develop

```shell
# test
mix test
# run server
mix phx.server
# REPL with current project load
iex -S mix
```
