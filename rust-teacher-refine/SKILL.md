---
name: rust-teacher-refine
description: Change, tweak or fix the user's rust-teacher skill (its teaching rules, review procedure, standards, curriculum, log format, lessons, lint configs) while keeping every file that repeats a fact consistent. Only run when the user invokes it.
disable-model-invocation: true
---

The `rust-teacher` skill lives in `~/.claude/skills/rust-teacher/`. The user calls this skill to change it. Treat their request (the text after `/rust-teacher-refine`, or ask what they want if empty) as the job.

This skill edits the teacher. It never edits the learner's work: no code in `~/source/repos/rust-learning/<exercise>/`, no solving their Rust problems, and no changing their levels in `log/` unless a log-format change forces a migration (see below). If a session mixes both, the teacher's own rules still apply to the Rust parts.

## Start
1. Read every file in `~/.claude/skills/rust-teacher/` except `rules/references/` (list them with `find`). Read `changes.md` in this skill's folder, `~/.claude/CLAUDE.md`, and `~/source/repos/rust-learning/CLAUDE.md` and `README.md`.
2. Say the request back in one line and name which area it belongs to: teaching behaviour (`SKILL.md`), review (`review.md`), standards or lints (`rules/`), curriculum, log format, a lesson, or the learner profile and paths.
3. If the request contradicts an existing rule, say so in one line and do what the user asked. Record the new decision and the old rule in `changes.md`. The user owns the skill, so their instruction wins.

## Facts that live in more than one place
Each row is one fact. If you change it, change every place in the row, or say which you left and why.

| Fact | Where it appears |
|---|---|
| Never give the answer, and what may be answered directly | `SKILL.md` (absolute rule, "What you may answer directly", example code, stuck, give up), `review.md` ("Never show the fix"), `lessons/00-setup.md` (opening paragraph), the `description` in `SKILL.md`, `~/source/repos/rust-learning/CLAUDE.md` |
| Permissions and what needs a go-ahead | `SKILL.md` (Permissions, Neovim), the user's global `~/.claude/CLAUDE.md`, `rust-learning/CLAUDE.md` |
| Pre-submission routine (`fmt`, `clippy -D warnings`, `test`) | `rules/standards.md` section 2 and its checklist near the end, `review.md` step 2, `lessons/00-setup.md` Part 5, `rust-learning/README.md`. Intended difference: the learner fixes layout with `cargo fmt`; the routine and reviews use `cargo fmt --check`; the teacher never runs plain `cargo fmt` |
| Lint configs | `rules/lints/` in this skill is the source. Copies: `rust-learning/clippy.toml` and `rust-learning/setup/lints-app.toml`, `lints-lib.toml`. They must stay byte-identical. Also described in `standards.md` section 2, `lessons/00-setup.md` Part 5, `rust-learning/README.md` |
| Understanding and Ability scales (0 to 4) | `log-format.md`, used in `review.md` section 5, `SKILL.md` (Keeping the log), and every entry in `rust-learning/log/*.md` |
| Log files and what each holds | `log-format.md` (Files), `rust-learning/README.md`, `SKILL.md` (Start of every session, Keeping the log), the actual files in `rust-learning/log/` |
| Curriculum numbers and topic names | `curriculum.md`, `review.md` ("Noted, comes later" topics), log file names, `SKILL.md` (start of session, Structure) |
| Modules (non-Rust topics) and their logs | `rust-teacher/modules/*.md`, `SKILL.md` (supporting files list, "Modules and design decisions"), `curriculum.md` (Side tracks), `log-format.md` (Files) |
| The learner's current project and its stack (TeamCord: Rust backend, Next.js front end, Postgres, Docker) | `SKILL.md` (Modules paragraph, C# section), `curriculum.md` (side tracks), `modules/`, `rust-learning/log/overview.md` (Project), `log/project-ideas.md` (Chosen direction) |
| Supporting files list | `SKILL.md` ("Supporting files in this skill's folder"), the real contents of the folder |
| Workspace and tool paths (`~/source/repos/rust-learning/`, `log/`, `~/.config/nvim/init.lua`, `~/source/docs/neovim-keybinds.md`) | `SKILL.md`, `log-format.md`, `lessons/*.md`, `~/.claude/CLAUDE.md` |
| When the skill triggers | `description` in `SKILL.md`, `rust-learning/CLAUDE.md` |
| Standards and their sources | `rules/standards.md`, `rules/references/*.md`, `rules/LICENSE-BeautifulRust.txt`, mentions in `SKILL.md` and `review.md` |
| Question cap and "don't make them reproduce things" | `review.md` (template, sections 5 and 6), `SKILL.md` (stuck step 1, Teaching style, No quizzes) |
| Facts about the learner (background, named gaps, C# rule, strictness) | `SKILL.md` (intro, C#, Beyond Rust), `rust-learning/log/overview.md` (Background) |

When a change adds a new fact that more than one file will repeat, add a row to this table in the same edit.

## How to work
1. **Find every place first.** Before editing, `grep` the skill folder, `rust-learning/CLAUDE.md`, `README.md` and `~/.claude/CLAUDE.md` for the key words of the thing being changed (old wording, file names, numbers). List each hit.
2. **Smallest edit set that keeps the table true.** Match the voice of the file you're in: plain, direct sentences, no filler, reasons given next to rules. Don't rewrite text the request doesn't touch.
3. **Prefer pointing to one source** over copying text into a new place, but don't consolidate existing duplicates unless asked. Mention them instead.
4. **Write for a fresh session.** The skill is read cold. No "as we discussed", no dates in rules, no learner progress inside skill files (that belongs in `log/`).
5. **Keep it readable.** If `SKILL.md` grows past about 200 lines, say so and suggest moving detail into a supporting file.

## Permissions
- Invoking this skill with a request is the go-ahead to edit `.md` files in `~/.claude/skills/rust-teacher/` and this folder, and to copy the skill folder to a backup first.
- Everything else needs an explicit go-ahead first: the `.toml` lint copies (including the ones in `rust-learning/`), any `CLAUDE.md`, `~/.config/nvim/`, and anything inside an exercise folder.
- Before the first edit, if the change touches more than one file and `~/.claude/skills/rust-teacher` isn't in git, copy it to `~/.claude/skill-backups/rust-teacher-<YYYY-MM-DD>-<n>/` (outside the skills folder, so it isn't loaded as a skill). Tell the user the path.
- Never commit unless asked.

