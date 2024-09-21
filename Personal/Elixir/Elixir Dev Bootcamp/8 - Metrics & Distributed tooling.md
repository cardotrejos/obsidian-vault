![[Screenshot 2024-03-04 at 8.45.14 PM.png]]


![[Screenshot 2024-03-04 at 8.45.26 PM.png]]
## Metric Examples
- Throughput
- Response time
- Uptime
- Queue Length
- Internal custom metrics

## Metric Types
- Counters (Increasing number measured every x period)
- Gauges (Single value as a metric)
- Histograms (Sample of observations in buckets)

## When to add metrics
- What do we need to track?
- What is going through our system?
	- Requests
	- Bytes
- What do we need to measure to make better decisions?
	- Latency
	- Reliability
- What are times we can measure to track how we're doing?

![[Screenshot 2024-03-04 at 8.50.24 PM.png]]

# Beam Telemetry

## Telemetry

- telemetry
	- Dispatching events for metrics and other instrumentation
	- Basically a cast based system
	- Most libraries implement metrics
 - telemetry_metrics
	 - Metric types to collect and aggregate values
	 - Gives you histogram/gauge/counter

![[Screenshot 2024-03-21 at 7.57.04 PM.png]]

![[Screenshot 2024-03-21 at 7.56.57 PM.png]]

![[Screenshot 2024-03-21 at 7.56.51 PM.png]]

![[Screenshot 2024-03-21 at 7.56.45 PM.png]]

![[Screenshot 2024-03-21 at 7.56.38 PM.png]]

![[Screenshot 2024-03-21 at 7.56.32 PM.png]]

## Setting up Prometheus and Grafana

### Quick setup

- Prometheus captures metrics (prometheus.io)
- Grafana displays charts from sources (grafana.com)
	- Prometheus
	- Stackdriver
	- Postgres


## Gathering metrics with Prometheus

- Telemetry.Metrics short term
- Scrapes our metrics externally (pull)
- Prometheus is a time series DB
- Can write PromQL queries
![[Screenshot 2024-03-23 at 4.40.09 PM.png]]

![[Screenshot 2024-03-23 at 4.39.59 PM.png]]

![[Screenshot 2024-03-23 at 4.39.49 PM.png]]

Collecting Telemetry.Metrics
- Utilise our existing Telemetry.Metrics
- Expose an telemetry data via endpoint
- Three libraries
	- theblitzapp/prometheus_telemetry_elixir
	- beam-telemetry/telemetry_metrics_prometheus
	- akoutmos/prom_ex
![[Screenshot 2024-03-23 at 4.44.38 PM.png]]

![[Screenshot 2024-03-23 at 4.44.14 PM.png]]

![[Screenshot 2024-03-23 at 4.44.06 PM.png]]

![[Screenshot 2024-03-23 at 4.43.56 PM.png]]

![[Screenshot 2024-03-23 at 4.43.46 PM.png]]

![[Screenshot 2024-03-23 at 4.43.28 PM.png]]

![[Screenshot 2024-03-23 at 4.43.16 PM.png]]

## Prometheus Counters, Gauges & Histograms

#### Counters, Gauges & Histograms

#### Metric Types

- Counters
	- Increasing number
	- We use these to measure rps/ops or total in time
- Gauges
	- Single value as metric
	- We use this to measure the current value
- 

![[Screenshot 2024-03-23 at 4.53.37 PM.png]]

![[Screenshot 2024-03-23 at 4.53.30 PM.png]]

![[Screenshot 2024-03-23 at 4.53.24 PM.png]]

![[Screenshot 2024-03-23 at 4.53.16 PM.png]]

![[Screenshot 2024-03-23 at 4.53.09 PM.png]]

![[Screenshot 2024-03-23 at 4.53.02 PM.png]]

![[Screenshot 2024-03-23 at 4.52.54 PM.png]]

![[Screenshot 2024-03-23 at 4.52.41 PM.png]]

![[Screenshot 2024-03-23 at 4.52.27 PM.png]]

![[Screenshot 2024-03-23 at 4.52.21 PM.png]]

![[Screenshot 2024-03-23 at 4.52.15 PM.png]]

![[Screenshot 2024-03-23 at 4.51.51 PM.png]]

![[Screenshot 2024-03-23 at 4.51.46 PM.png]]

![[Screenshot 2024-03-23 at 4.51.32 PM.png]]

![[Screenshot 2024-03-23 at 4.51.26 PM.png]]

![[Screenshot 2024-03-23 at 4.51.17 PM.png]]

![[Screenshot 2024-03-23 at 4.51.10 PM.png]]

