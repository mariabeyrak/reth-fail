# reth stage unwind

Unwinds a certain block range, deleting it from the database

```bash
$ reth stage unwind --help
Usage: reth stage unwind [OPTIONS] <COMMAND>

Commands:
  to-block    Unwinds the database from the latest block, until the given block number or hash has been reached, that block is not included
  num-blocks  Unwinds the database from the latest block, until the given number of blocks have been reached
  help        Print this message or the help of the given subcommand(s)

Options:
      --datadir <DATA_DIR>
          The path to the data dir for all reth files and subdirectories.
          
          Defaults to the OS-specific data directory:
          
          - Linux: `$XDG_DATA_HOME/reth/` or `$HOME/.local/share/reth/`
          - Windows: `{FOLDERID_RoamingAppData}/reth/`
          - macOS: `$HOME/Library/Application Support/reth/`
          
          [default: default]

      --chain <CHAIN_OR_PATH>
          The chain this node is running.
          Possible values are either a built-in chain or the path to a chain specification file.
          
          Built-in chains:
              mainnet, sepolia, goerli, holesky, dev
          
          [default: mainnet]

      --instance <INSTANCE>
          Add a new instance of a node.
          
          Configures the ports of the node to avoid conflicts with the defaults. This is useful for running multiple nodes on the same machine.
          
          Max number of instances is 200. It is chosen in a way so that it's not possible to have port numbers that conflict with each other.
          
          Changes to the following port numbers: - DISCOVERY_PORT: default + `instance` - 1 - AUTH_PORT: default + `instance` * 100 - 100 - HTTP_RPC_PORT: default - `instance` + 1 - WS_RPC_PORT: default + `instance` * 2 - 2
          
          [default: 1]

  -h, --help
          Print help (see a summary with '-h')

Database:
      --db.log-level <LOG_LEVEL>
          Database logging level. Levels higher than "notice" require a debug build

          Possible values:
          - fatal:   Enables logging for critical conditions, i.e. assertion failures
          - error:   Enables logging for error conditions
          - warn:    Enables logging for warning conditions
          - notice:  Enables logging for normal but significant condition
          - verbose: Enables logging for verbose informational
          - debug:   Enables logging for debug-level messages
          - trace:   Enables logging for trace debug-level messages
          - extra:   Enables logging for extra debug-level messages

      --db.exclusive <EXCLUSIVE>
          Open environment in exclusive/monopolistic mode. Makes it possible to open a database on an NFS volume
          
          [possible values: true, false]

Networking:
  -d, --disable-discovery
          Disable the discovery service

      --disable-dns-discovery
          Disable the DNS discovery

      --disable-discv4-discovery
          Disable Discv4 discovery

      --enable-discv5-discovery
          Enable Discv5 discovery

      --discovery.addr <DISCOVERY_ADDR>
          The UDP address to use for devp2p peer discovery version 4
          
          [default: 0.0.0.0]

      --discovery.port <DISCOVERY_PORT>
          The UDP port to use for devp2p peer discovery version 4
          
          [default: 30303]

      --discovery.v5.addr <DISCOVERY_V5_ADDR>
          The UDP address to use for devp2p peer discovery version 5
          
          [default: 0.0.0.0]

      --discovery.v5.port <DISCOVERY_V5_PORT>
          The UDP port to use for devp2p peer discovery version 5
          
          [default: 9000]

      --discovery.v5.lookup-interval <DISCOVERY_V5_LOOKUP_INTERVAL>
          The interval in seconds at which to carry out periodic lookup queries, for the whole run of the program
          
          [default: 60]

      --discovery.v5.bootstrap.lookup-interval <DISCOVERY_V5_bootstrap_lookup_interval>
          The interval in seconds at which to carry out boost lookup queries, for a fixed number of times, at bootstrap
          
          [default: 5]

      --discovery.v5.bootstrap.lookup-countdown <DISCOVERY_V5_bootstrap_lookup_countdown>
          The number of times to carry out boost lookup queries at bootstrap
          
          [default: 100]

      --trusted-peers <TRUSTED_PEERS>
          Comma separated enode URLs of trusted peers for P2P connections.
          
          --trusted-peers enode://abcd@192.168.0.1:30303

      --trusted-only
          Connect only to trusted peers

      --bootnodes <BOOTNODES>
          Comma separated enode URLs for P2P discovery bootstrap.
          
          Will fall back to a network-specific default if not specified.

      --peers-file <FILE>
          The path to the known peers file. Connected peers are dumped to this file on nodes
          shutdown, and read on startup. Cannot be used with `--no-persist-peers`.

      --identity <IDENTITY>
          Custom node identity
          
          [default: reth/<VERSION>-<SHA>/<ARCH>]

      --p2p-secret-key <PATH>
          Secret key to use for this node.
          
          This will also deterministically set the peer ID. If not specified, it will be set in the data dir for the chain being used.

      --no-persist-peers
          Do not persist peers.

      --nat <NAT>
          NAT resolution method (any|none|upnp|publicip|extip:\<IP\>)
          
          [default: any]

      --addr <ADDR>
          Network listening address
          
          [default: 0.0.0.0]

      --port <PORT>
          Network listening port
          
          [default: 30303]

      --max-outbound-peers <MAX_OUTBOUND_PEERS>
          Maximum number of outbound requests. default: 100

      --max-inbound-peers <MAX_INBOUND_PEERS>
          Maximum number of inbound requests. default: 30

      --pooled-tx-response-soft-limit <BYTES>
          Soft limit for the byte size of a `PooledTransactions` response on assembling a `GetPooledTransactions` request. Spec'd at 2 MiB.
          
          <https://github.com/ethereum/devp2p/blob/master/caps/eth.md#protocol-messages>.
          
          [default: 2097152]

      --pooled-tx-pack-soft-limit <BYTES>
          Default soft limit for the byte size of a `PooledTransactions` response on assembling a `GetPooledTransactions` request. This defaults to less than the [`SOFT_LIMIT_BYTE_SIZE_POOLED_TRANSACTIONS_RESPONSE`], at 2 MiB, used when assembling a `PooledTransactions` response. Default is 128 KiB
          
          [default: 131072]

Logging:
      --log.stdout.format <FORMAT>
          The format to use for logs written to stdout
          
          [default: terminal]

          Possible values:
          - json:     Represents JSON formatting for logs. This format outputs log records as JSON objects, making it suitable for structured logging
          - log-fmt:  Represents logfmt (key=value) formatting for logs. This format is concise and human-readable, typically used in command-line applications
          - terminal: Represents terminal-friendly formatting for logs

      --log.stdout.filter <FILTER>
          The filter to use for logs written to stdout
          
          [default: ]

      --log.file.format <FORMAT>
          The format to use for logs written to the log file
          
          [default: terminal]

          Possible values:
          - json:     Represents JSON formatting for logs. This format outputs log records as JSON objects, making it suitable for structured logging
          - log-fmt:  Represents logfmt (key=value) formatting for logs. This format is concise and human-readable, typically used in command-line applications
          - terminal: Represents terminal-friendly formatting for logs

      --log.file.filter <FILTER>
          The filter to use for logs written to the log file
          
          [default: debug]

      --log.file.directory <PATH>
          The path to put log files in
          
          [default: <CACHE_DIR>/logs]

      --log.file.max-size <SIZE>
          The maximum size (in MB) of one log file
          
          [default: 200]

      --log.file.max-files <COUNT>
          The maximum amount of log files that will be stored. If set to 0, background file logging is disabled
          
          [default: 5]

      --log.journald
          Write logs to journald

      --log.journald.filter <FILTER>
          The filter to use for logs written to journald
          
          [default: error]

      --color <COLOR>
          Sets whether or not the formatter emits ANSI terminal escape codes for colors and other text formatting
          
          [default: always]

          Possible values:
          - always: Colors on
          - auto:   Colors on
          - never:  Colors off

Display:
  -v, --verbosity...
          Set the minimum log level.
          
          -v      Errors
          -vv     Warnings
          -vvv    Info
          -vvvv   Debug
          -vvvvv  Traces (warning: very verbose!)

  -q, --quiet
          Silence all log output
```