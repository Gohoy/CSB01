# DB schema

Although Phoenix framework will generate test, seasoned programmer usually don't use them.

A good idea is start from schema, `lib/project/chats/message.ex`

```elixir
defmodule Project.Chats.Message do
  use Ecto.Schema
  import Ecto.Changeset

  schema "messages" do
    field :create_time, :naive_datetime, null: false
    field :name, :string
    field :body, :string
  end

  @doc false
  def changeset(message, attrs) do
    message
    |> cast(attrs, [:create_time, :name, :body])
    |> validate_required([:create_time, :name, :body])
  end
end
```

Then run `mix ecto.gen.migration create_message`, that will generate `priv/repo/migrations/<date number>_create_message.exs`, edit it

```elixir
defmodule Project.Repo.Migrations.CreateMessage do
  use Ecto.Migration

  def change do
    create table(:messages) do
        add :create_time, :naive_datetime, null: false
        add :name, :string
        add :body, :string
    end
  end
end
```

### Additional

Reference: https://hexdocs.pm/ecto/Ecto.html#module-schema

1. create a `Project.Chats.Room`
   ```
   %Room {
       name : String,
   }
   ```
2. let `Project.Chats.Message` belongs to `Project.Chats.Room`
