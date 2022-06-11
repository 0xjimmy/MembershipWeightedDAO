# MembershipWeightedDAO
> DAO framework based on a NFT membership with weighed consensus based on participation.

This is a DAO framework with governance based off membership per participant where each participant's voting power is based of their participation in the DAO.

**Design Goals** 
- Reward participation and stake in a organisation
- Assume those with highest participation have the best long term interest in the # organization and knowledge to make it happen
- Allow all participants to start on an equal playing-field and the same ability to gain voting power through positive involvement
- Increase the monetary and coordination cost for one group to take majority control of the DAO

## Calculating Weighting Vote Power
Whilst holding a membership token vote power will start at a base level and grow to a set max ratio of the total power, and then reset on token transfer.

**Global Values**
- ``basePower`` - Starting vote power
- ``maxVotePercent`` - Maximum share of total power 1 token can have
- ``hodlCeiling`` - Maximum value of time held

- ``hodlMultiplier``, ``voteMultiplier``, ``proposalMultiplier`` - Multiplier values for each power input type

- ``totalHodlTime`` - Sum of all token hold times, or *hodlCeiling* if above
- ``totalProposalPoints`` - Sum of all proposals made by all current members
- ``totalVotePoints`` - Sum of all votes made by all current members

**Account Specific Values**
- ``hodlTime`` - Time token held
- ``hodlStart`` - Time of token's last transfer
- ``proposalPoints`` - Sum of vote weights that participates in proposals submitted by the token holder
- ``votePoints`` - Sum of the vote power in votes cast by the token holder

**Formula For Power**

### Hodl Weight
Time held in seconds or maximum counted time (whichever is smaller) multiplied by the hold time multiplier.

```ts
const hodlCeiling = 7 776 000 // 90 Days
const hodlMultiplier = 1

let userHodlPower = hodlMultiplier * min(userHodlTime, hodlCeiling)
```

### Proposal Weight
Proposal weight is based of amount of the total vote power received in proposed votes - not including the proposers vote.

```ts
const proposalMultiplier = 1

let proposalPower, proposalVoters // Not including user in counts

let userProposalPower = proposalMultiplier * ((sumOfNetPositiveVoteWeight - usersVote) / userProposalCount)
```

### Vote Weight
Vote weight is based of amount of power used voting and amount of votes cast

```ts
const voteMultiplier = 1000

let userVoteCount

let userVotePower = voteMultiplier * userVoteCount
```

### Total User Vote Weight
```ts
const baseWeight = 100 000
let globalWeight, maxVotePercent

let userHodlPower, userProposalPower, userVotePower
let sum = userHodlPower + userProposalPower + userVotePower


let userWeight = min(globalWeight * maxVotePercent, max(baseWeight, sum))
```


## Contract Structure
The governance contract inherits both [ERC721](https://eips.ethereum.org/EIPS/eip-721) and OpenZeppelin's [IGovernor](https://docs.openzeppelin.com/contracts/4.x/api/governance#IGovernor) interfaces.
