---
description: Hyperledger Besu command line interface reference
tags:
  - private networks
---

# Command line options

This reference describes the syntax of the Hyperledger Besu command line interface (CLI) options.

!!! attention

    This reference contains options that apply to both public and private networks.
    For private-network-specific options, see the
    [private network options reference](../../../private-networks/reference/cli/options.md).

## Specify options

You can specify Besu options:

* On the command line.

    ```bash
    besu [OPTIONS] [SUBCOMMAND]
    ```

* As an environment variable.
  For each command line option, the equivalent environment variable is:
    * Uppercase.
    * `_` replaces `-`.
    * Has a `BESU_` prefix.

    For example, set `--miner-coinbase` using the `BESU_MINER_COINBASE` environment variable.

* In a [configuration file](../../how-to/configuration-file.md).

If you specify an option in more than one place, the order of priority is command line, environment
variable, configuration file.

If using Bash or Z shell, you can view option suggestions by entering `--` and pressing the Tab key twice.

```bash
besu --Tab+Tab
```

## Options

### `api-gas-price-blocks`

=== "Syntax"

    ```bash
    --api-gas-price-blocks=<INTEGER>
    ```

=== "Example"

    ```bash
    --api-gas-price-blocks=50
    ```

=== "Environment variable"

    ```bash
    BESU_API_GAS_PRICE_BLOCKS=50
    ```

=== "Example configuration file"

    ```bash
    api-gas-price-blocks=50
    ```

