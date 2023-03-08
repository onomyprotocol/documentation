# Overview & Concepts

_Disclaimer:  Mechanisms are susceptible to change._

### Onomy DAO (Governance) <a href="#onomy-dao-governance" id="onomy-dao-governance"></a>

DAO (Decentralized Autonomous Organization) is an organization represented by rules encoded as a transparent computer program, controlled by the organization members, and not influenced by a central governing entity. As the rules are embedded into the code, no managers are needed, thus removing any bureaucracy or hierarchy hurdles.Onomy will be governed by the Onomy DAO**, providing NOM holders with the opportunity to guide the decision-making process through NOM-weighed votes**.

#### Governance Actions <a href="#governance-actions" id="governance-actions"></a>

* **Submit Proposals:** Any holder of $NOM can submit a proposal as long as they put up the minimum deposit of 500 $NOM. Once submitted, it automatically enters the voting period.
* **Voting:** Delegators and validators who stake the governance coin $NOM are welcome to vote on any and all proposals that are currently running (has reached MinDeposit).
* **Voting Override:** Delegators override their validator's vote by placing a vote themselves, otherwise they will inherit their validator's vote.
* **Claiming deposit:** Users that deposited on proposals can recover their deposits if the proposal was accepted OR if the proposal never entered voting period.

**The governance process is divided in a few steps that are outlined below:**

* **Proposal submission:** Proposal is submitted to the blockchain with a deposit.
* **Vote:** Once deposit reaches a certain value (`MinDeposit`), proposal is confirmed and vote opens. Bonded NOM holders can then send `TxGovVote` transactions to vote on the proposal.
* **Execution** After a period of time, the votes are tallied and depending on the result, the messages in the proposal will be executed.

## Proposal submission

#### [#](https://docs.cosmos.network/v0.46/modules/gov/01\_concepts.html#right-to-submit-a-proposal) Right to submit a proposal <a href="#right-to-submit-a-proposal" id="right-to-submit-a-proposal"></a>

Every account can submit proposals by sending a `MsgSubmitProposal` transaction. Once a proposal is submitted, it is identified by its unique `proposalID`.

#### [#](https://docs.cosmos.network/v0.46/modules/gov/01\_concepts.html#proposal-messages) Proposal Messages <a href="#proposal-messages" id="proposal-messages"></a>

A proposal includes an array of `sdk.Msg`s which are executed automatically if the proposal passes. The messages are executed by the governance `ModuleAccount` itself. Modules such as `x/upgrade`, that want to allow certain messages to be executed by governance only should add a whitelist within the respective msg server, granting the governance module the right to execute the message once a quorum has been reached. The governance module uses the `MsgServiceRouter` to check that these messages are correctly constructed and have a respective path to execute on but do not perform a full validity check.

### [#](https://docs.cosmos.network/v0.46/modules/gov/01\_concepts.html#deposit) Deposit <a href="#deposit" id="deposit"></a>

To prevent spam, proposals must be submitted with a deposit in the coins defined by the `MinDeposit` param.

When a proposal is submitted, it has to be accompanied with a deposit that must be strictly positive, but can be inferior to `MinDeposit`. The submitter doesn't need to pay for the entire deposit on their own. The newly created proposal is stored in an _inactive proposal queue_ and stays there until its deposit passes the `MinDeposit`. Other token holders can increase the proposal's deposit by sending a `Deposit` transaction. If a proposal doesn't pass the `MinDeposit` before the deposit end time (the time when deposits are no longer accepted), the proposal will be destroyed: the proposal will be removed from state and the deposit will be burned (see x/gov `EndBlocker`). When a proposal deposit passes the `MinDeposit` threshold (even during the proposal submission) before the deposit end time, the proposal will be moved into the _active proposal queue_ and the voting period will begin.

The deposit is kept in escrow and held by the governance `ModuleAccount` until the proposal is finalized (passed or rejected).

#### [#](https://docs.cosmos.network/v0.46/modules/gov/01\_concepts.html#deposit-refund-and-burn) Deposit refund and burn <a href="#deposit-refund-and-burn" id="deposit-refund-and-burn"></a>

When a proposal is finalized, the coins from the deposit are either refunded or burned according to the final tally of the proposal:

* If the proposal is approved or rejected but _not_ vetoed, each deposit will be automatically refunded to its respective depositor (transferred from the governance `ModuleAccount`).
* When the proposal is vetoed with greater than 1/3, deposits will be burned from the governance `ModuleAccount` and the proposal information along with its deposit information will be removed from state.
* All refunded or burned deposits are removed from the state. Events are issued when burning or refunding a deposit.

## Vote

#### [#](https://docs.cosmos.network/v0.46/modules/gov/01\_concepts.html#participants) Participants <a href="#participants" id="participants"></a>

_Participants_ are users that have the right to vote on proposals. On the Onomy Network, participants are bonded NOM holders. Unbonded NOM holders and other users do not get the right to participate in governance. However, they can submit and deposit on proposals.

