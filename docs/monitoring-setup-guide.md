# AWS EC2 Monitoring Setup Guide

## 1. Project overview

Prometheus collects infrastructure metrics, evaluates alert rules, and sends alerts to Alertmanager. Alertmanager routes notifications to Slack.

## 2. Architecture

Prometheus scrapes metrics every 15 seconds. It monitors itself on port 9090, Alertmanager on port 9093, and Node Exporter on port 9100. EC2 service discovery identifies running instances in Auto Scaling Group java-app-asg in us-east-1.

## 3. Repository contents

prometheus/prometheus.yml.example is the Prometheus configuration example. prometheus/alert_rules.yml contains alert rules. alertmanager/alertmanager.yml.example contains the Alertmanager example.

## 4. Prerequisites

You need an AWS account, Prometheus, Alertmanager, Node Exporter, suitable IAM permissions for EC2 discovery, network access to port 9100, and a Slack webhook if Slack notifications are required.

## 5. Prometheus configuration

The recovered configuration uses a 15-second scrape interval, a 15-second evaluation interval, region us-east-1, and Auto Scaling Group java-app-asg. The example expects alert rules at /etc/prometheus/alert_rules.yml. Adjust paths and resource names for other environments.

## 6. Alert rules

InstanceDown detects a down Node Exporter target. HighCPUUsage evaluates CPU usage. HighDiskUsage evaluates disk usage. Review the actual expressions and thresholds in prometheus/alert_rules.yml before deploying.

## 7. Alertmanager and Slack

The example uses /etc/alertmanager/slack_webhook_url as a file containing the webhook URL and routes notifications to #aws-alerts. Create the file securely, restrict its permissions, and never commit the secret.

## 8. Validation

On the Prometheus host, run: sudo promtool check config /etc/prometheus/prometheus.yml and sudo promtool check rules /etc/prometheus/alert_rules.yml. Successful validation checks syntax and rules, not target reachability or notification delivery.

## 9. Troubleshooting

For down targets, check Node Exporter, port 9100, security groups, and Prometheus target errors. For discovery failures, check region, Auto Scaling Group, instance state, and IAM permissions. For missing Slack notifications, check Alertmanager, the webhook file, permissions, and logs.

## 10. Security and cost

Keep credentials and private keys out of Git. Use least-privilege IAM and restrict monitoring ports to trusted sources. Monitor AWS costs. Do not change or delete cloud resources without authorization.

## 11. Deployment note

This guide documents configuration recovered from the monitoring host. Review paths, IAM permissions, security groups, and notification settings before reproducing the deployment. Add Grafana exports and screenshots only after capturing or exporting and verifying them.

