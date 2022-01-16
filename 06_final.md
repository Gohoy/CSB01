# final

The final part is some hint to complete whole program, we didn't provide `list_message()` in any API endpoint, but users of course would like to see existed message.

### Router

In router, we insert `resources "/messages", MessageController, only: [:index]`, this will expect module `ProjectWeb.MessageController` is there.

```elixir
defmodule ProjectWeb.Router do
  use ProjectWeb, :router

  pipeline :api do
    plug :accepts, ["json"]
  end

  scope "/api", ProjectWeb do
    pipe_through :api

    resources "/messages", MessageController, only: [:index]
  end
end
```

### Controller

Create `lib/project_web/controllers/message_controller.ex`, edit

```elixir
defmodule ProjectWeb.MessageController do
  use ProjectWeb, :controller

  def index(conn, _data) do
    messages = Project.Chats.list_message()

    conn
    |> render("list_message.json", messages: messages)
  end
end
```

This lead us need `ProjectWeb.MessageView`.

### View

Create `lib/project_web/views/message_view.ex`, edit

```elixir
defmodule ProjectWeb.MessageView do
  use ProjectWeb, :view
  alias Project.Chats.Message

  def render("list_message.json", %{messages: messages}) do
    render_many(messages, __MODULE__, "message.json")
  end

  def render("message.json", %{message: %Message{name: name, body: body}}) do
    %{
      name: name,
      body: body
    }
  end
end
```

Now, you can access `<your site>/api/messages` to get JSON data.
