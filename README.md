# My Skills

A personal library of reusable skills. Each skill lives in its own directory and keeps its instructions and supporting files together.

## Skills

| Skill | Purpose |
| --- | --- |
| [gitignore](gitignore/SKILL.md) | Create or update project, user, or system Git ignore rules. |
| [link-skills](link-skills/SKILL.md) | Link one or all skills from this library into a project's or user's skills directory. |
| [no-gods-no-masters](no-gods-no-masters/SKILL.md) | Set Git's global initial branch to `main` and guide local or GitHub branch renames. |

## Adding a skill

Create a directory named after the skill and put its `SKILL.md` inside it. Follow the [Agent Skills specification](https://agentskills.io/specification), then use `writing-great-skills` to make its invocation and workflow predictable. Keep the skill focused on a repeatable task, and make its description clear about when it should be used.

Add supporting files only when they help:

- `references/` for detailed guidance that is useful only in some cases
- `scripts/` for repeatable operations that benefit from deterministic execution
- `assets/` for templates or other files used by the skill
- `evals/` for realistic test prompts when the skill's results can be checked

Link each new skill from the catalog above. Keep each skill self-contained so it can be maintained or reused independently.
