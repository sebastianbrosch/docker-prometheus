# Docker - Prometheus / Grafana
Monitoring with Prometheus running on a Docker Stack.

## Devices
Device | IP
:------ | :---
Raspberry Pi running Prometheus / Grafana | 192.168.0.10
Managed Switch (SNMP) | 192.168.0.100

## Querys (PromQL)
### Raspberry Pi running the Monitoring

**CPU Usage**
```
100 - (avg by (instance) (rate(node_cpu_seconds_total{mode="idle", instance="192.168.0.10:9100"}[1m])) * 100)
```
**Temperature**
```
node_thermal_zone_temp{instance="192.168.0.10:9100"}
```
**Disk Space (HDD / SSD / SD-Card)**

Free Space:
```
node_filesystem_free_bytes{mountpoint="/", instance="192.168.0.10:9100"}
```
Used Space:
```
node_filesystem_size_bytes{mountpoint="/", instance="192.168.0.10:9100"} - node_filesystem_free_bytes{mountpoint="/", instance="192.168.0.10:9100"}
```

**Memory Usage (RAM)**
```
node_memory_MemTotal_bytes{instance="192.168.0.10:9100"} - node_memory_MemAvailable_bytes{instance="192.168.0.10:9100"}
```