## When the log format changes
If a change alters how `log/*.md` entries are written (fields, scales, file names), migrate the existing files in `rust-learning/log/` in the same change. Keep every level, date, quote and piece of evidence; only reshape. `.md` edits there are allowed. Say what you migrated and what you couldn't map.

## Verify (don't skip)
- `find ~/.claude/skills/rust-teacher -type f` matches the supporting-files list in `SKILL.md`.
- `grep` for the old wording or old names returns nothing in the skill, `rust-learning/CLAUDE.md`, `README.md` and `~/.claude/CLAUDE.md`.
- Lint copies agree: `diff rules/lints/clippy.toml ~/source/repos/rust-learning/clippy.toml` and the same for `lints-app.toml` and `lints-lib.toml` against `rust-learning/setup/`.
- Re-read each table row your change belongs to and confirm every place in it agrees.
- Frontmatter: `name` matches the folder, `description` is one paragraph, and the first lines of `SKILL.md` still parse as YAML.
- Read the changed text as a fresh session would, against one realistic case (a review of a small crate; a learner who is stuck; a new topic). Does it still say what to do?

## Known traps
- The `description` in `rust-teacher/SKILL.md` controls when the teacher activates. Don't loosen "Do not use for ordinary Rust work", or normal Rust tasks will start refusing to write code.
- The teacher's absolute rule has no exceptions on purpose. If the user asks to weaken it, make the exact change they ask for in every place in the first table row, and record it in `changes.md`.
- `lessons/00-setup.md` has dated facts and describes a finished setup. Treat it as history; only touch it if the user asks, or to keep a table row true.
- `rules/standards.md` is derived from BeautifulRust (MIT). Keep `LICENSE-BeautifulRust.txt` and the attribution if you edit it.
- Lint configs are copied into every learner crate by hand, so changing them changes nothing for existing crates until the learner re-pastes. Say so.
- Curriculum renumbering breaks log file names and the "Topic:" fields in past reviews. Add topics at the end of a part, or tell the user what it breaks.

## Finish
1. Add an entry to `changes.md` in this folder: date, the request, files changed, why, and anything left inconsistent on purpose.
2. Tell the user what changed (files), what you verified, what you didn't, and how to undo it (the backup path, or git).
3. If the change alters what the learner will notice next session (a new review rule, a new log field), say what they will see differently.
