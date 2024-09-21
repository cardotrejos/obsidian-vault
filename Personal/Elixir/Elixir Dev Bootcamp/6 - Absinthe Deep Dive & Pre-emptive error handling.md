### Failure sources

- Third party APIs
- HTTP
- Ports
- System commands
- Async code
- Our own stuff


### Erroring gracefully

![[Screenshot 2023-12-24 at 2.18.52 PM.png]]![[Screenshot 2023-12-24 at 2.19.42 PM.png]]![[Screenshot 2023-12-24 at 2.20.05 PM.png]]![[Screenshot 2023-12-24 at 2.20.59 PM.png]]

### Writing to handle unknown errors

![[Screenshot 2023-12-24 at 2.25.17 PM.png]]

Types of Log

1 - Debug
2 - Info
3 - Warn
4 - Error

Writing Good Messages
- Be clear
- You can use a HereDoc/""" and /n
- Kernel.inspect to convert anything into a string
- Message read by others
- Adopt a format
	- [MyModule.SubModule] This says something
	- [MyModule.Submodule.function/2] Says something

Example use cases
- Ecto Queries
- Phoenix Events
-  Your Process actions
 
![[Screenshot 2023-12-30 at 10.49.26 AM.png]]

## Creating Error Systems for Debugging

#### Making known exceptions easier
- Exception are fatal
- Only if status tuples not feasible
- No backup path
- Separation with custom errors
- Monitoring services

#### Exception Debugging Aids
- Message
- Process.info(self(), :current_stacktrace)
- Exception.format_stacktrace()
- Relevant metadata

![[Screenshot 2023-12-30 at 10.57.42 AM.png]]

![[Screenshot 2023-12-30 at 10.59.23 AM.png]]

![[Screenshot 2023-12-30 at 10.59.58 AM.png]]

![[Screenshot 2023-12-30 at 11.00.47 AM.png]]

### Using IEX to Debug Code

#### IEx Commands

- `iex -S mix` - Load currrent project into iex
- `recompile` - Recompiles the current project
- `c` - Compile a single file
- `i` - Find information about a variable
- `h` - Help, prints out available commands and context help

![[Screenshot 2023-12-30 at 11.27.56 AM.png]]

![[Screenshot 2023-12-30 at 11.28.48 AM.png]]

![[Screenshot 2023-12-30 at 11.28.36 AM.png]]![[Screenshot 2023-12-30 at 11.29.24 AM.png]]

### Debugger with Observer

- Study the quiz
![[Screenshot 2023-12-31 at 11.14.02 AM.png]]

![[Screenshot 2023-12-31 at 11.13.23 AM.png]]

![[Screenshot 2023-12-31 at 11.12.32 AM.png]]

![[Screenshot 2023-12-31 at 11.12.24 AM.png]]

## Absinthe's Middleware


