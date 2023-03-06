# Starting a Full Node

## How to Run an Onomy Full Node

Minimum system requirements:

* Any modern Linux distribution (RHEL 8 or Fedora 36 are preferred)
* A quad-core CPU
* 16 GB of RAM
* 320 GB of storage

Make sure you have gone through [Installation Steps](pre-installation-steps.md) prior to setting up a node.

There are a couple of ways to initialize and start a full node

1. [Initialize and start a full node using scripts](setting-up-a-full-node.md#initialize-and-start-a-full-node)
2. Initialize and start a full node manually

## Initialize and start a full node using scripts

1. To start the Onomy full node, download scripts from the [GitHub](https://github.com/onomyprotocol/onomy/tree/main/deploy/scripts).

```
mkdir $HOME/.onomy/scripts
cd $HOME/.onomy/scripts
wget https://raw.githubusercontent.com/onomyprotocol/onomy/main/deploy/scripts/allow-cors.sh
wget https://raw.githubusercontent.com/onomyprotocol/onomy/main/deploy/scripts/init-mainnet-full-node.sh
wget https://raw.githubusercontent.com/onomyprotocol/onomy/main/deploy/scripts/start-onomyd.sh
wget https://raw.githubusercontent.com/onomyprotocol/onomy/main/deploy/scripts/stop-onomyd.sh
```

2\. Add executable permissions to the scripts using `chmod +x *`

3\. Download genesis file

```
wget https://raw.githubusercontent.com/onomyprotocol/onomy/main/genesis/mainnet/genesis-mainnet-1.json
```

4\. Run init-mainnet-full-node.sh script to initiate a new full node

`bash init-mainnet-full-node.sh`

This script will ask you for several details

* Node Name \[Text]: A name for your full node
* Seed IPs \[Comma separated list of IPs]: IP Addresses of seed nodes, There are a couple of IPs pre-configured, If you want to point your node to custom Seed node, you can do that here
* Your IP Address \[IP Address]

After the script is done, you will find node-id in the output. Make sure to take a note of the node id as you might need it afterwards. If you forgot to save your node id, you can find it using

`onomyd tendermint show-node-id`

4\. \[Optional] you can allow CORS using the script `allow-cors.sh`

5\. You can start and stop the node using `start-onomyd.sh` and `stop-onomyd.sh`. When you first start the node, give it enough time to sync with the blockchain

## Initialize and start a full node manually

1. Initialize onomy node and config files&#x20;

`onomyd init <node name> --chain-id onomy-mainnet-1`

2. Copy Genesis file from onomy repo&#x20;

If Onomy Github repository was downloaded in previous step, copy genesis file from the repo

`cp  onomy/genesis/genesis-mainnet-1.json $HOME/.onomy/config/genesis.json`

Otherwise, download genesis file using wget

```
wget https://raw.githubusercontent.com/onomyprotocol/onomy/main/genesis/mainnet/genesis-mainnet-1.json
cp genesis-mainnet-1.json $HOME/.onomy/config/genesis.json
```

3. Change config to add various details

```
crudini --set $HOME/.onomy/config/config.toml p2p addr_book_strict false
crudini --set $ HOME/.onomy/config/config.toml p2p external_address "\"tcp://<ip-address>:26656\""
crudini --set $ HOME/.onomy/config/config.toml p2p seeds "\”211535f9b799bcc8d46023fa180f3359afd4c1d3@44.213.44.5:26656, cd9a47cebe8eef076a5795e1b8460a8e0b2384e5@3.210.0.126:26656\”"
crudini --set $ HOME/.onomy/config/config.toml rpc laddr "\"tcp://0.0.0.0:26657\""
  
crudini --set $HOME/.onomy/config/app.toml grpc enable true
crudini --set $HOME/.onomy/config/app.toml grpc address "\”0.0.0.0:9191\”"
crudini --set $HOME/.onomy/config/app.toml api enable true
crudini --set $HOME/.onomy/config/app.toml api swagger true
```

4. Setup state-sync (Optional)&#x20;

We can setup state-sync to quickly sync up the node

These are the IP addresses for state sync nodes: `52.70.182.125, 44.195.221.88`

Open one RPC endpoint of one of the state-sync nodes to get the details of latest block: [http://52.70.182.125:26657/block](http://52.70.182.125:26657/block).&#x20;

* Get latest block height from result.block.header.height parameter and latest block hast from result.block\_id.hash parameter&#x20;
* Change the following parameters in config.toml file&#x20;

```
crudini --set $ONOMY_NODE_CONFIG statesync enable true 
crudini --set $ONOMY_NODE_CONFIG statesync rpc_servers "\" c213f678b9e3b7c37b9229318b3e27b95c9d5af4@52.70.182.125:26657,00ce2f84f6b91639a7cedb2239e38ffddf9e36de@44.195.221.88:26657\"" 
crudini --set $ONOMY_NODE_CONFIG statesync trust_height <block height from previous step> 
crudini --set $ONOMY_NODE_CONFIG statesync trust_hash "\" <block hash from previous step> \"" 
crudini --set $ONOMY_NODE_CONFIG statesync discovery_time "\"60s\"" 
crudini --set $ONOMY_NODE_CONFIG statesync chunk_request_timeout "\"60s\"" 
crudini --set $ONOMY_NODE_CONFIG statesync chunk_fetchers "\"10\"" 
```

5. Start the node&#x20;

* Increase open file limit to more than 65535&#x20;

`ulimit –n 65536`

* Start onomyd&#x20;

`onomyd start`

* If you are using cosmovisor then start cosmovisor&#x20;

`cosmovisor start`

### Check the status of the Onomy chain

You should be good to go! You can check the status of the Onomy chain by running:

```
curl http://localhost:26657/status
```

Your node is fully synced when `catching_up` is false.

You can also open the `http://localhost:26657` in your browser and check available end-points
