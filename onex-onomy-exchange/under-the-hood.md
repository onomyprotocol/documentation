# Under The Hood

This section provides a technical overview of the underlying mechanisms that power the Onomy Exchange (ONEX), including the AMM formula, order book, spread capture, rewards calculation, and the buy and burn function.

## **Orderbook + AMM DEX**

The Onomy Exchange combines an order book model with an Automated Market Maker (AMM) model. The order book records all the current buy and sell orders, while the AMM provides liquidity through liquidity pools. This hybrid approach aims to offer the efficiency of traditional order books with the decentralized benefits of AMMs.

#### **AMM Formula**

In the Onomy Exchange (ONEX), the Automated Market Maker (AMM) utilizes a constant product formula  $$k = XY$$ to determine the price of assets within a liquidity pool. This formula is fundamental to the operation of many decentralized exchanges and ensures a continuous and deterministic pricing model.&#x20;

The price in the ONEX AMM is determined by the ratio of the two balances in the liquidity pool, represented by $$X$$ and $$Y$$, where $$X$$ and $$Y$$ are the quantities of the two tokens in the pool. The price for one token in terms of the other is given by $$price= X / Y$$

#### **Break-Even Price and Spread**

The break-even price is the minimum price at which the AMM must transact to maintain the constant product invariant. During trade executions on ONEX, the design ensures that the AMM always charges a price higher than this break-even price. This difference between the actual price and the break-even price can be considered a spread, which contributes to ONEX's platform revenue used for rewarding liquidity providers, leaderboard rewards, and the programmatic buy and burn of NOM.

#### **Implications**

The constant product formula ensures that the price automatically adjusts based on the pool's balances. As trades occur and the quantities of $$X$$ and $$Y$$ change, the price dynamically shifts to maintain the invariant. This design promotes liquidity and fair pricing, while the spread capture mechanism aligns incentives for liquidity providers and the platform.

## **Orderbook Spread Capture**

#### **Spread Capture Mechanism**

In ONEX, orderbook spread capture is defined as the differential between the AMM-determined price and the limit order price recorded in the order book. As the AMM price fluctuates in response to market dynamics, it may surpass the limit order price. When this occurs, the limit order is executed, and the difference between the AMM price and the limit price is captured.

**Democratizing Market Making**

This innovative mechanism democratizes a function traditionally reserved for sophisticated market makers employing complex high-frequency trading algorithms. By making spread capture accessible to all liquidity providers within ONEX, regardless of the amount, it levels the playing field and opens opportunities that were previously exclusive to institutional players.

## **Rewards Calculation Mechanism**

The Onomy Exchange (ONEX) employs a specialized mechanism for calculating rewards for liquidity providers shares, referred to as "drops." This process is integral to incentivizing participation in liquidity pools. Below is a detailed explanation of the rewards calculation process in ONEX.

**Step 1: Calculation of Total Coins Owned by the Drop**

The total amount of each coin owned by a specific drop is determined based on the proportion of the drop to the total drops in the pool, multiplied by the balance of the respective coin:

$$
totalCoinA = (# of drops / # of drops in pool) * balance of Coin A 
totalCoinB = (# of drops / # of drops in pool) * balance of Coin B
$$

**Step 2: Calculation of the Final Product**

The rewards are calculated by comparing the constant product of the two amounts of coins deposited when the drop was created to the constant product at redemption. The initial constant product is stored with the drop upon creation, and the final constant product at redemption is calculated as:

$$
productFinal = totalCoinA * totalCoinB
$$

**Step 3: Calculation of the Drop Rewards**

The rewards for each coin are then calculated using the ratio of the difference between the final and initial constant products to the final constant product:

$$
rewardCoinA = totalCoinA * (productFinal - productInitial)/productFinal
$$

$$
rewardCoinB = totalCoinB * (productFinal - productInitial)/productFinal
$$

**Step 4: Distribution of Principal and Rewards**

The principal is returned in full to the liquidity provider, while the rewards are distributed as follows:

* 80% of the rewards go to the liquidity provider
* 20% of the rewards are allocated to pool leaders and the burn function

The principal for Coin A is calculated as:

$$
principalCoinA = totalCoinA - rewardCoinA
$$

ONEX's rewards calculation mechanism offers a transparent and systematic approach to incentivizing liquidity provision. By aligning the rewards with the constant product formula and providing a clear distribution model, it fosters participation and trust among liquidity providers.

## **Buy and Burn Mechanism**

#### **Module Accounts and Permissions**

In the Cosmos SDK, module accounts enable the interaction between `auth.Accounts` and other module accounts, facilitating the transfer, allocation, minting, and burning of tokens. Each module account is associated with a specific set of permissions, defining its capabilities and constraints.

The Bank module in the Cosmos SDK introduces a specialized type of `auth.Account`, known as a `ModuleAccount`, designed to be utilized by modules for various token-related operations. These operations are governed by permissions that must be registered upon the creation of a supply Keeper.

#### **Burner Permission**

One of the available permissions within the Bank module is the "Burner" permission. This permission grants a module the capability to burn a specific amount of coins, effectively removing them from circulation. The permission ensures that the burn operation is conducted securely and in accordance with the predefined rules of the network.

#### **Implementation in ONEX**

ONEX leverages the Burner permission within the Cosmos SDK's Bank module to execute its buy and burn function. This process involves the following steps:

1. **Asset Utilization**: Upon each completed trade with the AMM on any pair, the corresponding assets are received as part of the rewards distribution to the Buy and Burn Mechanism. These assets are then utilized to programmatically place market orders on pairs with NOM within ONEX
   * **Example Execution**: Following a specific example, if ETH and USDT are received from the AMM rewards, they would be used to enter market buys of NOM on the respective NOM/ETH and NOM/USDT pairs
2. **Burning Process**: The purchased NOM tokens are programmatically destroyed via the `Burn` coins function within the Cosmos SDK's Bank module.
3. **No Central Management:** This operation within ONEX is executed through an autonomous and programmatic process, adhering to the predefined permissions set within the Cosmos SDK's Bank module. This decentralized design ensures that the burn function aligns with the network's protocols, without any central oversight or intervention to enact the process.&#x20;

**Handling of Non-Existent Pairs**

1. **Temporary Holding**: If a trading pair does not yet exist between NOM and the asset received from a redeemed drop, the funds are temporarily held within the exchange module address.
2. **Pair Creation**: Once the missing pair is created, the funds will activate upon a trigger event.
3. **Triggering Buy and Burn**: The buy and burn mechanism is then triggered upon any subsequent drop redemption, utilizing the previously held funds to execute the market buys.

Embedding this functionality directly into the protocol ensures a transparent and programmatic mechanism for token burn without manual intervention, contributing to the stability and integrity of the platform and aligning with the principles of decentralized finance.

## TLA+ Specification

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>
