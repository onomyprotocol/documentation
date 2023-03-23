---
description: About the Onomy Validator Guild (OVG)
---

# Onomy Validator Guild (OVG)

![](https://miro.medium.com/max/1400/0\*IGJBZqooWrXh7o2R)

{% hint style="info" %}
_Want to skip ahead and get right into it?_ [_View instructions to join the validator set here_](creating-a-validator.md)__[_._](creating-a-validator.md)__
{% endhint %}

## About the Onomy Validator Guild (OVG) <a href="#0a33" id="0a33"></a>

Validators support The Onomy Network (ONET), a layer-1 blockchain built with Cosmos SDK / Tendermint BFT proof-of-stake (PoS). To optimize network-wide performance, maintain decentralization, and ensure security, the OVG maintains a high standard in operational performance and sustainability for the _bettermint_ of Onomy Protocol.

### How do I become a validator?

Any individual or entity may become a validator by first obtaining the minimum self-bond requirement of 225,000 NOM. The process then entails creating a validator and registering its validator profile. The candidate then broadcasts a `create-validator` transaction, which contains the following data:

* **PubKey:** Validator operators can have different accounts for validating and holding liquid funds. The PubKey submitted must be associated with the private key the validator will use to sign _prevotes_ and _precommits_.
* **Address:** A `onomyvaloper-` address. This is the address used to identify your validator publicly. The private key associated with this address is used to bond, unbond, and claim rewards.
* **Name** (also known as the **moniker**)
* **Website** _(optional but recommended)_
* **Description** _(optional but recommended)_
* **Initial commission rate:** The commission rate on block provisions, block rewards and fees charged to delegators.
* **Maximum commission:** The maximum commission rate which the validator will be allowed to charge. (This cannot be changed)
* **Commission change rate**: The maximum daily increase of the validator’s commission.(This cannot be changed)
* **Minimum self-bond amount**: The minimum amount of bonded NOM the validator needs at all times. Currently set to 225,000 NOM and may change according to a DAO governance vote. If the validator’s self-bonded stake falls below this limit, its entire staking pool will be unbonded.
* **Initial self-bond amount**: The initial amount of NOM the validator self-bonds.

### Validator Criteria <a href="#f86e" id="f86e"></a>

#### Recommended Hardware <a href="#f86e" id="f86e"></a>

Validators interested in joining the active set should match or exceed the following criteria:

* 32 core EPYC, 256GB of RAM, 5TB enterprise SSD
* 1+ GBPS dedicated internet connection

#### Operations

Validators run the following:&#x20;

* A validator node (including an Onomy Network full node)
* Full node for all integrated bridges to the Arc Bridge Hub
* Required bridges/orchestrators
* Optional sentry nodes, seed nodes, statesync nodes
* Full nodes for all chains will require significant storage capacity. ETH Mainnet alone recommends 1TB.&#x20;

**Validator Grants & Delegation**\
\
Validators may receive grants from the DAO treasury to assure the latest system specs are used, and to subsidize related costs to uphold performance required for the sustainability of the Onomy Network. Treasury tokens will be delegated based on a validator's self-bond amount in relation to the total self-bonded across all validators. Rather than a centrally-controlled Foundation controlling the keys to the treasury, and thus controlling delegations, the treasury exists in a DAO-governed on-chain wallet that programmatically rebalances delegations based on on-chain self-bond metrics.

**Ready to begin?** [**View instructions to join the validator set here**](creating-a-validator.md)**.**
