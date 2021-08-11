# Docker - Prometheus / Grafana
Monitoring with Prometheus running on a Docker Stack.

## Used Docker Images
 - Prometheus ([Docker Hub](https://hub.docker.com/r/prom/prometheus) - [GitHub](https://github.com/prometheus/prometheus))
 - SNMP exporter ([Docker Hub](https://hub.docker.com/r/prom/snmp-exporter) - [GitHub](https://github.com/prometheus/snmp_exporter))
 - Grafana ([Docker Hub](https://hub.docker.com/r/grafana/grafana) - [GitHub](https://github.com/grafana/grafana))
 - Speedtest exporter ([Docker Hub](https://hub.docker.com/r/billimek/prometheus-speedtest-exporter) - [GitHub](https://github.com/billimek/prometheus-speedtest-exporter))
 - Blackbox exporter ([Docker Hub](https://hub.docker.com/r/prom/blackbox-exporter) - [GitHub](https://github.com/prometheus/blackbox_exporter))

## Devices
Device | IP
:------ | :---
Raspberry Pi running Prometheus / Grafana | 192.168.0.10
Managed Switch (SNMP) | 192.168.0.100
Printer (SNMP) | 192.168.0.150

## Querys (PromQL)
### Raspberry Pi running the Monitoring

#### CPU Usage
```
100 - (avg by (instance) (rate(node_cpu_seconds_total{mode="idle", instance="192.168.0.10:9100"}[30s])) * 100)
```

#### Temperature

```
avg(node_hwmon_temp_celsius{instance="192.168.0.10:9100"})
```
or
```
node_thermal_zone_temp{instance="192.168.0.10:9100"}
```

#### Disk Space

Available Space:
```
node_filesystem_avail_bytes{mountpoint="/", instance="192.168.0.10:9100"}
```

Used Space:
```
node_filesystem_size_bytes{mountpoint="/", instance="192.168.0.10:9100"} - node_filesystem_avail_bytes{mountpoint="/", instance="192.168.0.10:9100"}
```

#### Memory Usage (RAM)

```
node_memory_MemTotal_bytes{instance="192.168.0.10:9100"} - node_memory_MemAvailable_bytes{instance="192.168.0.10:9100"}
```

#### Network Traffic on Ethernet (eth0)

Received
```
rate(node_network_receive_bytes_total{device="eth0", instance="192.168.0.10:9100"}[30s])*8
```

Sent
```
rate(node_network_transmit_bytes_total{device="eth0", instance="192.168.0.10:9100"}[30s])*8
```

## Reloading Prometheus configuration

After changing the Prometheus configuration you have to reload Prometheus, using this command:

```
docker exec $(docker ps -q --filter "name=prometheus\.") killall -HUP prometheus
```

## Clean-Up

Remove unused images:
```
docker image prune --all
```

Remove unused container:
```
docker container prune
```
