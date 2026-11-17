# TeamCord: checklist

Tick items off as they are done and shown. The plan and reasons are in `ROADMAP.md`. Tasks say what to achieve, never how. Detailed progress and levels stay in `log/` in `rust-learning/`.

## Stage 0: foundations

- [ ] A Rust crate for the offline message model, created by you with `cargo new`
- [ ] Types for a user and a message that make invalid states hard to build
- [ ] Validation for message content, with the failure cases as errors rather than panics
- [ ] Tests for the valid and invalid cases
- [ ] The routine passes cleanly: `cargo fmt --check`, `cargo clippy --all-targets --all-features -- -D warnings`, `cargo test`
- [ ] A review

## Stage 1: messages, running locally

- [ ] A Cargo workspace in `src/` with a `server` crate and a `protocol` crate
- [ ] Message types in `protocol`, encoded and decoded as MessagePack, with a type and a version field
- [ ] A message catalogue written down (what each message means and when it is sent)
- [ ] Postgres schema and migrations for users, conversations and messages with UUIDv7 IDs
- [ ] An HTTP endpoint that returns history for a conversation
- [ ] A WebSocket endpoint that accepts connections and the dev-only user picker identity
- [ ] A message goes from one tab to the other live
- [ ] The server assigns the ID and timestamp and acknowledges the sender
- [ ] A retried message with the same client ID is not stored twice
- [ ] A recipient who is offline sees the message in history next time
- [ ] Unit tests for encoding and decoding, integration tests against Postgres in Docker
- [ ] `tracing` logs that show what happened without logging message contents
- [ ] The front end uses a WebSocket client in place of SignalR, with runtime checks on incoming messages
- [ ] The front end shows connecting, disconnected and failed states
- [ ] `docker compose up` starts the front end, the Rust server and Postgres
- [ ] The C# backend and its tests are deleted, with `docker-compose.yml` pointing at the Rust server

## Stage 2: real auth

- [ ] Users and sessions tables; invite codes that are single-use and expire
- [ ] Sign-up with an invite code, passwords hashed with Argon2id
- [ ] Login that issues a session, sent as an HttpOnly, Secure, SameSite cookie
- [ ] Middleware that authenticates HTTP requests
- [ ] A single-use WebSocket ticket from an authenticated endpoint, and an `Origin` check
- [ ] Login rate limiting and temporary lockout
- [ ] Logout, a list of your sessions, and revoking one
- [ ] Password change
- [ ] A log of auth events that holds no passwords or tokens
- [ ] An admin password reset from the command line
- [ ] CSRF protection on state-changing requests
- [ ] The dev-only picker is gone
- [ ] Tests for the auth failure cases

## Stage 3: across the web

- [ ] Decide how your friend reaches it (see the options in `ROADMAP.md`) and note what it costs
- [ ] HTTPS and secure cookies work in that setup
- [ ] Your friend sends you a message from their machine
- [ ] A teardown that leaves nothing billing, if anything was rented

## Stage 4: friends list

- [ ] Send, accept and decline a friend request by exact username
- [ ] List your friends
- [ ] Only friends can start a DM
- [ ] Tests for duplicate and crossed requests

## Stage 5: servers and channels

- [ ] Create a server; one server exists on a fresh install
- [ ] Create several servers as one person
- [ ] Invite people, and join
- [ ] Create channels in a server and send messages in them
- [ ] Every query and action is scoped to the server and checked for permission
- [ ] Tests for access across servers

## Later

- [ ] Presence and typing indicators
- [ ] Voice
- [ ] Setup wizard or installer
- [ ] Two-factor and passkeys
- [ ] Roles and permissions
- [ ] Edits, deletes and reactions; file uploads; search
