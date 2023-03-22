# Creating a Validator

## Prerequisites

* [Operate a synced Onomy full node](broken-reference)
* 225K NOM to use for the minimum required self-delegation
  * _250K+ NOM is recommended for padding in case of unforeseen slashing_
* Ethereum Node and ETH account funded for the orchestrator

**There are two steps to start a validator:**

1. [Create a Validator](creating-a-validator.md#create-a-validator)
2. [Start the Orchestrator](creating-a-validator.md#start-the-orchestrator)

### Create a Validator

**STEP 1:** Create or add keys for the validator and the orchestrator. Be sure to secure the mnemonic from the output:

```
onomyd keys add validator
onomyd keys add orchestrator
```

You may also recover keys from a mnemonic:

```
onomyd keys add --recover validator
onomyd keys add --recover orchestrator
```

{% hint style="warning" %}
Be sure the validator account is funded with the minimum self-delegation of 225K NOM. Additionally, account for funding the orchestrator with NOM. An initial balance of 10 NOM is recommended for the orchestrator.
{% endhint %}

**STEP 2:** Verify balances with the following:

```
onomyd q bank balances <account address starting with onomy1>
```

**STEP 3:** Register your validator node:

```
onomyd tx staking create-validator \
--amount=225000000000000000000000anom \
--pubkey=<public key of the validator> \
--moniker=<name of the validator> \
--website=<validator website> \    # Optional
--identity=<validator identity signature> \    # Optional
--commission-rate=<valiator commission rate> \
--commission-max-rate=<validator max commission rate> \
--commission-max-change-rate=<validator max commission change rate> \ 
--min-self-delegation=22500000000000000000000anom \
--from=validator \
--gas="auto" \
--gas-adjustment=1.5 \
--chain-id="onomy-mainnet-1" \
-y
```

{% hint style="info" %}
_anom_ is the smallest denomination of _NOM_. The minimum required _225,000 NOM self-delegation amount_ is written as _225000000000000000000000anom. Update with the correct value you are self-delegating, as well as the other variables._\
__\
__**Descriptions:**

* **`amount`** : Amount of aNOMs to self delegate. 1 NOM = 10^18 aNOMs
* **`pubkey`** : Public key of the validator. Can be found using:
  * `onomyd tendermint show-validator`
* **`moniker`** : Name of the validator
* **`website` (optional)** : Website of the organization/individual running validator&#x20;
* **`identity` (optional)** : Pgp key generated using UPort or KeyBase
* **`commission-rate`** : Commission rate of the validator
* **`commission-max-rate`** : Maximum commission a validator can charge
* **`commission-max-change-rate`** : Maximum daily increase of a validator commission
* **`min-self-delegation`** : Minimum NOM a validator must keep bonded - this cannot be lower 225,000 NOM.
* **`from`** : Account which is used to create the validator (In this example it is set as validator)
{% endhint %}

**STEP 4:** Check if your validator is operational:

```
onomyd query staking validator "$(onomyd keys show validator --bech val --address)"
```

You have succeeded if the status is `BOND_STATUS_BONDED`

### Start the Orchestrator

{% hint style="warning" %}
**NOTE:** Orchestrator must be run alongside the validator. If not, validator will be slashed and jailed after 2500 blocks (approximately 4 Hours).
{% endhint %}

Download the latest version of the Gravity Bridge Tool (gbt) from the [Releases Page](https://github.com/onomyprotocol/arc/releases) of Onomy's Arc Bridge Hub. In this tutorial `Release v0.1.0eth2` is used.

**STEP 1:** Create a directory named `.gbt` in the home directory to be used as gbt's default directory

**STEP 2:** Create a new file named config.toml and copy the following settings into the file:

```
# Orchestrator configuration options
[orchestrator]
relayer_enabled = true

# Relayer configuration options
[relayer]
batch_request_mode = "EveryBatch"

[relayer.valset_relaying_mode]
mode = "EveryValset"

[relayer.batch_relaying_mode]
mode = "EveryBatch"
```

**STEP 3:** Register the delegate keys for the orchestrator and update the values as needed.

```
gbt -a onomy keys register-orchestrator-address \
      --cosmos-phrase=<orchestrator mnemonic> \
      --validator-phrase=<validator mnemonic> \
      --ethereum-key=<ethereum private key> \
      --cosmos-grpc="http://localhost:9191/" \
      --fees="1anom"
```

{% hint style="info" %}
* **`cosmos-phrase`** : Mnemonic of the account used for orchestrator (in this example it is `orchestrator`
* **`validator-phrase`** : Mnemonic of the account used for validator (in this example it is `validator`
* **`ethereum-key`** : Private key of the Ethereum account being used to sign transactions
* **`cosmos-grpc`** : gravity endpoint for the full node.
{% endhint %}

**STEP 4:** Once registered, run the orchestrator with following command:

```
gbt -a onomy orchestrator \
             --cosmos-phrase=<orchestrator mnemonic> \
             --ethereum-key=<ethereum private key> \
             --cosmos-grpc="http://localhost:9191/" \
             --ethereum-rpc=<ethereum RPC endpoint> \    # RPC endpoint of the Ethereum Node
             --fees="1anom" \
             --gravity-contract-address="0x85391fd149282C50B103aC810430F887685d575C"
```

**Welcome to the Onomy Validator Guild!**
