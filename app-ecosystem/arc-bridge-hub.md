---
description: Empowering an enhanced cross-chain experience.
---

# Arc Bridge Hub

Arc is a hub-and-spoke model bridge that enables users to transfer tokens from an integrated chain to Onomy and back again by locking up tokens on integrated chain side, and minting equivalent tokens on the Onomy side. Arc is completely non-custodial, you only need to trust in the security of the Onomy chain itself - not some third party bridge administrators who could run off with your funds. The security of the Onomy chain, and thus Arc, is through the Onomy Validator Guild (OVG) which is comprised of a decentralized network of globally situated independent validator firms. Learn more about the criteria to join as a validator [here](../validators-staking/onomy-validator-guild-ovg.md).\
\
Arc integrates chains such as Avalanche, Aurora, Polygon, Moonbeam and others to create a more inclusive multi-chain DeFi hub. Arc pairs with the Onomy ecosystem of applications including the Onomy Exchange (hybrid orderbook + AMM DEX) and Onomy Access (multi-chain mobile wallet application) + more as the Onomy Ecosystem grows.\
\
The Arc Bridge Hub code repository can be found [here](https://github.com/onomyprotocol/arc).

### Mint & Burn Mechanism

The task of any blockchain bridge is to hold tokens on one chain and issue representative assets on the other chain. Unlocking the assets at some point in the future when they are sent back across the bridge.

Since Arc is a bi-directional bridge that handles two asset sources, Cosmos and Ethereum tokens may be held on Ethereum or held on Cosmos based on where they originated. Likewise tokens are minted on both sides as well, to represent the assets from the other chain. The same process occurs between other integrated blockchains.

### Ethereum Based Assets

#### Ethereum -> Cosmos

An Ethereum asset implementing the ERC20 standard is deposited into the [Gravity.sol](https://github.com/onomyprotocol/arc/blob/main/solidity/contracts/Gravity.sol) contract using the Solidity function call `sendToCosmos`. This locks the ERC20 asset in the Gravity.sol contract where it will remain until it is withdrawn at some undetermined point in the future.

At the send of the `sendToCosmos` function call an Ethereum event `SendToCosmosEvent` executes. This event contains the amount and type of tokes, as well as a destination address on Cosmos for the funds.

The validators all run their oracle processes which submit `MsgDepositClaim` messages describing the deposit they have observed. Once more than 66% of all voting power has submitted a claim for this specific deposit representative tokens are minted and issued to the Cosmos address that the sender requested.

For further details on oracle operation see [oracle](https://github.com/onomyprotocol/arc/blob/main/docs/design/oracle.md) documentation.

#### Cosmos -> Ethereum

The Cosmos representation of the Ethereum ERC20 token, hereafter called a `voucher`, has been used and exchanged on the Cosmos chain for some time. Eventually some owner wishes to cash in the `voucher` and withdraw the actual asset on Ethereum.

To do this they send a `MsgSendToEth` on the Cosmos chain. This removes the `voucher` from the users account and places a transaction into a transaction pool of `MsgSendToEth` messages for that same ERC20 token type.

As part of `MsgSendToEth` the user specifies a fee, there is no mandatory minimum fee. But this fee must be paid in the same ERC20 asset they are returning to Ethereum in the default case. This is being adapted to have all bridge fees paid in Onomy's native NOM token.

At some point in the future a relayer will determine that the pool contains enough transactions to bother executing a batch of that type and relay it to Ethereum. For a discussion of edge cases here see the [batch creation spec](https://github.com/onomyprotocol/arc/blob/main/spec/batch-creation-spec.md).

When the batch transaction executes on the Ethereum chain the ERC20 token will be sent out the bridge to the Ethereum destination specified by the user in `MsgSendToEth`. An Ethereum event `TransactionBatchExecutedEvent` will be fired and picked up by the validators oracle processes. Once the resulting `MsgWithdrawClaim` has passed the `voucher` will finally be burned.

Note that batching logic reduces costs dramatically, over 75%, but at the cost of latency and implementation complexity. If a user wishes to withdraw quickly they will have to pay a much higher fee. But this fee will be about the same as the fee every withdraw from the bridge would require in a non-batching system.

### Cosmos Based Assets

#### Cosmos -> Ethereum

A Cosmos asset first must be represented on Ethereum before it's possible to bridge it. To do this the [Gravity.sol](https://github.com/onomyprotocol/arc/blob/main/solidity/contracts/Gravity.sol) contract contains an endpoint called `deployERC20`.

This endpoint is not permissioned. It is possible for anyone to pay for the creation of a new ERC20 representing a Cosmos asset, but it is up to the validators and the Gravity Cosmos module to declare any given ERC20 as the representation of a given asset.

When a user on Ethereum calls `deployERC20` they pass arguments describing the desired asset. [Gravity.sol](https://github.com/onomyprotocol/arc/blob/main/solidity/contracts/Gravity.sol) uses an ERC20 factory to deploy the actual ERC20 contract using a known good code and assigns ownership of the entire balance of the new token to itself before firing a `ERC20DeployedEvent`

The validators oracle processes observe this event and decide if a Cosmos asset has been accurately represented (correct decimals, correct name, no existing representation). If this is the case the ERC20 contract address is adopted and stored as the definitive representation of that Cosmos asset on Ethereum.

For further details on oracle operation see [oracle](https://github.com/onomyprotocol/arc/blob/main/docs/design/oracle.md) documentation.

From this point on the process is nearly identical to the Cosmos -> Ethereum process for Ethereum originated assets.

The user sends a `MsgSendToEth` that specifies a fee denominated in the token they are sending across the bridge. (for the reasoning view the [transaction batch rewards](https://github.com/onomyprotocol/arc/blob/main/docs/design/mint-and-lock.md/###transaction-batch-rewards))

At some point in the future a relayer will determine that the pool contains enough transactions to bother executing a batch of that type and relay it to Ethereum. For a discussion of edge cases here see the [batch creation spec](https://github.com/onomyprotocol/arc/blob/main/spec/batch-creation-spec.md)

When the batch transaction executes on the Ethereum chain the ERC20 token will be sent out the bridge to the Ethereum destination specified by the user in `MsgSendToEth`. An Ethereum event `TransactionBatchExecutedEvent` will be fired and picked up by the validators oracle processes. Once the resulting `MsgWithdrawClaim` has passed the Cosmos asset will be locked in the Gravity module to later be unlocked once it is returned to Cosmos.

The tl;dr of this process is that the 'minting' of Cosmos assets on Ethereum is done all at once and the total balance simply exists in the [Gravity.sol](https://github.com/onomyprotocol/arc/blob/main/solidity/contracts/Gravity.sol) contract at all times, which 'mints' them by sending them out and 'burns' them by locking them up again.

As a related consequence this means that the supply of a given Ethereum asset on Cosmos is correct, but the supply of a given Cosmos asset on Ethereum is the total number of tokens minus the number in the Gravity bridge address.

#### Ethereum -> Cosmos

This is identical to Ethereum -> Cosmos for Ethereum based assets described above.

### Security

The **Validator Set** is the actual set of keys with stake behind them, which are slashed for double-signs or other misbehavior. We typically consider the security of a chain to be the security of a _Validator Set_. This varies on each chain, but is our gold standard. Even IBC offers no more security than the minimum of both involved Validator Sets.

The **Eth Signer** is a binary run alongside the main Cosmos daemon (`gaiad` or equivalent) by the validator set. It exists purely as a matter of code organization and is in charge of signing Ethereum transactions, as well as observing events on Ethereum and bringing them into the Cosmos state. It signs transactions bound for Ethereum with an Ethereum key, and signs over events coming from Ethereum with a Cosmos SDK key. We can add slashing conditions to any mis-signed message by any _Eth Signer_ run by the _Validator Set_ and be able to provide the same security as the _Valiator Set_, just a different module detecting evidence of malice and deciding how much to slash. If we can prove a transaction signed by any _Eth Signer_ of the _Validator Set_ was illegal or malicious, then we can slash on the Cosmos chain side and potentially provide 100% of the security of the _Validator Set_. Note that this also has access to the 3 week unbonding period to allow evidence to slash even if they immediately unbond.

The **MultiSig Set** is a (possibly aged) mirror of the _Validator Set_ but with Ethereum keys, and stored on the Ethereum contract. If we ensure the _MultiSig Set_ is updated much more often than the unbonding period (eg at least once per week), then we can guarantee that all members of the _MultiSig Set_ have slashable atoms for misbehavior. However, in some extreme cases of stake shifting, the _MultiSig Set_ and _Validator Set_ could get quite far apart, meaning there is many of the members in the _MultiSig Set_ are no longer active validators and may not bother to transfer Eth messages. Thus, to avoid censorship attacks/inactivity, we should also update this everytime there is a significant change in the Validator Set (eg. > 3-5%). If we maintain those two conditions, the MultiSig Set should offer a similar level of security as the Validator Set.

### History

Onomy Arc is a bridge extended from AltheaNet's Gravity Bridge that was designed to run on the [Cosmos SDK blockchains](https://github.com/cosmos/cosmos-sdk) like the [Cosmos Hub](https://github.com/cosmos/gaia) focused on maximum design simplicity and efficiency. While initially a Cosmos <-> Ethereum bridge, Onomy has extended Gravity Bridge functionality, integrated chains, performance, and security with Arc. Additional functionality and integrations are audited by NCC Group.
