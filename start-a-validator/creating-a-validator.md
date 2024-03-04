# Creating a Validator

## Prerequisites

* [Operate a synced Onomy full node](broken-reference)
* 225K NOM to use for the minimum required self-delegation. Consider a buffer in case of unforeseen slashing.

### Create a Validator

**STEP 1:** Create or add keys for the validator. Be sure to secure the mnemonic from the output.

```
onomyd keys add validator
```

You may also recover keys from a mnemonic:

```
onomyd keys add --recover validator
```

{% hint style="warning" %}
Be sure the validator account is funded with the minimum self-delegation of 225K NOM.
{% endhint %}

**STEP 2:** Verify balances with the following:

```
onomyd q bank balances <account address starting with onomy1>
```

**STEP 3:** Register your validator node:

```
onomyd tx staking create-validator \
  --pubkey $(onomyd tendermint show-validator) \
  --moniker "the name of your validator" \
  --details "a description of your validator" \
  --identity "your keybase.io PGP key (your pfp will be used in apps)" \ # optional
  --website "http://homepage.validator.com" \ # optional
  --security-contact "contact@your.email" \ # optional
  --min-self-delegation 225000000000000000000000 \
  --commission-rate "0.05" \
  --commission-max-rate "0.20" \
  --commission-max-change-rate "0.01" \
  --amount 225000000000000000000000anom \
  --from validator \
  --chain-id onomy-mainnet-1 \
  --gas auto \
  --gas-adjustment 1.4 \
  --gas-prices 0anom
```

{% hint style="info" %}
_anom_ is the smallest denomination of _NOM_. The minimum required _225,000 NOM self-delegation amount_ is written as _225000000000000000000000anom. Update with the correct value you are self-delegating, as well as the other variables._\
\
**Descriptions:**

* **`amount`** : Amount of aNOMs to self delegate. 1 NOM = 10^18 aNOM
* **`pubkey`** : By using the command onomyd `tendermint show-validator`, the private key will be provided in .onomyd/config/priv\_validator\_key.json as your validator's signing key. Make sure to backup this file.
* **`moniker`** : Name of the validator
* **`website` (optional)** : Website of the organization/individual running validator&#x20;
* **`identity` (optional)** : Pgp key generated using UPort or KeyBase
* **`commission-rate`** : Commission rate of the validator
* **`commission-max-rate`** : Maximum commission a validator can charge
* **`commission-max-change-rate`** : Maximum daily increase of a validator commission
* **`min-self-delegation`** : Minimum NOM a validator must keep bonded - this cannot be lower 225,000 NOM. The minimum amount of self delegation for your validator to be active. If you withdraw your self delegation to below this threshold, your validator will be immediately removed from the active set. Your validator will not be slashed, but will stop earning staking rewards.&#x20;
{% endhint %}

**STEP 4:** Check if your validator is operational:

```
onomyd query staking validator $(onomyd keys show validator --bech val --address) --output json | jq
```

You have succeeded if the status is `BOND_STATUS_BONDED`

**Welcome to the Onomy Validator Guild!**

#### **Migrating servers**

To migrate your validator to a new server, you first sync up a new node (check the instruction on using snapshot or state sync). Then:

1. Shut down your old node
2. Copy your `priv_validator_key.json` and `priv_validator_state.json` to the new node
   * You may use `scp` command to download it from remote host to local machine, then upload it to the new remote host. Be sure to exit ssh before using `scp`
3. Restart your new node

NOTE: Step (1) must be done first, or your validator may double-sign!

Optional: Copy `node_key.json` to the new server as well. This is not mandatory, but helps your node to establish P2P connections faster.
