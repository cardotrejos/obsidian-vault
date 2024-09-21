
This works
```elixir
import Config

config :graphql_api_rtr, GraphqlApiRtr.Repo,
	database: "graphql_api_rtr_repo",
	username: "postgres",
	hostname: "localhost"

config :ecto_shorts,
	repo: GraphqlApiRtr.Repo,
	error_module: EctoShorts.Actions.Error

config :graphql_api_rtr,
	ecto_repos: [GraphqlApiRtr.Repo],
	secret_key: "ThisIsAReallySecretKeySecureAndReliable",
	token_max_age: %{unit: :hour, amount: 24}
```

But this was not working


```elixir
import Config

  

config :graphql_api_rtr,
	ecto_repos: [GraphqlApiRtr.Repo],
	database: "graphql_api_rtr_repo",
	username: "postgres",
	hostname: "localhost",
	secret_key: "ThisIsAReallySecretKeySecureAndReliable",
	token_max_age: %{unit: :hour, amount: 24}

  

config :ecto_shorts,
	repo: GraphqlApiRtr.Repo,
	error_module: EctoShorts.Actions.Error

  

config :graphql_api_rtr,
	ecto_repos: [GraphqlApiRtr.Repo]
```
