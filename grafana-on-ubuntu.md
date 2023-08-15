# Grafana on Ubuntu

## Installation

1. Install the required packages:
   ```bash
   sudo apt-get install -y adduser libfontconfig1
   ```

   if you have network issue, you can download the Grafana package to your local machine and copy it to the target VM. You can use `wget` to download the package and then copy it to `/tmp` on the VM.
   
   ```bash
   wget https://dl.grafana.com/oss/release/grafana_10.0.2_amd64.deb
   ```
   
   Once downloaded, copy it to the VM's `/tmp` directory.

   ```bash
   sudo cp /path/to/grafana_10.0.2_amd64.deb /tmp/
   ```
   
   Install the downloaded package:
   
   ```bash
   sudo dpkg -i /tmp/grafana_10.0.2_amd64.deb
   ```

2. Check Grafana server status and restart if needed:

   ```bash
   systemctl status grafana-server
   systemctl restart grafana-server
   ```

3. Login to Grafana by Default
   - Port: 3000
   - URL: http://<IP>:3000/
   - Username: admin
   - Password: admin

## Managed Setup of Grafana

1. Configure Grafana settings:
   Edit the Grafana configuration file:
   
   ```bash
   sudo vi /etc/grafana/grafana.ini
   ```
   
   Make necessary configuration changes and then restart Grafana:
   
   ```bash
   systemctl restart grafana-server
   systemctl status grafana-server
   ```

2. SMTP Configuration:
   In the configuration file, locate the SMTP section and enable it. Update the SMTP settings as needed:
   
   ```ini
   [smtp]
       enabled = true
       host = your-smtp-hostname
       port = your-smtp-port
       user = your-smtp-username
       password = your-smtp-password
       from_address = your-sender-email
       from_name = Grafana
   ```

3. Error Issue (database is locked):
   If you encounter the error message "failed to build query 'A': [sqlstore.max-retries-reached] retry 1: database is locked," consider adjusting the `wal` setting:
   
   ```ini
   wal = true
   ```

4. Disable Automatic Updates:
   To prevent Grafana from checking for updates automatically, add the following configuration:
   
   ```ini
   [analytics]
   check_for_updates = false
   ```

## Managed Setup of Grafana DataSource

If Prometheus on AKS hasn't set up a hostname, you might need to set up Prometheus hosts in your VM's hosts file. Edit the hosts file:

 ```ini
sudo vi /etc/hosts
 ```

Add the following line to map the Prometheus server(aks)'s IP address to a hostname:

 ```ini
IP hostname
 ```

Go to the Grafana UI and configure the URL to connect to your Prometheus data source.


## References
1. [Grafana Installation](https://grafana.com/grafana/download?edition=oss)
2. [Database is Locked Error](https://github.com/grafana/grafana/issues/64664)

