# Starting a Full Node

## How to Run an Onomy Testnet Full Node

Minimum system requirements:

* A Linux server with any modern Linux distribution,
* A quad-core CPU
* 8 GB of RAM
* 20GB of storage

Make sure you have gone through [Installation Steps](pre-installation-steps.md) prior to setting up a node.

### Initialize and start a full node

1. To start the Onomy full node, download scripts from the [GitHub](https://github.com/onomyprotocol/onomy/tree/dev/deploy/testnet/scripts).

```
mkdir $HOME/.onomy/scripts
cd $HOME/.onomy/scripts
wget https://raw.githubusercontent.com/onomyprotocol/onomy/dev/deploy/testnet/scripts/allow-cors.sh
wget https://raw.githubusercontent.com/onomyprotocol/onomy/dev/deploy/testnet/scripts/expose-metrics.sh
wget https://raw.githubusercontent.com/onomyprotocol/onomy/dev/deploy/testnet/scripts/init-full-node.sh
wget https://raw.githubusercontent.com/onomyprotocol/onomy/dev/deploy/testnet/scripts/start-onomyd.sh
wget https://raw.githubusercontent.com/onomyprotocol/onomy/dev/deploy/testnet/scripts/stop-onomyd.sh
```

2\. Add executable permissions to the scripts using `chmod +x *`

3\. Run init-full-node.sh script to initiate a new full node

`bash init-full-node.sh`

This script will ask you for several details

* Node Name \[Text]: A name for your full node
* Seed IPs \[Comma separated list of IPs]: IP Addresses of seed nodes, There are a couple of IPs pre-configured, If you want to point your node to custom Seed node, you can do that here
* Your IP Address \[IP Address]

After the script is done, you will find node-in in the output. Make sure to take a note of the node id as you might need it afterwards. If you forgot to save your node id, you can find it using

`onomyd tendermint show-node-id`

4\. \[Optional] you can expose metrics via Prometheus or allow CORS using the scripts `expose-metrics.sh` and `allow-cors.sh`

5\. You can start and stop the node using `start-onomyd.sh` and `stop=onomyd.sh`. When you first start the node, give it enough time to sync with the blockchain

### Check the status of the Onomy chain

You should be good to go! You can check the status of the Onomy chain by running:

```
curl http://localhost:26657/status
```

Your node is fully synced when `catching_up` is false.

You can also open the `http://localhost:26657` in your browser and check available end-points
