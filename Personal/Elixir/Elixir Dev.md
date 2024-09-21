#### Level One

What do you need to watch out for when writing functional code?
- Naming properly and clear
How can you as a user call other languages from Elixir?
- Ports and NIFS
What are the 3 types of elixir functions?
- BIFS, NIFS and User Functions

#### Level Two

Main Types 

- Tuples - {}
- Lists - []
- Maps - %{}
- Strings - ""
- Atom - :atom
- Ranges - 1..2
- Date/Time/DateTime/NaiveDateTime
- Numbers (Integer/Float) - 1 / 1.0
- Everything is data!

Strings and Charlist

- "" vs ''
- Codepoint is UTF-8
- Charlists are a list of code points
- IO.inspect with charlists: :as_lists

'abc' different to "abc"

Atoms

- Globally unique
- Value is the atom
- Finite amount

Important Modules (Most used!)

- Kernel
- String
- Enum
- List
- Map
- Keyword

The Map Module

- Mainly a helper module
- Can access maps via map[:key], map["key"]
- Multiple of the same functions (different behaviour)
- Lazy functions
- Embrace Map.new

The Enum module

- Map & List Helpers
- Works with anything that implements Enumerable
- Ranges are Enumerable's
- Higher order functions (map/reduce)
- Don't use Enum.each

Pattern Matching 
- Performed with === essentially
- Allows us to deconstruct
- Can be used for all types of data
- Can be used inside arguments

Pattern Matching with Maps
- The most forgiving form
- Can be used on any key
- Can be selectively used

Pattern Matching with Lists
- Not very forgiving
- Must match every key
- Can match on order `[head | tail]`
- Recursion with head tail

Pattern Matching with Functions
- Can be used inside arguments
- Can match anything!
- Define multiple functions with different matches

Keywords 
- Ultimately just a list of tuple pairs
- Can be fetched with `keyword[:key]`
- Not guaranteed to be unique
- Very common

Access Module
- Access is implemented for Keywords and Maps
- Can access them via `item[key]`
- Helpers `get_in/update_in/put_in`

Functions and short-form
- Anonymous functions `fn -> end`
- Short-form anon functions `&(&1)`
- Short form function passing `&function/1`

Structs
- Essentially a guarantee of keys
- Can match the struct
- Map.from_struct
- Can enforce keys to be provided on create
- Default values

Behaviours
- Allow for polymorphism
- Define a contract
- @impl

behaviour.ex
```elixir
defmodule MyBehaviour do
	@callback math(integer) :: integer

	def serialize_math(behaviour, value) do
		to_string(behaviour.math(value))
	end
end

defmodule Division do
	@behaviour MyBehaviour

	@impl MyBehaviour
	def math(int) do
		int / 2
	end
end

defmodule Multiplication do
	@behaviour MyBehaviour

	@impl MyBehaviour
	def math(int) do
		int * 5
	end
end
```

Protocols
- Allow for type based implementation
- Enumerable
- Powerful

#### Control Flow Options
- case
- if
- unless (don't use)
- cond
- with
- function declaration

clean_flow.ex
```elixir
defmodule CleanFlow do
	def ifelse(value) do
		if value do
			"true"
		else
			"else"
		end
	end

	def ifelse(value) do
		if value do
			"true"
		else
			"else"
		end
	end

	def case(value) do
		case value do
			%{} -> "value was a map"
			x when is_list(x) -> "value was a list"
			[] -> "value was an empty list"
			[_ | _] -> "value was a list"
			true -> "value fell through"
			_ -> "anything"
		end
	end

	def cond(value) do
		cond do
			value === 1 -> "value was one"
			is_nil(value) -> "value was nil"
			true -> "value was anything"
		end
	end

	def with_example(params) do
		with true <- params[:a],
			 false <- params[:b],
			 :ok <- params[:c] do
			"we passed all the cases"
		else 
			false -> "was false"
			:error -> "was an error"
			_ => "fell through"
	end

	def with_example(params) do
		with {:ok , value} <- functioncall(),
			 {:ok, value } <- save_to_database() do
end
```

### Pipes
- |>
- One Thing into another
- If using pipes go all in
- Skips first parameter

```elixir
defmodule PipesExample do
	def example do
		"string asdf"
		 |> String.split(" ")
		 |> length
		 |> Kernel.>(2)
		 |> Kernel.===(true)

```


## Functional Descriptive Flow

Descriptive Functions
- Keep things descriptive
- Try and describe what things do
- Don't be vague
- The cleaner the better
- Long is ok

Descriptive Hoisting
- Move core logic close to root function
- Describe what that main function does
- Singular module access points
- defdelegate


```elixir
defmodule GiphyScraper do

	def search(query) do
		base_url = "https://api.giphy.com/v1/gifs/search"
		api_key = "KCpGh0NPdCUSntXagzmISAGntlNxcF0v"
		limit = 25
		
		request_url = "#{base_url}?api_key=#{api_key}&q=#{query}&limit=#{limit}"
		headers = [{"Content-Type", "application/json"}]
		request = Finch.build(:get, request_url, headers)
		
		case Finch.request(request, GiphyScraper.Finch) do
			{:ok, %Finch.Response{status: 200, body: body}} ->
			body
			|> Jason.decode!()
			|> parse_results()
			_error ->
			[]
		end
	end
	
	defp parse_results(%{"data" => results}) when is_list(results) do
		Enum.map(results, fn result ->
			%GiphyScraper.GiphyImage{
			id: result["id"],
			url: result["images"]["original"]["url"],
			username: result["username"],
			title: result["title"]
			}
		end)
	end
end
```