Note that some _participants_ can be forbidden to vote on a proposal under a certain validator if:

* _participant_ bonded or unbonded NOM to said validator after proposal entered voting period.
* _participant_ became validator after proposal entered voting period.

This does not prevent _participant_ to vote with NOM bonded to other validators. For example, if a _participant_ bonded some NOM to validator A before a proposal entered voting period and other NOM to validator B after proposal entered voting period, only the vote under validator B will be forbidden.

#### [#](https://docs.cosmos.network/v0.46/modules/gov/01\_concepts.html#voting-period) Voting period <a href="#voting-period" id="voting-period"></a>

Once a proposal reaches `MinDeposit`, it immediately enters `Voting period`. We define `Voting period` as the interval between the moment the vote opens and the moment the vote closes. `Voting period` should always be shorter than `Unbonding period` to prevent double voting. The `Voting period` is 3 Days.

#### [#](https://docs.cosmos.network/v0.46/modules/gov/01\_concepts.html#option-set) Option set <a href="#option-set" id="option-set"></a>

The option set of a proposal refers to the set of choices a participant can choose from when casting its vote.

The initial option set includes the following options:

* `Yes`
* `No`
* `NoWithVeto`
* `Abstain`

`NoWithVeto` counts as `No` but also adds a `Veto` vote. `Abstain` option allows voters to signal that they do not intend to vote in favor or against the proposal but accept the result of the vote.

#### Quorum <a href="#quorum" id="quorum"></a>

Quorum is defined as the minimum percentage of voting power that needs to be casted on a proposal for the result to be valid.

#### [#](https://docs.cosmos.network/v0.46/modules/gov/01\_concepts.html#threshold) Threshold <a href="#threshold" id="threshold"></a>

Threshold is defined as the minimum proportion of `Yes` votes (excluding `Abstain` votes) for the proposal to be accepted.

Initially, the threshold is set at 50% of `Yes` votes, excluding `Abstain` votes. A possibility to veto exists if more than 1/3rd of all votes are `NoWithVeto` votes. Note, both of these values are derived from the `TallyParams` on-chain parameter, which is modifiable by governance. This means that proposals are accepted iff:

* There exist bonded tokens.
* Quorum has been achieved.
* The proportion of `Abstain` votes is inferior to 1/1.
* The proportion of `NoWithVeto` votes is inferior to 1/3, including `Abstain` votes.
* The proportion of `Yes` votes, excluding `Abstain` votes, at the end of the voting period is superior to 1/2.

#### [#](https://docs.cosmos.network/v0.46/modules/gov/01\_concepts.html#inheritance) Inheritance <a href="#inheritance" id="inheritance"></a>

If a delegator does not vote, it will inherit its validator vote.

* If the delegator votes before its validator, it will not inherit from the validator's vote.
* If the delegator votes after its validator, it will override its validator vote with its own. If the proposal is urgent, it is possible that the vote will close before delegators have a chance to react and override their validator's vote. This is not a problem, as proposals require more than 2/3rd of the total voting power to pass before the end of the voting period. Because as little as 1/3 + 1 validation power could collude to censor transactions, non-collusion is already assumed for ranges exceeding this threshold.

#### [#](https://docs.cosmos.network/v0.46/modules/gov/01\_concepts.html#validator-s-punishment-for-non-voting) Validator’s punishment for non-voting <a href="#validator-s-punishment-for-non-voting" id="validator-s-punishment-for-non-voting"></a>

At present, validators are not punished for failing to vote.

## Software Upgrade <a href="#software-upgrade" id="software-upgrade"></a>

If proposals are of type `SoftwareUpgradeProposal`, then nodes need to upgrade their software to the new version that was voted. This process is divided into two steps:

#### [#](https://docs.cosmos.network/v0.46/modules/gov/01\_concepts.html#signal) Signal <a href="#signal" id="signal"></a>

After a `SoftwareUpgradeProposal` is accepted, validators are expected to download and install the new version of the software while continuing to run the previous version. Once a validator has downloaded and installed the upgrade, it will start signaling to the network that it is ready to switch by including the proposal's `proposalID` in its _precommits_.(_Note: Confirmation that we want it in the precommit?_)

Note: There is only one signal slot per _precommit_. If several `SoftwareUpgradeProposals` are accepted in a short timeframe, a pipeline will form and they will be implemented one after the other in the order that they were accepted.

#### [#](https://docs.cosmos.network/v0.46/modules/gov/01\_concepts.html#switch) Switch <a href="#switch" id="switch"></a>

Once a block contains more than 2/3rd _precommits_ where a common `SoftwareUpgradeProposal` is signaled, all the nodes (including validator nodes, non-validating full nodes and light-nodes) are expected to switch to the new version of the software.

#### &#x20;<a href="#governance-address" id="governance-address"></a>
