## Transaction types

The different types of transaction in Web3j work with both `Transaction` and `RawTransaction` objects. The key difference is that `Transaction` objects are sent to the node for signing, while `RawTransaction` objects are signed locally and then submitted with `eth_sendRawTransaction`.

Web3j 5.0.3 supports the following transaction families:

- Legacy transactions
- EIP-2930 access list transactions
- EIP-1559 fee market transactions
- EIP-4844 blob transactions
- EIP-7702 authorization list transactions

### Legacy transactions

Legacy transactions use:

- `gasPrice`
- `gasLimit`
- `nonce`
- `to`
- `value`
- `data`

### EIP-2930 transactions

EIP-2930 transactions add an access list on top of the legacy format.

### EIP-1559 transactions

EIP-1559 transactions replace the single gas price with:

- `maxPriorityFeePerGas`
- `maxFeePerGas`

### EIP-4844 blob transactions

EIP-4844 transactions include blob data and `maxFeePerBlobGas`. These are used for blob-carrying transactions on networks that support proto-danksharding.

### EIP-7702 transactions

EIP-7702 transactions extend the EIP-1559 typed transaction format with an authorization list. See [EIP-7702 Authorization List Transaction](EIP_transaction_types/eip7702_transaction.md).

### Account Abstraction

Account Abstraction user operations are not encoded as `RawTransaction` values. Instead, Web3j exposes the relevant bundler JSON-RPC methods such as `eth_sendUserOperation`. See [Account Abstraction (EIP-4337)](account_abstraction_eip4337.md).
