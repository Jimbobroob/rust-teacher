# rust-teacher-refine

The skill that edits `rust-teacher`. It lives here so it travels with the teacher, but Claude Code only loads skills that sit directly in `~/.claude/skills/`, so install it as its own skill:

```
cp -r ~/.claude/skills/rust-teacher/rust-teacher-refine ~/.claude/skills/rust-teacher-refine
```

Then run `/rust-teacher-refine <what you want changed>`. Keep the two copies in step: edits made by the refine skill land in `~/.claude/skills/`, so copy them back here before committing.
