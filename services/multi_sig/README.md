# The Multi-Signature Module

Daowabunga will contain a Multi-Signature (Multi-Sig) module that acts as an intermidiary between multiple identities (signers) and outside parties.

## Initialization

The integration of the Multi-Sig module is a fairly easy task. When the canister is deployed, the identity that is responsible for
this action will become the first signer with a voting power that is equal to "1".

In order to add other signers, the signer has to create a proposal with ChangeVotingPower commands and assign new voting power values to new signers' principal IDs.

## Motions

Only signers can create motions and in order to do so, they need to call the `create_motion` method.

The Multi-Sig module stores the following information about every motion:

- **title:** A 120-character title for the motion.
- **description:** A 5000-character description for the motion.
- **commands:** A vector of commands of either type. Will get executed once the motion passes.
- **proposed_by:** The principal ID of the identity responsible for creating the motion.
- **proposed_at:** The UNIX timestamp of the moment the motion was created.
- **status:** The current status of the motion (Open, Rejected, Failed, Executing, Completed).
- **in_favor:** A HashMap of all signers who have voted in favor of the motion with their voting power at the time of voting.
- **against:** A HashMap of all signers who have voted against of the motion with their voting power at the time of voting.

Only the **first three fields** are required from signers to create new motions. Once a call is made to `create_motion` and a motion is created, the response from the call will be a Nat64 that is the **motion ID**.

## Votes

Only signers can participate in votings and they can either vote in favor (true) or against (false) the motion.
To vote, signers need to call the `cast_vote` method with the following information as parameters:

- **motion_id**: A Nat64 that represents the id of the motion.
- **vote**: A boolean that represents the vote of the signer.

## Commands

Currently the Multi-Sig module only executes two types of commands:

- **ChangeVotingPower:** Commands of this type change the voting power of a signer in the Multi-Sig canister.
- **Call:** Commands of this type call other canisters and return the result.

Note: In order to remove a signer's access to the Multi-Sig, others should create a proposal with a ChangeVotingPower command that changes the voting power of the signer to zero.

## Methods

- **cast_vote**: َUsed for casting new votes (Votes cannot be changed once they are cast).
- **create_motion**: Used to create new motions (Only signers can call this method).
- **get_all_motions**: Used to get the list of all motions (Including the ones that are closed).
- **get_motion**: Used to get the information related to a certain motion ID.
- **get_open_motions**: Used to get the list of all open motions.
- **get_vote**: Used to get the caller's vote on a certain motion.
- **motion_count**: Used to get the total number of all motions in the Multi-Sig module.
