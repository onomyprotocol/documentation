# Transaction Fees (Gas)

### Gas Fees

Gas is a small computational fee that covers the cost of processing a transaction added on to all transactions to avoid spamming. Validators set minimum gas prices and reject transactions that have implied gas prices below this threshold. Any transaction that does not contain enough gas will not be processed.&#x20;

* Validators can set their own minimum gas fees.
* Most transactions will estimate more than the minimum amount of gas, ensuring the transaction gets completed.
* Unused gas is not refunded.
* Transactions are not queued based on gas amounts, but in the order received.

`gas` is a special unit that is used to track the consumption of resources during execution. It serves two main purposes:

* Makes sure blocks are not consuming too many resources and will be finalized.&#x20;
* Prevents spam and abuse from end-user. To this end, `gas` consumed during `message` execution is typically priced, resulting in a `fee` (`fees = gas * gas-prices`). `fees` generally have to be paid by the sender of the `message`.

Every block, gas fees are sent to the reward pool and dispersed to validators who distribute them to delegators in the form of staking rewards.
