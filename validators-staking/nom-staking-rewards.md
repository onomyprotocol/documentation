# NOM Staking Rewards

{% hint style="info" %}
_Staking provides rewards in return for delegating to validators who support the security and operations of a blockchain network._ _Note that staking rewards are calculated and distributed based on a predetermined protocol-level monetary policy regarding inflationary mechanics and are not to be construed as a form of dividends or interest paid on debt. Staking rewards are distributed in additional NOM added to supply._
{% endhint %}

### What are the incentives to stake?

Each member of a validator’s staking pool earns different types of revenue.

* **Staking Rewards:** Total staking rewards are divided among a validator’s staking pool according to each validator’s weight. Their rewards are then divided among delegators in proportion to each delegator’s stake. Note that validators may charge a commission on their delegators’ rewards before it is distributed.\

  * **Inflation Rate:** Dynamically adjusted rate that undergoes an initial hyper-inflationary period followed by an indefinite stabilization period with a goal of 50%+ of NOM staked with nodes.
  * **Staking Ratio:** The amount of NOM being staked out of the total supply.\


![](<../.gitbook/assets/image (14) (1).png>)

### What is the incentive to run a validator?

Validators earn more revenue than their delegators through commission.

* **Staking Rewards:** Detailed above, plus validators earn more revenue than their delegators through commission.&#x20;
* **Product Rewards:** The Arc Bridge Hub each will uniquely have additional rewards for validators.&#x20;
* **Compute fees (gas)**: To prevent spamming, validators can set minimum gas fees for transactions to be included in their mempool. At the end of every block, compute fees are disbursed to the participating validators proportional to their stake.
* **Delegation:** Treasury tokens are currently delegated based on a validator's self-bond amount in relation to the total self-bonded across all validators. Rather than a centrally-controlled Foundation controlling the keys to the treasury, and thus controlling delegations, the treasury exists in a DAO-governed on-chain wallet that programmatically rebalances delegations based on on-chain self-bond metrics.
* [_Learn more about becoming a validator_](../start-a-validator/onomy-validator-guild-ovg.md)__

### What is a validator’s commission?

The revenue received by a validator’s pool is split between a validator and their delegators. A validator can apply a commission on the part of the revenue that goes to its delegators. This commission is set as a percentage. Each validator is free to set its initial commission, maximum daily commission change rate, and maximum commission. The mainnet enforces the parameters that each validator sets. These parameters can only be defined when initially declaring candidacy, and may only be constrained further after being declared.
