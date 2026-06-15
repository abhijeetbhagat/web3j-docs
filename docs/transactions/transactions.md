# Transactions

Broadly speaking there are three high-level transaction workflows supported in Web3j:

1.  [Transfer of Ether from one party to another](transfer_eth.md#transfer-of-ether-from-one-party-to-another)
2.  [Creation of a smart contract](transactions_and_smart_contracts.md#creation-of-a-smart-contract)
3.  [Transacting with a smart contract](transactions_and_smart_contracts.md#transacting-with-a-smart-contract)

To undertake any of these transactions, it is necessary to have Ether residing in the Ethereum account which the transactions are taking place from. This is to pay for [Gas](gas.md) costs, which are charged by the client that executes the transaction on your behalf. Instructions for obtaining Ether are described in [Obtaining Ether](obtaining_ether.md).

Additionally, it is possible to query the state of a smart contract. This is described in [Querying the state of a smart contract](transactions_and_smart_contracts.md#querying-the-state-of-a-smart-contract).

![image](../img/web3j_transaction.png)

At the transaction encoding level, Web3j 5.0.3 supports legacy transactions and the major typed transaction families used on Ethereum today. See [Transaction Types](transaction_types.md) for details.
