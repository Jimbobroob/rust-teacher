# rust-teacher changes

Newest first. One entry per refine run: date, request, files changed, why, anything left inconsistent on purpose.

## 2026-11-17 (newest): project changed from the figure tracker to TeamCord
- Request: replace the figure tracker and every reference to it and its stack with TeamCord (their own C# chat backend, to be rewritten in Rust, plus the front end and Docker). The stack is whatever is already in `~/source/repos/TeamCord/`; no Kubernetes for now.
- Files: `rust-teacher/SKILL.md` (supporting files list, Modules paragraph, C# section), `curriculum.md` (side tracks, topic 19 gains WebSockets), `log-format.md` (module logs), `modules/containers-docker.md` (replaces `containers-podman.md`), `modules/public-api-design.md` (rewritten for an authenticated chat API and WebSockets), `modules/typescript-react.md` (tracker-specific parts replaced); removed `modules/kubernetes.md`, `terraform-hetzner.md`, `ci-cd-make.md`. This skill's `SKILL.md` (new table row). `rust-learning/log/overview.md`, `project-ideas.md`, `decisions.md` (project text only; no levels touched). Memory `project_no-arabic.md` removed. Backup of everything: `~/.claude/skill-backups/rust-teacher-2026-11-17-1/` (includes the removed modules and the old log files).
- Old position: Rust, Postgres, TypeScript/React, Podman, k3s, Terraform, Hetzner Cloud, Make and CI for a read-only public data API.
- New rule: the teacher never translates their C# into Rust (`SKILL.md`, C# section), since the rewrite makes that an easy way to give the answer.
- Research quality: the Docker, API and WebSocket modules are general knowledge, not searched, and say so. Docker is not installed on their machine (Podman is); the module tells them once and leaves the choice to them.
- Left as is on purpose: the figure-tracker folder and `change-finder/` (their files); history entries in `decisions.md`, the session line in `overview.md` and older entries below; `change-finder` is still the planned next exercise until they decide. The `feedback-no-guessing-games-on-data` memory and the data-guessing rule in `SKILL.md` stay as general rules.

## 2026-11-16 (later still): CI/CD and Make module
- Request: add a module for CI/CD, since they will use Make targets and have Buildkite experience from work.
- Files: new `rust-teacher/modules/ci-cd-make.md`; `rust-teacher/SKILL.md` (supporting files list and Modules paragraph), `curriculum.md` (side tracks), `log-format.md` (log `ci-cd.md`). Backup: `~/.claude/skill-backups/rust-teacher-2026-11-16-4/`.
- Content decisions: Make stays thin and is the single entry point for local and CI; their choice between Buildkite and GitHub Actions is left open; a safety rule says never to use their employer's Buildkite organisation or machines for the personal project.
- Research quality: Buildkite pricing facts are from disagreeing third-party trackers; the module says to check Buildkite's own page. Make guidance is general knowledge, not searched.
- Left as is: `ROADMAP.md` has no CI/CD stage yet; it is implied between stages 6 and 11.

## 2026-11-16 (latest): hosting changed from AWS to Hetzner
- Request: they chose Hetzner Cloud (k3s on a small server) and asked for the new modules and overall plan to be rewritten to match.
- Files: new `rust-teacher/modules/terraform-hetzner.md`; removed `rust-teacher/modules/terraform-aws.md`; `rust-teacher/modules/kubernetes.md` (AWS references replaced, new k3s section from the official hardening guide); `rust-teacher/SKILL.md`, `curriculum.md`, `log-format.md` (names and log file `terraform-hetzner.md`); `~/source/repos/figure-tracker/ROADMAP.md` (technology, constraints, stage 11). Backup: `~/.claude/skill-backups/rust-teacher-2026-11-16-3/` (still holds the AWS module).
- Old position: Terraform, Kubernetes and AWS, with EKS costed at about $73 a month for the control plane alone. The Hetzner module deliberately quotes no price, because sources disagree about 2026 increases.
- Research quality: Hetzner firewall, backup and cost-sweep points are from third-party guides and a community module README; token scoping and k3s datastore backup were not found. The module says so and points to the official docs.
- Left as is: an earlier price I gave the learner (about €3.79) turned out to be out of date, and I corrected it in chat. The AWS entries remain in the older changes.md entries as history.

## 2026-11-16 (later): modules, design-input rule
- Request: split Podman, Kubernetes, Terraform and AWS, TypeScript and React into module skills the main skill references, with best practices and safety protocols for each. Also: their input on design is final, give suggestions, never ask them to work out the optimal design or use human lives as guessing points in data.
- Files: new `rust-teacher/modules/` (`containers-podman.md`, `kubernetes.md`, `terraform-aws.md`, `typescript-react.md`, `sql-postgres.md`, `public-api-design.md`), `rust-teacher/SKILL.md` (supporting files list; new "Modules and design decisions" section), `rust-teacher/curriculum.md` (side tracks), `rust-teacher/log-format.md` (module logs). Backup: `~/.claude/skill-backups/rust-teacher-2026-11-16-2/` (includes `refine-SKILL.md.bak`).
- Research: best practices and safety rules came from web searches, mostly secondary sources (vendor and community guides). Each module lists its sources and says what to check against primary docs. The Postgres security search returned nothing, so `sql-postgres.md` is general practice and says so. EKS prices are from AWS's pricing page snippet and should be rechecked.
- Left inconsistent on purpose: the "never give the answer" rule now covers all modules in `SKILL.md` (Modules and design decisions), but the `description` in `SKILL.md`, `review.md`, `lessons/00-setup.md` and `~/source/repos/rust-learning/CLAUDE.md` still say "Rust problem". `rust-learning/CLAUDE.md` needs an explicit go-ahead to change. Modules are "module skills" as files under `rust-teacher/modules/`, not separate registered skills, so they only load when the teacher reads them.
- Old behaviour changed: the teacher had asked the learner to reason about data to find revisions in casualty figures; that is now barred.

## 2026-11-16: project-led, just-in-time teaching, SQL side track
- Request: the structure felt too rigid. They want to learn what they need when they meet it, with the teacher predicting likely problems, warning beforehand with a small lesson, then giving a bigger lesson once they hit the real error. Also wants SQL and Postgres worked on, not left until `sqlx`.
- Files: `rust-teacher/SKILL.md` (Structure; new "Just in time" section), `rust-teacher/curriculum.md` (intro; new unnumbered "Side tracks"), `rust-teacher/log-format.md` (Files: `sql.md`, `project-ideas.md`). Backup: `~/.claude/skill-backups/rust-teacher-2026-11-16-1/`.
- Old rules: "Topics first, with separate exercises" and "They can switch to projects whenever they want. Suggest it once the core is mostly done, but don't push."
- Teacher's position, recorded: it preferred teaching ownership and borrowing before an API project, since compiler errors would arrive before the explanation. The learner chose just-in-time warnings instead. The pre-warning step must name concepts, not designs, so it never becomes the answer.
- Left as is: "never give the answer", review process, "no quizzes". `review.md` "Noted, comes later" unchanged. No renumbering: side tracks are unnumbered.

## 2026-11-15: cap review questions, no reproducing errors
- Request: the learner is barraged by questions at the end of a review, doesn't remember specific error messages, and doesn't want to be asked to break their code to show errors.
- Files: `rust-teacher/review.md` (template, section 5, section 6), `rust-teacher/SKILL.md` (stuck step 1, Teaching style, No quizzes), `rust-teacher-refine/SKILL.md` (new table row). Backup: `~/.claude/skill-backups/rust-teacher-2026-11-15-1/`.
- Why: "1-2 questions" plus step 6's "ask why the new version is correct" had no shared cap, and stuck step 1 asked them to say what the compiler message meant. Old rules: section 5 "one or two questions", section 6 uncapped "ask them why", stuck step 1 "what they think the compiler message means".
- Now: at most two questions per review including resubmission; never ask to recall error output; never ask them to modify their code to observe behaviour; no second round of questions.
- Left as is: "When they give up" still uses step-by-step small questions, since that is the learner working the problem, not end-of-review questioning.

## 2026-11-15: baseline
- Request: create a refine skill for `rust-teacher`.
- Files: added `~/.claude/skills/rust-teacher-refine/SKILL.md` and this file. `rust-teacher/` not touched.
- State of the skill when this was written:
  - Lint copies in `rust-learning/` were byte-identical to `rust-teacher/rules/lints/`.
  - Routine commands: `standards.md` says `cargo fmt` (learner fixes layout); `review.md`, `lessons/00-setup.md` and `rust-learning/README.md` say `cargo fmt --check`. This is intended.
  - `lessons/00-setup.md` Part 7 still describes setup steps that are done; kept as history.
  - `SKILL.md` lists supporting files: `curriculum.md`, `review.md`, `log-format.md`, `rules/standards.md`, `rules/references/`, `lessons/00-setup.md`. `lessons/00-aesthetics-plan.md` and `rules/lints/` exist on disk but aren't named in that list (the aesthetics plan is reached from `00-setup.md` Part 7). Left as is.
