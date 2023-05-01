#### Issues

```elixir
@spec create(String.t()) :: {:ok, WalletUser}
```
- The verb create is too generic and does not reflect the intent of the function and which must then be infered from the return spec 
- The `String.t()` does not describe clearly what type of string the `create` function requires inorder to create the `WalletUser`
- The `WalletUser` return type should be more inline with common elixir standards


### Solution

```elixir
@typedoc """
string_type_name is ....
"""
@type string_type_name :: String.t()
@spec create_user_wallet(string_type_name) :: {:ok, WalletUser.t()}
```

Instead the function spec should be named appropriatly to reflect what the
function will do `create_wallet_user` which is creating a wallet user on/in 
this wallet domain/context (DDD)

Notice the name is not `create_user` eigther which will be okay
if the application had a generic user type and that was the intent

Defining a custom type `string_type_name` with its doc so as to
describe the type of string required by the function. 
Situations like this are often overlooked and require extra validation in code
which is why I wrote the library ([Blueprint](https://github.com/mfoncho/blueprint)) 
to handle similar situations in with structs and also provide validation

If the intent is to return a `%WalletUser{...}` tuple then the conversion is to define it as `WalletUser.t()` or create a custom type in the spec if a specific struct type variant will be returned

