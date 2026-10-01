# Crowsi authority transport

`crowsi-authority-transport` carries bounded, signed, opaque metadata envelopes
between managed devices and an independently deployed authority over mutual TLS.
It deliberately does not understand identity, credential, provider, custody, or
account semantics.

The client completes TLS, verifies the exact configured server leaf certificate
digest, and only then sends a request. The server maps an exact client leaf
digest to one device and request-verification key. Both sides enforce finite
timeouts, frame limits, freshness, closed envelopes, and Ed25519 signatures.
Immediately after that exact mapping, the server calls its injected
`PeerStatusGuard`. The guard must validate current authority status before a
peer quota, request frame, replay record, or backend resource is touched.

Production listeners use `AuthorityServer::serve_until` with a configured
concurrent worker capacity and an explicit shutdown flag. The capacity limits
live handshake/request/backend workers; it is not a lifetime accept count. A
slow provider operation or invalid peer therefore fails only its connection.
`serve_batch` provides the same isolation for a finite accepted-connection
budget. The compatibility `serve_n` method must not be used as a long-lived
one-connection loop.

Multi-peer listeners require capacity of at least two. After mTLS identifies a
peer, only one request from that peer may be in flight; excess connections fail
before replay state or the backend is touched, reserving progress for another
peer. Peer request key IDs and public keys are constructor-enforced unique.

Production applications construct the server with `new_with_peer_status` and
provide a durable `PeerStatusGuard`, a durable atomic `ReplayGuard`, and an
`AuthorityBackend`. The status guard resolves its authority-issued token
locally on every connection; clients cannot supply status. Revocation therefore
takes effect without a gateway restart, while missing, stale, or tampered status
fails closed. In-memory guards are suitable only for tests. The deprecated
`new` constructor is a fail-closed migration cut set and always rejects its
configuration. The transport never stores authority state or secret material
in payloads.

This crate is a private shared library published as version `0.1.0` only to the
installer-managed `ecosystem-private` registry.
