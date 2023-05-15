# NOM Staking Concepts

{% hint style="info" %}
_Staking provides rewards in return for delegating to validators who support the security and operations of a blockchain network._ _Note that staking rewards are calculated and distributed based on a predetermined protocol-level monetary policy regarding inflationary mechanics and are not to be construed as a form of dividends or interest paid on debt. Staking rewards are distributed in additional NOM added to supply._
{% endhint %}

### What is a validator?

Onomy Protocol is powered by Tendermint BFT Proof-of-Stake consensus. Validators run full nodes, participate in consensus by broadcasting votes, commit new blocks to the blockchain, and participate in governance of the blockchain. Validators are able to cast votes on behalf of their delegators. A validator’s voting power is weighted according to their total stake.

### What is a full node?

A full node is a program that validates the transactions and blocks of a blockchain. Validators must run full nodes. Full nodes require more resources than light nodes, which only processes block headers and a small subset of transactions. Running a full node means you are running a non-compromised and up-to-date version with low network latency and no downtime.

It is possible and encouraged for any user to run full nodes even if they do not plan to be validators.

### What is staking?

When NOM holders delegate their NOM to a validator, they are _**staking.**_ Staking increases a validator’s weight, which helps them, and in return delegators get rewarded.

A validator’s weight (total stake) is determined by the amount of staking tokens (NOM) they self-bond from their own holdings plus the NOM bonded to them by external delegators. Validators for the Onomy Network must self-bond a minimum 225,000 NOM to be an active validator. Additional NOM bonded by external delegators boosts their weight. Validators with a higher weight will propose more blocks, and in turn earn more rewards.

The minimum self-bond, hardware specifications, and the bottom validator’s stake always forms the barrier for entry into the active set of validators. If validators double-sign, or are frequently offline, they risk their staked NOM, including NOM delegated by users, being slashed by the protocol to penalize negligence and misbehavior.\
\
**Read more about expected** [**NOM staking rewards**](nom-staking-rewards.md)**.**

### What is a delegator?

Delegators are NOM holders who want to receive staking rewards without the responsibility of running a validator. Through Onomy Access and Cosmos-based wallets, a user can delegate NOM to a validator and in exchange receive a part of a validator’s revenue. For more detail on how revenue is distributed, see [What are the incentives to stake?](nom-staking-rewards.md#what-are-the-incentives-to-stake) and [What is a validator’s commission?](nom-staking-rewards.md#what-is-a-validators-commission)

Delegators share the benefits and rewards of staking with their Validator. If a Validator is successful, its delegators will consistently share in the rewards structure. If a Validator is slashed, the delegator’s stake will also be slashed. This is why delegators should perform due-diligence on validators before delegating. Delegators can also diversify by spreading their stake over multiple validators.

Delegators play a critical role in the system, as they are responsible for choosing validators. Being a delegator is not a passive role. Delegators should remain vigilant, actively monitor the actions of their validators, and re-delegate whenever they feel their current validator does not meet their needs.

