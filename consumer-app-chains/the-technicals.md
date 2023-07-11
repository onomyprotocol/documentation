# The Technicals

Once a successful establishment of an IBC connection and a suitable channel between a provider and consumer chain is achieved, the provider chain assumes the responsibility of consistently transmitting validator set updates to the consumer chain via IBC. These validator set updates serve as the means for the consumer chain to synchronize and update its own validator set. As a result, the consumer chain effectively replicates the validator set of the provider chain.

In order to uphold the security of the consumer chain, provider delegators are unable to unbond their tokens until the unbonding periods specific to each consumer chain have fully elapsed. Nonetheless, this restriction is generally inconspicuous to the provider delegators due to the configuration of slightly shorter unbonding periods on consumer chains compared to the provider chain.

**Downtime Slashing**

In the event that a validator on a consumer chain initiates downtime, a downtime packet is transmitted to the provider chain to subject the respective validator to a predetermined period of jailing. Consequently, the validator who incurred downtime will forego staking rewards for the designated jailing duration.

**Equivocation (Double Sign) Slashing**

Instances of equivocation necessitate the submission of evidence to the provider chain's governance system for voting. This process serves as an additional safeguard prior to the implementation of slashing penalties on a validator..

### **Tokenomics and Rewards**

Consumer chains possess the freedom to establish their own native tokens, which can be utilized for transaction fees and issued as inflationary rewards within the consumer chain. These rewards serve as incentives to encourage user engagement and participation, such as providing liquidity or engaging in staking activities. A fraction of these fees and rewards is directed towards the stakers of the provider chain. However, the precise proportion is entirely customizable by developers and is subject to governance decisions.

Replicated Security Parametres&#x20;

The parameters necessary for Replicated Security are defined in

* the Params structure in proto/interchain\_security/ccv/provider/v1/provider.proto for the provider;
* the Params structure in proto/interchain\_security/ccv/consumer/v1/consumer.proto for the consumer.

### Time-based parameters

RS relies on the following time-based parameters:

ProviderUnbondingPeriod: is the unbonding period on the provider chain as configured during chain genesis. This parameter can later be changed via governance.

ConsumerUnbondingPeriod: is the unbonding period on the consumer chain.

ConsumerUnbondingPeriod is set via the ConsumerAdditionProposal governance proposal to add a new consumer chain. It is recommended that every consumer chain set and unbonding period shorter than ProviderUnbondingPeriod

Example:

ConsumerUnbondingPeriod = ProviderUnbondingPeriod - one day

Unbonding operations (such as undelegations) are completed on the provider only after the unbonding period elapses on every consumer.

TrustingPeriodFraction: is used to calculate the TrustingPeriod of created IBC clients on both provider and consumer chains.

Setting TrustingPeriodFraction to 0.5 would result in the following:

TrustingPeriodFraction = 0.5

ProviderClientOnConsumerTrustingPeriod = ProviderUnbondingPeriod \* 0.5

ConsumerClientOnProviderTrustingPeriod = ConsumerUnbondingPeriod \* 0.5

Note that a light clients must be updated within the TrustingPeriod in order to avoid being frozen.

