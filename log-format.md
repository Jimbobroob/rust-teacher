# Learning log format

The log lives in `~/source/repos/rust-learning/log/`. They read it to see their own abilities, and you use it to decide what to weave into new work. Write it for a person: detailed, specific, and honest.

## Files

- `overview.md`: the big picture. Setup status, current focus, what they can now build unaided, strongest and weakest areas, how well concepts connect, the last session in a few lines, and open items.
- `<topic>.md`: one per curriculum area, such as `ownership-borrowing.md` or `errors.md`. Holds the concept entries. Create these as topics come up.
- `neovim.md`: Neovim skills in the same format.
- `sql.md`, `containers.md`, `typescript-react.md`, `public-api.md`: one per module in `modules/`, in the same format. Create each when that topic first comes up.
- `project-ideas.md`: project suggestions, what they liked and rejected, and their interests. Update it as ideas come up.
- `decisions.md`: arguments they won (exceptions, with their reasoning) and overrules (disagreements, with both positions).

## Concept entry

```markdown
### Moves vs. copies
- **Understanding:** 2 - explains that a move invalidates the source, but can't yet say why `Copy` types differ
- **Ability:** 3 - used correctly unaided in `ownership-moves/`
- **What they understand:** the specific parts they have
- **What they struggle with:** the specific parts they don't
- **Small picture:** how this concept works on its own, in their current grasp
- **Bigger picture:** how well they connect it to the rest of Rust and to design decisions
- **Evidence:** `ownership-moves/src/main.rs:12` (correct, unaided); review on 2026-11-05 ("why" answer was wrong about Copy)
- **Weak spots:** specific recurring mistakes, and any "give up" walkthroughs
- **Last used:** 2026-11-05
```

## Levels

Keep Understanding and Ability separate, because someone can use a thing without being able to explain it, or the reverse.

**Understanding: can they explain it?**
- 0: not met
- 1: recognizes it, can't explain it
- 2: partial explanation, with gaps or errors
- 3: accurate explanation including why it exists
- 4: predicts behavior in new cases, explains tradeoffs, and connects it to other concepts

**Ability: can they use it?**
- 0: not attempted
- 1: only through a step-by-step walkthrough
- 2: with hints
- 3: unaided in an exercise focused on it
- 4: unaided in new contexts, repeatedly, without being prompted to use it

## Rules

- **Evidence only.** Base levels on their code, compiler struggles, review answers, and what they ask. How confident they feel doesn't count, and neither does finishing a task.
- **Levels go down.** A wrong "why" answer lowers Understanding. Struggling with something again lowers Ability. A completely wrong answer gets recorded as such.
- **Cite evidence** with file paths and dates. Every level has to be justified by something in the entry.
- **Giving up** caps Ability at 1 for that concept until they later do it unaided, and is listed under weak spots.
- **Skips** are recorded: if they jumped past a topic, note what they skipped and when.
- **Be specific.** "Struggles with lifetimes" is useless. "Doesn't see that a returned reference must be tied to an input" is useful.
- **Update `overview.md`** whenever the big picture changes, and at the end of every session. Include today's real date on entries.
- Use the log to choose concepts to weave in: weak spots first, then concepts with an old "last used" date.
