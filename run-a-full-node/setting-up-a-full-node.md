# Starting a Full Node

## How to Run an Onomy Full Node

Minimum system requirements:

* Any modern Linux distribution
  * [Note for cloud/server hosted versions of Ubuntu without a GUI](setting-up-a-full-node.md#note-for-cloud-server-hosted-versions-of-ubuntu)
* 8 Core CPU (16+ Preferred)
* 32 GB of RAM (64+ Preferred)
* 500 GB of NVMe Gen 4+ storage

Make sure you have gone through [Installation Steps](pre-installation-steps.md) prior to setting up a node.

There are a couple of ways to initialize and start a full node

* **Method 1:** Initialize and start a full node using scripts (Recommended)
* **Method 2:** Initialize and start a full node manually

## Method 1: Initialize and start a full node using scripts

1. First, create the `.onomy/scripts` directory and cd into the newly created scripts folder.

```
mkdir $HOME/.onomy/scripts
cd $HOME/.onomy/scripts
```

2. Download scripts from GitHub using the commands below:

```
wget https://raw.githubusercontent.com/onomyprotocol/validator/main/mainnet/scripts/allow-cors.sh
wget https://raw.githubusercontent.com/onomyprotocol/validator/main/mainnet/scripts/init-full-node.sh
wget https://raw.githubusercontent.com/onomyprotocol/validator/main/mainnet/scripts/start-onomyd.sh
wget https://raw.githubusercontent.com/onomyprotocol/validator/main/mainnet/scripts/stop-onomyd.sh
```

3. Add executable permissions to the scripts using `chmod +x *`
4. Download the genesis file found on GitHub using the following command:

```
wget https://raw.githubusercontent.com/onomyprotocol/validator/main/mainnet/genesis/genesis-mainnet-1.json
```

5. Start the initialization process with the following command:

`bash init-full-node.sh`

This script will ask you for the following...

* Node Name \[Text]: A name for your full node
* Seed IPs \[Comma separated list of IPs]: IP Addresses of seed nodes, There are a couple of IPs pre-configured, If you want to point your node to custom Seed node, you can do that here
* Your IP Address \[IP Address]

After the script is done, you will find `node-id` in the output. Make sure to take a note of the node id as you might need it afterwards. If you forgot to save your node id, you can find it using `onomyd tendermint show-node-id` in future.

6. **Optionally** allow CORS using the command `bash allow-cors.sh`
7. Increase ulimit to > 65535 with the command `ulimit -n 65536`
8. **Recommended:** Use StateSync to quickly sync with the blockchain. Download the `init-statesync.sh` script using:

```
wget https://raw.githubusercontent.com/onomyprotocol/validator/main/mainnet/scripts/init-statesync.sh
```

&#x20; Then, enter `bash init-statesync.sh`

9. It is recommended to run the node as a background service, enabling you to enter commands and utilize your terminal while the node operates. To do this, download the add-service.sh script.&#x20;

```
wget https://raw.githubusercontent.com/onomyprotocol/validator/main/mainnet/scripts/add-service.sh
```

Then, give it executable permissions with `chmod +x add-service.sh`. Next, run it from sudo user with `sudo ./add-service.sh onomyd ${PWD}/start-onomyd.sh`.

**Success! Your node should be running.** When you first start the node, give it enough time to sync with the blockchain. In future, you may use  `./stop-onomyd.sh` to stop the node at any time.  Use the same command above to restart it!

* _**Alternatively,**_ if you wish to run the node without using it in a background process, then simply start your node with `./start-onomyd.sh`

## Method 2: Initialize and start a full node manually

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

Open one RPC endpoint of one of the state-sync nodes to get the details of latest block

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

### Note for cloud/server hosted versions of Ubuntu

{% hint style="info" %}
Ensure you are using GLIBC 2.34 or higher, which typically comes with Ubuntu 22.04 LTS. To upgrade Ubuntu to the next available release, you can use the <mark style="color:blue;">`do-release-upgrade`</mark> tool. However, this tool is only recommended for server editions without a GUI. If you're running a desktop version, the update manager should prompt you to upgrade when a new release is available.

1.  **Prepare for the Upgrade**:

    * Backup your important files and data.
    * Update all your currently installed packages with:&#x20;

    <mark style="color:blue;">`sudo apt-get update && sudo apt-get upgrade`</mark>
2.  **Upgrade Ubuntu**:

    * Install the `update-manager-core` package if it is not already installed:

    <mark style="color:blue;">`sudo apt-get install update-manager-core`</mark>
3. Start the upgrade process:

&#x20;       <mark style="color:blue;">`sudo do-release-upgrade`</mark>

Follow the on-screen instructions to complete the upgrade process. After completion and restarting your server, you may check the version with <mark style="color:blue;">`ldd --version`</mark> to ensure you are up-to-date.
{% endhint %}

