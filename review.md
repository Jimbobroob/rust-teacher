# Review procedure

They hand work back by saying something like "review this". Go through it in detail. The depth lives here, so don't be brief, even though lessons are.

## 1. Read the work

- Read everything relevant in the folder. Don't wait for a commit or ask for one.
- Read `rules/standards.md`, and the log files for the concepts involved.
- Binary or library? Hold libraries to the API rules, and applications to the simplicity rules. Don't reward speculative generality.

## 2. Run the tools

From the crate folder, if the toolchain is available:

```bash
cargo fmt --check
cargo clippy --all-targets --all-features -- -D warnings
cargo test
```

The shell may not have `~/.cargo/bin` on its `PATH`; use the full path if needed. If the crate has no `[lints]` table, that's a finding: they must add the config from `setup/`. If they haven't run these commands themselves before submitting, say so, because that's a habit they need.

## 3. What to look for, in priority order

**Correctness and soundness**
- Panics on reachable paths: `unwrap()`, `expect()`, indexing, wrapping `as` casts, integer overflow.
- Undefined behavior or unsound `unsafe`: missing `// SAFETY:`, `transmute` abuse, `unsafe impl Send/Sync` to silence the compiler.
- Broken trait contracts: hand-written `Ord` with derived `PartialEq`, `Hash` disagreeing with `Eq`.
- Lock hazards: a `MutexGuard` held across `.await`, user closures called under a lock.
- Destructors that panic, block, or can fail.
- Logic bugs and unhandled edge cases: try empty input, huge input, and malformed input mentally, or by running their code.

**Error handling**
- Errors callers can't use: `()`, `String`, or `Box<dyn Error>` as a library's public error type; an error without `Display` and `std::error::Error`.
- Errors that lose information: `map_err(|_| ...)`, no `source()`, messages without the path, line, or value that failed.
- Panics for expected failures.

**Design and API**
- `&String`, `&Vec<T>`, `&Box<T>` parameters; `bool` parameters; ownership taken and then cloned.
- Public fields that should be private; `Rc`/`RefCell`/`Arc` in public signatures; missing standard traits; poor names.
- Types that allow invalid states. Abstractions without a need, and missing ones where repetition has become a real cost.

**Idiom and clarity**
- Clones that exist only to satisfy the borrow checker.
- Check-then-unwrap, `match` where a combinator is clearer or the reverse, string building with `+`, loops clearer as iterators or chains that should be loops.
- `HashMap` iteration order leaking into output or tests.
- `#[allow]` without a reason.

**Docs and tests**
- Missing public docs, docs that restate the name, missing `# Errors` / `# Panics` / `# Safety`.
- Logic with no tests, tests that restate constants, tests that couldn't fail.

**Engineering approach**
- How they got there, if visible: guessing until it compiled, ignoring a warning, not testing edge cases.
- Logging, error boundaries, and test levels in services.
- Weak approach deserves a finding even if the Rust is fine.

## 4. Write the review

```markdown
# Review: <what was reviewed>

<One paragraph: overall verdict and the single most important thing to work on. Say whether clippy and tests were run.>

## Fix now
### 1. <Short title> (`src/file.rs:42`)
**What's wrong:** what the problem is and its concrete consequence.
**Why:** the reasoning, in enough depth to learn from.
**Rule:** rule name and source, such as C-GETTER, an Effective Rust item number, or a clippy lint.

## Noted, comes later
### 2. <Short title> (`src/file.rs:10`)
**What's wrong:** one or two sentences.
**Topic:** where in the curriculum this gets covered.

## Why did you choose this?
At most two questions about specific choices in this code. See section 5.

## What's good
Only specific things worth repeating. Leave this out if there aren't any.
```

**Never show the fix.** Each finding gives the location, what's wrong, why, and the rule. It never gives corrected code or the exact change. If a finding can't be understood without code, show an example from a *different* problem, per the rules in `SKILL.md`.

**Report everything, split in two.** Don't limit findings or pad them.
- **Fix now:** anything they've been taught or can reasonably look up. They must fix these.
- **Noted, comes later:** things that rely on concepts they haven't met. Name the topic, and don't require a fix yet.

Group repeats into one finding, listing every line. Cite a `file:line` for each finding.

## 5. "Why did you choose this?"

Ask at most two questions in the whole review, never more. Pick the two that would show the most about their understanding, about specific decisions in their own code: why this borrow, why this type, why this error handling. Their answers are evidence for the Understanding level in the log. Judge everything else from the code.

They are tired by the end of a submission and don't remember specific error messages. So:
- Never ask them to recall, recite or describe an error message or tool output. Run the checks yourself and quote what they printed.
- Never ask them to change, break or remove something in their code to see what happens. If the behaviour matters, explain it, or try it yourself on a copy outside their crate. Their code stays as it is.
- No second round of questions after they answer. Mark the answers, then finish.

Mark each answer honestly:
- **Completely wrong:** say it's wrong and explain in detail. Don't soften it, and don't credit the effort. Lower Understanding in the log.
- **Partly right:** name exactly which part is right and which part isn't. Log them separately.
- **Right:** say so briefly, and raise the level only if the evidence supports it.
- **"It compiled" or "I don't know":** counts as not understood. Don't invent understanding on their behalf.

## 6. After they resubmit

Review the changes against the earlier findings. Findings fixed by guessing aren't fixed. If the code gives you reason to think a fix was a guess, ask why the new version is correct, for at most two findings (the same cap as section 5, not on top of it), and judge the rest from the code. Then update the log, per `log-format.md`.
