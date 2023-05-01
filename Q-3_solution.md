#### Delete stale fullfilled addresses

There are 3 main ways to go about running periodic jobs depending on the application, the infrastructure, and the data
    1) GenServer Process small periodic tasks which are NOT memory intensive
    2) Job Processing library for distributed tasks, when task locks are required and service needs to meet SLA's
    3) Cron jobs with mix tasks for running job that work on large data sets and primarily reduce or move data

Deleting customers home address two weeks after order is fullfilled by 
the mechant can be done using the GenServer process because
the task is not memory intensive and requires no task locks

Customer Shipping addresses will have its own table seperate 
from the Shipping Label and only references by the shipping address
On shipping address delete, address references will be mantained for analytics

```elixir
defmodule Coinable.Store.CustomerOrder do
    use Ecto.Schema
    schema "store_orders" do
        ...
        has_one :shipping_label,    Coinable.Store.ShippingLabel, foreign_key: :order_id
        timestamp()
    end
end

defmodule Coinable.Store.ShippingAddress do
    use Ecto.Schema
    schema "shipping_addresses" do
        ...
        field :street,              :string
        field :building_number,     :integer
        field :apartment_number,    :integer
        timestamp()
    end
end

defmodule Coinable.Store.ShippingLabel do
    use Ecto.Schema
    schema "shipping_labels" do
        ...
        field :state_province,      :string
        field :address_code,        :string
        field :country,             :string
        field :customer_notes       :string
        field :mechant_notes        :string
        field :tracking_code        :string
        belongs_to :order,          Coinable.Store.CustomerOrder,   foreign_key: :order_id
        belongs_to :address,        Coinable.Store.ShippingAddress, foreign_key: :address_id
        timestamp()
    end
end

defmodule Coinable.Store.FulFillment do
    use Ecto.Schema
    schema "store_fulfillment" do
        ...
        field :confirmation_code,   :string
        field :fulfilled_at,        :utc_datetime_usec
        belongs_to :order,          Coinable.Store.CustomerOrder,   foreign_key: :order_id
        timestamp()
    end
end
```

```elixir
defmodule Coinable.Store do
    ...
    @spec list_stale_addresses(fields_query, opts) :: list_of_shipping_address
    @spec delete_shipping_address(Coinable.Store.ShippingAddress.t()) :: {:ok, Coinable.Store.ShippingAddress.t()} | {:error, term}
end
```

The `list_stale_addresses` lists addresses for fulfilled orders with shipping labels
that still have valid addresses 2 weeks after fullfillments

Task
```elixir
defmodule Mix.Task.Coinable.DeleteFulfilledAddresses do
    use Mix.Task

    impl Mix.Task
    def run(_args) do
        Coinable.Store.list_stale_address()
        |> Enum.each(fn address -> 
            {:ok, _deleted} = Coinale.Store.delete_shipping_address(address) 
        end)
        :ok
    end

end
```

Worker
```elixir
defmodule Coninable.Workers.DeleteFulfilledAddressesWorker do
    use GenServer
    alias Mix.Task

    @impl true
    def init(opts) do
        send(self, :run)
        {:ok, opts}
    end

    @impl true
    def handle_info(:run, state) do
        :ok = 
            Task.Coinable.DeleteFulfilledAddresses
            |> Task.task_name()
            |> Task.run()
        {:noreply, state, {:continue, :reschedule}}
    end

    @impl true
    def handle_continue(:reschedule, state) do
        reschedule_next_run()
        {:noreply, state, :hibernate}
    end

    defp reschedule_next_run do
        timeout = 
            next_run_date()
            |> DateTime.diff(DateTime.utc_now(), :microsecond)
        Process.send_after(pid, :run, timeout)
    end

    @spec next_run_date() :: DateTime.t()

end
```

The worker should be started under the appropriate application worker supervision tree
