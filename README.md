# AWS EC2 Monitoring with Prometheus, Alertmanager and Grafana

## Project Overview

This project demonstrates infrastructure monitoring for an AWS EC2 application environment using Prometheus, Node Exporter, Alertmanager, Grafana, and Slack notifications.

The objective is to collect infrastructure metrics, visualize application-instance health, and configure alerts for potential system issues.

## Architecture

The monitoring architecture includes:

- **Amazon EC2:** Hosts the monitoring server and application instances.
- **Auto Scaling Group:** Manages the application EC2 instances.
- **Node Exporter:** Exposes operating-system metrics on port 9100.
- **Prometheus:** Collects and stores metrics and evaluates alert rules.
- **Alertmanager:** Handles alert routing and notification delivery.
- **Slack:** Receives monitoring notifications.
- **Grafana:** Displays infrastructure metrics through dashboards.

See the architecture and setup descriptions in `docs/monitoring-setup-guide.md`.

## Dashboard Metrics

The starter Grafana dashboard at `grafana/dashboards/aws-ec2-monitoring.json` contains panels for:

1. Healthy application instances
2. CPU usage by application instance
3. Memory usage by application instance
4. Disk usage by application instance

## Alerting

The configured Prometheus alert rules cover:

- Instance availability
- High CPU usage
- High disk usage

Alertmanager routes notifications to Slack. Refer to the included configuration examples and setup guide for details.

## Repository Structure

```text
aws-ec2-monitoring-prometheus-grafana/
├── README.md
├── .gitignore
├── docs/
│   ├── monitoring-setup-guide.md
│   └── screenshots/
├── prometheus/
│   ├── prometheus.yml.example
│   └── alert_rules.yml
├── alertmanager/
│   └── alertmanager.yml.example
└── grafana/
    └── dashboards/aws-ec2-monitoring.json
```

## Setup and Security

Read `docs/monitoring-setup-guide.md` before deployment. Review configuration paths, IAM permissions, network access, and secret handling. Never commit webhook URLs, credentials, private keys, or other secrets. Live dashboard import and notification delivery should be verified separately.
