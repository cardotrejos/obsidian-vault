
## Elixir Limitations

Sturdy Foundation
- Easy to create efficient code 
- OTP has rules
- Always profile and measure

Processes
- One message at a time
- Large messages are copied
- State causes memory increase
- Message queue size and speed

![[Screenshot 2024-01-22 at 7.50.21 PM.png]]

Solution

![[Screenshot 2024-01-22 at 7.53.17 PM.png]]

Atoms 
- Created whenever used
- Per node finite amount (tuneable)
- Memory used
- Can be dynamically created

Phoenix
- Time spent on request mean less throughput
- Time spent on your own code
- Overhead from Plug/Middleware
- Websocket Connections (Liveview)

Memory
- Atoms
- Process state
- Process inbox

Honorable mentions
- Accesing files will take up file descriptor limit
- Opening ports will also do the same

## Statistics to measure scale

- RPS/RPM/RPH
- Latency
- CPU
- Memory (RAM)

Measure and plan
- What's current patterns
- What's projected for scaling
- Scaling horizontally or vertically
- Know what the "normal" is

## Side Effects of Scaling

Resource limitations

- CPU
- Memory
Which resource is used the most

Define normal
- CPU Usage
- Memory usage
- Rate of requests
- Speed of request
- Other (Postgres, sockets)

Signs of scale
 - CPU/ Memory 
     - Application code
     - Caches
 - Request slowdown
	 - Resolvers/ Controllers
	 - Plugs/Middleware
 - Storage
	 - Disk IO/ Temp files
	 - Crash logging
 - External services
	 - Postgres
	 - Redis

Direction of scaling

- CPU/Memory - Depends (Can be horizontal or Vertically, what cases?)
- Request slowdown 
- Storage 
- External services 
![[Screenshot 2024-01-22 at 8.17.37 PM.png]]

![[Screenshot 2024-01-22 at 8.15.21 PM.png]]

![[Screenshot 2024-01-22 at 8.13.54 PM.png]]

![[Screenshot 2024-01-22 at 8.13.07 PM.png]]
Bottlenecks from Elixir

Processes
- Processes are single message at once (much repeat)
- Bigger message === slower message processing
- High bottleneck potential due to slowdowns keeping it slow

Overlooked processes
- Http requests need a process to make async
- Access to caches (Agent, GenServers)

External Applications
- External applications are usually the pain points
- Communication uses a process
- What are our external slowdowns

Pooling for increasing bottleneck width

![[Screenshot 2024-01-22 at 8.22.20 PM.png]]

![[Screenshot 2024-01-22 at 8.23.20 PM.png]]

Pooling Libraries Available
- Poolboy (HTTPoison)
- NimblePool
- DBConnection (Ecto)
- PG/PG2

### Pooling Example with Poolboy

![[Screenshot 2024-01-22 at 8.25.31 PM.png]]![[Screenshot 2024-01-22 at 8.25.59 PM.png]]![[Screenshot 2024-01-22 at 8.26.21 PM.png]]
Pooling limitations

- Need enough workers
- Pool manager process can get slow via message queue
- CPU use will grow

![[Screenshot 2024-01-22 at 8.26.58 PM.png]]

Bottlenecks on state access

- Genserver is an easy way to bottleneck access to state
- Agent has less a change of becoming a bottleneck but still posible
- What can we do to allow concurrent reads on state?

ETS Pros/Cons

- Can read concurrently
- Can write concurrently (buckets)
- Better querying capacity
- Can be serialized and stored on disk (:ets.tab2file)
- Slightly slower access than Agent

![[Screenshot 2024-01-22 at 8.52.43 PM.png]]

![[Screenshot 2024-01-22 at 8.54.11 PM.png]]

![[Screenshot 2024-01-22 at 8.53.36 PM.png]]

ETS Types

- Set (default) - One value per unique key
- ordered_set - Ordered by erlang term uses == to match keys
- bag - Multiple unique items under a single key (slow)
- duplicate_bag - Multiple items under a single key

