# Export OCI Metrics using Telegraf

## Prerequisites

### OCI Metrics -> OCI Streaming

Setup Service Connector Hub to export metrics from OCI Monitoring to OCI Streaming.

### Identify connection parameters to OCI Streaming

Go through the following steps to identify the required parameters to setup connection in the Telegraf `kafka_consumer` input.

## Installation

### Telegraf

Install Telegraf following the official documentation from [here](https://docs.influxdata.com/telegraf/v1/install/?t=Ubuntu+20.04+LTS+and+newer#install)

### Python dependencies

Install the required Python packages.

```
python3 -m pip install -r requirements.txt
```

if installation fails because of permission isssues, execute the command as root.

### Setup Telegraf configuration files

1. Update the Kafka connection and authentication parameters in the `input.conf` file.
2. Make the script `daemon.py` executable. Update the `daemon.py` script file path in the `filter.conf` file.
3. Update the `output.conf` with the desired output plugin or the location of the output file.
4. Copy the files: `input.conf`, `filter.conf` and `output.conf` into `/etc/telegraf/telegraf.d/` directory.
5. Disable default inputs in the `/etc/telegraf/telegraf.conf` file.

## Run

Execute the export with the command:

```
telegraf
```

For integration with Observe, you can use the following approach to configure the `http` output plugin.

```
[[outputs.http]]
  url = "https://${OBSERVE_CUSTOMER}.collect.observeinc.com/v1/http/telegraf"
  data_format = "json"
  content_encoding = "gzip"

  [outputs.http.headers]
    Authorization = "Bearer ${OBSERVE_TOKEN}"
    Content-Type = "application/json"
    X-Observe-Decoder = "nested"
```