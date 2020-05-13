## Running Validator only (remote Beacon Chain)

### Get a server

It will work on a "Micro" instance, with 512Mb RAM and 1 vCPU - however you will need to add 300Mb of swap, for keygen, raw txn data gen, and validator process startup.

## Network Settings

Open the following ports in the firewall to allow INGRESS `tcp` connections:

- `22` - to allow `ssh` connections
- `3000` - to allow remote access to Grafana

## Installation Commands

```
cd ~
sudo echo
sudo apt update -y
sudo apt upgrade -y

sudo fallocate -l 300M /swapfile
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

mkdir prysm && cd prysm 

curl https://raw.githubusercontent.com/prysmaticlabs/prysm/master/prysm.sh --output prysm.sh && chmod +x prysm.sh 

./prysm.sh validator accounts create --password changeme --keystore-path /home/ubuntu/.eth2validators

cd ~

wget https://raw.githubusercontent.com/chrishobcroft/prysmconfig/master/validator.service
wget https://raw.githubusercontent.com/chrishobcroft/prysmconfig/master/prometheus.service
wget https://raw.githubusercontent.com/chrishobcroft/prysmconfig/master/grafana.service

sudo nano validator.service
```
Add `--beacon-rpc-provider 89.145.162.87:4000`
```
sudo mv *.service /etc/systemd/system

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

sudo reboot
```

Notes

./prysm.sh validator --password changeme

- TO DO: Telegram Bot Alerting stuff

- Tail logs with
```
sudo journalctl -f --unit=validator.service
sudo journalctl -f --unit=prometheus.service
sudo journalctl -f --unit=grafana.service
```

- log in to http://{ip-address}:3000/login admin/admin (first time load will be slow)
  - add a **Prometheus** data source with URL `http://localhost:9090` and click "Save & Test" to see "Data source is working"
  - import new dashboard from https://github.com/GuillaumeMiralles/prysm-grafana-dashboard stuff

- Apply / Generate Keys
  - Note, in absence of a key, the process above will generate a key - delete this.