![[Screenshot 2024-01-22 at 8.57.45 PM.png]]![[Screenshot 2024-01-22 at 8.58.46 PM.png]]

Why not to use ETS

- The API is a PAIN IN THE ASS
- Mistakes can be made
- No TTL
- Use ConCache !!

![[Screenshot 2024-01-22 at 9.00.16 PM.png]]

Why to use ConCache?

- The API is clean
- TTLs baked in
- Transactional Locking
- Battle tested

How to Architect with Bottlenecks in Mind

What to think about?

- What will take long periods of time (third party services)
- How fast will the process be hit?
- Who is accesing the process?
- Where is my state being stored?

Make use of async patterns

- Don't block a process within a resolver/controller
- Make use of ETS
- Use Task.Supervisor.async_nolink to do work on GenServers

![[Screenshot 2024-01-22 at 9.04.46 PM.png]]

![[Screenshot 2024-01-22 at 9.05.09 PM.png]]

![[Screenshot 2024-01-22 at 9.05.45 PM.png]]
![[Screenshot 2024-01-22 at 9.05.52 PM.png]]
### Summary 

- Think about your processes a lot
- Who's calling them, how much
- Use async patterns

![[Screenshot 2024-01-22 at 9.15.37 PM.png]]

![[Screenshot 2024-01-22 at 9.15.18 PM.png]]

![[Screenshot 2024-01-22 at 9.12.07 PM.png]]

![[Screenshot 2024-01-22 at 9.11.59 PM.png]]

![[Screenshot 2024-01-22 at 9.11.14 PM.png]]

![[Screenshot 2024-01-22 at 9.10.54 PM.png]]

![[Screenshot 2024-01-22 at 9.07.56 PM.png]]

## Connecting Elixir Nodes

Clustering
- Grouping node together in a network
- Mesh topology
- TCP messaging between nodes

Cookies
- Release cookie must be the same to be clustered
- Can set during release with RELEASE_COOKIE
- Can be set in mix.exs release_config as cookie key
- Can use iex --cookie "SECRET" to set a cookie

 
![[Screenshot 2024-01-30 at 8.09.49 PM.png]]

![[Screenshot 2024-01-30 at 8.09.45 PM.png]]

![[Screenshot 2024-01-30 at 8.11.47 PM.png]]

![[Screenshot 2024-01-30 at 8.11.30 PM.png]]

Warnings

- Clustering is naive (No partition tolerance)
- Large amounts of nodes it becomes expensive
- Global space is shared (locking, processes, etc)


### Send messages across nodes

![[Screenshot 2024-01-30 at 8.14.52 PM.png]]

![[Screenshot 2024-01-30 at 8.14.08 PM.png]]

![[Screenshot 2024-01-30 at 8.13.44 PM.png]]

![[Screenshot 2024-01-30 at 8.27.23 PM.png]]

![[Screenshot 2024-01-30 at 8.24.32 PM.png]]

![[Screenshot 2024-01-30 at 8.23.02 PM.png]]

PID Type

- PID is a type which means to send it across nodes
- Can pass around PIDs to address distributed process

![[Screenshot 2024-01-30 at 8.29.04 PM.png]]![[Screenshot 2024-01-30 at 8.29.32 PM.png]]

![[Screenshot 2024-01-30 at 8.29.23 PM.png]]

### Receiving Messages

- Receive block
- GenServer handle_call/hanlde_cast/handle_info
- Agent calls

## Clustering Nodes with LibCluster

Distribution is hard
- Netsplits 
- Address changes
- Topology
- etc

![[Screenshot 2024-01-30 at 8.31.12 PM.png]]

Benefits

- Easy to setup
- Can use different strategies to setup communication
- Compatible with kubernetes easily
- Self healing

Strategies

- (default) EPMD - Default choise, uses epmd
- ErlangHosts - Uses .hosts.erlang file
- Gossip - Uses multicast UDP to automate discovery
- Kubernetes/Kubernetes.DNS - K8s automated discovery

