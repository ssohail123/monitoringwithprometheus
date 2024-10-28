# Node.js Application Monitoring with Prometheus and Grafana on Kubernetes

This project demonstrates how to instrument a Node.js application with custom metrics, deploy it on a Kubernetes cluster, and monitor it with Prometheus and Grafana. Alerts are configured to notify of high CPU usage and pod restarts.

## Overview

This project includes:
- Custom Prometheus metrics for HTTP request monitoring and async task duration in a Node.js application.
- Deployment of Prometheus and Grafana on Kubernetes via Helm.
- Email alerting for CPU usage and pod restarts.

## Final Achievements

By the end of this project:
1. **Complete Monitoring Solution**: Successfully implemented a monitoring solution for both the Kubernetes cluster and the Node.js application.
2. **Custom Metrics Tracking**: Tracked HTTP request totals, durations, and async task metrics using Prometheus.
3. **Automated Alerts**: Configured and tested alert notifications to email for CPU overuse and pod restarts, ensuring reliable application performance monitoring.
4. **Visualized Metrics**: Used Grafana to visualize metrics and monitor application and infrastructure health effectively.

## Installation

1. Install Prometheus and Grafana using Helm:
   ```bash
   helm install monitoring prometheus-community/kube-prometheus-stack -n monitoring -f ./custom_kube_prometheus_stack.yml
   ```

2. Set up a namespace and deploy the application:
   ```bash
   kubectl create ns dev
   kubectl apply -k kubernetes-manifest/
   ```

## Project Setup

### Instrumenting Node.js Application

The application is instrumented using the `prom-client` library to expose custom metrics for Prometheus. The following metrics were implemented to monitor HTTP request patterns and task durations:

- **Counters**: `http_requests_total` to track the total number of HTTP requests.
- **Histogram**: `http_request_duration_seconds` to capture the duration of each HTTP request.
- **Summary**: `http_request_duration_summary_seconds` to provide a summary of request durations.
- **Gauge**: `node_gauge_example` to measure the duration of asynchronous tasks.

These metrics are made available to Prometheus by integrating them into the Node.js application using `prom-client`, allowing Prometheus to collect, store, and visualize these metrics effectively.

### Deploying Prometheus and Grafana

Helm installs the Prometheus and Grafana stack in the `monitoring` namespace, configured for monitoring both the Kubernetes cluster and custom application metrics.

### Setting Up Alert Rules

Alert rules are configured to monitor application and resource health:
- **High CPU Usage**: Triggers a warning if average CPU usage exceeds 50% for more than 5 minutes.
- **Pod Restart**: Issues a critical alert if any pod restarts more than twice.

Apply the alert rules:
```bash
kubectl apply -k alerts-alertmanager-servicemonitor-manifest/
```

## Exposing Services

To access Grafana, Alertmanager, and Prometheus on the localhost, port forwarding was set up using the `0.0.0.0` address for external access:

- **Grafana**:
  ```bash
  kubectl port-forward service/monitoring-grafana -n monitoring 8080:80 --address 0.0.0.0
  ```
- **Alertmanager**:
  ```bash
  kubectl port-forward service/alertmanager-operated -n monitoring 9093:9093 --address 0.0.0.0
  ```
- **Prometheus Dashboard**:
  ```bash
  kubectl port-forward service/monitoring-kube-prometheus-prometheus -n monitoring 9090:9090 --address 0.0.0.0
  ```

## Metrics and Alerts

- **Custom Metrics**:
  - `http_requests_total`: Total HTTP requests received by the app.
  - `http_request_duration_seconds`: Duration of each HTTP request.
  - `http_request_duration_summary_seconds`: Summary of request durations.
  - `node_gauge_example`: Measures async task duration.

- **Alerts**:
  - **HighCpuUsage**: Warning for CPU usage exceeding 50%.
  - **PodRestart**: Critical alert if a pod restarts over two times.

