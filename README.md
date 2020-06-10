# prysmconfig

To store config and notes about how to build and operate an ETH 2.0 Beacon and Validator node

## Running Beacon and Validator (Single Server)

### Get a server

Best is to do it on your localhost.
- then you'll need to be logged in as a user called `ubuntu` with sudo privileges, or edit the files

Exoscale
Medium (512Mb, 1 x 2198 MHz, 10 GB)

Also maybe try Digital Ocean

## Network Settings

Open the following ports in the firewall to allow INGRESS `tcp` connections:

- `22` - to allow `ssh` connections
- `13000` - to allow improved p2p connectivity
- `3000` - to allow remote access to Grafana
- `4000` - to allow remote access to the Beacon Node

## Installation Commands

```
sudo fallocate -l 8G /swapfile
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
sudo apt update -y && sudo apt upgrade -y
wget https://dl.google.com/go/go1.14.4.linux-amd64.tar.gz
tar -C /usr/local -xzf go1.14.4.linux-amd64.tar.gz
rm go1.14.4.linux-amd64.tar.gz
export PATH=$PATH:/usr/local/go/bin
sudo nano ~/.profile
```
Add this to .profile
```
export PATH=$PATH:/usr/local/go/bin
```
and then continue:
```
cd ~
git clone git@github.com:prysmaticlabs/prysm.git

cd ~/prysm/beacon-chain
go build
cd ~/prysm/validator
go build
cd ~/prysm/slasher
go build

cd ~/prysm/validator
./validator accounts create
```
Make deposits at https://prylabs.net/participate
```

cd ~

wget https://raw.githubusercontent.com/chrishobcroft/prysmconfig/master/beacon.service
wget https://raw.githubusercontent.com/chrishobcroft/prysmconfig/master/validator.service
wget https://raw.githubusercontent.com/chrishobcroft/prysmconfig/master/slasher.service

sudo mv *.service /etc/systemd/system

sudo systemctl enable /etc/systemd/system/beacon.service
sudo systemctl enable /etc/systemd/system/validator.service
sudo systemctl enable /etc/systemd/system/slasher.service





wget https://raw.githubusercontent.com/chrishobcroft/prysmconfig/master/prometheus.service
sudo mv prometheus.service /etc/systemd/system

sudo systemctl enable /etc/systemd/system/prometheus.service

wget https://github.com/prometheus/prometheus/releases/download/v2.18.1/prometheus-2.18.1.linux-amd64.tar.gz

tar -zxvf prometheus-2.18.1.linux-amd64.tar.gz
rm prometheus-2.18.1.linux-amd64.tar.gz

cd prometheus-2.18.1.linux-amd64
rm prometheus.yml
wget https://raw.githubusercontent.com/chrishobcroft/prysmconfig/master/prometheus.yml

sudo systemctl start prometheus.service




wget https://raw.githubusercontent.com/chrishobcroft/prysmconfig/master/grafana.service
sudo mv grafana.service /etc/systemd/system
sudo systemctl enable /etc/systemd/system/grafana.service

wget https://dl.grafana.com/oss/release/grafana-7.0.1.linux-amd64.tar.gz

tar -zxvf grafana-7.0.1.linux-amd64.tar.gz
rm grafana-7.0.1.linux-amd64.tar.gz

sudo systemctl start grafana.service
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
sudo journalctl -f --unit=slasher.service
sudo journalctl -f --unit=prometheus.service
sudo journalctl -f --unit=grafana.service
```

- Wait for beacon node to sync

- Apply / Generate Keys
  - Note, in absence of a key, the process above will generate a key - delete this.
