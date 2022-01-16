# channel

Channel is a concept in Phoenix, it handles multiple clients.

A client can

1. join a topic
2. send event
3. listen event

A channel can

1. handle topic
2. sent event to all subscribe topic
3. listen event from all publish to this topic

### channel code

create `lib/project/channels/room_channel.ex`, edit

```elixir
defmodule Project.RoomChannel do
  use ProjectWeb, :channel
  alias Project.Chats

  @impl true
  def join("room:only", _payload, socket) do
    {:ok, socket}
  end

  # It is also common to receive messages from the client and
  # broadcast to everyone in the current topic (room:only).
  @impl true
  def handle_in("shout", payload = %{name: username, body: body}, socket) do
    Chats.new_message(from: username, body: body)
    broadcast(socket, "shout", payload)
    {:noreply, socket}
  end
end
```

The idea is simple, when a new `shout` event created in socket, handle it's payload by take `name` and `body` and stored it into DB. Then broadcast `payload` to all rest client. You only handle join request to `room:only`. You will not get repeated `shout` from `broadcast`, it won't send same event to self again.

### register channel

Now, you have to register channel, edit `lib/csb01_impl_web/channels/user_socket.ex`

```elixir
defmodule ProjectWeb.UserSocket do
  use Phoenix.Socket

  ## Channels
  channel "room:only", ProjectWeb.RoomChannel

  @impl true
  def connect(_params, socket, _connect_info) do
    {:ok, socket}
  end

  @impl true
  def id(_socket), do: nil
end
```

### Client side(JS example)

Client side code located at `assets/js`, socket library located at `assets/js/socket.js`

```javascript
import { Socket } from "phoenix";
let socket = new Socket("/socket", {});
socket.connect();

// join "room:only"
let channel = socket.channel("room:only", {});
channel.join();
// send "shout" event from client
channel.push("shout", { name: "your name", body: "say hi" });
// handle boardcast event from server
channel.on("shout", ({ name: name, body: body }) => {
  // anything you want
});
```

- swift phoenix client: [GitHub - davidstump/SwiftPhoenixClient: Connect your Phoenix and iOS applications through WebSockets!](https://github.com/davidstump/SwiftPhoenixClient)

### Additional

Let your channel handle multiple rooms(topics), hint: The following code is possible

```elixir
@impl true
def join("room:" <> room_id, _payload, socket) do
    {:ok, socket}
end
```
