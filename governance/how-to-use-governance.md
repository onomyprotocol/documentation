---
description: >-
  This page describes the necessary steps to be taken by those who wish to
  submit and vote on proposals in the Onomy DAO.
---

# How to Use Governance

The Onomy Protocol DAO was formed on mainnet launch, in November 2022. The following steps can be taken to query and interact with the Onomy Governance module using CLI.&#x20;

You will first have to [download the Onomy binaries available here](https://github.com/onomyprotocol/onomy/blob/main/docs/chain/installation.md). Follow the instructions to get them set up so you are able to access command line.&#x20;

## CLI <a href="#cli" id="cli"></a>

A user can query and interact with the `gov` module using the CLI.

## **Query** <a href="#query" id="query"></a>

The `query` commands allow users to query `gov` state.

&#x20;`onomyd query gov --help`

### **deposit**

The `deposit` command allows users to query a deposit for a given proposal from a given depositor.

`onomyd query gov deposit [proposal-id] [depositer-addr] [flags]`

Example:

`onomyd query gov deposit 1 onomy1..`

Example Output:&#x20;

`amount:`

`-amount: "100" denom: stake depositor: onomy1.. proposal_id: "1"`

### **Deposits**

The `deposits` command allows users to query all deposits for a given proposal.

`onomyd query gov deposits [proposal-id] [flags]`

Example:&#x20;

`onomyd query gov deposits 1`

Example Output:

`deposits:`

`-amount:`

`-amount: "100" denom: stake depositor: onomy1.. proposal_id: "1" pagination: next_key: null total: "0"`

### **Param**

The `param` command allows users to query a given parameter for the `gov` module.

`onomyd query gov param [param-type] [flags]`

Example:&#x20;

`onomyd query gov param voting`

Example Output:

`voting_period: "172800000000000"`

### **Params**

The `params` command allows users to query all parameters for the `gov` module.

`onomyd query gov params [flags]`

Example:&#x20;

`onomyd query gov params`

Example Output:&#x20;

<figure><img src="../.gitbook/assets/Screenshot 2022-12-21 at 14.57.13.png" alt=""><figcaption></figcaption></figure>

### **Proposal**

The `proposal` command allows users to query a given proposal.

`onomyd query gov proposal [proposal-id] [flags]`

Example:&#x20;

`onomyd query gov proposal 1`

Example Output:&#x20;

<figure><img src="../.gitbook/assets/Screenshot 2022-12-21 at 14.58.39.png" alt=""><figcaption></figcaption></figure>

### **Proposals**

The `proposals` command allows users to query all proposals with optional filters.

`onomyd query gov proposals [flags]`

Example:&#x20;

`onomyd query gov proposals`

Example Output:&#x20;

<figure><img src="../.gitbook/assets/Screenshot 2022-12-21 at 15.00.04.png" alt=""><figcaption></figcaption></figure>

### **Proposer**

The `proposer` command allows users to query the proposer for a given proposal.

`onomyd query gov proposer [proposal-id] [flags]`

Example:&#x20;

`onomyd query gov proposer 1`

Example Output:&#x20;

`proposal_id: "1" proposer: onomy1r0tllwu5c9dtgwg3wr28lpvf76hg85f5zmh9l2`

### **Tally**

The `tally` command allows users to query the tally of a given proposal vote.

`onomyd query gov tally [proposal-id] [flags]`

Example:&#x20;

`onomyd query gov tally 1`

Example Output:

`abstain: "0"`&#x20;

`"no": "0"`&#x20;

`no_with_veto: "0"`&#x20;

`"yes": "1"`

### **Vote**

The `vote` command allows users to query a vote for a given proposal.

`onomyd query gov vote [proposal-id] [voter-addr] [flags]`

Example:&#x20;

`onomyd query gov vote 1 onomy1..`

Example Output:&#x20;

`option: VOTE_OPTION_YES`&#x20;

`options:`

&#x20;`-option: VOTE_OPTION_YES`&#x20;

&#x20; `weight: "1.000000000000000000" proposal_id: "1"`&#x20;

`voter: onomy1..`

### **Votes**

The `votes` command allows users to query all votes for a given proposal.

`onomyd query gov votes [proposal-id] [flags]`

Example:&#x20;

`onomyd query gov votes 1`

Example Output:&#x20;

<figure><img src="../.gitbook/assets/Screenshot 2022-12-21 at 15.13.15.png" alt=""><figcaption></figcaption></figure>

### **Transactions**

The `tx` commands allow users to interact with the `gov` module.

`onomyd tx gov --help`

#### **Deposit**

The `deposit` command allows users to deposit tokens for a given proposal.

`onomyd tx gov deposit [proposal-id] [deposit] [flags]`

Example:

`onomyd tx gov deposit 1 10000000stake --from onomy1..`

#### **submit-proposal**

The `submit-proposal` command allows users to submit a governance proposal and to optionally include an initial deposit.

`onomyd tx gov submit-proposal [command] [flags]`

Example:

`onomyd tx gov submit-proposal --title="Test Proposal" --description="testing, testing, 1, 2, 3" --type="Text" --deposit="10000000stake" --from onomy1..`

Example (`cancel-software-upgrade`):

`onomyd tx gov submit-proposal cancel-software-upgrade --title="Test Proposal" --description="testing, testing, 1, 2, 3" --deposit="10000000stake" --from onomy1..`

Example (`param-change`):

`onomyd tx gov submit-proposal param-change proposal.json --from onomy1..`

Example (`software-upgrade`):

`onomyd tx gov submit-proposal software-upgrade v2 --title="Test Proposal" --description="testing, testing, 1, 2, 3" --upgrade-height 1000000 --from onomy1..`

**Vote**

The `vote` command allows users to submit a vote for a given governance proposal.

`onomyd tx gov vote [command] [flags]`

Example:&#x20;

`onomyd tx gov vote 1 yes --from onomy1..`

**Weighted-vote**

The `weighted-vote` command allows users to submit a weighted vote for a given governance proposal.

`onomyd tx gov weighted-vote [proposal-id] [weighted-options]`

Example:&#x20;

`onomyd tx gov weighted-vote 1 yes=0.5,no=0.5 --from onomy1`

### **gRPC**

A user can query the `gov` module using gRPC endpoints.

**Proposal**

The `Proposal` endpoint allows users to query a given proposal.

`onomy.gov.v1beta1.Query/Proposal`

Example:&#x20;

`grpcurl -plaintext`\
`-d '{"proposal_id":"1"}'`\
`localhost:9090`\
`onomy.gov.v1beta1.Query/Proposal`

Example Output:

<figure><img src="../.gitbook/assets/Screenshot 2022-12-21 at 15.53.12.png" alt=""><figcaption></figcaption></figure>

**Proposals**

The `Proposals` endpoint allows users to query all proposals with optional filters.

`onomy.gov.v1beta1.Query/Proposals`

Example:&#x20;

`grpcurl -plaintext`\
`localhost:9090`\
`onomy.gov.v1beta1.Query/Proposals`



### **Vote**

The `Vote` endpoint allows users to query a vote for a given proposal.

`onomy.gov.v1beta1.Query/Vote`

Example:&#x20;

`grpcurl -plaintext`\
`-d '{"proposal_id":"1","voter":"onomy1.."}'`\
`localhost:9090`\
`onomy.gov.v1beta1.Query/Vote`

Example Output:&#x20;

`{ "vote": {`&#x20;

&#x20; `"proposalId": "1",`&#x20;

&#x20; `"voter": "onomy1..",`&#x20;

&#x20; `"option": "VOTE_OPTION_YES",`&#x20;

&#x20; `"options": [`&#x20;

&#x20;     `{`&#x20;

&#x20;         `"option": "VOTE_OPTION_YES",`&#x20;

&#x20;         `"weight": "1000000000000000000"`&#x20;

&#x20;    `}`&#x20;

&#x20;  `]`&#x20;

&#x20; `}`&#x20;

`}`

**Votes**

The `Votes` endpoint allows users to query all votes for a given proposal.

`onomy.gov.v1beta1.Query/Votes`

Example:&#x20;

`grpcurl -plaintext`\
`-d '{"proposal_id":"1"}'`\
`localhost:9090`\
`onomy.gov.v1beta1.Query/Votes`

Example Output:

<figure><img src="../.gitbook/assets/Screenshot 2022-12-21 at 15.58.11.png" alt=""><figcaption></figcaption></figure>

**Params**

The `Params` endpoint allows users to query all parameters for the `gov` module.

`onomy.gov.v1beta1.Query/Params`

Example:&#x20;

`grpcurl -plaintext`\
`-d '{"params_type":"voting"}'`\
`localhost:9090`\
`onomy.gov.v1beta1.Query/Params`

Example Output:&#x20;

`{`

&#x20;`"votingParams": {`

&#x20; `"votingPeriod": "172800s"`

`},`

`"depositParams": { "maxDepositPeriod": "0s"`&#x20;

`},`&#x20;

`"tallyParams": {`

`"quorum": "MA==",`

`"threshold": "MA==", "vetoThreshold": "MA==" } }`

**Deposit**

The `Deposit` endpoint allows users to query a deposit for a given proposal from a given depositor.

`onomy.gov.v1beta1.Query/Deposit`

Example:&#x20;

`grpcurl -plaintext`\
`'{"proposal_id":"1","depositor":"onomy1.."}'`\
`localhost:9090`\
`onomy.gov.v1beta1.Query/Deposit`

Example Output:

`{ "deposit": {`&#x20;

&#x20; `"proposalId": "1",`&#x20;

&#x20; `"depositor": "onomy1..",`

&#x20;`"amount": [`&#x20;

`{`

&#x20;`"denom": "stake",`&#x20;

`"amount": "10000000" } ] } }`

**Deposits**

The `Deposits` endpoint allows users to query all deposits for a given proposal.

`onomy.gov.v1beta1.Query/Deposits`

Example:&#x20;

`grpcurl -plaintext`\
`-d '{"proposal_id":"1"}'`\
`localhost:9090`\
`onomy.gov.v1beta1.Query/Deposits`

Example Output:&#x20;

`{ "deposits": [ { "proposalId": "1", "depositor": "onomy1..", "amount": [ { "denom": "stake", "amount": "10000000" } ] } ], "pagination": { "total": "1" } }`

**TallyResult**

The `TallyResult` endpoint allows users to query the tally of a given proposal.

`onomy.gov.v1beta1.Query/TallyResult`

Example:&#x20;

`grpcurl -plaintext`\
`-d '{"proposal_id":"1"}'`\
`localhost:9090`\
`onomy.gov.v1beta1.Query/TallyResult`

Example Output:&#x20;

`{`&#x20;

&#x20; `"tally": {`&#x20;

&#x20;  `"yes": "1000000",`&#x20;

&#x20;  `"abstain": "0",`&#x20;

&#x20;  `"no": "0",`&#x20;

&#x20;  `"noWithVeto": "0"`&#x20;

&#x20;  `}`&#x20;

&#x20; `}`

### **REST**

A user can query the `gov` module using REST endpoints.

### **Proposal**

The `proposals` endpoint allows users to query a given proposal.

`/onomy/gov/v1beta1/proposals/{proposal_id}`

Example:&#x20;

`curl localhost:1317/onomy/gov/v1beta1/proposals/1`

Example Output:&#x20;

`{ "proposal": { "proposal_id": "1", "content": { "@type": "/onomy.gov.v1beta1.TextProposal", "title": "Test Proposal", "description": "testing, testing, 1, 2, 3" }, "status": "PROPOSAL_STATUS_VOTING_PERIOD", "final_tally_result": { "yes": "0", "abstain": "0", "no": "0", "no_with_veto": "0" }, "submit_time": "2021-09-16T19:40:08.712440474Z", "deposit_end_time": "2021-09-18T19:40:08.712440474Z", "total_deposit": [ { "denom": "stake", "amount": "10000000" } ], "voting_start_time": "2021-09-16T19:40:08.712440474Z", "voting_end_time": "2021-09-18T19:40:08.712440474Z" } }`

### **Proposals**

The `proposals` endpoint also allows users to query all proposals with optional filters.

`/onomy/gov/v1beta1/proposals`

Example:&#x20;

`curl localhost:1317/onomy/gov/v1beta1/proposals`



### **Voter Vote**

The `votes` endpoint allows users to query a vote for a given proposal.

`/onomy/gov/v1beta1/proposals/{proposal_id}/votes/{voter}`

Example:&#x20;

`curl localhost:1317/onomy/gov/v1beta1/proposals/1/votes/onomy1..`

Example Output:&#x20;

`{ "vote": { "proposal_id": "1", "voter": "onomy1..", "option": "VOTE_OPTION_YES", "options": [ { "option": "VOTE_OPTION_YES", "weight": "1.000000000000000000" } ] } }`

**Votes**

The `votes` endpoint allows users to query all votes for a given proposal.

`/onomy/gov/v1beta1/proposals/{proposal_id}/votes`

Example:&#x20;

`curl localhost:1317/onomy/gov/v1beta1/proposals/1/votes`

Example Output:&#x20;

`{ "votes": [ { "proposal_id": "1", "voter": "onomy1..", "option": "VOTE_OPTION_YES", "options": [ { "option": "VOTE_OPTION_YES", "weight": "1.000000000000000000" } ] } ], "pagination": { "next_key": null, "total": "1" } }`

**Params**

The `params` endpoint allows users to query all parameters for the `gov` module.

`/onmoy/gov/v1beta1/params/{params_type}`

Example:&#x20;

`curl localhost:1317/onomy/gov/v1beta1/params/voting`

Example Output:&#x20;

`{ "voting_params": { "voting_period": "172800s" }, "deposit_params": { "min_deposit": [ ], "max_deposit_period": "0s" }, "tally_params": { "quorum": "0.000000000000000000", "threshold": "0.000000000000000000", "veto_threshold": "0.000000000000000000" } }`

**Deposits**

The `deposits` endpoint allows users to query a deposit for a given proposal from a given depositor.

`/onomy/gov/v1beta1/proposals/{proposal_id}/deposits/{depositor}`

Example:&#x20;

`curl localhost:1317/onomy/gov/v1beta1/proposals/1/deposits/onomy1..`

Example Output:

`{ "deposit": { "proposal_id": "1", "depositor": "onomy1..", "amount": [ { "denom": "stake", "amount": "10000000" } ] } }`

**Proposal Deposits**

The `deposits` endpoint allows users to query all deposits for a given proposal.

`/onomy/gov/v1beta1/proposals/{proposal_id}/deposits`

Example:&#x20;

`curl localhost:1317/onomy/gov/v1beta1/proposals/1/deposits`

Example Output

`{ "deposits": [ { "proposal_id": "1", "depositor": "onomy1..", "amount": [ { "denom": "stake", "amount": "10000000" } ] } ], "pagination": { "next_key": null, "total": "1" } }`

**Tally**

The `tally` endpoint allows users to query the tally of a given proposal.

`/onomy/gov/v1beta1/proposals/{proposal_id}/tally`

Example:&#x20;

`curl localhost:1317/onomy/gov/v1beta1/proposals/1/tally`

Example Output:&#x20;

`{ "tally": { "yes": "1000000", "abstain": "0", "no": "0", "no_with_veto": "0" } }`









