# Feature Overview

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

The **Onomy Exchange (ONEX)** is a cross-chain and multi-chain hybrid decentralized exchange (DEX), with a technical architecture that converges Automated Market Maker (AMM) liquidity pools with an orderbook UI, thus creating a powerful, fair, and non-custodial approach to trading that supports stop losses, limit orders, conditional orders and advanced charting. It's the CEX experience, on-chain.

ONEX is a flagship consumer appchain of the Onomy Network. Features include trading, liquidity provision, leaderboards, using platform revenue to effectuate a buy and burn on NOM, and more.

For a more technical look, check [under-the-hood.md](under-the-hood.md "mention")

{% hint style="info" %}
**STATUS:** The Onomy Exchange has been deployed as a consumer chain to the Onomy Network on a testnet. A test interface exists for DAO member engagement and testing purposes. DAO Contributors may create their own interfaces to ONEX. A leading interface supported by major DAO contributors may be viewed, along with its progress, in the Onomy DAO forum and social channels.
{% endhint %}

## **Trade Interface**

This section explains how users can place trades on ONEX by selecting a trading pair and then choosing the base to determine the direction of the trade.

### **Selecting a Trading Pair**

* Definition: A trading pair consists of two cryptocurrencies, such as NOM/OSMO.
* Example: Choosing the NOM/OSMO pair.

### **Choosing the Base and Direction**

* Definition: The Base determines the direction of the trade.
* Example 1 (NOM/OSMO): Selecting NOM as the base, OSMO gets quoted in price calculations, and the user is using OSMO to buy NOM.
* Example 2 (OSMO/NOM): Selecting OSMO as the base, NOM gets quoted in price calculations, and the user is using NOM to buy OSMO.
* "Select Base" Dropdown: This option allows users to choose the base of the selected pair.

## Liquidity Management (Farm)

### **Introduction**

This section covers the creation and management of liquidity pools for a given pair. Liquidity providers (LPs) are provided shares of the pool known as drops that correspond to the LP’s share of the overall pool liquidity. Functions in the Farm section of ONEX include the ability to create new pools, manage drops in existing pools, and redeem your drops.

The breakdown of pool rewards are as follows:

* 80% of pool rewards go toward Liquidity Providers
* 10% of pool rewards go toward the Buy and Burn
* 10% of pool rewards go toward boosted rewards for Liquidity Provider Leaders

Pool Rewards are calculated as the current value of total drops minus the initial deposit value - further explanation here (link to Rewards Calculation).&#x20;

### **Creating Pools**

* Users can create liquidity pools by selecting two tokens and depositing any amount for each of the two tokens. The ratio of the two token amounts during pool creation determines the initial AMM price. The pool allows other users to trade between these two tokens, and the creator receives drops, representing their share of the total pool liquidity for the given pair. A new pool will initially be wholly owned by its creator until more liquidity is added by new LPs who receive additional drops.

### **Managing Liquidity/Drops**

* Adding Liquidity: Users can add liquidity by depositing both tokens in the selected ratio. They receive drops in return, representing their share of the pool.
* Removing Liquidity / Redeeming Drops: Users can remove liquidity by redeeming their drops. They receive both pair tokens corresponding to the drops, plus a share of the trading fees accumulated since they added liquidity that are paid as LP Rewards in the same ratio as the pool balances. Drops can be redeemed at any time for the corresponding share of the liquidity pool. The redemption process involves submitting a transaction specifying the drops to redeem and receiving the corresponding tokens in return.&#x20;

## Liquidity Provider Leaderboard

### **Introduction**

This section explains how leaders are selected among Liquidity Providers (LPs) and the boosted rewards for the top 3 LPs for any given pair liquidity pool. Both the boosted reward amount and the number of top LPs that share in the boosted rewards are configurable and votable via DAO governance.

### **Selection Criteria**

Leaders are selected based on the total liquidity provided metric per individual pool. In other words, the leaderboard ranks all LPs based on the % of drops (shares) they own out of the total liquidity pool for a given pair. Over time, the DAO may implement metrics such as consistency and duration.

### **Leaderboard Rewards**

Leaderboard rewards are currently configured as follows:

* 1st Place: Additional 5% of the total rewards pool
* 2nd Place: Additional 3% of the total rewards pool
* 3rd Place: Additional 2% of the total rewards pool

## Wallet

* **Balances:** Users can view and manage token balances in their Wallet Account and Exchange Account
* **Wallet Account:** Consists of assets from the connected wallet that have not been deposited to the ONEX exchange module
* **Exchange Account:** Consists of assets that have been deposited into the ONEX exchange module

### **Wallet Transfer Function**

* Users can move assets to and from their Exchange Account and Wallet Account by utilizing the Transfer function. Assets must be in the Exchange Account for use with ONEX. For assets to be available for purposes outside of ONEX, a user must ensure their assets are transferred back into the Wallet Account.

## Buy and Burn

### **Introduction**

This section explains the ONEX buy and burn mechanism, including rewards calculation, percentage utilization, and the burn process. This mechanism applies to Onomy’s native coin, NOM.

### **Rewards Calculation & Percentage Utilization**

* Rewards for liquidity pools are calculated as the current value of the drops (shares) minus the initial deposit value. Whenever drops from any pool on ONEX are redeemed, the respective rewards are distributed to the LP (80%), the LP Leaders (10%), and the buy and burn mechanism (10%).
* Along with the other distributions, the distribution to the buy and burn mechanism is paid in the assets that the redeemed drops represent. For example, if drops were redeemed from an ETH/USDT pair on ONEX, then the redeemed drops would provide ETH and USDT to the buy and burn mechanism.&#x20;

### **Buy and Burn Process**

* Using the received assets from the redeemed drops, market orders are placed on pairs with NOM to buy NOM tokens on ONEX. Following this example, the ETH would be used to enter market buys of NOM on the NOM/ETH pair and the USDT would be used to enter market buys of NOM on the NOM/USDT pair.&#x20;
* The purchased NOM are then programmatically destroyed via the \`Burn\` coins function.&#x20;
* If a pair does not yet exist between NOM and the asset received from a redeemed drop, then the funds remain in the exchange module address until the pair is created.
* Once the missing pair is created, the buy and burn mechanism will be triggered upon any drop redemption.

For a more detailed look at ONEX, check [Under The Hood](under-the-hood.md)