![[Screenshot 2024-03-23 at 4.50.47 PM.png]]

![[Screenshot 2024-03-23 at 4.50.36 PM.png]]

![[Screenshot 2024-03-23 at 4.50.22 PM.png]]

![[Screenshot 2024-03-23 at 4.50.09 PM.png]]
## Building Good Dashboards
- Clean
- Latency/Throughput
- Reliability
- Repeatability

## Using :global for Distributed Communication

Introducing :global
- Starts automatically on every node
- Keeps our mesh connected
	- Can disable this with a kernel flag in vm.args
	-  -connect_all false
- Contains distributed locking
- Registration of global processes

OTP 25 Changes
- Default prevents overlapping partitions
- Disabled with -prevent_overlapping_partitions false
- -net_setuptime arg for node connect time
- Node registration in large clusters go VERY expensive

Why to Lock
- Prevent the same thing running at the same time
- Lock a resource from access across a distributed cluster

Words of caution
- Locking is synchronous
- Sends a message to every node
- Messages creates a new processes


![[Screenshot 2024-04-14 at 4.30.54 PM 2.png]]

![[Screenshot 2024-04-14 at 4.29.21 PM 2.png]]

![[Screenshot 2024-04-14 at 4.29.15 PM 2.png]]

![[Screenshot 2024-04-14 at 4.29.06 PM 2.png]]

![[Screenshot 2024-04-14 at 4.29.01 PM 2.png]]

![[Screenshot 2024-04-14 at 4.28.54 PM 2.png]]

![[Screenshot 2024-04-14 at 4.28.43 PM 2.png]]

![[Screenshot 2024-04-14 at 4.28.37 PM 2.png]]
Summary

- Global can be used to register process names
- Used to lock with :global.set_lock
- Used as a name to GenServer for a global processes
	- Singleton strategy isn't great for already_started
	- Supervisor as a manager on each node it's better
	- https://github.com/arjan/singleton

## CAP Theorem Basics

![[Screenshot 2024-04-14 at 4.34.04 PM.png]]
Summary
- You only choose 2 in theory
- Multi-node, what happens in net-split?

## Creating Distributed Supervisor with Horde

Horde
- Library for distributed Registry/Supervisors
- Backend by DeltaCRDT
- AP part of CAP
  
  ![[Screenshot 2024-04-14 at 5.02.20 PM.png]]

![[Screenshot 2024-04-14 at 5.02.11 PM.png]]

![[Screenshot 2024-04-14 at 5.02.05 PM.png]]

![[Screenshot 2024-04-14 at 5.02.00 PM.png]]

![[Screenshot 2024-04-14 at 5.01.48 PM.png]]

![[Screenshot 2024-04-14 at 5.01.40 PM.png]]

![[Screenshot 2024-04-14 at 5.01.33 PM.png]]

![[Screenshot 2024-04-14 at 5.01.13 PM.png]]

![[Screenshot 2024-04-14 at 5.01.07 PM.png]]

![[Screenshot 2024-04-14 at 5.01.03 PM.png]]

![[Screenshot 2024-04-14 at 5.00.50 PM.png]]

![[Screenshot 2024-04-14 at 5.00.41 PM.png]]

![[Screenshot 2024-04-14 at 5.00.34 PM.png]]

## Replicating state across clustered node![[Screenshot 2024-04-14 at 5.08.43 PM.png]]

![[Screenshot 2024-04-14 at 5.08.36 PM.png]]

![[Screenshot 2024-04-14 at 5.08.25 PM.png]]

![[Screenshot 2024-04-14 at 5.07.37 PM.png]]

![[Screenshot 2024-04-14 at 5.07.30 PM.png]]

![[Screenshot 2024-04-14 at 5.05.25 PM.png]]

![[Screenshot 2024-04-14 at 5.05.05 PM.png]]

![[Screenshot 2024-04-14 at 5.04.53 PM.png]]

![[Screenshot 2024-04-14 at 5.04.29 PM.png]]

![[Screenshot 2024-04-14 at 5.03.59 PM.png]]

![[Screenshot 2024-04-14 at 5.03.39 PM.png]]

## Using DeltaCRDT to create a caching system

Why use Delta CRDT
- Conflict free replicated data types
- Underlying technology of Horde
- Only replicates deltas
- AP of CAP

Why use not delta CRDT
- High updates will cause network traffic
- Eventually consistent
- Hard to debug
![[Screenshot 2024-04-14 at 5.13.17 PM.png]]

![[Screenshot 2024-04-14 at 5.12.54 PM.png]]

## Distributed Hash Rings

## Using Redias as a Cache

## Distributed caching Types and trade-offs

