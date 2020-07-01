# Very Quick Start - Prysmatic Labs Ethereum 2.0 Client

```
sudo apt -y install wget git tar g++

wget https://dl.google.com/go/go1.14.4.linux-amd64.tar.gz
sudo tar -C /usr/local -xzf go1.14.4.linux-amd64.tar.gz
git clone https://github.com/prysmaticlabs/prysm.git
cd prysm/beacon-chain
/usr/local/go/bin/go run main.go
```

Explained:

```
sudo apt -y install wget git tar g++
```
- This installs some things (which you may already have), which are required by the process

```
wget https://dl.google.com/go/go1.14.4.linux-amd64.tar.gz
```
- This downloads the latest version of `go` to your Linux computer.
- `go` is the programming language used to write @prysmaticlabs ETH 2.0 client.

```
sudo tar -C /usr/local -xzf go1.14.4.linux-amd64.tar.gz
```
- This unzips the `go` software to a commonly used place on your computer.

```
git clone https://github.com/prysmaticlabs/prysm.git
```
- This downloads the latest source code for @prysmaticlabs `prysm` client
- This is the cutting edge of the cutting edge of the cutting edge ;)

```
cd prysm/beacon-chain
```
- This changes your context to the `prysm/beacon-chain` folder
- The `beacon-chain` software

```
/usr/local/go/bin/go run main.go
```
- This uses the `go` command to run the `main.go` software for the `beacon-chain`
- This will start the process to connect to other nodes to start syncing the beacon-chain blockchain to your computer
- This beacon-chain blockchain is a base-level requirement for all validating / slashing activities
