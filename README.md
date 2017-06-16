# prometheus-tunnel
Since Prometheus is pull based, it needs to be able to connect to the exporters.

Sometimes the exporters might not be reachable because they are behind NAT,
undiscoverable or unreachable for other reasons.

With prometheus-tunnel you can forward a local exporter to a remote Prometheus
server and configure the forwarded port as scrape target.

## Example
Let say you run the [node-exporter](https://github.com/prometheus/prometheus) on
it's default port 9100 on some system your Prometheus server can't scrape.

First you need to enable the
[file based service discovery](https://prometheus.io/docs/operating/configuration/#<file_sd_config>)
in Prometheus by adding a scrape config like this:

```
scrape_configs:
  - job_name: 'prometheus-tunnel'
    file_sd_configs:
      - files:
        - "/tmp/prometheus_sd_*.yaml"
```

To forward the local node-exporter and make your Prometheus server scrape it run:

```
./prometheus-tunnel prometheus.example.com:/tmp/prometheus_sd_foo.yaml 9100 remote_host=foo
```

This forwards the exporter port to your Prometheus server and generates a
config in `/tmp/prometheus_sd_foo.yaml`.
