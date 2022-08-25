# Incentives & Staking Rewards

{% hint style="info" %}
_Staking provides rewards in return for delegating to validators who support the security and operations of a blockchain network._ _Note that staking rewards are calculated and distributed based on a predetermined protocol-level monetary policy regarding inflationary mechanics and are not to be construed as a form of dividends or interest paid on debt. Staking rewards are distributed in additional NOM added to supply._
{% endhint %}

### What are the incentives to stake?

Each member of a validator’s staking pool earns different types of revenue.

* **Staking Rewards:** Calculated by dividing the _**inflation rate** _ by the _**staking ratio**_. Think of newly-minted NOM as a pie: inflation is the size of that pie. By staking, you're sitting at a table where fresh NOM is being served. Validators proportionately split the pie with everyone else at their table, so the more that others join, the smaller the slice of the pie is that you get. The bigger the pie, the more rewards everyone staking gets. \
  \
  In other words, the total staking rewards are divided among a validator’s staking pool according to each validator’s weight. Their rewards are then divided among delegators in proportion to each delegator’s stake. Note that validators may charge a commission on their delegators’ rewards before it is distributed.\

  * **Inflation Rate:** Dynamically adjusted rate that undergoes an initial hyper-inflationary period to incentivize early adoption, liquidity, and growth of the ecosystem followed by an indefinite stabilization period with a goal of 50% of NOM staked with nodes.&#x20;
  * **Staking Ratio:** The amount of NOM being staked out of the total supply.\

  * **Example Reward Rate Calculations**
    * **Scenario 1:** If the current inflation rate is _25%_ and _10%_ of all NOM is being staked, we would estimate an annual reward rate of _0.25 / 0.10 = 2.5 or **250%**_.&#x20;
    * **Scenario 2:** With the goal of incentivizing _50%_+ of NOM being staked and using the same _25%_ inflation rate as the previous scenario, we would now estimate an annual reward rate of _0.25 / 0.50 = 0.50 or **50%**_. We can see that more participants staking leads to stronger distribution of the staking rewads.
    * **Scenario 3:** After the initial hyper-inflationary regime, inflation will enter its stabilization period. Assuming an inflation rate of _10%_ and _50%_ of all NOM being staked, we would expect an annual reward rate of _0.10 / 0.50 = .20 or **20%** _ annual reward rate.
    * The current reward rate is displayed when selecting your validator to delegate to, but now you know how to calculate estimated rewards yourself! Remember that validators may charge different commissions, so be sure to review which validator you'd like to delegate to. Other metrics include the validator's uptime (reliability), self-bond rate, voting power on the network, and so on.

![](<../.gitbook/assets/image (14) (1).png>)

### What is the incentive to run a validator?

Validators earn more revenue than their delegators through commission.

* **Staking Rewards:** Detailed above, plus validators earn more revenue than their delegators through commission.&#x20;
* **Product Rewards:** The Onomy Exchange, Onomy Reserve, and Onomy Bridge Hub each will uniquely have additional rewards for validators. Details will be added to documentation.
* **Compute fees (gas)**: To prevent spamming, validators can set minimum gas fees for transactions to be included in their mempool. At the end of every block, compute fees are disbursed to the participating validators proportional to their stake.
* **Delegation:** Treasury tokens will be delegated based on a validator's self-bond amount in relation to the total self-bonded across all validators. Rather than a centrally-controlled Foundation controlling the keys to the treasury, and thus controlling delegations, the treasury exists in a DAO-governed on-chain wallet that programmatically rebalances delegations based on on-chain self-bond metrics.
* [_Learn more about becoming a validator_](onomy-validator-guild-ovg.md)__

### What is a validator’s commission?

The revenue received by a validator’s pool is split between a validator and their delegators. A validator can apply a commission on the part of the revenue that goes to its delegators. This commission is set as a percentage. Each validator is free to set its initial commission, maximum daily commission change rate, and maximum commission. The mainnet enforces the parameters that each validator sets. These parameters can only be defined when initially declaring candidacy, and may only be constrained further after being declared.
