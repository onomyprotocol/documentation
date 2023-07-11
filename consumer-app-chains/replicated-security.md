# Replicated Security

Replicated security, also known as interchain security, is a publicly available IBC application that facilitates the leasing of proof-of-stake security between Cosmos blockchains.

By leveraging replicated security, developers are empowered to initiate a "consumer" blockchain that utilizes the identical validator set as the "provider" blockchain through the creation of a governance proposal. Once the proposal gains acceptance, validators from the provider chain commence validation for the consumer chain. Consequently, consumer chains are endowed with the complete security and decentralization capabilities of the provider.&#x20;

This makes building sovereign app-chains in Cosmos significantly easier, whilst also enabling seamless IBC interoperability between these chains.&#x20;

## **The benefits of Replicated Security**&#x20;

* Full provider security. At launch, consumer chains are secured by the full validator set and market cap of the provider chain.
* Independent block-space. Transactions on consumer chains do not compete with any other applications. This means that there will be no unexpected congestion, and performance will generally be much better than on a shared smart contract platform such as Ethereum.
* Projects keep the majority of gas fees. Depending on configuration, these fees either go to the project’s community DAO, or can be used in the protocol in other ways.
* No validator search. Consumer chains do not have their own validator sets, and so do not need to find validators one by one. A governance vote will take place for a chain to get adopted by the provider validators which will encourage participation and signal strong buy-in into the project’s long-term success.

Instant sovereignty. Consumers can run arbitrary app logic similar to standalone chains. At any time in the future, a consumer chain can elect to become a completely standalone chain, with its own validator set.
