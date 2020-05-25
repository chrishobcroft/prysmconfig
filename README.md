# prysmconfig

To store config and notes about how to build and operate an ETH 2.0 Beacon and Validator node

## Running Beacon and Validator (Single Server)

### Get a server

Best is to do it on your localhost.
- then you'll need to be logged in as a user called `ubuntu` with sudo privileges, or edit the files

Exoscale
Medium (4GB, 2 x 2198 MHz, 50 GB)

Also maybe try Digital Ocean

## Network Settings

Open the following ports in the firewall to allow INGRESS `tcp` connections:

- `22` - to allow `ssh` connections
- `13000` - to allow improved p2p connectivity
- `3000` - to allow remote access to Grafana
- `4000` - to allow remote access to the Beacon Node

## Installation Commands

```
cd ~
sudo echo
sudo apt update -y
sudo apt upgrade -y

mkdir prysm && cd prysm 

curl https://raw.githubusercontent.com/prysmaticlabs/prysm/master/prysm.sh --output prysm.sh && chmod +x prysm.sh 

cd ~

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
rm prometheus-2.18.1.linux-amd64.tar.gz

cd prometheus-2.18.1.linux-amd64
rm prometheus.yml
wget https://raw.githubusercontent.com/chrishobcroft/prysmconfig/master/prometheus.yml
cd ~

wget https://dl.grafana.com/oss/release/grafana-6.7.3.linux-amd64.tar.gz

tar -zxvf grafana-6.7.3.linux-amd64.tar.gz
rm grafana-6.7.3.linux-amd64.tar.gz

sudo fallocate -l 4G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
sudo nano /etc/fstab
```
Add this to `/etc/fstab`
```
/swapfile swap swap defaults 0 0
```
then continue
```
sudo systemctl start grafana.service
sudo systemctl start prometheus.service
```

- log in to http://{ip-address}:3000/login admin/admin (first time load will be slow)
  - add a **Prometheus** data source with URL `http://localhost:9090` and click "Save & Test" to see "Data source is working"
  - import new dashboard from here: https://github.com/GuillaumeMiralles/prysm-grafana-dashboard#creatingimporting-dashboards

```
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

- Wait for beacon node to sync

- Apply / Generate Keys
  - Note, in absence of a key, the process above will generate a key - delete this.
