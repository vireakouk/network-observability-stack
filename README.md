# Network observability stack

## Motivation
I want a quick way to spin up all network observability stack to monitor my networking labs with one command. This stack is meant for testing and lab environment. 

## Stack
In order to monitor my lab, I need the following tools:

| Tools         | Version | Description                                                 |
| ------------- | ------- | ----------------------------------------------------------- |
| Nautobot      | v1.1.3  | Network documentation and Single source of truth.           |
| Prometheus    | v2.30.1 | metric TSDB                                                 |
| Loki          | v2.3.0  | log TSDB                                                    |
| Telegraf      | v1.20.0 | metric collector                                            |
| Promtail      | v2.3.0  | log collector                                               |
| Grafana       | v8.1.5  | visualizer for metric and logs from Prometheus and Loki     |
| Alertmanager  | v0.23.0 | notification and alert manager                              |
| cAdvisor      | v0.36.0 | container monitoring tool                                   |

## Pre-requisites
Please ensure you have the following installed in your host or VM. 
- Docker (https://docs.docker.com/get-docker/)
- Docker-compose (https://docs.docker.com/compose/install/)

My host is Ubuntu Server 20.04 LTS x86_64 with 32GB RAM running in VirtualBox (Windows 10 Pro). This stack only consumes a bit less than 2GB of RAM.

## Getting started
Clone the repo
```
git clone https://github.com/vireakouk/network-observability-stack.git
```

The config files and environment variables are located in their respective directories. You need to edit and pre-configure those `env` and `yml` to fit your needs.
```
alertmanager/
grafana/
loki/
nautobot/
prometheus/
promtail/
telegraf/
```
For Nautobot, please follow the instruction in official documentation to configure environment variables for your specific needs.
(https://github.com/nautobot/nautobot-docker-compose)

### Spinning up the stack
Navigate to the directory
```
cd network-observability-stack
```
Bring everything up with:
```
sudo docker-compose up
```

## Usages
First you need to create a superuser for Nautobot. (https://github.com/nautobot/nautobot-docker-compose)

## Production consideration
This stack may be used for small-scale production. Please consider security, ssl, long-term storage, and high-availability. That's the topic for another day.


## References
Apart from the official documentations, the following repos helped me put together this compose:
1. https://github.com/vegasbrianc/prometheus
2. https://github.com/stefanwalther/docker-grafana
3. https://dev.to/ablx/minimal-prometheus-setup-with-docker-compose-56mp
4. https://github.com/nautobot/nautobot-docker-compose
5. https://github.com/grafana/loki/blob/main/production/docker-compose.yaml
