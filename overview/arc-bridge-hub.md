---
description: Empowering an enhanced cross-chain experience.
---

# Arc Bridge Hub

[Arc](https://github.com/onomyprotocol/arc) is a hub-and-spoke model bridge that enables users to transfer tokens from an integrated chain to Onomy and back again - or to another integrated chain - by locking up tokens on integrated chain side of Arc, and minting equivalent tokens on the Onomy side of Arc. \
\
Arc is completely non-custodial, meaning there is no central intermediary or third party bridge administrators who could run off with your funds. The security of the Onomy Network, and thus Arc, is through the Onomy Validator Guild (OVG) which is comprised of a decentralized network of globally situated independent validator firms. Learn more about the criteria to join as a validator [here](../validators-staking/onomy-validator-guild-ovg.md). **Joining Onomy's validator set is permissionless.**&#x20;

| ARC Mainnet (A-Z) | ARC Testnet (A-Z)       | ARC Accepted Proposals |
| ----------------- | ----------------------- | ---------------------- |
| Ethereum          | Avalanche               | BNB Chain              |
| Cosmos Hub        | Aurora (NEAR EVM)       |                        |
| OKX Chain         | Fantom                  |                        |
| Osmosis           | Moonbeam (Polkadot EVM) |                        |
|                   | Neon (Solana EVM)       |                        |
|                   | Polygon                 |                        |

* **Total Bridge Txs:** 2,024+
  * _Timeframe: December 3, 2022 - February 26, 2023_

**Production Uses**

* ****[**Bonding Curve Platform**](https://bnom.onomy.io)**:**  Powers the Ethereum <> Onomy bridge for bonding curve users.
* **Bridge Interface @** [**app.onomy.io**](https://app.onomy.io)**:** Powers bridges between integrated chains.

## How Arc Works

### Mint & Burn Mechanism

The task of any blockchain bridge is to hold tokens on one chain and issue representative assets on the other chain, unlocking the assets at some point in the future when they are sent back across the bridge.

Since Arc is a bi-directional bridge that handles two asset sources, Cosmos and Ethereum tokens may be held on Ethereum or held on Cosmos based on where they originated. Likewise tokens are minted on both sides as well, to represent the assets from the other chain. The same process occurs between other integrated blockchains.

### Security

The **Validator Set** is the actual set of keys with stake behind them, which are slashed for double-signs or other misbehavior. Typically, the security of a chain is considered tied to the security of a _Validator Set_. This varies on each chain, but many people view it as a gold standard. Even IBC offers no more security than the minimum of both involved Validator Sets.

The **Eth Signer** is a binary run alongside the main Cosmos daemon (`gaiad` or equivalent) by the validator set. It exists purely as a matter of code organization and is in charge of signing Ethereum transactions, as well as observing events on Ethereum and bringing them into the Cosmos state. It signs transactions bound for Ethereum with an Ethereum key, and signs over events coming from Ethereum with a Cosmos SDK key. Slashing conditions may be added to any mis-signed message by any _Eth Signer_ run by the _Validator Set_ and be able to provide the same security as the _Valiator Set_, just a different module detecting evidence of malice and deciding how much to slash. If it can be proven that a transaction signed by any _Eth Signer_ of the _Validator Set_ was illegal or malicious, then it can be slashed on the Cosmos chain side and potentially provide 100% of the security of the _Validator Set_. Note that this also has access to the 3 week unbonding period to allow evidence to slash even if they immediately unbond.

The **MultiSig Set** is a (possibly aged) mirror of the _Validator Set_ but with Ethereum keys, and stored on the Ethereum contract. If the _MultiSig Set_ is updated much more often than the unbonding period (eg at least once per week), then it can be guaranteed that all members of the _MultiSig Set_ should have slashable atoms for misbehavior. However, in some extreme cases of stake shifting, the _MultiSig Set_ and _Validator Set_ could get quite far apart, meaning many of the members in the _MultiSig Set_ are no longer active validators and may not bother to transfer Eth messages. Thus, to avoid censorship attacks/inactivity, this should also be updated everytime there is a significant change in the Validator Set (eg. > 3-5%). If those two conditions are maintained, the MultiSig Set should offer a similar level of security as the Validator Set.

### Audits

Onomy Arc Bridge Hub has been audited by NCC Group. The Arc Bridge Hub code repository containing additional documentation can be found [here](https://github.com/onomyprotocol/arc).
