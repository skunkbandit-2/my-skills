---
name: link-skills
description: Link one or all skills from this library into an Agent Skills directory. Use when the user wants to install, link, or expose a named skill or the whole library to the current project or their global user environment.
license: Unlicense
metadata: { author: skunkbandit2, version: "-1" }
---

# Link skills

Link skill directories from this library into the selected Agent Skills directory without copying or overwriting existing content.

## 1. Resolve source and selection

Use the directory containing this `link-skills/` directory as the library root. Resolve the library root physically so links point to the source, not to an intermediate symlink.

For a named install, require an exact child-directory name and confirm that it contains `SKILL.md`. For an all-skills install, include each immediate child directory containing `SKILL.md`; ignore other directories and files.

If the library root or requested name cannot be determined, ask for the missing path or exact name before making changes.

## 2. Resolve destination

Use the scope the user requested:

| Scope | Destination |
| --- | --- |
| Current project | `<project-root>/.agents/skills/<skill-name>` |
| Global user | `$HOME/.agents/skills/<skill-name>`; on Windows, `%USERPROFILE%/.agents/skills/<skill-name>` |

For project scope, use the current Git repository root when available; otherwise use the current project directory. Ask only when the intended project or scope is ambiguous. Expand environment variables and normalize the destination to an absolute path.

## 3. Preflight every link

Before creating any directories or links, check all selected destination paths:

- If a destination already resolves to the selected source directory, treat it as already installed and leave it unchanged.
- If a different file, directory, or link occupies the destination, stop without changing any selected destination. Report each conflict and its path.
- Otherwise, mark the destination for creation.

This preflight applies to the whole selection, including an all-skills install; do not partially install when any destination conflicts.

## 4. Create and verify links

Create the destination parent directory if needed, then create one directory link per marked destination. Use symbolic links where supported. On Windows, if creating a symbolic link is unavailable, use a directory junction when the source and destination are on a compatible local volume. Do not copy skill contents as a fallback.

Verify that each created or already-installed destination resolves to its intended source directory and contains `SKILL.md`. If link creation or verification fails, report the exact path and error; preserve the source and any pre-existing destination.

Report the selected skill names, scope, destination root, links created, links already present, and any conflicts or failures. The task is complete only when every requested skill is verified at the requested destination or each unresolved path is reported.