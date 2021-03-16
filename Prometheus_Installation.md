![prometheushero](https://user-images.githubusercontent.com/44292083/74180194-f9233300-4c36-11ea-89f5-8c8027a95026.jpg)
# Preparing for the installation


The first thing we must do is create the necessary users and directories for Prometheus. You will also need to have NGINX installed on the system. To install NGINX, issue the following commands from a terminal window:
```sh
sudo apt install nginx

sudo systemctl start nginx

sudo systemctl enable nginx
```
With NGINX installed, you can then begin the process of creating special users (they will be service-only users, not users that can log into the system). Back at your terminal window, issue the following commands:
```sh
sudo useradd --no-create-home --shell /bin/false prometheus

sudo useradd --no-create-home --shell /bin/false node_exporter
```
Next we create the necessary directories that will be used to store files and data. Issue the following commands to create the directories and give them the correct permissions:
```sh
sudo mkdir /etc/prometheus

sudo mkdir /var/lib/prometheus

sudo chown prometheus:prometheus /etc/prometheus

sudo chown prometheus:prometheus /var/lib/prometheus
```
## Download and install

We're ready to download and install Prometheus. Back at your terminal window, issue the following commands to download the necessary file:
```sh
cd 

curl -LO https://github.com/prometheus/prometheus/releases/download/v2.0.0/prometheus-2.0.0.linux-amd64.tar.gz
```
Unpack that downloaded file with the command:
```sh
tar xvf prometheus-2.0.0.linux-amd64.tar.gz
```
Next we need to copy the binary files into the correct locations and fix the permissions. This is done with the following four commands:
```sh
sudo cp prometheus-2.0.0.linux-amd64/prometheus /usr/local/bin/

sudo cp prometheus-2.0.0.linux-amd64/promtool /usr/local/bin/

sudo chown prometheus:prometheus /usr/local/bin/prometheus

sudo chown prometheus:prometheus /usr/local/bin/promtool
```
With the binaries in place, there are libraries that need to next be copied and ownership fixed. Issue the following four commands:

```sh
sudo cp -r prometheus-2.0.0.linux-amd64/consoles /etc/prometheus

sudo cp -r prometheus-2.0.0.linux-amd64/console_libraries /etc/prometheus

sudo chown -R prometheus:prometheus /etc/prometheus/consoles

sudo chown -R prometheus:prometheus /etc/prometheus/console_libraries
```

# Configure and run Prometheus

A new configuration file must be created. Issue the command:

-sudo nano /etc/prometheus/prometheus.yml.

In that file add the following content:

```js
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: 'prometheus'
    scrape_interval: 5s
    static_configs:
      - targets: ['localhost:9090']
      
```
     
The above configuration will set up a scrape every 15 seconds. Save and close that file (we'll be coming back to it in a moment).


We now need to create a file for the systemd service. Issue the command:
```sh
sudo nano /etc/systemd/system/prometheus.service
```
In that new file, add the following content:

```js
[Unit]
Description=Prometheus
Wants=network-online.target
After=network-online.target

[Service]
User=prometheus
Group=prometheus
Type=simple
ExecStart=/usr/local/bin/prometheus \
    --config.file /etc/prometheus/prometheus.yml \
    --storage.tsdb.path /var/lib/prometheus/ \
    --web.console.templates=/etc/prometheus/consoles \
    --web.console.libraries=/etc/prometheus/console_libraries

[Install]
WantedBy=multi-user.target

```

Save and close that file. Reload systemd with the command sudo systemctl daemon-reload. With systemd reloaded, you can now start and enable Prometheus with the following two commands:

```sh
sudo systemctl start prometheus
sudo systemctl enable prometheus
```

If you point your browser to http://SERVER_IP:9090 (where SERVER_IP is the actual IP address of your server), you should now see the Prometheus site .

# Adding an exporter
As I mentioned, we're going to add node_exporter. The first thing you must down download the requisite file with the following two commands:

```sh
cd

curl -LO https://github.com/prometheus/node_exporter/releases/download/v0.15.1/node_exporter-0.15.1.linux-amd64.tar.gz
```

Extract the file with the following command:

```sh
tar xvf node_exporter-0.15.1.linux-amd64.tar.gz
```

Copy the binary file into the necessary directory, and give it the proper permissions with the following commands:

```sh
sudo cp node_exporter-0.15.1.linux-amd64/node_exporter /usr/local/bin

sudo chown node_exporter:node_exporter /usr/local/bin/node_exporter
```
Next we need to create a systemd file for node_exporter. Issue the command:
```sh
sudo nano /etc/systemd/system/node_exporter.service

```
Then add the following to the newly created file:

```js
[Unit]
Description=Node Exporter
Wants=network-online.target
After=network-online.target

[Service]
User=node_exporter
Group=node_exporter
Type=simple
ExecStart=/usr/local/bin/node_exporter

[Install]
WantedBy=multi-user.target
```

Save and close that file. Reload systemd with the command sudo systemctl daemon-reload and then start and enable node_explorer with the following commands:

```sh
sudo systemctl start node_explorer

sudo systemctl enable node_explorer
```

Configure Prometheus to scrape node_exporter
Let's go back to the Prometheus configuration file. Open that with the command sudo nano /etc/prometheus/prometheus.yml. At the end of that file, add the following:

```sh
 - job_name: 'node_exporter'
    scrape_interval: 5s
    static_configs:
      - targets: ['localhost:9100']
Save and close that file. Restart Prometheus with the command:

sudo systemctl restart prometheus
```

At this point, Prometheus is now scraping data from node_export. If you point your browser back to http://SERVER_IP:9090 (where SERVER_IP is the IP address of the server), you can select one of the many data queries from the drop-down and execute it to reveal a graph, based on the data scraped.

Special thanks to https://www.techrepublic.com/article/how-to-install-the-prometheus-monitoring-system-on-ubuntu-16-04/
