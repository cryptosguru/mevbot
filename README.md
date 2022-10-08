# mev-bot

A service that allows Ethereum Consensus Layer (CL) clients to outsource block construction to third party block builders in addition to execution clients.

### Request sequence

```mermaid
sequenceDiagram
    participant consensus
    participant mev_boost
    participant relays
    Title: Block Proposal
    Note over consensus: validator starts up
    consensus->>mev_boost: registerValidator
    mev_boost->>relays: registerValidator
    Note over consensus: wait for allocated slot
    consensus->>mev_boost: getHeader
    mev_boost->>relays: getHeader
    relays-->>mev_boost: getHeader response
    Note over mev_boost: verify response matches expected
    Note over mev_boost: select best payload
    mev_boost-->>consensus: getHeader response
    Note over consensus: sign the header
    consensus->>mev_boost: getPayload
    Note over mev_boost: identify payload source
    mev_boost->>relays: getPayload
    Note over relays: validate signature
    relays-->>mev_boost: getPayload response
    Note over mev_boost: verify response matches expected
    mev_boost-->>consensus: getPayload response
```

## Build

```
make build
```

and then run it with:

```
./mev-bot
```

## Lint & Test

```
make test
make lint
make run-mergemock-integration
```

The path to the mergemock repo is assumed to be `../mergemock`, you can override like so:

```
make MERGEMOCK_DIR=/PATH-TO-MERGEMOCK-REPO run-mergemock-integration
```

to run mergemock in dev mode:

```
make MERGEMOCK_BIN='go run .' run-mergemock-integration
```

## Testing with test-cli

[test-cli readme](cmd/test-cli/README.md)
