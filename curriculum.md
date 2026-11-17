# Curriculum map

This is a map, not a track. They choose where to go, and the default is to learn topics as a project needs them (see "Just in time" in `SKILL.md`). Use it to suggest the next two or three options, to predict what a project will need, to say what's missing when they jump ahead, and to name the topic for "noted, comes later" review findings. Order within a part is a suggested path, based on what each topic relies on.

## Side tracks

Not numbered, so nothing above renumbers. Teach these whenever they want, alongside whatever else is going on.

- **SQL and Postgres:** `psql`, schema design, joins, aggregates, window functions, indexes, transactions, reading query plans. Independent of Rust. It's a named work gap, and it feeds topic 21 (`sqlx`). Module: `modules/sql-postgres.md`. Log in `log/sql.md`.
- **Containers (Docker):** `modules/containers-docker.md`. Log in `log/containers.md`.
- **TypeScript and React:** `modules/typescript-react.md`. Log in `log/typescript-react.md`.
- **API design and operation, HTTP and WebSocket:** `modules/public-api-design.md`. Log in `log/public-api.md`. Overlaps topics 19, 20, 23 and 24.
- **Neovim:** tracked in `log/neovim.md`.

The module for SQL and Postgres is `modules/sql-postgres.md`.

Each topic ends when the log shows the evidence, not when an exercise is finished. Concepts matter most, so go as deep on the why as they'll take. Build older concepts into every new exercise.

Main reference material: The Rust Book, Rust by Example, std docs, the Rustonomicon, Rust Atomics and Locks, Rust for Rustaceans, Asynchronous Programming in Rust, the Tokio tutorial, and `rules/references/`. Prefer sending them to the primary source.

## Part 0: Setup

- Follow `lessons/00-setup.md`: toolchain, Neovim for Rust, and the rebuild check.

## Part 1: Core

Mostly separate exercises at this stage.

1. **Cargo and the toolchain:** packages, crates, `Cargo.toml`, `Cargo.lock`, `build` vs `run` vs `check`, debug vs release, clippy, rustfmt, how to read a compiler error properly.
2. **Syntax basics:** bindings, immutability by default, shadowing, primitive types, integer overflow behavior, functions, expressions vs statements, control flow.
3. **Ownership:** stack and heap, moves, `Copy`, `Clone`, `Drop` and scope, and why this replaces a garbage collector.
4. **Borrowing:** shared and mutable references, the aliasing-xor-mutation rule and why it exists, slices, non-lexical lifetimes, reading borrow checker errors. Expect to spend a long time here.
5. **Structs, enums, and pattern matching:** methods, data-carrying enums, `match`, `if let`, `let else`, exhaustiveness, making invalid states unrepresentable.
6. **Error handling I:** `Option`, `Result`, `?`, panic vs error, custom error types, `Display`, `std::error::Error`, `From` conversions, why `unwrap` is a decision. This is a named gap, so go slowly.
7. **Collections and strings:** `Vec`, `HashMap`, `String` vs `&str`, UTF-8, why strings can't be indexed, iteration order.
8. **Modules and crates:** `mod`, `use`, visibility, file layout, adding dependencies, semver, library plus binary.
9. **Generics and traits:** bounds, `impl Trait`, associated types, derive, standard traits, coherence and the orphan rule, static vs dynamic dispatch.
10. **Testing I:** unit tests, integration tests in `tests/`, doc tests, what makes a test worth having, failing test first. This is a named gap.
11. **Lifetimes:** what annotations actually say, elision, structs holding references, `'static`, when to own instead.
12. **Closures and iterators:** `Fn`, `FnMut`, `FnOnce`, captures, adapters, laziness, writing an `Iterator` impl.

They can move to projects whenever they want. Suggest it around here.

## Part 2: Applied

Best learned through projects they care about.

13. **Smart pointers:** `Box`, `Rc`, `Arc`, `Weak`, `RefCell`, `Cell`, `Deref`, interior mutability, the cost of each.
14. **Building a CLI:** argument parsing, file IO, exit codes, `anyhow` vs `thiserror`, stdin/stdout discipline.
15. **Concurrency:** threads, `Send` and `Sync`, channels, `Mutex`, `RwLock`, atomics at a usage level, deadlocks, scoped threads.
16. **Async fundamentals:** what a `Future` is, the executor model, `tokio`, tasks vs threads, cancellation, blocking in async, holding locks across `.await`.
17. **Error handling II:** application vs library errors, context, boundaries, propagation strategy in a real system, what the caller can act on.
18. **Logging and observability:** the purpose of logs, structured events, `tracing`, spans, levels, what not to log, request correlation, metrics and traces as concepts. Named gap.
19. **Web APIs:** `axum`, routing, extractors, shared state, request validation, mapping domain errors to responses, serialization with `serde`, config, graceful shutdown, WebSockets.
20. **Middleware:** what it's for and what it isn't, `tower` `Service` and `Layer`, ordering, timeouts, auth, rate limits, writing their own. Named gap.
21. **Databases:** `sqlx`, connection pools, migrations, transactions, compile-time checked queries.
22. **Testing II:** driving a router in integration tests, test databases, fakes vs mocks, property testing with `proptest`, coverage as a signal rather than a goal. Named gap.
23. **Performance testing:** benchmarks with `criterion` or `divan`, `black_box`, load testing, reading a flamegraph, measuring before optimizing. Named gap.
24. **Security and shipping basics:** input handling, secrets, auth concepts, `cargo audit`, `cargo deny`, CI, release builds, containers.

## Part 3: Deep

The intricacies. Requires nightly tools for some topics, such as Miri.

25. **Memory layout:** size and alignment, `repr`, niche optimization, fat pointers, vtables, what monomorphization does to binaries.
26. **Advanced ownership and lifetimes:** variance, higher-ranked trait bounds, `PhantomData`, drop check, reborrowing.
27. **Advanced traits and types:** GATs, const generics, trait object safety, blanket impls, the type-state pattern, API design for libraries.
28. **Unsafe Rust:** what `unsafe` actually permits, aliasing rules, undefined behavior, raw pointers, `MaybeUninit`, writing a sound safe abstraction, Miri. Build a `Vec` or linked list from scratch.
29. **Async internals:** state machines, `Pin` and `Unpin`, `Waker`, writing a `Future` by hand, building a tiny executor.
30. **Concurrency internals:** memory ordering, atomics in depth, writing a lock or channel, lock-free tradeoffs, loom.
31. **Macros:** `macro_rules!`, hygiene, proc macros, `syn` and `quote`, when not to use a macro.
32. **FFI:** the C ABI, `repr(C)`, `bindgen`, ownership across the boundary, panics at the boundary.
33. **Performance engineering:** allocation, cache behavior, profiling tools, `unsafe`-free optimization first, SIMD basics.
34. **Reading real code:** read and critique real crates, such as `tokio`, `serde`, `axum`, and optionally their `spotify-player` fork. Optional and available at any point.

## Being honest about the ceiling

Finishing this map is not mastery. Real mastery takes years of building things outside a course, and they should hear that when it becomes relevant, without it being used as discouragement.
