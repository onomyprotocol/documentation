# Tendermint BFT



### State machine

At its core, a blockchain is a [replicated deterministic state machine (opens new window)](https://en.wikipedia.org/wiki/State\_machine\_replication).

A state machine is a computer science concept whereby a machine can have multiple states, but only one at any given time. There is a `state`, which describes the current state of the system, and `transactions`, that trigger state transitions.

Given a state S and a transaction T, the state machine will return a new state S'.

![​ Cosmos SDK Tendermint Documentation](<../.gitbook/assets/image (17).png>)

In practice, the transactions are bundled in blocks to make the process more efficient. Given a state S and a block of transactions B, the state machine will return a new state S'.

![​ Cosmos SDK Tendermint Documentation](<../.gitbook/assets/image (11) (1).png>)

In a blockchain context, the state machine is deterministic. This means that if a node is started at a given state and replays the same sequence of transactions, it will always end up with the same final state.

The Cosmos SDK gives developers maximum flexibility to define the state of their application, transaction types and state transition functions. The process of building state-machines with the SDK will be described more in depth in the following sections. But first, let us see how the state-machine is replicated using **Tendermint**.

### [#](https://docs.cosmos.network/v0.44/intro/sdk-app-architecture.html#tendermint) Tendermint <a href="#tendermint" id="tendermint"></a>

Thanks to the Cosmos SDK, developers just have to define the state machine, and [_Tendermint_ (opens new window)](https://tendermint.com/docs/introduction/what-is-tendermint.html) will handle replication over the network for them.

![Cosmos SDK Tendermint Documentation](<../.gitbook/assets/image (15).png>)

[Tendermint (opens new window)](https://docs.tendermint.com/v0.34/introduction/what-is-tendermint.html) is an application-agnostic engine that is responsible for handling the _networking_ and _consensus_ layers of a blockchain. In practice, this means that Tendermint is responsible for propagating and ordering transaction bytes. Tendermint Core relies on an eponymous Byzantine-Fault-Tolerant (BFT) algorithm to reach consensus on the order of transactions.

The Tendermint [consensus algorithm (opens new window)](https://docs.tendermint.com/v0.34/introduction/what-is-tendermint.html#consensus-overview) works with a set of special nodes called _Validators_. Validators are responsible for adding blocks of transactions to the blockchain. At any given block, there is a validator set V. A validator in V is chosen by the algorithm to be the proposer of the next block. This block is considered valid if more than two thirds of V signed a [_prevote (opens new window)_](https://docs.tendermint.com/v0.34/spec/consensus/consensus.html#prevote-step-height-h-round-r) and a [_precommit (opens new window)_](https://docs.tendermint.com/v0.34/spec/consensus/consensus.html#precommit-step-height-h-round-r) on it, and if all the transactions that it contains are valid. The validator set can be changed by rules written in the state-machine.

### [#](https://docs.cosmos.network/v0.44/intro/sdk-app-architecture.html#abci) ABCI <a href="#abci" id="abci"></a>

Tendermint passes transactions to the application through an interface called the [ABCI (opens new window)](https://docs.tendermint.com/v0.34/spec/abci/), which the application must implement.

![Cosmos SDK Tendermint Documentation](<../.gitbook/assets/image (13).png>)

Note that **Tendermint only handles transaction bytes**. It has no knowledge of what these bytes mean. All Tendermint does is order these transaction bytes deterministically. Tendermint passes the bytes to the application via the ABCI, and expects a return code to inform it if the messages contained in the transactions were successfully processed or not.

Here are the most important messages of the ABCI:

* `CheckTx`: When a transaction is received by Tendermint Core, it is passed to the application to check if a few basic requirements are met. `CheckTx` is used to protect the mempool of full-nodes against spam transactions. A special handler called the [`AnteHandler`](https://docs.cosmos.network/v0.44/basics/gas-fees.html#antehandler) is used to execute a series of validation steps such as checking for sufficient fees and validating the signatures. If the checks are valid, the transaction is added to the [mempool (opens new window)](https://docs.tendermint.com/v0.34/tendermint-core/mempool.html#mempool) and relayed to peer nodes. Note that transactions are not processed (i.e. no modification of the state occurs) with `CheckTx` since they have not been included in a block yet.
* `DeliverTx`: When a [valid block (opens new window)](https://docs.tendermint.com/v0.34/spec/blockchain/blockchain.html#validation) is received by Tendermint Core, each transaction in the block is passed to the application via `DeliverTx` in order to be processed. It is during this stage that the state transitions occur. The `AnteHandler` executes again along with the actual [`Msg` service](https://docs.cosmos.network/v0.44/building-modules/msg-services.html) RPC for each message in the transaction.
* `BeginBlock`/`EndBlock`: These messages are executed at the beginning and the end of each block, whether the block contains transaction or not. It is useful to trigger automatic execution of logic. Proceed with caution though, as computationally expensive loops could slow down your blockchain, or even freeze it if the loop is infinite.
