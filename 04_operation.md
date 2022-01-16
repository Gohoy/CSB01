# operation on DB

Now, create `lib/project/chats.ex`, edit

```elixir
defmodule Project.Chats do
  import Ecto.Query, warn: false
  alias Project.Repo
  alias Project.Chats.Message

  def new_message(from: username, body: body) do
    %Message{}
    |> Message.changeset(%{create_time: DateTime.utc_now(), name: username, body: body})
    |> Repo.insert()
  end

  def list_message() do
    Message |> Repo.all()
  end
end
```

### Additional

With room concept, it's native to have

1. create room
2. message belongs to room([belongs_to](https://hexdocs.pm/ecto/Ecto.Schema.html#t:belongs_to/1))
3. list message under room
