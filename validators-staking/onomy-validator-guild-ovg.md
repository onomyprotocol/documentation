---
description: Joining the Onomy Validator Guild (OVG)
---

# Onomy Validator Guild (OVG)

![](https://miro.medium.com/max/1400/0\*IGJBZqooWrXh7o2R)

## Joining the Onomy Validator Guild (OVG) <a href="#0a33" id="0a33"></a>

Validators support The Onomy Network (ONET), our layer-1 blockchain built with Cosmos SDK / Tendermint BFT proof-of-stake (PoS). To optimize network-wide performance, maintain decentralization, and ensure security, the OVG maintains a high standard in operational performance and sustainability for the _bettermint_ of Onomy Protocol.

### How do I become a validator?

**Given that validators must validate for the Onomy Network and operate nodes for bridges chains, the active validator set during testnets and betanet will be limited to assure stability of the network and methodical expansion of the set.** \
\
Any participant in the network meeting the criteria can signal their intent to become a validator by first filling out the [OVG Form](onomy-validator-guild-ovg.md#ovg-interest-form). The process then entails creating a validator and registering its validator profile. The candidate then broadcasts a `create-validator` transaction, which contains the following data:

* **PubKey:** Validator operators can have different accounts for validating and holding liquid funds. The PubKey submitted must be associated with the private key the validator will use to sign _prevotes_ and _precommits_.
* **Address:** A `onomyvaloper-` address. This is the address used to identify your validator publicly. The private key associated with this address is used to bond, unbond, and claim rewards.
* **Name** (also known as the **moniker**)
* **Website** _(optional but recommended)_
* **Description** _(optional but recommended)_
* **Initial commission rate:** The commission rate on block provisions, block rewards and fees charged to delegators.
* **Maximum commission:** The maximum commission rate which the validator will be allowed to charge. (This cannot be changed)
* **Commission change rate**: The maximum daily increase of the validator’s commission.(This cannot be changed)
* **Minimum self-bond amount**: The minimum amount of bonded NOM the validator needs at all times. Currently set to 250,000 NOM and may change according to a DAO governance vote. If the validator’s self-bonded stake falls below this limit, its entire staking pool will be unbonded.
* **Initial self-bond amount**: The initial amount of NOM the validator self-bonds.

### Validator Criteria <a href="#f86e" id="f86e"></a>

#### Hardware <a href="#f86e" id="f86e"></a>

Validators interested in applying must hold servers that match or exceed the following criteria:

* 32 core EPYC, 256GB of RAM, 5TB enterprise SSD
* 10 GBPS dedicated internet connection

#### Minimum NOM Stake for Validators <a href="#c05b" id="c05b"></a>

Given the high demand expressed by partner groups (with over 150+ active validators on testnet), our plans for long-term collaboration, and the inherent performance needs of the Onomy Network, _**the minimum self-bond NOM stake per validator group is 250,000 NOM**_.\
\
**Validator Grants & Delegation**\
\
Validators may receive grants to assure the latest system specs are used, and to subsidize related costs to uphold performance required for the sustainability of the Onomy Network. Delegations may also be provided to enhance voting power distribution.&#x20;

### OVG Interest Form

{% embed url="https://dq8zk5x8w7j.typeform.com/to/EiXBpya5" %}
