

### Project:

This contract allows the creator to invite a select group of people to vote on something and provides an eth reward to the `for` voters if the proposal passes, otherwise refunds the reward to the creator.

- Roles:

* `creator` - Deployer of the protocol, they are a trusted used who will receive the funds if a vote fails.
* `AllowedVoters` - A list of addresses that are allowed to vote on proposals.


### Questions:

- How do we invite select group of People? `inside the constructor by creator`
- Do we have duplicate check where creator invites select group of people? `findings related to it it's not valid finding`
- Who are the `for` voters? `people that agree on the proposal`
- How a Proposal Passes?  `if (totalVotesAgainst < totalVotesFor)`
- How Proposal Looks like? `none`
- How much is the ETH reward? `1 ether or higher`


### Attack Vectors

- Can Someone That is Not Part of the Selected Group, Vote on Something? `no`
- Can we somehow add ourselves to that selected group of people? `no`
- Can we become part of the `for` voters somehow? `no`
- Can we somehow make `for` voters, unable to vote? `no`
- Can we do Something so Nothing Gets sent out as reward to `for` voters? maybe DOS? `no`
- Can we Steal `for` Voters ETH reward? `no`
- Can we Change the ETH reward considered for `for` voters? `no`


### Notes:

- the contract has atleast `1 ETH` or higher than that balance.
- voters are atleast `3` and at most `9`.
