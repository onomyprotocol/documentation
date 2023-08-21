# NOM's Bonding Curve (BC)

**Be mindful that the ONLY OFFICIAL LINK AND CONTRACT ADDRESS ARE:**&#x20;

**Link:** [**https://bnom.onomy.io/**](https://bnom.onomy.io)

**Bonding Curve Contract: `0x264C13cfEd981e3137Fb43B198D14D8D5D64977E`**

[**Learn everything about interacting with the mainnet Bonding Curve via this tutorial.** ](https://onomy.notion.site/onomy/The-Onomy-Bonding-Curve-A-Step-by-Step-Tutorial-fa4d92c142cc4eb68e3eabb4a2d6a46b#a0b52eb7c79144a5bc3ec969abb742f9)

## Definition

Bonding curves are crypto-economic token models that automate the relationship between price and supply. \
\
It is a decentralized automated market maker that is on-chain with community-driven liquidity. Liquidity within the automated market maker cannot be controlled by any central entity. The code cannot be adjusted after its deployment. The tokens in this model are referred to as Continuous Tokens because their price is continuously calculated. In continuous token models, there is no ICO or token launch. Instead of pre-selling tokens during a launch phase, the tokens are minted continuously over time via an automated market maker contract. Tokens are minted when purchased as needed, in conjunction with demand, and used within the protocol or application when required or desired. Continuous Tokens have other properties such as instant liquidity and deterministic price. Bonding curves act as an automated market maker such that token buyers and sellers have an instant market. Additionally, bonding curve models don’t have central authorities responsible for issuing the tokens. Instead, users can buy a project’s token through a smart contract platform. The cost to buy these tokens is determined by the supply of those tokens. Unlike traditional models, the cost of these tokens increases as the supply increases. This price is determined by a pre-existing algorithm, further described below. A fee of 1% is applied per trade with the bonding curve.\
\
The Bonding Curve Contract has been audited by NCC Group, an enterprise-level cybersecurity firm headquartered in the United Kingdom.

## bNOM to NOM Distribution

The Onomy Protocol token (_**NOM**_) is primarily distributed through a bonding curve contract deployed on the Ethereum Network where bondingNOM (_**bNOM**_) tokens are purchased that are bridged 1:1 for _NOM_ on the Onomy Network. _NOM_ is the native utility token of the Onomy Network. An interface is provided for this exchange at [bco.onomy.io](https://bco.onomy.io) - commonly referenced as "bridging" b_NOM_ tokens from Ethereum and into the Onomy ecosystem for native _NOM_. When bridged, b_NOM_ is burned from the bonding curve, and an equivalent amount of _NOM_ is provided on the Onomy Network.

The following is the equation governing the bonding curve:

![](<../.gitbook/assets/image (6) (2).png>)

100,000,000 is the supply of _bNOM_ loaded into the bonding curve. Supply is provided as it is purchased, thereby acting as a minting mechanism over time. The emission of tokens requires significant capital to prevent over-dilution of the NOM circulating supply.

![](<../.gitbook/assets/image (7) (2).png>)

## Distribution Benefits

The bonding curve has many benefits:

* **Deterministic Price:** The buy and sell prices of tokens increase and decrease along the price curve.
* **Continuous Price:** The price of token _n_ is less than token _n+1_ and more than _n-1._
* **Instant Liquidity:** Tokens can be bought or sold instantaneously at any time, with the bonding curve acting as an automated market maker. A bonding curve contract acts as the counterparty of the transaction and always holds enough ETH in reserve to buy **bNOM** back. _Note that once bNOM is bridged for NOM, it cannot be bridged back for bNOM._
* **Collateralization:** In conjunction with the staking inflation curve, ETH acts as reserve backing of the b_NOM_.

## Staking & Bridge Incentives

The Onomy Network Staking Curve carries a tight relationship with the Bonding Curve. The staking curve acts as a principal motivator for participants who purchased _bNOM_ to bridge from Ethereum to the Onomy Network to capture the opportunity to obtain staking rewards. _NOM_ holders can either choose to become validators themselves, or delegate their _NOM_ to a validator to earn staking rewards minus a validator fee.

![](<../.gitbook/assets/image (8) (1).png>)

The staking rewards incentivize _bNOM_ on the bonding curve to bridge into _NOM,_ thereby driving value into the Onomy Ecosystem. Once bridged, bonding curve participants formalize their entry into Onomy and relinquish the ability to sell back to the bonding curve. Liquidity for NOM is not guaranteed, nor are secondary markets.

The bonding curve is perpetual until all 100M _bNOM_ are issued.&#x20;

## Audits

{% file src="../.gitbook/assets/NCC_Group_Onomy_LLC_CustomerFacingDocument-4-23-21.pdf" %}
All findings were fixed and three informational findings were accepted.&#x20;
{% endfile %}

##

### Legal Disclaimer

{% hint style="info" %}
_Nothing in this post shall constitute or be construed as an offer to sell or the solicitation of an offer to purchase any securities. Nothing in this post shall be construed as investment advice, strategy, or investment recommendations by the Onomy DAO or any of its contributors and their affiliates. This post is for informational purposes only._

_This communication may contain forward-looking statements that are based on beliefs and assumptions based on information currently available. In some cases, you can identify forward-looking statements by the following words: “will,” “expect,” “would,” “intend,” “believe,” "anticipate," or other comparable terminology. These statements involve risks, uncertainties, assumptions, and other factors that may cause actual results or performance to be materially different. Onomy DAO, its contributors, or any of their affiliates cannot assure you that the forward-looking statements will prove to be accurate. These forward-looking statements speak only as of the date hereof. Onomy DAO, its contributors, or any of their affiliates disclaims any obligation to update these forward-looking statements._
{% endhint %}

### Revenue Disclosure

{% hint style="info" %}
_The Bonding Curve Automated Market Maker (AMM) earns revenue in two ways. The first is a 1% transaction fee applied to each trade with the AMM. The second is the release of Ether used to purchase bNOM in proportion to the amount of bNOM bridged to the Onomy Network. These two methods together constitute the "Revenue" of the bonding curve. Revenue is currently distributed to PENDULUM LABS LIMITED as voted by the Onomy DAO Proposal 5. PENDULUM LABS LIMITED_ may only obtain the Revenue in the manner described above and has no ability to change, manage, or otherwise alter the Bonding Curve smart contract that has been deployed to the Ethereum Mainnet.&#x20;
{% endhint %}

### Risk Disclosure

{% hint style="info" %}
_The Bonding Curve Automated Market Maker is a decentralized protocol that people can use to create liquidity and trade bNOM tokens, as well as convert bNOM (ERC-20) for NOM tokens on the Onomy Network one-for-one. Your use of the Bonding Curve Contract and its Platform involves various risks, including, but not limited to, losses due to the fluctuation of prices of tokens in a trading pair or smart contract vulnerabilities. You are responsible for doing your own diligence on those interfaces to understand the fees and risks they present._

_THE BONDING CURVE CONTRACT AND PLATFORM IS PROVIDED "AS IS", AT YOUR OWN RISK, AND WITHOUT WARRANTIES OF ANY KIND. Although PENDULUM LABS LIMITED developed the code, it does not provide, own, or control the Bonding Curve Contract or Platform, which is run by smart contracts deployed on the blockchain. No developer or entity involved in creating the Bonding Curve Contract or Platform will be liable for any claims or damages whatsoever associated with your use, inability to use, or your interaction with other users of, the Bonding Curve Contract or Platform, including any direct, indirect, incidental, special, exemplary, punitive or consequential damages, or loss of profits, cryptocurrencies, tokens, or anything else of value._
{% endhint %}
