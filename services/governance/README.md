# The Governance Module

Daowabunga’s governance module aims to provide a sophisticated, highly customizable, and easy-to-manage governance system for DAOs on different chains, starting with the Internet Computer Protocol (ICP).

The governance module is flexible and customizable in a way that allows any DAO regardless of their special needs, to use this module as their governance solution. Governance committee members and their voting power can be determined by the number of tokens they stake, their role in the community (admin, non-admin), or even the NFTs they hold.

Even though the framework is built around DAOs, there are many other use cases for it, e.g. NFT projects, treasury management, etc.

## Settings

The governance module offers settings that are optional and affect all proposals as listed below:

- **Deadline:** Amount of seconds a proposal stays open for vote. Once the voting window is over, the proposal's fate will be decided. Is optional.
- **Admin:** The admin can modify settings without making proposals. Just one admin is accepted and once the governance canister moves on from using this option, it will be closed for ever and never can be used again. Is optional.
- **Maximum number of open proposals:** Maximum number of proposals that can stay open at the same time. If the number of current open proposals is equal to this, no new proposal is accepted. Is optional.
- **Early campaigning**: The proposer needs to get a certain amount of endorsements in order to be able to create a new proposal to avoid spam proposals. Is optional.
- **Fees**: The proposer needs to pay a fee in order to be able to create a new proposal. Is optional.

Note: Admins cannot participate in proposals that affect their position such as disabling the Admins option because of the conflict of interest.

A certain type of settings, **criteria**, determines which proposals go through and pass and which proposals get rejected.

In all criterias, once a vote is casted, it can’t be changed.

The following list explains every pre-defined criteria that the module offers:

- **Staked Tokens:** The voting power of every member will be based on the number of tokens that have staked. Staked tokens will be locked after a vote is casted until the proposal voting is finished to avoid manipulation.
- **NFT Holdings:** The voting power of every member will be based on the amount of NFTs they hold (from a specific collection). NFTs can be transferred after a vote has been casted but the new owner won’t be able to vote again with the same NFT.
- **Levels:** Voting power per address is fixed and can only be altered by a proposal. (Not unlike how Multi-Sig manages to close and execute motions.)

Disclaimer: Voting based on token holdings will not be supported to avoid double voting with fungible tokens.

## Proposals

The following structure is used to store proposals:

```rust
pub struct Proposal {
		pub proposer: Prinicpal,
		pub name: String,
		pub description: String,
		pub status: ProposalStatus,
		pub proposed_at: u64,
		pub accepts_votes_at: u64,
		pub votes: Vec<Vote>,
		pub commands: Vec<Command>
}
```

## Votes

The following structure is used to store votes:

```rust
pub struct Vote {
		pub voter: Prinicpal,
		pub vote: bool,
		pub voted_at: u64,
		pub power: u64
}
```

## Commands

Six commands are available in the governance module:

- **Call:** Similar to Multi-Sig's Call command, redirects the call to another canister.
- **ChangeVotingPower:** Similar to Multi-Sig's ChangeVotingPower command, changes the voting power if the criteria is set as levels.
- **ChangeCriteria:** Changes the criteria of proposals in the canister.
- **ChangeTokenCanister:** Changes the principal ID associated with the token canister that is used to determine the voting power in criterias other than levels.
- **ChangeEarlyCampaigning:** Changes the early campaigning option and its details.
- **ChangeFees:** Changes the fees option and its details.
- **ChangeDeadlines:** Changes the deadlines option and its details.
- **ChangeMaxOpenProposals:** Changes the maximum number of open proposals.

## Code Upgrades

Once a new version of the governance module is released and the governance committee wants to upgrade the code, a member will open a proposal and others vote in favor or against the upgrade. If the proposal passes, the governance canister will update itself as it is the only controller of itself and creates an archive of all of the sensitive information inside of it in another canister.