Brief note on EPMD 
- EPMD - Erlang Port Mapper Daemon
- Acts as name service
- Registers name@host -> port

### Distributing work with Tasks

Load management

- Distributing work across nodes
- Don't use Node.spawn when possible
- Don't use send manually if not necessary
- Tasks/GenServers/Agents
- Spawn with Tasks

![[Screenshot 2024-01-30 at 8.38.49 PM.png]]
![[Screenshot 2024-01-30 at 8.39.20 PM.png]]

![[Screenshot 2024-01-30 at 8.39.15 PM.png]]
![[Screenshot 2024-01-30 at 8.39.35 PM.png]]![[Screenshot 2024-01-30 at 8.39.59 PM.png]]![[Screenshot 2024-01-30 at 8.40.18 PM.png]]![[Screenshot 2024-01-30 at 8.40.35 PM.png]]

### Distributing/Scaling Phoenix & Absinthe

Do we need distribution
- Are there web sockets
- If not just add more nodes

Steps to scale
- Ensure we have a valid cluster setup
- Add more nodes or memory/cpu
- You're done!

![[Screenshot 2024-01-30 at 8.43.35 PM.png]]

## Quiz

![[Screenshot 2024-01-30 at 8.53.41 PM.png]]

![[Screenshot 2024-01-30 at 8.53.30 PM.png]]

![[Screenshot 2024-01-30 at 8.53.19 PM.png]]

![[Screenshot 2024-01-30 at 8.53.07 PM.png]]

![[Screenshot 2024-01-30 at 8.52.58 PM.png]]

![[Screenshot 2024-01-30 at 8.52.50 PM.png]]

![[Screenshot 2024-01-30 at 8.52.40 PM.png]]

![[Screenshot 2024-01-30 at 8.52.28 PM.png]]

![[Screenshot 2024-01-30 at 8.52.16 PM.png]]
![[Screenshot 2024-01-30 at 8.52.05 PM.png]]


## Using GenStage to Process Lots of Data

What is a GenStage
- GenStage is a pipeline system
- Data moves from one "stage" to another
- Demand based
- Backpressure!

![[Screenshot 2024-01-30 at 8.56.35 PM.png]]![[Screenshot 2024-01-30 at 8.57.43 PM.png]]

![[Screenshot 2024-01-30 at 8.58.05 PM.png]]

GenStage Thoughts
- How should our pipeline restart
- Do we need to use multiple nodes for processing
- How will we scale
- Do we need back-pressure?

Demand
- Each stage has it's own min/max demand
- max = The maximum demand ever
- minimum = The minimum before asking for more
- Does not mean batching
![[Screenshot 2024-01-30 at 9.00.31 PM.png]]

## GenStage Dispatchers

What they do
- Algorithm for event sending
- 3 different built-ins

![[Screenshot 2024-01-30 at 9.02.51 PM.png]]
![[Screenshot 2024-01-30 at 9.03.17 PM.png]]
![[Screenshot 2024-01-30 at 9.03.57 PM.png]]

### GenStage Consumer Producer

Consumer & Producer
- Can both consume and emit events
- Good for middle stage of pipeline
- Not used for architecture
![[Screenshot 2024-01-30 at 9.05.30 PM.png]]

Bottlenecks

- Due to demand you can create a demand bottleneck
- Downstream stages have no work
- min_demand and max_demand will need tuning

### Quiz

![[Screenshot 2024-01-30 at 9.11.58 PM.png]]

![[Screenshot 2024-01-30 at 9.11.45 PM.png]]

![[Screenshot 2024-01-30 at 9.10.58 PM.png]]

![[Screenshot 2024-01-30 at 9.10.31 PM.png]]

![[Screenshot 2024-01-30 at 9.10.16 PM.png]]

![[Screenshot 2024-01-30 at 9.09.38 PM.png]]

