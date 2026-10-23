# AWS EC2 Monitoring with Prometheus, Alertmanager and Grafana

## Project Overview

This project demonstrates an infrastructure monitoring solution for an AWS EC2 application environment. It brings together Prometheus, Node Exporter, Alertmanager, Grafana, and Slack notification integration.

The goal is to monitor application-instance health, collect operating-system metrics, identify potential infrastructure issues, and provide visibility through dashboards and alerts.

## Architecture

The solution includes the following components:

| Component | Purpose |
|---|---|
| Amazon EC2 | Hosts the monitoring server and application instances |
| Auto Scaling Group | Manages the application EC2 instances |
| Node Exporter | Exposes operating-system metrics for Prometheus |
| Prometheus | Scrapes metrics, stores time-series data, and evaluates alert rules |
| Alertmanager | Groups, routes, and manages alert notifications |
| Slack | Receives notifications through the configured webhook integration |
| Grafana | Visualizes metrics through dashboards |

### Monitoring Flow

1. Node Exporter exposes system metrics from monitored EC2 instances.
2. Prometheus scrapes metrics from configured targets and evaluates alert rules.
3. When an alert condition is met, Prometheus sends the alert to Alertmanager.
4. Alertmanager routes eligible notifications to the configured Slack receiver.
5. Grafana queries Prometheus to display infrastructure metrics.

The Prometheus configuration includes AWS EC2 service discovery for running instances associated with the configured application Auto Scaling Group.

See [the monitoring setup guide](docs/monitoring-setup-guide.md) for configuration and deployment details.

## Dashboard

The repository includes a starter Grafana dashboard definition:

`grafana/dashboards/aws-ec2-monitoring.json`

The dashboard template contains panels intended to display:

- Healthy application instances
- CPU usage by application instance
- Memory usage by application instance
- Disk usage by application instance

The JSON file is a dashboard template. Import it into Grafana and verify the Prometheus data source and queries before relying on the panels in a live environment.

## Alerting

The Prometheus alert rules in `prometheus/alert_rules.yml` cover:

- **InstanceDown:** Detects unavailable monitoring targets.
- **HighCPUUsage:** Detects high CPU utilization.
- **HighDiskUsage:** Detects high disk utilization.

Alert conditions depend on the configured expressions, thresholds, and evaluation periods. Alertmanager routing and Slack delivery must be verified in the target environment.

## Repository Structure

```text
aws-ec2-monitoring-prometheus-grafana/
├── README.md
├── .gitignore
├── alertmanager/
│   └── alertmanager.yml.example
├── docs/
│   ├── monitoring-setup-guide.md
│   └── screenshots/
├── grafana/
│   └── dashboards/
│       └── aws-ec2-monitoring.json
└── prometheus/
    ├── prometheus.yml.example
    └── alert_rules.yml
