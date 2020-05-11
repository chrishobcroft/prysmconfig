# prysmconfig

To store config and notes about how to build and operate an ETH 2.0 Beacon and Validator node

## Get a server

Best is to do it on your localhost.
- then you'll need to be logged in as a user called `ubuntu` with sudo privileges, or edit the files

Exoscale
Medium (4GB, 2 x 2198 MHz, 50 GB)

Also maybe try Digital Ocean

Open ports `22`, `13000`, `3000` in the firewall to allow INGRESS `tcp` connections

## Commands

```
cd ~
sudo echo
sudo apt update -y
sudo apt upgrade -y
sudo apt dist-upgrade -y

mkdir prysm && cd prysm 

curl https://raw.githubusercontent.com/prysmaticlabs/prysm/master/prysm.sh --output prysm.sh && chmod +x prysm.sh 

wget https://raw.githubusercontent.com/chrishobcroft/prysmconfig/master/beacon.service
wget https://raw.githubusercontent.com/chrishobcroft/prysmconfig/master/validator.service
wget https://raw.githubusercontent.com/chrishobcroft/prysmconfig/master/prometheus.service
wget https://raw.githubusercontent.com/chrishobcroft/prysmconfig/master/grafana.service

sudo mv *.service /etc/systemd/system

sudo systemctl enable /etc/systemd/system/beacon.service
sudo systemctl enable /etc/systemd/system/validator.service
sudo systemctl enable /etc/systemd/system/prometheus.service
sudo systemctl enable /etc/systemd/system/grafana.service

cd ~

wget https://github.com/prometheus/prometheus/releases/download/v2.18.1/prometheus-2.18.1.linux-amd64.tar.gz

tar -zxvf prometheus-2.18.1.linux-amd64.tar.gz

cd prometheus-2.18.1.linux-amd64
rm prometheus.yml
wget https://raw.githubusercontent.com/chrishobcroft/prysmconfig/master/prometheus.yml
cd ~

wget https://dl.grafana.com/oss/release/grafana-6.7.3.linux-amd64.tar.gz

tar -zxvf grafana-6.7.3.linux-amd64.tar.gz

sudo reboot
```

Notes

- TO DO: Telegram Bot Alerting stuff

- Tail logs with
```
sudo journalctl -f --unit=beacon.service
sudo journalctl -f --unit=validator.service
sudo journalctl -f --unit=prometheus.service
sudo journalctl -f --unit=grafana.service
```

- log in to http://{ip-address}:3000/login admin/admin (first time load will be slow)
  - add a **Prometheus** data source with URL `http://localhost:9090` and click "Save & Test" to see "Data source is working"
  - import new dashboard using this as json https://raw.githubusercontent.com/GuillaumeMiralles/prysm-grafana-dashboard/master/more_10_validators.json

- Wait for beacon node to sync

- Apply / Generate Keys
  - Note, in absence of a key, the process above will generate a key - delete this.
