### Conditionals

Next is kind of a ternary on Elixir

```elixir
if condition, do: true_expr, else: false_expr


guess = "hangman"

answer = "hangman"

correct = if guess == answer, do: "Correct!", else: "Incorrect."
```

### Case

```elixir
guess = 12
answer = 13

correct = cond do
	guess == answer -> "Correct!"
	guess < answer -> "Too low!"
	guess > answer -> "Too high!"
end
```