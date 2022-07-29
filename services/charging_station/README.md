# The Charging Station Module
The charging station module focuses on making sure that its target canisters always have enough cycles to operate and their cycles balance
never falls below a certain threshold.

This module is highly customizable and checks the cycles balance of its target canister in specific intervals. The balance that the charging station uses to refill its target canisters cycles balance comes from an allowance that its admins have set aside for it using [Dank's XTC canister](https://github.com/Psychedelic/dank/tree/develop/xtc#set-an-allowance-to-another-identity---approve).

## Customizable Values

The charging station canister uses certain customizable values to determine when and how it is supposed to refill its target canister's cycles balance.

- **cycles_threshold:** The least amount of cycles that the target canister should always have. Will refill if the balance falls below this amount.
- **refill_amount:** The amount of cycles that should be transferred to the target canister's balance in case of the balance being below the threshold.
- **interval:** The amount of seconds that charging station waits for to pass until calling the management canister to get the new cycles balance again.
- **target_canister**: A vector of the principal IDs of its target canisters.

## Methods

- **set_threshold**: Used to update the *cycles_threshold* value.
- **set_refill_amount**: Used to update the *refill_amount* value.
- **interval**: Used to update the *interval* value.
- **target_canisters**: Used to update the *target_canisters* value.
- **add_admin**: Used to add a new admin to its Admin vector.
- **remove_admin**: Used to remove an admin from its Admin vector.
- **get_history**: Used to access history of refills.
