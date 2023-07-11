# Replicated Security FAQs

### What is the meaning of Validator Set Replication? <a href="#what-is-the-meaning-of-validator-set-replication" id="what-is-the-meaning-of-validator-set-replication"></a>

VSR simply means that the same validator set is used to secure both the provider and consumer chains. VSR is ensured through Replicated Security protocol which keeps consumers up to date with the validator set of the provider.

### What is a consumer chain? <a href="#what-even-is-a-consumer-chain" id="what-even-is-a-consumer-chain"></a>

A consumer chain is blockchain operated by the same validator operators as the provider chain. The RS protocol ensures the validator set replication properties (informs consumer chain about the current state of the validator set on the provider)

Consumer chains are run on infrastructure (virtual or physical machines) distinct from the provider, have their own configurations and operating requirements.

### What happens to the consumer if the provider chain is down? <a href="#what-happens-to-consumer-if-provider-is-down" id="what-happens-to-consumer-if-provider-is-down"></a>

In case the provider chain halts or experiences difficulties the consumer chain will keep operating - the provider chain and consumer chains represent different networks, which only share the validator set.

The consumer chain will not halt if the provider halts because they represent distinct networks and distinct infrastructures. Provider chain liveness does not impact consumer chain liveness.

However, if the `trusting_period` (currently 5 days for protocol safety reasons) elapses without receiving any updates from the provider, the consumer chain will essentially transition to a Proof of Authority chain. This means that the validator set on the consumer will be the last validator set of the provider that the consumer knows about.

Steps to recover from this scenario and steps to "release" the validators from their duties will be specified at a later point. At the very least, the consumer chain could replace the validator set, remove the ICS module and perform a genesis restart.&#x20;

### What happens to the provider if the consumer chain is down? <a href="#what-happens-to-provider-if-consumer-is-down" id="what-happens-to-provider-if-consumer-is-down"></a>

Consumer chains do not impact the provider chain. The RS protocol is concerned only with validator set replication and the only communication that the provider requires from the consumer is information about validator activity (essentially keeping the provider informed about slash events).

### Can I run the provider and consumer chains on the same machine? <a href="#can-i-run-the-provider-and-consumer-chains-on-the-same-machine" id="can-i-run-the-provider-and-consumer-chains-on-the-same-machine"></a>

Yes, but you should favor running them in separate environments so failure of one machine does not impact your whole operation.

### Can the consumer chain have its own token? <a href="#can-the-consumer-chain-have-its-own-token" id="can-the-consumer-chain-have-its-own-token"></a>

As any other cosmos-sdk chain the consumer chain can issue its own token, manage inflation parameters and use them to pay gas fees.

### How are Tx fees paid on consumer? <a href="#how-are-tx-fees-paid-on-consumer" id="how-are-tx-fees-paid-on-consumer"></a>

The consumer chain operates as any other cosmos-sdk chain. The ICS protocol does not impact the normal chain operations.

### Are there any restrictions the consumer chains need to abide by? <a href="#are-there-any-restrictions-the-consumer-chains-need-to-abide-by" id="are-there-any-restrictions-the-consumer-chains-need-to-abide-by"></a>

No. Consumer chains are free to choose how they wish to operate, which modules to include, use CosmWASM in a permissioned or a permissionless way. The only thing that separates consumer chains from standalone chains is that they share their validator set with the provider chain.

### What's in it for the validators and stakers? <a href="#whats-in-it-for-the-validators-and-stakers" id="whats-in-it-for-the-validators-and-stakers"></a>

The consumer chains sends a portion of its fees and inflation as reward to the provider chain as defined by `consumer_redistribution_fraction`. The rewards are distributed (sent to the provider) every `blocks_per_distribution_transmission`.

`consumer_redistribution_fraction` and `blocks_per_distribution_transmission` are parameters defined in the `ConsumerAdditionProposal` used to create the consumer chain. These parameters can be changed via consumer chain governance.

### Can the consumer chain have its own governance? <a href="#can-the-consumer-chain-have-its-own-governance" id="can-the-consumer-chain-have-its-own-governance"></a>

**Yes.**

In that case the validators are not necessarily part of the governance structure. Instead, their place in governance is replaced by "representatives" (governors). The representatives do not need to run validators, they simply represent the interests of a particular interest group on the consumer chain.

Validators can also be representatives but representatives are not required to run validator nodes.

This feature discerns between validator operators (infrastructure) and governance representatives which further democratizes the ecosystem. This also reduces the pressure on validators to be involved in on-chain governance.

### Can validators opt-out of replicated security? <a href="#can-validators-opt-out-of-replicated-security" id="can-validators-opt-out-of-replicated-security"></a>

At present, the validators cannot opt-out of validating consumer chains.

There are multiple opt-out mechanisms under research.

### How does Equivocation Governance Slashing work? <a href="#how-does-equivocation-governance-slashing-work" id="how-does-equivocation-governance-slashing-work"></a>

To avoid potential attacks directed at provider chain validators, a new mechanism was introduced:

When a validator double-signs on the consumer chain, a special type of slash packet is relayed to the provider chain. The provider will store information about the double signing validator and allow a governance proposal to be submitted. If the double-signing proposal passes, the offending validator will be slashed on the provider chain and tombstoned. Tombstoning will permanently exclude the validator from the active set of the provider.

An equivocation proposal cannot be submitted for a validator that did not double sign on any of the consumer chains.

### Can Consumer Chains perform Software Upgrades? <a href="#can-consumer-chains-perform-software-upgrades" id="can-consumer-chains-perform-software-upgrades"></a>

Consumer chains are standalone chains, in the sense that they can run arbitrary logic and use any modules they want (ie CosmWASM).

Consumer chain upgrades are unlikely to impact the provider chain, as long as there are no changes to the RS module.

### How do I start using ICS? <a href="#how-do-i-start-using-ics" id="how-do-i-start-using-ics"></a>

To become a consumer chain, check the technicals references in this documentation and the Cosmos SDK documentation @ Replicated Security / Interchain Security, and reach out to DAO contributors or validators should you have questions and require support.&#x20;

### Which relayers are supported? <a href="#which-relayers-are-supported" id="which-relayers-are-supported"></a>

Currently supported versions:

* Hermes 1.4.1
