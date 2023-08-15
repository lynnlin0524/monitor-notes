# Prometheus on AKS

## Installation

1. Open a command-line terminal on Rancher and add the Prometheus Operator Helm repository using the following command:
   ```bash
   helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
   ```

2. Update the Helm repository:
   ```bash
   helm repo update
   ```

3. Create a specific namespace:
   ```bash
   kubectl create namespace monitor
   ```

4. Install Prometheus Operator:
   ```bash
   helm install prometheus-operator prometheus-community/kube-prometheus-stack --namespace monitor
   ```

After executing the above commands, Helm will deploy Prometheus Operator and related components in the specified namespace.

Once the deployment is complete, you can view and manage Prometheus Operator-related resources in the Rancher interface.

## Prometheus-operated UI

1. Create an ingress:
   ```bash
   kubectl -n monitor apply -f ./prometheus-operated-ingress.yml
   ```

2. Update your host file (e.g., C:\Windows\System32\drivers\etc\hosts) and add an IP and Domain mapping. For example:
   ```
   IP    hostname
   ```

3. The configured ingress can be set as a data source in Grafana.

## Updating Prometheus Operator

To upgrade Prometheus Operator, use the following Helm command:
```bash
helm upgrade prometheus-operator -n <prometheus-namespace> prometheus-community/kube-prometheus-stack --recreate-pods
```

Replace `<prometheus-namespace>` with the appropriate namespace where Prometheus is installed.

For more details and configuration options, refer to the [Prometheus Operator Helm Chart documentation](https://github.com/prometheus-community/helm-charts/tree/main/charts/kube-prometheus-stack).
```

Feel free to copy and paste this content into your README.md file. Adjust any paths, hostnames, or specific details as needed for your setup.