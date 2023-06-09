# EVM Architecture

This is where all of our EVM smart contracts live. The basis of this IBC in Solidity implementation is provided by [yui-ibc-solidity](https://github.com/hyperledger-labs/yui-ibc-solidity). We provide [CometblsClient](./contracts/clients/CometblsClient.sol), which is a light client that implements [CometBLS](../docs/docs/architecture/cometbls.md).

```mermaid
---
title: Components
---
flowchart BT
    relayer(Relayer)

    subgraph EVM chain with BN254 precompile
        client(ICS-002 client)
        connection(ICS-003 connection)
        channel(ICS-004 channel)
        handler(ICS-025 handler)
    end

    handler -- CometBLS client --> client
    handler --> connection
    handler --> channel
    relayer --> handler

```

Note that all of the component upgrades are initiated from our Union chain through governance.

```mermaid
---
title: Setup Sequence
---
sequenceDiagram
    Relayer->>Handler: Register CometBLS client type
    Handler->>ICS-002 Client: Register CometBLS client type
    Relayer->>Handler: Create CometBLS client instance
    Handler->>ICS-002 Client: Create CometBLS client instance

    Relayer->>Handler: Create connection
    Handler->>ICS-003 Connection: Create connection

    Relayer->>Handler: Create channel
    Handler->>ICS-004 Channel: Create channel

```