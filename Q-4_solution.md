#### 3rd party API integration
Sample of 3rd party http mail delivery service 

##### 3rd party Mailer
Mailer encapsulates mailer adpaters, provides behaviour and 
common API for use in application

- Define Mailer adpater spec
- Implement Mailer

```elixir

Coinable.Mailer do
    @type mail :: Coinable.Mailer.Mail.t()
    @type opts :: list()
    @type uuid :: binary()
    @type reason :: binary()

    @callback send(mail, opts) :: {:ok, uuid} | {:error, reason}

    
    @spec send_mail(mail) :: {:ok, uuid}
    def send_mail(%Coinable.Mailer.Mail{}=mail, opts \\ []) do
        adapter = 
            Application.get_env(:coinable, mailer)
            |> Keyword.fetch!(:adapter)
        Kernel.apply(adapter, :send, [mail, opts])
    end

end
```

##### Implement Testable Adapter
Implement adapters to be used while testing application

```elixir
Coinable.Mailer.Interceptor
    @behaviour Coinable.Mailer
    # Intercepts mails and used to test application

    def send(%Coinable.Mailer.Mail{}=mail, opts \\ []) do
        ...
    end
end
```

##### Adapter
Implement adapters for 3rd party service

```elixir
Coinable.Mailer.Sendgrid
    @behaviour Coinable.Mailer
    def send(%Coinable.Mailer.Mail{}=mail, opts \\ []) do
        # Send mail with sendgrid api
        ...
    end
end
```


##### Integration
Implement integrate to application
```elixir
Coinable.Accounts do
    @type uuid :: binary()

    @spec welcome_user(WalletUser.t()) :: {:ok, uuid}
    def welcome_user(user) do
        user
        |> create_user_welcome_mail()
        |> Coinable.Mailer.send_mail()
    end
end
```
