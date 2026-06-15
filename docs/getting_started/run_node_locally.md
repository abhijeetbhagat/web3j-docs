Web3j interacts with Ethereum client nodes.
If you don't want to connect to one of the existing networks,
you have different options to start a local node.

## Use Geth or Besu

[Geth](https://geth.ethereum.org/docs/getting-started):

```bash
$ geth --http --http.api eth,net,web3,personal
```

[Hyperledger Besu](https://besu.hyperledger.org/en/stable/HowTo/Get-Started/Starting-node/#run-a-node-for-testing):

```bash
$ besu --network=dev
```

The `dev` network has [handy defaults for local testing](https://besu.hyperledger.org/en/stable/public-networks/reference/genesis-items/#development-mode).

Instructions on obtaining Ether to transact on the network can be found in the [testnet section of the docs](../transactions/ethereum_testnets.md).

If you need Parity/OpenEthereum-compatible RPC coverage for a legacy deployment, Web3j still provides the `parity` module. For most new projects, prefer Geth, Besu, or a hosted provider.

## Use Epirus Local

Epirus Local is a local Ethereum client, similar in spirit to Ganache, written in Kotlin.

This project relies on [web3j-evm](https://github.com/web3j/web3j-evm) to execute user requests.

### Features

- Allows JSON-RPC interactions with a local Ethereum blockchain.
- Generates a default genesis file containing 10 Ethereum accounts or lets you use your own genesis.
- Provides a CLI to run the client.
- Handles these operations:
    - `eth_blockNumber`
    - `eth_getTransactionCount`
    - `eth_getBalance`
    - `eth_sendTransaction`
    - `eth_sendRawTransaction`
    - `eth_estimateGas`
    - `eth_getBlockTransactionCountByHash`
    - `eth_getBlockTransactionCountByNumber`
    - `eth_getTransactionReceipt`
    - `eth_getCode`
    - `eth_call`

### Getting started

`git clone https://github.com/epirus-io/epirus-local && cd epirus-local`

`./gradlew installDist`

#### Run in Linux

`build/install/epirus-local/bin/epirus-local [command]`

#### Run in Windows

`build\install\epirus-local\bin\epirus-local.bat [command]`

### Start the client

```
epirus-local start -d=/path/to/directory -p=9090
```

This command generates a new genesis file with 10 accounts filled with 100 Ether.

![start command](https://raw.githubusercontent.com/epirus-io/epirus-local/master/resources/epirus-local-start-command-demo.gif)

### Load from previous configuration

```
epirus-local load -g=/path/to/genesis
```

This command loads a pre-existing genesis file from a certain path.

![load command](https://raw.githubusercontent.com/epirus-io/epirus-local/master/resources/epirus-local-load-command-demo.gif)
