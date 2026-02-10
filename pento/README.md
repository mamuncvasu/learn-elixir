# Pento
   |> mix phx.new pento
   |> cd pento
   |> edit: config/dev.exs
   |> mix ecto.create
   |> mix phx.server | iex -S phx.server
   
## Phoenix specific documentation
   |> iex -S mix #
   |> h Phoenix.LiveView.Socket
