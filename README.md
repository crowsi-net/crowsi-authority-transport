# crowsi-authority-transport

Exchange signed, bounded authority messages over an explicitly trusted mTLS connection.

## What you can do

- Validate peer and signed-message context.
- Carry device/authority metadata without exporting custody values.

## Current scope

Trust roots, certificates and endpoints are registered by the operator. Transport success does not authorize the payload operation.

Package distribution is not activated by this documentation. Use the checked-in source and the declared dependency versions; published availability must be verified separately.

## Getting started

Install Rust 1.97 or newer and make the declared dependencies available. Use the configured private registry when a dependency is not distributed publicly. Run from this repository:

```sh
cargo test --locked
```

## Documentation and source

[Usage guide](docs/getting-started.md)

[Detailed documentation](docs) · [Implementation and public interfaces](src) · [Verification cases](tests) · [Contributing](CONTRIBUTING.md) · [Security reporting](SECURITY.md) · [License](LICENSE) · [Attribution notices](NOTICE)
