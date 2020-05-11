# prysmconfig
To store config and notes about how to build and operate an ETH 2.0 Beacon and Validator node

## Get a server

Exoscale

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

- need to change the IP address in /etc/systemd/system/beacon.service

- log in to http://{ip-address}:3000/login admin/admin
  - add a Prometheus data source http://localhost:9090
  - import dashboard config from https://raw.githubusercontent.com/GuillaumeMiralles/prysm-grafana-dashboard/master/more_10_validators.json
  
- Wait for beacon node to sync

- Apply keys
