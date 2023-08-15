# Grafana & Prometheus on AKS (Not in Use)

## Installation

1. Open a command-line terminal and add the Prometheus Operator Helm repository using this command:
   ```
   helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
   ```

2. Update the Helm repository:
   ```
   helm repo update
   ```

3. Create a specific namespace:
   ```
   kubectl create namespace monitor
   ```

4. Install Prometheus Operator:
   ```
   helm install prometheus-operator prometheus-community/kube-prometheus-stack --namespace monitor
   ```

Upon executing these commands, Helm will deploy the Prometheus Operator and its related components in the designated namespace. 

## Configure SMTP
To set up SMTP in Grafana for sending email alerts, follow these steps:

1. Edit the ConfigMap of `prometheus-operator-grafana` and add the SMTP configuration:

   ```yaml
   apiVersion: v1
   data:
     grafana.ini: |
      ...
   
       [smtp]
       enabled = true
       host = your-smtp-hostname
       port = your-smtp-port
       user = your-smtp-username
       password = your-smtp-password
       from_address = your-sender-email
       from_name = Grafana
   ```

2. Fill in your SMTP settings:

   - `your-smtp-hostname`: Replace with your SMTP server's hostname.
   - `your-smtp-port`: Replace with your SMTP server's port (usually 25 or 587).
   - `your-smtp-username`: Replace with your SMTP server's username (if authentication is required).
   - `your-smtp-password`: Replace with your SMTP server's password (if authentication is required).
   - `your-sender-email`: Replace with the email address you want to use as the sender.

3. Save and apply the updated ConfigMap settings:

   ```bash
   kubectl apply -f prometheus-operator-grafana.yaml
   ```

4. Use this command to query the Grafana Deployment:

   ```bash
   kubectl get deployments -n monitor
   ```

5. Restart the Grafana Deployment:

   ```bash
   kubectl rollout restart deployment <grafana-deployment-name> -n monitor
   ```

6. Kubernetes will restart the Grafana Deployment, causing a short service interruption. Ensure you've saved the ConfigMap changes and applied the new configuration before restarting. After this, Grafana will utilize the new SMTP settings for sending email alerts.

## Grafana UI on AKS

1. Verify the service: prometheus-operator-grafana.
2. Confirm that the service type is NodePort.
3. Access the URL.
4. Use the Account/Password: admin/prom-operator.

## Prometheus-operated UI

1. Create an ingress:
   ```bash
   kubectl -n monitor apply -f ./prometheus-operated-ingress.yml
   ```

2. Update your host file (e.g., C:\Windows\System32\drivers\etc\hosts) and add an IP and Domain mapping. For example:
   ```
   IP    hostname
   ```

For upgrading Prometheus Operator, use this Helm command:
```bash
helm upgrade prometheus-operator -n <prometheus-namespace> prometheus-community/kube-prometheus-stack --recreate-pods
```

Replace `<prometheus-namespace>` with the appropriate namespace where Prometheus is installed.

For more details and configuration options, refer to the [Prometheus Operator Helm Chart documentation](https://github.com/prometheus-community/helm-charts/tree/main/charts/kube-prometheus-stack).
