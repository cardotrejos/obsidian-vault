OTPs Three components
- Erlang
- Libraries and tools
- Design principles

OTPs Design principles
- Process based (cheap)
- Supervision/Agents
- Process based concurrency

*Supervision trees*


Why OTP is good
- Isolated failure
- Supervision is part of architecture
- Makes it easy to work with concurrency
- Makes horizontal scalability easy

### OTP Supervisors and workers

#### Supervisor
Supervise workers
Set list of workers at start (unless Dynamic)
Restart workers based on strategy

- Supervisor one for one : If one failed that one will be restarted
- Supervisor one for all: If one failed other one will be killed and restarted
- Supervisor rest for one: Restart process after the process failed

#### Workers
Execute code
Can be kept alive or one time use
Restart strategy is important (child_spec)

```erlang
def child_spec(opts) do
 %{
	 id: some_child_id,
	 start: {SomeWorker, :start_link, [arg1, arg2]},
	 restart: :permanent | :trasient | :temporary
	 type: :worker | :supervisor
 }
end
```

Restart types
- :permanent - Always restart (max retries supervisor opts)
- :temporary - Never restart
- :transient - Restart if abnormal shutdown (:normal, :shutdown, {:shutdown, term})

#### Supervisors and workers
- Both Processes
- Processes can be named but will have a PID
- Can communicate via PID or Name


OTP Process Communication
- Can be sent or received
- Message box in background queues messages
- One message at a time
- Sent to a PID or a Process Name

### OTP Agents, GenServers & Tasks

#### Genservers
- General Servers
- Can be used to receive messages
- Can be used to send messages
- Can store state

#### Agent
- Can't receive custom messages
- Used to interact and store with state

#### Tasks
- Can't receive custom messages
- Can't be given a Name
- Doesn't have state
- Can be started on any node
- Can run a task
- Very simple single process abstraction

### OTP "Let it crash"

Dangers of crashing
- Restart strategy hits max Restarts
- To many crashes in a short period restarts Supervisor
- Crashing can bubble-up and end up crashing the VM

Try not to "Let it Crash"
- Handle errors if you know the can happen
- Don't rely on crash restarts

Let it crash meaning
- Crashing in one area shouldn't effect another
- Segregated crashing
- Recoverable if some processes crash

### OTP Process Limitations

Messages
- One at a time
- Data is copied between processes

Possible Bottlenecks
- Message processing takes to long
- Mailbox is full and processing is slow

Some bottleneck Solutions
- Pooling
- Logical Process Splits

![[Screenshot 2023-09-19 at 8.35.45 AM.png]]

*Study all in one and logical process split*

### OTP Quiz
What's the difference between Agents and GenServers?
Agents can't handle events and GenServers can

What does a Supervisor do?
Monitor workers

What key do you need to set in the mix.exs when setting up an application.ex for your project?
mod

If you're trying to send 100 messages, what's the best strategy?
Using a Task and sending 1 in each process

Why are status tuples useful?
{:ok, result} | {:error, reason}
They help to allow you to stop the application from crashing

How many messages can a process handle at the same time (in parallel)? 
1

## Testing

#### ExUnit
Built-in Testing Library
- Asynchronous
- test/describe syntax
- assert/refute keywords
- _test.exs (scripts)_

Library Support
- phoenix
- ecto
- absinthe
- plug

#### Writing testeable code
Layer of testing
- Unit (Flush edge case, heavy layer)
- Integration (Light layer)
- End to End (Hound library, not really needed)

What to test?
- Based on what software you are writing
- Testing glue?

Writing code that's testable
- Inputs and outputs in a perfect world
- Side effects make tests harder
- Unit tests shouldn't test side effects
- Integration tests are easier (sometimes)

#### Testing OTP
- Use `setup` to start Process
- Process will be destroyed on test finish
- Asynchronous tests require a process per test
#### Testing Ecto
- Set in the config
- Run inside a transaction
- Async database tests
Data Case
![[Screenshot 2023-09-21 at 6.37.29 PM.png]]

#### Testing REST
- ConnCase (DataCase)
- Routes
ConnCase
![[Screenshot 2023-09-21 at 6.39.46 PM.png]]

![[Screenshot 2023-09-21 at 6.41.12 PM 1.png]]
#### Testing GraphQL

![[Screenshot 2023-09-21 at 6.42.22 PM.png]]
#### Testing GraphQL Subscriptions
![[Screenshot 2023-09-21 at 6.44.31 PM.png]]
![[Screenshot 2023-09-21 at 6.44.44 PM.png]]



### Crushing Bugs with IEx, Logger and IO.Inspect

![[Screenshot 2023-10-01 at 2.16.49 PM.png]]

### Testing Maintainability Quiz
What's the difference between IO.inspect and IEx.pry
IEx pause the code at the pry and allows you to play with the variables in current scope

Why is it important to have unit tests and integration tests?
Integration tests can ensure your code works together

What are the primary ways to mock/stub function results?
Module stubs/mocks and function mocks



### Testing basics Quiz
What does ExUnit.Case do?
Provides us with describe/test/assert

What's the difference between a unit and integration test?
Unit tests check a single function that doesn't call third party functions and Integration tests check functions in combination with other functions

How do you test a Phoenix Controller request?
Build a fake conn and send it to the router

Why is it important to start a process per test if you're testing an OTP abstraction?
So parallel tests are separated

Why is an Ecto.Sandbox important for testing the DB?
It allows asynchronous tests to actually hit the database

Why is it important to split up the work into multiple modules?
It makes it easier to unit test your modules

What's the proper way to test GraphQL Subscriptions?
Send messages over a socket and assert their reply
