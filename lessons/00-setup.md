# Lesson 0: Setup

Assume they're new to both Rust tooling and Neovim. **They run every command and make every edit themselves.** Give one step at a time: say what the command does and what each part means, have them run it and show or describe the output, then continue. This is setup, so give exact commands. The "never give the answer" rule covers Rust problems, not this lesson. Update `log/overview.md` as steps finish, so the lesson can resume in a later session.

Facts checked on 2026-11-05. Re-check any that may have changed, especially the package list in Part 2.

## Part 1: Prove `spotify_player` works

1. `which spotify_player` should show `~/.local/bin/spotify_player`.
2. `spotify_player --version`
3. Teach: Rust compiles into a standalone program, so an installed program doesn't need Rust to run. This one only needs system libraries such as ALSA, D-Bus, and OpenSSL, which aren't being removed. `ldd ~/.local/bin/spotify_player` shows them.

## Part 2: Remove the old Rust

The installed `spotify_player` and `spotify_player.bak` stay untouched.

1. `sudo dnf remove rust cargo rust-std-static`
   - dnf lists what it will remove and asks to confirm. Have them read the list before answering. On 2026-11-05 it was exactly those three packages. **If anything else appears, such as `gcc` or anything with `-devel`, answer `n` and work out why,** because later steps need those.
2. `rm -rf ~/.cargo` removes 317 MB of cached downloads. Explain what `rm -rf` does and why it needs care. Have them check the path before pressing Enter.
3. `rm -rf ~/source/repos/spotify-player/target` removes 2.7 GB of build output.
   - The installed binary in `~/.local/bin` is a separate copy of the file built there, so deleting `target` doesn't touch it. **Before deleting,** verify with `ls -la ~/.local/bin/spotify_player`, which should show a regular file rather than a symlink (`->`) pointing into `target`.
4. Run `spotify_player --version` again to prove it still works, and `cargo --version` to prove Rust is gone.
5. Leave the `~/.cargo/bin` line in fish's config, because `rustup` uses that folder.

Neovim has no Rust config to remove: no Rust server or parser is enabled. Plugin files that mention Rust are part of those plugins and stay. Note that `blink.cmp` has its own prebuilt binary and doesn't need Rust installed.

## Part 3: Install through rustup

Teach: `rustup` manages Rust versions. It's used over Fedora's packages because the deep material needs nightly tools like Miri.

1. `sudo dnf install rustup`
2. `rustup-init`, and choose the default option when asked. Explain that it installs to `~/.cargo` and `~/.rustup`. The default profile includes `clippy` and `rustfmt`.
3. Open a new terminal, because `PATH` only updates for new shells. Then run `rustc --version` and `cargo --version`, and `which cargo` should point into `~/.cargo/bin`.
4. `rustup component add rust-analyzer rust-src`. Explain that `rust-analyzer` is the language server that gives the editor its smarts, and that `rust-src` lets it read the standard library.

## Part 4: Neovim for Rust

They edit `~/.config/nvim/init.lua` themselves, in Neovim. This is motion practice, so teach the keys as they go: open the file, search with `/`, move to the spot, enter insert mode, save with `Space w`.

1. Find the `local parsers = { ... }` line and add `"rust"`. Teach: Treesitter parses code for highlighting.
2. Find `vim.lsp.enable({ "clangd", "csharp_ls" })` and add `"rust_analyzer"`. Teach: this turns on the language server.
3. Quit and reopen Neovim. Check for parser install errors.
4. Don't test yet; Part 5 creates a real project.

## Part 5: First crate, with the lints

1. `cd ~/source/repos/rust-learning` and `cargo new setup-check`, then `cd setup-check`. Teach what it generated, and that it doesn't create its own git repo here because the folder is already inside one.
2. Open `src/main.rs` in Neovim. Check:
   - `K` on `println` shows docs
   - introduce a typo and an error appears inline; fix it
   - `Space f` formats the file
   - `:checkhealth vim.lsp` shows `rust_analyzer` attached
3. Add the lint config: paste `setup/lints-app.toml` into `Cargo.toml`. Teach what `[lints]` does.
4. Run `cargo run`, `cargo fmt --check`, `cargo clippy --all-targets --all-features -- -D warnings`, and `cargo test`. Teach what each does. This is the pre-submission routine for all work from now on.

The folder is theirs to keep or delete.

## Part 6: Prove `spotify-player` still builds

1. `cd ~/source/repos/spotify-player`
2. `cargo build --release`. It uses the default features, the same set as the installed binary, and it will take a while because it's a full rebuild. The system libraries it needs are still installed.
3. `./target/release/spotify_player --version`
4. This doesn't change the installed copy in `~/.local/bin`. They only replace that if they choose to.

## Part 7: Finish up

The teacher, not the learner, does these:

1. Update `~/source/docs/neovim-keybinds.md`: the "Code (C++ and C#)" heading should now include Rust, and `Space ch` stays C++ only. No keys changed. This is an `.md`, so it needs no go-ahead.
2. Mark setup complete in `log/overview.md`, and start `log/neovim.md` with what they did.
3. Tell them the Rust aesthetics layer is ready to be added, then see `lessons/00-aesthetics-plan.md`. Normal Claude or the teacher can build it, with their go-ahead.
4. Offer two or three options for what to do next from `curriculum.md`.
