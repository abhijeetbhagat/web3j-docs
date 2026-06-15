## Privacy with Hyperledger Besu

The Besu module in Web3j provides support for creating private transactions and privacy groups on Hyperledger Besu. For background on how privacy is implemented in Besu, see the [Besu privacy documentation](https://besu.hyperledger.org/).

Privacy in Besu refers to keeping transaction payloads and state visible only to the intended participants. A privacy group identifies the members that are allowed to view and execute the private payload.

Besu nodes maintain the public world state for the blockchain and a private state for each privacy group. The private state is not shared with non-members.

Private transactions have additional attributes beyond public Ethereum transactions:

- `privateFrom` - enclave public key of the transaction sender
- `privacyGroupId` - privacy group to receive the transaction
- `restriction` - whether the private transaction is restricted or unrestricted

The Web3j Besu transaction managers and response types let you build and submit these transactions from Java code.
