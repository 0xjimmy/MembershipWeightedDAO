# MembershipWeighedDAO
> DAO framework based on a NFT membership with weighed consensus based on participation.

This is a DAO framework with governance based off membership per participant where each participant's voting power is based of their participation in the DAO.

**Design Goals** 
- Reward participation and stake in a organisation
- Assume those with highest participation have the best long term interest in the # organization and knowledge to make it happen
- Allow all participants to start on an equal playingfield and the same ability to gain voting power through positive involement
- Increase the monetary and cordination cost for one group to take majoriy control of the DAO

## Calculating Weighting Vote Power
Whilst holding a membership token vote power will start at a base level and grow to a set max ratio of the total power, and then reset on token transfer.

**Global Values**
- ``basePower`` - Starting vote power
- ``maxVotePercent`` - Maximum share of total power 1 token can have
- ``hodlCeiling`` - Maximum value of time held

- ``hodlMultiplier``, ``voteMultiplier``, ``proposalMultiplier`` - Multiplier values for each power input type

- ``totalHodlTime`` - Sum of all token hold times, or *hodlCeiling* if above
- ``totalProposals`` - Sum of all proposals made by all current members
- ``totalVotes`` - Sum of all votes made by all current members

**Account Specific Values**
- ``hodlTime`` - Time token held
- ``hodlStart`` - Time of token's last transfer
- ``proposalsMade`` - Amount of proposals made with token
- ``votesCast`` - Amount of votes made with token

**Formula For Power**

```ts
const basePower = 100 000
const hodlCeiling = 7 776 000 // 90 Days
const hodlMultiplier = 1
const voteMultiplier = 1000
const proposalMultiplier = 5000

// @TODO

```

## Contract Structure
The governance contract inherits both [ERC721](https://eips.ethereum.org/EIPS/eip-721) and OpenZeppelin's [IGovernor](https://docs.openzeppelin.com/contracts/4.x/api/governance#IGovernor)interfaces.
