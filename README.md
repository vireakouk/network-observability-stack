# Network observability stack

## Motivation
I want a quick way to spin up all network observability stack to monitor and document my networking labs with one command. 

## Stack
In order to monitor my lab, I need the following tools:

| App           | Version | Description                                                 |
| ------------- | ------- | ----------------------------------------------------------- |
| Nautobot      | v1.1.3  | network documentation and single source of truth.           |
| Prometheus    | v2.30.1 | metric time-series database                                 |
| Loki          | v2.3.0  | log time-series database                                    |
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
sudo docker-compose up -d
Starting loki       ... done
Starting promtail   ... done
Starting redis           ... done                                                                                                                                                                                 Starting postgres   ... done
Starting nautobot     ... done
Starting cadvisor     ... done
Starting prometheus ... done
Starting alertmanager    ... done
Starting telegraf        ... done
Starting grafana         ... done
Starting nautobot-worker ... done
Starting celery_worker   ... done
```
### Verify the stack
Check if all the apps are running properly:
```
~/projects/network-observability-stack master* ❯ sudo docker-compose ps
     Name                    Command                   State                                              Ports
------------------------------------------------------------------------------------------------------------------------------------------------------
alertmanager      /bin/alertmanager --config ...   Up               0.0.0.0:9093->9093/tcp,:::9093->9093/tcp
cadvisor          /usr/bin/cadvisor -logtostderr   Up (healthy)     0.0.0.0:8080->8080/tcp,:::8080->8080/tcp
celery_worker     sh -c nautobot-server cele ...   Up (healthy)
grafana           /run.sh --config=/etc/graf ...   Up               0.0.0.0:3000->3000/tcp,:::3000->3000/tcp
loki              /usr/bin/loki --config.fil ...   Up               0.0.0.0:3100->3100/tcp,:::3100->3100/tcp
nautobot          /docker-entrypoint.sh naut ...   Up (healthy)     0.0.0.0:8000->8080/tcp,:::8000->8080/tcp, 0.0.0.0:8443->8443/tcp,:::8443->8443/tcp
nautobot-worker   nautobot-server rqworker         Up (unhealthy)
postgres          docker-entrypoint.sh postgres    Up               5432/tcp
prometheus        /bin/prometheus --web.enab ...   Up               0.0.0.0:9090->9090/tcp,:::9090->9090/tcp
promtail          /usr/bin/promtail --config ...   Up
redis             docker-entrypoint.sh sh -c ...   Up               6379/tcp
telegraf          /entrypoint.sh --config=/e ...   Up               8092/udp, 8094/tcp, 8125/udp, 0.0.0.0:9001->9001/tcp,:::9001->9001/tcp

~/projects/network-observability-stack master* ❯
```
* not sure why nautobot-worker show `unhealthy` state. Will try to find out later!.

For Nautobot, please follow the instruction in official documentation to create a superuser account.
(https://github.com/nautobot/nautobot-docker-compose)

Verify or login to each app via the following URLs:

| App           | URL                             | Default credentials                                         |
| ------------- | ------------------------------- | ----------------------------------------------------------- |
| Nautobot      | http://localhost:8000/          | superuser account created in the above step                 |
| Prometheus    | http://localhost:9090/          | N/A                                                         |
| Loki/Promtail | N/A                             | Loki & Promtail have no interface. Integrated with Grafana  |
| Telegraf      | http://localhost::9001/metrics  | This is a metric page where Prometheus scrape.              |
| Grafana       | http://localhost:3000/          | visualizer for metric and logs from Prometheus and Loki     |
| Alertmanager  | http://localhost:9093/          | N/A                                                         |
| cAdvisor      | http://localhost:8080/          | N/A                                                         |

## Production consideration
This stack is meant for testing and lab environment. For production, please consider using Docker Swarm or Kubernetes which will take care of security, long-term storage, scalability, high-availability, and disaster recovery. That's the topic for another day (learning those myself).


## References
Apart from the official documentations, the following repos helped me put together this compose:
1. https://github.com/vegasbrianc/prometheus
2. https://github.com/stefanwalther/docker-grafana
3. https://dev.to/ablx/minimal-prometheus-setup-with-docker-compose-56mp
4. https://github.com/nautobot/nautobot-docker-compose
5. https://github.com/grafana/loki/blob/main/production/docker-compose.yaml
