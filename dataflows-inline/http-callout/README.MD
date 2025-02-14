# HTTP Callout Example

HTTP Callout shows how to call external HTTP servers from inside the operators. In this example, we will read `car` events and send requests to an HTTP server for additional information about the `maker`.

<p align="center">
 <img width="700" src="img/http-callout.jpg">
</p>

The `car` events are generated via an [HTTP-Source connector](connectors).

## Pipeline Primitives

The pipeline uses the following primitives:
* _map_
* _capabilities_
  * _http_


## Step-by-step

Take a look at the [dataflow.yaml](./dataflow.yaml) to get an idea of what we're doing.

Make sure to [Install SSDK & Start a Cluster].

### 1. Run the Pipeline

Run the pipeline:

```bash
 ssdk run --ui
```

Use `--ui` to open the Studio.

### 2. Test the Pipeline

#### Start the HTTP Sink Connector

In a new terminal change direcotry to `./connectors`, download the connector binary, and start 2 connectors:

```bash
cd ./connectors
cdk hub download infinyon/http-source@0.3.3
cdk deploy start --ipkg infinyon-http-source-0.3.3.ipkg -c car-callout-connector.yaml
```

To see the events, run `fluvio consume cars`.  

For additional context, checkout [connectors](./connectors/).


#### Consume form Makers

The http lookup result is sent to a `makers` topic. Consume from `makers` topic:

```bash
fluvio consume makers
```

Hit <Ctrl-C> to exit.

## Clean-up

Note that your connector will continue to run in the background until you shut them down:

```bash
cdk deploy shutdown --name car-callout
```

Exit `ssdk` terminal and remove the topics:

```bash
fluvio topic delete cars
fluvio topic delete makers
```

[Install SSDK & Start a Cluster]: /README.MD#prerequisites
