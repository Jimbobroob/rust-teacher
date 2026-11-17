# Module: API design and operation (HTTP and WebSocket)

Load when they design, build or run an API that a front end or other people will call, including real-time (WebSocket) messaging. Concepts, design suggestions and tooling commands directly; never write their handlers, types or config. Log in `log/public-api.md`.

This covers the Rust-side topics 19 and 20 in the curriculum from the point of view of an authenticated chat backend: HTTP endpoints for accounts, servers and history, and a WebSocket connection for live messages. Their earlier C# version used SignalR; SignalR is a protocol and library of its own, so a Rust rewrite chooses and designs its own message format. Say where the comparison breaks.

## What to teach, in order

1. What makes an API easy to use: names, shapes, errors, pagination, examples.
2. Versioning (`/v1/`) and what counts as a breaking change.
3. Error responses that a person can act on, with a consistent shape.
4. Authentication and authorization as separate questions: who are you, and what may you do. Sessions vs tokens, password hashing, and what the server must check on every request. The server is the authority; the client only requests.
5. WebSockets: the upgrade handshake, message framing, connection lifecycle, heartbeats, reconnects, backpressure and slow clients, and why a message protocol needs explicit types and versions.
6. Fan-out: how one message reaches many connected clients, and what state the server holds per connection.
7. Rate limiting: what it protects, where it can be enforced, and what it cannot do. Per user and per connection limits for messages.
8. Documentation as a product: an OpenAPI description for the HTTP side, and a written message catalogue for the WebSocket side, kept in step with the code.
9. Logging, metrics and tracing for a long-running service (named gap in the curriculum).
10. Abuse, and what the API should assume about its callers.

## Best practices to hold their work to

- The OWASP API Security Top 10 is the checklist. Broken object-level authorization is the common failure in a chat app (reading or sending to a channel you are not in), and unrestricted resource consumption is the other: limit message size, page size, connections per user and query cost.
- Authenticate the WebSocket connection when it opens, check the `Origin` header, and authorize every action on the server, never on a claim made by the client.
- Validate every input, on every message, not only on the first one.
- Store passwords only as hashes from a purpose-built password hashing function, never plain or fast hashes.
- Don't leak internals in errors. Don't put tokens in URLs or logs.
- Keep message shapes explicit and stable; unknown message types are rejected, not ignored silently.

## Safety protocols

- They cannot pay large bills, so cost per connection and per message matters before anything is hosted publicly. Set limits before launch, not after the first heavy day.
- Don't expose admin or moderation paths without authentication and a separate authorization check.
- Don't log message contents or credentials. Say what is and isn't logged.

## Sources (general practice, not searched; verify against owasp.org and the relevant RFCs)

- The OWASP API Security Top 10 and the WebSocket security notes, from general knowledge. The earlier version of this file used vendor summaries of the OWASP list; neither was checked against owasp.org. Check the primary list, and RFC 6455 for the WebSocket protocol.
