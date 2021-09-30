# Network observability stack

# Motivation
I want a quick way to spin up all network observability stack to monitor my networking labs. This stack is meant for testing and lab environment. It is not meant for production environment.

# Tools
In order to monitor my lab, I need the following tools:
1. Nautobot: Network documentation and Single source of truth
2. Prometheus: metric TSDB
3. Loki: log TSDB
4. Telegraf: metric collector
5. Promtail: log collector
6. Grafana: visualizer for metric and logs from Prometheus and Loki
7. Alertmanager: notification and alert manager
8. cAdvisor: container monitoring tool
