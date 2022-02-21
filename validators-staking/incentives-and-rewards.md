# Incentives & Rewards

#### What are the incentives to stake?

Each member of a validator’s staking pool earns different types of revenue:

* **Compute fees (gas)**: To prevent spamming, validators can set minimum gas fees for transactions to be included in their mempool. At the end of every block, compute fees are disbursed to the participating validators proportional to their stake.
* **Product Rewards:** The Onomy Exchange, Onomy Reserve, and Onomy Bridge Hub each will uniquely have additional rewards for validators. Details will be added to documentation.
* **Staking Rewards:** Primary staking rewards are given based on a dual-regime model with an initial hyper-inflationary period that rewards staking participants with up to 100% APY followed by an indefinite stabilization period with a goal of 50% of NOM staked with nodes.

This total revenue is divided among a validator’s staking pool according to each validator’s weight. The revenue is then divided among delegators in proportion to each delegator’s stake. Note that a commission on delegators’ revenue is applied by the validator before it is distributed.

![](<../.gitbook/assets/image (14) (1).png>)

#### What is the incentive to run a validator?

Validators earn more revenue than their delegators through commission.

#### What is a validator’s commission?

The revenue received by a validator’s pool is split between a validator and their delegators. A validator can apply a commission on the part of the revenue that goes to its delegators. This commission is set as a percentage. Each validator is free to set its initial commission, maximum daily commission change rate, and maximum commission. The mainnet enforces the parameters that each validator sets. These parameters can only be defined when initially declaring candidacy, and may only be constrained further after being declared.
