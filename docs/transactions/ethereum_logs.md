Ethereum Logs
=============

Ethereum logs are returned from log queries such as `eth_getLogs` and contain the event data emitted by smart contracts.

### Example

```java
EthFilter filter =
        new EthFilter(
                DefaultBlockParameterName.EARLIEST,
                DefaultBlockParameterName.LATEST,
                "<contract-address>");

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
