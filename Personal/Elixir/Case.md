# Energy trading

There is a data provider that sends us data using AMPQ and we send back few events to initiate trade. The data coming from the data provider has couple types that we have documentation for and example XMLs as well.

## Services

I was thkinging the following structure, each of them is a simple Genserver with few functions.

- AMPQ Consumer (handling events like disconnect, connection close, etc. and event XMLs)
- Persistent Layer
  - SQL: creating a new file every day?
  - Disk: JSON 
- SerDe (from_xml, to_json, to_sql)
- Telemetry (messages sent / s , received / s , throttling budget, etc.)
- Simple Web UI to see the metrics we have in Telemetry
  - maybe use Prometheus to store telemetry data? 


## Extra info

We need our order book every couple of hours and the trading events saved as a continous stream.