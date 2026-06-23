Ethereum Logs
=============

Ethereum logs are the records produced by smart contract events during transaction execution. They are included in transaction receipts and can be queried later using log APIs or streamed as new matching logs are emitted on-chain.

In Web3j, logs are commonly used for:

- Monitoring smart contract events such as token transfers, approvals and ownership changes
- Querying historical contract activity for analytics or backfilling application state
- Reacting to on-chain events in near real time
- Filtering logs by contract address and indexed event topics

Historical log queries
----------------------

Use `EthFilter` to define the block range, contract address and topics you want to match, then call `ethGetLogs`.

```java
EthFilter filter =
        new EthFilter(
                DefaultBlockParameterName.EARLIEST,
                DefaultBlockParameterName.LATEST,
                "<contract-address>")
                .addSingleTopic("<event-signature-topic>");

EthLog ethLog = web3j.ethGetLogs(filter).send();
List<EthLog.LogResult<?>> logs = ethLog.getLogs();

for (EthLog.LogResult<?> logResult : logs) {
    if (logResult instanceof EthLog.LogObject) {
        Log log = ((EthLog.LogObject) logResult).get();
        System.out.println(log.getTransactionHash());
    }
}
```

When working with `EthLog`, Web3j uses `List<EthLog.LogResult<?>>` for the returned log entries.

Subscribing to Ethereum logs
----------------------------

To subscribe to new matching logs, use `ethLogFlowable(filter)`. Internally Web3j creates a `LogFilter` and emits matching `Log` objects through a Flowable.

```java
EthFilter filter =
        new EthFilter(
                DefaultBlockParameterName.LATEST,
                DefaultBlockParameterName.LATEST,
                "<contract-address>")
                .addSingleTopic("<event-signature-topic>");

Disposable subscription =
        web3j.ethLogFlowable(filter).subscribe(log -> {
            System.out.println("Block: " + log.getBlockNumber());
            System.out.println("Tx hash: " + log.getTransactionHash());
            System.out.println("Data: " + log.getData());
        });
```

You can cancel the subscription when it is no longer required:

```java
subscription.dispose();
```

Topic filtering
---------------

Ethereum logs can be filtered by indexed event parameters. In Web3j this is done through `EthFilter`.

- `addSingleTopic(...)` matches a specific topic
- `addOptionalTopics(..., ...)` matches one of several topics in the same position
- a contract address can be supplied directly in the `EthFilter` constructor

This makes it possible to subscribe only to the events that are relevant to your application.

Typical use cases
-----------------

- Listen for ERC-20 `Transfer` events to update balances in a wallet or explorer
- Watch protocol-specific events to trigger off-chain jobs
- Backfill historical logs for a dashboard or analytics pipeline
- Track emitted events from a single contract deployment across a chosen block range
