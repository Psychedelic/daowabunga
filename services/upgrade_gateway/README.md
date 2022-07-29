# The Upgrade Gateway Module
The upgrade gateway module aims to make code upgrades transparent and secure by acting as an intermidiary between its admins and Internet Computer's management canister.
This module acts as a **proxy** between its admins and Internet Computer's management canister that is responsible for doing code upgrades and service modifications.

## Initialization

Once deployed, the upgrade gateway sets the principal ID of the identity responsible for the deployment as the admin of the gateway and from then on, only accepts calls from that particular principal id.
In order to function, the upgrade gateway needs to be the **sole controller** of its target canister. This is a security measure that is needed to make sure no other party can make changes to the target canister without going unnoticed.

## History

The history of the upgrade gateway module follows CAP's history bucket standard.

The upgrade gateway canister stores every update as an Event in its history. These Events include:

- **Init**: Will only be inserted when the canister is first deployed.
- **Upgrade**: Will only be inserted when the canister successfully does a code upgrade by forwarding the call to Internet Computer's management canister.
- **Install**: Will only be inserted when the canister successfully does a code installation by forwarding the call to Internet Computer's management canister.
- **Uninstall**: Will only be inserted when the canister successfully does a code uninstallation by forwarding the call to Internet Computer's management canister.
- **Stop**: Will only be inserted when the canister successfully stops its target canister by forwarding the call to Internet Computer's management canister.
- **Start**: Will only be inserted when the canister successfully starts its target canister by forwarding the call to Internet Computer's management canister.
- **Delete**: Will only be inserted when the canister successfully deletes its target canister by forwarding the call to Internet Computer's management canister.
- **Detaches**: Will only be inserted when the canister successfully sets the admin as the controller of its target canister and removes itself.
- **AdminAdded**: Will only be inserted when the canister successfully adds a new admin to its Admin vector.
- **AdminRemoved**: Will only be inserted when the canister successfully removes an admin from its Admin vector.

## Methods

- **detach**: Used to set the caller as the controller of the target canister and remove upgrade gateway from the controllers list.
- **add_admin**: Used to add a new admin to the Admins vector of the module.
- **remove_admin**: Used to remove an admin from the Admins vector of the module.
- **call**: Used to forward calls to the management canister. Supports calls to these methods on the management canister: (start_canister, stop_canister, uninstall_code, delete_canister, install_code, delete_canister, and uninstall_code)

## Code Upgrades

The upgrade gateway canister has a simple and unique task and that is recording and storing every change that happens on the Internet Computer Protocol level of its target canister and because of this, no code upgrade for this service is expected. Thus, the upgrade gateway module **will not** accept or support any code upgrade for itself.
