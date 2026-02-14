# Pento
   |> mix phx.new pento
   |> cd pento
   |> edit: config/dev.exs
   |> mix ecto.create
   |> mix phx.server | iex -S phx.server
   
## Phoenix specific documentation
   |> iex -S mix #
   |> h Phoenix.LiveView.Socket

## Create first LiveView without event-handle

   |> edit : lib/pento_web/router.ex
          live "/guess", WrongLive
   |> create and edit : lib/pento_web/live/wrong_live.ex
          defmodule PentoWeb.WrongLive do
            use PentoWeb, :live_view
          
            def mount(_params, _session, socket) do
              {:ok, assign(socket, score: 0, message: "Make a guess:")}
            end
          
            @spec render(any) :: Phoenix.LiveView.Rendered.t()
            def render(assigns) do
              ~H"""
              <h1>Your score: {@score}</h1>
              <h2>
                {@message}
              </h2>
              <h2>
                <%= for n <- 1..10 do %>
                  <.link href="#" phx-click="guess" phx-value-number={n}>
                    {n}
                  </.link>
                <% end %>
              </h2>
              """
            end
          end
 ## Handle events
 
    |> Place link:
        <.link href="#" phx-click="guess" phx-value-number="1">1</.link>
