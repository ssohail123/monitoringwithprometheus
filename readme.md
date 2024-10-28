# Node.js Application, Kubernetes Cluster, and Node Monitoring with Prometheus and Grafana

This project demonstrates a comprehensive monitoring setup for a Node.js application deployed on a Kubernetes cluster. Using Prometheus and Grafana, we monitor application metrics, Kubernetes cluster health, and node performance. Alerts are configured to notify about critical conditions, such as high CPU usage and pod restarts.

## Overview

1. **Application Monitoring**: The Node.js application exposes custom metrics using the `prom-client` library. Key metrics include:
   - **`http_requests_total`**: Counts the total number of HTTP requests received by the application.
   - **`http_request_duration_seconds`**: Measures the duration of each HTTP request for analyzing response times.
   - **`http_request_duration_summary_seconds`**: Summarizes request duration metrics, providing insights into latency.
   - **`node_gauge_example`**: Tracks the duration of asynchronous tasks within the application.

   These metrics allow for real-time monitoring of application performance, helping identify issues related to request handling and task execution.

2. **Kubernetes Cluster Monitoring**: Prometheus collects metrics from the Kubernetes cluster, enabling monitoring of pod states, deployments, and resource usage (CPU and memory). This is crucial for maintaining cluster stability and performance.

3. **Node Monitoring**: The Prometheus Node Exporter gathers system metrics from each node, such as CPU, memory, and disk usage. This provides visibility into the underlying infrastructure, allowing for proactive management of resources.

## Achievements

- Implemented monitoring for application performance, Kubernetes cluster health, and node resource utilization.
- Configured custom metrics in the Node.js application to track HTTP requests and async task durations.
- Set up alerts for critical conditions, such as high CPU usage and pod restarts.

## Installation

1. **Install Minikube** on an Ubuntu server to create a local Kubernetes environment.
2. **Install Helm**, the package manager for Kubernetes, to facilitate the setup of Prometheus and Grafana.

### Deploying Prometheus and Grafana

1. Add and update the Helm chart repository:
   ```bash
   helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
   helm repo update
   ```

2. Install Prometheus and Grafana:
   ```bash
   helm install monitoring prometheus-community/kube-prometheus-stack -n monitoring -f ./custom_kube_prometheus_stack.yml
   ```

3. Create a namespace and deploy the Node.js application:
   ```bash
   kubectl create ns dev
   kubectl apply -k kubernetes-manifest/
   ```

4. To test alert functionality:
   - **High CPU Alert**: Stress a pod to trigger the high CPU usage alert.
   - **Pod Restart Alert**: Delete a pod multiple times to trigger the restart alert.

### Accessing Dashboards

Use port-forwarding with `0.0.0.0` for external access to the dashboards:

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

