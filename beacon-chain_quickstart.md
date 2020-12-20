Start from modern 64-bit OS, and run the following in terminal:

### For eth2 Mainnet:

```
mkdir prysm && cd prysm
curl https://raw.githubusercontent.com/prysmaticlabs/prysm/master/prysm.sh --output prysm.sh && chmod +x prysm.sh
./prysm.sh beacon-chain --http-web3provider {insert the http/https URL for your eth1 RPC endpoint}
```

For the `--http-web3provider`, you should connect it to your own eth1 node syncing with mainnet.

### For eth2 Pyrmont testnet:

```
mkdir prysm && cd prysm
curl https://raw.githubusercontent.com/prysmaticlabs/prysm/master/prysm.sh --output prysm.sh && chmod +x prysm.sh
./prysm.sh beacon-chain --pyrmont --http-web3provider {insert the http/https URL for your eth1 RPC endpoint}
```

For the `--http-web3provider`, you should connect it to your own eth1 node syncing with goerli testnet.

If you don't run your own eth1 node, you can try using Prysmatic Labs' node at `https://goerli.prylabs.network` (working at time of writing).