For more details, see the[ IBC specification of Tendermint clients](https://github.com/cosmos/ibc/blob/main/spec/client/ics-007-tendermint-client/README.md).

CCVTimeoutPeriod is the period used to compute the timeout timestamp when sending IBC packets.

For more details, see the[ IBC specification of Channel & Packet Semantics](https://github.com/cosmos/ibc/blob/main/spec/core/ics-004-channel-and-packet-semantics/README.md#sending-packets).

If a sent packet is not relayed within this period, then the packet times out. The CCV channel used by the replicated security protocol is closed, and the corresponding consumer is removed.

CCVTimeoutPeriod may have different values on the provider and consumer chains.

* CCVTimeoutPeriod on the provider must be larger than ConsumerUnbondingPeriod
* CCVTimeoutPeriod on the consumer is initial set via the ConsumerAdditionProposal

#### InitTimeoutPeriod

is the maximum allowed duration for CCV channel initialization to execute.

For any consumer chain, if the CCV channel is not established within InitTimeoutPeriod then the consumer chain will be removed and therefore will not be secured by the provider chain.

The countdown starts when the spawn\_time specified in the ConsumerAdditionProposal is reached.

#### VscTimeoutPeriod

is the provider-side param that enables the provider to timeout VSC packets even when a consumer chain is not live. If the VscTimeoutPeriod is ever reached for a consumer chain that chain will be considered not live and removed from replicated security.

VscTimeoutPeriod MUST be larger than the ConsumerUnbondingPeriod.

BlocksPerDistributionTransmission is the number of blocks between rewards transfers from the consumer to the provider.

TransferPeriodTimeout is the period used to compute the timeout timestamp when sending IBC transfer packets from a consumer to the provider.

If this timeout expires, then the transfer is attempted again after BlocksPerDistributionTransmission blocks.

* TransferPeriodTimeout on the consumer is initial set via the ConsumerAdditionProposal gov proposal to add the consumer
* TransferPeriodTimeout should be smaller than BlocksPerDistributionTransmission x avg\_block\_time

### Slash Throttle Parameters

SlashMeterReplenishPeriod exists on the provider such that once the slash meter becomes not-full, the slash meter is replenished after this period has elapsed.

The meter is replenished to an amount equal to the slash meter allowance for that block, or SlashMeterReplenishFraction \* CurrentTotalVotingPower.

SlashMeterReplenishFraction exists on the provider as the portion (in range \[0, 1]) of total voting power that is replenished to the slash meter when a replenishment occurs.

This param also serves as a maximum fraction of total voting power that the slash meter can hold. The param is set/persisted as a string, and converted to a sdk.Dec when used.

MaxThrottledPackets exists on the provider as the maximum amount of throttled slash or vsc matured packets that can be queued from a single consumer before the provider chain halts, it should be set to a large value.

This param would allow provider binaries to panic deterministically in the event that packet throttling results in a large amount of state-bloat. In such a scenario, packet throttling could prevent a violation of safety caused by a malicious consumer, at the cost of provider liveness.

For a technical deep dive into the replicated security protocol, see the[ specification](https://github.com/cosmos/ibc/blob/main/spec/app/ics-028-cross-chain-validation/README.md).

### Features

#### **Replicated Security Key Assignment**

Key Assignment, also referred to as key delegation, enables validator operators to employ distinct consensus keys for each validator node they manage across consumer chains. Utilizing different consensus keys on different chains offers several advantages, with the primary benefit being the prevention of compromise to the validator's provider chain consensus key in the event of a breach or compromise of their consumer chain node or other associated infrastructure. The replicated security module introduces queries and transactions that facilitate the assignment of keys on consumer chains.

[More details are available here. ](https://cosmos.github.io/interchain-security/features/key-assignment)

#### Replicated Security Reward Distribution

Consumer chains have the flexibility to allocate a portion of their block rewards (inflation tokens) and fees to provider chain validators and delegators. In the context of replicated security, these block rewards and fees are periodically transmitted from the consumer chain to the provider chain through an IBC transfer channel, which is established during the initialization of the consumer chain.

Within the provider chain, the distribution module takes charge of reward allocation. Validators and delegators are entitled to receive a proportion of the consumer chain tokens as staking rewards. It is important to note that the distributed reward tokens are in the form of IBC tokens and, as such, cannot be staked on the provider chain.

The process of sending and distributing rewards from consumer chains to the provider chain is effectively managed by the Reward Distribution sub-protocol.

[More details are available here. ](https://cosmos.github.io/interchain-security/features/reward-distribution)



### **What Are the Replicated Security Provider Proposals?**&#x20;

With the Replicated Security module on the Onomy Network, 3 new proposal types are introduced for the provider chain. These are used to propose and vote on replicated security events via the DAO governance module.&#x20;

1. **ConsumerAdditionProposal**

Used to suggest adding a new consumer chain that will rent security from the Onomy Network. &#x20;

2. **ConsumerRemovalProposal**

Used to suggest removing a consumer chain renting security from the Onomy Network.

3. **EquivocationProposal**

Used to suggest slashing a validator because it has double signed on the consumer chain. Thus, if this proposal passes, the validator will also be slashed for the same amount on the provider chain.&#x20;

[More details are available here. ](https://cosmos.github.io/interchain-security/features/proposals)

### &#x20;**What Is Consumer Initiated Slashing in Replicated Security?**&#x20;

A consumer chain, at its core, functions as a typical Cosmos-SDK based chain, just like Onomy is. However, it leverages the replicated security module to establish economic security through the staked funds deposited on the provider chain, rather than relying solely on its own chain. Essentially, the provider chain and consumer chains are distinct networks with separate infrastructures that are interconnected by virtue of sharing the provider's validator set. Through this connection to the provider's validator set, a consumer chain inherits the economic security assurances provided by the provider chain, specifically in terms of the total staked value.

In order to uphold the proof of stake model, the consumer chain possesses the capability to relay evidence of infractions, such as double signing and downtime, to the provider chain. This enables the identification and penalization of validators responsible for such infractions. Any infraction occurring on any consumer chain is reflected not only on the provider chain but also across all other consumer chains.

Here are the two changes that the replicated security module is bringing:&#x20;

**Downtime Infractions**

Once consumer chains report instances of downtime infractions, the provider chain promptly takes action upon receiving the evidence of such infractions.

Rather than resorting to slashing, the provider chain adopts a different approach by incarcerating the offending validator for a period specified in the chain parameters.

To safeguard against potential harm caused by malicious consumer chains, a mechanism known as slash throttling (also referred to as jail throttling) is implemented. This mechanism ensures that only a portion of the validator set can be incarcerated at any given time, effectively mitigating the risks posed by malicious actions originating from consumer chains.

**Double-signing (equivocation)**

Immediate action is not taken in response to double-signing (equivocation) infractions.

Upon receiving evidence of double signing, the provider chain acknowledges the evidence and permits the submission of an EquivocationProposal, which aims to slash the validator responsible for the infraction. However, it's important to note that any EquivocationProposal seeking to slash a validator who has not double signed on any of the consumer chains will be automatically rejected by the provider chain.

The actual slashing and tombstoning of the offending validator will solely occur if and when an EquivocationProposal is accepted and successfully passes through the governance process.

It's worth mentioning that the slashing and tombstoning of the offending validator will be implemented across all consumer chains, ensuring consistent consequences for the equivocation infraction committed.

More information about creating EquivocationProposals is available [here](https://cosmos.github.io/interchain-security/features/proposals#equivocationproposal).&#x20;



### **Developing a Consumer Chain on the Onomy Network Using RS**

While working on the development of an ICS consumer chain, it is crucial to allocate sufficient time not only for the implementation of your chain's specific logic but also to ensure compatibility with the ICS protocol. To assist you throughout this process, the ICS team has thoughtfully provided several examples of minimum viable consumer chain applications, serving as valuable references on your development journey.

#### **Basic Consumer Chain:**

The source code for the example application can be accessed [here](https://github.com/cosmos/interchain-security/tree/main/app/consumer).

Please take note that consumer chains do not incorporate the staking module. Instead, the validator set is replicated from the provider chain. This implies that the provider and consumer chains share the same validator set, and the stake held on the provider directly determines the stake on the consumer chain. Currently, there is no opt-in mechanism available, so all validators from the provider must also validate on the provider chain.

To integrate the consumer module into your chain, import it from x/consumer and register it in the appropriate locations within your app.go file. The x/consumer module facilitates communication between your chain and the provider using the ICS protocol. This module handles all IBC communication with the provider and can be easily integrated without the need to manage or override any code within the x/consumer module.

#### **Democracy Consumer Chain:**

The source code for the example application can be accessed [here](https://github.com/cosmos/interchain-security/tree/main/app/consumer).

This particular type of consumer chain encapsulates the basic CosmosSDK modules, such as x/distribution, x/staking, and x/governance, enabling the consumer chain to engage in democratic actions within its own governance system.

By leveraging these modules alongside the x/consumer module, the consumer chain can partake in various democratic activities. For instance, it can actively participate in voting and decision-making processes within its governance system.

Furthermore, these modules empower the consumer chain to mint its own governance tokens. These tokens can be delegated to key community members referred to as "representatives" (as opposed to "validators" in standalone chains). These tokens may serve purposes beyond governance voting, potentially unlocking additional functionalities.

Standalone Chain to Consumer Chain Changeover:

This feature is currently undergoing active development, and further information regarding this changeover will be provided at a later time.

### &#x20;**How Consumer Chain Governance Works on Onomy**

**Consumer Chain Governance:**

Governance approaches may vary across different consumer chains. However, regardless of the specific governance method employed, there are certain settings pertaining to consensus that consumer chain governance cannot alter. In the following "Whitelist" section, we will delve into the details of these unchangeable settings.

**Democracy Module:**

The democracy module offers a governance experience akin to that of a standalone Cosmos chain, with one crucial distinction. On a standalone Cosmos chain, validators can act as representatives for their delegators by voting with their stake, but only if the delegator themselves abstains from voting. This introduces a lightweight form of liquid democracy.

When utilizing the democracy module on a consumer chain, the experience remains nearly identical, except for the fact that the actual validator set of the chain (which consists of the Cosmos Hub validators) does not serve as representatives. Instead, a separate representative role exists, allowing token holders to delegate their voting power to these representatives. These representatives can fulfill the functions that validators typically perform in Cosmos governance, all without participating in the proof-of-stake consensus.

For an illustrative example, please refer to the [Democracy Consumer.](https://github.com/cosmos/interchain-security/tree/main/app/consumer-democracy)

**CosmWasm:**

There are several exceptional DAO and governance frameworks implemented as CosmWasm contracts. These frameworks can be utilized as the primary governance system for a consumer chain. Actions triggered by the governance contracts written in CosmWasm have the ability to influence parameters and initiate actions within the consumer chain.

**The Whitelist:**

Not all aspects of a consumer chain can be modified by the consumer chain's governance. Certain settings related to consensus and other essential factors can only be altered by the provider chain. To address this, consumer chains incorporate a whitelist containing parameters that the consumer chain's governance is allowed to modify.

### &#x20;**Onboarding Checklist onto Consumer Chains**

The following checklists serve as valuable resources for onboarding a new consumer chain to replicated security.

For a comprehensive guide on preparing and launching consumer chains, we recommend referring to the testnet repository.

1. **Complete Testing & Integration**

* Test integration with the ONET.
* Verify compatibility with supported relayer versions
* In case of any issues, reach out to the Onomy DAO contributors

2. **Create an Onboarding Repository**

To facilitate the onboarding process for validators and other node runners, it is essential to prepare a repository that provides comprehensive information on how to run your chain.

This repository should include, at a minimum:

* genesis.json without CCV data (before the proposal passes).
* genesis.json with CCV data (after the spawn time passes).
* relevant information about the seed/peer nodes you are operating.
* details on the compatible versions of the relayer.
* copy of your governance proposal in JSON format.
* optional: A script demonstrating how to start your chain and connect to peers.
* gather feedback from other developers, validators, and the community regarding your onboarding repository and make necessary improvements.

3. **Submit a Governance Proposal**

Before submitting a ConsumerChainAddition proposal, it is advisable to allow at least a day between the proposal's passing and the chain's spawn time. This allows validators, node operators, and the community to adequately prepare for the chain's launch. If feasible, set the spawn time to accommodate participants from different time zones in case of emergencies. Ideally, consider setting the spawn time between 12:00 UTC and 20:00 UTC, ensuring most validator operators are available and prepared to address any potential issues.

Additionally, reach out to the community through forums to formalize your intention to become an ICS consumer. This helps gather community support, accept feedback from validators, developers, and the wider community.

When submitting the proposal, consider the following:

* Determine your chain's spawn time.
* Identify the consumer chain parameters to include in the proposal.
* Ensure the proposal includes a link to your onboarding repository.
* Clearly describe the purpose and benefits of running your chain.

4. **Launch**

The consumer chain commences once a minimum of 66.67% of the provider's total voting power comes online. To achieve interchain security, it is crucial to establish the appropriate CCV channels and ensure the propagation of the first validator set update from the provider to the consumer.

To facilitate a successful launch, consider the following:

* Provide a repository containing comprehensive onboarding instructions specifically designed for validators (ensure that this repository is already listed in the proposal).
* Prepare the genesis.json file with populated CCV data, including the initial validator set.
* Provide relevant maintenance and emergency contact information, such as Discord, Telegram or other communication channels.
* Establish a block explorer to monitor chain activity and ensure the health of the network.

### Offboarding an Onomy Consumer Chain

To initiate the offboarding process for a consumer chain, simply submit a ConsumerRemovalProposal governance proposal that specifies a stop\_time. Once the stop time indicated in the proposal has elapsed, the provider chain will proceed to remove the consumer chain from the ICS protocol. As a result, the provider chain will cease sending validator set updates to the consumer chain.

#### Validators Guide

You are advised to join the[ Replicated Security testnet](https://github.com/cosmos/testnets/tree/master/replicated-security) to gain hands-on experience with running consumer chains.

At present, replicated security requires all validators of the provider chain (ie. Onomy or the Cosmos Hub) to run validator nodes for all governance-approved consumer chains.

Once a ConsumerAdditionProposal passes, validators need to prepare to run the consumer chain binaries (these will be linked in their proposals) and set up validator nodes on governance-approved consumer chains.

Provider chain and consumer chains represent standalone chains that only share the validator set ie. the same validator operators are tasked with running all chains.

To validate a consumer chain and be eligible for rewards, validators are required to be in the active set of the provider chain (20 in the case of Onomy).&#x20;

### Startup sequence overview

Consumer chains cannot start and be secured by the validator set of the provider unless a ConsumerAdditionProposal is passed. Each proposal defines a spawn\_time - the timestamp when the consumer chain genesis is finalized and the consumer chain clients get initialized on the provider.

Validators are required to run consumer chain binaries only after spawn\_time has passed.

Please note that any additional instructions pertaining to specific consumer chain launches will be available before spawn time. The chain start will be stewarded by the Cosmos Hub team and the teams developing their respective consumer chains.

#### 1. Consumer Chain init + 2. Genesis generation

Consumer chain team initializes the chain genesis.json and prepares binaries which will be listed in the ConsumerAdditionProposal

#### 3. Submit Proposal

Consumer chain team (or their advocates) submits a ConsumerAdditionProposal. The most important parameters for validators are:

* spawn\_time - the time after which the consumer chain must be started
* genesis\_hash - hash of the pre-ccv genesis.json; the file does not contain any validator info -> the information is available only after the proposal is passed and spawn\_time is reached
* binary\_hash - hash of the consumer chain binary used to validate the software builds

#### 4. CCV Genesis state generation

After reaching spawn\_time the provider chain will automatically create the CCV validator states that will be used to populate the corresponding fields in the consumer chain genesis.json. The CCV validator set consists of the validator set on the provider at spawn\_time.

#### 5. Updating the genesis file

Upon reaching the spawn\_time the initial validator set state will become available on the provider chain. The initial validator set is included in the final genesis.json of the consumer chain.

#### 6. Chain start

The consumer chain will start producing blocks as soon as 66.67% of the provider chain's voting power comes online (on the consumer chain). The relayer should be started after block production commences.

The new genesis.json containing the initial validator set will be distributed to validators by the consumer chain team (launch coordinator). Each validator should use the provided genesis.json to start their consumer chain node.

Please pay attention to any onboarding repositories provided by the consumer chain teams. Recommendations are available in[ Consumer Onboarding Checklist](https://cosmos.github.io/interchain-security/consumer-development/onboarding). Another comprehensive guide is available in the[ Replicated Security testnet repo](https://github.com/cosmos/testnets/blob/master/replicated-security/CONSUMER\_LAUNCH\_GUIDE.md).

#### 7. Creating IBC connections

Finally, to fully establish replicated security an IBC relayer is used to establish connections and create the required channels.

The relayer can establish the connection only after the consumer chain starts producing blocks.

hermes create connection --a-chain \<consumer chain ID> --a-client 07-tendermint-0 --b-client \<client assigned by provider chain>

hermes create channel --a-chain \<consumer chain ID> --a-port consumer --b-port provider --order ordered --a-connection connection-0 --channel-version 1

hermes start

### Downtime Infractions

At present, the consumer chain can report evidence about downtime infractions to the provider chain. The min\_signed\_per\_window and signed\_blocks\_window can be different on each consumer chain and are subject to changes via consumer chain governance.

Causing a downtime infraction on any consumer chain will not incur a slash penalty. Instead, the offending validator will be jailed on the provider chain and consequently on all consumer chains.

To unjail, the validator must wait for the jailing period to elapse on the provider chain and[ submit an unjail transaction](https://hub.cosmos.network/main/validators/validator-setup.html#unjail-validator) on the provider chain. After unjailing on the provider, the validator will be unjailed on all consumer chains.

More information is available in[ Downtime Slashing documentation](https://cosmos.github.io/interchain-security/features/slashing#downtime-infractions)

### Double-signing Infractions

To learn more about equivocation handling in replicated security check out the[ Slashing](https://cosmos.github.io/interchain-security/features/slashing#double-signing-equivocation) and[ EquivocationProposal](https://cosmos.github.io/interchain-security/features/proposals#equivocationproposal) documentation sections

### Key assignment

Validators can use different consensus keys on the provider and each of the consumer chains. The consumer chain consensus key must be registered on the provider before use.

For more information check our the[ Key assignment overview and guide](https://cosmos.github.io/interchain-security/features/key-assignment)

### References:

* [Cosmos Hub Validators FAQ](https://hub.cosmos.network/main/validators/validator-faq.html)
* [Cosmos Hub Running a validator](https://hub.cosmos.network/main/validators/validator-setup.html)
* [Startup Sequence](https://github.com/cosmos/testnets/blob/master/replicated-security/CONSUMER\_LAUNCH\_GUIDE.md#chain-launch)
* [Submit Unjailing Transaction](https://hub.cosmos.network/main/validators/validator-setup.html#unjail-validator)
* [Joining the Replicated Security testnet on the Cosmos Hub](https://cosmos.github.io/interchain-security/validators/joining-testnet)
* [Withdrawing consumer chain validator rewards](https://cosmos.github.io/interchain-security/validators/withdraw\_rewards)
