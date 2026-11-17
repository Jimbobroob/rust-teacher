# Rust aesthetics layer for Neovim

They asked for a Neovim addition that covers appearance only: **no keybinds and no plugins.** Build it after Lesson 0, because `rust-analyzer` has to be running to check how it looks. Unlike their exercises, this is yours to write, since it isn't part of what they're learning. It still needs their explicit go-ahead, since it changes non-`.md` files. It isn't a separate installable plugin.

## Where the changes go

- **Colors:** `~/source/rices/blackwall-rice/tools/gen_apps.py`, in `NVIM_TEMPLATE`. **Never edit `generated/nvim/colors/blackwall.lua` directly,** because it's generated and gets overwritten. Regenerate with `tools/generate.py`, using colors from the existing palette. Don't invent new ones. The rice repo may be on a working branch with other changes, so leave those alone and don't commit unless asked.
- **Rust-only display settings:** a new file, `~/.config/nvim/after/ftplugin/rust.lua`, or an `LspAttach` check for `rust_analyzer` if a setting isn't buffer-local.

## What to add

- Distinct highlights for lifetimes (`@lsp.type.lifetime.rust`), macros, attributes, and mutable variables (`@lsp.mod.mutable.rust`, an underline suits this). Consider `@lsp.mod.unsafe.rust`, which makes `unsafe` operations stand out. Inspect with `:Inspect` on real code to find the real group names, rather than trusting this list.
- Styled inlay type hints, on by default in Rust buffers via `vim.lsp.inlay_hint.enable`. Keep them dim enough to fade into the background, using `LspInlayHint`.
- Polished diagnostic appearance in Rust buffers if the current look can be improved, without affecting C++ and C#.

Theme changes in `NVIM_TEMPLATE` should be scoped with `.rust` so other languages keep their look.

## Finish

Have them look at real code and judge it, and adjust until they like it. Since no keys change, `neovim-keybinds.md` shouldn't need edits.
