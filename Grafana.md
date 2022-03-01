# Grafana

Visit Grafana website at https://grafana.com/ 

Then download Grafana under self-managed section - **Standalone Linux Binaries**:


```sh
$ wget https://dl.grafana.com/enterprise/release/grafana-enterprise-8.4.2.linux-amd64.tar.gz
$ tar -zxvf grafana-enterprise-8.4.2.linux-amd64.tar.gz
```

Change to Grafana directory

```sh
$ cd grafana-8.4.2/
```

Run Grafana server that listens at port 3000

```sh
$ ./bin/grafana-server
```