Number of blocks back from the head block to examine for [`eth_gasPrice`](../api/index.md#eth_gasprice).
The default is `100`.

### `api-gas-price-max`

=== "Syntax"

    ```bash
    --api-gas-price-max=<INTEGER>
    ```

=== "Example"

    ```bash
    --api-gas-price-max=20000
    ```

=== "Environment variable"

    ```bash
    BESU_API_GAS_PRICE_MAX=20000
    ```

=== "Example configuration file"

    ```bash
    api-gas-price-max=20000
    ```

Maximum gas price to return for [`eth_gasPrice`](../api/index.md#eth_gasprice), regardless of the
percentile value measured. The default is `500000000000` (500 GWei).

### `api-gas-price-percentile`

=== "Syntax"

    ```bash
    --api-gas-price-percentile=<DOUBLE>
    ```

=== "Example"

    ```bash
    --api-gas-price-percentile=75
    ```

=== "Environment variable"

    ```bash
    BESU_API_GAS_PRICE_PERCENTILE=75
    ```

=== "Example configuration file"

    ```bash
    api-gas-price-percentile=75
    ```

Percentile value to measure for [`eth_gasPrice`](../api/index.md#eth_gasprice).
The default is `50.0`.

For [`eth_gasPrice`](../api/index.md#eth_gasprice), to return the:

* Highest gas price in [`--api-gas-price-blocks`](#api-gas-price-blocks), set to `100`.
* Lowest gas price in [`--api-gas-price-blocks`](#api-gas-price-blocks), set to `0`.

### `auto-log-bloom-caching-enabled`

=== "Syntax"

    ```bash
    ---auto-log-bloom-caching-enabled[=<true|false>]
    ```

=== "Example"

    ```bash
    --auto-log-bloom-caching-enabled=false
    ```

=== "Environment variable"

    ```bash
    BESU_AUTO_LOG_BLOOM_CACHING_ENABLED=false
    ```

=== "Example configuration file"

    ```bash
    auto-log-bloom-caching-enabled=false
    ```

Enables or disables automatic log bloom caching. APIs such as
[`eth_getLogs`](../api/index.md#eth_getlogs) and
[`eth_getFilterLogs`](../api/index.md#eth_getfilterlogs) use the cache for improved performance.
The default is `true`.

If automatic log bloom caching is enabled and a log bloom query reaches the end of the cache, Besu
performs an uncached query for logs not yet written to the cache.

Automatic log bloom caching has a small impact on performance. If you are not querying logs blooms
for a large number of blocks, you might want to disable automatic log bloom caching.

### `banned-node-ids`

=== "Syntax"

    ```bash
    --banned-node-ids=<bannedNodeId>[,<bannedNodeId>...]...
    ```

=== "Example"

    ```bash
    --banned-node-ids=0xc35c3...d615f,0xf42c13...fc456
    ```

=== "Environment variable"

    ```bash
    BESU_BANNED_NODE_IDS=0xc35c3...d615f,0xf42c13...fc456
    ```

=== "Configuration file"

    ```bash
    banned-node-ids=["0xc35c3...d615f","0xf42c13...fc456"]
    ```

A list of node IDs with which this node will not peer. The node ID is the public key of the node.
You can specify the banned node IDs with or without the `0x` prefix.

!!!tip

    The singular `--banned-node-id` and plural `--banned-node-ids` are available and are two names
    for the same option.

### `bonsai-maximum-back-layers-to-load`

=== "Syntax"

    ```bash
    --bonsai-maximum-back-layers-to-load=256
    ```

=== "Example"

    ```bash
    --bonsai-maximum-back-layers-to-load=256
    ```

=== "Environment variable"

    ```bash
    BESU_BONSAI_MAXIMUM_BACK_LAYERS_TO_LOAD=256
    ```

=== "Example configuration file"

    ```bash
    bonsai-maximum-back-layers-to-load=256
    ```

When using [Bonsai Tries](../../concepts/data-storage-formats.md#bonsai-tries), the
[maximum number of layers back](../../concepts/data-storage-formats.md#accessing-data) Bonsai can go to reconstruct a
historical state.
The default is 512.

### `bootnodes`

=== "Syntax"

    ```bash
    --bootnodes[=<enode://id@host:port>[,<enode://id@host:port>...]...]
    ```

=== "Example"

    ```bash
    --bootnodes=enode://c35c3...d615f@1.2.3.4:30303,enode://f42c13...fc456@1.2.3.5:30303
    ```

=== "Environment variable"

    ```bash
    BESU_BOOTNODES=enode://c35c3...d615f@1.2.3.4:30303,enode://f42c13...fc456@1.2.3.5:30303
    ```

=== "Example configuration file"

    ```bash
    bootnodes=["enode://c35c3...d615f@1.2.3.4:30303","enode://f42c13...fc456@1.2.3.5:30303"]
    ```

A list of comma-separated [enode URLs](../../concepts/node-keys.md#enode-url) for
[P2P discovery bootstrap](../../../private-networks/how-to/configure/bootnodes.md).

When connecting to Mainnet or public testnets, the default is a predefined list of enode URLs.

In private networks defined using [`--genesis-file`](#genesis-file) or when using
[`--network=dev`](#network), the default is an empty list of bootnodes.

### `color-enabled`

=== "Syntax"

    ```bash
    --color-enabled[=<true|false>]
    ```

=== "Example"

    ```bash
    --color-enabled=false
    ```

=== "Environment variable"

    ```bash
    BESU_COLOR_ENABLED=false
    ```

=== "Example configuration file"

    ```bash
    color-enabled=false
    ```

Enables or disables color output to console.
The default is `true`.

### `compatibility-eth64-forkid-enabled`

=== "Syntax"

    ```bash
    --compatibility-eth64-forkid-enabled[=<true|false>]
    ```

=== "Example"

    ```bash
    --compatibility-eth64-forkid-enabled=true
    ```

=== "Environment variable"

    ```bash
    BESU_COMPATIBILITY_ETH64_FORKID_ENABLED=true
    ```

=== "Example configuration file"

    ```bash
    compatibility-eth64-forkid-enabled=true
    ```

Enables or disables the legacy Eth/64 fork ID. For any networks with nodes using Besu v1.4 or earlier and nodes
using Besu v20.10.1 or later, either:

* All nodes must be upgraded to v20.10.1 or later.
* All nodes using v20.10.1 or later must have `--compatibility-eth64-forkid-enabled` set to `true`.

The default is `false`.

!!! caution

    If networks have Besu nodes using v1.4 or earlier and other Besu nodes using v20.10.1 or later,
    the nodes on different versions cannot communicate unless `--compatibility-eth64-forkid-enabled`
    is set to `true`.

### `config-file`

=== "Syntax"

    ```bash
    --config-file=<FILE>
    ```

=== "Example"

    ```bash
    --config-file=/home/me/me_node/config.toml
    ```

=== "Environment variable"

    ```bash
    BESU_CONFIG_FILE=/home/me/me_node/config.toml
    ```

The path to the [TOML configuration file](../../how-to/configuration-file.md).
The default is `none`.

### `data-path`

=== "Syntax"

    ```bash
    --data-path=<PATH>
    ```

=== "Example"

    ```bash
    --data-path=/home/me/me_node
    ```

=== "Environment variable"

    ```bash
    BESU_DATA_PATH=/home/me/me_node
    ```

=== "Configuration file"

    ```bash
    data-path="/home/me/me_node"
    ```

The path to the Besu data directory. The default is the directory you installed Besu in, or
`/opt/besu/database` if using the [Besu Docker image](../../get-started/install/run-docker-image.md).

### `data-storage-format`

=== "Syntax"

    ```bash
    --data-storage-format=<FORMAT>
    ```

=== "Example"

    ```bash
    --data-storage-format=BONSAI
    ```

=== "Environment variable"

    ```bash
    BESU_DATA_STORAGE_FORMAT=BONSAI
    ```

=== "Configuration file"

    ```bash
    data-storage-format="BONSAI"
    ```

The [data storage format](../../concepts/data-storage-formats.md) to use.
Set to `BONSAI` for Bonsai Tries or `FOREST` for Forest of Tries.
The default is `FOREST`.

### `discovery-dns-url`

=== "Syntax"

    ```bash
    --discovery-dns-url=<enrtree URL>
    ```

=== "Environment variable"

    ```bash
    BESU_DISCOVERY_DNS_URL=enrtree://AM5FCQLWIZX2QFPNJAP7VUERCCRNGRHWZG3YYHIUV7BVDQ5FDPRT2@nodes.example.org
    ```

=== "Example configuration file"

    ```bash
    discovery-dns-url="enrtree://AM5FCQLWIZX2QFPNJAP7VUERCCRNGRHWZG3YYHIUV7BVDQ5FDPRT2@nodes.example.org"
    ```

The `enrtree` URL of the DNS node list for [node discovery via DNS](https://eips.ethereum.org/EIPS/eip-1459).
The default is `null`.

### `discovery-enabled`

=== "Syntax"

    ```bash
    --discovery-enabled[=<true|false>]
    ```

=== "Example"

    ```bash
    --discovery-enabled=false
    ```

=== "Environment variable"

    ```bash
    BESU_DISCOVERY_ENABLED=false
    ```

=== "Example configuration file"

    ```bash
    discovery-enabled=false
    ```

Enables or disables P2P discovery.
The default is `true`.

!!! note

    You can override the default DNS server if it's unreliable or doesn't serve TCP DNS requests, using the
    [experimental option](#xhelp) `--Xp2p-dns-discovery-server=<HOST>`.

### `engine-host-allowlist`

=== "Syntax"

    ```bash
    --engine-host-allowlist=<hostname>[,<hostname>...]... or "*"
    ```

=== "Example"

    ```bash
    --engine-host-allowlist=localhost,127.0.0.1
    ```

=== "Environment variable"

    ```bash
    BESU_ENGINE_HOST_ALLOWLIST=localhost,127.0.0.1
    ```

=== "Configuration file"

    ```bash
    engine-host-allowlist=localhost,127.0.0.1
    ```

A comma-separated list of hostnames to allow for Engine API access (applies to both HTTP and WebSocket).

!!!tip

    To allow all hostnames, use `"*"`. We don't recommend allowing all hostnames in production
    environments.

### `engine-jwt-disabled`

=== "Syntax"

    ```bash
    --engine-jwt-disabled[=<true|false>]
    ```

=== "Example"

    ```bash
    --engine-jwt-disabled=true
    ```

=== "Environment variable"

    ```bash
    BESU_ENGINE_JWT_DISABLED=true
    ```

=== "Configuration file"

    ```bash
    engine-jwt-disabled=true
    ```
Disables or enables [authentication](../../how-to/use-engine-api.md#authentication) for Engine APIs.
The default is `false` (authentication is enabled by default).

### `engine-jwt-secret`

=== "Syntax"

    ```bash
    --engine-jwt-secret=<FILE>
    ```

=== "Example"

    ```bash
    --engine-jwt-secret=jwt.hex
    ```

=== "Environment variable"

    ```bash
    BESU_ENGINE_JWT_SECRET="jwt.hex"
    ```

=== "Configuration file"

    ```bash
    engine-jwt-secret="jwt.hex"
    ```

Shared secret used to authenticate [consensus clients](../../concepts/the-merge.md) when using the Engine JSON-RPC API (both
HTTP and WebSocket).
Contents of file must be at least 32 hex-encoded bytes and not begin with `0x`.
May be a relative or absolute path.
See an [example of how to generate this](../../tutorials/merge-testnet.md#prerequisites).

### `engine-rpc-port`

=== "Syntax"

    ```bash
    --engine-rpc-port=<PORT>
    ```

=== "Example"

    ```bash
    --engine-rpc-port=8551
    ```

=== "Environment variable"

    ```bash
    BESU_ENGINE_RPC_PORT=8551
    ```

=== "Configuration file"

    ```bash
    engine-rpc-port=8551
    ```

The listening port for the Engine API calls (`ENGINE`, `ETH`) for JSON-RPC over HTTP and WebSocket.
The default is `8551`.

### `ethstats`

=== "Syntax"

    ```bash
    --ethstats=<nodename:secret@host:port>
    ```

=== "Example"

    ```bash
    --ethstats=Dev-Node-1:secret@127.0.0.1:3001
    ```

=== "Environment variable"

    ```bash
    BESU_ETHSTATS=Dev-Node-1:secret@127.0.0.1:3001
    ```

=== "Configuration file"

    ```bash
    ethstats="Dev-Node-1:secret@127.0.0.1:3001"
    ```

Reporting URL of an [Ethstats](../../../private-networks/how-to/deploy/ethstats.md) server.

### `ethstats-contact`

=== "Syntax"

    ```bash
    --ethstats-contact=<CONTACT>
    ```

=== "Example"

    ```bash
    --ethstats-contact=contact@mail.com
    ```

=== "Environment variable"

    ```bash
    BESU_ETHSTATS_CONTACT=contact@mail.com
    ```

=== "Configuration file"

    ```bash
    ethstats-contact="contact@mail.com"
    ```

Contact email address to send to the Ethstats server specified by [`--ethstats`](#ethstats).

!!! note

    A server must be specified by `--ethstats` in order to use this option.

### `fast-sync-min-peers`

=== "Syntax"

    ```bash
    --fast-sync-min-peers=<INTEGER>
    ```

=== "Example"

    ```bash
    --fast-sync-min-peers=8
    ```

=== "Environment variable"

    ```bash
    BESU_FAST_SYNC_MIN_PEERS=8
    ```

=== "Example configuration file"

    ```bash
    fast-sync-min-peers=8
    ```

The minimum number of peers required before starting [fast synchronization](../../how-to/connect/sync-node.md#run-a-full-node).
The default is 5.

!!! note

    If synchronizing in `FAST` mode, most historical world state data is unavailable. Any methods
    attempting to access unavailable world state data return `null`.

### `genesis-file`

Use the genesis file to create a custom network.

!!!tip

    To use a public Ethereum network such as Goerli, use the [`--network`](#network) option. The
    network option defines the genesis file for public networks.

=== "Syntax"

    ```bash
    --genesis-file=<FILE>
    ```

=== "Example"

    ```bash
    --genesis-file=/home/me/me_node/customGenesisFile.json
    ```

=== "Environment variable"

    ```bash
    BESU_GENESIS_FILE=/home/me/me_node/customGenesisFile.json
    ```

=== "Configuration file"

    ```bash
    genesis-file="/home/me/me_node/customGenesisFile.json"
    ```

The path to the genesis file.

!!!important

    You cannot use the [`--genesis-file`](#genesis-file) and [`--network`](#network) options at the
    same time.

### `graphql-http-cors-origins`

=== "Syntax"

    ```bash
    --graphql-http-cors-origins=<graphQLHttpCorsAllowedOrigins>
    ```

=== "Example"

    ```bash
    --graphql-http-cors-origins="http://medomain.com","https://meotherdomain.com"
    ```

=== "Environment variable"

    ```bash
    BESU_GRAPHQL_HTTP_CORS_ORIGINS="http://medomain.com","https://meotherdomain.com"
    ```

=== "Configuration file"

    ```bash
    graphql-http-cors-origins=["http://medomain.com","https://meotherdomain.com"]
    ```

A list of comma-separated origin domain URLs for CORS validation. The default is none.

### `graphql-http-enabled`

=== "Syntax"

    ```bash
    ---graphql-http-enabled[=<true|false>]
    ```

=== "Example"

    ```bash
    --graphql-http-enabled
    ```

=== "Environment variable"

    ```bash
    BESU_GRAPHQL_HTTP_ENABLED=true
    ```

=== "Configuration file"

    ```bash
    graphql-http-enabled=true
    ```

Enables or disables the GraphQL HTTP service. The default is `false`.

The default GraphQL HTTP service endpoint is `http://127.0.0.1:8547/graphql` if set to `true`.

### `graphql-http-host`

=== "Syntax"

    ```bash
    --graphql-http-host=<HOST>
    ```

=== "Example"

    ```bash
    # to listen on all interfaces
    --graphql-http-host=0.0.0.0
    ```

=== "Environment variable"

    ```bash
    # to listen on all interfaces
    BESU_GRAPHQL_HTTP_HOST=0.0.0.0
    ```

=== "Configuration file"

    ```bash
    graphql-http-host="0.0.0.0"
    ```

The host on which GraphQL HTTP listens.
The default is `127.0.0.1`.

To allow remote connections, set to `0.0.0.0`.

### `graphql-http-port`

=== "Syntax"

    ```bash
    --graphql-http-port=<PORT>
    ```

=== "Example"

    ```bash
    # to listen on port 6175
    --graphql-http-port=6175
    ```

=== "Environment variable"

    ```bash
    # to listen on port 6175
    BESU_GRAPHQL_HTTP_PORT=6175
    ```

=== "Configuration file"

    ```bash
    graphql-http-port="6175"
    ```

The port (TCP) on which GraphQL HTTP listens. The default is `8547`. Ports must be
[exposed appropriately](../../how-to/connect/configure-ports.md).

### `help`

=== "Syntax"

    ```bash
    -h, --help
    ```

Show the help message and exit.

### `host-allowlist`

=== "Syntax"

    ```bash
    --host-allowlist=<hostname>[,<hostname>...]... or "*"
    ```

=== "Example"

    ```bash
    --host-allowlist=medomain.com,meotherdomain.com
    ```

=== "Environment variable"

    ```bash
    BESU_HOST_ALLOWLIST=medomain.com,meotherdomain.com
    ```

=== "Configuration file"

    ```bash
    host-allowlist=["medomain.com", "meotherdomain.com"]
    ```

A comma-separated list of hostnames to [access the JSON-RPC API](../../how-to/use-besu-api/index.md#host-allowlist) and
[pull Besu metrics](../../how-to/monitor/metrics.md).
By default, Besu accepts requests from `localhost` and `127.0.0.1`.

!!! important

    This isn't a permissioning feature.
    If you want to restrict access to the API, we recommend using the
    [Besu authentication mechanism](../../how-to/use-besu-api/authenticate.md) with username and password
    authentication or JWT public key authentication.

!!! note

    If using [Prometheus](https://prometheus.io/) to pull metrics from a node, you must specify all
    the other nodes you want to pull metrics from in the list of allowed hostnames.

!!! tip

    To allow all hostnames, use `"*"`.
    We don't recommend allowing all hostnames for production environments.

### `identity`

=== "Syntax"

    ```bash
    --identity=<String>
    ```

=== "Example"

    ```bash
    --identity=MyNode
    ```

=== "Environment variable"

    ```bash
    BESU_IDENTITY=MyNode
    ```

=== "Configuration file"

    ```bash
    identity="MyNode"
    ```

The name for the node. If specified, it's the second section of the client ID provided by some
Ethereum network explorers. For example, in the client ID
`besu/MyNode/v1.3.4/linux-x86_64/oracle_openjdk-java-11`, the node name is `MyNode`.

If a name is not specified, the name section is not included in the client ID. For example,
`besu/v1.3.4/linux-x86_64/oracle_openjdk-java-11`.

### `key-value-storage`

=== "Syntax"

    ```bash
    --key-value-storage=<keyValueStorageName>
    ```

=== "Example"

    ```bash
    --key-value-storage=rocksdb
    ```

=== "Environment variable"

    ```bash
    BESU_KEY_VALUE_STORAGE=rocksdb
    ```

=== "Configuration file"

    ```bash
    key-value-storage="rocksdb"
    ```

The key-value storage to use. Use this option only if using a storage system provided with a
plugin. The default is `rocksdb`.

For development use only, the `memory` option provides ephemeral storage for sync testing and debugging.

### `logging`

=== "Syntax"

    ```bash
    -l, --logging=<LEVEL>
    ```

=== "Example"

    ```bash
    --logging=DEBUG
    ```

=== "Environment variable"

    ```bash
    BESU_LOGGING=DEBUG
    ```

=== "Example configuration file"

    ```bash
    logging="DEBUG"
    ```

Sets logging verbosity. Log levels are `OFF`, `FATAL`, `ERROR`, `WARN`, `INFO`, `DEBUG`, `TRACE`,
`ALL`. The default is `INFO`.

### `max-peers`

=== "Syntax"

    ```bash
    --max-peers=<INTEGER>
    ```

=== "Example"

    ```bash
    --max-peers=42
    ```

=== "Environment variable"

    ```bash
    BESU_MAX_PEERS=42
    ```

=== "Configuration file"

    ```bash
    max-peers=42
    ```

The maximum number of P2P connections you can establish. The default is 25.

!!! caution

    The minimum number of peers is set by the `--Xp2p-peer-lower-bound` option, which also has a default of 25.
    If you reduce the `--max-peers` from the default, you must also set the `--Xp2p-peer-lower-bound`
    option to the same value or lower.
    For example, if you decrease `--max-peers` to 20, set `--Xp2p-peer-lower-bound` to 20 or lower.

    Note, `Xp2p-peer-lower-bound` is an experimental option.

### `metrics-category`

=== "Syntax"

    ```bash
    --metrics-category=<metrics-category>[,metrics-category...]...
    ```

=== "Example"

    ```bash
    --metrics-category=BLOCKCHAIN,PEERS,PROCESS
    ```

=== "Environment variable"

    ```bash
    BESU_METRICS_CATEGORY=BLOCKCHAIN,PEERS,PROCESS
    ```

=== "Configuration file"

    ```bash
    metrics-category=["BLOCKCHAIN","PEERS","PROCESS"]
    ```

A comma-separated list of categories for which to track metrics. The defaults are `BLOCKCHAIN`,
`ETHEREUM`, `EXECUTORS`, `JVM`, `NETWORK`, `PEERS`, `PERMISSIONING`, `PROCESS`, `PRUNER`, `RPC`,
`STRATUM`, `SYNCHRONIZER`, and `TRANSACTION_POOL`.

Other categories are `KVSTORE_ROCKSDB`, `KVSTORE_PRIVATE_ROCKSDB`, `KVSTORE_ROCKSDB_STATS`, and
`KVSTORE_PRIVATE_ROCKSDB_STATS`.

Categories containing `PRIVATE` track metrics when you enable
[private transactions](../../../private-networks/concepts/privacy/index.md).

### `metrics-enabled`

=== "Syntax"

    ```bash
    ---metrics-enabled[=<true|false>]
    ```

=== "Example"

    ```bash
    --metrics-enabled
    ```

=== "Environment variable"

    ```bash
    BESU_METRICS_ENABLED=true
    ```

=== "Configuration file"

    ```bash
    metrics-enabled=true
    ```

Enables or disables the
[metrics exporter](../../how-to/monitor/metrics.md#monitor-node-performance-using-prometheus). The
default is `false`.

You can't specify `--metrics-enabled` with [`--metrics-push-enabled`](#metrics-push-enabled). That is, you can enable
either Prometheus polling or Prometheus push gateway support, but not both at once.

### `metrics-host`

=== "Syntax"

    ```bash
    --metrics-host=<HOST>
    ```

=== "Example"

    ```bash
    --metrics-host=127.0.0.1
    ```

=== "Environment variable"

    ```bash
    BESU_METRICS_HOST=127.0.0.1
    ```

=== "Configuration file"

    ```bash
    metrics-host="127.0.0.1"
    ```

The host on which [Prometheus](https://prometheus.io/) accesses
[Besu metrics](../../how-to/monitor/metrics.md#monitor-node-performance-using-prometheus). The
metrics server respects the [`--host-allowlist` option](#host-allowlist).

The default is `127.0.0.1`.

### `metrics-port`

=== "Syntax"

    ```bash
    --metrics-port=<PORT>
    ```

=== "Example"

    ```bash
    --metrics-port=6174
    ```

=== "Environment variable"

    ```bash
    BESU_METRICS_PORT=6174
    ```

=== "Configuration file"

    ```bash
    metrics-port="6174"
    ```

The port (TCP) on which [Prometheus](https://prometheus.io/) accesses
[Besu metrics](../../how-to/monitor/metrics.md#monitor-node-performance-using-prometheus). The
default is `9545`. Ports must be
[exposed appropriately](../../how-to/connect/configure-ports.md).

### `metrics-protocol`

=== "Syntax"

    ```bash
    --metrics-protocol=<metrics-protocol>
    ```

=== "Example"

    ```bash
    --metrics-protocol=OPENTELEMETRY
    ```

=== "Environment variable"

    ```bash
    BESU_METRICS_PROTOCOL=OPENTELEMETRY
    ```

=== "Configuration file"

    ```bash
    metrics-protocol="OPENTELEMETRY"
    ```

Metrics protocol to use: `PROMETHEUS`, `OPENTELEMETRY`, or `NONE`.
The default is `PROMETHEUS`.

### `metrics-push-enabled`

=== "Syntax"

    ```bash
    --metrics-push-enabled[=<true|false>]
    ```

=== "Example"

    ```bash
    --metrics-push-enabled=true
    ```

=== "Environment variable"

    ```bash
    BESU_METRICS_PUSH_ENABLED=true
    ```

=== "Configuration file"

    ```bash
    metrics-push-enabled=true
    ```

Enables or disables [push gateway integration].

You can't specify `--metrics-push-enabled` with [`--metrics-enabled`](#metrics-enabled). That is, you can enable
either Prometheus polling or Prometheus push gateway support, but not both at once.

### `metrics-push-host`

=== "Syntax"

    ```bash
    --metrics-push-host=<HOST>
    ```

=== "Example"

    ```bash
    --metrics-push-host=127.0.0.1
    ```

=== "Environment variable"

    ```bash
    BESU_METRICS_PUSH_HOST=127.0.0.1
    ```

=== "Configuration file"

    ```bash
    metrics-push-host="127.0.0.1"
    ```

The host of the [Prometheus Push Gateway](https://github.com/prometheus/pushgateway). The default
is `127.0.0.1`. The metrics server respects the [`--host-allowlist` option](#host-allowlist).

!!! note

    When pushing metrics, ensure you set `--metrics-push-host` to the machine on which the push
    gateway is. Generally, this is a different machine to the machine on which Besu is running.

### `metrics-push-interval`

=== "Syntax"

    ```bash
    --metrics-push-interval=<INTEGER>
    ```

=== "Example"

    ```bash
    --metrics-push-interval=30
    ```

=== "Environment variable"

    ```bash
    BESU_METRICS_PUSH_INTERVAL=30
    ```

=== "Configuration file"

    ```bash
    metrics-push-interval=30
    ```

The interval, in seconds, to push metrics when in `push` mode. The default is 15.

### `metrics-push-port`

=== "Syntax"

    ```bash
    --metrics-push-port=<PORT>
    ```

=== "Example"

    ```bash
    --metrics-push-port=6174
    ```

=== "Environment variable"

    ```bash
    BESU_METRICS_PUSH_PORT=6174
    ```

=== "Configuration file"

    ```bash
    metrics-push-port="6174"
    ```

The port (TCP) of the [Prometheus Push Gateway](https://github.com/prometheus/pushgateway). The
default is `9001`. Ports must be
[exposed appropriately](../../how-to/connect/configure-ports.md).

### `metrics-push-prometheus-job`

=== "Syntax"

    ```bash
    --metrics-push-prometheus-job=<metricsPrometheusJob>
    ```

=== "Example"

    ```bash
    --metrics-push-prometheus-job="my-custom-job"
    ```

=== "Environment variable"

    ```bash
    BESU_METRICS_PUSH_PROMETHEUS_JOB="my-custom-job"
    ```

=== "Configuration file"

    ```bash
    metrics-push-prometheus-job="my-custom-job"
    ```

The job name when in `push` mode. The default is `besu-client`.

### `min-block-occupancy-ratio`

=== "Syntax"

    ```bash
    --min-block-occupancy-ratio=<minBlockOccupancyRatio>
    ```

=== "Example"

    ```bash
    --min-block-occupancy-ratio=0.5
    ```

=== "Environment variable"

    ```bash
    BESU_MIN_BLOCK_OCCUPANCY_RATIO=0.5
    ```

=== "Configuration file"

    ```bash
    min-block-occupancy-ratio="0.5"
    ```

Minimum occupancy ratio for a mined block if the transaction pool is not empty. When filling a block
during mining, the occupancy ratio indicates the threshold at which the node stops waiting for smaller transactions
to fill the remaining space. The default is 0.8.

### `miner-coinbase`

=== "Syntax"

    ```bash
    --miner-coinbase=<Ethereum account address>
    ```

=== "Example"

    ```bash
    --miner-coinbase=fe3b557e8fb62b89f4916b721be55ceb828dbd73
    ```

=== "Environment variable"

    ```bash
    BESU_MINER_COINBASE=fe3b557e8fb62b89f4916b721be55ceb828dbd73
    ```

=== "Configuration file"

    ```bash
    miner-coinbase="0xfe3b557e8fb62b89f4916b721be55ceb828dbd73"
    ```

The account you pay mining rewards to. You must specify a valid coinbase when you enable mining
using the [`--miner-enabled`](#miner-enabled) option or the
[`miner_start`](../api/index.md#miner_start) JSON-RPC API method.

!!!note

    Besu ignores this option in networks using
    [Clique](../../../private-networks/how-to/configure/consensus/clique.md),
    [IBFT 2.0](../../../private-networks/how-to/configure/consensus/ibft.md), or
    [QBFT](../../../private-networks/how-to/configure/consensus/qbft.md) consensus protocols.

### `miner-enabled`

=== "Syntax"

    ```bash
    --miner-enabled[=<true|false>]
    ```

=== "Example"

    ```bash
    --miner-enabled=true
    ```

=== "Environment variable"

    ```bash
    BESU_MINER_ENABLED=true
    ```

=== "Configuration file"

    ```bash
    miner-enabled=true
    ```

Enables or disables mining when you start the node.
The default is `false`.

### `miner-extra-data`

=== "Syntax"

    ```bash
    --miner-extra-data=<Extra data>
    ```

=== "Example"

    ```bash
    --miner-extra-data=0x444F4E27542050414E4943202120484F444C2C20484F444C2C20484F444C2021
    ```

=== "Environment variable"

    ```bash
    BESU_MINER_EXTRA_DATA=0x444F4E27542050414E4943202120484F444C2C20484F444C2C20484F444C2021
    ```

=== "Configuration file"

    ```bash
    miner-extra-data="0x444F4E27542050414E4943202120484F444C2C20484F444C2C20484F444C2021"
    ```

A hex string representing the 32 bytes included in the extra data field of a mined block. The
default is 0x.

### `miner-stratum-enabled`

=== "Syntax"

    ```bash
    --miner-stratum-enabled
    ```

=== "Environment variable"

    ```bash
    BESU_MINER_STRATUM_ENABLED=true
    ```

=== "Configuration file"

    ```bash
    miner-stratum-enabled=true
    ```

Enables a node to perform stratum mining.
The default is `false`.

### `miner-stratum-host`

=== "Syntax"

    ```bash
    --miner-stratum-host=<HOST>
    ```

=== "Example"

    ```bash
    --miner-stratum-host=192.168.1.132
    ```

=== "Environment variable"

    ```bash
    BESU_MINER_STRATUM_HOST=192.168.1.132
    ```

=== "Configuration file"

    ```bash
    miner-stratum-host="192.168.1.132"
    ```

The host of the stratum mining service.
The default is `0.0.0.0`.

### `miner-stratum-port`

=== "Syntax"

    ```bash
    --miner-stratum-port=<PORT>
    ```

=== "Example"

    ```bash
    --miner-stratum-port=8010
    ```

=== "Environment variable"

    ```bash
    BESU_MINER_STRATUM_PORT=8010
    ```

=== "Configuration file"

    ```bash
    miner-stratum-port="8010"
    ```

The port of the stratum mining service. The default is `8008`. You must
[expose ports appropriately](../../how-to/connect/configure-ports.md).

### `min-gas-price`

=== "Syntax"

    ```bash
    --min-gas-price=<minTransactionGasPrice>
    ```

=== "Example"

    ```bash
    --min-gas-price=1337
    ```

=== "Environment variable"

    ```bash
    BESU_MIN_GAS_PRICE=1337
    ```

=== "Configuration file"

    ```bash
    min-gas-price=1337
    ```

The minimum price a transaction offers to include it in a mined block. The minimum gas price is the
lowest value [`eth_gasPrice`](../api/index.md#eth_gasprice) can return. The default is 1000
Wei.

!!! important
    In a [free gas network](../../../private-networks/how-to/configure/free-gas.md), ensure the minimum gas price is set to zero for every node.
    Any node with a minimum gas price set higher than zero will silently drop transactions with a zero gas price.
    You can query a node's gas configuration using [`eth_gasPrice`](../api/index.md#eth_gasprice).

### `nat-method`

=== "Syntax"

    ```bash
    --nat-method=UPNP
    ```

=== "Example configuration file"

    ```bash
    nat-method="UPNP"
    ```

Specify the method for handling [NAT environments](../../how-to/connect/specify-nat.md).
The options are:

* [`UPNP`](../../how-to/connect/specify-nat.md#upnp)
* [`UPNPP2PONLY`](../../how-to/connect/specify-nat.md#upnp)
* [`KUBERNETES`](../../how-to/connect/specify-nat.md#kubernetes)
* [`DOCKER`](../../how-to/connect/specify-nat.md#docker)
* [`AUTO`](../../how-to/connect/specify-nat.md#auto)
* [`NONE`](../../how-to/connect/specify-nat.md#none).

The default is `AUTO`. `NONE` disables NAT functionality.

!!!tip

    UPnP support is often disabled by default in networking firmware. If disabled by default,
    explicitly enable UPnP support.

!!!tip

    Use `UPNPP2PONLY` if you wish to enable UPnP for p2p traffic but not JSON-RPC.

!!!notes

    Specifying `UPNP` might introduce delays during node startup, especially on networks without a
    UPnP gateway device.

    You must specify `DOCKER` when using the
    [Besu Docker image](../../get-started/install/run-docker-image.md).

### `network`

=== "Syntax"

    ```bash
    --network=<NETWORK>
    ```

=== "Example"

    ```bash
    --network=goerli
    ```

=== "Environment variable"

    ```bash
    BESU_NETWORK=goerli
    ```

=== "Configuration file"

    ```bash
    network="goerli"
    ```

The predefined network configuration.
The default is `mainnet`.

Possible values are:

| Network   | Chain | Type        | Default Sync Mode  | Description                                                    |
|:----------|:------|:------------|:-------------------|:---------------------------------------------------------------|
| `mainnet` | ETH   | Production  | [FAST](#sync-mode) | The main network                                               |
| `goerli`  | ETH   | Test        | [FAST](#sync-mode) | A PoA network using Clique                                     |
| `sepolia` | ETH   | Test        | [FAST](#sync-mode) | A PoW network                                                  |
| `dev`     | ETH   | Development | [FULL](#sync-mode) | A PoW network with a low difficulty to enable local CPU mining |
| `classic` | ETC   | Production  | [FAST](#sync-mode) | The main Ethereum Classic network                              |
| `mordor ` | ETC   | Test        | [FAST](#sync-mode) | A PoW network                                                  |
| `kotti`   | ETC   | Test        | [FAST](#sync-mode) | A PoA network using Clique                                     |
| `astor`   | ETC   | Test        | [FAST](#sync-mode) | A PoW network                                                  |

!!! tip

    Values are case insensitive, so either `mainnet` or `MAINNET` works.

!!! important

    - You can't use the `--network` and [`--genesis-file`](#genesis-file) options at the same time.

    - The Ropsten, Rinkeby, and Kiln testnets are deprecated.

### `network-id`

=== "Syntax"

    ```bash
    --network-id=<INTEGER>
    ```

=== "Example"

    ```bash
    --network-id=8675309
    ```

=== "Environment variable"

    ```bash
    BESU_NETWORK_ID=8675309
    ```

=== "Configuration file"

    ```bash
    network-id="8675309"
    ```

The [P2P network identifier](../../concepts/network-and-chain-id.md).

Use this option to override the default network ID. The default value is the same as the chain ID
defined in the genesis file.

### `node-private-key-file`

=== "Syntax"

    ```bash
    --node-private-key-file=<FILE>
    ```

=== "Example"

    ```bash
    --node-private-key-file=/home/me/me_node/myPrivateKey
    ```

=== "Environment variable"

    ```bash
    BESU_NODE_PRIVATE_KEY_FILE=/home/me/me_node/myPrivateKey
    ```

=== "Configuration file"

    ```bash
    node-private-key-file="/home/me/me_node/myPrivateKey"
    ```

The private key file for the node. The default is the key file in the [data directory](#data-path).
If no key file exists, Besu creates a key file containing the generated private key, otherwise, the
existing key file specifies the node private key.

!!!attention

    The private key is not encrypted.

This option is ignored if [`--security-module`](#security-module) is set to
a non-default value.

### `p2p-enabled`

=== "Syntax"

    ```bash
    --p2p-enabled[=<true|false>]
    ```

=== "Example"

    ```bash
    --p2p-enabled=false
    ```

=== "Environment variable"

    ```bash
    BESU_P2P_ENABLED=false
    ```

=== "Configuration file"

    ```bash
    p2p-enabled=false
    ```

Enables or disables all P2P communication.
The default is `true`.

### `p2p-host`

=== "Syntax"

    ```bash
    --p2p-host=<HOST>
    ```

=== "Example"

    ```bash
    # to listen on all interfaces
    --p2p-host=0.0.0.0
    ```

=== "Environment variable"

    ```bash
    # to listen on all interfaces
    BESU_P2P_HOST=0.0.0.0
    ```

=== "Configuration file"

    ```bash
    p2p-host="0.0.0.0"
    ```

The advertised host that can be used to access the node from outside the network in
[P2P communication](../../how-to/connect/configure-ports.md#p2p-networking).
The default is `127.0.0.1`.

!!! info

    If [`--nat-method`](#nat-method) is set to [`NONE`](../../how-to/connect/specify-nat.md),
    `--p2p-host` is not overridden and must be specified for the node to be accessed from outside the network.

### `p2p-interface`

=== "Syntax"

    ```bash
    --p2p-interface=<HOST>
    ```

=== "Example"

    ```bash
    --p2p-interface=192.168.1.132
    ```

=== "Environment variable"

    ```bash
    BESU_P2P_INTERFACE=192.168.1.132
    ```

=== "Configuration file"

    ```bash
    p2p-interface="192.168.1.132"
    ```

The network interface on which the node listens for
[P2P communication](../../how-to/connect/configure-ports.md#p2p-networking). Use the
option to specify the required network interface when the device that Besu is running on has
multiple network interfaces. The default is 0.0.0.0 (all interfaces).

### `p2p-port`

=== "Syntax"

    ```bash
    --p2p-port=<PORT>
    ```

=== "Example"

    ```bash
    # to listen on port 1789
    --p2p-port=1789
    ```

=== "Environment variable"

    ```bash
    # to listen on port 1789
    BESU_P2P_PORT=1789
    ```

=== "Configuration file"

    ```bash
    p2p-port="1789"
    ```

The P2P listening ports (UDP and TCP). The default is `30303`. You must
[expose ports appropriately](../../how-to/connect/configure-ports.md).

### `pruning-block-confirmations`

=== "Syntax"

    ```bash
    --pruning-block-confirmations=<INTEGER>
    ```

=== "Example"

    ```bash
    --pruning-block-confirmations=5
    ```

=== "Environment variable"

    ```bash
    BESU_PRUNING_BLOCK_CONFIRMATIONS=5
    ```

=== "Configuration file"

    ```bash
    pruning-block-confirmations=5
    ```

The minimum number of confirmations on a block before marking of newly-stored or in-use state trie
nodes that cannot be pruned. The default is 10.

!!! important
    Using pruning with [private transactions](../../../private-networks/concepts/privacy/index.md) is not
    supported.

### `pruning-blocks-retained`

=== "Syntax"

    ```bash
    --pruning-blocks-retained=<INTEGER>
    ```

=== "Example"

    ```bash
    --pruning-blocks-retained=10000
    ```

=== "Environment variable"

    ```bash
    BESU_PRUNING_BLOCKS_RETAINED=10000
    ```

=== "Configuration file"

    ```bash
    pruning-blocks-retained=10000
    ```

The minimum number of recent blocks to keep the entire world state for. The default is 1024.

!!! important

    Using pruning with [private transactions](../../../private-networks/concepts/privacy/index.md) isn't
    supported.

### `pruning-enabled`

=== "Syntax"

    ```bash
    --pruning-enabled
    ```

=== "Example"

    ```bash
    --pruning-enabled=true
    ```

=== "Environment variable"

    ```bash
    BESU_PRUNING_ENABLED=true
    ```

=== "Configuration file"

    ```bash
    pruning-enabled=true
    ```

Enables [pruning](../../concepts/data-storage-formats.md#pruning) to reduce storage required for the
world state.
The default is `false`.

!!! important

    Using pruning with [private transactions](../../../private-networks/concepts/privacy/index.md) isn't
    supported.

!!! important

    Pruning is being deprecated for [Bonsai Tries](../../concepts/data-storage-formats.md#bonsai-tries)
    and is currently not being updated.

### `random-peer-priority-enabled`

=== "Syntax"

    ```bash
    --random-peer-priority-enabled[=<true|false>]
    ```

=== "Example"

    ```bash
    --random-peer-priority-enabled=true
    ```

=== "Environment variable"

    ```bash
    BESU_RANDOM_PEER_PRIORITY_ENABLED=true
    ```

=== "Configuration file"

    ```bash
    random-peer-priority-enabled=true
    ```

Enables or disables random prioritization of incoming connections. Enable in small, stable networks to prevent
closed groups of peers forming. The default is `false`.

### `remote-connections-limit-enabled`

=== "Syntax"

    ```bash
    --remote-connections-limit-enabled[=<true|false>]
    ```

=== "Example"

    ```bash
    --remote-connections-limit-enabled=false
    ```

=== "Environment variable"

    ```bash
    BESU_REMOTE_CONNECTIONS_LIMIT_ENABLED=false
    ```

=== "Configuration file"

    ```bash
    remote-connections-limit-enabled=false
    ```

Enables or disables using the [`--remote-connections-max-percentage`](#remote-connections-max-percentage) option to
limit the percentage of remote P2P connections initiated by peers.
The default is `true`.

!!! tip

    In private and permissioned networks with a level of trust between peers, disabling the remote connection limits
    may increase the speed at which nodes can join the network.

!!! important

    To prevent eclipse attacks, ensure you enable the remote connections limit when connecting to
    any public network, and especially when using [`--sync-mode`](#sync-mode) and
    [`--fast-sync-min-peers`](#fast-sync-min-peers).

### `remote-connections-max-percentage`

=== "Syntax"

    ```bash
    --remote-connections-max-percentage=<DOUBLE>
    ```

=== "Example"

    ```bash
    --remote-connections-max-percentage=25
    ```

=== "Environment variable"

    ```bash
    BESU_REMOTE_CONNECTIONS_MAX_PERCENTAGE=25
    ```

=== "Configuration file"

    ```bash
    remote-connections-max-percentage=25
    ```

The percentage of remote P2P connections you can establish with the node. Must be between 0 and
100, inclusive. The default is 60.

### `reorg-logging-threshold`

=== "Syntax"

    ```bash
    --reorg-logging-threshold=<INTEGER>
    ```

=== "Example"

    ```bash
    --reorg-logging-threshold=3
    ```

=== "Environment variable"

    ```bash
    BESU_REORG_LOGGING_THRESHOLD=3
    ```

=== "Configuration file"

    ```bash
    reorg-logging-threshold=3
    ```

Minimum depth of chain reorganizations to log. The default is 6.

### `required-block`

=== "Syntax"

    ```bash
    --required-block, --required-blocks[=BLOCK=HASH[,BLOCK=HASH...]...]
    ```

=== "Example"

    ```bash
    --required-block=6485846=0x43f0cd1e5b1f9c4d5cda26c240b59ee4f1b510d0a185aa8fd476d091b0097a80
    ```

=== "Environment variable"

    ```bash
    BESU_REQUIRED_BLOCK=6485846=0x43f0cd1e5b1f9c4d5cda26c240b59ee4f1b510d0a185aa8fd476d091b0097a80
    ```

=== "Configuration file"

    ```bash
    required-block=["6485846=0x43f0cd1e5b1f9c4d5cda26c240b59ee4f1b510d0a185aa8fd476d091b0097a80"]
    ```

Requires a peer with the specified block number to have the specified hash when connecting, or Besu
rejects that peer.

### `revert-reason-enabled`

=== "Syntax"

    ```bash
    --revert-reason-enabled[=<true|false>]
    ```

=== "Example"

    ```bash
    --revert-reason-enabled=true
    ```

=== "Environment variable"

    ```bash
    BESU_REVERT_REASON_ENABLED=true
    ```

=== "Configuration file"

    ```bash
    revert-reason-enabled=true
    ```

Enables or disables including the [revert reason](../../../private-networks/how-to/send-transactions/revert-reason.md) in the
transaction receipt, [`eth_estimateGas`](../api/index.md#eth_estimategas) error response,
[`eth_call`](../api/index.md#eth_call) error response, and [`trace`](../trace-types.md#trace) response.
The default is `false`.

!!! caution

    Enabling revert reason may use a significant amount of memory. We don't recommend enabling
    revert reason when connected to public Ethereum networks.

### `rpc-http-api`

=== "Syntax"

    ```bash
    --rpc-http-api=<api name>[,<api name>...]...
    ```

=== "Example"

    ```bash
    --rpc-http-api=ETH,NET,WEB3
    ```

=== "Environment variable"

    ```bash
    BESU_RPC_HTTP_API=ETH,NET,WEB3
    ```

=== "Configuration file"

    ```bash
    rpc-http-api=["ETH","NET","WEB3"]
    ```

A comma-separated list of APIs to enable on the HTTP JSON-RPC channel. When you use this option
you must also specify the `--rpc-http-enabled` option. The available API options are: `ADMIN`,
`CLIQUE`, `DEBUG`, `EEA`, `ETH`, `IBFT`, `MINER`, `NET`, `PERM`, `PLUGINS`, `PRIV`, `QBFT`, `TRACE`,
`TXPOOL`, and `WEB3`. The default is: `ETH`, `NET`, `WEB3`.

!!!tip

    The singular `--rpc-http-api` and plural `--rpc-http-apis` are available and are two names for
    the same option.

### `rpc-http-authentication-credentials-file`

=== "Syntax"

    ```bash
    --rpc-http-authentication-credentials-file=<FILE>
    ```

=== "Example"

    ```bash
    --rpc-http-authentication-credentials-file=/home/me/me_node/auth.toml
    ```

=== "Environment variable"

    ```bash
    BESU_RPC_HTTP_AUTHENTICATION_CREDENTIALS_FILE=/home/me/me_node/auth.toml
    ```

=== "Configuration file"

    ```bash
    rpc-http-authentication-credentials-file="/home/me/me_node/auth.toml"
    ```

The [credentials file](../../how-to/use-besu-api/authenticate.md#credentials-file) for JSON-RPC
API [authentication](../../how-to/use-besu-api/authenticate.md).

### `rpc-http-authentication-enabled`

=== "Syntax"

    ```bash
    --rpc-http-authentication-enabled[=<true|false>]
    ```

=== "Example"

    ```bash
    --rpc-http-authentication-enabled=true
    ```

=== "Environment variable"

    ```bash
    BESU_RPC_HTTP_AUTHENTICATION_ENABLED=true
    ```

=== "Configuration file"

    ```bash
    rpc-http-authentication-enabled=true
    ```

Enables or disables [authentication](../../how-to/use-besu-api/authenticate.md) for the HTTP JSON-RPC
service.

### `rpc-http-authentication-jwt-public-key-file`

=== "Syntax"

    ```bash
    --rpc-http-authentication-jwt-public-key-file=<FILE>
    ```

=== "Example"

    ```bash
    --rpc-http-authentication-jwt-public-key-file=publicKey.pem
    ```

=== "Environment variable"

    ```bash
    BESU_RPC_HTTP_AUTHENTICATION_JWT_PUBLIC_KEY_FILE="publicKey.pem"
    ```

=== "Configuration file"

    ```bash
    rpc-http-authentication-jwt-public-key-file="publicKey.pem"
    ```

The [JWT provider's public key file] used for JSON-RPC HTTP authentication with an external JWT.

### `rpc-http-cors-origins`

=== "Syntax"

    ```bash
    --rpc-http-cors-origins=<url>[,<url>...]... or all or "*"
    ```

=== "Example"

    ```bash

    $# You can allow one or more domains with a comma-separated list.

    --rpc-http-cors-origins=http://medomain.com,https://meotherdomain.com
    ```

=== "Environment variable"

    ```bash
    BESU_RPC_HTTP_CORS_ORIGINS=http://medomain.com,https://meotherdomain.com
    ```

=== "Configuration file"

    ```bash
    rpc-http-cors-origins=["http://medomain.com","https://meotherdomain.com"]
    ```

=== "Remix example"

    ```bash

    $# The following allows Remix to interact with your Besu node.

    --rpc-http-cors-origins=http://remix.ethereum.org
    ```

A list of domain URLs for CORS validation.

Listed domains can access the node using JSON-RPC. If your client interacts with Besu using a
browser app (such as Remix or a block explorer), add the client domain to the list.

The default value is `"none"`. If you do not list any domains, browser apps cannot interact
with your Besu node.

!!!note

    To run a local Besu node with MetaMask, set `--rpc-http-cors-origins` to
    `chrome-extension://nkbihfbeogaeaoehlefnkodbefgpgknn`.

    Remember to also include the dapp domain MetaMask interacts with, for example if your app is deployed
    on Remix and you're using MetaMask to interact with the contract, use
    `--rpc-http-cors-origins=chrome-extension://nkbihfbeogaeaoehlefnkodbefgpgknn,http://remix.ethereum.org`

!!!tip

    For testing and development purposes, use `"all"` or `"*"` to accept requests from any domain.
    We don't recommend accepting requests from any domain for production environments.

### `rpc-http-enabled`

=== "Syntax"

    ```bash
    --rpc-http-enabled[=<true|false>]
    ```

=== "Example"

    ```bash
    --rpc-http-enabled=true
    ```

=== "Environment variable"

    ```bash
    BESU_RPC_HTTP_ENABLED=true
    ```

=== "Configuration file"

    ```bash
    rpc-http-enabled=true
    ```

Enables or disables the HTTP JSON-RPC service.
The default is `false`.

### `rpc-http-host`

=== "Syntax"

    ```bash
    --rpc-http-host=<HOST>
    ```

=== "Example"

    ```bash
    # to listen on all interfaces
    --rpc-http-host=0.0.0.0
    ```

=== "Environment variable"

    ```bash
    BESU_RPC_HTTP_HOST=0.0.0.0
    ```

=== "Configuration file"

    ```bash
    rpc-http-host="0.0.0.0"
    ```

The host on which HTTP JSON-RPC listens. The default is `127.0.0.1`.

To allow remote connections, set to `0.0.0.0`.

!!! caution

    Setting the host to `0.0.0.0` exposes the RPC connection on your node to any remote connection.
    In a production environment, ensure you are using a firewall to avoid exposing your node to the
    internet.

### `rpc-http-max-active-connections`

=== "Syntax"

    ```bash
    --rpc-http-max-active-connections=<INTEGER>
    ```

=== "Example"

    ```bash
    --rpc-http-max-active-connections=100
    ```

=== "Environment variable"

    ```bash
    BESU_RPC_HTTP_MAX_ACTIVE_CONNECTIONS=100
    ```

=== "Configuration file"

    ```toml
    rpc-http-max-active-connections=100
    ```

The maximum number of allowed HTTP JSON-RPC connections. Once this limit is reached, incoming connections are rejected. The default is 80.

### `rpc-http-port`

=== "Syntax"

    ```bash
    --rpc-http-port=<PORT>
    ```

=== "Example"

    ```bash
    # to listen on port 3435
    --rpc-http-port=3435
    ```

=== "Environment variable"

    ```bash
    BESU_RPC_HTTP_PORT=3435
    ```

=== "Configuration file"

    ```bash
    rpc-http-port="3435"
    ```

The port (TCP) on which HTTP JSON-RPC listens. The default is `8545`. You must
[expose ports appropriately](../../how-to/connect/configure-ports.md).

### `rpc-http-tls-ca-clients-enabled`

=== "Syntax"

    ```bash
    --rpc-http-tls-ca-clients-enabled[=<true|false>]
    ```

=== "Example"

    ```bash
    --rpc-http-tls-ca-clients-enabled=true
    ```

=== "Environment variable"

    ```bash
    BESU_RPC_HTTP_TLS_CA_CLIENTS_ENABLED=true
    ```

=== "Configuration file"

    ```bash
    rpc-http-tls-ca-clients-enabled=true
    ```

Enables or disables clients with trusted CA certificates to connect. The default is `false`.

!!! note

    You must enable client authentication using the
    [`---rpc-http-tls-client-auth-enabled`](#rpc-http-tls-client-auth-enabled) option.

### `rpc-http-tls-client-auth-enabled`

=== "Syntax"

    ```bash
    --rpc-http-tls-client-auth-enabled[=<true|false>]
    ```

=== "Example"

    ```bash
    --rpc-http-tls-client-auth-enabled=true
    ```

=== "Environment variable"

    ```bash
    BESU_RPC_HTTP_TLS_CLIENT_AUTH_ENABLED=true
    ```

=== "Configuration file"

    ```bash
    rpc-http-tls-client-auth-enabled=true
    ```

Enables or disables TLS client authentication for the JSON-RPC HTTP service. The default is `false`.

!!! note

    You must specify [`--rpc-http-tls-ca-clients-enabled`](#rpc-http-tls-ca-clients-enabled) and/or
    [`rpc-http-tls-known-clients-file`](#rpc-http-tls-known-clients-file).

### `rpc-http-tls-cipher-suite`

=== "Syntax"

    ```bash
    --rpc-http-tls-cipher-suite=<cipherSuiteName>[, <cipherSuiteName>...]
    ```

=== "Example"

    ```bash
    --rpc-http-tls-cipher-suite=TLS_AES_256_GCM_SHA384,TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384,TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256
    ```

=== "Environment variable"

    ```bash
    BESU_RPC_HTTP_TLS_CIPHER_SUITE=TLS_AES_256_GCM_SHA384,TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384,TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256
    ```

=== "Configuration file"

    ```bash
    rpc-http-tls-cipher-suite=["TLS_AES_256_GCM_SHA384","TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384","TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256"]
    ```

A list of comma-separated TLS cipher suites to support.

!!!tip

    The singular `--rpc-http-tls-cipher-suite` and plural `--rpc-http-tls-cipher-suites` are available and are two names for
    the same option.

### `rpc-http-tls-enabled`

=== "Syntax"

    ```bash
    --rpc-http-tls-enabled[=<true|false>]
    ```

=== "Example"

    ```bash
    --rpc-http-tls-enabled=true
    ```

=== "Environment variable"

    ```bash
    BESU_RPC_HTTP_TLS_ENABLED=true
    ```

=== "Configuration file"

    ```bash
    rpc-http-tls-enabled=true
    ```

Enables or disables TLS for the JSON-RPC HTTP service. The default is `false`.

!!! note

    [`--rpc-http-enabled`](#rpc-http-enabled) must be enabled.

### `rpc-http-tls-keystore-file`

=== "Syntax"

    ```bash
    --rpc-http-tls-keystore-file=<FILE>
    ```

=== "Example"

    ```bash
    --rpc-http-tls-keystore-file=/home/me/me_node/keystore.pfx
    ```

=== "Environment variable"

    ```bash
    BESU_RPC_HTTP_TLS_KEYSTORE_FILE=/home/me/me_node/keystore.pfx
    ```

=== "Configuration file"

    ```bash
    rpc-http-tls-keystore-file="/home/me/me_node/keystore.pfx"
    ```

The Keystore file (in PKCS #12 format) that contains private key and the certificate presented to
the client during authentication.

### `rpc-http-tls-keystore-password-file`

=== "Syntax"

    ```bash
    --rpc-http-tls-keystore-password-file=<FILE>
    ```

=== "Example"

    ```bash
    --rpc-http-tls-keystore-password-file=/home/me/me_node/password
    ```

=== "Environment variable"

    ```bash
    BESU_RPC_HTTP_TLS_KEYSTORE_PASSWORD_FILE=/home/me/me_node/password
    ```

=== "Configuration file"

    ```bash
    rpc-http-tls-keystore-password-file="/home/me/me_node/password"
    ```

The path to the file containing the password to decrypt the keystore.

### `rpc-http-tls-known-clients-file`

=== "Syntax"

    ```bash
    --rpc-http-tls-known-clients-file=<FILE>
    ```

=== "Example"

    ```bash
    --rpc-http-tls-known-clients-file=/home/me/me_node/knownClients
    ```

=== "Environment variable"

    ```bash
    BESU_RPC_HTTP_TLS_KNOWN_CLIENTS_FILE=/home/me/me_node/knownClients
    ```

=== "Configuration file"

    ```bash
    rpc-http-tls-known-clients-file="/home/me/me_node/knownClients"
    ```

The path to the file used to
[authenticate clients](../../../private-networks/how-to/configure/tls/client-and-server.md#create-the-known-clients-file) using
self-signed certificates or non-public certificates.

Must contain the certificate's Common Name, and SHA-256 fingerprint in the format
`<CommonName> <hex-string>`.

!!! note

    You must enable client authentication using the
    [`---rpc-http-tls-client-auth-enabled`](#rpc-http-tls-client-auth-enabled) option.

### `rpc-http-tls-protocol`

=== "Syntax"

    ```bash
    --rpc-http-tls-protocol=<protocolName>[, <protocolName>...]
    ```

=== "Example"

    ```bash
    --rpc-http-tls-protocol=TLSv1.3,TLSv1.2
    ```

=== "Environment variable"

    ```bash
    BESU_RPC_HTTP_TLS_PROTOCOL=TLSv1.3,TLSv1.2
    ```

=== "Configuration file"

    ```bash
    rpc-http-tls-protocol=["TLSv1.3","TLSv1.2"]
    ```

A list of comma-separated TLS protocols to support. The default is `DEFAULT_TLS_PROTOCOLS`, a list which includes `TLSv1.3` and `TLSv1.2`.

!!!tip

    The singular `--rpc-http-tls-protocol` and plural `--rpc-http-tls-protocols` are available and are two names for
    the same option.

### `rpc-tx-feecap`

=== "Syntax"

    ```bash
    --rpc-tx-feecap=<MAX_FEE>
    ```

=== "Example"

    ```bash
    --rpc-tx-feecap=1200000000000000000
    ```

=== "Environment variable"

    ```bash
    BESU_RPC_TX_FEECAP=1200000000000000000
    ```

=== "Configuration file"

    ```bash
    rpc-tx-feecap=1200000000000000000
    ```

The maximum transaction fee (in Wei) accepted for transactions submitted through the
[`eth_sendRawTransaction`](../api/index.md#eth_sendrawtransaction) RPC. The default is 1000000000000000000 (1 ether).

If set to 0, then this option is ignored and no cap is applied.

### `rpc-ws-api`

=== "Syntax"

    ```bash
    --rpc-ws-api=<api name>[,<api name>...]...
    ```

=== "Example"

    ```bash
    --rpc-ws-api=ETH,NET,WEB3
    ```

=== "Environment variable"

    ```bash
    BESU_RPC_WS_API=ETH,NET,WEB3
    ```

=== "Configuration file"

    ```bash
    rpc-ws-api=["ETH","NET","WEB3"]
    ```

A comma-separated list of APIs to enable on the WebSockets channel. When you use this option
you must also specify the `--rpc-ws-enabled` option. The available API options are: `ADMIN`,
`CLIQUE`, `DEBUG`, `EEA`, `ETH`, `IBFT`, `MINER`, `NET`, `PERM`, `PLUGINS`, `PRIV`, `QBFT`, `TRACE`,
`TXPOOL`, and `WEB3`. The default is: `ETH`, `NET`, `WEB3`.

!!!tip

    The singular `--rpc-ws-api` and plural `--rpc-ws-apis` options are available and are two names
    for the same option.

### `rpc-ws-authentication-credentials-file`

=== "Syntax"

    ```bash
    --rpc-ws-authentication-credentials-file=<FILE>
    ```

=== "Example"

    ```bash
    --rpc-ws-authentication-credentials-file=/home/me/me_node/auth.toml
    ```

=== "Environment variable"

    ```bash
    BESU_RPC_WS_AUTHENTICATION_CREDENTIALS_FILE=/home/me/me_node/auth.toml
    ```

=== "Configuration file"

    ```bash
    rpc-ws-authentication-credentials-file="/home/me/me_node/auth.toml"
    ```

The path to the [credentials file](../../how-to/use-besu-api/authenticate.md#credentials-file)
for JSON-RPC API [authentication](../../how-to/use-besu-api/authenticate.md).

### `rpc-ws-authentication-enabled`

=== "Syntax"

    ```bash
    --rpc-ws-authentication-enabled[=<true|false>]
    ```

=== "Example"

    ```bash
    --rpc-ws-authentication-enabled=true
    ```

=== "Environment variable"

    ```bash
    BESU_RPC_WS_AUTHENTICATION_ENABLED=true
    ```

=== "Configuration file"

    ```bash
    rpc-ws-authentication-enabled=true
    ```

Enables or disables [authentication](../../how-to/use-besu-api/authenticate.md) for the WebSocket JSON-RPC
service.

!!! note

    `wscat` doesn't support headers. [Authentication](../../how-to/use-besu-api/authenticate.md)
    requires you to pass an authentication token in the request header. To use authentication with
    WebSockets, you need an app that supports headers.

### `rpc-ws-authentication-jwt-public-key-file`

=== "Syntax"

    ```bash
    --rpc-http-authentication-jwt-public-key-file=<FILE>
    ```

=== "Example"

    ```bash
    --rpc-http-authentication-jwt-public-key-file=publicKey.pem
    ```

=== "Environment variable"

    ```bash
    BESU_RPC_HTTP_AUTHENTICATION-JWT-PUBLIC-KEY-FILE="publicKey.pem"
    ```

=== "Configuration file"

    ```bash
    rpc-http-authentication-jwt-public-key-file="publicKey.pem"
    ```

The [JWT provider's public key file] used for JSON-RPC WebSocket authentication with an external
JWT.

### `rpc-ws-enabled`

=== "Syntax"

    ```bash
    --rpc-ws-enabled[=<true|false>]
    ```

=== "Example"

    ```bash
    --rpc-ws-enabled=true
    ```

=== "Environment variable"

    ```bash
    BESU_RPC_WS_ENABLED=true
    ```

=== "Configuration file"

    ```bash
    rpc-ws-enabled=true
    ```

Enables or disables the WebSocket JSON-RPC service. The default is `false`.

### `rpc-ws-host`

=== "Syntax"

    ```bash
    --rpc-ws-host=<HOST>
    ```

=== "Example"

    ```bash
    # to listen on all interfaces
    --rpc-ws-host=0.0.0.0
    ```

=== "Environment variable"

    ```bash
    BESU_RPC_WS_HOST=0.0.0.0
    ```

=== "Configuration file"

    ```bash
    rpc-ws-host="0.0.0.0"
    ```

The host on which WebSocket JSON-RPC listens. The default is `127.0.0.1`.

To allow remote connections, set to `0.0.0.0`

### `rpc-ws-max-active-connections`

=== "Syntax"

    ```bash
    --rpc-ws-max-active-connections=<INTEGER>
    ```

=== "Example"

    ```bash
    --rpc-ws-max-active-connections=100
    ```

=== "Environment variable"

    ```bash
    BESU_RPC_WS_MAX_ACTIVE_CONNECTIONS=100
    ```

=== "Configuration file"

    ```toml
    rpc-ws-max-active-connections=100
    ```

The maximum number of WebSocket connections allowed for JSON-RPC. Once this limit is reached, incoming connections are rejected. The default is 80.

### `rpc-ws-max-frame-size`

=== "Syntax"

    ```bash
    --rpc-ws-max-frame-size=<INTEGER>
    ```

=== "Example"

    ```bash
    --rpc-ws-max-frame-size=65536
    ```

=== "Environment variable"

    ```bash
    BESU_RPC_WS_MAX_FRAME_SIZE=65536
    ```

=== "Configuration file"

    ```toml
    rpc-ws-max-frame-size=65536
    ```

The maximum size in bytes for JSON-RPC WebSocket frames. If this limit is exceeded, the WebSocket disconnects. The default is 1048576 (or 1 MB).

### `rpc-ws-port`

=== "Syntax"

    ```bash
    --rpc-ws-port=<PORT>
    ```

=== "Example"

    ```bash
    # to listen on port 6174
    --rpc-ws-port=6174
    ```

=== "Environment variable"

    ```bash
    BESU_RPC_WS_PORT=6174
    ```

=== "Configuration file"

    ```bash
    rpc-ws-port="6174"
    ```

The port (TCP) on which WebSocket JSON-RPC listens. The default is `8546`. You must
[expose ports appropriately](../../how-to/connect/configure-ports.md).

### `security-module`

=== "Syntax"

    ```bash
    --security-module=<NAME>
    ```

=== "Example"

    ```bash
    --security-module=security_module
    ```

=== "Environment variable"

    ```bash
    BESU_SECURITY_MODULE=security_module
    ```

=== "Configuration file"

    ```bash
    security-module="security_module"
    ```

Name of the security module plugin to use. For example, a Hardware Security Module (HSM) or V3 filestore
plugin

The default is the node's local private key file specified using
[`--node-private-key-file`](#node-private-key-file).

### `static-nodes-file`

=== "Syntax"

    ```bash
    --static-nodes-file=<FILE>
    ```

=== "Example"

    ```bash
    --static-nodes-file=~/besudata/static-nodes.json
    ```

=== "Environment variable"

    ```bash
    BESU_STATIC_NODES_FILE=~/besudata/static-nodes.json
    ```

=== "Configuration file"

    ```bash
    static-nodes-file="~/besudata/static-nodes.json"
    ```

Static nodes JSON file containing the [static nodes](../../how-to/connect/static-nodes.md) for this node to
connect to. The default is `datapath/static-nodes.json`.

### `strict-tx-replay-protection-enabled`

=== "Syntax"

    ```bash
    --strict-tx-replay-protection-enabled[=<true|false>]
    ```

=== "Example"

    ```bash
    --strict-tx-replay-protection-enabled=false
    ```

=== "Environment variable"

    ```bash
    STRICT_TX_REPLAY_PROTECTION_ENABLED=false
    ```

=== "Configuration file"

    ```bash
    strict-tx-replay-protection-enabled=false
    ```

Enables or disables replay protection, in accordance with [EIP-155](https://eips.ethereum.org/EIPS/eip-155), on
transactions submitted using JSON-RPC.
The default is `false`.

### `sync-mode`

=== "Syntax"

    ```bash
    --sync-mode=X_SNAP
    ```

=== "Example"

    ```bash
    --sync-mode=X_SNAP
    ```

=== "Environment variable"

    ```bash
    BESU_SYNC_MODE=X_SNAP
    ```

=== "Configuration file"

    ```bash
    sync-mode="X_SNAP"
    ```

The synchronization mode.
Use `FAST` for [fast sync](../../how-to/connect/sync-node.md#fast-synchronization), `FULL` for
[full sync](../../how-to/connect/sync-node.md#run-an-archive-node), `X_SNAP` for
[snap sync](../../how-to/connect/sync-node.md#snap-synchronization), and `X_CHECKPOINT` for
[checkpoint sync](../../how-to/connect/sync-node.md#checkpoint-synchronization).

* The default is `FULL` when connecting to a private network by not using the [`--network`](#network)
  option and specifying the [`--genesis-file`](#genesis-file) option.
* The default is `FAST` when using the [`--network`](#network) option with named networks, except for the `dev`
  development network.
  `FAST` is also the default if running Besu on the default network (Ethereum Mainnet) by specifying neither
  [network](#network) nor [genesis file](#genesis-file).

!!! important

    Snap sync and checkpoint sync are experimental features.

    We recommend using snap sync over fast sync even in certain production environments (for example, staking), because
    snap sync can be faster by several days.
    If your snap sync completes successfully, you have the correct world state.

### `target-gas-limit`

=== "Syntax"

    ```bash
    --target-gas-limit=<INTEGER>
    ```

=== "Example"

    ```bash
    --target-gas-limit=8000000
    ```

=== "Environment variable"

    ```bash
    BESU_TARGET_GAS_LIMIT=8000000
    ```

=== "Configuration file"

    ```bash
    target-gas-limit="8000000"
    ```

The gas limit toward which Besu will gradually move on an existing network, if enough miners are in
agreement. To change the block gas limit set in the genesis file without creating a new network,
use `target-gas-limit`. The gas limit between blocks can change only 1/1024th, so the target tells
the block creator how to set the gas limit in its block. If the values are the same or within
1/1024th, Besu sets the limit to the specified value. Otherwise, the limit moves as far as it can
within that constraint.

If a value for `target-gas-limit` is not specified, the block gas limit remains at the value
specified in the [genesis file](../genesis-items.md#genesis-block-parameters).

Use the [`miner_changeTargetGasLimit`](../api/index.md#miner_changetargetgaslimit) API to update
the `target-gas-limit` while Besu is running. Alternatively restart Besu with an updated
`target-gas-limit` value.

### `tx-pool-max-size`

=== "Syntax"

    ```bash
    --tx-pool-max-size=<INTEGER>
    ```

=== "Example"

    ```bash
    --tx-pool-max-size=2000
    ```

=== "Environment variable"

    ```bash
    BESU_TX_POOL_MAX_SIZE=2000
    ```

=== "Configuration file"

    ```bash
    tx-pool-max-size="2000"
    ```

The maximum number of transactions kept in the transaction pool. The default is 4096.

### `tx-pool-hashes-max-size`

=== "Syntax"

    ```bash
    --tx-pool-hashes-max-size=<INTEGER>
    ```

=== "Example"

    ```bash
    --tx-pool-hashes-max-size=2000
    ```

=== "Environment variable"

    ```bash
    BESU_TX_POOL_HASHES_MAX_SIZE=2000
    ```

=== "Configuration file"

    ```bash
    tx-pool-hashes-max-size="2000"
    ```

!!! important

    `tx-pool-hashes-max-size` is deprecated. The option will be removed in a future release.

The maximum number of transaction hashes kept in the transaction pool. The default is 4096.

### `tx-pool-price-bump`

=== "Syntax"

    ```bash
    --tx-pool-price-bump=<INTEGER>
    ```

=== "Example"

    ```bash
    --tx-pool-price-bump=25
    ```

=== "Environment variable"

    ```bash
    BESU_TX_POOL_PRICE_BUMP=25
    ```

=== "Configuration file"

    ```bash
    tx-pool-price-bump=25
    ```

The price bump percentage to replace an existing transaction. The default is 10.

### `tx-pool-retention-hours`

=== "Syntax"

    ```bash
    --tx-pool-retention-hours=<INTEGER>
    ```

=== "Example"

    ```bash
    --tx-pool-retention-hours=5
    ```

=== "Environment variable"

    ```bash
    BESU_TX_POOL_RETENTION_HOURS=5
    ```

=== "Configuration file"

    ```bash
    tx-pool-retention-hours=5
    ```

The maximum period, in hours, to hold pending transactions in the transaction pool. The default is
13.

### `Xhelp`

=== "Syntax"

    ```bash
    -X, --Xhelp
    ```

Displays the experimental options and their descriptions, and exit.

!!! warning

    The displayed options are unstable and may change between releases.

### `version`

=== "Syntax"

    ```bash
    -V, --version
    ```

Prints version information and exit.

<!-- Links -->
[push gateway integration]: ../../how-to/monitor/metrics.md#running-prometheus-with-besu-in-push-mode
[JWT provider's public key file]: ../../how-to/use-besu-api/authenticate.md#jwt-public-key-authentication
